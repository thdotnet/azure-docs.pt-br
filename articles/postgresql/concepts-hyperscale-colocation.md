---
title: Conceitos de servidor no Banco de Dados do Azure para PostgreSQL
description: Este artigo fornece diretrizes e considerações para trabalhar com o Banco de Dados do Azure para servidores PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: d03cfd49887adf1f6a4650e374d3e13eeca735a4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077464"
---
# <a name="table-colocation-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Colocação de tabela no banco de dados do Azure para PostgreSQL – em hiperescala (Citus) (visualização)

A colocação significa armazenar informações relacionadas em conjunto nos mesmos nós. Consultas podem ir rápida quando os dados de todas as mídias necessárias estão disponíveis sem qualquer tráfego de rede. A colocação de dados relacionados em diferentes nós permite que as consultas executadas com eficiência em paralelo em cada nó.

## <a name="data-colocation-for-hash-distributed-tables"></a>Colocação de dados para tabelas distribuídas por hash

Em hiperescala, uma linha é armazenada em um fragmento, se o hash do valor na coluna de distribuição está dentro do intervalo de hash do fragmento. Fragmentos com o mesmo intervalo de hash sempre são colocados no mesmo nó. Linhas com valores de coluna de distribuição igual estão sempre no mesmo nó em tabelas.

![Fragmentos](media/concepts-hyperscale-colocation/colocation-shards.png)

## <a name="a-practical-example-of-colocation"></a>Um exemplo prático de colocação

Considere as tabelas a seguir que podem ser parte de uma análise da web multilocatário SaaS:

```sql
CREATE TABLE event (
  tenant_id int,
  event_id bigint,
  page_id int,
  payload jsonb,
  primary key (tenant_id, event_id)
);

CREATE TABLE page (
  tenant_id int,
  page_id int,
  path text,
  primary key (tenant_id, page_id)
);
```

Agora queremos responder a consultas que podem ser emitidas por um painel voltado ao cliente, tais como: "Retornar o número de visitas na última semana para todas as páginas começando com ' / blog' no locatário seis."

Se nossos dados estavam em uma opção de implantação de servidor único, com facilidade podemos expressar nossa consulta usando o conjunto avançado de operações relacionais oferecida pelo SQL:

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Desde que o [conjunto de trabalho](https://en.wikipedia.org/wiki/Working_set) para essa consulta se ajusta na memória, uma tabela de servidor único é uma solução apropriada. No entanto, vamos considerar as oportunidades de colocação do modelo de dados com a opção de implantação em hiperescala.

### <a name="distributing-tables-by-id"></a>Distribuindo tabelas por ID

Consultas de servidor único início lento à medida que aumenta o número de locatários e os dados armazenados para cada locatário. O conjunto de trabalho é interrompido, ajustando-se na memória e CPU se torna um gargalo.

Nesse caso, podemos fragmentos os dados em vários nós usando em hiperescala. A escolha de primeira e mais importante, precisamos fazer quando a fragmentação é a coluna de distribuição. Vamos começar com uma opção simples de usar `event_id` para a tabela de eventos e `page_id` para o `page` tabela:

```sql
-- naively use event_id and page_id as distribution columns

SELECT create_distributed_table('event', 'event_id');
SELECT create_distributed_table('page', 'page_id');
```

Quando os dados ficam dispersos entre diferentes trabalhos, não foi possível realizar uma junção, como podemos faria em um único nó do PostgreSQL. Em vez disso, precisamos emitir as duas consultas:

```sql
-- (Q1) get the relevant page_ids
SELECT page_id FROM page WHERE path LIKE '/blog%' AND tenant_id = 6;

-- (Q2) get the counts
SELECT page_id, count(*) AS count
FROM event
WHERE page_id IN (/*…page IDs from first query…*/)
  AND tenant_id = 6
  AND (payload->>'time')::date >= now() - interval '1 week'
GROUP BY page_id ORDER BY count DESC LIMIT 10;
```

Depois disso, os resultados das duas etapas precisam ser combinadas pelo aplicativo.

Executar consultas precisa consultar dados em fragmentos espalhados entre os nós.

![consultas ineficientes](media/concepts-hyperscale-colocation/colocation-inefficient-queries.png)

Nesse caso, a distribuição de dados cria as desvantagens significativas:

-   Sobrecarga de consultar cada fragmento, executando várias consultas
-   Sobrecarga de Q1 retornando o número de linhas para o cliente
-   Q2 ficando grande
-   A necessidade de escrever consultas em várias etapas exige alterações no aplicativo

Porque os dados ficam dispersos, as consultas podem ser paralelizadas. No entanto, ele só é útil se a quantidade de trabalho que faz a consulta é consideravelmente maior do que a sobrecarga de consultar vários fragmentos.

### <a name="distributing-tables-by-tenant"></a>Distribuindo tabelas por locatário

Em hiperescala, as linhas com o mesmo valor de coluna de distribuição são garantidas para estar no mesmo nó. Começar de novo, podemos criar nossos tabelas com `tenant_id` como a coluna de distribuição.

```sql
-- co-locate tables by using a common distribution column
SELECT create_distributed_table('event', 'tenant_id');
SELECT create_distributed_table('page', 'tenant_id', colocate_with => 'event');
```

Agora em hiperescala pode responder a consulta de servidor único original sem modificação (T1):

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Graças ao filtro e junção em tenant_id, hiperescala sabe que a consulta inteira pode ser respondida usando o conjunto de fragmentos colocados que contêm os dados para esse locatário específico. Um único nó PostgreSQL pode responder a consulta em uma única etapa.

![consulta de melhor](media/concepts-hyperscale-colocation/colocation-better-query.png)

Em alguns casos, as consultas e esquemas de tabela devem ser alteradas para incluir a ID de locatário em restrições exclusivas e condições de junção. No entanto, isso geralmente é uma alteração simples.

## <a name="next-steps"></a>Próximas etapas

- Veja como os dados de locatário são colocados no [tutorial multilocatário](tutorial-design-database-hyperscale-multi-tenant.md)

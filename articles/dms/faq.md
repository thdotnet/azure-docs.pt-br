---
title: Perguntas Frequentes sobre como usar o Serviço de Migração de Banco de Dados do Azure | Microsoft Docs
description: Saiba as perguntas frequentes sobre como usar o serviço de migração de banco de dados do Azure para executar migrações de banco de dados.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 07/04/2019
ms.openlocfilehash: f59af0bd0297b0413673fc42b26654c36351b815
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566233"
---
# <a name="faq-about-using-azure-database-migration-service"></a>Perguntas Frequentes sobre como usar o serviço de migração de banco de dados do Azure

Este artigo lista as perguntas frequentes sobre como usar o serviço de migração de banco de dados do Azure junto com as respostas relacionadas.

### <a name="q-what-is-azure-database-migration-service"></a>P. O que é o Serviço de Migração de Banco de Dados do Azure?

Serviço de migração de banco de dados do Azure é um serviço totalmente gerenciado projetado para permitir migrações perfeitas de várias fontes de banco de dados para plataformas de dados do Azure com o tempo de inatividade mínimo. Atualmente, o serviço está Geralmente Disponível, com esforços de desenvolvimento contínuo focados em:

* Confiabilidade e desempenho.
* Adição iterativa de pares de fonte-destino.
* Investimento contínuo em migrações sem conflitos.

### <a name="q-what-source-target-pairs-does-azure-database-migration-service-currently-support"></a>P. Quais pares origem-destino o serviço de migração de banco de dados do Azure atualmente dá suporte?

O serviço atualmente dá suporte para uma variedade de cenários de migração. Para obter uma listagem completa do status de cada cenário de migração disponível, consulte o artigo [Status dos cenários de migração com suporte pelo Serviço de Migração de Banco de Dados do Azure](https://docs.microsoft.com/azure/dms/resource-scenario-status). Outros cenários de migração estão em versão prévia e exigem enviando uma indicação por meio do site de visualização do DMS. Para obter uma listagem completa dos cenários em visualização e inscrever-se para participar de uma dessas ofertas, consulte o [site de visualização de DMS](https://aka.ms/dms-preview/).

### <a name="q-how-does-azure-database-migration-service-compare-to-other-microsoft-database-migration-tools-such-as-the-database-migration-assistant-dma-or-sql-server-migration-assistant-ssma"></a>P. Como o serviço de migração de banco de dados do Azure se compara a outras ferramentas de migração de banco de dados da Microsoft, como o Assistente de migração de banco de dados (DMA) ou o SQL Server Migration Assistant (SSMA)?

Serviço de migração de banco de dados do Azure é o método preferencial para a migração de banco de dados para o Microsoft Azure em grande escala. Para obter mais detalhes sobre como o serviço de migração de banco de dados do Azure se compara a outros produtos da Microsoft de banco de dados as ferramentas de migração e para obter recomendações sobre como usar o serviço para vários cenários, consulte a postagem de blog [migração de banco de dados da Microsoft diferenciação Ferramentas e serviços](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/).

### <a name="q-how-does-azure-database-migration-service-compare-to-the-azure-migrate-offering"></a>P. Como o serviço de migração de banco de dados do Azure se compara a oferta de migrações para Azure?

As migrações para Azure ajuda na migração de máquinas virtuais de locais para o Azure IaaS. O serviço avalia a adequação da migração e o dimensionamento com base no desempenho, e fornece estimativas de custo para a execução das máquinas virtuais locais no Azure. As Migrações para Azure são úteis para migrações de lift-and-shift de cargas de trabalho baseadas em VM local para VMs de IaaS do Azure. No entanto, ao contrário do serviço de migração de banco de dados do Azure, migrações para Azure não é um serviço de migração de banco de dados especializado oferta para plataformas de banco de dados relacional de PaaS do Azure, como o banco de dados SQL ou banco de dados de instância gerenciada do SQL.

### <a name="q-what-versions-of-sql-server-does-azure-database-migration-service-support-as-a-source"></a>P. Quais versões do SQL Server oferece suporte a serviço de migração de banco de dados do Azure como uma fonte?

Ao migrar do SQL Server, o serviço de migração de banco de dados do Azure dá suporte ao SQL Server 2005 por meio do SQL Server 2017.

### <a name="q-what-is-a-summary-of-the-steps-required-to-use-azure-database-migration-service-to-perform-a-database-migration"></a>P. O que é um resumo das etapas necessárias para usar o serviço de migração de banco de dados do Azure para executar uma migração de banco de dados?

Durante uma migração de banco de dados típica e simples, você:

1. Cria um banco de dados de destino.
2. Migra o esquema do banco de dados usando o [Assistente de Migração de Dados](https://www.microsoft.com/en-us/download/details.aspx?id=53595).
3. Criar uma instância do Serviço de Migração de Banco de Dados do Azure.
4. Cria um projeto de migração especificando os bancos de dados de origem, bancos de dados de destino e tabelas para migração.
5. Inicie o carregamento completo.
6. Escolhe a validação subsequente.
7. Execute uma mudança manual do seu ambiente de produção para o novo banco de dados baseado em nuvem.

### <a name="q-what-are-the-prerequisites-for-using-azure-database-migration-service"></a>P. Quais são os pré-requisitos para usar o serviço de migração de banco de dados do Azure?

Existem vários pré-requisitos necessários para garantir que o serviço de migração de banco de dados do Azure execute sem problemas ao executar migrações de banco de dados. Alguns dos pré-requisitos se aplicam em todos os cenários (pares de origem e destino) com suporte do serviço, enquanto outros pré-requisitos são exclusivos para um cenário específico.

Os pré-requisitos do Serviço de Migração de Banco de Dados do Azure que são comuns a todos os cenários de migração compatíveis incluem a necessidade de:

* Criar uma rede virtual para o serviço de migração de banco de dados do Azure usando o modelo de implantação do Azure Resource Manager, que fornece conectividade site a site aos servidores de origem locais usando o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [ VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Certifique-se de que sua rede Virtual do Azure (VNet) regras de grupo de segurança de rede não bloqueiam as seguintes portas de comunicação 443, 53, 9354, 445 e 12000. Veja mais detalhes sobre a filtragem de tráfego do NSG da VNet do Azure no artigo [Filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Ao usar um dispositivo de firewall na frente de seus bancos de dados de origem, talvez seja necessário adicionar regras de firewall para permitir que o Serviço de Migração de Banco de Dados do Azure acesse os bancos de dados de origem para migração.

Para obter uma lista de todos os pré-requisitos necessários para competir com cenários de migração específicos usando o serviço de migração de banco de dados do Azure, consulte os tutoriais relacionados no serviço de migração de banco de dados do Azure [documentação](https://docs.microsoft.com/azure/dms/dms-overview) em docs.microsoft.com.

### <a name="q-how-do-i-find-the-ip-address-for-azure-database-migration-service-so-that-i-can-create-an-allow-list-for-the-firewall-rules-used-to-access-my-source-database-for-migration"></a>P. Como localizar o endereço IP para o serviço de migração de banco de dados do Azure para que eu possa criar uma lista de permissões para as regras de firewall usado para acessar meu banco de dados de origem para migração?

Talvez você precise adicionar regras de firewall permitindo que o serviço de migração de banco de dados do Azure acessar o banco de dados de origem para migração. O endereço IP para o serviço é dinâmico, mas se você estiver usando o ExpressRoute esse endereço em particular será atribuído pela sua rede corporativa. A maneira mais fácil de identificar o endereço IP apropriado é procurar no mesmo grupo de recursos como o recurso de serviço de migração de banco de dados do Azure provisionado para localizar o adaptador de rede associado. Normalmente, o nome do recurso de Adaptador de Rede começa com o prefixo de NIC e é seguido por um caractere único e sequência numérica, por exemplo NIC-jj6tnztnmarpsskr82rbndyp. Ao selecionar esse recurso de interface de rede, você pode ver o endereço IP que deve ser incluído na lista de permissões na página do portal do Azure para visão de geral de recursos.

Talvez você precise incluir a origem da porta que o SQL Server está escutando na lista de permissões. Por padrão, é a porta 1433, mas o SQL Server de origem também pode estar configurado para escutar outras portas. Nesse caso, você também precisa incluir as portas na lista de permissões. Você pode determinar a porta que o SQL Server está escutando usando uma consulta de modo de exibição de Gerenciamento Dinâmico:

```sql
    SELECT DISTINCT
        local_tcp_port
    FROM sys.dm_exec_connections
    WHERE local_tcp_port IS NOT NULL
```

Você também pode determinar a porta que o SQL Server está escutando consultando o log de erros do SQL Server:

```sql
    USE master
    GO
    xp_readerrorlog 0, 1, N'Server is listening on'
    GO
```

### <a name="q-how-do-i-set-up-an-azure-virtual-network"></a>P. Como configurar uma Rede Virtual do Azure?

Embora existam vários tutoriais da Microsoft que podem orientar você durante o processo de configuração de uma VNET do Azure, a documentação oficial aparece no artigo [Rede Virtual do Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).


### <a name="q-im-setting-up-a-migration-project-in-dms-and-im-having-difficulty-connecting-to-my-source-database-what-should-i-do"></a>P. Eu estou configurando um projeto de migração no DMS e estou tendo dificuldade para se conectar ao meu banco de dados de origem. O que devo fazer?

Se você tiver problemas para se conectar ao seu sistema de banco de dados de origem enquanto estiver trabalhando na migração, crie uma máquina virtual na rede virtual com a qual você configurar sua instância do DMS. Na máquina virtual, você deve ser capaz de executar um teste de conexão, como usando um arquivo UDL para testar uma conexão ao SQL Server ou baixando o Robo 3T para testar as conexões do MongoDB. Se o teste de conexão for bem-sucedida, você não deve ter um problema com a conexão a seu banco de dados de origem. Se o teste de conexão não tiver êxito, entre em contato com seu administrador de rede.

### <a name="q-are-there-any-recommendations-for-optimizing-the-performance-of-azure-database-migration-service"></a>P. Há recomendações para otimizar o desempenho do serviço de migração de banco de dados do Azure?

Você pode fazer algumas coisas para acelerar a sua migração de banco de dados usando o serviço:

* Use os vários tipos de preços de uso geral da CPU ao criar sua instância de serviço para permitir que o serviço aproveite as várias vCPUs para paralelização e transferência de dados mais rápida.
* Temporariamente, escale verticalmente a sua instância de destino de banco de dados SQL do Azure para o SKU da camada Premium durante a operação de migração de dados para minimizar a limitação do banco de dados SQL do Azure que pode afetar as atividades de transferência de dados ao usar SKUs de nível inferior.

### <a name="q-why-is-my-azure-database-migration-service-unavailable-or-stopped"></a>P. Por que meu Serviço de Migração de Banco de Dados do Azure está indisponível ou parado?

Se o usuário explicitamente parar o Serviço de Migração de Banco de Dados do Azure (DMS) ou se o serviço ficar inativo por um período de 24 horas, o serviço será interrompido ou estará em estada de pausa automático. Em cada caso, o serviço estará indisponível e em status parado.  Para retomar as migrações ativas, reinicie o serviço.

### <a name="q-where-can-i-leave-feedback-about-azure-database-migration-service"></a>P. Onde posso deixar comentários sobre o serviço de migração de banco de dados do Azure?

Queremos ouvir você. Enviar comentários e / ideias que você tem sobre o serviço de migração de banco de dados do Azure por meio do User Voice [aqui](https://feedback.azure.com/forums/906100-azure-database-migration-service), ou contate a equipe de [faça migrações de banco de dados do Azure](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

## <a name="next-steps"></a>Próximas etapas

Para obter uma visão geral do Serviço de Migração de Banco de Dados do Azure e da disponibilidade regional, consulte o artigo [ O que é o Serviço de Migração de Banco de Dados do Azure ](dms-overview.md).

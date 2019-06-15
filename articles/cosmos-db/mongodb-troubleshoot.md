---
title: Solucionar problemas de erros comuns na API do Azure Cosmos DB de DB Mongo
description: Este documento discute as maneiras de solucionar problemas comuns encontrados na API do Azure Cosmos DB para MongoDB.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: roaror
ms.openlocfilehash: 5b3d3993a497240c1ea18f0fcf852c0e834f6e79
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735700"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>Solucionar problemas comuns na API do Azure Cosmos DB para MongoDB

O Azure Cosmos DB implementa os protocolos de transmissão de bancos de dados NoSQL comuns, incluindo MongoDB. Devido à implementação do protocolo de transmissão, transparentemente você pode interagir com o Azure Cosmos DB usando os SDKs do cliente existente, drivers e ferramentas que funcionam com bancos de dados NoSQL. O Azure Cosmos DB não usa qualquer código-fonte dos bancos de dados para fornecer APIs compatíveis com a transmissão para qualquer um dos bancos de dados NoSQL. Qualquer driver de cliente do MongoDB que compreende as versões do protocolo de transmissão pode se conectar ao Azure Cosmos DB.

Embora a API do Azure Cosmos DB para MongoDB é compatível com a versão 3.2 do protocolo de transmissão do MongoDB (os operadores de consulta e recursos adicionados na versão 3.4 disponíveis atualmente como uma versão prévia), há alguns códigos de erro personalizada que correspondem ao Azure Cosmos DB erros específicos. Este artigo explica os diferentes erros, códigos de erro e as etapas para resolver esses erros.

## <a name="common-errors-and-solutions"></a>Erros comuns e soluções

| Erro               | Código  | DESCRIÇÃO  | Solução  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | O número total de unidades de solicitação consumidas é mais do que a taxa de unidades de solicitação provisionada para a coleção e foi limitado. | Considere a possibilidade de dimensionar a taxa de transferência atribuída a um contêiner ou um conjunto de contêineres do portal do Azure ou tente a operação novamente. |
| ExceededMemoryLimit | 16501 | Como um serviço multilocatário, a operação excedeu a alocação de memória do cliente. | Reduza o escopo da operação por meio de um critério de consulta mais restritivo ou entre em contato com o suporte no [Portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Exemplo: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| Problemas de versão de transmissão do MongoDB | - | As versões mais antigas dos drivers do MongoDB não conseguem detectar o nome da conta do Azure Cosmos nas cadeias de conexão. | Acrescente *appName = @**accountName** @*  no final da API do Cosmos DB para a cadeia de caracteres de conexão do MongoDB, onde ***accountName*** é o nome da sua conta do Cosmos DB . |


## <a name="next-steps"></a>Próximas etapas

- Saiba como [usar o Studio 3T](mongodb-mongochef.md) com a API do Azure Cosmos DB para MongoDB.
- Saiba como [usar o Robo 3T](mongodb-robomongo.md) com a API do Azure Cosmos DB para MongoDB.
- Explore [exemplos](mongodb-samples.md) do MongoDB com a API do Azure Cosmos DB para MongoDB.


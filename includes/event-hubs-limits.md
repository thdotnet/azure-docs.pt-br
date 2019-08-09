---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 928b0bcfb32d1a319c811d00fc84a37f85702895
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68857310"
---
A tabela a seguir relaciona as cotas e limites específicos para os [Hubs de Eventos do Azure](https://azure.microsoft.com/services/event-hubs/). Para saber mais sobre os preços dos Hubs de Eventos, veja os [preços dos Hubs de Eventos](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limite | Escopo | Observações | Valor |
| --- | --- | --- | --- |
| Número de namespaces de Hubs de Eventos do Azure por assinatura |Assinatura |- |100 |
| Número de hubs de eventos por namespace |Namespace |As solicitações subsequentes para a criação de um novo hub de eventos são rejeitadas. |10 |
| O número de partições por hub de eventos |Entidade |- |32 |
| Tamanho máximo de eventos de Hubs de Eventos|Entidade |- |1 MB |
| Tamanho máximo do nome de um hub de eventos |Entidade |- |50 caracteres |
| Número de destinatários sem época por grupo de consumidores |Entidade |- |5 |
| Unidades de produtividade máxima |Namespace |Exceder o limite de unidade de produtividade faz com que os dados sejam restringidos e gera uma [exceção de servidor ocupado](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Para solicitar um número maior de unidades de produtividade para uma camada Standard, arquivo uma [solicitação de suporte](/azure/azure-supportability/how-to-create-azure-support-request). As [unidades de produtividade adicionais](../articles/event-hubs/event-hubs-auto-inflate.md) estão disponíveis em blocos de 20, em uma base de compra garantida. |20 |
| Número de regras de autorização por namespace |Namespace|As solicitações subsequentes para a criação da regra de autorização são rejeitadas.|12 |
| Número de chamadas para o método GetRuntimeInformation | Entidade | - | 50 por segundo | 
| Número de regras de rede virtual (VNet) e de configuração de IP | Entidade | - | 128 | 

### <a name="event-hubs-basic-and-standard---quotas-and-limits"></a>Hubs de eventos básico e padrão-cotas e limites
| Limite | Escopo | Observações | Basic | Standard |
| --- | --- | --- | -- | --- |
| Número de grupos de consumidores por hub de eventos |Entidade | - |1 |20 |
| Número de conexões AMQP por namespace |Namespace |As solicitações subsequentes para conexões adicionais são rejeitadas e uma exceção é recebida pelo código de chamada. |100 |5\.000|
| Período de retenção máximo dos dados do evento |Entidade | - |1 dia |Um a sete dias |


### <a name="event-hubs-dedicated---quotas-and-limits"></a>Hubs de Eventos Dedicados-cotas e limites
A oferta de Hubs de Eventos Dedicados é cobrada a um preço mensal fixo, com um mínimo de 4 horas de uso. A camada dedicada oferece todos os recursos do plano padrão, mas com capacidade e limites de escala empresarial para clientes com cargas de trabalho exigentes. 

| Recurso | Limites |
| --- | ---|
| Largura de banda |  20 CUs |
| Namespaces | 50 por CU |
| Hubs de Eventos |  1000 por namespace |
| Ingressar em eventos | Incluídas |
| Tamanho da mensagem | 1 milhão bytes |
| Partições | 2000 por CU |
| Grupos de consumidores | Nenhum limite por CU, 1000 por Hub de eventos |
| Conexões agenciadas | 100.000 incluídos |
| Retenção de Mensagem | Até 7 dias (retenção de 90 dias em breve), 10 TB incluídos por CU |
| Capturar | Incluídas |

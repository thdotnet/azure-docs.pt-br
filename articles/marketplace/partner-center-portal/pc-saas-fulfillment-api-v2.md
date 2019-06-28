---
title: SaaS cumprimento API v2 | O Azure Marketplace
description: Este artigo explica como criar e gerenciar uma oferta de SaaS no Azure Marketplace e AppSource, usando o preenchimento associado APIs v2.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 05/23/2019
ms.author: evansma
ms.openlocfilehash: ecee1669c29d7b298741f9e5521de03da6dd7e3b
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331628"
---
# <a name="saas-fulfillment-apis-version-2"></a>Cumprimento de SaaS APIs, versão 2 

Este artigo fornece detalhes sobre as APIs que permitem que os parceiros vender seus aplicativos SaaS no AppSource marketplace e o Azure Marketplace. Essas APIs são que um requisito para SaaS transacionáveis oferece no Azure Marketplace e AppSource.

## <a name="managing-the-saas-subscription-life-cycle"></a>Gerenciar o ciclo de vida da assinatura de SaaS

SaaS do Azure gerencia todo o ciclo de vida de uma compra de assinatura de SaaS. Ele usa o APIs de preenchimento como um mecanismo para direcionar o cumprimento real, alterações aos planos e a exclusão da assinatura com o parceiro. Fatura do cliente baseia-se o estado da assinatura de SaaS que a Microsoft mantém. O diagrama a seguir descreve os estados e as operações que orientam as alterações entre estados.

![Estados de ciclo de vida de assinatura de SaaS](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Estados de uma assinatura de SaaS

A tabela a seguir lista os estados de provisionamento para uma assinatura de SaaS, incluindo um diagrama de sequência e a descrição para cada (se aplicável). 

#### <a name="provisioning"></a>Provisionamento

Quando um cliente inicia uma compra, o parceiro recebe essas informações em um código de autorização em uma página da web interativos de cliente que usa um parâmetro de URL. Um exemplo é `https://contoso.com/signup?token=..`, enquanto que a URL da página de aterrissagem no Partner Center é `https://contoso.com/signup`. O código de autorização pode ser validado e trocado para obter os detalhes do serviço de provisionamento chamando a API de resolver.  Quando um serviço SaaS concluir o provisionamento, ele envia uma chamada de ativar para sinalizar que o cumprimento é concluído e o cliente pode ser cobrado. 

O diagrama a seguir mostra a sequência de chamadas à API para um cenário de provisionamento.  

![Chamadas à API para provisionar um serviço de SaaS](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Provisionado

Esse estado é o estado estável de um serviço provisionado.

##### <a name="provisioning-for-update"></a>O provisionamento de atualização 

Esse estado significa que uma atualização para um serviço existente está pendente. Essa atualização pode ser iniciada pelo cliente, do marketplace ou no serviço de SaaS (somente para transações de cliente para direct).

##### <a name="provisioning-for-update-when-its-initiated-from-the-marketplace"></a>Provisionamento de atualização (quando ela é iniciada do marketplace)

O diagrama a seguir mostra a sequência de ações quando uma atualização é iniciada do marketplace.

![Chamadas à API quando a atualização é iniciada do marketplace](./media/saas-update-api-v2-calls-from-marketplace-a.png)

##### <a name="provisioning-for-update-when-its-initiated-from-the-saas-service"></a>(Quando ele é iniciado do serviço de SaaS) de provisionamento para atualização

O diagrama a seguir mostra as ações quando uma atualização é iniciada do serviço SaaS. (A chamada do webhook é substituída por uma atualização para a assinatura iniciada pelo serviço SaaS). 

![Chamadas à API quando a atualização é iniciada do serviço de SaaS](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Suspenso

Este estado indica que o pagamento do cliente ainda não foram recebido. Por diretiva, forneceremos o cliente de um período de carência antes de cancelar a assinatura. Quando uma assinatura estiver nesse estado: 

- Como parceiro, você pode optar por diminuir ou bloquear o acesso do usuário para o serviço.
- A assinatura deve ser mantida em um estado recuperável que pode restaurar a funcionalidade completa sem qualquer perda de dados ou configurações. 
- Espere receber uma solicitação de restabelecimento desta assinatura por meio de APIs de preenchimento ou uma solicitação de desprovisionamento no final do período de cortesia. 

#### <a name="unsubscribed"></a>Assinatura cancelada 

Assinaturas atinjam esse estado em resposta a uma solicitação de cliente explícita ou a falta de pagamento de dívidas. A expectativa do parceiro é que os dados do cliente são mantidos para a recuperação de solicitação para um determinado número de dias e, em seguida, excluídos. 


## <a name="api-reference"></a>Referência de API

Esta seção documenta o SaaS *API de assinatura* e *operações API*.  O valor de `api-version` APIs de parâmetro para a versão 2 é `2018-08-31`.  


### <a name="parameter-and-entity-definitions"></a>Definições de parâmetro e de entidade

A tabela a seguir lista as definições de parâmetros e entidades usadas pelo cumprimento de APIs comuns.

|     Entidade/parâmetro     |     Definição                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | O identificador GUID de um recurso de SaaS.  |
| `name`                   | Um nome amigável fornecido para este recurso pelo cliente. |
| `publisherId`            | Um identificador de cadeia de caracteres exclusiva para cada publicador (por exemplo: "contoso"). |
| `offerId`                | Um identificador de cadeia de caracteres exclusiva para cada oferta (por exemplo: "offer1").  |
| `planId`                 | Um identificador de cadeia de caracteres exclusiva para cada plano/SKU (por exemplo: "Prata"). |
| `operationId`            | O identificador GUID de uma determinada operação.  |
|  `action`                | A ação que está sendo executada em um recurso, ou `subscribe`, `unsubscribe`, `suspend`, `reinstate`, ou `changePlan`, `changeQuantity`, `transfer`.  |
|   |   |

Identificadores globais exclusivos ([GUIDs](https://en.wikipedia.org/wiki/Universally_unique_identifier)) são números (hexadecimal com 32) de 128 bits que normalmente são gerados automaticamente. 

#### <a name="resolve-a-subscription"></a>Resolver uma assinatura 

O ponto de extremidade de resolução permite que o Editor resolver um token do marketplace para uma ID de recurso persistentes. A ID de recurso é o identificador exclusivo para uma assinatura de SaaS. Quando um usuário é redirecionado para o site de um parceiro, a URL contém um token nos parâmetros de consulta. O parceiro deve usar esse token e fazer uma solicitação para resolvê-lo. A resposta contém o ID de assinatura de SaaS, nome, ID de oferta e plano para o recurso exclusivo. Esse token é válido somente por uma hora. 

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsresolveapi-versionapiversion"></a>Postar<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  A versão da operação a ser usada para esta solicitação.  |

*Cabeçalhos de solicitação:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |  Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
|  x-ms-correlationid |  Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  authorization     |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |
|  x-ms-marketplace-token  |  O parâmetro de consulta de token na URL quando o usuário é redirecionado para o site do parceiro de SaaS do Azure (por exemplo: `https://contoso.com/signup?token=..`). *Observação:* A URL decodifica o valor do token do navegador antes de usá-lo.  |

*Códigos de resposta:*

Código: 200<br>
Resolve o token opaco para uma assinatura de SaaS. Corpo da resposta:
 

```json
{
    "id": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" 
}
```

Código: 400<br>
Solicitação inválida. x-ms-marketplace-token estiver ausente, malformadas ou expiradas.

Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual.

Código: 404<br>
Não encontrado.

Código: 500<br>
Erro interno do servidor.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

### <a name="subscription-api"></a>API de assinatura

A API de assinatura suporta as seguintes operações de HTTPS: **Obtenha**, **Post**, **Patch**, e **excluir**.


#### <a name="list-subscriptions"></a>Listar assinaturas

Lista todas as assinaturas para um publicador do SaaS.

##### <a name="getbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsapi-versionapiversion"></a>Obter<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  A versão da operação a ser usada para esta solicitação.  |

*Cabeçalhos de solicitação:*

|                    |                   |
|  ---------------   |  ---------------  |
| Content-Type       |  `application/json`  |
| x-ms-requestid     |  Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
| x-ms-correlationid |  Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
| authorization      |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Códigos de resposta:*

Código: 200 <br/>
Obtém o publicador e o correspondentes assinaturas para ofertas de todas as do Editor, com base no token de autenticação.
Carga de resposta:<br>

```json
{
  [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```

O token de continuação estará presente somente se houver adicionais "páginas" de planos para recuperar. 

Código: 403 <br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual. 

Código: 500<br>
Erro interno do servidor.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="get-subscription"></a>Obter assinatura

Obtém a assinatura especificada de SaaS. Use essa chamada para obter informações de licença e informações do plano.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Obter<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Um identificador exclusivo da assinatura do SaaS é obtido depois de resolver o token por meio da API de resolver.   |
|  ApiVersion        |   A versão da operação a ser usada para esta solicitação.   |

*Cabeçalhos de solicitação:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`  |
|  x-ms-requestid    |  Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
|  x-ms-correlationid |  Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  authorization     |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*Códigos de resposta:*

Código: 200<br>
Obtém a assinatura de SaaS do identificador. Carga de resposta:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"Contoso Cloud Solution",
        "publisherId": "contoso",
        "offerId": "offer1",
        "planId": "silver",
        "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "status": "Subscribed", // Indicates the status of the operation.
}
```

Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual.

Código: 404<br>
Não encontrado.<br> 

Código: 500<br>
Erro interno do servidor.<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Lista de planos disponíveis

Use essa chamada para descobrir se há qualquer oferta pública ou privada para o publicador atual.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidlistavailableplansapi-versionapiversion"></a>Obter<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   A versão da operação a ser usada para esta solicitação.  |

*Cabeçalhos de solicitação:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|   x-ms-requestid   |   Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
|  x-ms-correlationid  | Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
|  authorization     |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*Códigos de resposta:*

Código: 200<br>
Obtém uma lista de planos disponíveis para um cliente. Corpo da resposta:

```json
{
    "plans": [{
        "planId": "Platinum001",
        "displayName": "Private platinum plan for Contoso",
        "isPrivate": true
    }]
}
```

Código: 404<br>
Não encontrado.<br> 

Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual. <br> 

Código: 500<br>
Erro interno do servidor.<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="activate-a-subscription"></a>Ativar uma assinatura

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidactivateapi-versionapiversion"></a>Postar<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  A versão da operação a ser usada para esta solicitação.  |
| subscriptionId     | Um identificador exclusivo da assinatura do SaaS é obtido depois de resolver o token usando a API de resolver.  |

*Cabeçalhos de solicitação:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json`  |
|  x-ms-requestid    | Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  x-ms-correlationid  | Um valor de cadeia de caracteres exclusiva para a operação no cliente. Essa cadeia de caracteres se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  authorization     |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*Carga de solicitação:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Códigos de resposta:*

Código: 200<br>
Ativa a assinatura.<br>

Código: 400<br>
Solicitação incorreta: falhas de validação.

Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual.

Código: 404<br>
Não encontrado.

Código: 500<br>
Erro interno do servidor.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="change-the-plan-on-the-subscription"></a>Alterar o plano de assinatura

Atualize o plano na assinatura.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  A versão da operação a ser usada para esta solicitação.  |
| subscriptionId     | Um identificador exclusivo da assinatura do SaaS é obtido depois de resolver o token usando a API de resolver.  |

*Cabeçalhos de solicitação:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  x-ms-correlationid  |  Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.    |
| authorization      |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Carga de solicitação:*

```json
Request Body:
{
    "planId": "gold"
}
```

*Cabeçalhos de solicitação:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | O link para um recurso para obter o status da operação.   |

*Códigos de resposta:*

Código: 202<br>
A solicitação de alteração de plano foi aceita. O parceiro deve sondar a operação de localização para determinar o êxito ou falha. <br>

Código: 400<br>
Solicitação incorreta: falhas de validação.

Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual.

Código: 404<br>
Não encontrado.

Código: 500<br>
Erro interno do servidor.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Apenas um plano ou quantidade pode ser corrigida ao mesmo tempo, não ambos. Edita em uma assinatura com **atualização** não está em `allowedCustomerOperations`.

#### <a name="change-the-quantity-on-the-subscription"></a>Alterar a quantidade da assinatura

Atualize a quantidade na assinatura.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  A versão da operação a ser usada para esta solicitação.  |
| subscriptionId     | Um identificador exclusivo da assinatura do SaaS é obtido depois de resolver o token usando a API de resolver.  |

*Cabeçalhos de solicitação:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  x-ms-correlationid  |  Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.    |
| authorization      |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Carga de solicitação:*

```json
Request Body:
{
    "quantity": 5
}
```

*Cabeçalhos de solicitação:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Vincular a um recurso para obter o status da operação.   |

*Códigos de resposta:*

Código: 202<br>
A solicitação para alterar a quantidade foi aceita. O parceiro deve sondar a operação de localização para determinar o êxito ou falha. <br>

Código: 400<br>
Solicitação incorreta: falhas de validação.


Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual.

Código: 404<br>
Não encontrado.

Código: 500<br>
Erro interno do servidor.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Apenas um plano ou quantidade pode ser corrigida ao mesmo tempo, não ambos. Edita em uma assinatura com **atualização** não está em `allowedCustomerOperations`.

#### <a name="delete-a-subscription"></a>Excluir uma assinatura

Cancelar a assinatura e exclua a assinatura especificada.

##### <a name="deletebr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionid-api-versionapiversion"></a>Excluir<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId> ?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  A versão da operação a ser usada para esta solicitação.  |
| subscriptionId     | Um identificador exclusivo da assinatura do SaaS é obtido depois de resolver o token usando a API de resolver.  |

*Cabeçalhos de solicitação:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |   Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.   |
|  x-ms-correlationid  |  Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.   |
|  authorization     |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Códigos de resposta:*

Código: 202<br>
O parceiro iniciou uma chamada para cancelar a assinatura de uma assinatura de SaaS.<br>

Código: 400<br>
Excluir em uma assinatura com **exclua** não está no `allowedCustomerOperations`.

Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual.

Código: 404<br>
Não encontrado.

Código: 500<br>
Erro interno do servidor.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```


### <a name="operations-api"></a>Operações de API

A API de operações suporta as seguintes operações de Patch e Get.

#### <a name="list-outstanding-operations"></a>Lista de operações pendentes 

Lista as operações pendentes para o publicador atual. 

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsapi-versionapiversion"></a>Obter<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   A versão da operação a ser usada para esta solicitação.                |
| subscriptionId     | Um identificador exclusivo da assinatura do SaaS é obtido depois de resolver o token usando a API de resolver.  |

*Cabeçalhos de solicitação:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |  Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  x-ms-correlationid |  Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  authorization     |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Códigos de resposta:*

Código: 200<br> Obtém a lista de operações em uma assinatura pendentes. Carga de resposta:

```json
[{
    "id": "<guid>",  
    "activityId": "<guid>",
    "subscriptionId": "<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",  
    "status": "NotStarted"  
}]
```


Código: 400<br>
Solicitação incorreta: falhas de validação.

Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual.

Código: 404<br>
Não encontrado.

Código: 500<br>
Erro interno do servidor.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>Obter status da operação

Permite que o Editor acompanhar o status da operação especificada assíncrona disparado (como `subscribe`, `unsubscribe`, `changePlan`, ou `changeQuantity`).

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Obter<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  A versão da operação a ser usada para esta solicitação.  |

*Cabeçalhos de solicitação:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`   |
|  x-ms-requestid    |   Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  x-ms-correlationid |  Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
|  authorization     |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Códigos de resposta:*<br>

Código: 200<br> Obtém um especificado SaaS operação pendente. Carga de resposta:

```json
Response body:
{
    "id  ": "<guid>",
    "activityId": "<guid>",
    "subscriptionId":"<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}

```

Código: 400<br>
Solicitação incorreta: falhas de validação.

Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual.
 
Código: 404<br>
Não encontrado.

Código: 500<br> Erro interno do servidor.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>O status de uma operação de atualização

Atualize o status de uma operação para indicar êxito ou falha com os valores fornecidos.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Parâmetros de consulta:*

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  A versão da operação a ser usada para esta solicitação.  |
| subscriptionId     | Um identificador exclusivo da assinatura do SaaS é obtido depois de resolver o token usando a API de resolver.  |
|  operationId       | A operação que está sendo concluída. |

*Cabeçalhos de solicitação:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     | `application/json`   |
|   x-ms-requestid   |   Um valor de cadeia de caracteres exclusivo para acompanhamento da solicitação do cliente, preferencialmente um GUID. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
|  x-ms-correlationid |  Um valor de cadeia de caracteres exclusiva para a operação no cliente. Esse parâmetro se correlaciona a todos os eventos da operação de cliente com eventos no lado do servidor. Se esse valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
|  authorization     |  [Obter o token de portador do JSON web token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Carga de solicitação:*

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Códigos de resposta:*

Código: 200<br> Uma chamada para informar de conclusão de uma operação no lado do parceiro. Por exemplo, essa resposta pode sinalizar a alteração de estações ou planos.

Código: 400<br>
Solicitação incorreta: falhas de validação.

Código: 403<br>
Não autorizado. O token de autenticação não foi fornecido ou é inválido ou a solicitação está tentando acessar uma aquisição que não pertence do publicador atual.

Código: 404<br>
Não encontrado.

Código: 409<br>
Conflito. Por exemplo, uma transação mais recente já é atendida.

Código: 500<br> Erro interno do servidor.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="implementing-a-webhook-on-the-saas-service"></a>Implementando um webhook no serviço de SaaS

O publicador deve implementar um webhook neste serviço de SaaS para notificar os usuários das alterações no seu serviço. O serviço de SaaS deve chamar as operações API para validar e autorizar antes de realizar uma ação na notificação de webhook.

```json
{
  "id": "<this is a GUID operation id, you can call operations API with this to get status>",
  "activityId": "<this is a Guid correlation id>",
  "subscriptionId": "<Guid to uniquely identify this resource>",
  "publisherId": "<this is the publisher’s name>",
  "offerId": "<this is the offer name>",
  "planId": "<this is the plan id>",
  "quantity": "<the number of seats, will be null if not per-seat saas offer>",
  "timeStamp": "2019-04-15T20:17:31.7350641Z",
  "action": "Unsubscribe",
  "status": "NotStarted"  

}
```
Onde a ação pode ser um dos seguintes: 
- `subscribe` (quando o recurso tiver sido ativado)
- `unsubscribe` (quando o recurso foi excluído)
- `changePlan` (quando estiver concluída, a operação de alteração de plano)
- `changeQuantity` (quando estiver concluída, a operação de quantidade de alteração)
- `suspend` (quando recursos foi suspenso)
- `reinstate` (quando recursos foi restabelecido após suspensão)

Em que o status pode ser um dos seguintes: 
- **NotStarted** <br>
 - **InProgress** <br>
- **Êxito** <br>
- **Com falha** <br>
- **Conflict** <br>

Em uma notificação de webhook, status acionáveis são **bem-sucedido** e **falha**. Ciclo de vida de uma operação é de **NotStarted** para um estado terminal como **Succeeded**, **falha**, ou **conflito**. Se você receber **NotStarted** ou **InProgress**, continue solicitar o status por meio da API obter até que a operação atinja um estado terminal antes de tomar. 

## <a name="mock-apis"></a>APIs de simulação

Você pode usar nossas APIs fictícios para ajudá-lo a começar com o desenvolvimento, especialmente criação de protótipos, testes, bem como projetos. 

Hospedar o ponto de extremidade: `https://marketplaceapi.microsoft.com/api` (nenhuma autenticação necessária)<br/>
Versão da API: `2018-09-15`<br/>
URI de exemplo: `https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

Os caminhos de ponto de extremidade de API são os mesmos em APIs fictícios e real, mas as versões de API são diferentes. É a versão `2018-09-15` para obter a versão fictícia e `2018-08-31` para a versão de produção. 

Qualquer uma das chamadas à API neste artigo podem ser feitas para o ponto de extremidade do host fictício. Em geral, espere receber dados fictícios, como uma resposta. Chamadas para os métodos de assinatura de atualização sobre a API de simulação sempre retornam 500. 

## <a name="next-steps"></a>Próximas etapas

Os desenvolvedores podem também programaticamente recuperar e manipular cargas de trabalho, ofertas e perfis de publicador usando o [as APIs REST do Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).

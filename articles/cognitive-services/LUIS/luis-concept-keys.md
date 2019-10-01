---
title: Criação e chaves de tempo de execução-LUIS
titleSuffix: Azure Cognitive Services
description: O LUIS usa duas chaves, a chave de criação para criar seu modelo e a chave de tempo de execução para consultar o ponto de extremidade de previsão com o usuário declarações.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 70e58077fa40ce685324cd24b447886ec3411034
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703193"
---
# <a name="authoring-and-runtime-keys"></a>Chaves de criação e de runtime


>[!NOTE]
>[Migre](luis-migration-authoring.md) todos os aplicativos, que não usam o recurso de criação do Azure, antes de continuar.

O LUIS usa dois tipos de recursos do Azure, cada tipo tem chaves: 
 
* [Criação](#programmatic-key) para criar tentativas, entidades e rotular declarações, treinar e publicar. Quando estiver pronto para publicar seu aplicativo LUIS, você precisará de uma [chave de ponto de extremidade de previsão para o tempo de execução](luis-how-to-azure-subscription.md) atribuído ao aplicativo.
* [Chave de ponto de extremidade de previsão para o tempo de execução](#prediction-endpoint-runtime-key). Aplicativos cliente, como um bot de chat, precisam acessar o ponto de extremidade de **previsão de consulta** do tempo de execução por meio dessa chave. 

|Chave|Finalidade|@No__t de serviço cognitiva-0|@No__t de serviço cognitiva-0|
|--|--|--|--|
|[Chave de criação](#programmatic-key)|Criação, treinamento, publicação, teste.|`LUIS.Authoring`|`Cognitive Services`|
|[Chave de tempo de execução de ponto de extremidade de previsão](#prediction-endpoint-runtime-key)| Tempo de execução de ponto de extremidade de previsão de consulta com um usuário expressão para determinar tentativas e entidades.|`LUIS`|`Cognitive Services`|

O LUIS também fornece uma [chave inicial](luis-how-to-azure-subscription.md#starter-key) com uma cota de ponto de extremidade de previsão de transações de 1000 por mês. 

Embora você não precise criar as duas chaves ao mesmo tempo, é muito mais fácil se você fizer isso.

É importante criar aplicativos LUIS em [regiões](luis-reference-regions.md#publishing-regions) em que você deseja publicar e consultar.

<a name="programmatic-key" ></a>

## <a name="authoring-key"></a>Chave de criação

Uma chave de criação é criada automaticamente quando você cria uma conta do LUIS e é gratuita. Quando você começa com o LUIS, você tem uma chave de início em todos os seus aplicativos LUIS para cada [região](luis-reference-regions.md)de criação. A finalidade da chave de criação é fornecer autenticação para gerenciar seu aplicativo LUIS ou para testar consultas de ponto de extremidade de previsão. 

A criação de chaves de criação no portal do Azure permite que você controle permissões para o recurso de criação atribuindo pessoas à [função colaborador](#contributions-from-other-authors). Você precisa de permissão no nível de assinatura do Azure para adicionar colaboradores. 

Para localizar a chave de criação, entre no [LUIS](luis-reference-regions.md#luis-website) e clique no nome da conta na barra de navegação superior direita para abrir as **Configurações de Conta**.

![chave de criação](./media/luis-concept-keys/authoring-key.png)

Quando você quiser fazer **consultas de tempo de execução**, crie o [recurso Luis](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/)do Azure. 

> [!CAUTION]
> Para sua conveniência, muitos dos exemplos usam a [chave inicial](#starter-prediction-endpoint-runtime-key) porque ele fornece algumas chamadas de ponto de extremidade de previsão gratuitas em sua [cota](luis-boundaries.md#key-limits).  

<a name="endpoint-key"></a>

## <a name="prediction-endpoint-runtime-key"></a>Chave de tempo de execução de ponto de extremidade de previsão 

Quando você precisar de **consultas de ponto de extremidade de tempo de execução**, crie um recurso de reconhecimento vocal (Luis) e, em seguida, atribua-o ao aplicativo Luis. 

[!INCLUDE [Azure runtime resource creation for Language Understanding and Cognitive Service resources](../../../includes/cognitive-services-luis-azure-resource-instructions.md)]

Quando o processo de criação de recursos for concluído, [atribua a chave](luis-how-to-azure-subscription.md) ao aplicativo. 

* A chave de tempo de execução (ponto de extremidade de previsão de consulta) permite uma cota de ocorrências de ponto de extremidade com base no plano de uso especificado ao criar a chave de tempo de execução. Confira [Preços de Serviços Cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) para obter informações sobre preços.

* A chave de tempo de execução pode ser usada para todos os seus aplicativos LUIS ou para aplicativos LUIS específicos. 
* Não use a chave de tempo de execução para criar aplicativos LUIS. 

### <a name="starter-prediction-endpoint-runtime-key"></a>Chave de tempo de execução de ponto de extremidade de previsão iniciada

A chave do ponto de extremidade de previsão **inicial** é fornecida gratuitamente e inclui consultas de ponto de extremidade de previsão 1000. Depois que essas consultas forem usadas, você deverá criar seu próprio recurso de ponto de extremidade de previsão para Reconhecimento vocal.  

Esse é um recurso especial criado para você. Ela não aparece na lista de recursos do Azure porque ela se destina como uma chave inicial temporária. 

<a name="use-endpoint-key-in-query"></a>

### <a name="use-runtime-key-in-query"></a>Usar chave de tempo de execução na consulta
O ponto de extremidade do LUIS Runtime aceita dois estilos de consulta, ambos usam a chave de tempo de execução de ponto de extremidade de previsão, mas em locais diferentes.

O ponto de extremidade usado para acessar o tempo de execução usa um subdomínio que é exclusivo para a região do seu recurso, indicado com `{region}` na tabela a seguir. 


#### <a name="v2-prediction-endpointtabv2"></a>[Ponto de extremidade de previsão v2](#tab/V2)

|Verbo|Local da chave e da URL de exemplo|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?runtime-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`|

#### <a name="v3-prediction-endpointtabv3"></a>[Ponto de extremidade de previsão v3](#tab/V3)

|Verbo|Local da chave e da URL de exemplo|
|--|--|
|[GET](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0-preview/operations/5cb0a91e54c9db63d589f433)|`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?runtime-key=your-endpoint-key-here&query=turn%20on%20the%20lights`|
|[POST](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0-preview/operations/5cb0a5830f741b27cd03a061)| `https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict`| 

Saiba mais sobre o [ponto de extremidade de previsão v3](luis-migration-api-v3.md).

* * * 

**OBTER**: Altere o valor da consulta do ponto de extremidade para o `runtime-key` da chave de criação (de início) para a nova chave do ponto de extremidade para usar a taxa de cota da chave do ponto de extremidade LUIS. Se você criar a chave e atribuí-la, mas não alterar o valor da consulta do ponto de extremidade para `runtime-key`, você não usará sua cota da chave do ponto de extremidade.

**POST**: Alterar o valor do cabeçalho para `Ocp-Apim-Subscription-Key`<br>Se você criar a chave de tempo de execução e atribuir a chave de tempo de execução, mas não alterar o valor de consulta de ponto de extremidade para `Ocp-Apim-Subscription-Key`, você não está usando sua chave de tempo de execução.

A ID do aplicativo usado nas URLs anteriores, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, é o aplicativo público de IoT usado para a [demonstração interativa](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="assignment-of-the-runtime-key"></a>Atribuição da chave de tempo de execução

Você pode [atribuir](luis-how-to-azure-subscription.md) a chave de tempo de execução no [portal do Luis](https://www.luis.ai) ou por meio das APIs correspondentes. 

## <a name="key-limits"></a>Limites de chave

Você pode criar até 10 chaves de criação por região por assinatura. 

Consulte [limites de chave](luis-boundaries.md#key-limits) e [regiões do Azure](luis-reference-regions.md). 

Regiões de publicação são diferentes de regiões de criação. Certifique-se de criar um aplicativo na região de criação correspondente à região de publicação que você deseja que seu aplicativo cliente esteja localizado.

## <a name="key-limit-errors"></a>Erros no limite de chave
Se você exceder sua cota de transações por segundo (TPS), receberá um erro HTTP 429. Se você exceder sua cota de transação por mês (TPS), receberá um erro HTTP 403. 

## <a name="contributions-from-other-authors"></a>Contribuições de outros autores



O gerenciamento de contribuições de colaboradores depende do status atual do aplicativo.

**Para [criar aplicativos migrados de recursos](luis-migration-authoring.md)** : os _colaboradores_ são gerenciados no portal do Azure para o recurso de criação, usando a página de **controle de acesso (iam)** . Saiba [como adicionar um usuário](luis-how-to-collaborate.md), usando o endereço de email da colaboração e a função _colaborador_ . 

**Para aplicativos que ainda não foram migrados**: todos os _colaboradores_ são gerenciados no portal do Luis a partir da página **Gerenciar > colaboradores** .

### <a name="contributor-roles-vs-entity-roles"></a>Funções de colaborador vs. funções de entidade

As [funções de entidade](luis-concept-roles.md) se aplicam ao modelo de dados do aplicativo Luis. As funções colaborador/colaborador se aplicam a níveis de acesso de criação. 

## <a name="moving-or-changing-ownership"></a>Movendo ou alterando a propriedade

Um aplicativo é definido por seus recursos do Azure, que é determinado pela assinatura do proprietário. 

Você pode mover seu aplicativo LUIS. Use os seguintes recursos de documentação no portal do Azure ou CLI do Azure:

* [Mover o aplicativo entre os recursos de criação do LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Mover recurso para novo grupo de recursos ou assinatura](../../azure-resource-manager/resource-group-move-resources.md)
* [Mover recurso na mesma assinatura ou em assinaturas](../../azure-resource-manager/move-limitations/app-service-move-limitations.md)
* [Transferir a propriedade](../../billing/billing-subscription-transfer.md) de sua assinatura 

## <a name="access-for-private-and-public-apps"></a>Acesso para aplicativos públicos e privados

Para um aplicativo **privado** , o acesso ao tempo de execução está disponível para proprietários e colaboradores. Para um aplicativo **público** , o acesso ao tempo de execução está disponível para todos que têm seu próprio [serviço cognitiva](../cognitive-services-apis-create-account.md) do Azure ou recurso de tempo de execução [Luis](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) e tem a ID do aplicativo público. 

Atualmente, não há um catálogo de aplicativos públicos.

### <a name="authoring-access"></a>Acesso de criação
O acesso ao aplicativo no portal do [Luis](luis-reference-regions.md#luis-website) ou nas [APIs de criação](https://go.microsoft.com/fwlink/?linkid=2092087) é controlado pelo recurso de criação do Azure. 

O proprietário e todos os colaboradores têm acesso para criar o aplicativo. 

|O acesso de criação inclui|Observações|
|--|--|
|Adicionar ou remover chaves de ponto de extremidade||
|Versão de exportação||
|Exportar logs de ponto de extremidade||
|Versão de importação||
|Tornar um aplicativo público|Quando um aplicativo for público, qualquer pessoa com uma chave de criação ou de ponto de extremidade poderá consultá-lo.|
|Modificar modelo|
|Publicar|
|Examinar declarações de ponto de extremidade para [aprendizado ativo](luis-how-to-review-endpoint-utterances.md)|
|Trem|

### <a name="prediction-endpoint-runtime-access"></a>Acesso ao tempo de execução de ponto de extremidade de previsão

O acesso para consultar o ponto de extremidade de previsão é controlado por uma configuração na página de **informações do aplicativo** na seção **gerenciar** . 

![Definir aplicativo para público](./media/luis-concept-security/set-application-as-public.png)

|[Ponto de extremidade privado](#runtime-security-for-private-apps)|[Ponto de extremidade público](#runtime-security-for-public-apps)|
|:--|:--|
|Disponível para o proprietário e os colaboradores|Disponível para o proprietário, colaboradores e qualquer outra pessoa que conheça a ID do aplicativo|

Você pode controlar quem vê sua chave de tempo de execução LUIS chamando-a em um ambiente de servidor para servidor. Se você estiver usando o LUIS de um bot, a conexão entre o bot e o LUIS já estará segura. Se você estiver chamando o ponto de extremidade LUIS diretamente, deverá criar a API do servidor (como uma [função](https://azure.microsoft.com/services/functions/) do Azure) com acesso controlado (como [AAD](https://azure.microsoft.com/services/active-directory/)). Quando a API do lado do servidor for chamada e autenticada e a autorização for verificada, passe a chamada para LUIS. Embora essa estratégia não impeça ataques man-in-the-Middle, ela ofusca sua chave e a URL do ponto de extremidade dos usuários, permite que você acompanhe o acesso e permite que você adicione o log de resposta do ponto de extremidade (como [Application insights](https://azure.microsoft.com/services/application-insights/)).

#### <a name="runtime-security-for-private-apps"></a>Segurança de tempo de execução para aplicativos privados

O tempo de execução de um aplicativo privado só está disponível para o seguinte:

|Chave e usuário|Explicação|
|--|--|
|Chave de criação do proprietário| Até 1000 ocorrências de ponto de extremidade|
|Colaboradores/chaves de criação de colaborador| Até 1000 ocorrências de ponto de extremidade|
|Qualquer chave atribuída a LUIS por um autor ou colaboração/colaborador|Com base na camada de uso da chave|

#### <a name="runtime-security-for-public-apps"></a>Segurança de tempo de execução para aplicativos públicos

Depois que um aplicativo for configurado como público, _qualquer_ chave de criação LUIS válida ou chave do ponto de extremidade LUIS poderá consultar seu aplicativo, desde que a chave não tenha usado toda a cota de ponto de extremidade.

Um usuário que não seja um proprietário ou colaborador, só poderá acessar o tempo de execução de um aplicativo público se a ID do aplicativo for fornecida. O LUIS não tem um _mercado_ público ou outra maneira de pesquisar um aplicativo público.  

Um aplicativo público é publicado em todas as regiões para que um usuário com uma chave de recurso do LUIS baseada em região possa acessar o aplicativo em qualquer região associada com a chave de recurso.

## <a name="transfer-of-ownership"></a>Transferência de propriedade

**Para [criar aplicativos migrados de recursos](luis-migration-authoring.md)** : Como o proprietário do recurso, você pode adicionar um `contributor`.

**Para aplicativos que ainda não foram migrados**: Exporte seu aplicativo como um arquivo JSON. Outro usuário do LUIS pode importar o aplicativo, tornando-se o proprietário do aplicativo. O novo aplicativo terá uma ID de aplicativo diferente.  

## <a name="securing-the-endpoint"></a>Protegendo o ponto de extremidade 

Você pode controlar quem pode ver sua chave de ponto de extremidade do LUIS de tempo de execução de previsão chamando-a em um ambiente de servidor para servidor. Se você estiver usando o LUIS de um bot, a conexão entre o bot e o LUIS já estará segura. Se você estiver chamando o ponto de extremidade LUIS diretamente, deverá criar a API do servidor (como uma [função](https://azure.microsoft.com/services/functions/) do Azure) com acesso controlado (como [AAD](https://azure.microsoft.com/services/active-directory/)). Quando a API do servidor for chamada e a autenticação e a autorização forem verificadas, passe a chamada para o LUIS. Embora essa estratégia não impeça ataques "man-in-the-middle", ela oculta seu ponto de extremidade dos usuários, permite que você controle o acesso e que você adicione um log de resposta de ponto de extremidade (como o [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="next-steps"></a>Próximas etapas

* Entender os conceitos de [controle de versão](luis-concept-version.md). 
* Saiba [como criar chaves](luis-how-to-azure-subscription.md).

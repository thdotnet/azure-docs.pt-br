---
title: Criar uma conta de Serviços Cognitivos no portal do Azure
titlesuffix: Azure Cognitive Services
description: Como criar uma conta de APIs de serviços Cognitivos do Azure no portal do Azure.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: aahi
ms.openlocfilehash: b857ee0395c447c8699b8f6a812853528812a7bd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445864"
---
# <a name="create-a-cognitive-services-account-using-the-azure-portal"></a>Criar uma conta de serviços Cognitivos usando o portal do Azure

Neste início rápido, você aprenderá como inscrever-se para os serviços Cognitivos do Azure e criar uma conta que tenha uma assinatura de serviço único ou vários serviço. Esses serviços são representados pelos [recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) do Azure, que permitem a conexão com uma ou mais APIs de Serviços Cognitivos do Azure.

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura válida do Azure. [Criar uma conta](https://azure.microsoft.com/free/) gratuitamente.

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Criar um novo recurso de serviços Cognitivos do Azure

Antes de criar um recurso, você deve ter um grupo de recursos do Azure. Cada conta dos serviços Cognitivos (e seus recursos do Azure associados) devem pertencer a um grupo de recursos do Azure. Quando você cria uma conta, você tem a opção para criar um novo grupo de recursos, ou usar um existente. Este artigo mostra como criar um novo grupo de recursos.

1. Entrar para o [portal do Azure](https://portal.azure.com), clique em  **criar+ um recurso**.

    ![Selecionar APIs de Serviços Cognitivos](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Você pode encontrar os serviços Cognitivos disponíveis com o das seguintes maneiras:
    * Use a barra de pesquisa e insira o nome do serviço que você deseja assinar.
        * Para criar um recurso para uma assinatura de vários serviço, digite **dos serviços Cognitivos** na pesquisa de barras e selecione o **dos serviços Cognitivos** recursos.

        ![Pesquise pelos Serviços Cognitivos](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    * Para ver todos os serviços cognitivos disponíveis, selecione **IA + Machine Learning**, em **do Azure Marketplace**. Se você não vir o serviço que você está interessado, clique em **ver todos** e role até **dos serviços Cognitivos**. Clique em **mais** para exibir todo o catálogo de serviços Cognitivos.
    
        ![Selecionar APIs de Serviços Cognitivos](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. Na página **Criar**, forneça as seguintes informações:

    > [!IMPORTANT]
    > Lembre-se o local do Azure, pois você talvez tenha ao chamar os serviços Cognitivos do Azure.

    |    |    |
    |--|--|
    | **Nome** | Um nome descritivo para o recurso de serviços cognitivos. É recomendável usar um nome descritivo, por exemplo *MyCognitiveServicesAccount*. |
    | **Assinatura** | Selecione uma das suas assinaturas do Azure disponíveis. |
    | **Localidade** | A localização da sua instância de serviço cognitivo. Locais diferentes podem introduzir latência, mas não têm impacto sobre a disponibilidade de tempo de execução do seu recurso. |
    | **Tipo de preços** | O custo da sua conta de Serviços Cognitivos depende das opções escolhidas e do seu uso. Para obter mais informações, consulte a API [detalhes de preços](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Grupo de recursos** | O [grupo de recursos do Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) que conterá o recurso de serviços Cognitivos. Você pode criar um grupo ou adicioná-lo a um grupo pré-existente. |

    ![Tela de criação de recursos](media/cognitive-services-apis-create-account/resource_create_screen.png)


## <a name="get-the-keys-for-your-subscription"></a>Obter as chaves para sua assinatura

Depois de criar seu recurso, você pode acessá-lo no painel do Azure se você fixou-o. Caso contrário, você pode encontrá-lo em **Grupos de Recursos**. Depois de selecionar o recurso, você pode obter as chaves, selecionando **teclas** sob **gerenciamento de recursos**.

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Se você quiser limpar e remover uma assinatura dos serviços Cognitivos, você pode excluir o recurso ou grupo de recursos. Também é excluir o grupo de recursos exclui todos os outros recursos associados com o grupo de recursos.

Para remover um grupo de recursos usando o portal do Azure:

1. No portal do Azure, expanda o menu à esquerda para abrir o menu de serviços e escolha **Grupo de Recursos** para exibir a lista dos seus grupos de recursos.
2. Localize o grupo de recursos para excluir e clique no botão de mais (...) no lado direito da listagem.
3. Selecione **Excluir grupo de recursos** e confirme.

## <a name="see-also"></a>Consulte também

* [Autenticar solicitações para os Serviços Cognitivos do Azure](authentication.md)
* [Quais são os serviços Cognitivos do Azure?](Welcome.md)
* [Suporte de idioma natural](language-support.md)
* [Suporte de contêiner do docker](cognitive-services-container-support.md)

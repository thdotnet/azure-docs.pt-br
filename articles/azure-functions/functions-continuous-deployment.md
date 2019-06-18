---
title: Implantação contínua do Azure Functions | Microsoft Docs
description: Use os recursos de implantação contínua do serviço de aplicativo do Azure para publicar suas funções.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: d4d2f24a0a7b1f01627ed2cea4a5732ca0e001c9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068353"
---
# <a name="continuous-deployment-for-azure-functions"></a>Implantação contínua para Azure Functions

O Azure Functions permite que você implante seu código continuamente por meio [integração de controle de origem](functions-deployment-technologies.md#source-control). Isso permite que um fluxo de trabalho em que o código atualiza o gatilho de implantação no Azure. Se você for novo no Azure Functions, comece com o [visão geral do Azure Functions](functions-overview.md).

Implantação contínua é uma ótima opção para projetos em que você estiver integrando vários e frequentes contribuições. Ele também permite manter uma única fonte verdadeira para o código de função. Você pode configurar a implantação contínua no Azure Functions nos seguintes locais de código de origem:

* [Repositórios do Azure](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

A unidade de implantação para o Azure functions é o aplicativo de funções. Isso significa que todas as funções em um aplicativo de funções são implantadas ao mesmo tempo. Depois que a implantação contínua é habilitada, o acesso ao código de função no portal do Azure é configurado como *somente leitura*, uma vez que a fonte da verdade é definida para ser em outro lugar.

## <a name="requirements-for-continuous-deployment"></a>Requisitos para implantação contínua

Para a implantação contínua seja bem-sucedida, sua estrutura de diretório deve ser compatível com a seguinte estrutura de pasta básica que espera de Azure Functions:

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="credentials"></a>Configurar implantação contínua

Utilize o procedimento a seguir para configurar a implantação contínua para um aplicativo de função existente. Essas etapas demonstram a integração com um repositório do GitHub, mas etapas semelhantes se aplicam para repositórios do Azure ou outros repositórios de código-fonte.

1. Em seu aplicativo de função na [portal do Azure](https://portal.azure.com), selecione **recursos da plataforma** > **Deployment center**.

    ![Abrindo o Centro de implantação](./media/functions-continuous-deployment/platform-features.png)

2. Sobre o **Deployment Center**, selecione **GitHub** para **controle de origem** > **autorizar**.

    ![Central de implantação](./media/functions-continuous-deployment/github.png)

3. Selecione **autorizar AzureAppService** > **continuar**.

    ![Autorizando](./media/functions-continuous-deployment/authorize.png)

4. Selecione um dos seguintes provedores de compilação:

    * **O serviço de aplicativo de serviço de compilação** – melhor quando você não precisa de um build ou se precisar de uma compilação genérica.
    * **Pipelines do Azure (visualização)** – melhor quando você precisar de mais controle sobre a compilação. Esse provedor está atualmente em visualização.

    ![Selecionando um provedor de build](./media/functions-continuous-deployment/build.png)

5. Configure as informações específicas para a opção de controle de origem especificado. Para o GitHub, você deve fornecer o **organização**, **repositório**, e **Branch** na qual reside o seu código. Em seguida, selecione **Continuar**.

    ![Configurando o GitHub](./media/functions-continuous-deployment/github-specifics.png)

6. Por fim, examine todos os detalhes e selecione **concluir** para concluir a configuração de implantação.

    ![Resumo](./media/functions-continuous-deployment/summary.png)

Quando o processo for concluído, todo o código da origem especificada é implantado em seu aplicativo. Nesse ponto, as alterações na fonte de implantação disparam uma implantação dessas alterações ao seu aplicativo de funções no Azure.

## <a name="deployment-scenarios"></a>Cenários de implantação

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Mover as funções existentes para implantação contínua

Se você já escreveu funções [portal do Azure](https://portal.azure.com) e não quiser baixar o conteúdo de seu aplicativo antes de alternar para a implantação contínua, você deve navegar até a **visão geral** guia da sua função aplicativo e clique no **baixar conteúdo do aplicativo** botão.

![Download do conteúdo do aplicativo](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Depois de configurar a integração contínua, você não pode editar seus arquivos de origem no portal do Functions.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Práticas recomendadas para o Azure Functions](functions-best-practices.md)

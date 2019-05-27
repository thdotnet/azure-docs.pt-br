---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 02/19/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 97387e24d5b55c1438a69da1a1fd0a9bc1720e47
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66136159"
---
1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto e selecione **Publicar**.

1. No **Publish** caixa de diálogo, selecione **serviço de aplicativo do Microsoft Azure**, escolha **criar novo**e, em seguida, selecione **publicar**.

   ![Escolha o destino de publicação](./media/webjobs-publish-netcore/pick-publish-target.png)

1. No **criar serviço de aplicativo** caixa de diálogo, use as configurações de hospedagem conforme especificadas na tabela abaixo da imagem:

    ![Criar caixa de diálogo do Serviço de Aplicativo](./media/webjobs-publish-netcore/app-service-dialog.png)

    | Configuração      | Valor sugerido  | Descrição                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome do aplicativo** | Nome globalmente exclusivo | Nome que identifica seu novo aplicativo de funções de forma exclusiva. |
    | **Assinatura** | Escolha sua assinatura | A assinatura do Azure a utilizar. |
    | **[Grupo de Recursos](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Nome do grupo de recursos no qual criar o seu aplicativo de funções. Escolha **Novo** para criar um novo grupo de recursos.|
    | **[Plano de hospedagem](../articles/app-service/overview-hosting-plans.md)** | plano de Serviço de Aplicativo | Um [plano do Serviço de Aplicativo](../articles/app-service/overview-hosting-plans.md) especifica o local, tamanho e recursos do farm de servidores Web que hospeda o aplicativo. Você pode economizar dinheiro ao hospedar vários aplicativos configurando os aplicativos Web para compartilhar um único plano do Serviço de Aplicativo. Planos do serviço de aplicativo definem a região, tamanho da instância, contagem da escala e SKU (gratuito, compartilhado, básico, Standard ou Premium). Escolher **New** para criar um novo plano do serviço de aplicativo. |

1. Clique em **criar** para criar um trabalho Web e recursos relacionados no Azure com essas configurações e implantar o código do projeto.
---
title: Configurar o push do Baidu Cloud nos hubs de notificação do Azure | Microsoft Docs
description: Saiba como definir as configurações de Baidu para um hub de notificação do Azure.
services: notification-hubs
author: jwargo
manager: patniko
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 9feb5f66c5ae979a239c3349b2b1347ad307ce49
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70884653"
---
# <a name="deprecated-configure-baidu-cloud-push-settings-for-a-notification-hub-in-the-azure-portal"></a>Preterido Definir as configurações de push do Baidu Cloud para um hub de notificação no portal do Azure

Este artigo mostra como definir as configurações de push do Baidu Cloud para um hub de notificação do Azure usando o portal do Azure.

> [!IMPORTANT]
> Este tutorial foi preterido. 

## <a name="prerequisites"></a>Pré-requisitos
Se você ainda não criou um hub de notificação, crie um agora. Para obter mais informações, consulte [Criar um hub de notificação do Azure no portal do Azure](create-notification-hub-portal.md). 

## <a name="configure-baidu-cloud-push"></a>Configurar o push da nuvem Baidu
O procedimento a seguir fornece as etapas para definir as configurações de push do Baidu Cloud para um hub de notificação:

1. Na portal do Azure, na página **Hub de notificação** , selecione **Baidu (Android China)** no menu à esquerda. 
2. Insira a **chave de API** obtida do console do Baidu no projeto de push da nuvem Baidu. 
3. Insira a **chave secreta** obtida do console do Baidu no projeto de push da nuvem Baidu. 
4. Clique em **Salvar**. 

    ![Captura de tela de Hubs de Notificação que mostra a configuração para notificações por push do Baidu (Android China)](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

## <a name="next-steps"></a>Próximas etapas
Para obter um tutorial com as instruções passo a passo para enviar notificações por push ao Baidu usando os hubs de notificação do Azure e o Baidu Cloud Push, consulte Introdução [aos hubs de notificação usando o Baidu](notification-hubs-baidu-china-android-notifications-get-started.md).

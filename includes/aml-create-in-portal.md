---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/31/2019
ms.openlocfilehash: 7be5b0dbe4bbfba30ea469eec662877c1ef56c7c
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689617"
---
1. Entre no [portal do Microsoft Azure](https://portal.azure.com/) usando as credenciais de assinatura do Azure que você usará. 

1. No canto superior esquerdo do portal do Azure, selecione **Criar um recurso**.

1. Use a barra de pesquisa para localizar o **Workspace de serviço do Machine Learning**.

1. Selecione **Workspace de serviço do Machine Learning**.

1. No painel **Espaço de trabalho do serviço do Machine Learning**, escolha **Criar** para começar.

1. Configure o novo workspace fornecendo o respectivo nome, assinatura, grupo de recursos e local.

    ![Criar workspace](./media/aml-create-in-portal/workspace-create-main-tab.png)

   Campo|DESCRIÇÃO
   ---|---
   Nome do workspace |Insira um nome único que identifique seu workspace. Para este exemplo, usamos **docs-ws**. Os nomes devem ser únicos em todo o grupo de recursos. Use um nome que seja fácil de lembrar e diferenciar de workspaces criados por outras pessoas.  
   Subscription |Selecione a assinatura do Azure que você deseja usar.
   Resource group | Use um grupo de recursos existente na sua assinatura ou insira um nome para criar um grupo de recursos. Um grupo de recursos mantém os recursos relacionados a uma solução do Azure. Para este exemplo, usamos **docs-aml**. 
   Location | Selecione a localização mais próxima aos usuários e recursos de dados para criar o workspace.

1. Quando terminar de configurar o workspace, selecione **Criar**. 

   Pode demorar um pouco para o workspace ser criado.

   Quando o processo é finalizado, será exibida uma mensagem de êxito da implantação. Ele também está presente na seção notificações. Para exibir o novo espaço de trabalho, selecione **Ir para o recurso**.

   ![Status de criação do espaço de trabalho](./media/aml-create-in-portal/notifications.png)

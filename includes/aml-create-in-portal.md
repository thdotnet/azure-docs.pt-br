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
ms.date: 05/21/2019
ms.openlocfilehash: 2ec45b67367198c14fc9d03cdb659a51aed8a504
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841554"
---
1. Entre no [portal do Microsoft Azure](https://portal.azure.com/) usando as credenciais de assinatura do Azure que você usará. 

   ![Portal do Azure](./media/aml-create-in-portal/portal-dashboard-05-2019.png)

1. No canto superior esquerdo do Portal, selecione **Criar um recurso**.

   ![Criar um recurso no Portal do Azure](./media/aml-create-in-portal/portal-create-a-resource-07-2019.png)

1. Use a barra de pesquisa para selecionar **Workspace de serviço do Machine Learning**.

   ![Pesquisa de workspace](./media/aml-create-in-portal/allservices-search.png)

1. No painel **Espaço de trabalho do serviço do Machine Learning**, escolha **Criar** para começar.

    ![Botão Criar](./media/aml-create-in-portal/portal-create-button.png)

1. No painel **Espaço de trabalho de serviço ML**, configure seu espaço de trabalho.

    ![Criar workspace](./media/aml-create-in-portal/workspace-create-main-tab.png)

   Campo|DESCRIÇÃO
   ---|---
   Nome do workspace |Insira um nome único que identifique seu workspace. Para este exemplo, usamos **docs-ws**. Os nomes devem ser únicos em todo o grupo de recursos. Use um nome que seja fácil de lembrar e diferenciar de espaços de trabalho criados por outras pessoas.  
   Assinatura |Selecione a assinatura do Azure que você deseja usar.
   Grupo de recursos | Use um grupo de recursos existente na sua assinatura ou insira um nome para criar um novo grupo de recursos. Um grupo de recursos mantém os recursos relacionados a uma solução do Azure. Para este exemplo, usamos **docs-aml**. 
   Local padrão | Selecione a localização mais próxima a seus usuários e recursos de dados. Este local é onde o local de trabalho é criado.

1. Examine a configuração do workspace e selecione **Criar**. Pode demorar um pouco para o workspace ser criado.

1. Quando o processo é finalizado, será exibida uma mensagem de êxito da implantação. Ele também está presente na seção notificações. Para exibir o novo espaço de trabalho, selecione **Ir para o recurso**.

   ![Status de criação do espaço de trabalho](./media/aml-create-in-portal/notifications.png)

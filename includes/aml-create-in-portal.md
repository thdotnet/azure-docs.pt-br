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
ms.openlocfilehash: a33eb98525ea857ee52ad2fffa5937207504909d
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720554"
---
1. Entre no [portal do Microsoft Azure](https://portal.azure.com/) usando as credenciais de assinatura do Azure que você usará. 

   ![Portal do Azure](./media/aml-create-in-portal/portal-dashboard-05-2019.png)

1. No canto superior esquerdo do Portal, selecione **Criar um recurso**.

   ![Criar um recurso no Portal do Azure](./media/aml-create-in-portal/portal-create-a-resource-07-2019.png)

1. Na barra de pesquisa, digite **Machine Learning**. Selecione o resultado da pesquisa do **Espaço de trabalho de serviço de Machine Learning**.

   ![Pesquisa de workspace](./media/aml-create-in-portal/allservices-search.png)

1. No painel **Espaço de trabalho do serviço do Machine Learning**, escolha **Criar** para começar.

    ![Botão Criar](./media/aml-create-in-portal/portal-create-button.png)

1. No painel **Espaço de trabalho de serviço ML**, configure seu espaço de trabalho.

   Campo|DESCRIÇÃO
   ---|---
   Nome do workspace |Insira um nome único que identifique seu workspace. Para este exemplo, usamos **docs-ws**. Os nomes devem ser únicos em todo o grupo de recursos. Use um nome que seja fácil de lembrar e diferenciar de espaços de trabalho criados por outras pessoas.  
   Assinatura |Selecione a assinatura do Azure que você deseja usar.
   Grupo de recursos | Use um grupo de recursos existente na sua assinatura ou insira um nome para criar um novo grupo de recursos. Um grupo de recursos mantém os recursos relacionados a uma solução do Azure. Para este exemplo, usamos **docs-aml**. 
   Local padrão | Selecione a localização mais próxima a seus usuários e recursos de dados. Este local é onde o local de trabalho é criado.

1. Para iniciar o processo de criação, escolha **Examinar + Criar**.

    ![Criar workspace](./media/aml-create-in-portal/workspace-create-main-tab.png)

1. Examine a configuração do espaço de trabalho. Se estiver correta, escolha **Criar**. Pode demorar um pouco para o workspace ser criado.

1. Para verificar o status da implantação, selecione o ícone de notificações **sino** na barra de ferramentas.

1. Quando o processo é finalizado, será exibida uma mensagem de êxito da implantação. Ele também está presente na seção notificações. Para exibir o novo espaço de trabalho, selecione **Ir para o recurso**.

   ![Status de criação do espaço de trabalho](./media/aml-create-in-portal/notifications.png)

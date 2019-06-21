---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: d90ef1737f0107e5e323c1e90e3f9021efd4e4f7
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205796"
---
### <a name="prerequisites"></a>Pré-requisitos
* Uma conta [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)  

Antes de usar a conta SMTP em um aplicativo lógico, você deve autorizar o aplicativo lógico a se conectar à sua conta SMTP. Felizmente, você pode fazer isso facilmente dentro do seu aplicativo lógico no Portal do Azure.  

Aqui estão as etapas para autorizar seu aplicativo lógico a se conectar à sua conta SMTP:  

1. Para criar uma conexão com o SMTP, no designer do aplicativo lógico, selecione **Mostrar APIs gerenciadas da Microsoft** na lista suspensa, em seguida, digite *SMTP* na caixa de pesquisa. Selecione o gatilho ou ação que gostaria de usar:  
   ![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Se você não tiver criado nenhuma conexão com o SMTP antes, suas credenciais do SMTP serão solicitadas. Essas credenciais serão usadas para autorizar seu aplicativo lógico a se conectar aos dados da sua conta SMTP e usá-los:  
   ![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Observe que a conexão foi criada e agora você pode continuar com as outras etapas em seu aplicativo lógico:  
   ![](./media/connectors-create-api-smtp/smtp-3.png)  


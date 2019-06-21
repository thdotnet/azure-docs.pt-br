---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: 0cabc58d856c09accd9b1924fe63d6518b1cb9ef
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205787"
---
Para se conectar ao **SharePoint Online**, você precisa fornecer sua identidade (nome de usuário e senha, credenciais de cartão inteligente, etc.) para o SharePoint Online. Depois de se autenticar, você poderá continuar e usar o Conector do SharePoint Online em seu aplicativo lógico. 

No designer do aplicativo lógico, siga estas etapas para entrar no SharePoint e criar a **conexão** que será usada em seu aplicativo lógico:

1. Insira SharePoint na caixa de pesquisa e aguarde até que a pesquisa retorne todos os gatilhos com SharePoint Online no nome:   
   ![Configurar o SharePoint][1]  
2. Selecione o gatilho **SharePoint Online - Quando um arquivo é criado**  
3. Selecione **Entrar no SharePoint Online**:   
   ![Configurar o SharePoint][2]    
4. Forneça suas credenciais do SharePoint para entrar e se autenticar com o SharePoint   
   ![Configurar o SharePoint][3]     
5. Após a autenticação, você será redirecionado ao seu aplicativo lógico. Pronto, a conexão foi criada. Observe a mensagem na parte inferior que indica que você agora está conectado ao SharePoint.  
   ![Configurar o SharePoint][4]  
6. Em seguida, é possível adicionar outros gatilhos e outras ações necessárias para concluir seu aplicativo lógico.   

[1]: ./media/connectors-create-api-sharepointonline/connectionconfig1.png
[2]: ./media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ./media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ./media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ./media/connectors-create-api-sharepointonline/connectionconfig5.png

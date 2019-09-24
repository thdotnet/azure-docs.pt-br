---
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 11/09/2018
ms.author: clemensv
ms.openlocfilehash: 3e3617f68d58e51f24affaae7d69812195b72fd0
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71210142"
---
1. Entre no [Portal do Azure][Azure portal].
1. Selecione **Criar um recurso**. Em seguida, selecione **Integração** > **Retransmissão**. Se a opção **Retransmissão** não for exibida na lista, selecione **Ver Tudo** no canto superior direito.
1. Selecione **criar**e insira um nome de namespace no campo **nome** . Portal do Azure verifica se o nome está disponível.
1. Escolha uma assinatura do Azure na qual criar o namespace.
1. Para [grupo de recursos](../articles/azure-resource-manager/manage-resource-groups-portal.md), escolha um grupo de recursos existente no qual o namespace será colocado ou crie um novo.  
1. Selecione o país ou região no qual o namespace deve ser hospedado.

    ![Criar namespace][create-namespace]

1. Selecione **Criar**. O portal do Azure cria o namespace e o habilita. Depois de alguns minutos, o sistema provisiona recursos para sua conta.

### <a name="get-management-credentials"></a>Obter credenciais de gerenciamento

1. Selecione **todos os recursos**e, em seguida, escolha o nome do namespace recém-criado.
1. Selecione **Políticas de acesso compartilhado**.  
1. Em **Políticas de acesso compartilhado**, selecione **RootManageSharedAccessKey**.
1. Em **política SAS: RootManageSharedAccessKey**, selecione o botão **copiar** ao lado da **cadeia de conexão primária**. Essa ação copia a cadeia de conexão para a área de transferência para uso posterior. Cole esse valor no Bloco de notas ou em outro local temporário.
1. Repita a etapa anterior para copiar e colar o valor de **Chave primária** para um local temporário para uso posterior.  

    ![connection-string][connection-string]

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace-vs2019.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string-vs2019.png
[Azure portal]: https://portal.azure.com

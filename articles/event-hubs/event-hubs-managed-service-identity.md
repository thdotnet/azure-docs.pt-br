---
title: Identidades gerenciadas para recursos do Azure - Hubs de Eventos do Azure | Microsoft Docs
description: Este artigo fornece informações sobre como usar as entidades gerenciadas para os recursos do Azure com os Hubs de Eventos do Azure
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 05/20/2019
ms.author: shvija
ms.openlocfilehash: dbef1db94d7835bd9326102bd62921c6b3d88d74
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707059"
---
# <a name="managed-identities-for-azure-resources-with-event-hubs"></a>Identidades gerenciadas para recursos do Azure com Hubs de Eventos

[Identidades gerenciadas para recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md) é um recurso do Azure que permite criar uma identidade segura associada à implantação na qual o código do aplicativo é executado. Em seguida, você pode associar essa identidade a funções de controle de acesso que concedem permissões personalizadas para acessar os recursos do Azure específicos que seu aplicativo precisa. 

Com identidades gerenciadas, a plataforma do Azure gerencia essa identidade de tempo de execução. Você não precisa armazenar e proteger as chaves de acesso no código ou na configuração do aplicativo, seja para a identidade ou para os recursos que você precisa acessar. Um aplicativo cliente do Hubs de Eventos em execução dentro de um aplicativo de Serviço de Aplicativo do Azure ou em uma máquina virtual com suporte para entidades gerenciadas para recursos do Azure habilitado não precisa lidar com regras e chaves de SAS nem com nenhum outro token de acesso. O aplicativo cliente precisa apenas do endereço do ponto de extremidade do namespace dos Hubs de Eventos. Quando o aplicativo se conecta, os Hubs de Eventos associam o contexto da identidade gerenciada com o cliente em uma operação que será mostrada em um exemplo mais adiante neste artigo.

Quando um cliente dos Hubs de Eventos está associado a uma identidade gerenciada, ele pode realizar todas as operações autorizadas. A autorização é concedida ao associar uma identidade gerenciada com funções de Hubs de Eventos. 

## <a name="event-hubs-roles-and-permissions"></a>Permissões e funções dos Hubs de Eventos
Você pode adicionar uma identidade gerenciada à função de **proprietário de dados dos hubs de eventos** de um namespace de hubs de eventos. Essa função concede a identidade, controle total (para operações de gerenciamento e de dados) em todas as entidades no namespace.

>[!IMPORTANT]
> Anteriormente, damos suporte à adição de identidade gerenciada à função de **proprietário** ou **colaborador** . No entanto, os privilégios de acesso a dados para a função de **proprietário** e **colaborador** não são mais respeitados. Se você estiver usando a função de **proprietário** ou **colaborador** , mude para usando a função de **proprietário de dados dos hubs de eventos** .

Para usar a nova função interna, siga estas etapas: 

1. Navegue até o [portal do Azure](https://portal.azure.com)
2. Navegue até o namespace de hubs de eventos.
3. Na página **namespace de hubs de eventos** , selecione **controle de acesso (iam)** no menu à esquerda.
4. Na página **controle de acesso (iam)** , selecione **Adicionar** na seção **Adicionar uma atribuição de função** . 

    ![Adicionar um botão de atribuição de função](./media/event-hubs-managed-service-identity/add-role-assignment-button.png)
5. Na página **Adicionar atribuição de função** , execute as seguintes etapas: 
    1. Para **função**, selecione **proprietário de dados dos hubs de eventos do Azure**. 
    2. Selecione a **identidade** a ser adicionada à função.
    3. Clique em **Salvar**. 

        ![Função de proprietário de dados dos hubs de eventos](./media/event-hubs-managed-service-identity/add-role-assignment-dialog.png)
6. Alterne para a página atribuições de **função** e confirme se o usuário foi adicionado à função de proprietário de dados dos **hubs de eventos do Azure** . 

    ![Confirmar que o usuário foi adicionado à função](./media/event-hubs-managed-service-identity/role-assignments.png)
 
## <a name="use-event-hubs-with-managed-identities-for-azure-resources"></a>Usar Hubs de Eventos com identidades gerenciadas para Recursos do Azure

A seção a seguir descreve as etapas a seguir:

1. Criar e implantar um aplicativo de exemplo executado em uma identidade gerenciada.
2. Conceda a essa identidade o acesso a um namespace de Hubs de eventos.
3. Como o aplicativo interage com os hubs de eventos usando essa identidade.

Esta introdução descreve um aplicativo Web hospedado no [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/). As etapas necessárias para um aplicativo hospedado em VM são semelhantes.

### <a name="create-an-app-service-web-application"></a>Criar um aplicativo Web do Serviço de Aplicativo

A primeira etapa é criar um aplicativo ASP.NET do Serviço de Aplicativo. Se você não estiver familiarizado com a forma de fazer isso no Azure, siga [este guia de instruções](../app-service/app-service-web-get-started-dotnet-framework.md). No entanto, em vez de criar um aplicativo MVC, conforme é mostrado no tutorial, crie um aplicativo Web Forms.

### <a name="set-up-the-managed-identity"></a>Configurar a identidade gerenciada

Depois de criar o aplicativo, navegue até o aplicativo Web recém-criado no portal do Azure (também mostrado nas instruções) e, em seguida, navegue até a página **Identidade de Serviço Gerenciada** e habilite o recurso: 

![Página de Identidade de Serviço Gerenciada](./media/event-hubs-managed-service-identity/msi1.png)
 
Depois de habilitar o recurso, uma nova identidade do serviço será criada no Azure Active Directory e configurada no host do Serviço de Aplicativo.

### <a name="create-a-new-event-hubs-namespace"></a>Criar um novo namespace dos Hubs de Eventos

Em seguida, [crie um namespace de hubs de eventos](event-hubs-create.md). 

Navegue até a página **Controle de Acesso (IAM)** do namespace no portal e, em seguida, clique em **Adicionar atribuição de função** para adicionar a identidade gerenciada à função **Proprietário**. Para fazer isso, procure o nome do aplicativo Web no campo **Selecionar** do painel **Adicionar permissões** e, em seguida, clique na entrada. Em seguida, clique em **Salvar**. A identidade gerenciada para o aplicativo Web agora tem acesso ao namespace de Hubs de Eventos e ao hub de eventos criado anteriormente. 

### <a name="run-the-app"></a>Executar o aplicativo

Agora, modifique a página padrão do aplicativo ASP.NET que você criou. Você também pode usar o código do aplicativo Web [deste repositório GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/ManagedIdentityWebApp). 

Depois de iniciar o aplicativo, aponte o navegador para EventHubsMSIDemo.aspx. Você também pode defini-lo como sua página inicial. O código pode ser encontrado no arquivo EventHubsMSIDemo.aspx.cs. O resultado é um aplicativo Web mínimo com alguns campos de entrada e os botões **enviar** e **receber** que se conectam aos Hubs de Eventos para enviar ou receber eventos. 

Observe como o objeto [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) é inicializado. Em vez de usar o provedor de token SAS (Token de Acesso Compartilhado), o código cria um provedor de token para a identidade gerenciada com a chamada `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)`. Dessa forma, não há segredos a serem salvos e usados. O fluxo do contexto da identidade gerenciada para o Hubs de Eventos e o handshake de autorização são manipulados automaticamente pelo provedor de token, que é um modelo mais simples do que o uso do SAS.

Depois de fazer essas alterações, publique e execute o aplicativo. Você pode obter os dados de publicação corretos baixando e, em seguida, importando um perfil de publicação no Visual Studio:

![Importar o perfil de publicação](./media/event-hubs-managed-service-identity/msi3.png)
 
Para enviar ou receber mensagens, insira o nome do namespace e o nome da entidade que você criou e, em seguida, clique em **enviar** ou **receber**. 
 
A identidade gerenciada só funciona dentro do ambiente do Azure e somente na implantação do Serviço de Aplicativo em que você a configurou. Identidades gerenciadas não funcionam com slots de implantação do Serviço de Aplicativo no momento.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre Hubs de Eventos, acesse os seguintes links:

* Introdução com um [Tutorial de Hubs de Eventos](event-hubs-dotnet-standard-getstarted-send.md)
* [Perguntas frequentes sobre os Hubs de Eventos](event-hubs-faq.md)
* [Detalhes dos Preços de Hubs de Evento](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Aplicativos de exemplo que usam Hub de Eventos](https://github.com/Azure/azure-event-hubs/tree/master/samples)

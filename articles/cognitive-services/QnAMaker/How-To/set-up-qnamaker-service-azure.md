---
title: Configurar um serviço de QnA Maker-QnA Maker
titleSuffix: Azure Cognitive Services
description: Antes de criar quaisquer bases de dados de conhecimento do QnA Maker, primeiro você deve configurar um serviço de QnA Maker no Azure. Qualquer pessoa com autorização para criar novos recursos em uma assinatura pode configurar o serviço QnA Maker.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: bec154a7e437621f377c503642b186cf166016cc
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195312"
---
# <a name="manage-qna-maker-resources"></a>Gerenciar QnA Maker recursos

Antes de criar quaisquer bases de dados de conhecimento do QnA Maker, primeiro você deve configurar um serviço de QnA Maker no Azure. Qualquer pessoa com autorização para criar novos recursos em uma assinatura pode configurar o serviço QnA Maker.

## <a name="types-of-keys-in-qna-maker"></a>Tipos de chaves no QnA Maker

O serviço QnA Maker lida com dois tipos de chaves, **chaves de assinatura** e **chaves de ponto de extremidade**.

![gerenciamento de chaves](../media/qnamaker-how-to-key-management/key-management.png)

|Nome|Location|Finalidade|
|--|--|--|
|Chave de Assinatura|[Portal do Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)|essas chaves são usadas para acessar as [APIs do serviço de gerenciamento do QnA Maker](https://go.microsoft.com/fwlink/?linkid=2092179). Essas APIs permitem que você edite as perguntas e respostas em sua base de dados de conhecimento e publique sua base de dados de conhecimento. Essas chaves são criadas quando você cria um novo serviço de QnA Maker.<br><br>Localize essas chaves no recurso de **Serviços cognitivas** , na página **chaves** .|
|Chave do ponto de extremidade|[Portal de QnA Maker](http://www.qnamaker.ai)|Essas chaves são usadas para acessar o ponto de extremidade da base de dados de conhecimento publicado para obter uma resposta para uma pergunta de usuário. Normalmente, você usa esse ponto de extremidade em seu bot de chat ou código de aplicativo cliente que se conecta ao serviço de QnA Maker. Essas chaves são criadas quando você publica sua base de dados de conhecimento QnA Maker.<br><br>Localize essas chaves na página **configurações do serviço** . Localize essa página na parte superior, menu do usuário direito como uma opção na lista suspensa.|

## <a name="create-a-new-qna-maker-service"></a>Criar um novo serviço do QnA Maker

Este procedimento cria os recursos do Azure necessários para gerenciar o conteúdo da base de dados de conhecimento. Ao concluir essas etapas, você encontrará as chaves de _assinatura_ na página **chaves** do recurso no portal do Azure.

1. Entre no portal do Azure e [crie um recurso de QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) .

1. Selecione **criar** depois de ler os termos e condições.

    ![Criar um novo serviço do QnA Maker](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. Em **QnA Maker**, selecione os preços e regiões adequados.

    ![Criar um serviço do QnA Maker – tipo de preço e regiões](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Preencha o **Nome** com um nome exclusivo para identificar este serviço QnA Maker. Esse nome também identifica o ponto de extremidade do QnA Maker ao qual suas bases de conhecimento serão associadas.
    * Escolha a **Assinatura** na qual o recurso QnA Maker será implantado.
    * Selecione o **tipo de preço** para os serviços de gerenciamento de QnA Maker (portal e APIs de gerenciamento). Clique [aqui](https://aka.ms/qnamaker-pricing) para obter detalhes sobre os preços de SKUs.
    * Crie um novo **Grupo de Recursos** (recomendado) ou use um existente no qual implantar esse recurso QnA Maker. QnA Maker cria vários recursos do Azure; ao criar um grupo de recursos para manter esses recursos, você pode facilmente localizar, gerenciar e excluir esses recursos pelo nome do grupo de recursos.
    * Selecione um **local do grupo de recursos**.
    * Escolha o **Tipo de preço de pesquisa** do serviço de Azure Search. Se você vir a opção Camada gratuita esmaecida, significa que você já tem uma Camada gratuita do Azure Search implantada em sua assinatura. Nesse caso, você precisará iniciar com a Camada Básica do Azure Search. Consulte os detalhes de preços do Azure Search [aqui](https://azure.microsoft.com/pricing/details/search/).
    * Escolha o **Local de Pesquisa** onde você deseja implantar os dados do Azure Search. As restrições de armazenamento dos dados informarão o local escolhido para o Azure Search.
    * Dê um nome para o serviço de Aplicativo em **Nome do aplicativo**.
    * Por padrão, o serviço de Aplicativo assume o padrão da camada (S1) padrão. Você pode alterar o plano após a criação. Confira mais detalhes sobre preços do Serviço de Aplicativo [aqui](https://azure.microsoft.com/pricing/details/app-service/).
    * Escolha o **Local do site** onde o Serviço de Aplicativo será implantado.

        > [!NOTE]
        > O Local da pesquisa pode ser diferente do Site local.

    * Escolha se deseja habilitar **Application Insights** ou não. Se o **Application Insights** estiver habilitado, o QnA Maker coletará a telemetria em tráfego, logs de chat e erros.
    * Escolha o **local do App insights** onde o recurso do Application Insights será implantado.
    * Para medidas de economia de custo, você pode [compartilhar](#share-existing-services-with-qna-maker) alguns, mas não todos os recursos do Azure criados para QnA Maker. 

1. Depois que todos os campos forem validados, selecione **criar**. Pode levar alguns minutos para ser concluído.

1. Após a conclusão da implantação, você verá os seguintes recursos criados em sua assinatura.

    ![O recurso criou um serviço do QnA Maker](../media/qnamaker-how-to-setup-service/resources-created.png)

    O recurso com o tipo _cognitiva Services_ tem suas chaves de _assinatura_ .

## <a name="find-subscription-keys-in-azure-portal"></a>Localizar chaves de assinatura no portal do Azure

Você pode exibir e redefinir suas chaves de assinatura, que editam o do portal do Azure em que você criou o recurso de QnA Maker. 

1. Vá para o recurso de QnA Maker no portal do Azure e selecione o recurso com o tipo _Serviços cognitivas_.

    ![Lista de recursos do QnA Maker](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Vá até **Chaves**.

    ![chave de assinatura](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="find-endpoint-keys-in-qna-maker-portal"></a>Localizar chaves de ponto de extremidade no portal QnA Maker

As chaves de ponto de extremidade podem ser gerenciadas a partir do [portal do QnA Maker](https://qnamaker.ai).

1. Faça logon no [Portal do QnA Maker](https://qnamaker.ai), vá até seu perfil e, em seguida, clique em **Configurações de serviço**.

    ![chave de ponto de extremidade](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Exibir ou redefinir suas chaves.

    ![gerenciador de chaves de ponto de extremidade](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Atualize as chaves se você achar que elas estão comprometidas. Isso pode exigir que sejam feitas as alterações correspondentes no seu aplicativo cliente ou código bot.

## <a name="share-existing-services-with-qna-maker"></a>Compartilhar serviços existentes com o QnA Maker

QnA Maker cria vários recursos do Azure. Para reduzir o gerenciamento e beneficiar-se do compartilhamento de custos, use a tabela a seguir para entender o que você pode e não consegue compartilhar:

|Serviço|Compartilhar|Reason|
|--|--|--|
|Serviços Cognitivos|X|Não é possível por design|
|Plano de serviço de aplicativo|✔|Espaço em disco fixo alocado para um plano do serviço de aplicativo. Se outros aplicativos, compartilhando o mesmo plano do serviço de aplicativo, usarem um espaço em disco significativo, o serviço de aplicativo do QnAMaker será executado em problemas.|
|Serviço de aplicativo|X|Não é possível por design|
|Application Insights|✔|Pode ser compartilhada|
|Serviço Search|✔|1. `testkb` é um nome reservado para o serviço QnAMaker, ele não pode ser usado por outros.<br>2. O mapa de sinônimos `synonym-map` pelo nome é reservado para o serviço QnAMaker.<br>3. O número de KBs publicados é limitado pela camada de serviço de pesquisa. Se houver índices livres disponíveis, outros serviços poderão usá-lo.|

### <a name="using-a-single-search-service"></a>Usando um único serviço de pesquisa

Se você criar um serviço QnA e suas dependências (como pesquisa) por meio do portal, um serviço de pesquisa será criado para você e vinculado ao serviço de QnA Maker. Depois que esses recursos forem criados, você poderá atualizar a configuração do serviço de aplicativo para usar um serviço de pesquisa já existente e remover o serviço de pesquisa recém-criado.

Se você criar um serviço QnA por meio de modelos de Azure Resource Manager, poderá criar todos os recursos e controlar a criação do serviço de aplicativo para usar um serviço de pesquisa existente. 

## <a name="upgrade-qna-maker"></a>Atualizar QnA Maker

|Atualizar|Reason|
|--|--|
|[Atualizar](#upgrade-qna-maker-sku) o SKU de gerenciamento de QnA Maker|Você precisa ter mais perguntas e respostas em sua base de dados de conhecimento.|
|[Atualizar](#upgrade-app-service) o SKU do serviço de aplicativo|Sua base de dados de conhecimento precisa atender a mais solicitações de seu aplicativo cliente, como um bot de chat.|
|[Atualizar](#upgrade-azure-search-service) o Serviço de Azure Search|Você planeja ter muitas bases de dados de conhecimento.|


### <a name="upgrade-qna-maker-sku"></a>Atualizar QnA Maker SKU

Quando você precisar ter mais perguntas e respostas em sua base de dados de conhecimento, além de sua camada atual, atualize o seu serviço QnA Maker tipo de preço. 

Para fazer upgrade da SKU de gerenciamento do QnA Maker:

1. Vá para o recurso QnA Maker no portal do Azure e selecione **Camada de preços**.

    ![Recurso do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Escolha o SKU apropriado e pressione **Selecionar**.

    ![Preços do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

### <a name="upgrade-app-service"></a>Fazer upgrade do serviço Aplicativo

 Quando sua base de dados de conhecimento precisar fornecer mais solicitações do aplicativo cliente, atualize o tipo de preço do serviço de aplicativo.

Você pode [escalar verticalmente](https://docs.microsoft.com/azure/app-service/manage-scale-up) ou reduzir verticalmente o serviço Aplicativo.

Vá para o recurso de serviço Aplicativo no portal do Azure e selecione as opções **escalar verticalmente** ou **reduzir verticalmente** conforme necessário.

![Escala do serviço de aplicativo do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-azure-search-service"></a>Fazer upgrade do serviço Azure Search

Ao planejar ter muitas bases de conhecimento, atualize o tipo de preço do serviço do Azure Search. 

Atualmente, não é possível executar um local na SKU de pesquisa de atualização do Azure. No entanto, pode criar um novo recurso de pesquisa do Azure com o SKU desejado, restaurar os dados para o novo recurso e vinculá-lo com a pilha do QnA Maker.

1. Crie um novo recurso de pesquisa do Azure no portal do Azure e escolha a SKU desejada.

    ![Recurso de pesquisa do Azure do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Restaure os índices de seu recurso original da pesquisa do Azure para o novo. Consulte o código de exemplo de backup e restauração [aqui](https://github.com/pchoudhari/QnAMakerBackupRestore).

1. Uma vez que os dados sejam restaurados, vá para o novo recurso de pesquisa do Azure, selecione **Chaves**e anote o **Nome** e a **chave Admin**.

    ![Chaves de pesquisa do QnA Maker Azure](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Para vincular o novo recurso de pesquisa do Azure para a pilha do QnA Maker, vá para o serviço de Aplicativo QnA Maker.

    ![Serviço de aplicativo QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. Selecione **Configurações do aplicativo** e substitua os campos **AzureSearchName** e **AzureSearchAdminKey** da etapa 3.

    ![Configuração de serviço de aplicativo QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. Reinicie o serviço de Aplicativo.

    ![Reinício do serviço de aplicativo QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="get-latest-runtime-updates"></a>Obter atualizações de tempo de execução mais recentes

O tempo de execução do QnAMaker faz parte do serviço Azure App implantado quando você [cria um serviço QnAMaker](./set-up-qnamaker-service-azure.md) no portal do Azure. Atualizações são feitas periodicamente para o tempo de execução. QnA Maker serviço de aplicativo está no modo de atualização automática após a versão da extensão de site de abril de 2019 (versão 5 +). Isso já foi projetado para cuidar do tempo de inatividade ZERO durante as atualizações. 

Você pode verificar a versão atual em https://www.qnamaker.ai/UserSettings. Se sua versão for anterior à versão 5. x, você deverá reiniciar o serviço de aplicativo para aplicar as atualizações mais recentes.

1. Acesse o serviço do QnAMaker (grupo de recursos) no [portal do Azure](https://portal.azure.com)

    ![Grupo de recursos do Azure do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Clique no Serviço de Aplicativo e abra a seção Visão Geral

     ![Serviço de Aplicativo do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Reinicie o serviço de Aplicativo. Isso deve concluir dentro de alguns segundos. Quaisquer aplicativos dependentes ou bots que usam esse serviço QnAMaker não estarão disponíveis para os usuários finais durante esse período de reinicialização.

    ![Reinício do serviço de aplicativo do QnAMaker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="management-service-region"></a>Região do serviço de gerenciamento

O serviço de gerenciamento do QnA Maker, usado somente para o portal de QnA Maker & para processamento de dados inicial, está disponível apenas no oeste dos EUA. Nenhum dado do cliente é armazenado neste serviço oeste dos EUA.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar e publicar uma base de conhecimento](../Quickstarts/create-publish-knowledge-base.md)

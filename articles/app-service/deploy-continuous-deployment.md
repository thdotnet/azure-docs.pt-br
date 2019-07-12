---
title: Implantação contínua – Serviço de Aplicativo do Azure | Microsoft Docs
description: Saiba como habilitar a implantação contínua no Serviço de Aplicativo do Azure
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2019
ms.author: cephalin;dariagrigoriu
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 3a207f5513c24f2c33571df929f7490ec7f2eba5
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836755"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Implantação contínua no Serviço de Aplicativo do Azure

[O serviço de aplicativo do Azure](overview.md) permite a implantação contínua do GitHub, BitBucket, e [repositórios Azure](https://azure.microsoft.com/services/devops/repos/) repositórios puxando as atualizações mais recentes. Este artigo mostra como usar o portal do Azure para implantar continuamente seu aplicativo por meio do serviço de build Kudu ou [Pipelines do Azure](https://azure.microsoft.com/services/devops/pipelines/). 

Para obter mais informações sobre os serviços de controle do código-fonte, consulte [criar um repositório (GitHub)], [criar um repositório (BitBucket)], ou [criar um novo repositório Git (repositórios do Azure)].

Para configurar manualmente a implantação contínua de um repositório de nuvem que o portal não suporta diretamente, como [GitLab](https://gitlab.com/), consulte [configurar implantação contínua usando etapas manuais](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="authorize-azure-app-service"></a>Autorizar o serviço de aplicativo do Azure 

Para usar os repositórios do Azure, certifique-se de que sua organização de DevOps do Azure esteja vinculada à sua assinatura do Azure. Para obter mais informações, consulte [configurar uma conta de serviços de DevOps do Azure para que ele possa implantar em um aplicativo web](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Para o Bitbucket ou GitHub, autorize o serviço de aplicativo do Azure para se conectar ao seu repositório. Você só precisará autorizar com um serviço de controle do código-fonte, uma vez. 

1. Selecione **serviços de aplicativos** na [portal do Azure](https://portal.azure.com) barra de navegação esquerda e, em seguida, selecione o aplicativo web que você deseja implantar. 
   
1. Na página do aplicativo, selecione **Deployment Center** no menu à esquerda.
   
1. Sobre o **Central de implantação** página, selecione **GitHub** ou **Bitbucket**e, em seguida, selecione **autorizar**. 
   
   ![Selecione o serviço de controle do código-fonte e selecione Autorizar.](media/app-service-continuous-deployment/github-choose-source.png)
   
1. Entrar para o serviço, se necessário e siga os prompts de autorização. 

## <a name="enable-continuous-deployment"></a>Habilitar a implantação contínua 

Depois que você autoriza um serviço de controle do código-fonte, configurar seu aplicativo para implantação contínua por meio de interna [servidor de compilação do serviço de aplicativo do Kudu](#option-1-use-the-app-service-build-service), ou por meio das [Pipelines do Azure](#option-2-use-azure-pipelines). 

### <a name="option-1-use-the-app-service-build-service"></a>Opção 1: Usar o serviço de aplicativo do serviço de compilação

Você pode usar o interno de servidor para implantar continuamente de compilação de serviço de aplicativo do Kudu do GitHub, Bitbucket ou repositórios do Azure. 

1. Selecione **serviços de aplicativos** na [portal do Azure](https://portal.azure.com) barra de navegação esquerda e, em seguida, selecione o aplicativo web que você deseja implantar. 
   
1. Na página do aplicativo, selecione **Deployment Center** no menu à esquerda.
   
1. Selecione seu provedor de controle de origem autorizada na **Central de implantação** da página e selecione **continuar**. Para o GitHub ou Bitbucket, você também pode selecionar **alterar conta** para alterar a conta autorizada. 
   
   > [!NOTE]
   > Para usar os repositórios do Azure, certifique-se de que sua organização de serviços de DevOps do Azure esteja vinculada à sua assinatura do Azure. Para obter mais informações, consulte [configurar uma conta de serviços de DevOps do Azure para que ele possa implantar em um aplicativo web](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   
1. Para GitHub ou repositórios do Azure, sobre o **provedor de Build** página, selecione **serviço de aplicativo de serviço de compilação**e, em seguida, selecione **continuar**. Bitbucket sempre usa o serviço de compilação do serviço de aplicativo.
   
   ![Selecionar serviço de compilação do serviço de aplicativo e, em seguida, selecione continuar.](media/app-service-continuous-deployment/choose-kudu.png)
   
1. Sobre o **configurar** página:
   
   - Para o GitHub, lista suspensa e selecione o **organização**, **repositório**, e **Branch** você deseja implantar continuamente.
     
     > [!NOTE]
     > Se você não vir todos os repositórios, talvez você precise autorizar o serviço de aplicativo do Azure no GitHub. Navegue até seu repositório do GitHub e vá para **as configurações** > **aplicativos** > **autorizado a aplicativos de OAuth**. Selecione **serviço de aplicativo do Azure**e, em seguida, selecione **Grant**.
     
   - Para o Bitbucket, selecione o Bitbucket **Team**, **repositório**, e **Branch** você deseja implantar continuamente.
     
   - Para os repositórios do Azure, selecione a **organização de DevOps do Azure**, **Project**, **repositório**, e **Branch** você deseja implantar continuamente.
     
     > [!NOTE]
     > Se sua organização de DevOps do Azure não estiver listada, verifique se que ele está vinculado a sua assinatura do Azure. Para obter mais informações, consulte [configurar uma conta de serviços de DevOps do Azure para que ele possa implantar em um aplicativo web](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)...
     
1. Selecione **Continuar**.
   
   ![Preencha as informações de repositório, em seguida, selecione continuar.](media/app-service-continuous-deployment/configure-kudu.png)
   
1. Depois de configurar o provedor de build, examine as configurações na **resumo** página e, em seguida, selecione **concluir**.
   
   Novas confirmações no repositório selecionado e ramificação agora implantar continuamente em seu aplicativo de serviço de aplicativo. Você pode acompanhar as confirmações e implantações na **Deployment Center** página.
   
   ![Acompanhar confirmações e implantações no Centro de implantação](media/app-service-continuous-deployment/github-finished.png)

### <a name="option-2-use-azure-pipelines"></a>Opção 2: Use o Azure Pipelines 

Se sua conta tem as permissões necessárias, você pode configurar Pipelines do Azure para implantar continuamente de repositórios do GitHub ou repositórios do Azure. Para obter mais informações sobre a implantação por meio de Pipelines do Azure, consulte [implantar um aplicativo web nos serviços de aplicativos do Azure](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps).

Para o serviço de aplicativo do Azure criar Pipelines do Azure a entrega contínua na sua organização de DevOps do Azure: 

- Sua conta do Azure deve ter permissões para gravar no Azure Active Directory e criar um serviço. 
  
- Sua conta do Azure deve ter o **proprietário** função em sua assinatura do Azure.

- Você deve ser um administrador no projeto de DevOps do Azure que você deseja usar.

Para configurar os Pipelines do Azure (visualização):

1. Selecione **serviços de aplicativos** na [portal do Azure](https://portal.azure.com) barra de navegação esquerda e, em seguida, selecione o aplicativo web que você deseja implantar. 
   
1. Na página do aplicativo, selecione **Deployment Center** no menu à esquerda.
   
1. Sobre o **provedor de Build** página, selecione **Pipelines do Azure (visualização)** e, em seguida, selecione **continuar**. 
   
1. Sobre o **configurar** página, o **código** seção:
   
   - Para o GitHub, lista suspensa e selecione o **organização**, **repositório**, e **Branch** você deseja implantar continuamente.
     
     > [!NOTE]
     > Se você não vir todos os repositórios, talvez você precise autorizar o serviço de aplicativo do Azure no GitHub. Navegue até seu repositório do GitHub e vá para **as configurações** > **aplicativos** > **autorizado a aplicativos de OAuth**. Selecione **serviço de aplicativo do Azure**e, em seguida, selecione **Grant**.
     
   - Para os repositórios do Azure, selecione a **organização de DevOps do Azure**, **Project**, **repositório**, e **Branch** você deseja implantar continuamente, ou configure uma nova organização do DevOps do Azure.
     
     > [!NOTE]
     > Se sua organização de DevOps do Azure existente não estiver listada, você precisa vinculá-lo a sua assinatura do Azure. Para obter mais informações, consulte [definem seu pipeline de lançamento de CD](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).
     
1. Selecione **Continuar**.
   
1. Para os repositórios do Azure, nos **construir** seção, especifique a estrutura de linguagem que os Pipelines do Azure deve usar para executar tarefas de compilação e, em seguida, selecione **continuar**.
   
1. Sobre o **teste** página, escolha se deseja habilitar testes de carga e, em seguida, selecione **continuar**.
   
1. Dependendo do seu plano do serviço de aplicativo [tipo de preço](https://azure.microsoft.com/pricing/details/app-service/plans/), você poderá ver uma **implantar no preparo** página. Escolha se deseja [habilite slots de implantação](deploy-staging-slots.md)e, em seguida, selecione **continuar**.
   
   > [!NOTE]
   > Pipelines do Azure não permite a entrega contínua para o slot de produção. Essa restrição impede a implantações acidentais para produção. Configurar entrega contínua para um slot de preparo, verifique se as alterações ali mesmo e, em seguida, alternar os slots quando estiver pronto.
   
1. Depois de configurar o provedor de build, examine as configurações na **resumo** página e, em seguida, selecione **concluir**.
   
   Novas confirmações no repositório selecionado e ramificação agora implantar continuamente em seu aplicativo de serviço de aplicativo. Você pode acompanhar as confirmações e implantações na **Deployment Center** página.
   
   ![Acompanhar confirmações e implantações no Centro de implantação](media/app-service-continuous-deployment/github-finished.png)

## <a name="disable-continuous-deployment"></a>Desabilitar a implantação contínua

Para desabilitar a implantação contínua, selecione **Disconnect** na parte superior do seu aplicativo **Deployment Center** página.

![Desabilitar a implantação contínua](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Recursos adicionais

* [Investigar problemas comuns com implantação contínua](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Usar o Azure PowerShell](/powershell/azureps-cmdlets-docs)
* [Documentação do Git](https://git-scm.com/documentation)
* [Kudu do Projeto](https://github.com/projectkudu/kudu/wiki)

[Criar um repositório (GitHub)]: https://help.github.com/articles/create-a-repo
[Criar um repositório (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Criar um novo repositório Git (repositórios do Azure)]: /azure/devops/repos/git/creatingrepo

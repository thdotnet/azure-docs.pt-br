---
title: Implantar do repositório Git local – Serviço de Aplicativo do Azure
description: Saiba como habilitar a implantação do Git local no Serviço de Aplicativo do Azure.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2019
ms.author: dariagrigoriu;cephalin
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: fa961e43408fed014be2266c14c7972d39435b97
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836865"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Implantação do Git local no serviço de aplicativo do Azure

Este guia de instruções mostra como implantar seu aplicativo para [serviço de aplicativo do Azure](overview.md) de um repositório Git no computador local.

## <a name="prerequisites"></a>Pré-requisitos

Para seguir as etapas neste guia de instruções:

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- [Instalar o Git](https://www.git-scm.com/downloads).
  
- Ter um repositório Git local com o código que você deseja implantar. Para baixar um repositório de exemplo, execute o seguinte comando na janela do terminal local:
  
  ```bash
  git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
  ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-build-server"></a>Implantar com o servidor de compilação do Kudu

A maneira mais fácil de habilitar a implantação do Git local para seu aplicativo com o servidor de compilação do serviço de aplicativo do Kudu é usar o Azure Cloud Shell. 

### <a name="configure-a-deployment-user"></a>Configurar um usuário de implantação

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="get-the-deployment-url"></a>Obter a URL de implantação

Para obter a URL para habilitar a implantação Git local para um aplicativo existente, execute [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) no Cloud Shell. Substitua \<nome do aplicativo > e \<nome do grupo > com os nomes de seu aplicativo e seu grupo de recursos do Azure.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```

Ou, para criar um novo aplicativo habilitado para Git, execute [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) no Cloud Shell com o `--deployment-local-git` parâmetro. Substituir \<nome do aplicativo >, \<nome do grupo >, e \<plano-name > com os nomes para seu novo aplicativo de Git, seu grupo de recursos do Azure e seu plano do serviço de aplicativo do Azure.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

O comando retorna uma URL como: `https://<deployment-username>@<app-name>.scm.azurewebsites.net/<app-name>.git`. Use essa URL para implantar seu aplicativo na próxima etapa.

Em vez de usar essa URL no nível da conta, você também pode habilitar o Git local, usando as credenciais de nível de aplicativo. O serviço de aplicativo do Azure gera automaticamente essas credenciais para cada aplicativo. 

Obter as credenciais do aplicativo, executando o seguinte comando no Cloud Shell. Substitua \<nome do aplicativo > e \<nome do grupo > com o nome do seu aplicativo e o nome do grupo de recursos do Azure.

```azurecli-interactive
az webapp deployment list-publishing-credentials --name <app-name> --resource-group <group-name> --query scmUri --output tsv
```

Use a URL que retorna para implantar seu aplicativo na próxima etapa.

### <a name="deploy-the-web-app"></a>Implantar o aplicativo Web

1. Abra uma janela do terminal local para seu repositório Git local e adicione um remoto do Azure. No comando a seguir, substitua \<url > com a URL específica do usuário de implantação ou uma URL específica do aplicativo que você obteve na etapa anterior.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Envie por push para o Azure remoto com `git push azure master`. 
   
1. No **Git Credential Manager** janela, digite sua [senha de usuário de implantação](#configure-a-deployment-user), não sua senha do Azure entrar.
   
1. Examine a saída. Você pode ver a automação de tempo de execução específica, como o MSBuild para ASP.NET, `npm install` para Node. js, e `pip install` para Python. 
   
1. Navegue até seu aplicativo no portal do Azure para verificar se o conteúdo foi implantado.

## <a name="deploy-with-azure-pipelines-builds"></a>Implantar com compilações de Pipelines do Azure

Se sua conta tem as permissões necessárias, você pode configurar Pipelines do Azure (visualização) para habilitar a implantação do Git local para seu aplicativo. 

- Sua conta do Azure deve ter permissões para gravar no Azure Active Directory e criar um serviço. 
  
- Sua conta do Azure deve ter o **proprietário** função em sua assinatura do Azure.

- Você deve ser um administrador no projeto de DevOps do Azure que você deseja usar.

Para habilitar a implantação do Git local para seu aplicativo com Pipelines do Azure (visualização):

1. Navegue até sua página de aplicativo do serviço de aplicativo do Azure na [portal do Azure](https://portal.azure.com)e selecione **Deployment Center** no menu à esquerda.
   
1. Sobre o **Central de implantação** página, selecione **Git Local**e, em seguida, selecione **continuar**. 
   
   ![Selecione o Local do Git e, em seguida, selecione continuar](media/app-service-deploy-local-git/portal-enable.png)
   
1. Sobre o **provedor de Build** página, selecione **Pipelines do Azure (visualização)** e, em seguida, selecione **continuar**. 
   
   ![Selecione os Pipelines do Azure (visualização) e, em seguida, selecione continuar.](media/app-service-deploy-local-git/pipeline-builds.png)

1. Sobre o **configurar** página, configure uma nova organização do DevOps do Azure, ou especifique uma organização existente e, em seguida, selecione **continuar**.
   
   > [!NOTE]
   > Se sua organização de DevOps do Azure existente não estiver listada, você precisa vinculá-lo a sua assinatura do Azure. Para obter mais informações, consulte [definem seu pipeline de lançamento de CD](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).
   
1. Dependendo do seu plano do serviço de aplicativo [tipo de preço](https://azure.microsoft.com/pricing/details/app-service/plans/), você poderá ver uma **implantar no preparo** página. Escolha se deseja [habilite slots de implantação](deploy-staging-slots.md)e, em seguida, selecione **continuar**.
   
1. Sobre o **resumo** página, examine as configurações e, em seguida, selecione **concluir**.
   
1. Quando o Pipeline do Azure estiver pronto, copie a URL do repositório Git do **Deployment Center** página a ser usada na próxima etapa. 
   
   ![Copie a URL do repositório Git](media/app-service-deploy-local-git/vsts-repo-ready.png)

1. Na janela do terminal local, adicione um Azure remoto para seu repositório Git local. No comando, substitua \<url > com a URL do repositório Git que você obteve na etapa anterior.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Envie por push para o Azure remoto com `git push azure master`. 
   
1. Sobre o **Git Credential Manager** página, entrar com seu nome de usuário do visualstudio.com. Para obter outros métodos de autenticação, consulte [visão geral da autenticação de serviços do Azure DevOps](/vsts/git/auth-overview?view=vsts).
   
1. Depois que a implantação for concluída, exiba o progresso da compilação na `https://<azure_devops_account>.visualstudio.com/<project_name>/_build`e o progresso da implantação em `https://<azure_devops_account>.visualstudio.com/<project_name>/_release`.
   
1. Navegue até seu aplicativo no portal do Azure para verificar se o conteúdo foi implantado.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-deployment"></a>Solucionar problemas de implantação

Você pode ver as seguintes mensagens de erro comuns quando você usa Git para publicar um aplicativo de serviço de aplicativo no Azure:

|Mensagem|Causa|Resolução
---|---|---|
|`Unable to access '[siteURL]': Failed to connect to [scmAddress]`|O aplicativo não estiver em execução.|inicie o aplicativo no portal do Azure. Implantação do Git não está disponível quando o aplicativo web é interrompido.|
|`Couldn't resolve host 'hostname'`|As informações de endereço do 'azure' remoto estão incorretas.|use o comando `git remote -v` para listar todos os remotes (remotos), juntamente com a URL associada. Verifique se a URL do remote do 'azure' está correta. Se necessário, remova e recrie esse remoto usando a URL correta.|
|`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`|Você não especificou um branch durante `git push`, ou você não tiver definido a `push.default` valor em `.gitconfig`.|Execute `git push` novamente, especificando o branch mestre: `git push azure master`.|
|`src refspec [branchname] does not match any.`|Você tentou enviar por push para uma ramificação que não seja mestre em 'azure' remoto.|Execute `git push` novamente, especificando o branch mestre: `git push azure master`.|
|`RPC failed; result=22, HTTP code = 5xx.`|esse erro poderá ocorrer se você tentar efetuar push de um repositório Git de grande porte por HTTPS.|Alterar a configuração do git no computador local para fazer o `postBuffer` maiores. Por exemplo: `git config --global http.postBuffer 524288000`.|
|`Error - Changes committed to remote repository but your web app not updated.`|Você implantou um aplicativo Node. js com um _Package. JSON_ arquivo que especifica os módulos adicionais necessários.|Examine o `npm ERR!` mensagens de erro antes desse erro para obter mais contexto sobre a falha. A seguir estão as causas conhecidas desse erro e o correspondente `npm ERR!` mensagens:<br /><br />**Arquivo malformado Package. JSON**: `npm ERR! Couldn't read dependencies.`<br /><br />**Módulo nativo não tem uma distribuição binária para o Windows**:<br />`npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1` <br />ou <br />' npm ERR! [modulename@version] preinstall: \make || gmake\`|

## <a name="additional-resources"></a>Recursos adicionais

- [Documentação do projeto Kudu](https://github.com/projectkudu/kudu/wiki)
- [Implantação contínua no serviço de aplicativo do Azure](deploy-continuous-deployment.md)
- [Exemplo: Criar um aplicativo web e implantar o código de um repositório Git local (CLI do Azure)](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
- [Exemplo: Criar um aplicativo web e implantar o código de um repositório Git local (PowerShell)](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)

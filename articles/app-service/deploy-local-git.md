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
ms.date: 06/14/2019
ms.author: dariagrigoriu;cephalin
ms.custom: seodec18
ms.openlocfilehash: e66c625c3f30580715762d2dd3f48eeaa6e548dc
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67143974"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Implantação do Git local para o Serviço de Aplicativo do Azure

Este guia de instruções mostra como implantar seu código para o [Serviço de Aplicativo do Azure](overview.md) de um repositório do Git no computador local.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para seguir as etapas neste guia de instruções:

* [Instalar o Git](https://www.git-scm.com/downloads).
* Mantenha um repositório Git local com o código que deseja implantar.

Para usar um repositório de amostra para acompanhar, execute o seguinte comando na janela do terminal local:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-builds"></a>Implantar com compilações do Kudu

A maneira mais fácil de habilitar a implantação do Git local no seu aplicativo com o servidor de build do Kudu é usar o Cloud Shell.

### <a name="configure-a-deployment-user"></a>Configurar um usuário de implantação

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

> [!NOTE]
> Em vez de credenciais no nível da conta, você também pode implantar com credenciais de nível de aplicativo, que são geradas automaticamente para cada aplicativo.
>

### <a name="enable-local-git-with-kudu"></a>Habilitar Git local com Kudu

Para habilitar a implantação do Git local no seu aplicativo com o servidor de build do Kudu, execute [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) no Cloud Shell.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```

Para criar um aplicativo habilitado para Git em vez disso, execute [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) no Cloud Shell com o parâmetro `--deployment-local-git`.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

### <a name="deploy-your-project"></a>Implante o seu projeto

De volta na _janela do terminal local_, adicione um remoto do Azure ao repositório Git local. Substitua  _\<nome de usuário >_ com o usuário de implantação do [configurar um usuário de implantação](#configure-a-deployment-user) e  _\<nome do aplicativo >_ com o nome do aplicativo do [Permitem que o Git para seu aplicativo](#enable-local-git-with-kudu).

```bash
git remote add azure https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git
```

> [!NOTE]
> Para implantar com credenciais de aplicativo em vez disso, obtenha as credenciais específicas para seu aplicativo executando o seguinte comando no Cloud Shell:
>
> ```azurecli-interactive
> az webapp deployment list-publishing-credentials -n <app-name> -g <group-name> --query scmUri --output tsv
> ```
>
> Em seguida, use o comando de saída para executar `git remote add azure <url>` como acima.

Envie por push para o Azure remoto para implantar seu aplicativo com o comando a seguir. Quando for solicitada uma senha, certifique-se de que você insira a senha criada na [configurar um usuário de implantação](#configure-a-deployment-user), não a senha que você usa para entrar no portal do Azure.

```bash
git push azure master
```

Você pode ver a automação específica do tempo de execução na saída, como MSBuild para ASP.NET, `npm install` para Node.js e `pip install` para o Python. 

Navegue até seu aplicativo para verificar se o conteúdo foi implantado.

## <a name="deploy-with-azure-devops-builds"></a>Implantar com compilações de DevOps do Azure

> [!NOTE]
> Para o Serviço de Aplicativo criar os Azure Pipelines necessários em sua organização do Azure DevOps Services, sua conta do Azure deve ter a função de **Proprietário** em sua assinatura do Azure.
>

Para habilitar a implantação do Git local no seu aplicativo com o servidor de build do Kudu, navegue até o aplicativo no [portal do Azure](https://portal.azure.com).

No painel de navegação esquerdo da página do aplicativo, clique em **Centro de Implantação** > **Git Local** > **Continuar**.

![](media/app-service-deploy-local-git/portal-enable.png)

Clique em **Pipelines do Azure (versão prévia)**  > **continuar**.

![](media/app-service-deploy-local-git/pipeline-builds.png)

No **configurar** página, configure uma nova organização do DevOps do Azure ou especificar uma organização existente. Ao terminar, clique em **Continuar**.

> [!NOTE]
> Se você quiser usar uma organização existente do DevOps do Azure que não esteja listada, você precisará [vincular a organização de serviços de DevOps do Azure à sua assinatura do Azure](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Dependendo do [tipo de preço](https://azure.microsoft.com/pricing/details/app-service/plans/) do seu plano do Serviço de Aplicativo, talvez você possa ver uma página **Implantar no preparo**. Escolha se deseja habilitar slots de implantação e clique em **Continuar**.

Na página **Resumo**, verifique as opções e clique em **Concluir**.

Demora alguns minutos para que a organização do Azure DevOps Services esteja pronta. Quando estiver pronta, copie a URL do repositório Git no centro de implantação.

![](media/app-service-deploy-local-git/vsts-repo-ready.png)

De volta na _janela do terminal local_, adicione um remoto do Azure ao repositório Git local. Substitua _\<url>_ pela URL da última etapa.

```bash
git remote add vsts <url>
```

Envie por push para o Azure remoto para implantar seu aplicativo com o comando a seguir. Quando solicitado pelo Gerenciador de Credenciais do Git, entre com seu usuário do visualstudio.com. Para métodos de autenticação adicionais, veja [Visão geral da autenticação do Azure DevOps Services](/vsts/git/auth-overview?view=vsts).

```bash
git push vsts master
```

Depois que a implantação for concluída, você poderá encontrar o progresso do build em `https://<vsts_account>.visualstudio.com/<project_name>/_build` e o progresso da implantação em `https://<vsts_account>.visualstudio.com/<project_name>/_release`.

Navegue até seu aplicativo para verificar se o conteúdo foi implantado.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshooting-kudu-deployment"></a>Solução de problemas de implantação do Kudu

Estes são erros ou problemas comuns ao usar o Git para publicar em um aplicativo do Serviço de Aplicativo no Azure:

---
**Sintoma**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Causa**: esse erro poderá ocorrer se o aplicativo não estiver em funcionamento.

**Resolução**: inicie o aplicativo no portal do Azure. A implantação do Git não está disponível quando o Aplicativo Web é interrompido.

---
**Sintoma**: `Couldn't resolve host 'hostname'`

**Causa**: esse erro poderá ocorrer se as informações de endereço inseridas ao criar o remote (remoto) 'azure' estiverem incorretas.

**Resolução**: use o comando `git remote -v` para listar todos os remotes (remotos), juntamente com a URL associada. Verifique se a URL do remote do 'azure' está correta. Se necessário, remova e recrie esse remoto usando a URL correta.

---
**Sintoma**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Causa**: esse erro poderá ocorrer se você não especificar um branch durante `git push` ou se você não tiver configurado o valor `push.default` em `.gitconfig`.

**Resolução**: Execute `git push` novamente, especificando o branch mestre. Por exemplo:

```bash
git push azure master
```

---
**Sintoma**: `src refspec [branchname] does not match any.`

**Causa**: esse erro poderá ocorrer se você tentar efetuar push para um branch que não seja o mestre no remote (remoto) 'azure'.

**Resolução**: Execute `git push` novamente, especificando o branch mestre. Por exemplo:

```bash
git push azure master
```

---
**Sintoma**: `RPC failed; result=22, HTTP code = 5xx.`

**Causa**: esse erro poderá ocorrer se você tentar efetuar push de um repositório Git de grande porte por HTTPS.

**Resolução**: altere a configuração do Git no computador local para aumentar o postBuffer

```bash
git config --global http.postBuffer 524288000
```

---
**Sintoma**: `Error - Changes committed to remote repository but your web app not updated.`

**Causa**: esse erro poderá ocorrer se você estiver implantando um aplicativo do Node.js com um arquivo _package.json_ que especifica os módulos adicionais necessários.

**Resolução**: mensagens adicionais com 'npm ERR!' devem ser registradas antes desse erro e podem fornecer um contexto extra sobre a falha. A seguir estão as causas conhecidas desse erro e a mensagem 'npm ERR!' correspondente: mensagem:

* **Arquivo malformado package.json**: npm ERR! Não foi possível ler as dependências.
* **Um módulo nativo que não tenha uma distribuição binária para o Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OU
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Recursos adicionais

* [Documentação do projeto Kudu](https://github.com/projectkudu/kudu/wiki)
* [Implantação contínua no Serviço de Aplicativo do Azure](deploy-continuous-deployment.md)
* [Exemplo: criar Aplicativo Web e implantar código de um repositório Git local (CLI do Azure)](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Exemplo: criar Aplicativo Web e implantar código de um repositório Git local (PowerShell)](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)

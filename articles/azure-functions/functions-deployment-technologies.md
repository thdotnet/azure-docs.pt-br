---
title: Tecnologias de implantação no Azure Functions | Microsoft Docs
description: Saiba as vantagens e desvantagens das diferentes maneiras de que implantar o código para o Azure Functions.
services: functions
documentationcenter: .net
author: ColbyTresness
manager: dariac
ms.service: azure-functions
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: cotresne
ms.openlocfilehash: 689e54877a5b0a405fe7b3e3981d98ba6b42edb7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67062940"
---
# <a name="deployment-technologies-in-azure-functions"></a>Tecnologias de implantação no Azure Functions

Há algumas tecnologias diferentes, que você pode usar para implantar o código do projeto do Azure Functions no Azure. Este artigo fornece uma lista completa dessas tecnologias, informa quais tecnologias estão disponíveis para quais tipos de funções, explica o que realmente está acontecendo quando cada método é usado e fornece recomendações para o melhor método para usar em vários cenários. Várias ferramentas que oferecem suporte à implantação no Azure Functions são voltadas para a tecnologia certa com base em seu contexto.

## <a name="deployment-technology-availability"></a>Disponibilidade de tecnologia de implantação

> [!IMPORTANT]
> Azure Functions dá suporte a cruzada local de desenvolvimento de plataforma e hospedagem em dois sistemas operacionais: Windows e Linux. Existem três planos de hospedagem disponíveis no momento, cada um com comportamentos diferentes - [consumo](functions-scale.md#consumption-plan), [Premium](functions-scale.md#premium-plan), e [dedicado (serviço de aplicativo)](functions-scale.md#app-service-plan). Nem todas as tecnologias de implantação estão disponíveis para cada sabor de funções do Azure.

| Tecnologia de implantação | Consumo do Windows | Premium (visualização) do Windows | Windows dedicado  | Consumo do Linux (visualização) | Linux dedicado |
|-----------------------|:-------------------:|:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| URL do pacote externo<sup>1</sup> |✔|✔|✔|✔|✔|
| Implantar ZIP |✔|✔|✔| |✔|
| Contêiner do docker | | | | |✔|
| Implantação da Web |✔|✔|✔| | |
| Controle do código-fonte |✔|✔|✔| |✔|
| Git local<sup>1</sup> |✔|✔|✔| |✔|
| Sincronização de nuvem<sup>1</sup> |✔|✔|✔| |✔|
| FTP<sup>1</sup> |✔|✔|✔| |✔|
| Edição de portal |✔|✔|✔| |✔<sup>2</sup>|

<sup>1</sup> tecnologia de implantação que requer [sincronização do gatilho manual](#trigger-syncing).
<sup>2</sup> portal edição está habilitado para apenas os gatilhos de HTTP e o Timer para funções no Linux usando o plano dedicado.

## <a name="key-concepts"></a>Principais conceitos

Antes de continuar, é importante aprender alguns conceitos-chave que serão essenciais para entender como as implantações funcionam no Azure Functions.

### <a name="trigger-syncing"></a>Sincronização do gatilho

Quando você alterar qualquer um dos seus gatilhos, a infra-estrutura de funções precisa estar atento essas alterações. Esta sincronização ocorre automaticamente para muitas tecnologias de implantação. No entanto, em alguns casos você deverá sincronizar manualmente seus gatilhos. Quando você implanta as atualizações usando uma URL do pacote externo, local Git, sincronização de nuvem ou FTP, certifique-se de ter sincronizar manualmente seus gatilhos. Você pode sincronizar os gatilhos em uma das três maneiras:

* Reinicie o aplicativo de função no portal do Azure
* Envie uma solicitação HTTP POST para `https://www.{functionappname}.azurewebsites.net/admin/host/synctriggers?code=<API_KEY>` usando o [chave mestra](functions-bindings-http-webhook.md#authorization-keys).
* Envie uma solicitação HTTP POST para `https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Web/sites/<FUNCTION_APP_NAME>/syncfunctiontriggers?api-version=2016-08-01`. Substitua os espaços reservados com sua ID de assinatura, nome do grupo de recursos e o nome do seu aplicativo de funções.

## <a name="deployment-technology-details"></a>Detalhes da tecnologia de implantação  

### <a name="external-package-url"></a>URL do pacote externo

__O que ele faz:__ Permite que você faça referência a um arquivo de pacote remoto (. zip) que contém seu aplicativo de funções. O arquivo é baixado da URL fornecida e o aplicativo é executado [execução do pacote](run-functions-from-deployment-package.md) modo.

__Como usá-lo:__ Adicionar `WEBSITE_RUN_FROM_PACKAGE` para as configurações do aplicativo. O valor dessa configuração deve ser uma URL - o local do arquivo de pacote específico que você deseja executar. Você pode adicionar configurações de qualquer um dos [no portal do](functions-how-to-use-azure-function-app-settings.md#settings) ou [usando a CLI do Azure](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set). Se usar o armazenamento de BLOBs do Azure, você deve usar um contêiner privado com um [assinatura de acesso compartilhado (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#attach-a-storage-account-by-using-a-shared-access-signature-sas) para conceder acesso de funções ao pacote. A qualquer momento o aplicativo ser reiniciado ele buscará uma cópia do conteúdo, o que significa que sua referência deve ser válida para o tempo de vida do aplicativo.

__Quando usá-lo:__ Esse é o método de implantação somente tem suportado para funções do Azure em execução no Linux no plano de consumo (visualização). Ao atualizar o arquivo de pacote que faz referência a um aplicativo de funções, você deve [sincronizar manualmente os disparadores](#trigger-syncing) para informar ao Azure que seu aplicativo foi alterado.

### <a name="zip-deploy"></a>Implantar ZIP

__O que ele faz:__ Permite que você envie um arquivo zip que contém seu aplicativo de funções no Azure. Opcionalmente, você também pode especificar para que o aplicativo Iniciar em [execução do pacote](run-functions-from-deployment-package.md) modo.

__Como usá-lo:__ Implantar usando seus favoritos a ferramenta de cliente - [Vscode](functions-create-first-function-vs-code.md#publish-the-project-to-azure), [Visual Studio](functions-develop-vs.md#publish-to-azure), ou o [CLI do Azure](functions-create-first-azure-function-azure-cli.md#deploy-the-function-app-project-to-azure). Para implantar manualmente um arquivo zip para seu aplicativo de funções, siga as instruções em [Implantando de um arquivo zip ou uma url](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url).

Além disso, quando a implantação por meio do zip implantar, os usuários podem especificar para executar seu aplicativo no [execução do pacote](run-functions-from-deployment-package.md) modo definindo o `WEBSITE_RUN_FROM_PACKAGE` valor de configuração de aplicativo como `1`. Essa opção é sugerida e resulta em tempos de carregamento mais rápidos para seus aplicativos. Isso é feito por padrão para as ferramentas de cliente acima.

__Quando usá-lo:__ Essa é a tecnologia de implantação recomendada para o Azure Functions em execução no Windows e o Azure Functions em execução no Linux no plano dedicado.

### <a name="docker-container"></a>Contêiner do docker

__O que ele faz:__ Especifica uma imagem de contêiner que efetuar pull e executar funções do Azure.

__Como usá-lo:__ Criar um aplicativo de funções do Linux no plano dedicado e especificar qual imagem de contêiner para executar do. É possível fazer isso de duas formas:

* Crie um aplicativo de funções do Linux em um plano do serviço de aplicativo no portal do Azure. Selecione **imagem do Docker** para **publicar**e defina o contêiner, fornecendo o local onde a imagem está hospedada.
* Crie um aplicativo de funções do Linux em um plano do serviço de aplicativo por meio da CLI do Azure. Saiba como revisando [criar uma função no Linux usando uma imagem personalizada](functions-create-function-linux-custom-image.md#create-and-deploy-the-custom-image).

Para implantar um aplicativo existente usando um contêiner personalizado, use o [ `func deploy` ](functions-run-local.md#publish) comando da [as ferramentas básicas do Azure Functions](functions-run-local.md).

__Quando usá-lo:__ Use esta opção quando você precisa de mais controle sobre o ambiente do Linux em que seu aplicativo de funções é executado. Esse mecanismo de implantação só está disponível para as funções em execução no Linux em um plano do serviço de aplicativo.

### <a name="web-deploy-msdeploy"></a>(MSDeploy) de implantação da Web

__O que ele faz:__ Empacota e implanta os aplicativos do Windows para qualquer servidor do IIS - incluindo seus aplicativos de funções do Azure em execução no Windows.

__Como usá-lo:__ Use o [ferramentas do Visual Studio para o Azure Functions](functions-create-your-first-function-visual-studio.md), e não escala o `Run from package file (recommended)` caixa de seleção.

Como alternativa, chame `MSDeploy.exe` diretamente após o download [Web implantar 3.6](https://www.iis.net/downloads/microsoft/web-deploy).

__Quando usá-lo:__ Essa tecnologia de implantação tem suporte e não tem nenhum problema, mas o mecanismo preferido é agora [Zip implantar com a execução do pacote habilitado](#zip-deploy). Para saber mais, visite o [guia de desenvolvimento do Visual Studio](functions-develop-vs.md#publish-to-azure).

### <a name="source-control"></a>Controle do código-fonte

__O que ele faz:__ Permite que você para seu aplicativo de funções de gancho até um repositório git, de modo que todas as atualizações para o código naquele repositório dispara a implantação. Para obter mais informações, examine os [Kudu Wiki](https://github.com/projectkudu/kudu/wiki/VSTS-vs-Kudu-deployments).

__Como usá-lo:__ Use o Centro de implantação no portal do Azure Functions para configurar a publicação do controle de origem. Para saber mais, confira [Implantação contínua do Azure Functions](functions-continuous-deployment.md).

__Quando usá-lo:__ Usando o controle do código-fonte é a prática recomendada para equipes colaboram em seus aplicativos de funções, e isso é uma ótima opção que permite que os pipelines de implantação mais sofisticados.

### <a name="local-git"></a>Git local

__O que ele faz:__ Permite que você envie código em seu computador local para Azure Functions usando o Git.

__Como usá-lo:__ Siga as instruções em [implantação do Git Local no serviço de aplicativo do Azure](../app-service/deploy-local-git.md).

__Quando usá-lo:__ Em geral, os outros métodos de implantação são recomendados. Durante a publicação do git local, você deve [sincronizar manualmente os disparadores](#trigger-syncing).

### <a name="cloud-sync"></a>Sincronização de nuvem

__O que ele faz:__ Permite que você sincronize o conteúdo do Dropbox e OneDrive para o Azure Functions.

__Como usá-lo:__ Siga as instruções em [sincronizar o conteúdo de uma pasta de nuvem](../app-service/deploy-content-sync.md).

__Quando usá-lo:__ Em geral, os outros métodos de implantação são recomendados. Ao publicar com a sincronização de nuvem, você deve [sincronizar manualmente os disparadores](#trigger-syncing).

### <a name="ftp"></a>FTP

__O que ele faz:__ Permite que você diretamente os arquivos de transferência para o Azure Functions.

__Como usá-lo:__ Siga as instruções em [implantar o conteúdo usando FTP/s](../app-service/deploy-ftp.md).

__Quando usá-lo:__ Em geral, os outros métodos de implantação são recomendados. Ao publicar com o FTP, você deve [sincronizar manualmente os disparadores](#trigger-syncing).

### <a name="portal-editing"></a>Edição de portal

__O que ele faz:__ Usando o editor baseado no portal permite editar diretamente arquivos no seu aplicativo de funções (essencialmente implantando a qualquer momento que você clique em **salvar**).

__Como usá-lo:__ Para poder editar suas funções no portal do Azure, você deve ter [criado suas funções no portal do](functions-create-first-azure-function.md). Usando qualquer outro método de implantação torna sua função somente leitura e impede a edição portal contínuo, a preservar a uma única fonte de verdade. Para retornar a um estado em que você pode editar os arquivos usando o portal do Azure, você pode manualmente ativar o modo de edição para `Read/Write` e remover as configurações relacionadas à implantação de aplicativo (como `WEBSITE_RUN_FROM_PACKAGE`).

#### <a name="portal-editing-availability"></a>Editando a disponibilidade do Portal

| | Consumo do Windows | Premium (visualização) do Windows | Windows dedicado | Consumo do Linux (visualização) | Linux dedicado |
|-|:-----------------: |:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| JavaScript (Node.js) |✔|✔|✔| |✔<sup>1</sup>|
| TypeScript (Node.js) | | | | | |
| C# | | | | | |
| Script do C# |✔|✔|✔| |✔<sup>1</sup>|
| F# | | | | | |
| Java | | | | | |
| Python (versão prévia) | | | | | |
| PowerShell (visualização) |✔|✔|✔| | |

<sup>1</sup> portal edição está habilitado para apenas os gatilhos de HTTP e o Timer para funções no Linux usando o plano dedicado.

__Quando usá-lo:__ O portal é uma ótima maneira de começar a usar com o Azure Functions, mas para qualquer trabalho de desenvolvimento mais intenso usando o cliente de ferramentas é recomendável:

* [Introdução ao uso do VS Code](functions-create-first-function-vs-code.md)
* [Começar a usar as ferramentas básicas do Azure Functions](functions-run-local.md)
* [Introdução ao uso do Visual Studio](functions-create-your-first-function-visual-studio.md)

## <a name="other-relevant-information"></a>Outras informações relevantes

### <a name="deployment-slots"></a>Slots de implantação

Quando você implanta seu aplicativo de funções no Azure, você pode implantar em um slot de implantação separado em vez de diretamente para a produção. Para obter mais informações sobre os slots de implantação, consulte [a documentação de Slots de serviço de aplicativo do Azure](../app-service/deploy-staging-slots.md).

#### <a name="deployment-slots-levels-of-support"></a>Níveis de slots de implantação de suporte

Há dois níveis de suporte:

* _Geralmente disponível (GA)_ – com suporte total e aprovado para uso em produção.
* _Versão prévia_ – ainda não tem suporte, mas é esperado para alcançar o status de GA no futuro.

| Plano do serviço de sistema operacional/de hospedagem | Nível de suporte |
| --------------- | ------ |
| Consumo do Windows | Visualização |
| Premium (visualização) do Windows | Visualização |
| Windows dedicado | Disponível para o público geral |
| Consumo do Linux | Sem suporte |
| Linux dedicado | Disponível para o público geral |

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Implantação contínua para o Azure Functions](functions-continuous-deployment.md)
> [entrega contínua com o Azure DevOps](functions-how-to-azure-devops.md)
> [Zip implantações do Azure Functions](deployment-zip-push.md) 
>  [Executar o Azure Functions de um arquivo de pacote](run-functions-from-deployment-package.md)
> [automatizar a implantação de recursos para seu aplicativo de funções no Azure Functions](functions-infrastructure-as-code.md)

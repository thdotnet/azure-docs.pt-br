---
title: Tecnologias de implantação no Azure Functions | Microsoft Docs
description: Aprenda as diferentes maneiras que você pode implantar o código para Azure Functions.
services: functions
documentationcenter: .net
author: ColbyTresness
manager: dariac
ms.service: azure-functions
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: cotresne
ms.openlocfilehash: 47d8bf33fd686942326db3b1cc606978bf47a1bb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594388"
---
# <a name="deployment-technologies-in-azure-functions"></a>Tecnologias de implantação no Azure Functions

Você pode usar algumas tecnologias diferentes para implantar o código do projeto do Azure Functions no Azure. Este artigo fornece uma lista completa dessas tecnologias, descreve quais tecnologias estão disponíveis para quais tipos de funções, explica o que acontece quando usar cada método e fornece recomendações para o melhor método para usar em vários cenários . Várias ferramentas que oferecem suporte à implantação no Azure Functions são voltadas para a tecnologia certa com base em seu contexto.

## <a name="deployment-technology-availability"></a>Disponibilidade de tecnologia de implantação

> [!IMPORTANT]
> O Azure Functions dá suporte ao desenvolvimento de plataforma cruzada local e hospedagem no Windows e Linux. No momento, três planos de hospedagem estão disponíveis: [Consumo](functions-scale.md#consumption-plan), [Premium](functions-scale.md#premium-plan), e [dedicado (serviço de aplicativo do Azure)](functions-scale.md#app-service-plan). Cada plano tem comportamentos diferentes. Nem todas as tecnologias de implantação estão disponíveis para cada sabor de funções do Azure.

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
<sup>2</sup> portal edição é habilitada somente para gatilhos HTTP e o Timer para funções no Linux usando o plano dedicado.

## <a name="key-concepts"></a>Principais conceitos

Alguns dos principais conceitos são essenciais para entender como as implantações funcionam no Azure Functions.

### <a name="trigger-syncing"></a>Sincronização do gatilho

Quando você alterar qualquer um dos seus gatilhos, a infraestrutura de funções deve estar ciente das alterações. A sincronização acontece automaticamente para muitas tecnologias de implantação. No entanto, em alguns casos, você deverá sincronizar manualmente seus gatilhos. Quando você implanta as atualizações, fazendo referência a uma URL do pacote externo, local Git, sincronização de nuvem ou FTP, você deverá sincronizar manualmente seus gatilhos. Você pode sincronizar os gatilhos em uma das três maneiras:

* Reinicie o aplicativo de função no portal do Azure
* Envie uma solicitação HTTP POST para `https://{functionappname}.azurewebsites.net/admin/host/synctriggers?code=<API_KEY>` usando o [chave mestra](functions-bindings-http-webhook.md#authorization-keys).
* Envie uma solicitação HTTP POST para `https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Web/sites/<FUNCTION_APP_NAME>/syncfunctiontriggers?api-version=2016-08-01`. Substitua os espaços reservados com sua ID de assinatura, nome do grupo de recursos e o nome do seu aplicativo de funções.

## <a name="deployment-technology-details"></a>Detalhes da tecnologia de implantação 

Os seguintes métodos de implantação estão disponíveis no Azure Functions.

### <a name="external-package-url"></a>URL do pacote externo

Você pode usar uma URL do pacote externo para fazer referência a um arquivo de pacote remoto (. zip) que contém seu aplicativo de funções. O arquivo é baixado da URL fornecida e o aplicativo é executado [execução do pacote](run-functions-from-deployment-package.md) modo.

>__Como usá-lo:__ Adicionar `WEBSITE_RUN_FROM_PACKAGE` para as configurações do aplicativo. O valor dessa configuração deve ser uma URL (o local do arquivo de pacote específico que você deseja executar). Você pode adicionar configurações de qualquer um dos [no portal do](functions-how-to-use-azure-function-app-settings.md#settings) ou [usando a CLI do Azure](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set). 
>
>Se você usar o armazenamento de BLOBs do Azure, use um contêiner privado com um [assinatura de acesso compartilhado (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) para conceder acesso de funções ao pacote. Sempre que o aplicativo ser reiniciado, ele busca uma cópia do conteúdo. Sua referência deve ser válida para o tempo de vida do aplicativo.

>__Quando usá-lo:__ URL do pacote externo é o método de implantação com suporte somente para o Azure Functions em execução no Linux no plano de consumo (visualização). Quando você atualiza o arquivo de pacote que faz referência a um aplicativo de funções, você deve [sincronizar manualmente os disparadores](#trigger-syncing) para informar ao Azure que seu aplicativo foi alterado.

### <a name="zip-deploy"></a>Implantar ZIP

Implantar zip de uso para enviar por push um arquivo. zip que contém seu aplicativo de funções no Azure. Opcionalmente, você pode definir seu aplicativo para iniciar em [execução do pacote](run-functions-from-deployment-package.md) modo.

>__Como usá-lo:__ Implante usando sua ferramenta favorita de cliente: [Código do VS](functions-create-first-function-vs-code.md#publish-the-project-to-azure), [Visual Studio](functions-develop-vs.md#publish-to-azure), ou o [da CLI do Azure](functions-create-first-azure-function-azure-cli.md#deploy-the-function-app-project-to-azure). Para implantar manualmente um arquivo. zip para seu aplicativo de funções, siga as instruções em [implantar a partir de um arquivo. zip ou uma URL](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url).
>
>Quando você implanta usando zip implantar, você pode definir seu aplicativo seja executado [execução do pacote](run-functions-from-deployment-package.md) modo. Para definir o modo de execução do pacote, defina as `WEBSITE_RUN_FROM_PACKAGE` valor de configuração do aplicativo para `1`. Recomendamos a implantação de zip. Esse processo resulta em tempos de carregamento mais rápidos para seus aplicativos e é o padrão para o VS Code, Visual Studio e a CLI do Azure.

>__Quando usá-lo:__ Implantar ZIP é a tecnologia de implantação recomendada para o Azure Functions em execução no Windows e para o Azure Functions em execução no Linux no plano dedicado.

### <a name="docker-container"></a>Contêiner do docker

Você pode implantar uma imagem de contêiner do Linux que contém seu aplicativo de funções.

>__Como usá-lo:__ Criar um aplicativo de funções do Linux no plano dedicado e especificar qual imagem de contêiner para executar do. É possível fazer isso de duas formas:
>
>* Crie um aplicativo de funções do Linux em um plano do serviço de aplicativo do Azure no portal do Azure. Para **Publish**, selecione **imagem do Docker**e, em seguida, defina o contêiner. Insira o local onde a imagem está hospedada.
>* Crie um aplicativo de funções do Linux em um plano do serviço de aplicativo usando a CLI do Azure. Para saber como, consulte [criar uma função no Linux usando uma imagem personalizada](functions-create-function-linux-custom-image.md#create-and-deploy-the-custom-image).
>
>Para implantar um aplicativo existente usando um contêiner personalizado, no [as ferramentas básicas do Azure Functions](functions-run-local.md), use o [ `func deploy` ](functions-run-local.md#publish) comando.

>__Quando usá-lo:__ Use a opção de contêiner do Docker quando precisar de mais controle sobre o ambiente do Linux em que seu aplicativo de funções é executado. Esse mecanismo de implantação está disponível somente para funções em execução no Linux em um plano do serviço de aplicativo.

### <a name="web-deploy-msdeploy"></a>(MSDeploy) de implantação da Web

A implantação da Web empacota e implanta os aplicativos do Windows para qualquer servidor do IIS, incluindo seus aplicativos de funções em execução no Windows no Azure.

>__Como usá-lo:__ Use [ferramentas do Visual Studio para o Azure Functions](functions-create-your-first-function-visual-studio.md). Desmarque a **execução do arquivo de pacote (recomendado)** caixa de seleção.
>
>Você também pode baixar [Web implantar 3.6](https://www.iis.net/downloads/microsoft/web-deploy) e chame `MSDeploy.exe` diretamente.

>__Quando usá-lo:__ Suporte para a implantação da Web e não tem nenhum problema, mas é o mecanismo preferido [zip implantar com a execução do pacote habilitado](#zip-deploy). Para obter mais informações, consulte o [guia de desenvolvimento do Visual Studio](functions-develop-vs.md#publish-to-azure).

### <a name="source-control"></a>Controle do código-fonte

Use controle de origem para conectar seu aplicativo de funções a um repositório Git. Uma atualização para o código naquele repositório dispara a implantação. Para obter mais informações, consulte o [Kudu Wiki](https://github.com/projectkudu/kudu/wiki/VSTS-vs-Kudu-deployments).

>__Como usá-lo:__ Use a Central de implantação no portal do Azure Functions para configurar a publicação do controle de origem. Para saber mais, confira [Implantação contínua do Azure Functions](functions-continuous-deployment.md).

>__Quando usá-lo:__ Usando o controle do código-fonte é a prática recomendada para equipes que colaboram em seus aplicativos de funções. Controle de origem é uma opção de implantação boa que permite que os pipelines de implantação mais sofisticados.

### <a name="local-git"></a>Git local

Você pode usar Git local para o código de envio por push em seu computador local ao Azure Functions usando o Git.

>__Como usá-lo:__ Siga as instruções em [implantação do Git Local no serviço de aplicativo do Azure](../app-service/deploy-local-git.md).

>__Quando usá-lo:__ Em geral, é recomendável que você use um método de implantação diferentes. Quando você publica do Git local, você deve [sincronizar manualmente os disparadores](#trigger-syncing).

### <a name="cloud-sync"></a>Sincronização de nuvem

Uso de nuvem uma sincronização para outra seu conteúdo do Dropbox e OneDrive para o Azure Functions.

>__Como usá-lo:__ Siga as instruções em [sincronizar o conteúdo de uma pasta de nuvem](../app-service/deploy-content-sync.md).

>__Quando usá-lo:__ Em geral, é recomendável que outros métodos de implantação. Quando você publicar usando a sincronização de nuvem, você deve [sincronizar manualmente os disparadores](#trigger-syncing).

### <a name="ftp"></a>FTP

Você pode usar o FTP para transferir diretamente arquivos no Azure Functions.

>__Como usá-lo:__ Siga as instruções em [implantar o conteúdo usando FTP/s](../app-service/deploy-ftp.md).

>__Quando usá-lo:__ Em geral, é recomendável que outros métodos de implantação. Quando você publica por meio de FTP, você deve [sincronizar manualmente os disparadores](#trigger-syncing).

### <a name="portal-editing"></a>Edição de portal

No editor baseado no portal, você pode editar diretamente os arquivos que estão em seu aplicativo de funções (essencialmente Implantando toda vez que você salvar as alterações).

>__Como usá-lo:__ Para poder editar suas funções no portal do Azure, você deve ter [criado suas funções no portal do](functions-create-first-azure-function.md). Para preservar uma única fonte verdadeira, usar qualquer outro método de implantação torna sua função somente leitura e impede a edição de portal contínuo. Para retornar a um estado em que você pode editar os arquivos no portal do Azure, você pode manualmente ativar o modo de edição para `Read/Write` e remover as configurações relacionadas à implantação de aplicativo (como `WEBSITE_RUN_FROM_PACKAGE`). 

>__Quando usá-lo:__ O portal é uma boa maneira de começar a usar com o Azure Functions. Para o trabalho de desenvolvimento mais intenso, é recomendável que você use o ferramentas de cliente:
>
>* [Introdução ao uso do VS Code](functions-create-first-function-vs-code.md)
>* [Começar a usar as ferramentas básicas do Azure Functions](functions-run-local.md)
>* [Introdução ao uso do Visual Studio](functions-create-your-first-function-visual-studio.md)

A tabela a seguir mostra os sistemas operacionais e linguagens que edição portal de suporte:

| | Consumo do Windows | Premium (visualização) do Windows | Windows dedicado | Consumo do Linux (visualização) | Linux dedicado |
|-|:-----------------: |:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| C# | | | | | |
| Script do C# |✔|✔|✔| |✔<sup>*</sup>|
| F# | | | | | |
| Java | | | | | |
| JavaScript (Node.js) |✔|✔|✔| |✔<sup>*</sup>|
| Python (versão prévia) | | | | | |
| PowerShell (visualização) |✔|✔|✔| | |
| TypeScript (Node.js) | | | | | |

<sup>*</sup> Portal de edição é habilitada somente para gatilhos HTTP e o Timer para funções no Linux usando o plano dedicado.

## <a name="deployment-slots"></a>Slots de implantação

Quando você implanta seu aplicativo de funções no Azure, você pode implantar em um slot de implantação separado em vez de implantar diretamente para a produção. Para obter mais informações sobre os slots de implantação, consulte [slots de serviço de aplicativo do Azure](../app-service/deploy-staging-slots.md).

### <a name="deployment-slots-levels-of-support"></a>Níveis de slots de implantação de suporte

Há dois níveis de suporte para slots de implantação:

* **Disponibilidade geral (GA)** : Totalmente compatível e aprovados para uso em produção.
* **Versão Prévia**: Ainda não tem suporte, mas é esperado para alcançar o status de GA no futuro.

| Plano do serviço de sistema operacional/de hospedagem | Nível de suporte |
| --------------- | ------ |
| Consumo do Windows | Visualizar |
| Premium (visualização) do Windows | Visualizar |
| Windows dedicado | Disponibilidade Geral |
| Consumo do Linux | Sem suporte |
| Linux dedicado | Disponibilidade Geral |

## <a name="next-steps"></a>Próximas etapas

Leia estes artigos para saber mais sobre como implantar seus aplicativos de funções: 

+ [Implantação contínua para Azure Functions](functions-continuous-deployment.md)
+ [Entrega contínua com o uso de DevOps do Azure](functions-how-to-azure-devops.md)
+ [Implantações do ZIP para o Azure Functions](deployment-zip-push.md)
+ [Executar o Azure Functions de um arquivo de pacote](run-functions-from-deployment-package.md)
+ [Automatizar a implantação de recursos para seu aplicativo de funções no Azure Functions](functions-infrastructure-as-code.md)

---
title: Como o Visual Studio Code funciona com espaços de desenvolvimento do Azure
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 07/08/2019
ms.topic: conceptual
description: Como o Visual Studio Code funciona com espaços de desenvolvimento do Azure
keywords: Espaços de desenvolvimento do Azure, os espaços de desenvolvimento, Docker, Kubernetes, Azure, AKS, serviço Kubernetes do Azure, contêineres
ms.openlocfilehash: a7ec20908b75ae07532c16daab8950ace9cd67ae
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712144"
---
# <a name="how-visual-studio-code-works-with-azure-dev-spaces"></a>Como o Visual Studio Code funciona com espaços de desenvolvimento do Azure

Você pode usar o Visual Studio Code e o [extensão de espaços de desenvolvimento do Azure][azds-extension] para preparar, executar e depurar seus serviços com espaços de desenvolvimento do Azure. Com o Visual Studio Code e a extensão de espaços de desenvolvimento do Azure, você pode:

* Gerar ativos para executar e depurar serviços no AKS
* Executar os serviços Java, Node. js e .NET Core em um espaço de desenvolvimento
* Depurar diretamente de seus serviços de Java, Node. js e .NET Core em execução em um espaço de desenvolvimento

## <a name="generate-assets"></a>Gerar ativos

Visual Studio Code e a extensão de espaços de desenvolvimento do Azure geram os seguintes ativos para seu projeto:

* Dockerfiles para aplicativos .NET Core, aplicativos Node. js e aplicativos Java usando Maven
* Gráficos do Helm para quase todos os idiomas com um Dockerfile
* Uma `azds.yaml` arquivo, que é o [arquivo de configuração de espaços de desenvolvimento do Azure][azds-yaml] para seu projeto
* Um `.vscode` pasta com a configuração de lançamento do Visual Studio Code do seu projeto para aplicativos .NET Core, aplicativos Node. js e aplicativos Java usando Maven

O Dockerfile, o gráfico do Helm e `azds.yaml` arquivos são os mesmos ativos gerados durante a execução `azds prep`. Esses arquivos também podem ser usados fora do código do Visual Studio para executar o projeto no AKS, como executar `azds up`. O `.vscode` pasta é usada somente pelo código do Visual Studio para executar o projeto no AKS do Visual Studio Code.

## <a name="run-your-service-in-aks"></a>Executar seu serviço no AKS

Depois de gerar os ativos para seu projeto, você pode executar os serviços de Java, Node. js e .NET Core em um espaço de desenvolvimento existentes do Visual Studio Code. No *depurar* página de código do Visual Studio, você pode invocar a configuração de inicialização do `.vscode` diretório para executar o projeto.

Você deve criar o cluster do AKS e habilitar espaços de desenvolvimento do Azure em seu cluster fora do Visual Studio Code. Por exemplo, você pode usar a CLI do Azure ou o portal do Azure para fazer essa configuração. Você pode reutilizar existente Dockerfiles, gráficos do Helm e `azds.yaml` arquivos criados fora do Visual Studio Code, como os ativos gerados pela execução `azds prep`. Se você reutilizar ativos gerados fora do Visual Studio Code, você ainda precisa ter um `.vscode` directory. Isso `.vscode` directory pode ser regenerado pelo Visual Studio code e a extensão de espaços de desenvolvimento do Azure e não substituirá os ativos existentes.

Para projetos do .NET Core, você deve ter o [ C# extensão][csharp-extension] installed to run your .NET service from Visual Studio Code. Also for Java projects using Maven, you must have the [Java Debugger for Azure Dev Spaces extension][java-extension] instalados, bem como [Maven instalado e configurado][maven] para executar seu código Java serviço do Visual Studio Code.

## <a name="debug-your-service-in-aks"></a>Depurar seu serviço no AKS

Depois de iniciar o seu projeto, você pode depurar seus serviços Java, Node. js e .NET Core em execução em um espaço de desenvolvimento diretamente do Visual Studio Code. A configuração de inicialização no `.vscode` directory fornece as informações de depuração adicionais para a execução de um serviço com a depuração habilitada em um espaço de desenvolvimento. Visual Studio Code também anexa ao processo de depuração no contêiner em execução em seus espaços de desenvolvimento, permitindo que você definir pontos de interrupção, inspecionar variáveis e executar outras operações de depuração.


## <a name="use-visual-studio-code-with-azure-dev-spaces"></a>Usar o Visual Studio Code com espaços de desenvolvimento do Azure

Você pode ver o Visual Studio Code e a extensão de espaços de desenvolvimento do Azure que trabalham com espaços de desenvolvimento do Azure em inícios rápidos a seguir:

* [Desenvolver com Java][quickstart-java]
* [Desenvolver com o .NET][quickstart-netcore]
* [Desenvolver com Node. js][quickstart-node]

[azds-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azds-yaml]: how-dev-spaces-works.md#prepare-your-code
[csharp-extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp
[java-extension]: https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds
[maven]: https://maven.apache.org
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
---
title: Saiba mais sobre como criar e publicar o.Net Core aplicativos em um cluster Linux do Azure Service Fabric remoto | Microsoft Docs
description: Criar e publicar o.Net Core aplicativos direcionados para um cluster do Linux remoto do Visual Studio
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/20/2019
ms.author: pepogors
ms.openlocfilehash: 46d76edbe8cede12e8c7811f43c28a65c1ebaed0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078658"
---
# <a name="use-visual-studio-to-create-and-publish-net-core-applications-targeting-a-remote-linux-service-fabric-cluster"></a>Usar o Visual Studio para criar e publicar o.Net Core aplicativos direcionados para um cluster Linux do Service Fabric remoto
Com o Visual Studio das ferramentas você podem desenvolver e publicar o.Net Core do Service Fabric aplicativos direcionados para um cluster do Linux do Service Fabric. A versão do SDK deve ser 3.4 ou superior para implantar um.Net Core clusters do aplicativo de direcionamento do Linux do Service Fabric do Visual Studio.

> [!Note]
> Visual Studio não dá suporte a depuração de aplicativos do Service Fabric que Linux de destino.
>

## <a name="create-a-service-fabric-application-targeting-net-core"></a>Criar um aplicativo do Service Fabric direcionando o.Net Core
1. Inicie o Visual Studio como um **administrador**.
2. Crie um projeto com **arquivo -> Novo -> projeto**.
3. No **novo projeto** caixa de diálogo, escolha **Cloud -> aplicativo do Service Fabric**.
![create-application]
4. Nome do aplicativo e clique em **Okey**.
5. Sobre o **novo serviço Service Fabric** , selecione o tipo de serviço que você deseja criar na **.Net Core seção**.
![create-service]

## <a name="deploy-to-a-remote-linux-cluster"></a>Implantar em um cluster remoto do Linux
1. No Gerenciador de soluções, clique com botão direito no aplicativo e selecione **Build**.
![build-application]
2. Depois que o processo de compilação para o aplicativo for concluída, clique com o botão direito no serviço e selecione Editar os **arquivo csproj**.
![edit-csproj]
3. Edite a propriedade UpdateServiceFabricManifestEnabled de True para **falsos** se o serviço é um **tipo de projeto de ator**. Se seu aplicativo não tiver um serviço de ator, vá para a etapa 4.
```xml
    <UpdateServiceFabricManifestEnabled>False</UpdateServiceFabricManifestEnabled>
```
> [!Note]
> Definir UpdateServiceFabricManifestEnabled como falso, desabilitará atualizações em servicemanifest. XML durante a compilação. Qualquer alteração tais como adicionar, remover ou renomear para o serviço não será refletida no servicemanifest. XML. Se todas as alterações forem feitas, você deve atualizar que servicemanifest manualmente ou temporariamente definir UpdateServiceFabricManifestEnabled como true e crie o serviço que atualizará o servicemanifest. XML e, em seguida, revertê-lo fazer o como false.
>

4. Atualize o RuntimeIndetifier do win7-x64 para a plataforma de destino no projeto de serviço.
```xml
    <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```
5. No ServiceManifest, atualize o programa do ponto de entrada para remover .exe. 
```xml
    <EntryPoint> 
    <ExeHost> 
        <Program>Actor1</Program> 
    </ExeHost> 
    </EntryPoint>
```
6. No Gerenciador de soluções, clique com botão direito no aplicativo e selecione **publicar**. A caixa de diálogo **Publicar** é exibida.
7. Na **ponto de extremidade de Conexão**, selecione o ponto de extremidade para o cluster Linux do Service Fabric remoto que você deseja direcionar.
![publish-application]

<!--Image references-->
[create-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-application-remote-linux.png
[create-service]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-service-remote-linux.png
[build-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/build-application-remote-linux.png
[edit-csproj]:./media/service-fabric-how-to-vs-remote-linux-cluster/edit-csproj-remote-linux.png
[publish-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/publish-remote-linux.png

## <a name="next-steps"></a>Próximas etapas
* Saiba mais sobre [Introdução ao Service Fabric com o.Net Core](https://azure.microsoft.com/resources/samples/service-fabric-dotnet-core-getting-started/)

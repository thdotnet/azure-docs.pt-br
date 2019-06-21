---
title: Versões de Service Fabric do Azure
description: Notas de versão para os últimos recursos e aprimoramentos no Service Fabric.
author: athinanthny
manager: chackdan
ms.author: atsenthi
ms.date: 6/10/2019
ms.topic: conceptual
ms.service: service-fabric
hide_comments: true
hideEdit: true
ms.openlocfilehash: 78fb96cae3d3d128da608183f37771b2ecf66dcf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165353"
---
# <a name="service-fabric-releases"></a>Versões do Service Fabric

| <a href="https://github.com/Azure/Service-Fabric-Troubleshooting-Guides" target="blank">Guias de solução de problemas</a> 
| <a href="https://github.com/Azure/service-fabric-issues" target="blank">acompanhamento de questões</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-support" target="blank">opções de suporte</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-versions" target="blank">versões com suporte</a>  
|  <a href="https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0" target="blank">Exemplos de código</a>

Este artigo fornece mais informações sobre as versões mais recentes e atualizações para a execução do Service Fabric e os SDKs.

## <a name="whats-new-in-service-fabric"></a>**O que há de novo no Service Fabric**

### <a name="service-fabric-65"></a>O Service Fabric 6.5

A versão mais recente do Service Fabric inclui aprimoramentos de desempenho, confiabilidade e capacidade de suporte para novos recursos, correções de bug e aprimoramentos para facilitar o gerenciamento de ciclo de vida do cluster e aplicativo.

> [!IMPORTANT]
> Serviço do Fabric 6.5 é a versão final com o Service Fabric, ferramentas de suportam no Visual Studio 2015. Os clientes são aconselhados a mover para [Visual Studio de 2019](https://visualstudio.microsoft.com/vs/) no futuro.

Aqui está o que há de novo no Service Fabric 6.5:

- Explorador do Service Fabric inclui um [Visualizador de imagem do Store](service-fabric-visualizing-your-cluster.md#image-store-viewer) para inspecionar os aplicativos que você carregou na repositório de imagens.

- [Aplicativo de orquestração de patch (POA)](service-fabric-patch-orchestration-application.md) versão [1.4.0](https://github.com/microsoft/Service-Fabric-POA/releases/tag/v1.4.0) inclui vários aprimoramentos de autodiagnóstico. Os clientes de POA são recomendados para mover para esta versão.

- [EventStore Service está habilitado por padrão](service-fabric-visualizing-your-cluster.md#event-store) para clusters do Service Fabric 6.5, a menos que você tenha recusado.

- Adicionado [eventos de ciclo de vida de réplica](service-fabric-diagnostics-event-generation-operational.md#replica-events) para serviços com estado.

- [Melhor visibilidade do status do nó de semente](service-fabric-understand-and-troubleshoot-with-system-health-reports.md#seed-node-status), incluindo avisos do nível do cluster, se um nó de propagação não está íntegro (*para baixo*, *removido* ou *desconhecido*).

- [Ferramenta de recuperação de desastres do Service Fabric aplicativo](https://github.com/Microsoft/Service-Fabric-AppDRTool) permite que os serviços com monitoração de estado do Service Fabric para recuperação rápida quando o cluster primário encontra um desastre. Dados do cluster primário continuamente são sincronizados no aplicativo standby secundário usando o backup periódico e restauração.

- Suporte para Visual Studio [publicação de aplicativos do .NET Core para clusters baseados em Linux](service-fabric-how-to-publish-linux-app-vs.md).

- [CLI de malha de serviço do Azure (SFCTL)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) serão instalados automaticamente para o Service Fabric 6.5 (e versões posteriores) quando você atualizar ou criar um novo cluster do Linux no Azure.

- [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) é instalado por padrão em clusters de OneBox MacOS/Linux.

Para obter mais detalhes, consulte o [notas de versão do Service Fabric 6.5](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf).

## <a name="previous-versions"></a>Versões anteriores

### <a name="service-fabric-64-releases"></a>Versões 6.4 de malha de serviço

| Data de liberação | Versão | Mais informações |
|---|---|---|
| 30 de novembro de 2018 | [Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2018/11/30/azure-service-fabric-6-4-release/)  | [Notas de versão](https://msdnshared.blob.core.windows.net/media/2018/12/Service-Fabric-6.4-Release.pdf)|
| 12 de dezembro de 2018 | [Clusters do Service Fabric 6.4 atualização de versão para Windows do Azure](https://blogs.msdn.microsoft.com/azureservicefabric/2018/12/12/azure-service-fabric-6-4-refresh-for-windows-clusters/)  | [Notas de versão](https://msdnshared.blob.core.windows.net/media/2018/12/Links.pdf)  |
| 4 de fevereiro de 2019 | [Versão de atualização do serviço do Azure Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/02/04/azure-service-fabric-6-4-refresh-release/) | [Notas de versão](https://msdnshared.blob.core.windows.net/media/2019/02/Service-Fabric-6.4CU3-Release-Notes.pdf) |
| 4 de março de 2019 | [Versão de atualização do serviço do Azure Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/03/12/azure-service-fabric-6-4-refresh-release-2/) | [Notas de versão](https://msdnshared.blob.core.windows.net/media/2019/03/Service-Fabric-6.4CU4-Release-Notes.pdf)
| 8 de abril de 2019 | [Versão de atualização do serviço do Azure Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/04/08/azure-service-fabric-6-4-refresh-release-5/) | [Notas de versão](https://msdnshared.blob.core.windows.net/media/2019/04/Service-Fabric-6.4CU5-ReleaseNotes3.pdf)
| 2 de maio de 2019 | [Versão de atualização do serviço do Azure Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/02/azure-service-fabric-6-4-refresh-release-3/) | [Notas de versão](https://msdnshared.blob.core.windows.net/media/2019/05/Service-Fabric-64CU6-Release-Notes-V2.pdf)
| 28 de maio de 2019 | [Versão de atualização do serviço do Azure Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/28/azure-service-fabric-6-4-refresh-release-4/) | [Notas de versão](https://msdnshared.blob.core.windows.net/media/2019/05/Service_Fabric_64CU7_Release_Notes1.pdf)

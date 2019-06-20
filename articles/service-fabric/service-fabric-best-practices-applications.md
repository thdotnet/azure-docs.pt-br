---
title: Azure Service Fabric application práticas recomendadas de design | Microsoft Docs
description: Práticas recomendadas para o desenvolvimento de aplicativos do Service Fabric.
services: service-fabric
documentationcenter: .net
author: markfussell
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/18/2019
ms.author: msfussell
ms.openlocfilehash: 30d696337061ade6b79c7ec0e4c4de67651f0dad
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203450"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Azure Service Fabric application práticas recomendadas de design

Este artigo fornece diretrizes de práticas recomendadas para a criação de aplicativos e serviços no Azure Service Fabric.
 
## <a name="get-familiar-with-service-fabric"></a>Familiarize-se com o Service Fabric
* Leia as [para que você deseja saber mais sobre o Service Fabric?](service-fabric-content-roadmap.md) artigo.
* Leia sobre [cenários de aplicativo do Service Fabric](service-fabric-application-scenarios.md).
* Entender as opções de modelo de programação, lendo [visão geral do modelo de programação do Service Fabric](service-fabric-choose-framework.md).



## <a name="application-design-guidance"></a>Diretrizes de design de aplicativo
Familiarize-se com o [arquitetura geral](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) de aplicativos do Service Fabric e seus [considerações de design](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations).

### <a name="choose-an-api-gateway"></a>Escolha um gateway de API
Use um serviço de gateway de API que se comunica com os serviços de back-end que podem ser escalados horizontalmente. Os serviços de gateway de API mais comuns usados são:

- [Gerenciamento de API do Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-api-management-overview), que é [integrado com o Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-api-management).
- [O IoT Hub](https://docs.microsoft.com/azure/iot-hub/) ou [Hubs de eventos](https://docs.microsoft.com/azure/event-hubs/), usando o [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) para ler de partições do Hub de eventos.
- [Proxy reverso do Træfik](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/), usando o [provedor do Azure Service Fabric](https://docs.traefik.io/configuration/backends/servicefabric/).
- [Gateway de Aplicativo do Azure](https://docs.microsoft.com/azure/application-gateway/).

   > [!NOTE] 
   > O Gateway de aplicativo do Azure não está diretamente integrado com o Service Fabric. Gerenciamento de API do Azure normalmente é a opção preferencial.
- Seu próprio personalizadas [ASP.NET Core](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore) gateway de aplicativo web.

### <a name="stateless-services"></a>Serviços sem monitoração de estado
É recomendável que você sempre inicie criando serviços sem monitoração de estado por meio [Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) e armazenar o estado em um banco de dados do Azure, Azure Cosmos DB ou armazenamento do Azure. Estado externalizado é a abordagem mais familiar para a maioria dos desenvolvedores. Essa abordagem também permite que você tirar proveito dos recursos de consulta no repositório.  

### <a name="when-to-use-stateful-services"></a>Quando usar os serviços com estado
Quando você tiver um cenário de baixa latência e precisa manter os dados próximos a computação, considere os serviços com estado. Alguns cenários de exemplo incluem dispositivos do IoT gêmeo digital, o estado do jogo, estado de sessão, cache de dados de um banco de dados e fluxos de trabalho de longa execução para rastrear chamadas para outros serviços.

Decida sobre o período de retenção de dados:

- **Dados armazenados em cache**. Usar o cache quando a latência em armazenamentos externos é um problema. Usar um serviço com monitoração de estado como seu próprio cache de dados ou considere usar o [Cache distribuído do código-fonte aberto SoCreate Service Fabric](https://github.com/SoCreate/service-fabric-distributed-cache). Nesse cenário, você não precisa se preocupar se perder todos os dados no cache.
- **Dados de limite de tempo**. Nesse cenário, você precisa manter dados próximos de computação para um período de tempo de latência, mas você pode perder os dados em um *desastres*. Por exemplo, em muitas soluções de IoT, dados precisam estar próximo de computação, como quando a temperatura média nos últimos dias está sendo calculada, mas se esses dados são perdidos, os pontos de dados específico registrados não são tão importantes. Além disso, esse cenário não normalmente se preocupa fazendo backup de pontos de dados individuais. Você só pode fazer backup valores computados de média que periodicamente são gravados no armazenamento externo.  
- **Dados de longo prazo**. Coleções confiáveis podem armazenar seus dados permanentemente. Mas nesse caso, você precisará [preparar para recuperação de desastres](https://docs.microsoft.com/azure/service-fabric/service-fabric-disaster-recovery), incluindo [configurar políticas de backup periódicas](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-configure-periodic-backup) para seus clusters. Na verdade, você configurar o que acontece se o cluster for destruído em caso de desastre, em que você precisa criar um novo cluster e como implantar novas instâncias de aplicativo e recuperar a partir do backup mais recente.

Economizar custos e melhorar a disponibilidade:
- Você pode reduzir os custos usando os serviços com monitoração de estado porque você não incorra em acesso a dados e os custos de transações de armazenamento remoto e porque você não precisará usar outro serviço, como o Cache do Azure para Redis.
- Usando os serviços com monitoração de estado principalmente para armazenamento e não para a computação é cara, e não é recomendável. Pense serviços com monitoração de estado como a computação com barato armazenamento local.
- Removendo dependências em outros serviços, você pode melhorar a disponibilidade do serviço. Gerenciamento de estado com alta disponibilidade no cluster, você isola de outros tempos de inatividade do serviço ou problemas de latência.

## <a name="how-to-work-with-reliable-services"></a>Como trabalhar com Reliable Services
Service Fabric Reliable Services permite que você crie facilmente serviços com e sem monitoração de estado. Para obter mais informações, consulte o [Introdução aos Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction).
- Sempre respeitam a [token de cancelamento](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-lifecycle#stateful-service-primary-swaps) na `RunAsync()` método para serviços com e sem estado e o `ChangeRole()` método para serviços com monitoração de estado. Se você não fizer isso, o Service Fabric não sabe se seu serviço pode ser fechado. Por exemplo, se você não respeitar o token de cancelamento, tempos de atualização muito mais tempo do aplicativo podem ocorrer.
-   Abertura e fechamento [ouvintes de comunicação](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication) de forma oportuna e honrar os tokens de cancelamento.
-   Nunca misture código de sincronização com o código assíncrono. Por exemplo, não use `.GetAwaiter().GetResult()` em suas chamadas assíncronas. Usar async *totalmente* por meio da pilha de chamadas.

## <a name="how-to-work-with-reliable-actors"></a>Como trabalhar com Reliable Actors
Reliable Actors do Service Fabric permite que você crie facilmente atores com monitoração de estado, virtuais. Para obter mais informações, consulte o [Introdução aos Reliable Actors](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction).

- Considere seriamente usando pub/sub mensagens entre seus atores para dimensionar seu aplicativo. As ferramentas que fornecem esse serviço incluem o [livre SoCreate serviço Fabric Pub/Sub](https://service-fabric-pub-sub.socreate.it/) e [do barramento de serviço do Azure](https://docs.microsoft.com/azure/service-bus/).
- Verifique o estado do ator como [granular quanto possível](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices).
- Gerenciar o [ciclo de vida do ator](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices). Exclua atores se você não vai usá-los novamente. Exclusão de atores desnecessários é especialmente importante quando você estiver usando o [provedor de estado volátil](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#state-persistence-and-replication), porque todo o estado é armazenado na memória.
- Por causa da suas [simultaneidade baseada em turno](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction#concurrency), os atores são melhor usados como objetos independentes. Não criar grafos de chamadas de método síncrono de ator várias (cada um deles provavelmente se torna uma chamada de rede separado) ou criar solicitações de ator circular. Esses serão afetar significativamente o desempenho e escala.
- Não misture códigos de sincronização com o código assíncrono. Use async consistentemente para evitar problemas de desempenho.
- Não faça chamadas de longa execução nos atores. Chamadas de longa execução bloqueará outras chamadas para o mesmo ator, devido à simultaneidade baseada em turno.
- Se você estiver se comunicando com outros serviços usando [comunicação remota do Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-remoting) e você estiver criando um `ServiceProxyFactory`, criar a fábrica no [serviço de ator](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-using) nível e *não* no nível de ator.


## <a name="application-diagnostics"></a>Diagnóstico de aplicativo
Seja completa sobre como adicionar [log de aplicativo](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) em chamadas de serviço. Ele ajudará você a diagnosticar a cenários em que os serviços chamam uns aos outros. Por exemplo, quando um chamadas chamadas B C chama D, a chamada pode falhar em qualquer lugar. Se você não tem suficiente registro em log, as falhas são difíceis de diagnosticar. Se os serviços estão fazendo muito por causa de volumes de chamada, certifique-se de que pelo menos registrar erros e avisos.

## <a name="iot-and-messaging-applications"></a>Aplicativos de mensagens e IoT
Quando você está lendo mensagens de [IoT Hub do Azure](https://docs.microsoft.com/azure/iot-hub/) ou [Hubs de eventos](https://docs.microsoft.com/azure/event-hubs/), use [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor). ServiceFabricProcessor se integra ao Service Fabric Reliable Services para manter o estado de leitura do hub de eventos particiona e envia novas mensagens para seus serviços por meio de `IEventProcessor::ProcessEventsAsync()` método.


## <a name="design-guidance-on-azure"></a>Diretrizes de design no Azure
* Visite o [Centro de arquitetura do Azure](https://docs.microsoft.com/azure/architecture/microservices/) para obter diretrizes de design sobre [criação de microsserviços no Azure](https://docs.microsoft.com/azure/architecture/microservices/).

* Visite [Introdução ao Azure para jogos](https://docs.microsoft.com/gaming/azure/) para obter diretrizes de design sobre [usando o Service Fabric nos serviços de jogos](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf).

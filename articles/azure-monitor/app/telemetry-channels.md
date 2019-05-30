---
title: Canais de telemetria no Azure Application Insights | Microsoft Docs
description: Como personalizar os canais de telemetria no Azure Application Insights SDKs para.NET/.NET Core.
services: application-insights
documentationcenter: .net
author: cijothomas
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/14/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: fa99e29e9ec6c2bef7296a50dd381ee5fc60a1cb
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255817"
---
# <a name="telemetrychannel-in-application-insights"></a>TelemetryChannel no Application Insights

TelemetryChannel é parte integrante do [SDKs do Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Ele gerencia o armazenamento em buffer e transmissão de telemetria ao serviço Application Insights. As versões do .NET e .NET Core dos SDKs têm dois TelemetryChannels internos - `InMemoryChannel` e `ServerTelemetryChannel`. Este artigo descreve cada canal detalhadamente, incluindo como os usuários podem personalizar o comportamento do canal.

## <a name="what-is-a-telemetrychannel"></a>O que é um TelemetryChannel?

`TelemetryChannel` é responsável pelo armazenamento em buffer e enviar itens de telemetria ao serviço Application Insights, onde ele é armazenado para consultar e análise. É qualquer classe que implementa a interface [`Microsoft.ApplicationInsights.ITelemetryChannel`](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.channel.itelemetrychannel?view=azure-dotnet)

O `Send(ITelemetry item)` método de TelemetryChannel é chamado depois que todos os `TelemetryInitializer`s e `TelemetryProcessor`s são chamados. Isso significa que todos os itens descartados pelo `TelemetryProcessor` não atingirá o canal. `Send()` não normalmente enviam os itens instantaneamente para o back-end. Eles são geralmente armazenados em buffer na memória e enviados em lotes para transmissão eficiente.

[LiveMetrics](live-stream.md) também tem um canal personalizado, que ativa o streaming ao vivo de telemetria. Esse canal é independente do canal de telemetria regular, e este documento não se aplica ao canal usado pelo `LiveMetrics`.

## <a name="built-in-telemetrychannels"></a>TelemetryChannels internos

SDK do Application Insights.NET/.NET Core é fornecido com dois canais internos:

* **InMemoryChannel** 
 `InMemoryChannel` é um canal de leve, que armazena itens em buffer na memória até que ele seja enviado. Os itens são armazenados em buffer na memória e liberados de uma vez a cada 30 segundos ou sempre que 500 itens têm armazenada em buffer. Esse canal oferece garantias de confiabilidade mínima, como ele não tentará repetir o envio de telemetria após falhas. Este canal não manter itens no disco, portanto, todos os itens não enviados serão perdidos permanentemente após o desligamento do aplicativo (normalmente ou não). Há um `Flush()` método implementado por este canal, que pode ser usado para forçar liberação de todos os itens na memória telemetria sincronicamente. Isso é adequado para aplicativos de curta execução em que uma liberação síncrona é ideal.

    Esse canal é fornecido como parte do `Microsoft.ApplicationInsights` nuget do pacote em si e é o canal padrão o SDK usa quando nada mais é configurado.

* **ServerTelemetryChannel** 
 `ServerTelemetryChannel` é um canal mais avançado, que tem políticas de repetição e a capacidade de armazenar dados no disco local. Esse canal de repetições de envio de telemetria, se ocorrerem erros transitórios. Esse canal também usa o armazenamento de disco local para manter os itens no disco durante interrupções de rede ou volumes de telemetria alta. Devido a esses mecanismos de repetição e o armazenamento de disco local, esse canal é considerado mais confiável e é recomendado para todos os cenários de produção. Esse canal é o padrão para [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) e [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) aplicativos, que são configurados de acordo com os documentos oficiais vinculados. Esse canal é otimizado para cenários de servidor de processos de longa execução. O [ `Flush()` ](#which-channel-should-i-use) método implementado por este canal não é síncrono.

    Esse canal é enviado como o pacote do NuGet `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel`e é colocado automaticamente ao usar qualquer um dos pacotes NuGet `Microsoft.ApplicationInsights.Web` ou `Microsoft.ApplicationInsights.AspNetCore`.

## <a name="configuring-telemetrychannel"></a>Configurando TelemetryChannel

Canal de telemetria pode ser configurado definindo o estado desejado `TelemetryChannel` sobre o Active Directory `TelemetryConfiguration`. Para aplicativos Asp.Net, configuração envolve a configuração `TelemetryChannel` na `TelemetryConfiguration.Active`, ou modificando `ApplicationInsights.config`. Para aplicativos ASP.NET Core, a configuração envolve a adição de canal desejado para o contêiner de injeção de dependência.

A seguir mostra um exemplo em que o usuário está configurando o `StorageFolder` para o canal. `StorageFolder` é apenas uma das definições configuráveis. A lista completa de definições de configuração é descrita [na seção configurações](telemetry-channels.md#configurable-settings-in-channel).

## <a name="configuration-using-applicationinsightsconfig-for-aspnet-applications"></a>Configuração usando o applicationinsights. config para aplicativos ASP.NET

A seguinte seção de [applicationinsights. config](configuration-with-applicationinsights-config.md) mostra ServerTelemetryChannel configurado com `StorageFolder` definido como um local personalizado.

```xml
    <TelemetrySinks>
        <Add Name="default">
            <TelemetryProcessors>
                <!--TelemetryProcessors omitted for brevity  -->
            </TelemetryProcessors>
            <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel">
                <StorageFolder>d:\temp\applicationinsights</StorageFolder>
            </TelemetryChannel>
        </Add>
    </TelemetrySinks>
```

## <a name="configuration-in-code-for-aspnet-applications"></a>Configuração no código para aplicativos ASP.NET

O código a seguir configura ServerTelemetryChannel com `StorageFolder` definido como um local personalizado. Esse código deve ser adicionado no início do aplicativo, normalmente no método Application_Start () em `Global.aspx.cs`

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
protected void Application_Start()
{
    var serverTelemetryChannel = new ServerTelemetryChannel();
    serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
    serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
    TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
}
```

## <a name="configuration-in-code-for-aspnet-core-applications"></a>Configuração no código para aplicativos ASP.NET Core

Modifique `ConfigureServices` método de `Startup.cs` classe conforme mostrado abaixo.

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

public void ConfigureServices(IServiceCollection services)
{
    // This sets up ServerTelemetryChannel with StorageFolder set to a custom location.
    services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel() {StorageFolder = @"d:\temp\applicationinsights" });

    services.AddApplicationInsightsTelemetry();
}

```

> [!NOTE]
> É importante observar que configurar um canal usando `TelemetryConfiguration.Active` não é recomendado para aplicativos ASP.NET Core.

## <a name="configuring-telemetrychannel-in-code-for-netnet-core-console-applications"></a>Configurando TelemetryChannel no código para aplicativos de Console Core.NET/.NET

Para aplicativos de Console, o código é o mesmo para .NET e .NET Core e é mostrado abaixo.

```csharp
var serverTelemetryChannel = new ServerTelemetryChannel();
serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
```

## <a name="operational-details-of-servertelemetrychannel"></a>Detalhes operacionais de ServerTelemetryChannel

O `ServerTelemetryChannel` buffers itens que chegam em um buffer na memória. Ele é serializado, compactado e armazenado em `Transmission` após cada 500 30 segundos ou quando itens são armazenados em buffer da instância. Um único `Transmission` instância contém até 500 itens e representa um lote de telemetria que está sendo enviado ao longo de uma chamada de https único para o serviço Application Insights. Por padrão, pode haver um máximo de 10 `Transmission`s que estão sendo enviadas em paralelo. Se a telemetria é que chegam mais rapidamente ou se o back-end de rede/Application Insights é lento, em seguida, `Transmission`s armazenada na memória. A capacidade padrão desse buffer de transmissão na memória é de 5 MB. Depois de excede a capacidade de memória, `Transmission`s são armazenados no disco local para até 50 MB. `Transmission`s são armazenados no disco local, quando há problemas de rede também. Somente os itens armazenados no disco local sobrevivem a uma falha de aplicativo, que é enviado sempre que o aplicativo for iniciado novamente.

## <a name="configurable-settings-in-channel"></a>Definições configuráveis no canal

A lista completa de definições configuráveis para cada canal estão aqui:

[InMemoryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Channel/InMemoryChannel.cs)

[ServerTelemetryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)

Mais comumente usadas configurações para `ServerTelemetryChannel` estão listadas abaixo:

1. `MaxTransmissionBufferCapacity` -Máximo de memória, em bytes, usada pelo canal para transmissões de buffer na memória. Quando essa capacidade é atingida, novos itens serão armazenados diretamente no disco local. O padrão é 5 MB. Definir um valor maior de clientes potenciais menor uso do disco, mas é importante observar que os itens na memória serão perdidos em caso de falha de aplicativo.

2. `MaxTransmissionSenderCapacity` -Máximo de `Transmission`s que serão enviados ao Application Insights ao mesmo tempo. O padrão é 10, mas ele pode ser configurado como um número maior. Isso é recomendado quando um grande volume de telemetria é gerado, normalmente ao realizar testes de carga e/ou quando a amostragem é desativada.

3. `StorageFolder` -A pasta usada pelo canal para armazenar itens em disco conforme necessário. No Windows, % LocalAppData % ou % Temp % é usado, se nada estiver configurado explicitamente. Em ambientes não Windows, usuário **deve** configurar um local válido, sem a qual telemetria não armazenada no disco local.

## <a name="which-channel-should-i-use"></a>Qual canal deve usar?

`ServerTelemetryChannel` é recomendado para a maioria dos cenários de produção de aplicativos de longa execução. O `Flush()` implementação de método de `ServerTelemetryChannel` não é síncrono, e `Flush()` não garante o envio de todos os itens pendentes do disco/memória. Se esse canal é usado em cenários onde o aplicativo está prestes a desligar e, em seguida, é recomendável fazer algum atraso após a chamada `Flush()` neste canal. O valor exato do atraso necessário não é previsível, pois ela depende de fatores como quantos itens ou `Transmissions` estão na memória, quantos estão no disco, quantos estão sendo transmitidos para backup, e se o canal está no meio de cenários de retirada exponenciais. Se não houver a necessidade de fazer a liberação síncrona, em seguida, `InMemoryChannel` é recomendado.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="does-applicationinsights-channel-offer-guaranteed-telemetry-delivery-or-what-are-the-scenarios-where-telemetry-can-be-lost"></a>*O canal do Application Insights oferece entrega garantida de telemetria ou quais são os cenários em que a telemetria pode ser perdida?*

* Resposta curta é que nenhum dos canais internos oferece garantia de tipo de transação sobre a entrega de telemetria para o back-end. Embora `ServerTelemetryChannel` é muito mais avançado em comparação com `InMemoryChannel` para a entrega confiável de telemetria, ele também faz uma tentativa de melhor esforço para enviar a telemetria e telemetria ainda pode ser perdida em vários cenários. Alguns dos cenários comuns onde a telemetria é perdida incluem:

1. Itens na memória são perdidos sempre que o aplicativo falhe.
1. Telemetria é armazenada no disco local durante interrupções de rede ou problemas com o back-end do Application Insights. No entanto, os itens com mais de 24 horas serão descartadas. Portanto, telemetria é perdida durante o período de problemas de rede.
1. Os locais de disco padrão para armazenar telemetria no Windows são % LocalAppData % ou % Temp %. Esses locais são normalmente locais no computador. Se o aplicativo fisicamente migra de um local para outro, qualquer telemetria armazenada nesse local será perdida.
1. Em aplicativos Web do Azure (Windows), o local de disco padrão é "D:\local\LocalAppData". Esse local não é mantido e forem apagado em reinicializações de aplicativo, escala desvantagens e assim por diante, levando à perda de telemetria armazenada nesses locais. Os usuários podem substituir o armazenamento para um local persistente, como "D:\home.", mas esses locais persistentes abaixo são atendidas pelo armazenamento remoto e podem ser lentos.

### <a name="does-servertelemetrychannel-work-in-non-windows-systems"></a>*ServerTelemetryChannel funciona em sistemas não Windows?*

* Apesar do nome do pacote/namespace sendo WindowsServer, esse canal tem suporte em sistemas não Windows com a seguinte exceção. Em não-Windows, o canal não cria uma pasta de armazenamento local por padrão. Os usuários devem criar uma pasta de armazenamento local e configurar o canal para usá-lo. Depois que o armazenamento local é configurado, o canal funciona mesmo no Windows e em sistemas não Windows.

### <a name="does-the-sdk-create-temporary-local-storage-is-the-data-encrypted-at-storage"></a>*O SDK criar armazenamento local temporário? Os dados são criptografados no armazenamento?*

* SDK armazena itens de telemetria no armazenamento local durante a problemas de rede ou de limitação. Esses dados não são criptografados localmente.
Para sistemas Windows, o SDK automaticamente cria uma pasta local temporária no diretório TEMP ou APPDATA e restringe o acesso a administradores e o usuário atual.
Para não-Windows, nenhum armazenamento local é criado automaticamente pelo SDK e, portanto, nenhum dado é armazenado localmente por padrão. Os usuários podem criar um diretório de armazenamento em si e configurar o canal para usá-lo. Nesse caso, o usuário é responsável por garantir que esse diretório está protegido.
Leia mais sobre [privacidade e proteção de dados](data-retention-privacy.md#does-the-sdk-create-temporary-local-storage).

## <a name="open-source-sdk"></a>SDK do código-fonte aberto
Como todos os SDKs do Application Insights, os canais também são código-fonte aberto. Ler e contribuir para o código ou relatar problemas na [repositório GitHub oficial](https://github.com/Microsoft/ApplicationInsights-dotnet).

## <a name="next-steps"></a>Próximas etapas

* [Amostragem](../../azure-monitor/app/sampling.md)
* [Solução de problemas de SDK](../../azure-monitor/app/asp-net-troubleshoot-no-data.md)

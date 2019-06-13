---
title: Resiliência e recuperação de desastre da Configuração de Aplicativos do Azure | Microsoft Docs
description: Uma visão geral de como implementar a resiliência e a recuperação de desastre com a Configuração de Aplicativos do Azure.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 05/29/2019
ms.author: yegu
ms.openlocfilehash: 39e7a62899a7d1d6feb5bfd3b45ad4adc3c996a0
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394065"
---
# <a name="resiliency-and-disaster-recovery"></a>Resiliência e recuperação de desastre

Atualmente, a Configuração de Aplicativos do Azure é um serviço regional. Cada repositório de configuração é criado em uma determinada região do Azure. Uma interrupção em toda a região terá impacto sobre todos os repositórios dessa região. A Configuração de Aplicativos não oferece failover automático para outra região. Este artigo fornece orientações gerais sobre como você pode usar vários repositórios de configuração nas regiões do Azure para aumentar a resiliência geográfica de seu aplicativo.

## <a name="high-availability-architecture"></a>Arquitetura de alta disponibilidade

Para obter redundância entre regiões, crie vários repositórios de configuração de aplicativos em regiões diferentes. Com essa configuração, seu aplicativo terá pelo menos um repositório de configuração adicional para fazer fallback para o qual o repositório primário ficará inacessível. Veja abaixo um diagrama que ilustra a topologia entre seu aplicativo e seus repositórios de configuração primário e secundário.

![Repositórios com redundância geográfica](./media/geo-redundant-app-configuration-stores.png)

Seu aplicativo carregará sua configuração dos repositórios primário e secundário em paralelo. Isso aumentará significativamente a chance de obter dados de configuração com sucesso. Você é responsável por manter os dados em ambos repositórios em sincronia. As seções a seguir explicam como você pode criar a resiliência geográfica em seu aplicativo.

## <a name="failover-between-configuration-stores"></a>Failover entre os repositórios de configuração

Tecnicamente, seu aplicativo não está executando um failover. Ela está tentando recuperar o mesmo conjunto de dados de configuração de dois repositórios de configuração de aplicativos simultaneamente. Você deve organizar seu código de forma que ele seja carregado primeiro do repositório secundário e depois do repositório primário. Essa abordagem garantirá que os dados de configuração do repositório primário tenham precedência sempre que estiverem disponíveis. O snippet de código a seguir mostra como você pode implementar isso no .NET Core.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                  .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
        })
        .UseStartup<Startup>();
    }
```

Observe o parâmetro `optional` passado para a função `AddAzureAppConfiguration`. Quando definido como `true`, esse parâmetro impedirá que o aplicativo falhe o andamento do processo se a função não puder carregar dados de configuração.

## <a name="synchronization-between-configuration-stores"></a>Sincronização entre os repositórios de configuração

É importante que os repositórios de configurações com redundância geográfica tenham todos o mesmo conjunto de dados. Você pode usar a função **Export** na Configuração de Aplicativos para copiar dados do repositório primário para o secundário sob demanda. Essa função está disponível no portal e na CLI do Azure.

No portal do Azure, você pode enviar uma alteração por push para outro repositório de configurações seguindo estas etapas:

1. Navegue até a guia **Importar/Exportar**, escolha **Exportar**, **Configuração de Aplicativos** como o serviço de **Destino** e clique em **Escolher um recurso**.

2. Na nova folha que foi aberta, especifique a assinatura, o grupo de recursos e o nome do recurso do repositório secundário e clique em **Aplicar**.

3. A interface do usuário será atualizada para que você possa escolher quais dados de configuração deseja exportar para o repositório secundário. Você pode deixar o valor de hora padrão como está e definir **Do rótulo** e **Rótulo** com o mesmo valor. Clique em **Aplicar**.

4. Repita as etapas acima para todas as alterações da configuração.

Você pode automatizar esse processo de exportação usando a CLI do Azure. O comando a seguir mostra como exportar uma alteração de configuração única do repositório primário para o secundário.

    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você aprendeu como aumentar seu aplicativo para obter a resiliência geográfica durante o tempo de execução da Configuração de Aplicativos. Como alternativa, você pode inserir dados de configuração da Configuração de Aplicativos no momento da compilação ou da implantação. Para saber mais, confira [Integrar com o pipeline de CI/CD](./integrate-ci-cd-pipeline.md).


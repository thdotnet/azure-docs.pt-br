---
title: Anotações de versão para o Application Insights | Microsoft Docs
description: Adicione uma implantação ou compile marcadores para seus gráficos do gerenciador de métricas no Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mbullwin
ms.openlocfilehash: 6567d7f2ebaab5bd7b5bc8fb7b5a62970f169161
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476176"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Anotações sobre gráficos de métricas no Application Insights

As anotações de versão nos gráficos do [Metrics Explorer](../../azure-monitor/app/metrics-explorer.md) mostram onde você implantou uma nova compilação ou outro evento relevante. Elas facilitam ver se suas alterações tinham qualquer efeito sobre o desempenho do aplicativo. Eles podem ser criados automaticamente pelo [sistema de build do Azure DevOps Services](https://docs.microsoft.com/azure/devops/pipelines/tasks/). Você também pode criar anotações para sinalizar eventos desejados criando-as no PowerShell.

> [!NOTE]
> Este artigo reflete a experiência de métricas clássicas **reprovada**. Anotações só estão disponíveis no momento em que a experiência clássica em **[pastas de trabalho](../../azure-monitor/app/usage-workbooks.md)** . Para saber mais sobre a experiência de métricas atual, você pode consultar [este artigo](../../azure-monitor/platform/metrics-charts.md).

![Exemplo de anotações](./media/annotations/0-example.png)

## <a name="release-annotations-with-azure-devops-services-build"></a>Anotações de versão com o build do Azure DevOps Services

Anotações de versão são um recurso do serviço Azure Pipelines baseado em nuvem do Azure DevOps Services.

### <a name="install-the-annotations-extension-one-time"></a>Instalar a extensão Annotations (uma vez)
Para poder criar anotações de versão, você precisará instalar uma das muitas extensões do Azure DevOps Services disponíveis no Visual Studio Marketplace.

1. Entre no projeto do [Azure DevOps Services](https://azure.microsoft.com/services/devops/).
2. No Visual Studio Marketplace, [obtenha a extensão Anotações de Versão](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) e adicione-a à sua organização do Azure DevOps Services.

![Selecione uma organização de DevOps do Azure e, em seguida, instale.](./media/annotations/1-install.png)

Você só precisa fazer isso uma vez para sua organização do Azure DevOps Services. As anotações de versão agora podem ser configuradas para qualquer projeto na sua organização.

### <a name="configure-release-annotations"></a>Criar anotações de versão

Você precisa obter uma chave de API separada para cada modelo de versão do Azure DevOps Services.

1. Entrar para o [portal do Microsoft Azure](https://portal.azure.com) e abra o recurso Application Insights que monitora seu aplicativo. (Ou [crie um agora](../../azure-monitor/app/app-insights-overview.md), se você ainda não fez isso.)
2. Abra o **acesso à API** guia e copiar a **ID do Application Insights**.
   
    ![Em portal.azure.com, abra o recurso do Application Insights e escolha Configurações. Abra Acesso à API. Copie a ID do Aplicativo](./media/annotations/2-app-id.png)

4. Em um janela de navegador separada, abra (ou crie) o modelo de versão que gerencia suas implantações do Azure DevOps Services.
   
    Adicione uma tarefa e selecione a tarefa de anotação de versão do Application Insights do menu.

   ![Clique no sinal de adição para adicionar tarefa e selecione anotação de versão do Application Insights. Cole a ID do Application Insights.](./media/annotations/3-add-task.png)

    Cole a **ID do aplicativo** que você copiou do guia de acesso à API.
   
    ![Cole a ID do Application Insights](./media/annotations/4-paste-app-id.png)

5. De volta à janela do Azure, crie uma nova chave de API e faça uma cópia dela.
   
    ![Na guia acesso à API na janela do Azure, clique em criar chave de API.](./media/annotations/5-create-api-key.png)

    ![Na guia chaves de API para criação, forneça um comentário, marque gravar anotações e clique em Gerar chave. Copie a nova chave.](./media/annotations/6-create-api-key.png)

6. Abra a guia Configuração do modelo de versão.
   
    Crie uma definição de variável para `ApiKey`.
   
    Cole sua chave de API para a definição da variável ApiKey.
   
    ![Na janela Serviços de DevOps do Azure, selecione a guia de variável e clique em Adicionar. Defina o nome para ApiKey e no valor, cole a chave que você gerou e clique no ícone de bloqueio.](./media/annotations/7-paste-api-key.png)
1. Por fim, **Salve** o pipeline de lançamento.


## <a name="view-annotations"></a>Exibir anotações
Agora, sempre que você usar o modelo de versão para implantar uma nova versão, uma anotação será enviada ao Application Insights. As anotações aparecerão em gráficos no Metrics Explorer.

Clique em qualquer marcador de anotação (seta cinza clara) para abrir os detalhes sobre a versão, incluindo o solicitante, a ramificação de controle do código-fonte, release pipeline, o ambiente e muito mais.

![Clique em qualquer marcador de anotação de versão.](./media/annotations/8-release.png)

## <a name="create-custom-annotations-from-powershell"></a>Criar anotações personalizadas no PowerShell
Você também pode criar anotações de qualquer processo que desejar (sem usar o Azure DevOps Services). 


1. Faça uma cópia local do [script do Powershell do GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Obtenha a ID do aplicativo e crie uma chave de API a partir da guia de acesso à API.

3. Chame o script assim:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

É fácil modificar o script, por exemplo, para criar as anotações para o que já passou.

## <a name="next-steps"></a>Próximas etapas

* [Criar um item de trabalho](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [Automação com o PowerShell](../../azure-monitor/app/powershell.md)

---
title: Usar análise de alterações de aplicativo no Azure Monitor para localizar problemas de aplicativo web | Microsoft Docs
description: Use análise de alterações de aplicativo no Azure Monitor para solucionar problemas de aplicativos em sites ao vivo no serviço de aplicativo do Azure.
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: 45df8f9e57223ea60a11c6af2187d362184cae2b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443369"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Usar análise de alterações de aplicativo (visualização) no Azure Monitor

Quando um problema de site ativo ou uma interrupção ocorre, é essencial determinar rapidamente a causa raiz. Soluções de monitoramento padrão podem alertá-lo a um problema. Eles ainda podem indicar qual componente está falhando. Mas esse alerta não ocorrem sempre imediatamente explica a causa da falha. Você sabe que seu site trabalhado há cinco minutos, e agora estiver quebrado. O que mudou nos últimos cinco minutos? Esta é a pergunta que a análise de alterações de aplicativo é projetado para responder no Azure Monitor.

Criando o poder das [grafo de recursos do Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview), análise de alterações fornece percepções sobre as alterações de aplicativo do Azure para aumentar a observação e reduzir o MTTR (tempo médio para reparo).

> [!IMPORTANT]
> Análise de alterações está atualmente em visualização. Esta versão de visualização é fornecida sem um contrato de nível de serviço. Esta versão não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter recursos restritos. Para obter mais informações, consulte [termos de uso complementares para visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Visão geral

Análise de alterações detecta vários tipos de alterações, da camada de infraestrutura para implantação do aplicativo. Ele é um provedor de recursos de nível de assinatura do Azure que verifica as alterações de recursos na assinatura. Análise de alterações fornece dados de várias ferramentas de diagnóstico ajudar os usuários a entender quais alterações podem ter causado problemas.

O diagrama a seguir ilustra a arquitetura de análise de alterações:

![Diagrama da arquitetura de como alterar análise obtém dados de alteração e fornece-o a ferramentas de cliente](./media/change-analysis/overview.png)

Atualmente, a análise de alterações é integrado ao **diagnosticar e resolver problemas** experiência no aplicativo serviço de aplicativo web. Para habilitar a detecção de alteração e exibir as alterações no aplicativo web, consulte a *alterar análise para o recurso aplicativos Web* seção mais adiante neste artigo.

### <a name="azure-resource-manager-deployment-changes"></a>Alterações na implantação do Gerenciador de recursos do Azure

Usando o [grafo de recursos do Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview), análise de alterações fornece um registro histórico de como os recursos do Azure que hospedam seu aplicativo foram alterados ao longo do tempo. Por exemplo, análise de alterações pode detectar, alterações em regras de configuração de IP, identidades gerenciadas e as configurações de SSL. Portanto, se uma marca é adicionada a um aplicativo web, análise alterar reflete a alteração. Essas informações estão disponíveis, desde o `Microsoft.ChangeAnalysis` provedor de recursos está habilitado na assinatura do Azure.

### <a name="changes-in-web-app-deployment-and-configuration"></a>Alterações na configuração e implantação de aplicativo web

Análise de alteração captura o estado de implantação e configuração de um aplicativo a cada 4 horas. Ele pode detectar, por exemplo, as alterações nas variáveis de ambiente do aplicativo. A ferramenta calcula as diferenças e apresenta o que mudou. Ao contrário de alterações do Gerenciador de recursos, informações de alteração de implantação de código podem não estar disponíveis imediatamente na ferramenta. Para exibir as alterações mais recentes na análise de alterações, selecione **verificação agora altera**.

![Captura de tela do botão "Verificação de alterações agora"](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Alterações de dependência

Alterações em dependências de recursos também podem causar problemas em um aplicativo web. Por exemplo, se um aplicativo web chama um cache Redis, o SKU do cache Redis pode afetar o desempenho do aplicativo web. Para detectar alterações em dependências, análise de alterações verifica o registro DNS do aplicativo web. Dessa forma, ele identifica as alterações em todos os componentes de aplicativo que poderia causar problemas.

## <a name="change-analysis-for-the-web-apps-feature"></a>Análise de alterações para o recurso de aplicativos Web

No Azure Monitor, a análise de alterações atualmente é integrado ao autoatendimento **diagnosticar e resolver problemas** experiência. Acessar essa experiência da **visão geral** página do seu aplicativo de serviço de aplicativo.

![Captura de tela do botão "Visão geral" e "diagnosticar e solucionar problemas de" botão](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Habilitar a análise de alteração em diagnosticar e resolver a ferramenta de problemas

1. Selecione **disponibilidade e desempenho**.

    ![Captura de tela do "Disponibilidade e desempenho" opções de solução de problemas](./media/change-analysis/availability-and-performance.png)

1. Selecione **alterações no aplicativo**. Não que o recurso também está disponível no **aplicativo travar**.

   ![Captura de tela do botão "Falha de aplicativo"](./media/change-analysis/application-changes.png)

1. Para habilitar a análise de alterações, selecione **habilitar agora**.

   ![Captura de tela das opções de "Falha de aplicativo"](./media/change-analysis/enable-changeanalysis.png)

1. Ative **análise de alterações** e selecione **salvar**.

    ![Captura de tela da interface do usuário "Habilitar análise de alterações"](./media/change-analysis/change-analysis-on.png)


1. Para acessar a análise de alterações, selecione **diagnosticar e solucionar problemas** > **desempenho e disponibilidade** > **aplicativo travar**. Você verá um gráfico que resume o tipo de alterações ao longo do tempo, juntamente com detalhes sobre essas alterações:

     ![Captura de tela da exibição de comparação de alteração](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-at-scale"></a>Habilitar a análise de alteração em grande escala

Se sua assinatura inclui vários aplicativos web, permitindo que o serviço no nível do aplicativo web seria ineficiente. Nesse caso, siga estas instruções alternativas.

### <a name="register-the-change-analysis-resource-provider-for-your-subscription"></a>Registrar o provedor de recursos de análise de alterações para a sua assinatura

1. Registre-se o sinalizador de recursos de análise de alterações (versão prévia). Porque o sinalizador de recurso está em visualização, você precisa registrá-lo para torná-lo visível à sua assinatura:

   1. Abra o [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/).

      ![Captura de tela de alteração do Cloud Shell](./media/change-analysis/cloud-shell.png)

   1. Altere o tipo de shell para **PowerShell**.

      ![Captura de tela de alteração do Cloud Shell](./media/change-analysis/choose-powershell.png)

   1. Execute o seguinte comando do PowerShell:

        ``` PowerShell
        Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to enable
        Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
        Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag
        ```

1. Registre o provedor de recursos de análise de alterações para a assinatura.

   - Vá para **assinaturas**e selecione a assinatura que você deseja habilitar no serviço de alteração. Em seguida, selecione os provedores de recursos:

        ![Captura de tela mostrando como registrar o provedor de recursos de análise de alterações](./media/change-analysis/register-rp.png)

       - Selecione **Microsoft.ChangeAnalysis**. Em seguida, na parte superior da página, selecione **registrar**.

       - Depois que o provedor de recursos é habilitado, você pode definir uma marca oculta no aplicativo web para detectar alterações no nível da implantação. Para definir uma marca oculta, siga as instruções em **não é possível buscar informações de análise de alterações**.

   - Como alternativa, você pode usar um script do PowerShell para registrar o provedor de recursos:

        ```PowerShell
        Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
        ```

        Para usar o PowerShell para definir um marcador oculto em um aplicativo web, execute o seguinte comando:

        ```powershell
        $webapp=Get-AzWebApp -Name <name_of_your_webapp>
        $tags = $webapp.Tags
        $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
        Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
        ```

     > [!NOTE]
     > Depois de adicionar a marca ocultada, ainda convém aguardar até quatro horas antes de você começar a ver as alterações. Os resultados são atrasados porque a análise de alterações examina seu aplicativo web somente a cada 4 horas. O agendamento de 4 horas limita o impacto no desempenho do exame.

## <a name="next-steps"></a>Próximas etapas

- Monitorar o serviço de aplicativo com mais eficiência por [habilitação de recursos do Application Insights](azure-web-apps.md) no Azure Monitor.
- Saiba mais sobre [grafo de recursos do Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview), que ajuda a análise de alterações de energia.

---
title: Aplicativo do Azure Monitor alterar análise - descobrir alterações que possam afetar a problemas de site ativo/interrupções com o aplicativo do Azure Monitor alteram análise | Microsoft Docs
description: Solucionar problemas de site ao vivo do aplicativo em serviços de aplicativo do Azure com análise de alterações de aplicativo do Azure Monitor
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: 5bd3816e65398283de85b4551a137b3f97db4cc7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226312"
---
# <a name="application-change-analysis-public-preview"></a>Análise de alterações de aplicativo (visualização pública)

Quando ocorre uma problema/interrupção do site ativo, determinar a causa raiz rapidamente é fundamental. Padrão de soluções de monitoramento pode ajudar você a identificar rapidamente o que há um problema e muitas vezes até mesmo componente que está falhando. Mas isso não será sempre levar a uma explicação imediata por que a falha está ocorrendo. Seu site trabalhou cinco minutos atrás, agora ele está quebrado. O que mudou nos últimos cinco minutos? Esta é a pergunta que o novo recurso do Azure Monitor, análise de alterações de aplicativo é projetado para responder. Criando o poder da [grafo de recursos do Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview) análise de alterações de aplicativo fornece informações sobre as alterações de aplicativo do Azure para aumentar a observação e reduzir o MTTR (tempo médio para reparo).

> [!IMPORTANT]
> Análise de alteração de aplicativo do Azure Monitor está atualmente em visualização pública.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview-of-change-analysis-service"></a>Visão geral do serviço de análise de alterações
O serviço de análise de alteração detecta vários tipos de alterações da camada de infraestrutura para implantação do aplicativo. Ele é um provedor de recursos do Azure de nível de assinatura que se parece em alterações de recursos na assinatura e fornece dados de várias ferramentas de diagnóstico ajudar os usuários a entender quais alterações podem ter causado problemas.

O diagrama a seguir ilustra a arquitetura do serviço de análise de alteração: ![Diagrama da arquitetura para como o serviço de análise de alteração obtém dados de alteração e fornecer dados para as ferramentas de cliente](./media/change-analysis/overview.png)

Atualmente, a ferramenta é integrada em serviços de aplicativo do aplicativo web diagnosticar e resolver problemas de experiência. Ver *serviço de análise de alterações para o aplicativo Web de serviços de aplicativo* seção sobre como habilitar e exibir as alterações feitas em um aplicativo web.

### <a name="azure-resource-manager-deployment-changes"></a>Alterações na implantação do Gerenciador de recursos do Azure
Aproveitando [grafo de recursos do Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview) a ferramenta de análise de alterações fornece um registro histórico de como os recursos do Azure que hospedam seu aplicativo foram alterados ao longo do tempo. Por exemplo, se um aplicativo web tiver uma marca adicionados a ele, a alteração será refletida na ferramenta de análise de alteração.
Essas informações estão sempre disponíveis, desde o `Microsoft.ChangeAnalysis` provedor de recursos estiver integrado à assinatura do Azure.

### <a name="web-application-deployment-and-configuration-changes"></a>Alterações de configuração e implantação de aplicativo da Web
Ferramenta de análise de alteração captura o estado de implantação e configuração de um aplicativo a cada 4 horas para computar as diferenças e apresentar o que mudou. Exemplos de tais alterações incluem alterações de variável de ambiente do aplicativo, as alterações de regra de configuração de IP, alterações de identidade de serviço gerenciado, alterações nas configurações de SSL e assim por diante.
Ao contrário de alterações do Gerenciador de recursos, esse tipo de informação de alteração não estejam disponível imediatamente na ferramenta. Para exibir as alterações mais recentes, use o botão 'Altera verificação agora' na ferramenta.

![Captura de tela de verificação para que as alterações agora botão diagnosticar e resolver a ferramenta de problemas com a integração de análise de alteração para o aplicativo web do serviço de aplicativo](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Alterações de dependência
O recurso de dependências também pode ser a causa dos problemas. Por exemplo, se um aplicativo web chama um cache Redis, o desempenho do aplicativo web pode ser afetado pelo cache Redis SKU. Ao examinar o registro DNS do aplicativo web altere analysis service também apresentam as dependências de informações de alteração para identificar alterações em todos os componentes de um aplicativo que possa ter causado problemas.


## <a name="change-analysis-service-for-app-services-web-app"></a>Serviço de análise de alteração para o aplicativo Web de serviços de aplicativo

Análise de alteração de aplicativo do Azure Monitor atualmente é integrado ao autoatendimento **diagnosticar e solucionar problemas** experiência, que podem ser acessado da **visão geral** seção do serviço de aplicativo do Azure aplicativo:

![Visão geral da captura de tela do serviço de aplicativo do Azure com as caixas vermelhas em torno do botão visão geral de página e diagnosticar e resolver o botão de problemas](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-diagnose-and-solve-problems-tool"></a>Habilitar a análise de alteração em diagnosticar e resolver a ferramenta de problemas

1. Selecione **disponibilidade e desempenho**

    ![captura de tela de opções de solução de problemas de desempenho e disponibilidade](./media/change-analysis/availability-and-performance.png)

2. Clique o **aplicativo travar** lado a lado.

   ![Captura de tela com o bloco de falhas do aplicativo](./media/change-analysis/application-crashes-tile.png)

3. Para habilitar **análise de alterações** selecionar **habilitar agora**.

   ![captura de tela de opções de solução de problemas de desempenho e disponibilidade](./media/change-analysis/application-crashes.png)

4. Para tirar proveito de todo o alterar conjunto de funcionalidades de análise **alterar Analysis**, **examinar alterações de código de**, e **Always on** para **em** e selecione **salvar**.

    ![Captura de tela de habilitar o serviço de aplicativo do Azure alterar a interface do usuário de análise](./media/change-analysis/change-analysis-on.png)

    Se **análise de alterações** é habilitado, você será capaz de detectar alterações no nível do recurso. Se **examinar alterações de código de** é habilitado, você verá também os arquivos de implantação e as alterações de configuração do site. Habilitando **Always on** otimizará a desempenho da verificação de alteração, mas pode incorrer em custos adicionais de uma perspectiva de cobrança.

5.  Depois que tudo estiver habilitada, selecionando **diagnosticar e solucionar problemas** > **desempenho e disponibilidade** > **aplicativo travar** permitirá que você acesse a experiência de análise de alteração. O grafo resume o tipo de alterações que ocorreram ao longo do tempo, juntamente com detalhes sobre essas alterações:

     ![Captura de tela da exibição de comparação de alteração](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-service-at-scale"></a>Habilitar o serviço de análise de alterações em grande escala
Se você tiver muitos aplicativos web em sua assinatura, habilitar o serviço no nível do aplicativo web será ineficiente. Aqui estão algumas instruções de integração alternativo.

#### <a name="registering-change-analysis-resource-provider-for-your-subscription"></a>Registrando provedor de recursos de análise de alterações para a sua assinatura

1. Registre-se o sinalizador de recursos de visualização de análise de alterações

    Como esse recurso está em visualização, você precisa em primeiro lugar, registrar o sinalizador de recurso para que ele fique visível à sua assinatura.
    - Abra o [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/).

    ![Captura de tela de alteração do Azure Cloud Shell](./media/change-analysis/cloud-shell.png)

    - Altere o tipo de shell para PowerShell:

    ![Captura de tela de alteração do Azure Cloud Shell](./media/change-analysis/choose-powershell.png)

    - Execute o seguinte comando do PowerShell:

    ``` PowerShell

    Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to onboard
    Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
    Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag

    ```

2. Registrar o provedor de recursos de análise de alteração para a assinatura

    - Navegue até as assinaturas, selecione a assinatura que você deseja integrar o serviço de alteração e clique em provedores de recursos:

        ![Captura de tela de registro RP de análise de alterações na folha de assinaturas](./media/change-analysis/register-rp.png)

    - Selecione *Microsoft.ChangeAnalysis* e clique em *registrar* no topo da página.

    - Depois que o provedor de recursos estiver integrado, siga as instruções da *não é possível buscar informações de análise alterar* abaixo para definir a marca oculta no aplicativo web para habilitar a implantação de nível de detecção de alterações no aplicativo web.

3. Como alternativa para a etapa 2 acima, você pode se registrar para o provedor de recursos por meio de script do PowerShell:

    ```PowerShell
    Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
    ```

4. Para definir uma marca oculta em um aplicativo web usando o PowerShell, execute o seguinte comando:

    ```powershell
    $webapp=Get-AzWebApp -Name <name_of_your_webapp>
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
    ```

> [!NOTE]
> Depois que a marca ocultada é adicionada, você ainda precisa inicialmente aguardar até quatro horas para ser capaz de exibir primeiro a alterações. Isso ocorre porque o freqeuncy de 4 horas que o serviço de análise de alteração usa para verificar seu aplicativo web enquanto os limita o impacto no desempenho da verificação.

## <a name="next-steps"></a>Próximas etapas

- Melhorar o monitoramento dos serviços de aplicativo do Azure [, permitindo que os recursos do Application Insights](azure-web-apps.md) do Azure Monitor.
- Aprimorar seu entendimento do [grafo de recursos do Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview) que ajuda a aplicativo do Azure Monitor power alterar análise.

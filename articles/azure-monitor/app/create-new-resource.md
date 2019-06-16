---
title: Criar um novo recurso do Azure Application Insights | Microsoft Docs
description: Configure manualmente o monitoramento do Application Insights para um novo aplicativo em tempo real.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mbullwin
ms.openlocfilehash: 9da52e5a9dfa3b55431d66ed3162172226f71a40
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073273"
---
# <a name="create-an-application-insights-resource"></a>Criar um recurso do Application Insights

O Azure Application Insights exibe dados sobre o seu aplicativo em um *recurso* do Microsoft Azure. Por isso, criar um novo recurso faz parte da [configuração do Application Insights para monitorar um novo aplicativo][start]. Depois de criar o novo recurso, você pode obter sua chave de instrumentação e usá-la para configurar o SDK do Application Insights. A chave de instrumentação vincula a telemetria ao recurso.

## <a name="sign-in-to-microsoft-azure"></a>Entrar no Microsoft Azure

Se você não tiver uma assinatura do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="create-an-application-insights-resource"></a>Criar um recurso do Application Insights

Entrar para o [portal do Azure](https://portal.azure.com)e crie um recurso do Application Insights:

![Clique no sinal '+' no canto superior esquerdo. Selecione Ferramentas de desenvolvedor, seguido pelo Application Insights](./media/create-new-resource/new-app-insights.png)

   | Configurações        |  Value           | DESCRIÇÃO  |
   | ------------- |:-------------|:-----|
   | **Nome**      | Valor Globalmente Exclusivo | Nome que identifica o aplicativo que você está monitorando. |
   | **Grupo de recursos**     | myResourceGroup      | Nome para o grupo de recursos novo ou existente para hospedar dados do App Insights. |
   | **Localidade** | Leste dos EUA | Escolha um local perto de você ou perto onde seu aplicativo está hospedado. |

Insira os valores apropriados para os campos obrigatórios e, em seguida, selecione **revisar + criar**.

![Insira valores nos campos necessários e, em seguida, selecione "revisar + criar".](./media/create-new-resource/review-create.png)

Quando seu aplicativo tiver sido criado, um novo painel é aberto. Esse painel é onde você vê os dados de desempenho e uso sobre seu aplicativo monitorado. 

## <a name="copy-the-instrumentation-key"></a>Copiar a chave de instrumentação

A chave de instrumentação identifica o recurso que você deseja associar com os dados de telemetria. Você precisará copiar para adicionar a chave de instrumentação ao código do seu aplicativo.

![Clique e copie a chave de instrumentação](./media/create-new-resource/instrumentation-key.png)

## <a name="install-the-sdk-in-your-app"></a>Instalar o SDK em seu aplicativo

Instale o SDK do Application Insights SDK no aplicativo. Esta etapa depende muito do tipo do seu aplicativo.

Use a chave de instrumentação para configurar o [SDK instalado no seu aplicativo][start].

O SDK inclui módulos padrão que enviam telemetria sem a necessidade de escrever qualquer código adicional. Para rastrear ações do usuário ou diagnosticar problemas mais detalhadamente, [use a API][api] para enviar sua própria telemetria.

## <a name="creating-a-resource-automatically"></a>Criando um recurso automaticamente
Você pode escrever um [script do PowerShell](../../azure-monitor/app/powershell.md) para criar um recurso automaticamente.

## <a name="next-steps"></a>Próximas etapas
* [Pesquisa de Diagnóstico](../../azure-monitor/app/diagnostic-search.md)
* [Explorar métricas](../../azure-monitor/app/metrics-explorer.md)
* [Escrever consultas do Analytics](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[start]: ../../azure-monitor/app/app-insights-overview.md
---
title: Consultar dados de um ambiente Azure Time Series Insights GA usando C# código | Microsoft Docs
description: Este artigo descreve como consultar dados de um ambiente do Azure Time Series Insights codificando um aplicativo personalizado escrito na linguagem C# (C-sharp) do .NET.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
reviewer: jasonwhowell, kfile, tsidocs
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: 28f255c66c0e4a929914d037ee9c93b509a3602b
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68677776"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Consultar dados do ambiente Azure Time Series Insights GA usandoC#

Este C# exemplo demonstra como consultar dados do ambiente Azure Time Series insights ga.

O exemplo mostra vários exemplos básicos do uso da API de consulta:

1. Como uma etapa de preparação, obtenha o token de acesso usando a API do Azure Active Directory. Passe esse token no cabeçalho `Authorization` de cada solicitação de API de Consulta. Para configurar aplicativos não interativos, veja [Autenticação e autorização](time-series-insights-authentication-and-authorization.md). Além disso, certifique-se de que todas as constantes definidas no início do exemplo estão definidas corretamente.
1. A lista de ambientes aos quais o usuário tem acesso é obtida. Um dos ambientes é escolhido como o ambiente de interesse e outros dados são consultados para esse ambiente.
1. Como um exemplo de solicitação HTTPS, os dados de disponibilidade são solicitados para o ambiente de interesse.
1. Como um exemplo de solicitação do soquete Web, os dados de eventos agregados são solicitados para o ambiente de interesse. Os dados são solicitados para todo o intervalo de tempo de disponibilidade.

> [!NOTE]
> O código de exemplo está disponível [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample)em.

## <a name="project-dependencies"></a>Dependências do projeto

Adicione pacotes `Microsoft.IdentityModel.Clients.ActiveDirectory` NuGet e `Newtonsoft.Json`.

## <a name="c-example"></a>Exemplo de C#

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre consultas, leia a [referência de API de consulta](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api).

- Leia como [conectar um aplicativo JavaScript de página única](tutorial-create-tsi-sample-spa.md) para Time Series insights.
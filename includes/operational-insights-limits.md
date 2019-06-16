---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 06/10/2019
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: c5fedc59c80c68fc222693a67664ef60ddd210a9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133185"
---
Os seguintes limites se aplicam a cada espaço de trabalho do Log Analytics no atual com base no consumo de tipo de preço introduzido em abril de 2018:

|     | Por GB de 2018 |
| --- | --- | 
| Volume de dados coletado por dia | Nenhum |
| Período de retenção de dados | 30 a 730 dias<sup>1</sup> |

Os limites a seguir se aplicam a cada espaço de trabalho de análise de Log mais recente herdado tipos de preço:

|  | Grátis | Autônomo (por GB) | Por nó (OMS) |
| --- | --- | --- | --- | --- | --- |--- |
| Volume de dados coletado por dia |500 MB<sup>2</sup> |Nenhum |Nenhum |
| Período de retenção de dados |7 dias | 30 a 730 dias<sup>1</sup> | 30 a 730 dias<sup>1</sup> |

Os seguintes limites se aplicam a cada espaço de trabalho do Log Analytics herdado mais antigo que tipos de preço:

|  | Standard | Premium | 
| --- | --- | --- | --- | --- | --- |--- |
| Volume de dados coletado por dia | Nenhum | Nenhum | 
| Período de retenção de dados |30 dias | 365 dias |

<sup>1</sup>retenção de dados além dos 31 dias está disponível para encargos adicionais. Saiba mais sobre o [preço do Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

<sup>2</sup>quando seu espaço de trabalho atinge o limite de transferência de dados diário 500 MB, a análise de dados será interrompida e reiniciada no início do dia seguinte. Um dia é baseado em UTC.

>[!NOTE]
>Dependendo do quanto você usou o Log Analytics, você pode ter acesso para as camadas de preços herdado. Saiba mais sobre [herdado do Log Analytics que tipos de preço](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers). 
>

Os seguintes limites se aplicam aos recursos do Azure Log Analytics (espaços de trabalho) por assinatura.

| Tipo de preço    | Número de espaços de trabalho por assinatura | Comentários
| --- | --- | --- |
| Camada gratuita  | 10 | Esse limite não pode ser aumentado. |
| Todos os tipos diferentes de gratuito | N/D | Você está limitado pelo número de recursos dentro de um grupo de recursos e o número de grupos de recursos por assinatura. | 

Os seguintes limites se aplicam para as APIs de análise de Log:

| Categoria | limites | Comentários
| --- | --- | --- |
| API do Coletor de Dados | Tamanho máximo para uma única postagem é de 30 MB.<br>Tamanho máximo para valores de campo é 32 KB. | Dividir volumes maiores em várias postagens.<br>Campos com mais de 32 KB são truncados. |
| API de Pesquisa | 5\.000 registros retornados para dados não agregados.<br>500\.000 registros para os dados agregados. | Os dados agregados são uma pesquisa que inclui o `summarize` comando.
 

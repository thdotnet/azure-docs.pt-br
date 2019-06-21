---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: dfcf47a95d1dbff34ff322768fc4ac6c9674cff4
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296004"
---
**Retenção e volume de coleta de dados** 

| Camada | Limitar por dia | Retenção de dados | Comentário |
|:---|:---|:---|:---|
| Tipo de preço por GB atual<br>(apresentados de abril de 2018) | Sem limite | 30 - 730 dias | Retenção de dados além dos 31 dias está disponível para encargos adicionais. Saiba mais sobre os preços do Azure Monitor. |
| Camadas gratuitas herdadas<br>(apresentados de abril de 2016) | 500 MB | 7 dias | Quando o seu espaço de trabalho atinge o limite de transferência de dados diário 500 MB, a análise de dados será interrompida e reiniciada no início do dia seguinte. Um dia é baseado em UTC. |
| Camada de autônomo por GB herdada<br>(apresentados de abril de 2016) | Sem limite | 30 a 730 dias | Retenção de dados além dos 31 dias está disponível para encargos adicionais. Saiba mais sobre os preços do Azure Monitor. |
| Herdado por nó (OMS)<br>(apresentados de abril de 2016) | Sem limite | 30 a 730 dias | Retenção de dados além dos 31 dias está disponível para encargos adicionais. Saiba mais sobre os preços do Azure Monitor. |
| Camada Standard herdada | Sem limite | 30 dias  | Retenção não pode ser ajustada. |
| Camada Premium herdada | Sem limite | 365 dias  | Retenção não pode ser ajustada. |

**Número de espaços de trabalho por assinatura.**

| Tipo de preço    | Limite de espaço de trabalho | Comentários
|:---|:---|:---|
| Camada gratuita  | 10 | Esse limite não pode ser aumentado. |
| Todas as outras camadas. | Sem limite | Você está limitado pelo número de recursos dentro de um grupo de recursos e o número de grupos de recursos por assinatura. |

**Portal do Azure**

| Categoria | limites | Comentários |
|:---|:---|:---|
| Máximo de registros retornado por uma consulta de log | 10.000 | Reduza os resultados usando o escopo da consulta, o intervalo de tempo e filtros na consulta. |


**API do Coletor de dados**

| Categoria | limites | Comentários |
|:---|:---|:---|
| Tamanho máximo para uma única postagem | 30 MB | Dividir volumes maiores em várias postagens. |
| Tamanho máximo para valores de campo  | 32 KB | Campos com mais de 32 KB são truncados. |

**API de pesquisa**

| Categoria | limites | Comentários |
|:---|:---|:---|
| Máximo de registros retornado para dados não agregados | 5\.000 | |
| Máximo de registros para os dados agregados | 500,000 | Os dados agregados são uma pesquisa que inclui o `summarize` comando. |
| Tamanho máximo dos dados retornados | 64.000.000 bytes (~ 61 MiB)| |
| Tempo de execução de consulta máxima | 10 minutos | Ver [tempos limite](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts) para obter detalhes.  |
| Taxa de solicitação máxima | 200 solicitações por 30 segundos por endereço IP de cliente ou usuário do AAD | Ver [limites de taxa](https://dev.loganalytics.io/documentation/Using-the-API/Limits) para obter detalhes. |

**Limites de espaço de trabalho geral**

| Categoria | limites | Comentários |
|:---|:---|:---|
| Máximo de colunas em uma tabela         | 500 | |
| Número máximo de caracteres para nome de coluna | 500 | |
| Regiões na capacidade total | Centro-Oeste dos EUA | No momento, você não pode criar um novo espaço de trabalho nesta região, pois ele está no limite de capacidade temporário. Esse limite é planejado para ser resolvido com o final de setembro de 2019. |
| Exportação de dados | Não disponível no momento | Use a função do Azure ou o aplicativo lógico para agregar e exportar dados. | 

>[!NOTE]
>Dependendo do quanto você usou o Log Analytics, você pode ter acesso para as camadas de preços herdado. Saiba mais sobre [herdado do Log Analytics que tipos de preço](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers). 
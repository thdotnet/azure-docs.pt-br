---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 10/11/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 9ffb02fce41e8805dfccf1dfd6e982cf107039ec
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127539"
---
| **Recurso** | **Limite padrão** | **Limite máximo** |
| --- | --- | --- |
| Contas do lote do Azure por região e assinatura | 1-3 |50 |
| Núcleos dedicados por conta do Lote | 10-100 | N/A<sup>1</sup> |
| Núcleos de baixa prioridade por conta do Lote | 10-100 | N/A<sup>2</sup> |
| Trabalhos ativos e agendas de trabalho<sup>3</sup> por Conta de lote | 100-300 | 1,000<sup>4</sup> |
| Pools por conta do Lote | 20-100 | 500<sup>4</sup> |

> [!NOTE]
> Os limites padrão variam dependendo do tipo de assinatura que você usa para criar uma conta do Lote. As cotas de núcleos mostradas são para as contas do Lote no modo do Serviço de lote. [Exibir as cotas na conta do Lote](../articles/batch/batch-quota-limit.md#view-batch-quotas). 

<sup>1</sup>o número de núcleos dedicados por conta do lote pode ser aumentado, mas o número máximo não está especificado. Para discutir as opções de aumento, entre em contato com o suporte do Azure.

<sup>2</sup>o número de núcleos de baixa prioridade por conta do lote pode ser aumentado, mas o número máximo não está especificado. Para discutir as opções de aumento, entre em contato com o suporte do Azure.

<sup>3</sup>trabalhos concluídos e as agendas de trabalho não forem limitadas.

<sup>4</sup>para solicitar um aumento além desse limite, entre em contato com o suporte do Azure.

---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 05/22/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 3f94481e6a8550479788d92c744327e1dc3b58c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66376932"
---
Os seguintes limites se aplicam a tópicos de sistema de grade de eventos do Azure e tópicos personalizados, *não* domínios do evento.

| Resource | Limite |
| --- | --- |
| Tópicos personalizados por assinatura do Microsoft Azure | 100 |
| Assinaturas de Evento por tópico | 500 |
| Publicar a taxa de um tópico personalizado (entrada) | 5\.000 eventos por segundo, por tópico |
| Solicitações de publicação | 250 por segundo |
| Tamanho do evento | Suporte para 64 KB em geral GA (disponibilidade). Suporte para 1 MB está atualmente em visualização. |

Os seguintes limites se aplicam a somente os domínios de evento.

| Resource | Limite |
| --- | --- |
| Tópicos por domínio de evento | 1\.000 durante a visualização pública |
| Assinaturas de evento por tópico dentro de um domínio | 50 durante a visualização pública |
| Assinaturas de evento de escopo de domínio | 50 durante a visualização pública |
| Publicar a taxa para um domínio do evento (entrada) | 5\.000 eventos por segundo durante a visualização pública |
| Solicitações de publicação | 250 por segundo |
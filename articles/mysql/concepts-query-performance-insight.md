---
title: Análise de desempenho de consulta no banco de dados do Azure para MySQL
description: Este artigo descreve o recurso de análise de desempenho de consultas no banco de dados do Azure para MySQL
author: ajlam
ms.author: andrela
ms.service: MySQL
ms.topic: conceptual
ms.date: 06/05/2019
ms.openlocfilehash: 1243ae8ae20d08ea643661606639abedbc56ab9c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078775"
---
# <a name="query-performance-insight-in-azure-database-for-mysql"></a>Análise de desempenho de consulta no banco de dados do Azure para MySQL

**Aplica-se a:**  banco de dados do Azure para MySQL 5.7

> [!NOTE]
> Análise de desempenho de consulta está em visualização. Suporte para análise de desempenho de consultas no portal do Azure está sendo distribuído e não ainda estejam disponível em sua região.

A Análise de Desempenho de Consultas ajuda você a identificar rapidamente quais são suas consultas de execução mais longa, como elas mudam ao longo do tempo e quais esperas as estão afetando.

## <a name="permissions"></a>Permissões

**Proprietário** ou **Colaborador** permissões necessárias para exibir o texto das consultas na análise de desempenho de consulta. ** Leitor** podem exibir gráficos e tabelas, mas não o texto da consulta.

## <a name="prerequisites"></a>Pré-requisitos

Para análise de desempenho de consulta para a função, os dados devem existir na [Store consulta](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Exibição de análises de desempenho

A visualização da [Análise de Desempenho de Consultas](concepts-query-performance-insight.md) no portal do Azure será superficial visualizações em informações do Repositório de Consultas.

Na página do portal do banco de dados do Azure para servidor MySQL, selecione **análise de desempenho de consultas** sob o **desempenho inteligente** seção da barra de menus.

![Consultas de execução longa da Análise de Desempenho de Consultas](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png) 

O **consultas longas** guia mostra as consultas top 5 por duração média por execução, agregados em intervalos de 15 minutos. Você pode exibir mais consultas, selecionando a partir de **número de consultas** lista suspensa. As cores do gráfico pode ser alteradas para uma ID de consulta específica ao fazer isso.

Você pode clicar e arrastar no gráfico para restringi-lo a uma janela de tempo específico. Como alternativa, use o zoom in e out ícones para exibir um período de tempo menor ou maior, respectivamente.

Selecione o **estatísticas de espera** guia para exibir as visualizações correspondentes em espera no servidor.

Consultas exibidas na exibição de estatísticas de espera são agrupadas pelas consultas que apresentam o maior espera durante o intervalo de tempo especificado.

![Análise de desempenho de consulta aguarda estatísticas](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre [monitoramento e ajuste](concepts-monitoring.md) no banco de dados do Azure para MySQL.
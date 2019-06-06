---
title: Visualização de dados de Data Explorer do Azure
description: Saiba mais sobre as diferentes maneiras que você pode visualizar os dados do Data Explorer do Azure
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 85c37b6d626fc9942f5df956e738431d2727d282
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481828"
---
# <a name="data-visualization-with-azure-data-explorer"></a>Visualização de dados com o Data Explorer do Azure 

Gerenciador de dados do Azure é um serviço de exploração de dados rápida e altamente escalonável para dados de telemetria e de log que são usados para criar soluções de análises complexas para grandes quantidades de dados. Gerenciador de dados do Azure integra-se com várias ferramentas de visualização, portanto, você pode visualizar seus dados e compartilhar os resultados na sua organização. Esses dados podem ser transformados em informações acionáveis para causar impacto nos negócios.

Visualização de dados e emissão de relatórios é uma etapa crítica no processo de análise de dados. Gerenciador de dados do Azure dá suporte a vários serviços de BI, você pode usar aquele que melhor se adapta a seu cenário e orçamento.

* Visualizações do Data Explorer do Azure: Usando a linguagem de consulta Kusto a [ `render operator` ](/azure/kusto/query/renderoperator) oferece vários tipos de visualização para representar os resultados da consulta. Visualizações de consulta são úteis na detecção de anomalias e de previsão, aprendizado de máquina e muito mais.

* [Power BI](https://powerbi.microsoft.com): O Gerenciador de dados do Azure fornece a capacidade de conectar-se ao Power BI usando vários métodos: 

  * [Conector interno de nativos do Power BI](/azure/data-explorer/power-bi-connector)

  * [Importar de consulta do Data Explorer do Azure no Power BI](/azure/data-explorer/power-bi-imported-query)
 
  * [Consulta SQL](/azure/data-explorer/power-bi-sql-query).

* [Microsoft Excel](https://products.office.com/excel): Gerenciador de dados do Azure fornece a capacidade de se conectar ao Excel usando o conector interno do Excel nativo ou importar uma consulta de Data Explorer do Azure para o Excel.

* [Grafana](https://grafana.com): Grafana fornece um plug-in do Gerenciador de dados do Azure que permite que você visualize os dados de Data Explorer do Azure. Você [configurar o Data Explorer do Azure como uma fonte de dados para o Grafana e depois visualize os dados](/azure/data-explorer/grafana)

* [Sisense](https://www.sisense.com): O Gerenciador de dados do Azure fornece a capacidade para se conectar ao Sisense usando o conector do JDBC. Você [configurar o Data Explorer do Azure como uma fonte de dados para Sisense e, em seguida, visualizar os dados](/azure/data-explorer/sisense).

* [Tableau](https://www.tableau.com): Gerenciador de dados do Azure fornece a capacidade de se conectar ao Tableau usando o [conector ODBC e visualizar os dados no Tableau](/azure/data-explorer/connect-odbc).

* [Qlik](https://www.qlik.com): Gerenciador de dados do Azure fornece a capacidade de se conectar ao Qlik usando o [conector ODBC](/azure/data-explorer/connect-odbc).
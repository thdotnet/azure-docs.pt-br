---
title: Usar conexão de conectividade de banco de dados aberto (ODBC) para o Data Explorer do Azure para visualizar dados com o Tableau
description: Neste artigo, você aprenderá a usar uma conexão ODBC Open Database Connectivity () para conexão do Gerenciador de dados do Azure para visualizar dados com o Tableau.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: a0948ae35a5c23768df117979db819861ac64529
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499078"
---
# <a name="visualize-data-from-azure-data-explorer-in-tableau"></a>Visualizar dados do Azure Data Explorer no Tableau

 [Tableau](https://www.tableau.com/) é uma plataforma de análise visual para o business intelligence. Para se conectar ao Data Explorer do Azure do Tableau e trazer dados de um cluster de exemplo, use o driver de conectividade de banco de dados aberto (ODBC) do SQL Server. 

## <a name="prerequisites"></a>Pré-requisitos

Você precisará do seguinte para concluir este artigo:

* [Conectar-se ao Gerenciador de dados do Azure com o ODBC](connect-odbc.md) usando o driver ODBC do SQL Server, para se conectar ao Data Explorer do Azure do Tableau. 

* Tableau Desktop, completo, ou [avaliação](https://www.tableau.com/products/desktop/download) versão.

* Um cluster que inclui os dados de exemplo do StormEvents. Para obter mais informações, consulte [criar um cluster do Gerenciador de dados do Azure e o banco de dados](create-cluster-database-portal.md) e [ingestão de dados de exemplo no Data Explorer do Azure](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="visualize-data-in-tableau"></a>Visualizar dados no Tableau 

Depois que você tiver terminado de configurar o ODBC, você pode colocar dados de exemplo em Tableau.

1. No Tableau Desktop, no menu à esquerda, selecione **outros bancos de dados (ODBC)** .

    ![Conecte-se ODBC](media/tableau/connect-odbc.png)

1. Para **DSN**, selecione a fonte de dados que você criou para ODBC e selecione **Sign In**.

    ![Entrada do ODBC](media/tableau/odbc-sign-in.png)

1. Para **banco de dados**, selecione o banco de dados em seu cluster de exemplo, como *TestDatabase*. Para **esquema**, selecione *dbo*e para **tabela**, selecione o *StormEvents* tabela de exemplo.

    ![Selecione o banco de dados e tabela](media/tableau/select-database-table.png)

1. Tableau agora mostra o esquema para os dados de exemplo. Selecione **atualizar agora** para trazer os dados para o Tableau.

    ![Atualizar dados](media/tableau/update-data.png)

    Quando os dados são importados, Tableau mostra linhas de dados semelhante à imagem a seguir.

    ![Conjunto de resultados](media/tableau/result-set.png)

1. Agora você pode criar visualizações no Tableau, com base nos dados trazidos do Gerenciador de dados do Azure. Para obter mais informações, consulte [Tableau Learning](https://www.tableau.com/learn).

## <a name="next-steps"></a>Próximas etapas

* [Gravar consultas para Azure Data Explorer](write-queries.md)
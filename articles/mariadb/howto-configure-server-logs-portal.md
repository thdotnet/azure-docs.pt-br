---
title: Configurar e acessar logs do servidor do banco de dados do Azure para MariaDB no portal do Azure
description: Este artigo descreve como configurar e acessar os logs de servidor no banco de dados do Azure para MariaDB do portal do Azure.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 3dbf7064e409230916668e62ef861c0ce149fdbb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065633"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Configurar e acessar os logs de servidor no Portal do Azure

Você pode configurar, listar e baixar o [banco de dados do Azure para logs de consulta lentos MariaDB](concepts-server-logs.md) do portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de instruções, você precisa:
- [Banco de dados do Azure para MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Configurar o registro em log
Configure o acesso ao log de consultas lentas. 

1. Entre no [Portal do Azure](https://portal.azure.com/).

2. Selecione o servidor do Banco de Dados do Azure para MariaDB.

3. Na seção **Monitoramento** da barra lateral, selecione **Logs de servidor**. 
   ![Selecione os Logs do servidor e clique para configurar](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. Selecione o título **Clique aqui para habilitar os logs e configurar os parâmetros de log** para ver os parâmetros do servidor.

5. Altere os parâmetros que você precisa ajustar, incluindo o "slow_query_log" para "ON". Todas as alterações feitas nessa sessão são realçadas em roxo. 

   Depois que você alterar os parâmetros, clique em **Salvar**. Ou então, você pode **Descartar** suas alterações.

   ![Clique em salvar ou descartar](./media/howto-configure-server-logs-portal/3-save-discard.png)

6. Retorne para a lista de logs clicando no **botão Fechar** (ícone de X) na página **Parâmetros do servidor**.

## <a name="view-list-and-download-logs"></a>Exibir a lista e baixar os logs
Depois que o log começa, você pode exibir uma lista de logs de consulta lentos disponíveis e baixar arquivos de log individuais no painel de Logs do servidor. 

1. Abra o portal do Azure.

2. Selecione o servidor do Banco de Dados do Azure para MariaDB.

3. Na seção **Monitoramento** da barra lateral, selecione **Logs de servidor**. A página exibe uma lista dos seus arquivos de log, como mostrado:

   ![Lista de logs](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > A convenção de nomenclatura do log é **mysql-slow-< nome do seu servidor>-aaaammddhh.log**. A data e hora usados no nome do arquivo é o momento em que o log foi emitido. Os arquivos de log são girados a cada 24 horas ou 7,5 GB, o que ocorrer primeiro.

4. Se necessário, use a **caixa de pesquisa** para restringir rapidamente a um log específico com base na data/hora. A pesquisa busca o nome do log.

5. Baixe arquivos de log individuais usando o botão **download** (ícone de seta para baixo) ao lado de cada arquivo de log na linha de tabela, conforme mostrado:

   ![Clique no ícone download](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre [lento do Logs de consulta](concepts-server-logs.md) no banco de dados do Azure para MariaDB.
- Para obter mais informações sobre as definições de parâmetros e o registro, consulte a documentação do MariaDB em [Logs](https://mariadb.com/kb/en/library/slow-query-log-overview/).

<!--- See [Access Server Logs in CLI](howto-configure-server-logs-in-cli.md) to learn how to download logs programmatically. -->

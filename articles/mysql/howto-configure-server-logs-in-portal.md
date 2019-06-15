---
title: Configurar e acessar logs de consulta lenta do banco de dados do Azure para MySQL no portal do Azure
description: Este artigo descreve como configurar e acessar os logs lento no banco de dados do Azure para MySQL no portal do Azure.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: b16ac525d41eb2423828a647fdb75fd3f4a80a31
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052724"
---
# <a name="configure-and-access-slow-query-logs-in-the-azure-portal"></a>Configurar e logs de consulta lenta de acesso no portal do Azure

Você pode configurar, listar e baixar o [banco de dados do Azure para logs de consulta lenta do MySQL](concepts-server-logs.md) do portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de instruções, você precisa:
- [Servidor do Banco de Dados do Azure para MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Configurar o registro em log
Configure o acesso aos logs de consulta lenta do MySQL. 

1. Entre no [Portal do Azure](https://portal.azure.com/).

2. Selecione seu servidor de Banco de Dados do Azure para MySQL.

3. Na seção **Monitoramento** da barra lateral, selecione **Logs de servidor**. 
   ![Selecione os Logs do servidor e clique para configurar](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Selecione o título **Clique aqui para habilitar os logs e configurar os parâmetros de log** para ver os parâmetros do servidor.

5. Altere os parâmetros que você precisa ajustar. Todas as alterações feitas nessa sessão são realçadas em roxo. 

   Depois que você alterar os parâmetros, clique em **Salvar**. Ou então, você pode **Descartar** suas alterações.

   ![Clique em salvar ou descartar](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

6. Retorne para a lista de logs clicando no **botão Fechar** (ícone de X) na página **Parâmetros do servidor**.

## <a name="view-list-and-download-logs"></a>Exibir a lista e baixar os logs
Depois que o log começa, você pode exibir uma lista de logs de consulta lentos disponíveis e baixar arquivos de log individuais no painel de Logs do servidor.

1. Abra o portal do Azure.

2. Selecione seu servidor de Banco de Dados do Azure para MySQL.

3. Na seção **Monitoramento** da barra lateral, selecione **Logs de servidor**. A página exibe uma lista dos seus arquivos de log, como mostrado:

   ![Lista de logs](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > A convenção de nomenclatura do log é **mysql-slow-< nome do seu servidor>-aaaammddhh.log**. A data e hora usados no nome do arquivo é o momento em que o log foi emitido. Os arquivos de log são girados a cada 24 horas ou 7,5 GB, o que ocorrer primeiro.

4. Se necessário, use a **caixa de pesquisa** para restringir rapidamente a um log específico com base na data/hora. A pesquisa busca o nome do log.

5. Baixe arquivos de log individuais usando o botão **download** (ícone de seta para baixo) ao lado de cada arquivo de log na linha de tabela, conforme mostrado:

   ![Clique no ícone download](./media/howto-configure-server-logs-in-portal/5-download.png)

## <a name="next-steps"></a>Próximas etapas
- Ver [Logs de consulta lenta de acesso na CLI](howto-configure-server-logs-in-cli.md) para saber como baixar logs de consulta lentos programaticamente.
- Saiba mais sobre [lento do logs de consulta](concepts-server-logs.md) no banco de dados do Azure para MySQL.
- Para obter mais informações sobre as definições de parâmetros e o registro em log no MySQL, consulte a documentação do MySQL em [Logs](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).
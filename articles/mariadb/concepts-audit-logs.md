---
title: Logs de auditoria do banco de dados do Azure para MariaDB
description: Descreve os logs de auditoria disponíveis no banco de dados do Azure para MariaDB e os parâmetros disponíveis para habilitar níveis de log.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 765db8461465b74ac068782c1b91d3c68b73f7d4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079516"
---
# <a name="audit-logs-in-azure-database-for-mariadb"></a>Logs de auditoria no banco de dados do Azure para MariaDB

No banco de dados do Azure para MariaDB, o log de auditoria está disponível aos usuários. O log de auditoria pode ser usado para rastrear a atividade de nível de banco de dados e normalmente é usado para fins de conformidade.

> [!IMPORTANT]
> Funcionalidade de log de auditoria está atualmente em visualização.

## <a name="configure-audit-logging"></a>Configurar o log de auditoria

Por padrão, o log de auditoria está desabilitado. Para habilitá-lo, defina `audit_log_enabled` como ON.

Outros parâmetros que você pode ajustar incluem:

- `audit_log_events`: controla os eventos a serem registrados. Consulte a tabela a seguir para eventos de auditoria específicos.
- `audit_log_exclude_users`: MariaDB usuários a serem excluídos do log. Permite no máximo quatro usuários. O comprimento máximo do parâmetro é de 256 caracteres.

| **Evento** | **Descrição** |
|---|---|
| `CONNECTION` | -Iniciação de Conexão (bem-sucedidas ou não) <br> -Reautenticação do usuário com usuário/senha diferente durante a sessão <br> -Encerramento da Conexão |
| `DML_SELECT`| Consultas SELECT |
| `DML_NONSELECT` | INSERT/DELETE/UPDATE queries |
| `DML` | DML = DML_SELECT + DML_NONSELECT |
| `DDL` | Consultas como "DROP DATABASE" |
| `DCL` | Consultas como "CONCEDER permissão de" |
| `ADMIN` | Consultas como "Mostrar STATUS" |
| `GENERAL` | Tudo em DML_SELECT, DML_NONSELECT, DML, DDL, DCL e administração |

## <a name="access-audit-logs"></a>Acesse os logs de auditoria

Os logs de auditoria são integrados com os Logs de diagnóstico do Azure Monitor. Depois de habilitar os logs de auditoria no servidor do MariaDB, você pode emiti-los para o armazenamento do Azure, Hubs de eventos ou logs do Azure Monitor. Para saber mais sobre como habilitar os logs de diagnóstico no portal do Azure, consulte a [artigo portal do log de auditoria](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs).

## <a name="schemas"></a>Esquemas

As seções a seguir descrevem o que é a saída de logs de auditoria do MariaDB com base no tipo de evento. Dependendo do método de saída, os campos incluídos e a ordem em que aparecem podem variar.

### <a name="connection"></a>Conexão

| **Propriedade** | **Descrição** |
|---|---|
| `TenantId` | Sua ID de locatário |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Carimbo de data/hora quando o log foi gravado, em UTC |
| `Type` | Tipo do log. Sempre `AzureDiagnostics` |
| `SubscriptionId` | GUID para a assinatura a que o servidor pertence |
| `ResourceGroup` | Nome do grupo de recursos ao qual o servidor pertence |
| `ResourceProvider` | Nome do provedor de recursos. Sempre `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nome do servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `connection_log` |
| `event_subclass` | `CONNECT`, `DISCONNECT` |
| `connection_id` | ID de conexão exclusiva gerada pelo MariaDB |
| `host` | Em branco |
| `ip` | Endereço IP do cliente conectando-se ao MariaDB |
| `user` | Nome do usuário que executa a consulta |
| `db` | Nome do banco de dados conectado |
| `\_ResourceId` | URI de recurso |

### <a name="general"></a>Geral

Esquema abaixo se aplica a tipos de eventos gerais, DML_SELECT, DML_NONSELECT, DML, DDL, DCL e administrador.

| **Propriedade** | **Descrição** |
|---|---|
| `TenantId` | Sua ID de locatário |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Carimbo de data / hora quando tshe log foi gravado em UTC |
| `Type` | Tipo do log. Sempre `AzureDiagnostics` |
| `SubscriptionId` | GUID para a assinatura a que o servidor pertence |
| `ResourceGroup` | Nome do grupo de recursos ao qual o servidor pertence |
| `ResourceProvider` | Nome do provedor de recursos. Sempre `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nome do servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `general_log` |
| `event_subclass` | `LOG`, `ERROR`, `RESULT` |
| `event_time` | Consulta inicie segundos no carimbo de hora do UNIX |
| `error_code` | Código de erro se a falha na consulta. `0` significa que nenhum erro |
| `thread_id` | ID do thread que executou a consulta |
| `host` | Em branco |
| `ip` | Endereço IP do cliente conectando-se ao MariaDB |
| `user` | Nome do usuário que executa a consulta |
| `sql_text` | Texto da consulta completa |
| `\_ResourceId` | URI de recurso |

### <a name="table-access"></a>Acesso à tabela

| **Propriedade** | **Descrição** |
|---|---|
| `TenantId` | Sua ID de locatário |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Carimbo de data/hora quando o log foi gravado, em UTC |
| `Type` | Tipo do log. Sempre `AzureDiagnostics` |
| `SubscriptionId` | GUID para a assinatura a que o servidor pertence |
| `ResourceGroup` | Nome do grupo de recursos ao qual o servidor pertence |
| `ResourceProvider` | Nome do provedor de recursos. Sempre `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | URI de recurso |
| `Resource` | Nome do servidor |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `table_access_log` |
| `event_subclass` | `READ`, `INSERT`, `UPDATE`, ou `DELETE` |
| `connection_id` | ID de conexão exclusiva gerada pelo MariaDB |
| `db` | Nome do banco de dados acessado |
| `table` | Nome da tabela acessada |
| `sql_text` | Texto da consulta completa |
| `\_ResourceId` | URI de recurso |

## <a name="next-steps"></a>Próximas etapas

- [Como configurar logs de auditoria no portal do Azure](howto-configure-audit-logs-portal.md)
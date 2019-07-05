---
title: Solucionando problemas de erros ao fazer backup de bancos de dados do SAP HANA usando o Backup do Azure | Microsoft Docs
description: Este guia explica como solucionar erros comuns durante a tentativa de backup bancos de dados do SAP HANA usando o Backup do Azure.
services: backup
author: pvrk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: pullabhk
ms.openlocfilehash: 33f1b4674aad35d55ab014c45cd73753533931cb
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514177"
---
# <a name="troubleshoot-back-up-of-sap-hana-server-on-azure"></a>Solucionar problemas de backup do servidor do SAP HANA no Azure

Este artigo fornece informações de solução para proteger bancos de dados do SAP HANA em máquinas virtuais do Azure. Antes de prosseguir para a solução de problemas, vamos entender alguns pontos fundamentais sobre permissões e configurações.

## <a name="understanding-pre-requisites"></a>Pré-requisitos de compreensão

Como parte da [pré-requisitos](backup-azure-sap-hana-database.md#prerequisites), o script de pré-registro deve ser executado na máquina virtual em que o HANA está instalado para configurar as permissões corretas.

### <a name="setting-up-permissions"></a>Configurando permissões

O que faz o script de pré-registro:

1. Cria AZUREWLBACKUPHANAUSER no sistema do HANA e adiciona permissões conforme listado abaixo e as funções necessárias:
    - ADMINISTRADOR de banco de dados - criar novos bancos de dados durante a restauração
    - LEITURA de catálogo para ler o catálogo de backup
    - SAP_INTERNAL_HANA_SUPPORT – para acessar algumas tabelas privadas
2. Adiciona a chave para Hdbuserstore para plug-in do HANA fazer todas as operações (consulta de banco de dados, configurar o backup, fazendo backup, fazendo a restauração)
   
   - Para confirmar a criação da chave, execute o comando HDBSQL dentro da máquina do HANA com as credenciais SIDADM:

    ``` hdbsql
    hdbuserstore list
    ```
    
    A saída do comando deve exibir a chave {SID} {DBNAME} com o usuário como 'AZUREWLBACKUPHANAUSER'.

> [!NOTE]
> Verifique se você tem um conjunto exclusivo de arquivos SSFS no caminho "/ usr/sap/{SID}/home/.hdb/". Deve haver apenas uma pasta sob esse caminho.

### <a name="setting-up-backint-parameters"></a>Configuração de parâmetros BackInt

Depois que um banco de dados for escolhido para backup, o serviço de Backup do Azure configurará backInt parâmetros no nível de banco de dados.

- [catalog_backup_using_backint:true]
- [enable_accumulated_catalog_backup:false]
- [parallel_data_backup_backint_channels:1]
- [log_backup_timeout_s:900)]
- [backint_response_timeout:7200]

> [!NOTE]
> Verifique se esses parâmetros não estão presentes no nível do HOST. Parâmetros de nível de host substituirão esses parâmetros e podem causar um comportamento diferente do que o esperado.

## <a name="understanding-common-user-errors"></a>Noções básicas sobre erros comuns de usuário

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Falha ao conectar ao system.check HANA que seu sistema está em execução.| O serviço de Backup do Azure não é capaz de se conectar ao HANA porque o BD do HANA está inativo. Ou HANA está em execução, mas não permitindo que o serviço de Backup do Azure para se conectar | Verifique se o banco de dados de HANA/serviço está inativo. Se o serviço da DB HANA está em execução, verifique se todas as permissões estão configuradas como mencionado [aqui](#setting-up-permissions). Se a chave estiver ausente, executar novamente o script de pré-registro para criar uma nova chave. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Configuração de Backint inválidos detectados. Interromper a proteção e reconfigure o banco de dados.| Os parâmetros de backInt incorretamente são especificados para Backup do Azure. | Verificar os parâmetros são como mencionado [aqui](#setting-up-backint-parameters). Se backInt com base em parâmetros estiverem presentes no HOST, em seguida, removê-los. Se os parâmetros não estão presentes no HOST, mas foram modificados manualmente em um nível de banco de dados, em seguida, revertê-los com os valores apropriados conforme mencionado acima. Ou 'Parar proteção com reter dados' do portal do Azure e 'retomar backup' novamente.|

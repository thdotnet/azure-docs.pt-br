---
title: Exemplo de Script do Azure PowerShell - Restaurar um aplicativo Web de um backup | Microsoft Docs
description: Exemplo de Script do Azure PowerShell - Restaurar um aplicativo Web de um backup
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.topic: sample
ms.date: 11/21/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 85cbcb4b01161e0004d38f08ae8c30fd398727ff
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087902"
---
# <a name="restore-a-web-app-from-a-backup-using-azure-powershell"></a>Restaurar um aplicativo Web de um backup usando o Azure PowerShell

Esse exemplo de script recupera um backup concluído anteriormente de um aplicativo Web existente e o restaura substituindo seu conteúdo. 

Se necessário, instale o Azure PowerShell usando a instrução encontrada no [guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Connect-AzAccount` para criar uma conexão com o Azure. 

## <a name="sample-script"></a>Script de exemplo

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore/backup-restore.ps1?highlight=1-2 "Restore a web app from a backup")]

## <a name="clean-up-deployment"></a>Limpar a implantação 

Se você não precisar mais do aplicativo Web, use o comando a seguir para remover o grupo de recursos, o aplicativo Web e todos os recursos relacionados.

```powershell
Remove-AzResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os seguintes comandos. Cada comando da tabela é vinculado à documentação específica do comando.

| Comando | Observações |
|---|---|
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Obtém uma lista de backups para um aplicativo Web. |
| [Restore-AzWebAppBackup](/powershell/module/az.websites/restore-azwebappbackup) | Restaura um aplicativo Web de um backup concluído anteriormente. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o módulo do Azure PowerShell, confira a [Documentação do Azure PowerShell](/powershell/azure/overview).

Exemplos adicionais do Azure PowerShell para Aplicativos Web do Serviço de Aplicativo do Azure podem ser encontrados nos [exemplos do Azure PowerShell](../samples-powershell.md).

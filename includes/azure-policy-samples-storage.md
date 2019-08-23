---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/21/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 42e965b188db2b84579ab322fbe19781000dff7e
ms.sourcegitcommit: a3a40ad60b8ecd8dbaf7f756091a419b1fe3208e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69894086"
---
## <a name="storage"></a>Armazenamento

|  |  |
|---------|---------|
| [SKUs permitidas para Contas de Armazenamento e Máquinas Virtuais](../articles/governance/policy/samples/allowed-skus-storage.md) | Exige que contas de armazenamento e máquinas virtuais usem SKUs aprovadas. Usa as políticas internas para garantir SKUs aprovadas. Especifique uma matriz de SKUs de máquinas virtuais aprovadas e uma matriz de SKUs de conta de armazenamento aprovadas. |
| [SKUs de conta de armazenamento permitidas](../articles/governance/policy/samples/allowed-storage-account-skus.md) | Exige que contas de armazenamento usem uma SKU aprovada. Especifique uma matriz de SKUs aprovados. |
| [Negar camadas de acesso esporádico para contas de armazenamento](../articles/governance/policy/samples/deny-cool-access-tiering.md) | Proíbe o uso de camadas de acesso esporádico para contas de armazenamento de blobs.  |
| [Assegurar tráfego https somente para contas de armazenamento](../articles/governance/policy/samples/ensure-https-storage-account.md) | Exige que contas de armazenamento usem tráfego HTTPS.  |
| [Assegurar a criptografia de arquivo de armazenamento](../articles/governance/policy/samples/ensure-storage-file-encryption.md) | Exige que a criptografia de arquivo esteja habilitada para contas de armazenamento.  |
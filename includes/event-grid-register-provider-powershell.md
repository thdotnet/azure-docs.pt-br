---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 68a208af1a9aa9e73f2af99021d195f264fb21f1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155993"
---
## <a name="enable-event-grid-resource-provider"></a>Habilitar provedor de recursos da Grade de Eventos

Se você ainda não tiver usado a Grade de Eventos em sua assinatura do Azure, talvez seja necessário registrar o provedor de recursos da Grade de Eventos. Execute o comando a seguir:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

A conclusão do registro pode demorar um pouco. Para verificar o status, execute:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Quando `RegistrationStatus` for `Registered`, você está pronto para continuar.

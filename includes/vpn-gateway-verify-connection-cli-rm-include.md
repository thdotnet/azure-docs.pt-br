---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0e009354e66ab13cdb9fbc3cf9e4b37e904bdfd1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121174"
---
Você pode verificar se a conexão foi bem-sucedida usando o comando [az network vpn-connection show](/cli/azure/network/vpn-connection). No exemplo, "--name" refere-se ao nome da conexão que você deseja testar. Quando a conexão ainda está sendo estabelecida, seu status de conexão mostra "Conectando". Quando a conexão for estabelecida, o status será alterado para "Conectado".

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```

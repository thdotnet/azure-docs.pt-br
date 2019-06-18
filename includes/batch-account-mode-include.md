---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: b5bb5da2b68bae36353b81a04993814b395ce67b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66127558"
---
> [!NOTE]
> Ao criar uma conta do Lote, você pode escolher entre dois modos de *alocação do pool*: **assinatura de usuário** e **serviço do Lote**. Na maioria dos casos, você deve usar o modo de serviço de Lote padrão, no qual os pools são alocados em segundo plano nas assinaturas gerenciadas do Azure. No modo de assinatura alternativo do usuário, as VMs do Lote e outros recursos são criados diretamente em sua assinatura, quando um pool é criado. Modo de assinatura de usuário é necessário se você quiser criar pools do Lote usando [Instâncias de VM Reservadas do Azure](https://azure.microsoft.com/pricing/reserved-vm-instances/). Para criar uma conta do Lote no modo de assinatura do usuário, você também deverá registrar sua assinatura com o Lote do Azure e associar a conta com um Azure Key Vault.
---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: backup
author: rayne-wiselman
ms.service: backup
ms.topic: include
ms.date: 12/07/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 9e9c09c1825f5c8383a708e8bd343146396f878e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66238204"
---
Os limites a seguir se aplicam ao Backup do Azure.

| **Limite** | **Padrão** |
| --- | --- |
| Servidores ou computadores que podem ser registrados em um cofre. | Windows Server/Windows Client/System Center Data Protection Manager: 50. <br/><br/> VMs de IaaS: 1,000.  |
| Tamanho da fonte de dados no armazenamento do cofre. |Máximo de 54,400 GB. O limite não se aplica ao backup de VM IaaS. |
| Cofres de backup em uma assinatura do Azure. |500 cofres por região. |
| Agende backups diários. |Windows Server/Client: Três por dia.<br/> System Center DPM: Dois por dia. <br/> VMs de IaaS: Uma vez por dia.  |
| Discos de dados anexados a uma VM do Azure para backup. | 16 |
| Anexado à VM do Azure para backup de discos de dados individuais.| 4\.095 GB|

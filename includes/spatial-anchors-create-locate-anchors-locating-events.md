---
ms.openlocfilehash: f478956c3a220ea4435c4f6e8b1096a1e9fdcadf
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70014684"
---
Depois que o observador for criado, o evento `AnchorLocated` será acionado para cada âncora solicitada. Esse evento é acionado quando uma âncora é localizada ou se a âncora não puder ser localizada. Se essa situação ocorrer, o motivo será indicado no status. Após todas as âncoras de um observador serem processadas, sejam elas encontradas ou não, o evento `LocateAnchorsCompleted` será acionado. Há um limite de 35 identificadores por observador. 

---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66110764"
---
## <a name="update-properties"></a>Atualizar propriedades

Para atualizar as propriedades em uma âncora, você deve usar o `UpdateAnchorProperties()` método. Se dois ou mais dispositivos tentarem atualizar as propriedades para a mesma âncora ao mesmo tempo, usamos um modelo de simultaneidade otimista. O que significa que a primeira gravação será a vencedora.  Todas as outras gravações obterá um erro de "Simultaneidade": uma atualização das propriedades seria necessários antes de tentar novamente.

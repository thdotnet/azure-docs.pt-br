---
ms.openlocfilehash: 7219a457a2631f9ff6beee06eff34bce0ff5a23f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66110693"
---
Como com tokens de acesso, se um token do AD do Azure não for definido, você deve tratar o evento TokenRequired ou implementar o método tokenRequired no protocolo delegado.

Você pode manipular o evento de forma síncrona, definindo a propriedade nos argumentos de evento.

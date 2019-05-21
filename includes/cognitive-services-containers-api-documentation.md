---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2019
ms.openlocfilehash: f4925401235aedb341a7e29ca36b079126647f7b
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65885029"
---
## <a name="validate-that-a-container-is-running"></a>Validar se um contêiner está em execução 

Há várias maneiras de validar se um contêiner está em execução. 

|Solicitação|Finalidade|
|--|--|
|`http://localhost:5000/`|O contêiner oferece uma home page.|
|`http://localhost:5000/status`|Solicitado com GET, para validar se o contêiner está em execução sem gerar uma consulta de ponto de extremidade. Essa solicitação pode ser usada para [testes de preparação e de execução](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) do Kubernetes.|
|`http://localhost:5000/swagger`|O contêiner fornece um conjunto completo de documentação para os pontos de extremidades e um recurso `Try it now`. Com esse recurso, é possível inserir suas configurações em um formulário HTML baseado na Web e realizar a consulta sem precisar escrever nenhum código. Após a consulta ser retornada, um exemplo de comando CURL será fornecido para demonstrar o formato do corpo e dos cabeçalhos HTTP exigidos. |

![Home page do contêiner](./media/cognitive-services-containers-api-documentation/container-webpage.png)

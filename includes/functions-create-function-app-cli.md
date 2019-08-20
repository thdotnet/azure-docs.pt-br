---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 08/05/2019
ms.author: glenga
ms.openlocfilehash: c90587ebd7b0a608e41f19ab600f82dc484bad0a
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949998"
---
## <a name="create-the-local-function-app-project"></a>Criar o projeto de aplicativo de funções local

Execute o comando a seguir na linha de comando para criar um projeto de aplicativo de funções na pasta `MyFunctionProj` do diretório local atual. Um repositório GitHub também é criado em `MyFunctionProj`.

```command
func init MyFunctionProj
```

Quando solicitado, selecione um tempo de execução de trabalho entre as seguintes opções de linguagem:

+ `dotnet`: cria um projeto de biblioteca de classes do .NET (.csproj).
+ `node`: cria um projeto baseado em Node.js. Escolha `javascript` ou `typescript`. 
+ `python`: para um projeto Python, conclua [Criar uma função disparada por HTTP n Azure](../articles/azure-functions/functions-create-first-function-python.md).
+ `powershell`: para um projeto PowerShell, conclua [Criar sua primeira função do PowerShell no Azure](../articles/azure-functions/functions-create-first-function-powershell.md). 


Depois que o projeto é criado, use o seguinte comando para navegar até a nova pasta do projeto `MyFunctionProj`.

```command
cd MyFunctionProj
```

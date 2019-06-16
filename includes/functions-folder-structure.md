---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9f74365f3fe935be45fa9c45e5b12c45b97b2f8a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068425"
---
O código para todas as funções em um aplicativo de funções específico está localizado em uma pasta do projeto raiz que contém um arquivo de configuração do host e uma ou mais subpastas. Cada subpasta contém o código para uma função separada, como na seguinte representação:

```
FunctionApp
 | - host.json
 | - Myfirstfunction
 | | - function.json
 | | - ...  
 | - mysecondfunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

Na versão 2.x do tempo de execução de funções, todas as funções no aplicativo de funções devem compartilhar a mesma pilha de linguagem.  

O arquivo [host.json](../articles/azure-functions/functions-host-json.md), que contém algumas configurações específicas de tempo de execução, está na pasta raiz do aplicativo de funções. Uma pasta `bin` contém pacotes e outros arquivos de biblioteca exigidos pelo aplicativo de funções. Consulte os requisitos específicos a um idioma para um projeto de aplicativo de função:

* [Biblioteca de classes C# (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [Script do C# (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [Script em F#](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)




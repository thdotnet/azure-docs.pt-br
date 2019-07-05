---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 05/25/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d62da82b4a4dd35532dd8776a9111689db469201
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448386"
---
## <a name="register-extensions"></a>Extensões de registro

Com exceção dos gatilhos HTTP e o timer, associações de funções na versão de tempo de execução 2.x são implementados como pacotes de extensão. Na versão 2.x do tempo de execução do Azure Functions, você deve registrar explicitamente as extensões para os tipos de associação usados em suas funções. As exceções a isso são as associações HTTP e gatilhos de temporizador, que não exigem extensões.

Você pode optar por instalar as extensões de associação individualmente, ou você pode adicionar uma referência de pacote de extensão para o arquivo de projeto do host. JSON. Pacotes de extensão remove a possibilidade de ter problemas de compatibilidade de pacote ao usar vários tipos de associação. É a abordagem recomendada para registrar as extensões de associação. Pacotes de extensão também elimina a necessidade de instalar o .NET Core 2.x do SDK. 

### <a name="extension-bundles"></a>pacotes de extensão

[!INCLUDE [Register extensions](functions-extension-bundles.md)]

Para obter mais informações, consulte [as extensões de associação de funções do Azure registrar](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Você deve adicionar pacotes de extensão para o host. JSON antes de adicionar associações para o arquivo do Functions.

### <a name="register-individual-extensions"></a>Registrar extensões individuais

Se você precisar instalar extensões que não estão em um pacote, você poderá registrar manualmente os pacotes de extensão individuais para associações específicas. 

> [!NOTE]
> Para registrar manualmente extensões usando `func extensions install`, você deve ter o .NET Core 2.x SDK instalado.

Depois de atualizar o arquivo *function.json* para incluir todas as associações que a função precisa, execute o comando a seguir na pasta do projeto.

```bash
func extensions install
```

O comando lê o arquivo *function.json* para saber quais são os pacotes necessários, instala-os e recria o projeto de extensão. Ele adiciona as novas associações na versão atual, mas não atualiza as associações existentes. Use a opção `--force` para atualizar as associações existentes para a versão mais recente nas novas instalações.
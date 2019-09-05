---
title: Arquivo de inclusão
description: Arquivo de inclusão
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 05/31/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d47caaf9dbca14364771e0d8ba1e7f6ce16ef2d6
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70174578"
---
## <a name="run-the-function-locally"></a>Executar a função localmente

As Ferramentas Principais do Azure Functions permitem executar um projeto do Azure Functions no seu computador de desenvolvimento local.

1. Para testar sua função, defina um ponto de interrupção no código da função e pressione F5 para iniciar o projeto de aplicativo de funções. A saída do Core Tools é exibida no painel **Terminal**.

1. No painel **Terminal**, copie o ponto de extremidade de URL da sua função disparada por HTTP. 

    ![Saída local do Azure](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Cole a URL para a solicitação HTTP na barra de endereços do navegador. Acrescente o valor de cadeia de consulta `?name=<yourname>` a essa URL e execute a solicitação. A execução é pausada quando o ponto de interrupção é atingido.

1. Quando você continua a execução, o exemplo a seguir mostra a resposta no navegador à solicitação GET:

    ![Resposta da função localhost no navegador](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. Para interromper a depuração, pressione Shift + F5.
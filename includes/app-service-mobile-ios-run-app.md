---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: a5bde1a56bf6a1f5fca4b775c7c8e9bb7477eb6b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66240260"
---
1. Abra o projeto de cliente baixados usando o Xcode.

2. Vá para o [portal do Azure](https://portal.azure.com/) e navegue até o aplicativo móvel que você criou. Sobre o `Overview` folha, procure a URL que é o ponto de extremidade público para seu aplicativo móvel. Exemplo – o nome do site para o meu nome de aplicativo "test123" será https://test123.azurewebsites.net.

3. Para o projeto Swift, abra o arquivo `ToDoTableViewController.swift` nesta pasta - ZUMOAPPNAME/ZUMOAPPNAME/ToDoTableViewController.swift. O nome do aplicativo é `ZUMOAPPNAME`.

4. Na `viewDidLoad()` método, substitua `ZUMOAPPURL` parâmetro com o ponto de extremidade público acima.

    `let client = MSClient(applicationURLString: "ZUMOAPPURL")`

    torna-se
    
    `let client = MSClient(applicationURLString: "https://test123.azurewebsites.net")`
    
5. Para o projeto de Objective-C, abra o arquivo `QSTodoService.m` nesta pasta - ZUMOAPPNAME/ZUMOAPPNAME. O nome do aplicativo é `ZUMOAPPNAME`.

6. Na `init` método, substitua `ZUMOAPPURL` parâmetro com o ponto de extremidade público acima.

    `self.client = [MSClient clientWithApplicationURLString:@"ZUMOAPPURL"];`

    torna-se
    
    `self.client = [MSClient clientWithApplicationURLString:@"https://test123.azurewebsites.net"];`

7. Pressione o botão **Executar** para criar o projeto e iniciar o aplicativo no simulador de iOS.

8. No aplicativo, clique no ícone de adição ( **+** ), digite um texto significativo, como *Concluir o tutorial* e, em seguida, clique no botão Salvar. Isso envia uma solicitação POST para o back-end do Azure implantado anteriormente. O back-end insere dados da solicitação na tabela SQL TodoItem e retorna informações sobre os itens armazenados recentemente no aplicativo móvel. O aplicativo móvel exibe esses dados na lista.

   ![Aplicativo de início rápido em execução no iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure portal]: https://portal.azure.com/

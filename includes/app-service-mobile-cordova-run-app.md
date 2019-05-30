---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: 8d7731480b6239c572d39f52b6a0217d2ac48d25
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240251"
---
1. Navegue até o arquivo de solução no projeto do cliente (. sln) e abri-lo usando o Visual Studio.

2. No Visual Studio, escolha a plataforma de solução (Android, iOS ou Windows) no menu suspenso ao lado da seta de início. Selecione um emulador ou dispositivo de implantação específico clicando na lista suspensa na seta verde. Você pode usar a plataforma Android padrão e o emulador Ripple. Tutoriais mais avançados (por exemplo, notificações por push) exigirão que você selecione um emulador ou dispositivo com suporte.

3. Abra o arquivo `ToDoActivity.java` nesta pasta - ZUMOAPPNAME/aplicativo/src/main/java/com/exemplo/zumoappname. O nome do aplicativo é `ZUMOAPPNAME`.

4. Vá para o [portal do Azure](https://portal.azure.com/) e navegue até o aplicativo móvel que você criou. Sobre o `Overview` folha, procure a URL que é o ponto de extremidade público para seu aplicativo móvel. Exemplo – o nome do site para o meu nome de aplicativo "test123" será https://test123.azurewebsites.net.

5. Vá para o `index.js` arquivo no ZUMOAPPNAME/www/js/index.js e no `onDeviceReady()` método, substitua `ZUMOAPPURL` parâmetro com o ponto de extremidade público acima.

    `client = new WindowsAzure.MobileServiceClient('ZUMOAPPURL');`
    
    torna-se
    
    `client = new WindowsAzure.MobileServiceClient('https://test123.azurewebsites.net');`
    
6. Pressione F5 ou clique na seta verde para compilar e executar o aplicativo Cordova. Se for mostrada uma caixa de diálogo de segurança no emulador que solicita acesso à rede, aceite-a.

7. Depois que o aplicativo for iniciado no dispositivo ou emulador, digite um texto significativo em **Inserir novo texto**, como *concluir o tutorial* e, em seguida, clique no **Add** botão.

Os back-end insere dados da solicitação na tabela TodoItem no Banco de Dados SQL e retorna informações sobre os itens armazenados recentemente no aplicativo móvel. O aplicativo móvel exibe esses dados na lista.

Você pode repetir as etapas 3 a 5 para outras plataformas.
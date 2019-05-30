---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: 63c54f8af91b6b4a76ba49d5e6fc7b3cda9f5b98
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240333"
---
1. Abra o projeto usando o **Android Studio**, usando **Importar projeto (Eclipse ADT, Gradle, etc.)** . Certifique-se de fazer essa seleção de importação para evitar erros de JDK.

2. Abra o arquivo `ToDoActivity.java` nesta pasta - ZUMOAPPNAME/aplicativo/src/main/java/com/exemplo/zumoappname. O nome do aplicativo é `ZUMOAPPNAME`.

3. Vá para o [portal do Azure](https://portal.azure.com/) e navegue até o aplicativo móvel que você criou. Sobre o `Overview` folha, procure a URL que é o ponto de extremidade público para seu aplicativo móvel. Exemplo – o nome do site para o meu nome de aplicativo "test123" será https://test123.azurewebsites.net.

4. Na `onCreate()` método, substitua `ZUMOAPPURL` parâmetro com o ponto de extremidade público acima.
    
    `new MobileServiceClient("ZUMOAPPURL", this).withFilter(new ProgressFilter());` 
    
    torna-se
    
    `new MobileServiceClient("https://test123.azurewebsites.net", this).withFilter(new ProgressFilter());`
    
5. Pressione o botão **Executar “aplicativo”** para compilar o projeto e iniciar o aplicativo no simulador de Android.

4. No aplicativo, digite um texto significativo, como *Concluir o tutorial* e depois clique no botão Adicionar. Isso envia uma solicitação POST para o back-end do Azure implantado anteriormente. O back-end insere dados da solicitação na tabela SQL TodoItem e retorna informações sobre os itens armazenados recentemente no aplicativo móvel. O aplicativo móvel exibe esses dados na lista.
    ![Início rápido do Android](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)
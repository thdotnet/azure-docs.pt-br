---
title: Introdução aos Aplicativos Móveis do Serviço de Aplicativo do Azure para aplicativos Xamarin.iOS | Microsoft Docs
description: Siga este tutorial para começar a usar os Aplicativos Móveis para desenvolvimento do Xamarin.iOS.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: 559050cbc575fce5bdb5b32ec266e1cc3d09b2d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66242720"
---
# <a name="create-a-xamarinios-app"></a>Criar um aplicativo Xamarin.iOS
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Visão geral
Este tutorial mostra como adicionar um serviço de back-end baseado em nuvem a um aplicativo móvel Xamarin.iOS usando um back-end de Aplicativo Móvel do Azure.  Você cria um novo back-end do aplicativo móvel e um aplicativo Xamarin.iOS *Todo list* simples que armazena dados de aplicativo no Azure.

Concluir este tutorial é um pré-requisito para todos os outros tutoriais do Xamarin.iOS sobre como usar o recurso de Aplicativos Móveis no Serviço de Aplicativo do Azure.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, você precisará dos seguintes pré-requisitos:

* Uma conta ativa do Azure. Caso você não tenha uma conta, inscreva-se para uma avaliação do Azure e obtenha até 10 aplicativos móveis gratuitos que você pode continuar a usar mesmo após o fim do seu período de avaliação. Para obter detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).
* O Visual Studio para Mac. Consulte [configuração e instalação do Visual Studio para Mac](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019)
* Um Mac com Xcode 9.0 ou posterior.
  
## <a name="create-an-azure-mobile-app-backend"></a>Criar um back-end de aplicativo móvel do Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Criar uma conexão de banco de dados e configurar o projeto de cliente e servidor
[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinios-app"></a>Execute o aplicativo xamarin. IOS
1. Abra o projeto xamarin. IOS.

2. Vá para o [portal do Azure](https://portal.azure.com/) e navegue até o aplicativo móvel que você criou. Sobre o `Overview` folha, procure a URL que é o ponto de extremidade público para seu aplicativo móvel. Exemplo – o nome do site para o meu nome de aplicativo "test123" será https://test123.azurewebsites.net.

3. Abra o arquivo `QSTodoService.cs` nesta pasta - xamarin.iOS/ZUMOAPPNAME. O nome do aplicativo é `ZUMOAPPNAME`.

4. Na `QSTodoService` classe, substitua `ZUMOAPPURL` variável com o ponto de extremidade público acima.

    `const string applicationURL = @"ZUMOAPPURL";`

    torna-se
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Pressione a tecla F5 para implantar e executar o aplicativo no emulador do iPhone.

6. No aplicativo, digite um texto significativo, como *concluir o tutorial* e, em seguida, clique o botão +.

    ![][10]

    Os dados da solicitação são inseridos na tabela TodoItem. Itens armazenados na tabela são retornados pelo back-end do aplicativo móvel e os dados aparecem na lista.

   > [!NOTE]
   > Você pode examinar o código que acessa o back-end do aplicativo móvel para consultar e inserir dados que estão localizados no arquivo ToDoActivity.cs C#.
   
<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png
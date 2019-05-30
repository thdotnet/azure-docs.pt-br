---
title: 'Tutorial: Criar um aplicativo Web de página única do Azure Time Series Insights | Microsoft Docs'
description: Saiba como criar um aplicativo Web de página única que consulta e renderiza dados de um ambiente do Azure Time Series Insights.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/25/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 5ee6e9aefb235feb28468798c3bd6b107f8c7c49
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244033"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Tutorial: Criar um aplicativo Web de página única do Azure Time Series Insights

Este tutorial orienta você pelo processo de criação de seu próprio SPA (aplicativo Web de página única) para acessar dados do Azure Time Series Insights. 

Neste tutorial, você aprenderá:

> [!div class="checklist"]
> * O design do aplicativo
> * Como registrar o aplicativo no Azure AD (Azure Active Directory)
> * Como criar, publicar e testar o aplicativo Web

> [!NOTE]
> * O código-fonte completo deste tutorial está disponível no [GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * O [aplicativo de exemplo do cliente](https://insights.timeseries.azure.com/clientsample) do Time Series Insights está hospedado para mostrar o aplicativo concluído usado neste tutorial.

## <a name="prerequisites"></a>Pré-requisitos

* Inscreva-se em uma [assinatura do Azure gratuita](https://azure.microsoft.com/free/) se já não tiver uma.

* Uma cópia gratuita do Visual Studio. Baixe as [versões Community 2017 ou 2019](https://www.visualstudio.com/downloads/) para começar.

* Os componentes das principais ferramentas do IIS Express, da Implantação da Web e dos Serviços de Nuvem do Azure para o Visual Studio. Adicione os componentes modificando sua instalação do Visual Studio.

## <a name="application-design"></a>Design do aplicativo

O SPA de exemplo do Time Series Insights é a base para o design e o código usados neste tutorial. O código usa a biblioteca de clientes JavaScript do Time Series Insights. A biblioteca de clientes do Time Series Insights fornece uma abstração para duas categorias principais de API:

- **Métodos de wrapper para chamar as APIs de consulta do Time Series Insights**: As APIs REST que você pode usar para consultar dados do Time Series Insights utilizando expressões baseadas em JSON. Os métodos estão organizados no namespace TsiClient.server da biblioteca.

- **Métodos para criar e popular os vários tipos de controles de criação de gráficos**: Métodos que você pode usar para visualizar dados do Time Series Insights em uma página da Web. Os métodos estão organizados no namespace TsiClient.ux da biblioteca.

Este tutorial também usa os dados do ambiente do Time Series Insights do aplicativo de exemplo. Para saber detalhes sobre a estrutura do aplicativo de exemplo e como ele usa a biblioteca de clientes do Time Series Insights, confira o tutorial [Explorar a biblioteca de clientes JavaScript do Azure Time Series Insights](tutorial-explore-js-client-lib.md).

## <a name="register-the-application-with-azure-ad"></a>Registrar o aplicativo no Azure AD

Antes de compilar o aplicativo, você precisará registrá-lo no Azure AD. O registro fornece a configuração de identidade, permitindo que o aplicativo use o suporte do OAuth para logon único. O OAuth requer que os SPAs usem o tipo de concessão de autorização implícita. Você atualiza a autorização no manifesto do aplicativo. Um manifesto do aplicativo é uma representação JSON da configuração de identidade do aplicativo.

1. Entre no [portal do Azure](https://portal.azure.com) usando sua conta da assinatura do Azure.  
1. Selecione **Azure Active Directory** > **Registros do aplicativo** > **Novo registro do aplicativo**.

   [![Portal do Azure – Inicie o registro de aplicativo do Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration.png#lightbox)

1. No painel **Criar**, preencha os parâmetros necessários.

   Parâmetro|DESCRIÇÃO
   ---|---
   **Nome** | Forneça um nome de registro relevante.  
   **Tipo de Aplicativo** | Deixe como **Aplicativo Web/API**.
   **URL de logon** | Insira a URL para a página de entrada (Página Inicial) do aplicativo. Como o aplicativo estará hospedado no Serviço de Aplicativo do Azure posteriormente, você deverá usar a URL dentro do domínio https:\//azurewebsites.net. Neste exemplo, o nome é baseado no nome do registro.

   Clique em **Criar** para criar o novo registro de aplicativo.

   [![Portal do Azure – A opção Criar no painel de registro de aplicativo do Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-create.png#lightbox)

1. Os aplicativos de recurso fornecem as APIs REST usadas por outros aplicativos. As APIs também estão registradas no Azure AD. As APIs fornecem acesso granular, protegido para aplicativos cliente pela exposição de *escopos*. Como o aplicativo chama a API do Azure Time Series Insights, você deve especificar o escopo e a API. A permissão é concedida para a API e o escopo em tempo de execução. Selecione **Configurações** > **Permissões necessárias** > **Adicionar**.

   [![Portal do Azure – A opção Adicionar para adicionar permissões ao Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms.png#lightbox)

1. No painel **Adicionar acesso à API**, selecione **1 Selecionar uma API** para especificar a API do Azure Time Series Insights. No painel **Selecionar uma API** digite **azure time** no campo de pesquisa. Em seguida, selecione **Azure Time Series Insights** na lista de resultados. Escolha **Selecionar**.

   [![Portal do Azure – A opção Pesquisar para adicionar permissões ao Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api.png#lightbox)

1. Para selecionar um escopo para a API, no painel **Adicionar acesso à API**, selecione **2 Selecionar permissões**. No painel **Habilitar Acesso**, selecione o escopo **Serviço Azure Time Series Insights**. Escolha **Selecionar**. Você será retornado ao painel **Adicionar acesso à API**. Selecione **Concluído**.

   [![Portal do Azure – Definir um escopo para adicionar permissões ao Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-add-perms-api-scopes.png#lightbox)

1. No painel **Permissões necessárias**, já será possível ver a API do Azure Time Series Insights. Você também precisa dar consentimento prévio para o aplicativo poder acessar a API e o escopo para todos os usuários. Selecione **Conceder permissões** e, em seguida, selecione **Sim**.

   [![Portal do Azure – A opção de Conceder permissões para adicionar as permissões necessárias ao Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-required-permissions-consent.png#lightbox)

1. Conforme discutido anteriormente, você também deve atualizar o manifesto do aplicativo. No menu horizontal na parte superior do painel (a "trilha"), selecione o nome do aplicativo para retornar ao painel **Aplicativo registrado**. Selecione **Manifesto**, altere a propriedade `oauth2AllowImplicitFlow` para `true` e clique em **Salvar**.

   [![Portal do Azure – Atualize o manifesto do Azure AD](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-update-manifest.png#lightbox)

1. Na trilha, selecione o nome do aplicativo para retornar ao painel **Aplicativo registrado**. Copie os valores de **Home page** e **ID do aplicativo** para seu aplicativo. Você usará essas propriedades mais tarde no tutorial.

   [![Portal do Azure – Copie a URL da Home Page e os valores de ID do Aplicativo para o aplicativo](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png)](media/tutorial-create-tsi-sample-spa/ap-aad-app-registration-application.png#lightbox)

## <a name="build-and-publish-the-web-application"></a>Compilar e publicar o aplicativo Web

1. Crie um diretório para armazenar os arquivos do projeto de aplicativo. Em seguida, acesse cada uma das URLs a seguir. Clique com o botão direito do mouse no link **Bruto** no canto superior direito da página e selecione **Salvar como** para salvar os arquivos no diretório do projeto.

   - [*index.html*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html): HTML e JavaScript para a página
   - [*sampleStyles.css*]( https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css): Folha de estilos CSS

   > [!NOTE]
   > Dependendo do navegador, você precisará alterar as extensões de arquivo para .html ou .css antes de salvá-lo.

1. Verifique se que os componentes necessários estão instalados no Visual Studio. Os componentes das principais ferramentas do IIS Express, da Implantação da Web e dos Serviços de Nuvem do Azure precisam ser instalados.

    [![Visual Studio – Modificar os componentes instalados](media/tutorial-create-tsi-sample-spa/vs-installation.png)](media/tutorial-create-tsi-sample-spa/vs-installation.png#lightbox)

    > [!NOTE]
    > Sua experiência do Visual Studio pode variar um pouco dos exemplos descritos dependendo de suas definições de configuração e versão.

1. Abra o Visual Studio e entre. Para criar um projeto para o aplicativo Web, no menu **Arquivo**, selecione **Abrir** > **Site da Web**.

    [![Visual Studio – Criar uma nova solução](media/tutorial-create-tsi-sample-spa/vs-solution-create.png)](media/tutorial-create-tsi-sample-spa/vs-solution-create.png#lightbox)

1. No painel **Abrir Site da Web**, selecione o diretório de trabalho em que você armazenou os arquivos HTML e CSS e clique em **Abrir**.

   [![Visual Studio – O menu Arquivo, com as opções de Abrir e Site da Web](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png#lightbox)

1. No menu **Exibição** do Visual Studio, selecione **Gerenciador de Soluções**. A nova solução é aberta. Ela contém um projeto de site (ícone de globo), que contém os arquivos HTML e CSS.

   [![Visual Studio – A nova solução no Gerenciador de Soluções](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png#lightbox)

1. Antes de publicar o aplicativo, altere as definições de configuração em *index. HTML*.

   1. Remova a marca de comentário das três linhas abaixo do comentário `"PROD RESOURCE LINKS"` para alternar as dependências de DESENVOLVIMENTO para PRODUÇÃO. Comente as três linhas abaixo do comentário `"DEV RESOURCE LINKS"`.

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      As dependências devem ser comentadas como no exemplo a seguir:

      ```HTML
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css">

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="../../dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="../../dist/tsiclient.css"> -->
      ```

   1. Para configurar o aplicativo para usar a ID de registro do aplicativo do Azure AD, altere os valores `clientID` e `postLogoutRedirectUri` para usar os valores da **ID do aplicativo** e da **Home Page** que você copiou na etapa 9 em [Registrar o aplicativo no Azure AD](#register-the-application-with-azure-ad).

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Por exemplo: 

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ```

   1. Quando você terminar de fazer modificações, salve *index.html*.

1. Publique o aplicativo Web em sua assinatura do Azure como um Serviço de Aplicativo do Azure.  

   > [!NOTE]
   > Várias opções nas capturas de tela que são mostradas nas etapas a seguir são preenchidas automaticamente com os dados de sua assinatura do Azure. Pode levar alguns segundos para cada painel carregar completamente.  

   1. No Gerenciador de Soluções, clique com o botão direito do mouse no nó do projeto de site da Web e selecione **Publicar Aplicativo Web**.  

      [![Visual Studio – Selecionar a opção Publicar Aplicativo Web no Gerenciador de Soluções](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png#lightbox)

   1. Selecione **Iniciar** para começar a publicar o aplicativo.

      [![Visual Studio – o painel Perfil de publicação](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png#lightbox)

   1. Selecione a assinatura que você quer usar para publicar o aplicativo. Selecione o projeto **TsiSpaApp**. Depois, selecione **OK**.

      [![Visual Studio – o painel do Serviço de Aplicativo Perfil de publicação](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png#lightbox)

   1. Clique em **Publicar** para implantar o aplicativo Web.

      [![Visual Studio – A opção Publicar e a saída de log de publicação](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png#lightbox)

   1. Você deve ver um log de publicação bem-sucedido no painel **Saída** do Visual Studio. Após a implantação, o Visual Studio abrirá o aplicativo Web em uma guia do navegador e solicitará a entrada. Depois de entrar, os controles do Time Series Insights serão preenchidos com os dados.

## <a name="troubleshoot"></a>Solucionar problemas  

Condição/código de erro | DESCRIÇÃO
---------------------| -----------
*AADSTS50011: Nenhum endereço de resposta está registrado para o aplicativo.* | O registro do Azure AD não tem a propriedade **URL de resposta**. Acesse **Configurações** > **URLs de Resposta** do registro de aplicativo do Azure AD. Verifique se a URL de **Logon** especificada na etapa 3 de [Registrar o aplicativo no Azure AD](#register-the-application-with-azure-ad) está presente.
*AADSTS50011: A URL de resposta especificada na solicitação não corresponde às URLs de resposta configuradas para o aplicativo: '\<GUID da ID de Aplicativo>'.* | O `postLogoutRedirectUri` especificado na etapa 6 de [Compilar e publicar o aplicativo Web](#build-and-publish-the-web-application) deve corresponder ao valor especificado em **Configurações** > **URLs de Resposta** no registro de aplicativo do Azure AD. Também não se esqueça de alterar o valor da **URL de destino** para usar *https* de acordo com a etapa 5 em [Compilar e publicar o aplicativo Web](#build-and-publish-the-web-application).
O aplicativo Web é carregado, mas tem uma página de entrada somente texto e sem estilo, com uma tela de fundo branca. | Verifique se os caminhos discutidos na etapa 4 de [Compilar e publicar o aplicativo Web](#build-and-publish-the-web-application) estão corretos. Se o aplicativo Web não puder localizar os arquivos .css, a página não será estilizada corretamente.

## <a name="clean-up-resources"></a>Limpar recursos

Este tutorial cria vários serviços do Azure em execução. Se você não planeja concluir esta série de tutoriais, recomendamos a exclusão de todos os recursos para evitar incorrer em custos desnecessários.

No menu esquerdo do portal do Azure:

1. Clique em **Grupos de recursos**, depois selecione o grupo de recursos que você criou para o ambiente do Time Series Insights. Na parte superior da página, clique em **Excluir grupo de recursos**, insira o nome do grupo de recursos e, em seguida, clique em **Excluir**.
1. Clique em **Grupos de recursos**, depois selecione o grupo de recursos que foi criado com o acelerador de solução de simulação do dispositivo. Na parte superior da página, clique em **Excluir grupo de recursos**, insira o nome do grupo de recursos e, em seguida, clique em **Excluir**.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:

> [!div class="checklist"]
> * O design do aplicativo
> * Como registrar o aplicativo no Azure AD
> * Como criar, publicar e testar o aplicativo Web

Este tutorial se integra ao Azure AD e usa a identidade do usuário conectado para adquirir um token de acesso. Para saber como acessar a API do Time Series Insights usando a identidade de um aplicativo de serviço ou daemon, confira este artigo:

> [!div class="nextstepaction"]
> [Autenticação e autorização para API do Azure Time Series Insights](time-series-insights-authentication-and-authorization.md)

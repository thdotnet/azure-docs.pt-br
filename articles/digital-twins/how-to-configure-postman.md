---
title: Como configurar o Postman para os Gêmeos Digitais do Azure | Microsoft Docs
description: Como configurar o Postman para os Gêmeos Digitais do Azure.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 09/30/2019
ms.author: v-adgera
ms.openlocfilehash: f33e5be2408d2ebacd215c5f0601d712197254a7
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803439"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Como configurar o Postman para os Gêmeos Digitais do Azure

Este artigo descreve como configurar o cliente Postman REST para interagir e testar APIs de Gerenciamento dos Gêmeos Digitais do Azure. Especificamente, ele descreve:

* Como configurar um aplicativo do Azure Active Directory para usar o fluxo de concessão implícita do OAuth 2.0.
* Como usar o cliente Postman REST para fazer solicitações HTTP de influência de token para suas APIs de gerenciamento.
* Como usar o Postman para fazer solicitações POST com várias partes para suas APIs de gerenciamento.

## <a name="postman-summary"></a>Resumo do Postman

Começar em nos Gêmeos Digitais do Azure usando uma ferramenta de cliente REST como [Postman](https://www.getpostman.com/) para preparar seu ambiente de teste local. O cliente Postman ajuda a criar rapidamente as solicitações HTTP complexas. Faça o download da versão da área de trabalho do cliente Postman acessando [www.getpostman.com/apps](https://www.getpostman.com/apps).

[Postman](https://www.getpostman.com/) é uma ferramenta de teste REST que localiza as funcionalidades de solicitação HTTP principal na área de trabalho útil e GUI baseado em plugin.

Por meio do cliente Postman, os desenvolvedores de soluções podem especificar o tipo de solicitação HTTP (*POST*, *GET*, *UPDATE*, *PATCH* e *DELETE*), ponto de extremidade de API para a chamada e o uso de SSL. O Postman também dá suporte para adicionar cabeçalhos de solicitação HTTP, parâmetros, dados de formulário e corpos.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>Configurar o Azure Active Directory para usar o fluxo de concessão implícita do OAuth 2.0

Configurar seu aplicativo Azure Active Directory para usar o fluxo de concessão implícita OAuth 2.0.

1. Abra o painel **Permissões de API** para o registro do aplicativo. Selecione o botão **Adicionar uma permissão**. No painel **Solicitar permissões de API**, selecione a guia **APIs que a minha organização usa** e pesquise por:
    
    1. `Azure Digital Twins`. Selecione a **API de Gêmeos Digitais do Azure**.

        [![API de Pesquisa ou Gêmeos Digitais do Azure](../../includes/media/digital-twins-permissions/aad-aap-search-api-dt.png)](../../includes/media/digital-twins-permissions/aad-aap-search-api-dt.png#lightbox)

    1. Como alternativa, pesquise `Azure Smart Spaces Service`. Selecione a API **Azure Smart Spaces Service**.

        [![API de Pesquisa para Espaços Inteligentes do Azure](../../includes/media/digital-twins-permissions/aad-app-search-api.png)](../../includes/media/digital-twins-permissions/aad-app-search-api.png#lightbox)

    > [!IMPORTANT]
    > O nome e a ID da API do Azure AD que serão exibidos dependerá do seu locatário:
    > * As contas de locatário e cliente de teste devem Pesquisar `Azure Digital Twins`.
    > * Outras contas da Microsoft devem Pesquisar `Azure Smart Spaces Service`.

1. A API selecionada é exibida como **Gêmeos Digitais do Azure** no mesmo painel **Solicitar permissões de API**. Selecione a lista suspensa **Ler (1)** e a caixa de seleção **Read.Write**. Selecione o botão **Adicionar permissões**.

    [![Adicionar permissões de API](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png#lightbox)

1. Dependendo das configurações da sua organização, talvez você precise seguir etapas adicionais para conceder acesso de administrador a essa API. Entre em contato com seu administrador para obter mais informações. Assim que o acesso de administrador for aprovado, a coluna **CONSENTIMENTO DO ADMINISTRADOR NECESSÁRIO** no painel de **Permissões de API** será mostrada da seguinte forma para as suas APIs:

    [![Adicionar permissões de API](../../includes/media/digital-twins-permissions/aad-app-admin-consent.png)](../../includes/media/digital-twins-permissions/aad-app-admin-consent.png#lightbox)


1. Selecione **manifesto** para abrir o manifesto do aplicativo para seu aplicativo. Definir *oauth2AllowImplicitFlow* para `true`.

    [![Azure Active Directory fluxo implícito](media/how-to-configure-postman/implicit-flow.png)](media/how-to-configure-postman/implicit-flow.png#lightbox)

1. Configurar uma **URL de Resposta** para `https://www.getpostman.com/oauth2/callback`.

    [URL de resposta do ![Azure Active Directory](media/how-to-configure-postman/reply-url.png)](media/how-to-configure-postman/reply-url.png#lightbox)

1. Copie e mantenha a **ID do Aplicativo** do seu aplicativo do Azure Active Directory. Ele é usado nas etapas a seguir.

   [![ID do aplicativo do Azure Active Directory](../../includes/media/digital-twins-permissions/aad-app-reg-app-id.png)](../../includes/media//digital-twins-permissions/aad-app-reg-app-id.png#lightbox)


## <a name="obtain-an-oauth-20-token"></a>Obter um token do OAuth 2.0

[!INCLUDE [digital-twins-management-api](../../includes/digital-twins-management-api.md)]

Configure e configure o postmaster para obter um token de Azure Active Directory. Em seguida, faça uma solicitação HTTP autenticada para os Gêmeos Digitais do Azure usando o token adquirido:

1. Vá para [www.getpostman.com](https://www.getpostman.com/) para baixar o aplicativo.
1. Verifique se sua **URL de Autorização** está correta. Deve pegar o formato:

    ```plaintext
    https://login.microsoftonline.com/YOUR_AZURE_TENANT.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

    | Nome  | Substitua por | Exemplo |
    |---------|---------|---------|
    | YOUR_AZURE_TENANT | O nome do seu locatário ou organização | `microsoft` |

1. Selecione a guia **Autorização**, selecione **OAuth 2.0** e, em seguida, selecione **Obter novo token de acesso**.

    | Campo  | Valor |
    |---------|---------|
    | Tipo de concessão | `Implicit` |
    | URL de retorno de chamada | `https://www.getpostman.com/oauth2/callback` |
    | URL de autenticação | Use a **URL de autorização** da **etapa 2** |
    | ID do cliente | Use a **ID do aplicativo** para o Azure Active Directory aplicativo que foi criado ou reutilizado da seção anterior |
    | Escopo | Deixar em branco |
    | Estado | Deixar em branco |
    | Autenticação de Cliente | `Send as Basic Auth header` |

1. Agora, o cliente deve se parecer com:

    [exemplo de cliente ![Postman](media/how-to-configure-postman/postman-oauth-token.png)](media/how-to-configure-postman/postman-oauth-token.png#lightbox)

1. Selecione **Solicitação de Token**.

    >[!TIP]
    >Se você receber a mensagem de erro "OAuth 2 não pôde ser concluído", tente o seguinte:
    > * Feche o Postman e reabra-o e tente novamente.
  
1. Role para baixo e selecione **uso Token**.

## <a name="make-a-multipart-post-request"></a>Fazer uma solicitação POST com várias partes

Depois de concluir as etapas anteriores, configure o Postman para fazer uma solicitação POST com várias partes HTTP autenticada:

1. Na guia **Cabeçalho**, adicione uma chave do cabeçalho da solicitação HTTP **Content-Type** com valor `multipart/mixed`.

   [![Filtragem de tipo com várias partes/misturadas](media/how-to-configure-postman/content-type.png)](media/how-to-configure-postman/content-type.png#lightbox)

1. Serialize dados não textuais em arquivos. Os dados JSON seriam salvos como um arquivo JSON.
1. Na guia **Corpo**, adicione cada arquivo atribuindo um nome de **chave**, selecionando `file` ou `text`.
1. Em seguida, selecione cada arquivo por meio do botão **Escolher arquivo**.

   [exemplo de cliente ![Postman](media/how-to-configure-postman/form-body.png)](media/how-to-configure-postman/form-body.png#lightbox)

   >[!NOTE]
   > * O cliente Postman não requer que as partes com várias partes tenham um **Content-Type** ou **Content-Disposition** atribuído manualmente.
   > * Você não precisará especificar esses cabeçalhos para cada parte.
   > * É necessário selecionar `multipart/mixed` ou outro **Content-Type** adequado para a solicitação inteira.

1. Por fim, selecione **Enviar** para enviar sua solicitação HTTP post de várias partes.

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre as APIs de gerenciamento dos Gêmeos Digitais do Azure e como usá-las, leia [Como usar as APIs de Gerenciamento dos Gêmeos Digitais do Azure](how-to-navigate-apis.md).

- Usar solicitações com várias partes para [adicionar blobs às entidades dos Gêmeos Digitais do Azure](./how-to-add-blobs.md).

- Para saber mais sobre autenticar as APIs de Gerenciamento, leia [Autenticar com APIs](./security-authenticating-apis.md).

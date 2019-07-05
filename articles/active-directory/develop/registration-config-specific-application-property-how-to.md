---
title: Como preencher campos específicos para um aplicativo personalizado | Microsoft Docs
description: Orientações sobre como preencher campos específicos ao registrar um aplicativo personalizado com o Azure AD
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9bccaa28d34ebff47c7de73a4d9b3d8296ae9fef
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476136"
---
# <a name="how-to-fill-out-specific-fields-for-a-custom-developed-application"></a>Como preencher campos específicos para um aplicativo personalizado

Este artigo oferece uma breve descrição de todos os campos disponíveis no formulário de registro do aplicativo no [portal do Azure](https://portal.azure.com).

## <a name="register-a-new-application"></a>Registrar um novo aplicativo

-   Para registrar um novo aplicativo, navegue até o [portal do Azure](https://portal.azure.com).

-   No painel de navegação esquerdo, clique em **Azure Active Directory.**

-   Escolha **Registros do aplicativo** e clique em **Adicionar**.

-   Isso abre o formulário de registro do aplicativo.

## <a name="fields-in-the-application-registration-form"></a>Campos do formulário de registro do aplicativo


| Campo            | DESCRIÇÃO                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| NOME             | O nome do aplicativo. Ele deve ter um mínimo de quatro caracteres.                |
| Tipos de conta com suporte| Selecione as contas que você gostaria de seu aplicativo para dar suporte a: contas somente nesse diretório organizacional, contas em qualquer diretório organizacional ou contas em qualquer diretório organizacional e contas pessoais da Microsoft.  |
| (Opcional) do URI de redirecionamento | Selecione o tipo de aplicativo que você está compilando, **Web** ou **cliente público (dispositivos móvel e desktop)** e, em seguida, insira o URI de redirecionamento (ou a URL de resposta) para seu aplicativo. Para aplicativos Web, informe a URL base do aplicativo. Por exemplo, http://localhost:31544 pode ser uma URL para um aplicativo Web em execução no seu computador local. Os usuários usariam essa URL para entrar em um aplicativo cliente Web. Para aplicativos cliente públicos, informe o URI usado pelo Azure AD para retornar respostas de token. Insira um valor específico para seu aplicativo, como myapp://auth. Para ver exemplos específicos de aplicativos da Web ou aplicativos nativos, confira nossos [inícios rápidos](https://docs.microsoft.com/azure/active-directory/develop/#quickstarts).|

Depois de preencher os campos acima, o aplicativo está registrado no portal do Azure, e você será redirecionado à página de visão geral do aplicativo. As páginas de configurações no painel esquerdo, sob **gerenciar** ter mais campos para personalizar seu aplicativo. As tabelas a seguir descrevem todos os campos. Você verá apenas um subconjunto desses campos, dependendo se você criou um aplicativo web ou um aplicativo cliente público.

### <a name="overview"></a>Visão geral
| Campo           | DESCRIÇÃO        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ID do aplicativo  | Quando você registra um aplicativo, o Azure AD atribui a seu aplicativo uma ID de aplicativo. A ID do aplicativo pode ser usada para identificar exclusivamente o aplicativo nas solicitações de autenticação no Azure AD, além de acessar recursos, como a API do Graph.                                                          |
| URI de ID do aplicativo      | Deve ser um URI exclusivo, geralmente com o formato **https://&lt;locatário\_nome&gt;/&lt;aplicativo\_nome&gt;.** Isso é usado durante o fluxo de concessão de autorização, como um identificador exclusivo para especificar o recurso que o token deve ser emitido. Ele também se torna a declaração "aud" no token de acesso emitido. |

### <a name="branding"></a>Identidade visual

| Campo           | DESCRIÇÃO        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Carregar novo logotipo | Pode ser usado para carregar um logotipo para seu aplicativo. O logotipo deve estar no formato. bmp,. jpg ou. PNG e o tamanho do arquivo deve ser menor que 100 KB. As dimensões da imagem devem ser 215x215 pixels, com as dimensões da imagem central de 94x94 pixels.|
| URL da home page   | É a URL de logon especificada durante o registro do aplicativo.|

### <a name="authentication"></a>Authentication

| Campo           | DESCRIÇÃO        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| URL de logoff      | Esta é a URL de logoff único. O Azure AD envia uma solicitação de logoff para essa URL quando o usuário limpa a sessão com o Azure AD usando qualquer outro aplicativo registrado.|
| Tipos de conta com suporte  | Esta opção especifica se o aplicativo pode ser usado por vários locatários. Normalmente, isso significa que organizações externas podem usar seu aplicativo registrando-o em seu locatário e concedendo acesso aos dados da organização.|
| URLs de redirecionamento      | O redirecionamento ou resposta, URLs são os pontos de extremidade onde o Azure AD retornará os tokens que seus aplicativo solicitarem. Para aplicativos nativos, é para onde o usuário é enviado após uma autorização bem-sucedida. O Azure AD verifica se o seu aplicativo fornece na solicitação OAuth 2.0 de URI de redirecionamento corresponde a um dos valores registrados no portal.|

### <a name="certificates-and-secrets"></a>Certificados e segredos

| Campo           | DESCRIÇÃO        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Segredos do cliente            | Você pode criar o cliente segredos ou chaves para acessar programaticamente APIs protegidas pelo AD do Azure sem qualquer interação do usuário web. Dos **novo segredo do cliente** página, insira uma descrição da chave e a data de validade e salve para gerar a chave. Certifique-se de salvá-la em algum lugar seguro, pois você não poderá acessá-la depois.             |

## <a name="next-steps"></a>Próximas etapas
[Gerenciando aplicativos com o Azure Active Directory](../manage-apps/what-is-application-management.md)

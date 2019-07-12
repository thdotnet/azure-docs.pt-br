---
title: Configurar um SSO com um clique para seu aplicativo na Galeria de aplicativos Azure AD | Microsoft Docs
description: Etapas para configurar o SSO com um clique para o aplicativo na Galeria de aplicativos Azure AD.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: e0416991-4b5d-4b18-89bb-91b6070ed3ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 358240823da469551e254356fc0613bea20d78c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064810"
---
# <a name="one-click-sso-feature-for-azure-ad-gallery-applications"></a>Recurso SSO com um clique para aplicativos da Galeria do Azure AD

 Neste tutorial, você aprenderá a fazer o SSO com um clique para todos os aplicativos SAML que fornecem interface do usuário para configuração de SSO.

## <a name="introduction-to-one-click-sso"></a>Introdução ao SSO com um clique

O recurso SSO com um clique foi apresentado para configurar o Logon Único para aplicativos da Galeria do Azure AD que oferecem suporte ao protocolo SAML. Na página de configuração do SSO do Azure AD, fornecemos essa opção para permitir que nossos clientes configurem os metadados do Azure AD no lado do aplicativo automaticamente. O objetivo é permitir que os clientes configurem o SSO rapidamente com o mínimo de esforço manual. 

## <a name="advantages-of-the-one-click-sso"></a>Vantagens do SSO com um clique

- Configuração rápida de SSO nos aplicativos da galeria quando os clientes precisam fazer configuração manual no lado do aplicativo.
- Forma mais precisa e eficiente de configuração.
- Sem necessidade de comunicação com parceiros ou suporte para a configuração, já que o aplicativo fornece a interface do usuário para configuração SAML.

## <a name="prerequisites"></a>Pré-requisitos

- Assinatura ativa do aplicativo com credenciais de administrador que serão configuradas com o SSO com um clique.
- **Extensão de navegador Entrada Segura dos Meus Aplicativos** da Microsoft instalada no navegador. Se quiser saber mais sobre essa extensão, confira este [link](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access).

## <a name="one-click-sso-feature-step-by-step-details"></a>Detalhes passo a passo do recurso SSO com um clique

1. Adicione o aplicativo por meio da Galeria de aplicativos Azure AD.

2. Clique em Logon único.

3. Clique em Habilitar Logon único.

4. Preencha os valores de configuração obrigatórios na seção Configuração Básica do SAML.

    > [!NOTE] 
    > Se o aplicativo exigir configuração de declarações personalizadas, configure-as antes de realizar o SSO com um clique.

5. Se o recurso SSO com um clique tiver sido implementado em aplicativos da galeria, você verá a tela a seguir. Se a **extensão de navegador Entrada Segura dos Meus Aplicativos** ainda não estiver instalada, você precisará clicar na opção **Instalar a extensão**.

    ![Instalar a extensão de navegador Entrada Segura dos Meus Aplicativos](./media/one-click-sso-tutorial/install-myappssecure-extension.png)

6. Depois de adicionar a extensão ao navegador, clique em **Configurar Nome do Aplicativo** para ser redirecionado ao portal de administrador do aplicativo. Você precisará entrar como administrador para acessar o aplicativo.

    ![Configurar o nome do aplicativo](./media/one-click-sso-tutorial/setup-sso.png)

7. A extensão do navegador agora configurará o aplicativo para você automaticamente. Ela pedirá sua confirmação para prosseguir. Clique em **Sim**.

    ![Salvando os dados preenchidos automaticamente](./media/one-click-sso-tutorial/save-autopopulate.png)

    > [!NOTE]
    > Se um aplicativo precisar de mais navegação ou etapas, você verá as devidas mensagens pedindo a execução dessas etapas. 

8. Depois que a configuração for concluída, clique em **OK** para salvar as alterações.

    ![Salvar os dados preenchidos automaticamente](./media/one-click-sso-tutorial/save-data.png)

9. Uma mensagem pop-up de confirmação de êxito é exibida e suas configurações de SSO são definidas com sucesso. Você agora pode testar o aplicativo.

    ![SSO configurado](./media/one-click-sso-tutorial/sso-configured.png)

10. Depois que a configuração é concluída com sucesso, o aplicativo é desconectado e você é redirecionado ao portal do Azure.

11. Clique no botão Testar para testar o Logon único.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
* [O que é a extensão de navegador Entrada Segura dos Meus Aplicativos](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access)
 
---
title: 'Tutorial: Configurar o provisionamento automático de usuário de Peakon com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para provisionar e desprovisionar automaticamente contas de usuário para Peakon.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2019
ms.author: zhchia
ms.openlocfilehash: 2547f34432ca1b4b52f34c343bb4aad2f2407f53
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673139"
---
# <a name="tutorial-configure-peakon-for-automatic-user-provisioning"></a>Tutorial: Configurar Peakon para provisionamento automático de usuário

O objetivo deste tutorial é demonstrar as etapas a serem executadas no Peakon e o Azure Active Directory (Azure AD) para configurar o Azure AD automaticamente provisionar e desprovisionar usuários e/ou grupos para Peakon.

> [!NOTE]
>  Este tutorial descreve um conector compilado na parte superior do Serviço de Provisionamento de Usuário do Microsoft Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Esse conector está atualmente em visualização. Para obter mais informações sobre os termos do Microsoft Azure gerais de uso para recursos de visualização, consulte [termos de uso adicionais para visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos

* Um locatário do Azure AD.
* [Um locatário Peakon](https://peakon.com/us/pricing/).
* Uma conta de usuário no Peakon com permissões de administrador.

## <a name="assigning-users-to-peakon"></a>Atribuir usuários ao Peakon

Azure Active Directory usa um conceito chamado *atribuições* para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de usuário, somente os usuários e/ou grupos que foram atribuídos a um aplicativo no Azure AD são sincronizados.

Antes de configurar e habilitar o provisionamento automático de usuário, você deve decidir quais usuários e/ou grupos no Azure AD precisam acessar Peakon. Depois de decidir, você pode atribuir esses usuários e/ou grupos para Peakon seguindo as instruções aqui:

* [Atribuir um usuário ou um grupo a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-peakon"></a>Dicas importantes para atribuir usuários ao Peakon 

* É recomendável que um único usuário do Azure AD seja atribuído para Peakon para testar a configuração de provisionamento automático de usuário. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

* Ao atribuir um usuário ao Peakon, você deve selecionar qualquer função específica do aplicativo válida (se disponível) na caixa de diálogo de atribuição. Usuários com a função **Acesso padrão** são excluídos do provisionamento.

## <a name="set-up-peakon-for-provisioning"></a>Configurar Peakon para provisionamento

1.  Entrar no seu [Console de administração do Peakon](https://app.Peakon.com/login). Clique em **configuração**. 

    ![Console de administração de Peakon](media/Peakon-provisioning-tutorial/Peakon-admin-configuration.png)

2.  Selecione **integrações**.
    
    ![Peakon-funcionário-provisão](media/Peakon-provisioning-tutorial/Peakon-select-integration.png)

3.  Habilitar **funcionário provisionamento**.

    ![Peakon-funcionário-provisão](media/Peakon-provisioning-tutorial/peakon05.png)

4.  Copie os valores para **SCIM 2.0 URL** e **Token de portador OAuth**. Esses valores serão inseridos na **URL do locatário** e **Token secreto** campo na guia provisionamento do seu aplicativo Peakon no portal do Azure.

    ![Peakon criar Token](media/Peakon-provisioning-tutorial/peakon04.png)

## <a name="add-peakon-from-the-gallery"></a>Adicionar Peakon da Galeria

Para configurar o Peakon para provisionamento com o Azure AD automático de usuário, você precisará adicionar Peakon da Galeria de aplicativos do Azure AD à sua lista de aplicativos SaaS gerenciados.

1. No  **[portal do Azure](https://portal.azure.com)** , no painel de navegação à esquerda, selecione **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Vá para **Aplicativos da empresa**, em seguida, selecione **Todos os aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Para adicionar um novo aplicativo, selecione a **novo aplicativo** botão na parte superior do painel.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Peakon**, selecione **Peakon** no painel de resultados e em seguida, clique o **Add** botão para adicionar o aplicativo.

    ![Peakon na lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-peakon"></a>Configurando o provisionamento automático de usuário para Peakon 

Esta seção orienta você pelas etapas de configuração do AD do Azure serviço de provisionamento para criar, atualizar e desabilitar usuários e/ou grupos no Peakon com base nas atribuições de usuário e/ou grupo no AD do Azure.

> [!TIP]
> Você também pode optar por habilitar baseado em SAML single sign-on para Peakon, seguindo as instruções fornecidas na [tutorial do logon único do Peakon](peakon-tutorial.md). O logon único pode ser configurado independentemente do provisionamento automático de usuário, embora esses dois recursos sejam complementares.

### <a name="to-configure-automatic-user-provisioning-for-peakon--in-azure-ad"></a>Para configurar o provisionamento automático de usuário para Peakon no Azure AD:

1. Entre no [Portal do Azure](https://portal.azure.com). Selecione **aplicativos empresariais**, em seguida, selecione **todos os aplicativos**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Peakon**.

    ![O link Peakon na lista de aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento**.

    ![Guia de provisionamento](common/provisioning.png)

4. Defina o **Modo de Provisionamento** como **Automático**.

    ![Guia de provisionamento](common/provisioning-automatic.png)

5. Sob o **credenciais de administrador** seção, insira o **SCIM 2.0 URL** e **Token de portador OAuth** valores recuperados anteriormente no **URL do locatário** e **segredo do Token** , respectivamente. Clique em **Conexão de teste** para garantir que o Azure AD pode se conectar ao Peakon. Se a conexão falhar, verifique se que sua conta Peakon tem permissões de administrador e tente novamente.

    ![URL do locatário + Token](common/provisioning-testconnection-tenanturltoken.png)

7. No campo **Notificação por Email**, insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e selecione a caixa de seleção - **Enviar uma notificação por email quando ocorrer uma falha**.

    ![Email de notificação](common/provisioning-notification-email.png)

8. Clique em **Salvar**.

9. Sob o **mapeamentos** seção, selecione **sincronizar o Azure Active Directory Users para Peakon**.

    ![Mapeamentos de usuário Peakon](media/Peakon-provisioning-tutorial/Peakon-user-mappings.png)

10. Examine os atributos de usuário que são sincronizados do Azure AD para Peakon na **mapeamento de atributos** seção. Os atributos selecionados como **correspondentes** propriedades são usadas para corresponder as contas de usuário no Peakon para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

    ![Atributos de usuário Peakon](media/Peakon-provisioning-tutorial/Peakon-user-attributes.png)

12. Para configurar filtros de escopo, consulte as instruções a seguir fornecidas na [tutorial do filtro de escopo](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).
    
    ![Escopo de provisionamento](common/provisioning-scope.png)

15. Quando estiver pronto para provisionar, clique em **Salvar**.

    ![Salvando a configuração de provisionamento](common/provisioning-configuration-save.png)

Essa operação inicia a sincronização inicial de todos os usuários e/ou grupos definidos no **Escopo** na seção **Configurações**. Observe que a sincronização inicial levará mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço de provisionamento do Microsoft Azure Active Directory esteja em execução. Você pode usar o **detalhes de sincronização** seção para monitorar o progresso e siga os links para relatório de atividade, que descreve todas as ações executadas pelo serviço no Peakon de provisionamento do Azure AD de provisionamento.

Para saber mais sobre como ler os logs de provisionamento do Azure AD, consulte [Relatórios sobre o provisionamento automático de contas de usuário](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitações do conector

* Todos os atributos de usuário personalizada no Peakon precisará ser estendido a partir da extensão usuário SCIM do Peakon personalizada de `urn:ietf:params:scim:schemas:extension:peakon:2.0:User`.

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
## <a name="next-steps"></a>Próximas etapas

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../manage-apps/check-status-user-account-provisioning.md)
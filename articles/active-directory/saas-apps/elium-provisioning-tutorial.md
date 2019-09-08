---
title: 'Tutorial: Configurar o Elium para o provisionamento automático de usuário com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar Azure Active Directory para provisionar e desprovisionar automaticamente contas de usuário para o Elium.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: fb48deae-4653-448a-ba2f-90258edab3a7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2019
ms.author: Zhchia
ms.openlocfilehash: 30f6f81234ce34e40a29a04f6b795aab991a8824
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70802856"
---
# <a name="tutorial-configure-elium-for-automatic-user-provisioning"></a>Tutorial: Configurar o Elium para provisionamento automático de usuário

O objetivo deste tutorial é demonstrar as etapas a serem executadas no Elium e no Azure Active Directory (Azure AD) para configurar o Azure AD para provisionar e desprovisionar automaticamente usuários e/ou grupos no Elium.

> [!NOTE]
> Este tutorial descreve um conector compilado na parte superior do Serviço de Provisionamento de Usuário do Microsoft Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Atualmente, esse conector está em versão prévia pública. Para obter mais informações sobre os termos de uso geral de Microsoft Azure para recursos de visualização, consulte [termos de uso suplementares para visualizações de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos:

* Um locatário do Azure AD.
* [Um locatário do Elium](https://www.elium.com/pricing/)
* Uma conta de usuário no Elium com permissões de administrador.

## <a name="assigning-users-to-elium"></a>Atribuindo usuários ao Elium

Azure Active Directory usa um conceito chamado *atribuições* para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de usuário, somente os usuários e/ou grupos que foram atribuídos a um aplicativo no Azure AD são sincronizados.

Antes de configurar e habilitar o provisionamento automático de usuário, você deve decidir quais usuários e/ou grupos no Azure AD precisam de acesso ao Elium. Depois de decidir, você pode atribuir esses usuários e/ou grupos ao Elium seguindo as instruções aqui:
* [Atribuir um usuário ou um grupo a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-elium"></a>Dicas importantes para atribuir usuários ao Elium 

* É recomendável que um único usuário do Azure AD seja atribuído ao Elium para testar a configuração automática de provisionamento de usuário. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

* Ao atribuir um usuário ao Elium, você deve selecionar qualquer função específica do aplicativo válida (se disponível) na caixa de diálogo de atribuição. Usuários com a função **Acesso padrão** são excluídos do provisionamento.

## <a name="set-up-elium-for-provisioning"></a>Configurar o Elium para provisionamento

Antes de configurar o Elium para o provisionamento automático de usuário com o Azure AD, será necessário habilitar o provisionamento do SCIM no Elium.

1. Faça logon no Elium. Navegue até **minhas** > **configurações**de perfil.

    ![Elium](media/Elium-provisioning-tutorial/setting.png)

2. No canto inferior esquerdo, em avançado, selecione **segurança**.

    ![Elium](media/Elium-provisioning-tutorial/security.png)

3. Copie o **token secreto**. Esse valor será inserido no campo **token secreto** na guia provisionamento do seu aplicativo Elium no portal do Azure.

    ![Elium](media/Elium-provisioning-tutorial/token.png)


## <a name="add-elium--from-the-gallery"></a>Adicionar o Elium da Galeria

Para configurar o Elium para o provisionamento automático de usuário com o Azure AD, você precisará adicionar o Elium da Galeria de aplicativos do Azure AD à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Elium da Galeria de aplicativos do Azure AD, execute as seguintes etapas:**

1. No **[portal do Azure](https://portal.azure.com)** , no painel de navegação à esquerda, selecione **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Vá para **Aplicativos da empresa**, em seguida, selecione **Todos os aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Para adicionar um novo aplicativo, selecione o botão **novo aplicativo** na parte superior do painel.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, insira **Elium**, selecione **Elium** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Elium na lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-elium"></a>Configurando o provisionamento automático de usuário para o Elium  

Esta seção orienta você pelas etapas para configurar o serviço de provisionamento do Azure AD para criar, atualizar e desabilitar usuários e/ou grupos no Elium com base em atribuições de usuário e/ou grupo no Azure AD.

> [!TIP]
> Você também pode optar por habilitar o logon único baseado em SAML para o Elium, seguindo as instruções fornecidas no [tutorial de logon único do Elium](Elium-tutorial.md). O logon único pode ser configurado independentemente do provisionamento automático de usuário, embora esses dois recursos se complementem uns aos outros

### <a name="to-configure-automatic-user-provisioning-for-elium--in-azure-ad"></a>Para configurar o provisionamento automático de usuário para Elium no Azure AD:

1. Entre no [Portal do Azure](https://portal.azure.com). Selecione **aplicativos empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escolha **Elium**.

    ![O link do Elium na lista de aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento**.

    ![Guia provisionamento](common/provisioning.png)

4. Defina o **Modo de Provisionamento** como **Automático**.

    ![Guia provisionamento](common/provisioning-automatic.png)

5. Na seção credenciais de administrador, insira `<tenantURL>/scim/v2` onde **{TeanantURL}** é o valor recuperado anteriormente do console de administração do Elium. Insira o valor do **token secreto** no **token secreto** clique em **testar conexão** para garantir que o Azure ad possa se conectar ao Elium. Se a conexão falhar, verifique se o Elium tem permissões de administrador e tente novamente.

    ![URL do locatário + token](common/provisioning-testconnection-tenanturltoken.png)

6. No campo **Notificação por Email**, insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e selecione a caixa de seleção - **Enviar uma notificação por email quando ocorrer uma falha**.

    ![Email de Notificação](common/provisioning-notification-email.png)

7. Clique em **Salvar**.

8. Na seção **mapeamentos** , selecione **sincronizar Azure Active Directory usuários para Elium**.

    ![Mapeamentos de usuário Elium](media/Elium-provisioning-tutorial/usermapping.png)

9. Examine os atributos de usuário que são sincronizados do Azure AD para o Elium na seção **mapeamento de atributos** . Os atributos selecionados como propriedades **correspondentes** são usados para corresponder as contas de usuário no Elium para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

    ![Atributos de usuário do Elium](media/Elium-provisioning-tutorial/userattribute.png)


11. Para configurar filtros de escopo, consulte as seguintes instruções fornecidas no [tutorial do Filtro de Escopo](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Para habilitar o serviço de provisionamento do Azure AD para o Elium, altere o **status de provisionamento** para **ativado** na seção **configurações** .

    ![Status de provisionamento alternado em](common/provisioning-toggle-on.png)

13. Defina os usuários e/ou grupos que você deseja provisionar para o Elium escolhendo os valores desejados no **escopo** na seção **configurações** .

    ![Escopo de provisionamento](common/provisioning-scope.png)

14. Quando estiver pronto para provisionar, clique em **Salvar**.

    ![Salvando configuração de provisionamento](common/provisioning-configuration-save.png)

Essa operação inicia a sincronização inicial de todos os usuários e/ou grupos definidos no **Escopo** na seção **Configurações**. A sincronização inicial demora mais para ser executada do que as sincronizações subsequentes. Para obter mais informações sobre quanto tempo levará para o provisionamento de usuários e/ou grupos, consulte [quanto tempo levará para provisionar usuários](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users).

Você pode usar a seção **status atual** para monitorar o progresso e seguir os links para o relatório de atividade de provisionamento, que descreve todas as ações executadas pelo serviço de provisionamento do Azure AD no Elium. Para obter mais informações, consulte [verificar o status do provisionamento do usuário](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md). Para ler os logs de provisionamento do Azure AD, consulte [relatórios sobre o provisionamento automático de conta de usuário](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciando o provisionamento de conta de usuário para aplicativos empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md).
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Próximas etapas

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../manage-apps/check-status-user-account-provisioning.md)

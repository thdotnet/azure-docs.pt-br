---
title: 'Tutorial: Configurar 4me para provisionamento automático de usuário com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para provisionar e desprovisionar automaticamente contas de usuário para 4me.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/3/2019
ms.author: zchia
ms.openlocfilehash: e2e7c27d8cfa79bc7a8f8462def4d46e598cb508
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595087"
---
# <a name="tutorial-configure-4me-for-automatic-user-provisioning"></a>Tutorial: Configurar 4me para provisionamento automático de usuário

O objetivo deste tutorial é demonstrar as etapas a serem executadas no 4me e o Azure Active Directory (Azure AD) para configurar o AD do Azure para automaticamente provisionar e desprovisionar usuários e/ou grupos para 4me.

> [!NOTE]
> Este tutorial descreve um conector compilado na parte superior do Serviço de Provisionamento de Usuário do Microsoft Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Atualmente, esse conector está em versão prévia pública. Para obter mais informações sobre os termos do Microsoft Azure gerais de uso para recursos de visualização, consulte [termos de uso adicionais para visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos:

* Um locatário do Azure AD
* [Um locatário 4me](https://www.4me.com/trial/)
* Uma conta de usuário no 4me com permissões de administrador.

## <a name="add-4me-from-the-gallery"></a>Adicionar 4me da Galeria

Antes de configurar 4me para provisionamento automático de usuário com o Azure AD, você precisará adicionar 4me da Galeria de aplicativos do Azure AD à sua lista de aplicativos SaaS gerenciados.

**Para adicionar 4me da Galeria de aplicativos do Azure AD, execute as seguintes etapas:**

1. No  **[portal do Azure](https://portal.azure.com)** , no painel de navegação à esquerda, selecione **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Vá para **Aplicativos da empresa**, em seguida, selecione **Todos os aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Para adicionar um novo aplicativo, selecione a **novo aplicativo** botão na parte superior do painel.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **4me**, selecione **4me** no painel de resultados e em seguida, clique o **Add** botão para adicionar o aplicativo.

    ![4me na lista de resultados](common/search-new-app.png)

## <a name="assigning-users-to-4me"></a>Atribuir usuários ao 4me

Azure Active Directory usa um conceito chamado *atribuições* para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de usuário, somente os usuários e/ou grupos que foram atribuídos a um aplicativo no Azure AD são sincronizados.

Antes de configurar e habilitar o provisionamento automático de usuário, você deve decidir quais usuários e/ou grupos no Azure AD precisam acessar 4me. Depois de decidir, você pode atribuir esses usuários e/ou grupos para 4me seguindo as instruções aqui:

* [Atribuir um usuário ou um grupo a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-4me"></a>Dicas importantes para atribuir usuários ao 4me

* É recomendável que um único usuário do Azure AD seja atribuído para 4me para testar a configuração de provisionamento automático de usuário. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

* Ao atribuir um usuário ao 4me, você deve selecionar qualquer função específica do aplicativo válida (se disponível) na caixa de diálogo de atribuição. Usuários com a função **Acesso padrão** são excluídos do provisionamento.

## <a name="configuring-automatic-user-provisioning-to-4me"></a>Configurando o provisionamento automático de usuário para 4me 

Esta seção orienta você pelas etapas de configuração do AD do Azure serviço de provisionamento para criar, atualizar e desabilitar usuários e/ou grupos no 4me com base em atribuições de usuário e/ou grupo no AD do Azure.

> [!TIP]
> Você também pode optar por habilitar baseado em SAML single sign-on para 4me, seguindo as instruções fornecidas na [tutorial do logon único 4me](4me-tutorial.md). O logon único pode ser configurado independentemente do provisionamento automático de usuário, embora esses dois recursos sejam complementares.

### <a name="to-configure-automatic-user-provisioning-for-4me-in-azure-ad"></a>Para configurar o provisionamento automático de usuário para 4me no Azure AD:

1. Entre no [Portal do Azure](https://portal.azure.com). Selecione **aplicativos empresariais**, em seguida, selecione **todos os aplicativos**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escolha **4me**.

    ![O link do 4me na lista de Aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento**.

    ![Guia de provisionamento](common/provisioning.png)

4. Defina o **Modo de Provisionamento** como **Automático**.

    ![Guia de provisionamento](common/provisioning-automatic.png)

5. Para recuperar o **URL do locatário** e **Token secreto** da sua conta 4me, siga o passo a passo, conforme descrito na etapa 6.

6. Entrar no Console do administrador de 4me. Navegue até **configurações**.

    ![Configurações de 4me](media/4me-provisioning-tutorial/4me01.png)

    Digite **aplicativos** na barra de pesquisa.

    ![aplicativos 4me](media/4me-provisioning-tutorial/4me02.png)

    Abra o **SCIM** na lista suspensa para recuperar o segredo do Token e o ponto de extremidade SCIM.

    ![4me SCIM](media/4me-provisioning-tutorial/4me03.png)

7. Ao popular os campos mostrados na etapa 5, clique em **Conexão de teste** para garantir que o Azure AD pode se conectar ao 4me. Se a conexão falhar, verifique se que sua conta 4me tem permissões de administrador e tente novamente.

    ![A criptografia do token](common/provisioning-testconnection-tenanturltoken.png)

8. No campo **Notificação por Email**, insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e selecione a caixa de seleção - **Enviar uma notificação por email quando ocorrer uma falha**.

    ![Email de notificação](common/provisioning-notification-email.png)

9. Clique em **Salvar**.

10. Sob o **mapeamentos** seção, selecione **sincronizar o Azure Active Directory Users para 4me**.

    ![Mapeamentos de usuário 4me](media/4me-provisioning-tutorial/4me-user-mapping.png)
    
11. Examine os atributos de usuário que são sincronizados do Azure AD para 4me na **mapeamento de atributos** seção. Os atributos selecionados como **correspondentes** propriedades são usadas para corresponder as contas de usuário no 4me para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

    ![Mapeamentos de usuário 4me](media/4me-provisioning-tutorial/4me-user-attributes.png)
    
12. Sob o **mapeamentos** seção, selecione **sincronizar Azure grupos do Active Directory para 4me**.

    ![Mapeamentos de usuário 4me](media/4me-provisioning-tutorial/4me-group-mapping.png)
    
13. Examine os atributos de grupo que são sincronizados do Azure AD para 4me na **mapeamento de atributos** seção. Os atributos selecionados como **correspondentes** propriedades são usadas para corresponder os grupos no 4me para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

    ![Mapeamentos de grupo 4me](media/4me-provisioning-tutorial/4me-group-attribute.png)

14. Para configurar filtros de escopo, consulte as seguintes instruções fornecidas no [tutorial do Filtro de Escopo](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Para habilitar o Azure AD 4me serviço de provisionamento, altere o **Status de provisionamento** para **na** no **configurações** seção.

    ![Status de provisionamento ativado](common/provisioning-toggle-on.png)

16. Defina os usuários e/ou grupos que você deseja a serem provisionados para 4me escolhendo os valores desejados em **escopo** na **configurações** seção.

    ![Escopo de provisionamento](common/provisioning-scope.png)

17. Quando estiver pronto para provisionar, clique em **Salvar**.

    ![Salvando a configuração de provisionamento](common/provisioning-configuration-save.png)

Essa operação inicia a sincronização inicial de todos os usuários e/ou grupos definidos no **Escopo** na seção **Configurações**. Observe que a sincronização inicial levará mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço de provisionamento do Microsoft Azure Active Directory esteja em execução. Você pode usar o **detalhes de sincronização** seção para monitorar o progresso e siga os links para relatório de atividade, que descreve todas as ações executadas pelo serviço no 4me de provisionamento do Azure AD de provisionamento.

Para saber mais sobre como ler os logs de provisionamento do Azure AD, consulte [Relatórios sobre o provisionamento automático de contas de usuário](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitações do conector

* 4me tem diferentes URLs de ponto de extremidade SCIM para ambientes de teste e produção. A primeira opção termina com **.qa** enquanto o último termina com **.com**
* Tokens de segredo 4me gerado tem uma data de expiração de um mês de geração.
* não dá suporte a 4me **excluir** operações

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Próximas etapas

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../manage-apps/check-status-user-account-provisioning.md)
---
title: 'Tutorial: Configurar o diretório de federado para provisionamento automático de usuário com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para provisionar e desprovisionar automaticamente contas de usuário no diretório de federado.
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
ms.date: 07/12/2019
ms.author: zhchia
ms.openlocfilehash: 19f5690a6852161abce2565a8c4a52ce86ff5187
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840583"
---
# <a name="tutorial-configure-federated-directory-for-automatic-user-provisioning"></a>Tutorial: Configurar o diretório de federado para provisionamento automático de usuário

O objetivo deste tutorial é demonstrar as etapas a serem executadas no diretório federado e o Azure Active Directory (Azure AD) para configurar o Azure AD para provisionar e desprovisionar automaticamente usuários e/ou grupos ao diretório federado.

> [!NOTE]
>  Este tutorial descreve um conector compilado na parte superior do Serviço de Provisionamento de Usuário do Microsoft Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Atualmente, esse conector está em versão prévia pública. Para obter mais informações sobre os termos do Microsoft Azure gerais de uso para recursos de visualização, consulte [termos de uso adicionais para visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos:

* Um locatário do Azure AD.
* [Um diretório de federado](https://www.federated.directory/pricing).
* Uma conta de usuário no diretório federado com permissões de administrador.

## <a name="assign-users-to-federated-directory"></a>Atribuir usuários ao diretório federado
O Azure Active Directory usa um conceito chamado atribuições para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de usuário, somente os usuários e/ou grupos que foram atribuídos a um aplicativo no Azure AD são sincronizados.

Antes de configurar e habilitar o provisionamento automático de usuário, você deve decidir quais usuários e/ou grupos no Azure AD precisam de acesso ao diretório de federado. Depois de decidir, você pode atribuir esses usuários e/ou grupos ao diretório federado, seguindo as instruções aqui:

 * [Atribuir um usuário ou um grupo a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md) 
 
 ## <a name="important-tips-for-assigning-users-to-federated-directory"></a>Dicas importantes para atribuir usuários ao diretório federado
 * É recomendável que um único usuário do Azure AD seja atribuído ao diretório de federado para testar a configuração de provisionamento automático de usuário. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

* Ao atribuir um usuário ao diretório federado, você deve selecionar qualquer função específica do aplicativo válida (se disponível) na caixa de diálogo de atribuição. Usuários com a função de acesso padrão são excluídos do provisionamento.
    
 ## <a name="set-up-federated-directory-for-provisioning"></a>Configurar o diretório de federado para provisionamento

Antes de configurar o diretório de federado para provisionamento automático de usuário com o Azure AD, você precisará habilitar o provisionamento de SCIM no diretório de federado.

1. Entrar no seu [federado Console de administração de diretório](https://federated.directory/of)

    ![Tutorial de diretório federado](media/federated-directory-provisioning-tutorial/companyname.png)

2. Navegue até **diretórios > diretórios de usuário** e selecione seu locatário. 

    ![diretório federado](media/federated-directory-provisioning-tutorial/ad-user-directories.png)

3.  Para gerar um token de portador permanente, navegue até **chaves do diretório > Criar nova chave.** 

    ![diretório federado](media/federated-directory-provisioning-tutorial/federated01.png)

4. Crie uma chave de diretório. 

    ![diretório federado](media/federated-directory-provisioning-tutorial/federated02.png)
    

5. Copie o valor de **Token de Acesso**. Esse valor será inserido na **segredo do Token** campo na guia provisionamento do seu aplicativo federado diretório no portal do Azure. 

    ![diretório federado](media/federated-directory-provisioning-tutorial/federated03.png)
    
## <a name="add-federated-directory-from-the-gallery"></a>Adicionar diretório federado da Galeria

Para configurar o diretório de federado para provisionamento com o Azure AD automático de usuário, você precisará adicionar o diretório de federado da Galeria de aplicativos do Azure AD à sua lista de aplicativos SaaS gerenciados.

**Para adicionar um diretório federado da Galeria de aplicativos do Azure AD, execute as seguintes etapas:**

1. No  **[portal do Azure](https://portal.azure.com)** , no painel de navegação à esquerda, selecione **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Vá para **Aplicativos da empresa**, em seguida, selecione **Todos os aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Para adicionar um novo aplicativo, selecione a **novo aplicativo** botão na parte superior do painel.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Directory federado**, selecione **Federated Directory** no painel de resultados.

    ![Diretório federado na lista de resultados](common/search-new-app.png)

5. Navegue até a **URL** realçado abaixo em um navegador separado. 

    ![diretório federado](media/federated-directory-provisioning-tutorial/loginpage1.png)

6. Clique em **faça logon no**.

    ![diretório federado](media/federated-directory-provisioning-tutorial/federated04.png)

7.  Como diretório federado é um aplicativo OpenIDConnect, optar por fazer logon no diretório federado usando sua conta corporativa da Microsoft.
    
    ![diretório federado](media/federated-directory-provisioning-tutorial/loginpage3.png)
 
8. Após uma autenticação bem-sucedida, aceite a solicitação de consentimento para a página de consentimento. O aplicativo, em seguida, será adicionado automaticamente ao seu locatário e você será redirecionado para sua conta de diretório federado.

    ![diretório de federado adicionar SCIM](media/federated-directory-provisioning-tutorial/premission.png)



## <a name="configuring-automatic-user-provisioning-to-federated-directory"></a>Configurando o provisionamento automático de usuário para o diretório de federado 

Esta seção orienta você pelas etapas de configuração do AD do Azure serviço de provisionamento para criar, atualizar e desabilitar usuários e/ou grupos no diretório federado baseado no usuário e/ou as atribuições de grupo no AD do Azure.

### <a name="to-configure-automatic-user-provisioning-for-federated-directory-in-azure-ad"></a>Para configurar o provisionamento automático de usuário para o diretório de federado no Azure AD:

1. Entre no [Portal do Azure](https://portal.azure.com). Selecione **aplicativos empresariais**, em seguida, selecione **todos os aplicativos**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Federated Directory**.

    ![O link de diretório federado na lista de aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento**.

    ![Guia de provisionamento](common/provisioning.png)

4. Defina o **Modo de Provisionamento** como **Automático**.

    ![Guia de provisionamento](common/provisioning-automatic.png)

5. Sob o **credenciais de administrador** seção, entrada `https://api.federated.directory/v2/` URL do locatário. Insira o valor recuperado e salvo anteriormente do diretório federado no **segredo do Token**. Clique em **Conexão de teste** para garantir que o Azure AD pode se conectar ao diretório federado. Se a conexão falhar, verifique se que sua conta de diretório federado tem permissões de administrador e tente novamente.

    ![URL do locatário + Token](common/provisioning-testconnection-tenanturltoken.png)

8. No campo **Notificação por Email**, insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e selecione a caixa de seleção - **Enviar uma notificação por email quando ocorrer uma falha**.

    ![Email de notificação](common/provisioning-notification-email.png)

9. Clique em **Salvar**.

10. Sob o **mapeamentos** seção, selecione **sincronizar Azure usuários do Active Directory para o diretório de federado**.

    ![Tutorial de diretório federado](media/federated-directory-provisioning-tutorial/user-mappings.png)
    
    
11. Examine os atributos de usuário que são sincronizados do Azure AD para o diretório de federado na **mapeamento de atributos** seção. Os atributos selecionados como **correspondentes** propriedades são usadas para corresponder as contas de usuário no diretório de federado para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

    ![Tutorial de diretório federado](media/federated-directory-provisioning-tutorial/user-attributes.png)
    

12. Para configurar filtros de escopo, consulte as seguintes instruções fornecidas no [tutorial do Filtro de Escopo](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar o provisionamento de serviço para o diretório federado do Azure AD, altere o **Status de provisionamento** ao **na** no **configurações** seção.

    ![Status de provisionamento ativado](common/provisioning-toggle-on.png)

14. Definir os usuários e/ou grupos que você gostaria serem provisionados para o diretório federado, escolhendo os valores desejados em **escopo** na **configurações** seção.

    ![Escopo de provisionamento](common/provisioning-scope.png)

15. Quando estiver pronto para provisionar, clique em **Salvar**.

    ![Salvando a configuração de provisionamento](common/provisioning-configuration-save.png)

Essa operação inicia a sincronização inicial de todos os usuários e/ou grupos definidos no **Escopo** na seção **Configurações**. Observe que a sincronização inicial levará mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço de provisionamento do Microsoft Azure Active Directory esteja em execução. Você pode usar o **detalhes de sincronização** seção para monitorar o progresso e siga os links para relatório de atividade, que descreve todas as ações executadas pelo serviço de diretório federado de provisionamento do Azure AD de provisionamento.

Para obter mais informações sobre como ler os logs de provisionamento do Azure AD, consulte [relatórios sobre o provisionamento automático de usuário](../manage-apps/check-status-user-account-provisioning.md)
## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Próximas etapas

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../manage-apps/check-status-user-account-provisioning.md)
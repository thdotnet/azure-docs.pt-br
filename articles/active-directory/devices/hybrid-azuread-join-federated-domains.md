---
title: Configurar o ingresso no Azure Active Directory híbrido para os domínios federados | Microsoft Docs
description: Saiba como configurar o ingresso no Azure Active Directory híbrido para domínios federados.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 600d6b9f1eb8d8073e1658dd5b8196a3d8137e42
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733711"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Tutorial: Configurar o ingresso no Azure Active Directory híbrido para os domínios federados

De maneira semelhante a um usuário, um dispositivo é outra identidade principal que você deseja proteger e usá-la para proteger seus recursos a qualquer hora e em qualquer local. É possível atingir essa meta colocando e gerenciando identidades de dispositivo no Azure AD usando um dos métodos a seguir:

- Ingresso no Azure AD
- Ingresso no Azure AD Híbrido
- Registro do Azure AD

Ao colocar os dispositivos no Azure AD, você maximiza a produtividade dos usuários por meio de SSO (logon único) em toda a nuvem e recursos locais. Ao mesmo tempo, é possível proteger o acesso à sua nuvem e aos recursos locais com [acesso condicional](../active-directory-conditional-access-azure-portal.md).

Neste tutorial, você aprenderá como configurar o ingresso no Azure AD híbrido para dispositivos de computadores ingressados no domínio do AD em um ambiente federado usando o AD FS.

> [!NOTE]
> Se seu ambiente federado estiver usando um provedor de identidade diferente do AD FS, será necessário verificar se ele é compatível com o protocolo WS-Trust. O WS-Trust é necessário para autenticar seus dispositivos ingressados no Azure AD híbrido atuais do Windows com o Azure AD. Além disso, se você tiver dispositivos de nível inferior do Windows que você precisa ingressar no Azure AD híbrido, seu provedor de identidade precisará dar suporte à declaração WIAORMULTIAUTHN. 


> [!div class="checklist"]
> * Configurar ingresso no Azure AD híbrido
> * Habilitar dispositivos de nível inferior do Windows
> * Verificar o registro
> * Solucionar problemas

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial assume que você está familiarizado com:

- [Introdução ao gerenciamento de identidades do dispositivo no Azure Active Directory](../device-management-introduction.md)
- [Como planejar a implementação de ingresso no Azure Active Directory híbrido](hybrid-azuread-join-plan.md)
- [Como realizar a validação controlada de ingresso no Azure AD híbrido](hybrid-azuread-join-control.md)

Para configurar o cenário neste tutorial, é necessário ter:

- Windows Server 2012 R2 com AD FS
- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) versão 1.1.819.0 ou posterior.

A partir da versão 1.1.819.0, o Azure AD Connect fornece um assistente para configurar o ingresso no Azure AD híbrido. O assistente permite simplificar significativamente o processo de configuração. O assistente relacionado:

- Configura SCP (pontos de conexão de serviço) para registro do dispositivo
- Faz backup da terceira parte confiável do Azure AD
- Atualiza as regras de declaração na confiança do Azure AD

As etapas de configuração neste artigo são baseadas nesse assistente. Se você tiver uma versão mais antiga do Azure AD Connect instalada, será necessário atualizá-la para a versão 1.1.819 ou superior. Se a instalação da versão mais recente do Azure AD Connect não for uma opção para você, confira [como configurar manualmente o ingresso no Azure AD híbrido](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual).

O ingresso no Azure AD híbrido requer que os dispositivos tenham acesso aos seguintes recursos da Microsoft dentro da rede da organização:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- STS de sua organização (domínios federados)
- `https://autologon.microsoftazuread-sso.com` (Se você estiver usando ou planejando usar SSO Contínuo)

Começando com o Windows 10 1803, se o ingresso instantâneo no Azure AD híbrido para o ambiente federado do AD FS falhar, dependeremos do Azure AD Connect para sincronizar o objeto de computador no Azure AD que será usado posteriormente para concluir o registro de dispositivo para ingresso no Azure AD híbrido. Verifique se o Azure AD Connect sincronizou os objetos de computador dos dispositivos que você deseja ser o Azure AD híbrido para o Azure AD. Se os objetos de computador pertencem a unidades organizacionais (OU) específicas, essas OUs precisam ser configuradas para sincronização no Azure AD Connect também. Para saber mais sobre como sincronizar objetos de computador usando o Azure AD Connect, confira o artigo em [Configurar filtragem usando o Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering#organizational-unitbased-filtering).

Se sua organização exigir acesso à Internet por meio de um proxy de saída, a Microsoft recomendará [implantar a WPAD (Descoberta Automática de Proxy Web)](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) para permitir que computadores Windows 10 realizem o registro do dispositivo com o Azure AD. Se você estiver tendo problemas com a configuração e o gerenciamento do WPAD, acesse [Solução de problemas de detecção automática](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10)). 

Se não estiver usando a WPAD e precisar definir configurações de proxy em seu computador, será possível fazer isso a partir do Windows 10 1709 [definindo as configurações de WinHTTP usando um GPO (objeto de política de grupo)](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/).

> [!NOTE]
> Se você definir configurações de proxy em seu computador usando as configurações de WinHTTP, então os computadores que não puderem se conectar ao proxy configurado falharão ao se conectar à Internet.

Se a organização exigir acesso à Internet por meio de um proxy de saída autenticado, será necessário garantir que os computadores com Windows 10 possam ser autenticados com êxito no proxy de saída. Como computadores com Windows 10 executam o registro de dispositivos usando o contexto do computador, será necessário configurar a autenticação de proxy de saída usando o contexto do computador. Acompanhe com o provedor de proxy de saída nos requisitos de configuração.

## <a name="configure-hybrid-azure-ad-join"></a>Configurar ingresso no Azure AD híbrido

Para configurar um ingresso no Azure AD híbrido usando o Azure AD Connect, será necessário ter:

- Credenciais de um administrador global para o locatário do Azure AD.  
- As credenciais de administrador corporativo para cada uma das florestas.
- As credenciais do administrador do AD FS.

**Para configurar um ingresso no Azure AD híbrido usando o Azure AD Connect:**

1. Inicie o Azure AD Connect e clique em **Configurar**.

   ![Bem-Vindo](./media/hybrid-azuread-join-federated-domains/11.png)

1. Na página **Tarefas adicionais**, selecione **Configurar opções de dispositivo** e, em seguida, clique em **Avançar**.

   ![Tarefas adicionais](./media/hybrid-azuread-join-federated-domains/12.png)

1. Na página de **Visão Geral**, clique em **Avançar**.

   ![Visão geral](./media/hybrid-azuread-join-federated-domains/13.png)

1. Na página **Conectar o Azure AD**, insira as credenciais de um administrador global para o locatário do Azure AD e clique em**Avançar**.

   ![Conecte-se ao AD do Azure](./media/hybrid-azuread-join-federated-domains/14.png)

1. Na página **Opções do dispositivo**, selecione **Configurar ingresso no Azure AD Híbrido** e, em seguida, clique em **Avançar**.

   ![Opções do dispositivo](./media/hybrid-azuread-join-federated-domains/15.png)

1. Na página **SCP**, execute as etapas a seguir e, em seguida, clique em **Avançar**:

   ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

   1. Selecione a floresta.
   1. Selecione o serviço de autenticação. Você deve selecionar o servidor do AD FS, a menos que sua organização tenha exclusivamente os clientes do Windows 10 e você tiver configurado a sincronização do computador/dispositivo ou sua organização estiver usando SSO de conexão remota.
   1. Clique em **Adicionar** para inserir as credenciais do administrador corporativo.

1. Na página **Sistemas operacionais do dispositivo**, selecione os sistemas operacionais usados pelos dispositivos no ambiente do Active Directory e clique em **Avançar**.

   ![Sistema operacional do dispositivo](./media/hybrid-azuread-join-federated-domains/17.png)

1. Na página **Configuração de federação**, insira as credenciais do administrador do AD FS e clique em **Avançar**.

   ![Configuração de federação](./media/hybrid-azuread-join-federated-domains/18.png)

1. Na página **Pronto para configurar**, clique em **Configurar**.

   ![Pronto para configurar](./media/hybrid-azuread-join-federated-domains/19.png)

1. Na página **Configuração completa**, clique em **Sair**.

   ![Configuração concluída](./media/hybrid-azuread-join-federated-domains/20.png)

## <a name="enable-windows-down-level-devices"></a>Habilitar dispositivos de nível inferior do Windows

Se alguns dos seus dispositivos ingressados no domínio forem dispositivos de nível inferior do Windows, será necessário:

- Definir as configurações de Intranet Local para registro do dispositivo
- Instalar o Workplace Join da Microsoft em computadores Windows de nível inferior

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Definir as configurações de Intranet Local para registro do dispositivo

Para concluir com êxito o ingresso no Azure AD híbrido dos dispositivos de nível inferior do Windows e evitar prompts de certificado quando os dispositivos autenticarem no Azure AD, envie uma política por push aos dispositivos ingressados no domínio para adicionar as seguintes URLs à zona da Intranet Local no Internet Explorer:

- `https://device.login.microsoftonline.com`
- Serviço de Token de Segurança da organização (STS - domínios federados)
- `https://autologon.microsoftazuread-sso.com` (para SSO Contínuo).

Além disso, é necessário habilitar **Permitir atualizações na barra de status via script** na zona da Intranet Local do usuário.

### <a name="install-microsoft-workplace-join-for-windows-down-level-computers"></a>Instalar o Workplace Join da Microsoft em computadores Windows de nível inferior

Para registrar dispositivos de nível inferior do Windows, as organizações devem instalar o [Microsoft Workplace Join para computadores não Windows 10](https://www.microsoft.com/download/details.aspx?id=53554) disponível no Centro de Download da Microsoft.

É possível implantar o pacote usando um sistema de distribuição de software como o [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). O pacote dá suporte às opções de instalação silenciosa padrão com o parâmetro quiet. O atual branch do Configuration Manager oferece benefícios adicionais em relação às versões anteriores, como a capacidade de rastrear registros concluídos.

O instalador cria uma tarefa agendada no sistema que é executada no contexto do usuário. A tarefa é disparada quando o usuário entra no Windows. A tarefa ingressa silenciosamente o dispositivo com Azure AD com as credenciais do usuário após a autenticação no Azure AD.

## <a name="verify-the-registration"></a>Verificar o registro

Para verificar o estado do registro do dispositivo no locatário do Azure, é possível usar o cmdlet **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** no **[módulo do PowerShell do Azure Active Directory](/powershell/azure/install-msonlinev1?view=azureadps-2.0)** .

Ao usar o cmdlet **Get-MSolDevice** para verificar os detalhes do serviço:

- É necessário que haja um objeto com a **identificação do dispositivo** correspondente à ID no cliente do Windows.
- O valor para **DeviceTrustType** deverá ser **Ingressado no Domínio**. Isso equivale ao estado **ingressado no Azure AD híbrido** na página Dispositivos no portal do Azure AD.
- O valor para **Habilitado** deverá ser **True** e **DeviceTrustLevel** deve ser **Gerenciado** para dispositivos usados em acesso condicional.

**Para verificar os detalhes do serviço:**

1. Abra o **Windows PowerShell** como administrador.
1. Digite `Connect-MsolService` para conectar o locatário do Azure.  
1. Digite `get-msoldevice -deviceId <deviceId>`.
1. Verifique se **Habilitado** está definido como **Verdadeiro**.

## <a name="troubleshoot-your-implementation"></a>Solucionar problemas de implementação

Se estiver tendo problemas para concluir o ingresso do Azure AD híbrido para dispositivos Windows ingressados no domínio, consulte:

- [Solucionar problemas de ingresso no Azure AD Híbrido para dispositivos atuais do Windows](troubleshoot-hybrid-join-windows-current.md)
- [Solucionar problemas de ingresso no Azure AD Híbrido para dispositivos de nível inferior do Windows](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre como gerenciar identidades de dispositivos no portal do Azure AD, confira [como gerenciar identidades de dispositivos usando o portal do Azure](device-management-azure-portal.md).

<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png

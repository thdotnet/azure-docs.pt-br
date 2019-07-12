---
title: Configurar o logon sem senha do Active Directory do Azure (versão prévia)
description: Habilitar o logon sem senha no AD do Azure usando chaves de segurança de FIDO2 ou o aplicativo Microsoft Authenticator (visualização)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ba2545467ebfbd032408aeee25b82b92a628f2a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712066"
---
# <a name="enable-passwordless-sign-in-for-azure-ad-preview"></a>Habilitar o logon sem senha no AD do Azure (visualização)

## <a name="requirements"></a>Requisitos

* Autenticação Multifator do Azure
* Combinados a visualização de registro
* Visualização de chave de segurança FIDO2 requer chaves de segurança FIDO2 compatíveis
* WebAuthN requer o Microsoft Edge no Windows 10 versão 1809 ou superior
* FIDO2 requer o logon de Windows com base no Azure AD ingressados no Windows 10 versão 1809 ou superior

## <a name="prepare-devices-for-preview"></a>Preparar os dispositivos para visualização

Dispositivos que será um piloto com devem estar executando o Windows 10 versão 1809 ou superior. É a melhor experiência no Windows 10 versão 1903 ou superior.

## <a name="enable-security-keys-for-windows-sign-in"></a>Habilitar as chaves de segurança para o logon no Windows

As organizações podem optar por usar um ou mais dos métodos a seguir para habilitar o uso de chaves de segurança para o Windows entrar.

### <a name="enable-credential-provider-via-intune"></a>Habilitar o provedor de credenciais por meio do Intune

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Navegue até **Microsoft Intune** > **registro de dispositivo** > **registro do Windows** > **Windows Hello para empresas** > **propriedades**.
1. Sob **as configurações** defina **usar chaves de segurança para entrar no** para **habilitado**.

Configuração de chaves de segurança para entrar, não é dependente de configurar o Windows Hello para empresas.

#### <a name="enable-targeted-intune-deployment"></a>Habilitar implantação direcionada do Intune

Para direcionar grupos de dispositivos específicos para habilitar o provedor de credenciais, use as seguintes configurações personalizadas por meio do Intune. 

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Navegue até **Microsoft Intune** > **configuração do dispositivo** > **perfis** > **criar perfil**.
1. Configurar o novo perfil com as seguintes configurações
   1. Nome: Chaves de segurança para entrar no Windows
   1. Descrição: Permite que as chaves de segurança FIDO a serem usados durante o logon do Windows no
   1. Plataforma: Windows 10 e posterior
   1. Tipo de plataforma: Personalizado
   1. Configurações de OMA-URI personalizadas:
      1. Nome: Ativar as chaves de segurança FIDO para entrar no Windows
      1. OMA-URI: ./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      1. Tipo de dados: Inteiro
      1. Valor: 1 
1. Esta política pode ser atribuída a usuários específicos, dispositivos ou grupos. Mais informações podem ser encontradas no artigo [atribuir perfis de usuário e dispositivo no Microsoft Intune](https://docs.microsoft.com/intune/device-profile-assign).

![Criação de política de configuração de dispositivo personalizado do Intune](./media/howto-authentication-passwordless-enable/intune-custom-profile.png)

### <a name="enable-credential-provider-via-provisioning-package"></a>Habilitar o provedor de credenciais por meio do pacote de provisionamento

Para dispositivos não gerenciados pelo Intune, um pacote de provisionamento pode ser instalado para habilitar a funcionalidade. O aplicativo Windows Configuration Designer pode ser instalado do [Microsoft Store](https://www.microsoft.com/store/apps/9nblggh4tx22).

1. Inicie o Designer de configuração do Windows.
1. Selecione **arquivo** > **novo projeto**.
1. Dê um nome ao seu projeto e observe o caminho onde o projeto é criado.
1. Selecione **Avançar**.
1. Deixe **pacote de provisionamento** selecionado como o **fluxo de trabalho de projeto selecionado** e selecione **próxima**.
1. Selecione **edições de área de trabalho do Windows todos os** sob **escolher quais configurações para exibir e configurar** e selecione **próxima**.
1. Selecione **Concluir**.
1. No seu projeto recém-criado, navegue até **configurações de tempo de execução** > **WindowsHelloForBusiness** > **SecurityKeys**  >  **UseSecurityKeyForSignIn**.
1. Definir **UseSecurityKeyForSignIn** à **habilitado**.
1. Selecione **exportar** > **pacote de provisionamento**
1. Mantenha os padrões na **construir** janela sob **descrevem o pacote de provisionamento** e selecione **próxima**.
1. Mantenha os padrões no **construir** janela sob **selecionar detalhes de segurança para o pacote de provisionamento** e selecione **próxima**.
1. Anote ou alterar o caminho na **construir** windows sob **selecione onde salvar o pacote de provisionamento** e selecione **próxima**.
1. Selecione **compilar** sobre o **criar o pacote de provisionamento** página.
1. Salve os dois arquivos criados (ppkg e cat) para um local onde você pode aplicá-las às máquinas mais tarde.
1. Siga as orientações neste artigo [se aplicam a um pacote de provisionamento](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package)para aplicar o pacote de provisionamento que você criou.

## <a name="obtain-fido2-security-keys"></a>Obter chaves de segurança FIDO2

Consulte a seção chaves de segurança FIDO2, no artigo [What ' s sem senha?](concept-authentication-passwordless.md) para obter mais informações sobre chaves com suporte e fabricantes.

> [!NOTE]
> Se você comprar e planeja usar NFC com base em chaves de segurança você precisará de um leitor de NFC com suporte.

## <a name="enable-passwordless-authentication-methods"></a>Habilitar métodos de autenticação sem senha

### <a name="enable-the-combined-registration-experience"></a>Habilitar a experiência de registro combinado

Recursos de registro para chaves de segurança FIDO2 contam com a visualização de registro combinado. Siga as etapas abaixo para habilitar a visualização de registro combinado.

1. Entre no [Portal do Azure](https://portal.azure.com)
1. Navegue até **do Azure Active Directory** > **as configurações do usuário**
   1. Clique em **gerenciar as configurações de recursos de visualização do painel de acesso**
   1. Sob **os usuários podem usar recursos de visualização para registrar e gerenciar informações de segurança - aprimorada**.
      1. Escolher **selecionados** e escolha um grupo de usuários que participará na visualização.
      1. Ou escolha **todos os** habilitar para todos em seu diretório.
1. Clique em **Salvar**

### <a name="enable-new-passwordless-authentication-methods"></a>Habilitar novos métodos de autenticação sem senha

1. Entre no [Portal do Azure](https://portal.azure.com)
1. Navegue até **Azure Active Directory** > **métodos de autenticação** > **política de método de autenticação (versão prévia)**
1. Em cada **método**, escolha as opções a seguir
   1. **Habilitar** – Sim ou não
   1. **Destino** -todos os usuários ou selecione os usuários
1. **Salvar** cada método

> [!WARNING]
> O FIDO2 "Políticas de restrição de chave" não ainda funciona. Essa funcionalidade estará disponível antes da disponibilidade geral, não altere essas políticas de padrão.

> [!NOTE]
> Você não precisa aceitar os métodos sem senha (se você quiser visualizar apenas um método sem senha, você pode permitir apenas esse método). Recomendamos que você experimente os dois métodos, pois ambos têm seus próprios benefícios.

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Registro do usuário e o gerenciamento de chaves de segurança FIDO2

1. Navegue até [https://myprofile.microsoft.com](https://myprofile.microsoft.com)
1. Assinar se ainda não já
1. Clique em **informações de segurança**
   1. Se o usuário já tem pelo menos um método de autenticação multifator do Azure registrado, eles podem registrar imediatamente uma chave de segurança FIDO2.
   1. Se eles não tiverem pelo menos um método de autenticação multifator do Azure registrado, eles devem adicionar um.
1. Adicionar uma chave de segurança FIDO2 clicando **o método Add** e escolhendo **chave de segurança**
1. Escolher **dispositivo USB** ou **dispositivo NFC**
1. Ter sua chave prontos e escolha **Avançar**
1. Uma caixa aparecem e pedir que você crie/inserir um PIN para sua chave de segurança e executar o gesto necessário para sua chave biométrica ou touch.
1. Você será retornado para a experiência de registro combinado e solicitado a fornecer um nome significativo para seu token para que você pode identificar qual delas se você tiver várias. Clique em **Avançar**.
1. Clique em **feito** para concluir o processo

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Gerenciar chave de segurança biométrica, PIN, ou redefinir a chave de segurança

* Windows 10 versão 1809
   * Software complementar do fornecedor de chave de segurança é necessário
* Windows 10 versão 1903 ou superior
   * Os usuários podem abrir **configurações do Windows** em seu dispositivo > **contas** > **chave de segurança**
   * Os usuários podem alterar o PIN, atualizar biometria ou redefinir sua chave de segurança

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Registro do usuário e o gerenciamento de aplicativo Microsoft Authenticator

Para configurar o aplicativo Microsoft Authenticator para entrada por telefone, siga as diretrizes neste artigo [entrar em suas contas usando o aplicativo Microsoft Authenticator](../user-help/user-help-auth-app-sign-in.md).

## <a name="sign-in-with-passwordless-credentials"></a>Entrar com credenciais sem senha

### <a name="sign-in-at-the-lock-screen"></a>Entrar na tela de bloqueio

No exemplo abaixo, um usuário Bala Sandhu já tenha provisionado sua chave de segurança FIDO2. Bala pode escolher o provedor de credenciais de chave de segurança da tela de bloqueio do Windows 10 e insira a chave de segurança para entrar no Windows.

![Chave de segurança entrar na tela de bloqueio do Windows 10](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="sign-in-on-the-web"></a>Entrar na web

No exemplo abaixo, um usuário já tenha provisionado sua chave de segurança FIDO2. O usuário pode optar por entrar na web com sua chave de segurança FIDO2 dentro do navegador Microsoft Edge no Windows 10 versão 1809 ou superior.

![Entrada de chave de segurança no Microsoft Edge](./media/howto-authentication-passwordless-enable/fido2-windows-10-1903-edge-sign-in.png)

Para obter informações sobre a assinatura usando o aplicativo Microsoft Authenticator, consulte o artigo [entrar em suas contas usando o aplicativo Microsoft Authenticator](../user-help/user-help-auth-app-sign-in.md).

## <a name="known-issues"></a>Problemas conhecidos

### <a name="fido2-security-keys"></a>Chaves de segurança FIDO2

#### <a name="security-key-provisioning"></a>O provisionamento de chave de segurança

Administrador de provisionamento e desprovisionamento de chaves de segurança não está disponível na visualização pública.

#### <a name="hybrid-azure-ad-join"></a>Ingresso no Azure AD Híbrido

Usuários WIA SSO da terceira parte confiável que usam credenciais gerenciadas como chaves de segurança FIDO2 ou entrar sem senha com o aplicativo Microsoft Authenticator precisará híbrida ingressar no Windows 10 para obter os benefícios do SSO. No entanto, chaves de segurança funcionam apenas para máquinas do Azure Active Directory ingressado no momento. É recomendável que experimentar apenas FIDO2 chaves de segurança para a tela de bloqueio do Windows nas máquinas puras Unido do Azure Active Directory. Essa limitação não se aplica para a web.

#### <a name="upn-changes"></a>Alterações UPN

Estamos trabalhando no suporte a um recurso que permite a alteração UPN no hybrid AADJ e dispositivos AADJ. Se o UPN do usuário for alterado, você não pode mais modificar FIDO2 chaves de segurança para levar isso em conta. Portanto, a única abordagem é redefinir o dispositivo e o usuário deve registrar novamente.

### <a name="authenticator-app"></a>Aplicativo autenticador

#### <a name="ad-fs-integration"></a>Integração do AD FS

Quando um usuário tiver habilitado a credencial sem senha do Microsoft Authenticator, a autenticação para esse usuário sempre enviará como padrão uma notificação de aprovação. Essa lógica impede que os usuários em um locatário híbrido que está sendo direcionado para o ADFS para verificação de entrada sem que o usuário tirar uma etapa adicional para clique em "Usar sua senha em seu lugar." Esse processo também ignorará quaisquer políticas de Acesso Condicional locais e fluxos de autenticação de Passagem. A exceção a esse processo é se um login_hint for especificado, um usuário será AutoEncaminhada ao AD FS e ignorar a opção de usar a credencial sem senha.

#### <a name="azure-mfa-server"></a>Servidor MFA do Azure

Os usuários finais que estão habilitados para MFA por meio do servidor de MFA do Azure da organização local ainda pode criar e usar uma entrada de telefone sem senha na credencial. Se o usuário tentar atualizar várias instalações (+5) do Microsoft Authenticator com a credencial, essa alteração poderá resultar em um erro.  

#### <a name="device-registration"></a>Registro de dispositivos

Um dos pré-requisitos para criar essa nova credencial forte é que o dispositivo no qual ela reside esteja registrado no locatário do Azure AD para um usuário individual. Devido às restrições de registro de dispositivo, um dispositivo só pode ser registrado em um único locatário. Esse limite significa que apenas uma conta corporativa ou de estudante no aplicativo Microsoft Authenticator pode ser habilitada para entrada por telefone.

## <a name="next-steps"></a>Próximas etapas

[Saiba mais sobre registro de dispositivo](../devices/overview.md)

[Saiba mais sobre a Autenticação Multifator do Azure](../authentication/howto-mfa-getstarted.md)

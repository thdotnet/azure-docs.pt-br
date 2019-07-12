---
title: Noções básicas sobre um mundo sem senhas com o Azure Active Directory | Microsoft Docs
description: Este guia Ajuda CEOs, CIOs, CISOs, arquitetos de identidade principais, arquitetos corporativos e segurança e tomadores de decisão de TI responsáveis por escolher um método de autenticação sem senha para sua implementação do Active Directory do Azure.
services: active-directory
keywords: passwordless, azuread
author: martincoetzer
ms.author: martinco
ms.date: 07/09/2019
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: 00f66b6010bead3de131095a47ba1e419d2511c0
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723435"
---
# <a name="a-world-without-passwords-with-azure-active-directory"></a>Um mundo sem senhas com o Azure Active Directory

É hora de dividir sua relação com senhas. As senhas tenham sido boas para nós no passado, mas no espaço de trabalho digital hoje que eles se tornaram um vetor de ataque relativamente fácil para os hackers. Os hackers adoram senhas e não é difícil ver por que quando você considera que mais comumente rejeitadas senhas no Azure Active Directory (Azure AD) incluem termos como o ano, mês, a temporada ou sports se um local. Além disso, [pesquisa mostrou](https://aka.ms/passwordguidance) que recomendações tradicionais para gerenciamento de senhas como requisitos de comprimento, requisitos de complexidade e as frequências de alteração contraproducentes para uma variedade de razões relacionadas à natureza humana.

Três tipos de ataques comumente usados para comprometer contas de usuário são spray de senha, phishing e violações de reprodução. Recursos do Azure AD, como [bloqueio inteligente](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-smart-lockout), [senhas banidas](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises), e [proteção por senha](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) pode ajudar a proteger contra esses tipos de ataques. Da mesma forma, implementando [a autenticação multifator](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) (MFA), ou a verificação em duas etapas, fornece segurança adicional, exigindo uma segunda forma de autenticação. Mas a longo prazo, uma solução sem senha é a melhor solução para garantir que o método mais seguro de autenticação.

Este artigo é o início de sua jornada para ajudá-lo a compreender e implementar soluções sem senha da Microsoft e ajudam que você a escolher entre uma ou mais das seguintes opções:

* **Windows Hello para empresas**. No Windows 10, Windows Hello para empresas substitui as senhas com autenticação de dois fatores forte em PCs e dispositivos móveis. Essa autenticação consiste em um novo tipo de credencial de usuário que está associada a um dispositivo e usa um PIN ou biometria.

* **Entrar sem senha com o Microsoft Authenticator**. O aplicativo Microsoft Authenticator pode ser usado para entrar em uma conta do Azure AD sem usar uma senha. Assim como a tecnologia do Windows Hello para empresas, o Microsoft Authenticator usa autenticação baseada em chave para habilitar uma credencial de usuário que estiver associada a um dispositivo e usa biometria ou PIN.

* **As chaves de segurança FIDO2**. FIDO2 fornece credenciais de logon criptográficas que são exclusivas em cada site e são armazenadas em um dispositivo local, como o Windows Hello ou chaves de segurança externas. Essas chaves de segurança são resistentes aos riscos de phishing, roubo de senha e ataques de repetição. A solução combinada com a verificação de usuário por meio de biometria ou PIN, é duas necessidades de segurança modernas de reunião de verificação de fator.

## <a name="the-future-of-passwordless-authentication"></a>O futuro da autenticação sem senha

Hoje em dia, bancos, empresas de cartão de crédito e outras organizações e serviços online geralmente protegem sua conta, exigindo que você verificar sua identidade duas vezes: uma vez usando sua senha, em seguida, novamente por telefone, texto ou um aplicativo. Enquanto a autenticação multifator aborda o problema de segurança de senhas que estão sendo compartilhadas, roubadas ou adivinhadas, ele não aborda o fator de transtorno de tentar se lembrar delas. O que deseja que os usuários e organizações na era da nuvem de hoje é métodos de autenticação sem senha que são altamente seguros *e* conveniente.

![Segurança do vs de conveniência](./media/azure-ad/azure-ad-pwdless-image1.png)

Windows Hello para negócio, sem senha entrar com o Microsoft Authenticator e chaves de segurança de FIDO2 todos compartilham uma arquitetura simple, é comum que oferecem aos usuários um método de autenticação que é altamente seguro e conveniente de usar. Todos os três são baseados na tecnologia de chave pública/privada, necessidade de um gesto de local, como biometria ou PIN e chaves privadas associadas a um único dispositivo e com segurança armazenados e nunca compartilhado.

## <a name="windows-hello-for-business"></a>Windows Hello for Business

No Windows 10, Windows Hello para empresas substitui as senhas com autenticação de dois fatores forte em PCs e dispositivos. A autenticação consiste em um novo tipo de credencial do usuário que estiver associado a um dispositivo e usa um gesto biométrico ou um PIN que permite que os usuários autentiquem no AD do Azure, bem como um Active Directory no local. Simplesmente assinatura em um dispositivo usando o Windows Hello para empresas é fácil. Você usar um PIN ou a biométrico gesto, como um reconhecimento facial ou impressão digital.

### <a name="windows-hello-for-business-scenarios"></a>Windows Hello para cenários de negócios

Windows Hello para empresas é ideal para os operadores de informações que têm seus próprios PCs Windows designado. A biométrica e as credenciais estão diretamente vinculadas ao PC do usuário, o que impede o acesso de qualquer pessoa que não seja o proprietário. Com a integração de PKI e o suporte interno para logon único (SSO), Windows Hello para empresas fornece um método simples e conveniente para acessar diretamente recursos corporativos locais e na nuvem.

### <a name="windows-hello-for-business-deployment-considerations"></a>Windows Hello para considerações de implantação de negócios

Windows Hello para empresas é um sistema distribuído que usa vários componentes para realizar o registro de dispositivo, o provisionamento e autenticação. Portanto, a implantação requer planejamento adequado em várias equipes dentro da organização. O Windows Hello para empresas [guia de planejamento](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) pode ser usado para ajudá-lo a tomar decisões sobre o tipo do Windows Hello para implantação de negócios e as opções que você precisará considerar.

Para implantações locais ou híbrida, espera-se que você tenha:

* Uma rede bem conectada, em funcionamento

* Acesso à Internet

* Servidor de autenticação multifator para dar suporte a MFA durante o Windows Hello para o provisionamento de negócios

* Resolução de nome apropriada, nomes internos e externos

* Active Directory e um número adequado de controladores de domínio por site para dar suporte à autenticação

* Active Directory Certificate Services 2012 ou posteriores

* Um ou mais computadores de estação de trabalho que executam o Windows 10, versão 1903

Já que todos os tipos do Windows Hello para implantações de negócios, use certificados emitidos pela empresa para controladores de domínio como uma raiz de confiança, você será deseja renovar automaticamente certificados expirados por [Configurando o registro automático para o servidor e do usuário certificados](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/configure-server-certificate-autoenrollment). Para implantações locais, o provedor de identidade é o servidor local que executa a função do Windows Server Active Directory Federation Services (AD FS). Normalmente, os sistemas federados exigem uma matriz com balanceamento de carga de servidores, conhecida como um farm. Esse farm é configurado em uma rede interna e topologia de rede de perímetro para garantir a alta disponibilidade para as solicitações de autenticação.

Quando você definir uma política de grupo para exigir Windows Hello para empresas no local de trabalho, convém preparar usuários em sua organização, explicando como usar o Windows Hello. Por exemplo, quando alguém configura um novo dispositivo, ele é solicitado a escolher entre **este dispositivo pertence à minha organização** ou **esse é meu próprio dispositivo pessoal**. Para dispositivos corporativos, eles devem selecionar o primeiro. Os usuários também precisarão ser informado de qual opção de conexão, eles devem selecionar: **Associação ao Azure AD** ou **configurar uma conta local (ingresso no domínio posteriormente)** .

É comum para os usuários que estão acostumados a usar um gesto biométrico para fazer autenticação dia após dia para eventualmente se esquecer o PIN. Para evitar chamadas à assistência técnica, é recomendável que você fornecer aos usuários a capacidade de redefinir esquecido [senhas](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment) e [PINs](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-features#pin-reset).

Há muitas opções que você pode escolher ao implantar o Windows Hello para empresas. Fornecendo várias opções garante que quase todas as organizações podem implantar o Windows Hello para empresas. Considere os seguintes tipos de implantações que têm suporte:

* [Implantação de confiança de chave do Azure AD híbrido adicionado](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-key-trust)

* [Implantação de certificados de confiança do Azure AD híbrido adicionado](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cert-trust)

* [Guias de implantação de logon único do ingresso do Azure AD](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso)

* [Na implantação de confiança de chave local](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-key-trust)

* [Certificado de confiança implantação local](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-cert-trust)

Fornece muitas opções faz com que a implantação pareça complexo. No entanto, a maioria das organizações provavelmente irá determinar que eles já implementou a maioria da infraestrutura do qual o Windows Hello for Business deployment depende. Independentemente disso, é importante entender que o Windows Hello para empresas é um sistema distribuído e o planejamento adequado é recomendado.

É recomendável que você leia [planejamento de um Windows Hello for Business Deployment](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) para ajudá-lo a decidir sobre o modelo de implantação melhor adequada para sua organização específica. Em seguida, com base em fazer o planejamento, consulte à [Windows Hello for Business Deployment Guide](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide) para ajudar a garantir uma implantação bem-sucedida do Windows Hello para empresas em seu ambiente existente.

### <a name="how-windows-hello-for-business-works"></a>Como o Windows Hello for Business funciona

#### <a name="user-sets-up-windows-hello-for-business"></a>Usuário configura o Windows Hello para empresas

Após uma verificação inicial em duas etapas do usuário durante o registro, o Windows Hello está configurado no dispositivo do usuário e Windows solicita que o usuário defina um gesto, que pode ser um biométrica, como um reconhecimento facial ou impressão digital ou um PIN. Uma vez definido, o usuário fornece o gesto para verificar sua identidade. Windows, em seguida, usa o Windows Hello para autenticar usuários.

Com base nos recursos do seu dispositivo Windows 10, você terá que um enclave seguro internos, conhecido como um TPM de software ou hardware trusted platform module (TPM). O TPM armazena a chave privada, o que requer sua face, impressão digital ou PIN para desbloqueá-lo. Os dados biométricos não usam perfis móveis e nunca são enviados para servidores ou dispositivos externos. Não há nenhum ponto de coleção única, que um invasor pode comprometer para roubar dados biométricos, como o Windows Hello só armazena dados de identificação biométrica no dispositivo.

> [!TIP]
> Na superfície de um PIN parece com uma senha, mas é realmente mais segura. Uma diferença importante entre uma senha e um PIN é que o PIN está vinculado ao dispositivo específico no qual ele foi configurado. Alguém que rouba sua senha pode entrar em sua conta em qualquer lugar. Mas se eles roubam seu PIN, eles teriam de roubar seu dispositivo físico muito! Além disso, uma vez que um PIN é local para o dispositivo, ele não é transmitido em qualquer lugar; não pode ser interceptada durante a transmissão ou roubado de um servidor.

#### <a name="user-using-windows-hello-for-business-for-sign-in"></a>Usuário usando o Windows Hello para empresas para entrar

Usar o Windows Hello para biometria de negócios e PINs (pública/privada chave assimétrica) criptografia para autenticação. Durante a autenticação, a criptografia está vinculada para a chave de sessão TLS, que protege o processo de autenticação para que um ataque (MiTM) de man-in-the-middle não poderá roubar o token de segurança resultante ou o artefato e reproduzi-lo de outros locais.

Windows Hello para empresas oferece uma experiência de entrada conveniente que autentica o usuário no AD do Azure e recursos do Active Directory. Dispositivos que ingressaram no AD do Azure se autenticar no Azure durante a entrada e, opcionalmente, podem se autenticar no Active Directory. Dispositivos do Azure Active Directory ingressado híbrido autentiquem no Active Directory durante o logon e autentiquem para o Azure Active Directory em segundo plano.

![Consulte a imagem de origem](./media/azure-ad/azure-ad-pwdless-image2.jpeg)

As etapas a seguir ilustram a autenticação de entrada para o Azure Active Directory.

![Tela de bloqueio do Windows 10](./media/azure-ad/azure-ad-pwdless-image3.png)

1. Usuário se autentica no Windows usando biométrica ou um gesto PIN. O gesto desbloqueia o Windows Hello para a chave privada de negócios e é enviado para o provedor de suporte de segurança de autenticação de nuvem, conhecido como o provedor do AP da nuvem.

2. O provedor de nuvem AP solicita um nonce do Active Directory do Azure.

3. O Azure AD retorna um valor de uso único que seja válida por 5 minutos.

4. O provedor de nuvem AP assina o nonce usando a chave privada do usuário e retorna o nonce assinado para o Azure Active Directory.

5. O Azure Active Directory valida o nonce assinado usando a chave do usuário registrado com segurança pública em relação a assinatura de nonce. Depois de validar a assinatura, o Azure AD, em seguida, valida o nonce assinado retornado. Depois de validar o nonce, o Azure AD criará um PRT com chave de sessão que é criptografado como chave de transporte do dispositivo e retorna-a para o provedor do AP da nuvem.

6. O provedor de nuvem AP recebe o PRT criptografado com chave de sessão. Usando a chave do dispositivo transporte particular, o provedor de nuvem AP descriptografa a chave de sessão e protege a chave de sessão usando o TPM do dispositivo.

7. O provedor de nuvem AP retorna uma resposta de autenticação bem-sucedida para o Windows após o qual o usuário é capaz de acessar o Windows, bem como a nuvem e aplicativos sem precisar se autenticar novamente no local (SSO).

Para uma análise mais profunda o processo de autenticação em outros cenários que envolvem a Windows Hello para empresas, consulte [Windows Hello for Business e autenticação](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-how-it-works-authentication#Azure-AD-join-authentication-to-Active-Directory-using-a-Key).

#### <a name="user-manages-their-windows-hello-for-business-credentials"></a>Usuário gerencia seus Windows Hello para credenciais de negócios

O [serviços de redefinição de PIN da Microsoft](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-features#pin-reset) é um recurso do Azure AD que permite aos usuários redefinir o PIN, se necessário. Usando a diretiva de grupo, Microsoft Intune ou um MDM compatível, um administrador pode configurar dispositivos Windows 10 para usar com segurança o serviço de redefinição de PIN da Microsoft que permite aos usuários redefinir o PIN esquecido por meio das configurações ou acima da tela de bloqueio sem a necessidade de novo registro.

Às vezes, os usuários precisam voltar a usar senhas. [Redefinição de senha de autoatendimento](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment) (SSPR) é outro recurso do AD do Azure que permite que os usuários redefinam suas senhas sem a necessidade de entrar em contato com a equipe de TI. Os usuários devem se registrar para ou ser registrados para autoatendimento a redefinição de senha antes de usar o serviço. Durante o registro, o usuário escolhe um ou mais métodos de autenticação habilitados pela sua organização. SSPR permite aos usuários obter o desbloqueio rapidamente e continuar trabalhando, independentemente de onde eles estão ou a hora do dia. Permitindo que os usuários desbloquear a mesmos, sua organização pode reduzir o tempo não produtivas e custos de suporte de alta dos problemas mais comuns relacionados à senha.

## <a name="passwordless-sign-in-with-microsoft-authenticator"></a>Entrar sem senha com o Microsoft Authenticator

Entrar sem senha com o Microsoft Authenticator é outra solução sem senha pode ser usada para entrar em contas do AD do Azure usando a entrada pelo telefone no. Você ainda deve verificar sua identidade, fornecendo a algo que você sabe e algo que você tem, mas entrar permite que você ignore a inserir sua senha e executa sua verificação de identidade em seu dispositivo móvel usando sua impressão digital, face ou PIN do telefone.

### <a name="microsoft-authenticator-passwordless-scenarios"></a>Cenários sem senha do Microsoft Authenticator

O aplicativo Microsoft Authenticator permite que os usuários verifiquem sua identidade e se autenticar em sua conta corporativa ou pessoal. Ele também pode ser usado para incrementar uma senha com uma senha única ou notificação por push ou substitui a necessidade de uma senha completamente. Em vez de usar uma senha, os usuários confirmar sua identidade usando o telefone celular, por meio do exame de impressão digital, reconhecimento facial ou íris ou PIN. Criado sobre tecnologia segura semelhante a que o Windows Hello usa, essa ferramenta é empacotada em um aplicativo simples em um dispositivo móvel, tornando-uma opção conveniente para os usuários. O aplicativo Microsoft Authenticator está disponível para Android e iOS.

### <a name="microsoft-authenticator-deployment-considerations"></a>Considerações de implantação do Microsoft Authenticator

Pré-requisitos para usar o aplicativo Microsoft Authenticator para fazer logon sem senha no AD do Azure incluem o seguinte:

* Os usuários finais estão habilitados para autenticação multifator do Azure

* A capacidade dos usuários registrar seus dispositivos usando o Microsoft Intune ou uma solução MDM (gerenciamento) do dispositivo móvel de terceiros

Supondo que esses requisitos forem atendidos, os administradores habilitar telefone sem senha entrar no locatário usando [Windows PowerShell.](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-phone-sign-in#enable-my-users) Depois que a entrada pelo telefone no está habilitada no locatário, os usuários finais pode optar por entrar usando seu telefone, selecionando sua conta corporativa ou de estudante na **contas** tela do aplicativo, em seguida, selecionando **Habilitar entrada pelo telefone** .

Os usuários finais supondo que entrar sem senha está habilitado por um administrador, será necessário atender aos seguintes requisitos:

* Registrados na autenticação multifator do Azure

* Versão mais recente do Microsoft Authenticator instalado em dispositivos que executam o iOS 8.0 ou posterior ou Android 6.0 ou superior

* Conta corporativa ou escolar com notificações por push adicionada ao aplicativo

Para evitar a possibilidade de bloqueio fora de sua conta ou a recriação contas em um novo dispositivo, é recomendável que você use o Microsoft Authenticator para [fazer backup de suas credenciais de conta](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-backup-recovery) para a nuvem. Após o backup, também será possível usar o aplicativo para recuperar as informações em um novo dispositivo, potencialmente evitando o bloqueio ou a recriação de contas.

Como a maioria dos usuários estão acostumados a usar apenas senhas para autenticar, é importante que sua organização educa os usuários sobre esse processo. Conscientização pode reduzir a probabilidade de que os usuários chamar o suporte técnico para qualquer [problemas](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-phone-sign-in#known-issues) relacionados à entrada usando o aplicativo Microsoft Authenticator.

> [!NOTE]
> Um ponto de falha para esta solução potencial é quando o roaming de um usuário estiver em um local em que não há nenhuma conectividade com a Internet. As chaves de segurança FIDO2 e Windows Hello para empresas não estão sujeitos a mesma limitação.

### <a name="how-passwordless-sign-in-with-microsoft-authenticator-works"></a>Entrar como sem senha com Microsoft Authenticator funciona

#### <a name="user-sets-up-passwordless-sign-in-with-microsoft-authenticator"></a>Usuário configura entrar sem senha com o Microsoft Authenticator

Antes do aplicativo Microsoft Authenticator pode ser usado como uma solução sem senha para entrar uma conta do AD do Azure, as etapas devem ser executadas por um administrador e os usuários finais.

Primeiro, um administrador precisará [habilitar o uso do aplicativo como uma credencial](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-phone-sign-in#enable-my-users) no locatário usando o Windows PowerShell. O administrador também será necessário permitir que usuários finais para Azure a autenticação multifator (MFA do Azure) e configurar o aplicativo Microsoft Authenticator como uma da [métodos de verificação](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-mfasettings#verification-methods).

Os usuários finais precisarão [Baixe e instale](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install) o aplicativo Microsoft Authenticator e [configurar sua conta](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-auth-app) para usar o aplicativo Microsoft Authenticator como um dos métodos de verificação.

> [!VIDEO https://www.youtube.com/embed/uWbkLuI4g30]

#### <a name="user-using-microsoft-authenticator-for-passwordless-sign-in"></a>Usuário usando o Microsoft Authenticator para entrar sem senha

O aplicativo Microsoft Authenticator pode ser usado para entrar em qualquer conta do Azure AD sem usar uma senha. Enquanto a tela de bloqueio do Windows 10 não inclui o aplicativo Microsoft Authenticator como uma opção de entrar, os usuários podem ainda inserir seu nome de usuário e, em seguida, receber uma notificação por push em seu dispositivo móvel para verificar a presença. Os usuários confirmar sua presença por correspondência de um número na tela de entrada, em seguida, fornecendo uma verificação de detecção facial, a impressão digital ou o PIN para desbloquear a chave privada e concluir a autenticação. Esse método de verificação multifator é mais segura do que uma senha e mais convenientes do que digitar uma senha e um código.

![Autenticador entrar](./media/azure-ad/azure-ad-pwdless-image4.png)

Autenticação sem senha usando o Microsoft Authenticator segue o mesmo padrão de básico como o Windows Hello para empresas, mas é um pouco mais complicado, pois o usuário precisa ser identificadas de forma que o Azure AD pode encontrar a versão de aplicativo Microsoft Authenticator sendo usado.

![Processo de autenticador](./media/azure-ad/azure-ad-pwdless-image5.png)

1. O usuário insere seu nome de usuário.

2. Azure AD detecta que o usuário tem uma credencial forte e inicia o fluxo de credenciais fortes.

3. Notificação é enviada para o aplicativo por meio do Apple APNS Push Notification Service () em dispositivos iOS, ou por meio do Firebase Cloud Messaging (FCM) em dispositivos Android.

4. O usuário recebe a notificação por push e abre o aplicativo.

5. O aplicativo chama o Azure AD e recebe um desafio de prova de presença e nonce.

6. O usuário conclui o desafio, inserindo seus biometria ou PIN para desbloquear a chave privada.

7. O nonce é assinado com a chave privada e enviado de volta ao Azure AD.

8. Azure AD executa a validação de chave pública/privada e retorna um token.

#### <a name="user-manages-their-passwordless-sign-in-with-microsoft-authenticator-credentials"></a>Usuário gerencia seu entrar sem senha com credenciais do Microsoft Authenticator

Com o [combinados registro](https://docs.microsoft.com/azure/active-directory/authentication/concept-registration-mfa-sspr-combined), os usuários podem registrar e obter as vantagens da autenticação multifator do Azure e a redefinição de senha de autoatendimento. Os usuários se registrar e gerenciar essas configurações, navegando até seus [página Meu perfil](https://aka.ms/mysecurityinfo). Além de habilitar SSPR, combinados registro suporta vários métodos de autenticação e ações.

## <a name="fido2-security-keys"></a>Chaves de segurança FIDO2

FIDO2 é a versão mais recente do padrão de FIDO Alliance e tem dois componentes - padrão de autenticação da Web (WebAuthN) do W3C e o protocolo de cliente-autenticador FIDO Alliance (CTAP2) correspondente. Os padrões de FIDO2 habilitar os usuários aproveitem o hardware - mobile- e com base em biometria autenticadores autentiquem facilmente com muitos aplicativos e sites em ambientes móveis e da área de trabalho.

Parceiros do setor e Microsoft têm trabalhado juntas em FIDO2 dispositivos de segurança para o Windows Hello habilitar a autenticação fácil e segura em dispositivos compartilhados. As chaves de segurança FIDO2 permitem que você executar sua credencial com você e autenticar com segurança para um [do Azure AD](https://aka.ms/azuread418)-dispositivo Windows 10 ingressado no que faz parte de sua organização.

WebAuthN define uma API que permite o desenvolvimento e implementação de alta segurança, a autenticação sem senha por serviços e aplicativos web. O protocolo CTAP permite que dispositivos externos, como chaves de segurança FIDO compatíveis para trabalhar com WebAuthN e servir como autenticadores. Com a autenticação da Web, os usuários podem entrar serviços online com seus rosto, impressão digital, PIN ou chaves de segurança FIDO2 portátil, aproveitando forte credenciais de chave pública em vez de senhas. No momento WebAuthN tem suporte no Microsoft Edge e o suporte para o Chrome e Firefox está em desenvolvimento.

Dispositivos e tokens que seguem protocolos FIDO2, WebAuthN e CTAP trazem sobre uma solução de plataforma cruzada de autenticação forte, sem o uso de senhas. Parceiros da Microsoft estão trabalhando em uma variedade de fatores de formulário de chave de segurança, como chaves de segurança USB e habilitado para NFC os cartões inteligentes.

### <a name="fido2-security-keys-scenarios"></a>Cenários de chaves de segurança FIDO2

As chaves de segurança FIDO2 podem ser usadas para entrar no AD do Azure, escolhendo a chave de segurança como o provedor de credenciais na tela de bloqueio do Windows 10. Um nome de usuário ou senha não é necessário que torna uma solução ideal para primeira linha trabalhadores compartilham PCs entre vários usuários. Eles também são uma opção de autenticação excelente quando as políticas corporativas ditam que as credenciais do usuário devem ser fisicamente separadas do seu dispositivo. Os usuários também podem optar por entrar em sites da web usando sua chave de segurança FIDO2 dentro do navegador Microsoft Edge no Windows 10 versão 1809 ou superior.

### <a name="fido2-security-keys-deployment-considerations"></a>Considerações de implantação de chaves de segurança FIDO2

Os administradores podem habilitar o suporte de FIDO2 no Azure AD e atribuir a capacidade para os usuários ou grupos. As políticas também podem ser criados para como as chaves são provisionadas e configurar as restrições, como permitir ou bloquear um conjunto específico de chaves de segurança de hardware. As chaves devem ser fisicamente distribuídas aos usuários finais.

**Os requisitos para habilitar sem senha entram no AD do Azure e sites da web usando as chaves de segurança FIDO2 incluem o seguinte:**

* AD do Azure

* Autenticação Multifator do Azure

* Combinados a visualização de registro

* Visualização de chave de segurança FIDO2 requer uma chave de segurança FIDO2 compatível

* Autenticação da Web (WebAuthN) requer o Microsoft Edge no Windows 10 versão 1809 ou superior

* FIDO2 requer o logon de Windows com base no Azure AD ingressados no Windows 10 versão 1809 ou superior (a melhor experiência é no Windows 10 versão 1903 ou superior)

Fatores de forma compatível com FIDO2 incluem dispositivos USB, NFC e Bluetooth. Recomendamos que você escolha o fator de forma que atenda às suas necessidades específicas, desde algumas plataformas e navegadores ainda não estão em conformidade FIDO2.

Também recomendamos que cada organização cria um protocolo para usuários e administradores a seguir devem uma segurança chave ser perdida ou roubado. Os usuários devem relatar a chave perdida ou roubada, para que os administradores ou o usuário pode excluir suas chaves de segurança do perfil do usuário e provisionar um novo.

### <a name="how-fido2-security-keys-works"></a>Como as chaves de segurança FIDO2 funciona

#### <a name="user-sets-up-fido2-security-key"></a>Usuário define uma chave de segurança FIDO2

Embora os administradores podem [provisionar manualmente as chaves](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-enable) e distribuí-los aos usuários finais, o provisionamento e habilitar o provedor de credenciais na tela de bloqueio do Windows 10 FIDO2 terão suporte por meio de [Intune](https://docs.microsoft.com/intune/windows-enrollment-methods). Os administradores também precisam usar o [portal do Azure](https://portal.azure.com/) para permitir que dispositivos de hardware token como um método de autenticação sem senha.

Implantar chaves de segurança FIDO2 também requer que os usuários registrar suas chaves usando [combinados registro](https://docs.microsoft.com/azure/active-directory/authentication/concept-registration-mfa-sspr-combined). Com o registro combinado, os usuários registrar uma vez e obtenham as vantagens da autenticação multifator do Azure e a redefinição de senha de logon único (SSPR).

Além de selecionar o token de hardware como o método de autenticação multifator padrão, é recomendável que você também pode selecionar uma opção de verificação adicional.

* Microsoft Authenticator - notificação

* Hardware ou aplicativo autenticador de token – o código

* chamada telefônica

* mensagem de texto

#### <a name="user-using-fido2-security-key-for-sign-in"></a>Usuário usando a chave de segurança FIDO2 para entrar

FIDO2 fornece uma camada de abstração entre o fator de forma que está sendo usado como o autenticador e criptografia de chave pública/privada para habilitar autenticadores de plataforma interna, como o Windows Hello e chaves de segurança resolver para uma chave privada e fornecer uma chave pública que pode ser usado como um identificador para acessar recursos externos. As chaves de segurança FIDO2 são equipadas com seu próprios internos enclave seguro que armazena a chave privada e requer a biometria ou PIN para desbloqueá-lo. As credenciais não podem ser reutilizadas, repetido, ou compartilhados entre serviços e não estão sujeitos a phishing e ataques MiTM ou servidor violações.

![FIDO2 entrar](./media/azure-ad/azure-ad-pwdless-image6.png)

As chaves de segurança FIDO2 fornecem autenticação segura, independente do fator de formulário. A chave de segurança contém a credencial e deve ser protegida com um segundo fator adicional, como uma impressão digital (integrados a chave de segurança) ou um PIN a ser inserido ao entrar no Windows. Parceiros da Microsoft estão trabalhando em uma variedade de fatores de formulário de chave de segurança. Alguns exemplos incluem as chaves de segurança USB e NFC habilitado cartões inteligentes.

> [!NOTE]
> Uma chave de segurança deve implementar determinados recursos e extensões do protocolo a ser FIDO2 CTAP [compatível com o Microsoft](https://aka.ms/fido2securitykeys). A Microsoft testou essas soluções para compatibilidade com Windows 10 e o Azure Active Directory.

![Processo de logon FIDO2](./media/azure-ad/azure-ad-pwdless-image9.png)

1. O usuário conecta o dispositivo FIDO2 no computador.

2. Windows detecta que a chave de segurança FIDO2.

3. Windows envia uma solicitação de autenticação.

4. O Azure AD envia um nonce.

5. O usuário conclui sua gesto para desbloquear a chave privada armazenada no enclave seguro da chave de segurança FIDO2.

6. A chave de segurança FIDO2 assina o nonce com a chave privada.

7. A solicitação de token de PRT com nonce assinado é enviada para o Azure AD.

8. O Azure AD verifica o nonce assinado usando a chave pública do FIDO2.

9. O Azure AD retorna PRT para habilitar o acesso aos recursos locais.

#### <a name="user-manages-their-fido2-security-key-credentials"></a>Usuário gerencia suas credenciais de chave de segurança FIDO2

Assim como o aplicativo Microsoft Authenticator, gerenciamento de credencial para chaves de segurança FIDO2 baseia-se na experiência de registro combinado para os usuários finais.

## <a name="deciding-a-passwordless-method"></a>Decisão de um método sem senha

Escolha entre essas três opções sem senha depende da segurança da sua empresa, plataforma e requisitos de aplicativo.

Aqui estão alguns fatores a considerar ao escolher a tecnologia da Microsoft sem senha:

||**Configurar o Hello for Business**|**Entrar sem senha com o aplicativo Microsoft Authenticator**|**Chaves de segurança FIDO2**|
|:-|:-|:-|:-|
|**Pré-requisito**| Windows 10, versão 1809 ou posterior<br>Azure Active Directory| Aplicativo Microsoft Authenticator<br>Telefone (iOS e Android dispositivos que executam o Android 6.0 ou superior.)|Windows 10, versão 1809 ou posterior<br>Azure Active Directory|
|**Modo**|Plataforma|Software|Hardware|
|**Sistemas e dispositivos**|PC com uma internos Trusted Platform Module (TPM)<br>Reconhecimento de PIN e biometria |Reconhecimento de PIN e biometria no telefone|Dispositivos de segurança FIDO2 que são compatíveis com a Microsoft|
|**Experiência do usuário**|Entre usando um PIN ou a biométrico reconhecimento (facial, íris ou impressão digital) com dispositivos Windows.<br>Autenticação do Windows Hello está vinculada ao dispositivo; o usuário precisa que o dispositivo e um componente de entrada como um PIN ou a biométrica fator para acessar recursos corporativos.|Entrar usando um telefone celular com o reconhecimento de verificação, rosto ou íris de impressão digital ou FIXAR.<br>Os usuários entram no trabalho ou conta pessoal do seu PC ou telefone celular.|Entrar usando um dispositivo de segurança FIDO2 (biometria, PIN e NFC)<br>Usuário pode acessar o dispositivo com base nos controles de organização e autenticar com base no PIN, biometria de dispositivos, como chaves de segurança USB e cartões inteligentes de habilitado para NFC, chaves ou dispositivos portáteis.|
|**Cenários habilitados**| Experiência de sem senha com o dispositivo do Windows.<br>Aplicável para o computador de trabalho dedicada com capacidade para logon único para dispositivos e aplicativos.|Sem senha em qualquer lugar solução usando o telefone celular.<br>Aplicável para acessar a aplicativos pessoais na web ou trabalho de qualquer dispositivo.|Experiência sem senha para trabalhadores usando NFC, biometria e o PIN.<br>Aplicável para computadores compartilhados e onde um telefone celular não é uma opção viável (como para a equipe de suporte técnico, quiosque público ou equipe hospital)|

Use a tabela a seguir para escolher qual método dará suporte a seus requisitos e os usuários.

|Persona|Cenário|Ambiente|Tecnologia sem senha|
|:-|:-|:-|:-|
|**Admin**|Proteger o acesso a um dispositivo para tarefas de gerenciamento|Dispositivo atribuído do Windows 10|Windows Hello for Business e/ou FIDO2 chave de segurança|
|**Admin**|Tarefas de gerenciamento em dispositivos não Windows| Dispositivo móvel ou não-windows|Entrar sem senha com o aplicativo Microsoft Authenticator|
|**Operador de informações**|Trabalho de produtividade|Dispositivo atribuído do Windows 10|Windows Hello for Business e/ou FIDO2 chave de segurança|
|**Operador de informações**|Trabalho de produtividade| Dispositivo móvel ou não-windows|Entrar sem senha com o aplicativo Microsoft Authenticator|
|**Malwares enviados por trabalho**|Quiosques em uma fábrica, planta, varejo ou dados de entrada|Dispositivos compartilhados do Windows 10|Chaves de segurança FIDO2|

## <a name="getting-started"></a>Introdução

A autenticação sem senha é a onda do futuro e o caminho para um ambiente mais seguro. É recomendável que as organizações começam a planejar para que essa alteração e reduzir suas dependências em senhas. Para começar, considere as seguintes metas:

* Habilite usuários para MFA, Microsoft aplicativo autenticador + acesso condicional.

* Proteção de senha do Azure AD de distribuição + políticas de atualização.

* Permitir que os usuários para SSPR com o registro combinado.

* Implante o aplicativo Microsoft Authenticator para mobilidade.

* Implantar o Windows Hello para empresas (1903: mantenha-se atualizado).

* Implante FIDO2 dispositivos para usuários que não é possível usar telefones.

* Quando possível, desabilite a autenticação baseada em senha.

Para atingir essas metas, recomendamos a seguinte abordagem:

1. Habilite o Azure Active Directory aproveitar ao máximo de recursos, como o Azure MFA e o acesso condicional.

2. Habilite a autenticação multifator fornecer proteção adicional.

3. Habilite o autoatendimento de redefinição de senha no caso dos usuários precisam voltar a usar uma senha.

4. Implante o Microsoft Authenticator phone entrar para a mobilidade adicionada.

5. Implante o Windows Hello para empresas para todos os seus dispositivos Windows 10.

6. Prepare para FIDO2 chaves de segurança.

> [!NOTE]
> Consulte o Azure Active Directory [página de licenciamento](https://azure.microsoft.com/pricing/details/active-directory/) para obter detalhes sobre os requisitos de licenciamento para métodos sem senha.

## <a name="conclusion"></a>Conclusão

Nos últimos anos, a Microsoft continuou o compromisso com a habilitação de um mundo sem senhas. Com o Windows 10, a Microsoft introduziu Windows Hello para empresas, uma ótima, hardware protegido credencial de dois fatores que habilita o logon único em Azure Active Directory e o Active Directory. Assim como a tecnologia do Windows Hello para empresas, o aplicativo Microsoft Authenticator usa autenticação baseada em chave para habilitar uma credencial de usuário que estiver associada a um dispositivo móvel e usa biometria ou PIN. Agora as chaves de segurança FIDO2 permitem que você executar sua credencial com você e entrar no AD do Azure, escolhendo a chave de segurança como o provedor de credenciais na tela de bloqueio do Windows 10. Todos os três dessas soluções sem senha reduzem o risco de phishing, spray de senha e ataques de repetição e fornecer aos usuários uma maneira altamente segura e conveniente de entrar e acessar dados de qualquer lugar.

A adoção de tecnologias de autenticação moderna de vários fatores, como biométrica e criptografia de chave pública em dispositivos amplamente acessíveis é uma das etapas mais impactantes que podem reduzir significativamente o risco de identidade da empresa. Sem senha é uma abordagem de longo prazo para a autenticação segura, e ainda está em evolução. Devido a novos requisitos, as organizações podem preparar próprios fazendo um plano para começar a mover para tecnologias sem senha.

## <a name="next-steps"></a>Próximas etapas

* Uma visão geral de [What ' s sem senha?](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless)
* [Como habilitar sem senha no Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-enable)

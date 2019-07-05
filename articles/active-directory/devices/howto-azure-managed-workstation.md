---
title: Implantar gerenciados pelo Azure estações de trabalho - Azure Active Directory
description: Saiba como implantar estações de trabalho seguras e gerenciada pelo Azure para reduzir o risco de violação devido a configuração incorreta ou o comprometimento.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51d8bbc8b8be9679fbf024d7c51de53c430dc493
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67550478"
---
# <a name="deploy-a-secure-azure-managed-workstation"></a>Implantar uma estação de trabalho segura e gerenciada pelo Azure

Agora que você [entender as estações de trabalho seguras](concept-azure-managed-workstation.md), é hora de começar o processo de implantação. Com este guia, você pode usar perfis definidos para criar uma estação de trabalho que é mais segura desde o início.

![Implantação de uma estação de trabalho segura](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Você deve selecionar um perfil antes de implantar a solução. Você pode usar vários perfis simultaneamente em uma implantação e atribuí-los com as marcas ou grupos.
> [!NOTE]
> Aplicação de qualquer um dos perfis conforme necessário por seus requisitos. Você pode mover a outro perfil atribuindo-o no Intune.

| Perfil | Baixo | Avançado | Alto | Especializada | Protegido | Isolado |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Usuário do Azure AD | Sim | sim | sim | sim | sim | Sim |
| Intune-managed | Sim | sim | sim | sim | sim | Sim |
| Dispositivo - Azure AD registrado | Sim |  |  |  |  | |   |
| Dispositivo - ingressado no Azure AD |   | Sim | sim | sim | sim | Sim |
| Linha de base de segurança do Intune aplicada |   | Sim <br> (Avançado) | Sim <br> (HighSecurity) | Sim <br> (NCSC) | Sim <br> (Protegido) |  ND |
| Hardware atende aos padrões de 10 Windows seguro |   | Sim | sim | sim | sim | Sim |
| Microsoft Defender ATP habilitado |   | Sim  | sim | sim | sim | Sim |
| Remoção de direitos de administrador |   |   | Sim  | sim | sim | Sim |
| Implantação usando o Microsoft Autopilot |   |   | Sim  | sim | sim | Sim |
| Aplicativos instalados apenas pelo Intune |   |   |   | Sim | sim |Sim |
| Restringido a lista aprovada de URLs |   |   |   | Sim | sim |Sim |
| Internet bloqueada (entrada/saída) |   |   |   |  |  |Sim |

## <a name="license-requirements"></a>Requisitos de licença

Os conceitos abordados neste guia que você tenha o Microsoft 365 Enterprise E5 ou uma SKU equivalente. Algumas das recomendações neste guia podem ser implementadas com SKUs inferiores. Para obter mais informações, consulte [licenciamento do Microsoft 365 Enterprise](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

Para automatizar o provisionamento de licença, considere [licenciamento baseado em grupo](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) para seus usuários.

## <a name="azure-active-directory-configuration"></a>Configuração do Active Directory do Azure

Azure Active Directory (AD do Azure) gerencia os usuários, grupos e dispositivos para suas estações de trabalho do administrador. Você precisa habilitar os serviços de identidade e recursos com um [conta de administrador](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

Quando você cria a conta de administrador de estação de trabalho segura, você expõe a conta de sua estação de trabalho atual. Certifique-se de que usar um dispositivo de seguro conhecido para fazer a configuração inicial e todas as configurações globais. Para reduzir a exposição ao ataque para a experiência pela primeira vez, considere o seguinte a [orientação para evitar infecções por malware](https://docs.microsoft.com/windows/security/threat-protection/intelligence/prevent-malware-infection).

Você também deve exigir a autenticação multifator, pelo menos para seus administradores. Ver [implantar o MFA baseado em nuvem](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted) para obter diretrizes de implementação.

### <a name="azure-ad-users-and-groups"></a>Usuários e grupos do azure AD

1. No portal do Azure, navegue até **Azure Active Directory** > **usuários** > **novo usuário**.
1. Criar o administrador do dispositivo seguindo as etapas a [criar usuário tutorial](https://docs.microsoft.com/Intune/quickstart-create-user).
1. Digite:
   * **Nome** -proteger a estação de trabalho do administrador
   * **Nome de usuário** - `secure-ws-admin@identityitpro.com`
   * **Função de diretório** - **administrador limitado** e selecione o **administrador do Intune** função.
1. Selecione **Criar**.

Em seguida, crie dois grupos: os usuários da estação de trabalho e dispositivos de estação de trabalho.

No portal do Azure, navegue até **Azure Active Directory** > **grupos** > **novo grupo**.

1. O grupo de usuários da estação de trabalho, você talvez queira configurar [licenciamento baseado em grupo](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) para automatizar o provisionamento de licenças aos usuários.
1. O grupo de usuários da estação de trabalho, digite:
   * **Tipo de grupo** -segurança
   * **Nome do grupo** -Proteja usuários da estação de trabalho
   * **Tipo de associação** - atribuído
1. Adicione o usuário de administrador de estação de trabalho segura: `secure-ws-admin@identityitpro.com`
1. Você pode adicionar outros usuários que gerenciarão estações de trabalho protegidas.
1. Selecione **Criar**.

1. Para o grupo de dispositivos de estação de trabalho, digite:
   * **Tipo de grupo** -segurança
   * **Nome do grupo** -Proteja estações de trabalho
   * **Tipo de associação** - atribuído
1. Selecione **Criar**.

### <a name="azure-ad-device-configuration"></a>Configuração de dispositivo do Azure AD

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Especifique quem pode ingressar dispositivos no Azure AD

Configure os dispositivos de configuração no Active Directory para permitir que seu grupo de segurança administrativa unir dispositivos ao seu domínio. Para configurar essa configuração do portal do Azure:

1. Vá para **Azure Active Directory** > **dispositivos** > **configurações do dispositivo**.
1. Escolher **Selected** sob **os usuários podem unir dispositivos ao Azure AD**e, em seguida, selecione o grupo "Usuários de estação de trabalho segura".

#### <a name="removal-of-local-admin-rights"></a>Remoção de direitos de administrador local

Esse método requer que o VIP, DevOps e estações de trabalho de nível de segurança tenha usuários sem direitos de administrador em seus computadores. Para configurar essa configuração do portal do Azure:

1. Vá para **Azure Active Directory** > **dispositivos** > **configurações do dispositivo**.
1. Selecione **None** sob **administradores locais adicionais no AD do Azure ingressado em dispositivos**.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Exigir a autenticação multifator para unir dispositivos

Para reforçar ainda mais o processo de ingresso de dispositivos para o Azure AD:

1. Vá para **Azure Active Directory** > **dispositivos** > **configurações do dispositivo**.
1. Selecione **Yes** sob **exigir multi-Factor Auth para unir dispositivos**.
1. Clique em **Salvar**.

#### <a name="configure-mdm"></a>Configurar MDM

No Portal do Azure:

1. Navegue até **do Azure Active Directory** > **mobilidade (MDM e MAM)**  > **Microsoft Intune**.
1. Alterar o **escopo do usuário MDM** definir como **todos os**.
1. Clique em **Salvar**.

Estas etapas permitem que você gerencie qualquer dispositivo com o Intune. Para obter mais informações, consulte [Intune Quickstart: Configurar o registro automático para dispositivos Windows 10](https://docs.microsoft.com/Intune/quickstart-setup-auto-enrollment). Você pode criar políticas de configuração e a conformidade do Intune em uma etapa futura.

#### <a name="azure-ad-conditional-access"></a>Acesso condicional do Azure AD

Acesso condicional do AD do Azure pode ajudar a restringir as tarefas administrativas com privilégios para os dispositivos em conformidade. Predefinidos de membros do **Proteja usuários da estação de trabalho** grupo são necessárias para executar a autenticação multifator ao entrar em aplicativos de nuvem. Uma prática recomendada é excluir as contas de acesso de emergência da política. Para obter mais informações, consulte [gerenciar contas de acesso de emergência no Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access).

Para configurar o acesso condicional no portal do Azure:

1. Vá para **Azure Active Directory** > **acesso condicional** > **nova política**.
1. Digite:
   * **Nome** -seguro de dispositivos necessários política
   * Atribuições
     * **Usuários e grupos**
       * Incluir - **usuários e grupos** – selecione o **Proteja usuários da estação de trabalho** grupo criado anteriormente.
       * Excluir - **usuários e grupos** -selecionar contas de acesso de emergência da sua organização.
     * **Aplicativos de nuvem** -inclui **todos os aplicativos em nuvem**.
    * Controles de acesso
      * **Grant** - marque **conceder acesso** botão de opção.
        * **Exigir autenticação multifator**.
        * **Exigir que o dispositivo seja marcado como em conformidade**.
        * Para vários controles - **exigir todos os controles selecionados**.
    * Habilitar política – **em**.

Você tem a opção de criar políticas que bloqueiam os países em que os usuários não acessaria os recursos da empresa. Para obter mais informações sobre políticas de acesso condicional com base no local do IP, consulte [a condição de localização no acesso condicional do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition).

## <a name="intune-configuration"></a>Configuração do Intune

### <a name="configure-enrollment-status"></a>Configurar o status do registro

É importante garantir que sua estação de trabalho segura é um dispositivo limpo confiável. Ao comprar novos dispositivos, você pode insistir que eles são o conjunto de fábrica para [Windows 10 Pro no modo S](https://docs.microsoft.com/Windows/deployment/Windows-10-pro-in-s-mode), que limita a exposição a vulnerabilidades durante o gerenciamento da cadeia de fornecedores. Depois de receber um dispositivo de seu fornecedor, você pode usar o Autopilot para alterá-lo de modo S. As diretrizes a seguir fornecem detalhes sobre como aplicar o processo de transformação.

Para garantir que os dispositivos são totalmente configurados antes do uso, o Intune fornece um meio **bloquear o uso do dispositivo até que todos os perfis e aplicativos sejam instalados**.

No **portal do Azure**:
1. Vá para **Microsoft Intune** > **registro de dispositivo** > **registro do Windows** > **Status do registro Página** > **padrão** > **configurações**.
1. Definir **mostrar o progresso de instalação de perfil de aplicativo** à **Sim**.
1. Definir **bloquear o uso do dispositivo até que todos os perfis e aplicativos sejam instalados** à **Sim**.

### <a name="create-an-autopilot-deployment-profile"></a>Criar um perfil de implantação do Autopilot

Depois de criar um grupo de dispositivos, você deve criar um perfil de implantação para configurar os dispositivos do Autopilot.

No Intune no portal do Azure:

1. Selecione **registro de dispositivo** > **registro do Windows** > **perfis de implantação** > **criar perfil** .
1. Digite:
   * Nome – **proteger o perfil de implantação de estação de trabalho**.
   * Descrição - **implantação de estações de trabalho seguras**.
   * Definir **Converter destino todos os dispositivos Autopilot** à **Sim**. Essa configuração garante que todos os dispositivos na lista se registrar com o serviço de implantação do Autopilot. Permitir a 48 horas para que o registro a ser processado.
1. Selecione **Avançar**.
   * Para **modo de implantação**, escolha **Implantando automaticamente (visualização)** . Dispositivos com esse perfil são associados com o usuário que registra o dispositivo. Credenciais de usuário são necessárias para registrar o dispositivo.
   * O **ingressar no Azure AD como** deve mostrar a caixa **ingressado no Azure AD** e ser esmaecida.
   * Selecione seu idioma (região), tipo de conta de usuário **padrão**. 
1. Selecione **Avançar**.
   * Selecione uma marca de escopo, se você tiver um pré-configurados.
1. Selecione **Avançar**.
1. Escolher **atribuições** > **atribuir ao** > **grupos selecionados**. Na **selecionar grupos para incluir**, escolha **Proteja usuários da estação de trabalho**.
1. Selecione **Avançar**.
1. Selecione **Criar** para criar o perfil. O perfil de implantação do Autopilot agora está disponível para atribuir a dispositivos.

### <a name="configure-windows-update"></a>Configurar o Windows Update

Manter o Windows 10 atualizado é uma das coisas mais importantes que você pode fazer. Para manter o Windows em um estado protegido, você implanta um anel de atualização para gerenciar o ritmo que as atualizações são aplicadas às estações de trabalho. 

Este guia recomenda que você crie um novo grupo de atualização e alterar as seguintes configurações padrão:

No Portal do Azure:

1. Vá para **Microsoft Intune** > **atualizações de Software** > **anéis de atualização do Windows 10**.
1. Digite:
   * Nome - **atualizações de estação de trabalho gerenciados do Azure**
   * Manutenção de canal - **Windows Insider – rápido**
   * Adiamento da atualização de qualidade (dias) - **3**
   * Período de adiamento da atualização de recurso (dias) - **3**
   * Comportamento de atualização automática - **instalação automática e reinicialização sem controle de usuário final**
   * Impedir que o usuário de atualizações do Windows pausas - **bloco**
   * Exigir aprovação do usuário seja reiniciado fora das horas de trabalho - **necessária**
   * Permitir que o usuário reiniciar (engajado reiniciar) - **necessária**
   * Fazer a transição de que os usuários engajado reinicialização após uma reinicialização automática (dias) - **3**
   * Lembrete de reinicialização envolvido de adiamento (dias) - **3**
   * Definir prazo final para pendente é reiniciado (dias) - **3**

1. Selecione **Criar**.
1. No **atribuições** guia, adicione o **estações de trabalho seguro** grupo.

Para obter informações adicionais sobre as políticas de atualização do Windows, consulte [CSP de política - atualização](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-update).

### <a name="windows-defender-atp-intune-integration"></a>Integração com o Windows Defender ATP Intune

O Windows Defender ATP e o Microsoft Intune funcionam em conjunto para ajudar a prevenir violações de segurança. Eles também podem limitar o impacto de violações. Recursos de ATP fornecem detecção de ameaças em tempo real, bem como habilitar abrangentes de auditoria e registro dos dispositivos de ponto de extremidade.

Para configurar a integração do Windows Defender ATP e o Intune, acesse o portal do Azure.

1. Navegue até **Microsoft Intune** > **conformidade do dispositivo** > **Windows Defender ATP**.
1. Na etapa 1 em **Configurando o Windows Defender ATP**, selecione **conectar-se o Windows Defender ATP no Microsoft Intune na Central de segurança do Windows Defender**.
1. Na Central de segurança do Windows Defender:
   1. Selecione **as configurações** > **recursos avançados**.
   1. Para **conexão do Microsoft Intune**, escolha **em**.
   1. Selecione **salvar preferências**.
1. Depois de uma conexão é estabelecida, retorne ao Intune e selecione **Refresh** na parte superior.
1. Definir **Windows conectar-se a versão de dispositivos 10.0.15063 e acima para o Windows Defender ATP** à **em**.
1. Clique em **Salvar**.

Para obter mais informações, consulte [Windows Defender Advanced Threat Protection](https://docs.microsoft.com/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="finish-workstation-profile-hardening"></a>Concluir o perfil da estação de trabalho do sistema de proteção

Para concluir com êxito a proteção da solução, baixe e execute o script apropriado. Encontrar os links de download para seu desejada **nível de perfil**:

| Perfil | Local de download | Nome de arquivo |
| --- | --- | --- |
| Baixa segurança | N/D |  N/D |
| Segurança aprimorada | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809).ps1 |
| Alta segurança  | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Especializada | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline.ps1 |
| Conformidade especializado * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Protegido | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

\* Conformidade especializada é um script que impõe a configuração especializada fornecida no NCSC Windows10 SecurityBaseline.

Depois que o script for executado com êxito, você pode fazer atualizações para os perfis e políticas do Intune. Os scripts para perfis avançado e seguro criam políticas e perfis para você, mas você deve atribuir a política para seus **Proteja estações de trabalho** grupo.

* Aqui é onde você pode encontrar os perfis de configuração de dispositivo do Intune criados pelos scripts: **Portal do Azure** > **Microsoft Intune** > **configuração do dispositivo** > **perfis**.
* Aqui é onde você pode encontrar o dispositivo do Intune políticas de conformidade criadas pelos scripts: **Portal do Azure** > **Microsoft Intune** > **conformidade de dispositivo** > **políticas**.

Para revisar as alterações feitas pelos scripts, você pode exportar os perfis. Dessa forma, você pode determinar o sistema de proteção adicional que pode ser necessário, conforme descrito na documentação do SECCON.

Execute o script de exportação de dados do Intune `DeviceConfiguration_Export.ps1` do [DeviceConfiguration GiuHub repositório](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) para exportar todos os perfis do Intune atuais.

## <a name="additional-configurations-and-hardening-to-consider"></a>Configurações adicionais e proteção a considerar

Seguindo as diretrizes aqui, você implantou uma estação de trabalho segura. No entanto, você também deve considerar os controles adicionais. Por exemplo:

* limitar o acesso a navegadores alternativos
* Permitir HTTP de saída
* bloquear sites select
* definir permissões para a execução de scripts personalizados do PowerShell

### <a name="set-rules-in-the-firewall-configuration-service-provider-csp"></a>Definir regras no provedor de serviço de configuração de firewall (CSP)

Você pode fazer alterações adicionais para o gerenciamento de regras de entrada e saídas, conforme necessário para seus pontos de extremidade permitidos e bloqueados. Enquanto você continua a proteger a estação de trabalho segura, você pode simplificar a restrição que nega todo o tráfego de entrada e saído. Você pode adicionar sites de saída permitidos para incluir sites confiáveis e comuns. Para obter mais informações, consulte [serviço de configuração de Firewall](https://docs.microsoft.com/Windows/client-management/mdm/firewall-csp).

Recomendações de padrão restringido são:

* Negar todas as entradas
* Negar todas as saídas

O projeto Spamhaus mantém [a lista de blocos de domínio (duplo)](https://www.spamhaus.org/dbl/): um bom recurso para usar como ponto de partida para o bloqueio de sites.

### <a name="manage-local-applications"></a>Gerenciar aplicativos locais

A estação de trabalho segura move para um estado protegido realmente quando aplicativos locais são removidos, incluindo aplicativos de produtividade. Aqui, você adiciona o Chrome como navegador padrão e usar o Intune para limitar a capacidade de um usuário para modificar o navegador e seus plug-ins.

#### <a name="deploy-applications-using-intune"></a>Implantar aplicativos usando o Intune

Em algumas situações, os aplicativos, como o navegador Google Chrome são necessários na estação de trabalho protegida. O exemplo a seguir fornece instruções para instalar o Chrome em dispositivos no grupo de segurança **Proteja estações de trabalho**.

1. Baixar o instalador offline [pacote do Chrome para Windows 64‑bit](https://cloud.google.com/chrome-enterprise/browser/download/).
1. Extraia os arquivos e anote o local do `GoogleChromeStandaloneEnterprise64.msi` arquivo.
1. No **portal do Azure** procure **Microsoft Intune** > **aplicativos cliente** > **aplicativos**  >  **Adicionar**.
1. Sob **tipo de aplicativo**, escolha **Line-of-business**.
1. Sob **arquivo de pacote de aplicativo**, selecione o `GoogleChromeStandaloneEnterprise64.msi` arquivo no local extraído e selecione **Okey**.
1. Sob **informações do aplicativo**, forneça uma descrição e um publicador. Selecione **OK**.
1. Selecione **Adicionar**.
1. Sobre o **atribuições** guia, selecione **disponível para dispositivos registrados** sob **tipo de atribuição**.
1. Sob **grupos incluídos**, adicione o **estações de trabalho seguro** grupo.
1. Selecione **OK** e, em seguida, **Salvar**.

Para obter instruções sobre como definir configurações do Chrome, consulte [gerenciar o navegador do Chrome com o Microsoft Intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Configurando o portal da empresa para aplicativos personalizados

Em um modo seguro, a instalação do aplicativo é restrita ao portal da empresa do Intune. No entanto, a instalação do portal exige acesso ao Microsoft Store. Em sua solução segura, você pode disponibilizar o portal da empresa para todos os dispositivos por meio de um modo offline.

Uma cópia de gerenciados pelo Intune do [Portal da empresa](https://docs.microsoft.com/Intune/store-apps-company-portal-app) lhe dá acesso sob demanda para ferramentas adicionais que você pode enviar por push para baixo para os usuários das estações de trabalho protegidas.

Você talvez precise instalar aplicativos do Windows de 32 bits ou outros aplicativos cuja implantação exigem preparação especial. Nesses casos, o [ferramenta de preparação conteúdo do Microsoft win32](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) pode fornecer um prontos para uso `.intunewin` arquivo de formato para a instalação.

### <a name="use-powershell-to-create-custom-settings"></a>Usar o PowerShell para criar configurações personalizadas

Você também pode usar o PowerShell para estender recursos de gerenciamento de host. Esse script de exemplo configura um plano de fundo padrão no host. Ele é um recurso que também está disponível por meio do portal do Azure e perfis. O script de exemplo serve apenas para ilustrar a funcionalidade do PowerShell.

Talvez você precise configurar alguns controles personalizados e as configurações nas estações de trabalho seguras. Este exemplo altera o plano de fundo da estação de trabalho, usando a capacidade do Powershell para identificar facilmente o dispositivo como uma estação de trabalho prontos para uso e seguro.

O [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) script do Microsoft Scripting Center permite que o Windows de carregá-lo [imagem de plano de fundo gratuito e genérico](https://i.imgur.com/OAJ28zO.png) na inicialização.

1. Baixe o script em um dispositivo local.
1. Atualize o customerXXXX e o local de download da imagem de plano de fundo. Em nosso exemplo, substituímos customerXXXX a planos de fundo.  
1. Navegue até a **portal do Azure** > **Microsoft Intune** > **configuração do dispositivo** > **PowerShell scripts** > **Add**.
1. Fornecer um **nome** para o script e especifique a **local do Script**.
1. Selecione **Configurar**.
   1. Definir **executar este script usando o fez logon em credenciais** à **Sim**.
   1. Selecione **OK**.
1. Selecione **Criar**.
1. Selecione **atribuições** > **selecionar grupos**.
   1. Adicionar o grupo de segurança **Proteja estações de trabalho**.
   1. Clique em **Salvar**.

## <a name="enroll-and-validate-your-first-device"></a>Registrar e validar seu primeiro dispositivo

1. Para registrar seu dispositivo, você precisa ter as seguintes informações:
   * **Número de série** - encontrado no chassi do dispositivo.
   * **ID de produto do Windows** - encontrado em **System** > **sobre** no menu de configurações do Windows.
   * Você pode executar [Get-WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) para obter um arquivo de hash CSV com todas as informações necessárias para o registro de dispositivo.
   
     Executar `Get-WindowsAutoPilotInfo – outputfile device1.csv` para saída de informações como um arquivo CSV que você pode importar no Intune.

     > [!NOTE]
     > O script exige direitos elevados. Ele é executado como remoto conectado. O `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` comando permite que o script seja executado corretamente.

   * Você pode coletar essas informações ao entrar em uma versão do Windows 10 1809 ou o dispositivo maior. Seu revendedor de hardware também pode fornecer essas informações.
1. No **portal do Azure**, acesse **Microsoft Intune** > **registro de dispositivo** > **registro do Windows**  >  **Dispositivos - gerenciar o Windows Autopilot**.
1. Selecione **importação** e escolha o arquivo CSV.
1. Adicionar o dispositivo para o **Proteja estações de trabalho** grupo de segurança.
1. No dispositivo Windows 10 você deseja configurar, vá para **configurações do Windows** > **atualização e segurança** > **recuperação**.
   1. Escolher **começar** sob **redefinir esse PC**.
   1. Siga os prompts para redefinir e reconfigurar o dispositivo com as políticas de conformidade e de perfil configuradas.

Depois de configurar o dispositivo, conclua uma revisão e verifique a configuração. Confirme se o primeiro dispositivo está configurado corretamente antes de continuar a implantação.

## <a name="assign-and-monitor"></a>Atribuir e monitorar

Para atribuir usuários e dispositivos, você precisa mapear as [selecionado perfis](https://docs.microsoft.com/intune/device-profile-assign) ao seu grupo de segurança. Todos os novos usuários que precisam de permissões para o serviço devem ser adicionados ao grupo de segurança.

Você pode monitorar perfis com [monitoramento de perfil do Intune](https://docs.microsoft.com/intune/device-profile-monitor).

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre [Microsoft Intune](https://docs.microsoft.com/intune/index).
* Entender [do Azure AD](https://docs.microsoft.com/azure/active-directory/index).

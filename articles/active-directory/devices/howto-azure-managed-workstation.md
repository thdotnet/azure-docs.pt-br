---
title: Implantar estações de trabalho - Azure Active Directory gerenciados do Azure
description: Saiba como implantar estações de trabalho seguras gerenciados do Azure para reduzir o risco de violação devido a configuração incorreta ou o comprometimento
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
ms.openlocfilehash: 242926c0821e4951d2a2bd2f858f63691baf1017
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307221"
---
# <a name="deploy-a-secure-workstation"></a>Implantar uma estação de trabalho segura

Agora que você entende [por proteger o acesso de estação de trabalho é importante?](concept-azure-managed-workstation.md) é hora de começar o processo de implantação usando as ferramentas disponíveis. Este guia usará os perfis definidos para criar uma estação de trabalho que é mais segura desde o início.

![Implantação de uma estação de trabalho segura](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Antes de implantar a solução, o que você usará o perfil deve ser selecionado. É importante observar que você pode aplicar qualquer um dos perfis selecionados e mover para outra por meio da atribuição de perfil no Intune com base em suas necessidades. Vários perfis podem ser usados simultaneamente em uma implantação e atribuídos usando as atribuições da marca ou grupo.

| Perfil | Baixo | Avançado | Alto | Especializada | Protegido | Isolado |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Usuário do Azure AD | Sim | sim | sim | sim | sim | Sim |
| Gerenciados pelo Intune | Sim | sim | sim | sim | sim | Sim |
| Dispositivos registrados no Azure AD | Sim |  |  |  |  | |   |
| Dispositivo ingressado no AD do Azure |   | Sim | sim | sim | sim | Sim |
| Linha de base de segurança do Intune aplicada |   | Sim <br> (Avançado) | Sim <br> (HighSecurity) | Sim <br> (NCSC) | Sim <br> (Protegido) |  ND |
| Hardware atende aos padrões de 10 Windows seguro |   | Sim | sim | sim | sim | Sim |
| Microsoft Defender ATP habilitado |   | Sim  | sim | sim | sim | Sim |
| Remoção de direitos de administrador |   |   | Sim  | sim | sim | Sim |
| Implantação usando o Microsoft Autopilot |   |   | Sim  | sim | sim | Sim |
| Aplicativos instalados apenas pelo Intune |   |   |   | Sim | sim |Sim |
| Restringido a somente lista aprovada de URLs |   |   |   | Sim | sim |Sim |
| Internet (entrada/saída bloqueado) |   |   |   |  |  |Sim |

## <a name="license-requirements"></a>Requisitos de licença

Os conceitos abordados neste guia assumirá o Microsoft 365 Enterprise E5 ou uma SKU equivalente. Algumas das recomendações neste guia podem ser implementadas com SKUs inferiores. Informações adicionais podem ser encontradas no [licenciamento do Microsoft 365 Enterprise](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

Você talvez queira configurar [licenciamento baseado em grupo](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) para seus usuários automatizar o provisionamento de licenças.

## <a name="azure-active-directory-configuration"></a>Configuração do Active Directory do Azure

Configurar seu diretório do Azure Active Directory (Azure AD), que irá gerenciar a seus usuários, grupos e dispositivos para suas estações de trabalho do administrador exige que você habilite os serviços de identidade e recursos com um [contadeadministrador](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

Quando você cria a conta de administrador de estação de trabalho segura, você está expondo a conta de sua estação de trabalho atual. É recomendável que fazer essa configuração inicial e todas as configurações globais de um dispositivo de segurança conhecido. Você pode considerar as diretrizes para [evitar infecções por malware](https://docs.microsoft.com/windows/security/threat-protection/intelligence/prevent-malware-infection) para reduzir o risco de expor primeiro a experiência inicial de ataque.

As organizações devem exigir a autenticação multifator, pelo menos para seus administradores. Ver [implantar o MFA baseado em nuvem](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted) para obter diretrizes de implementação.

### <a name="azure-ad-users-and-groups"></a>Usuários e grupos do azure AD

No portal do Azure, navegue até **Azure Active Directory** > **usuários** > **novo usuário**. [Crie seu usuário da estação de trabalho segura](https://docs.microsoft.com/Intune/quickstart-create-user), quem será o administrador do dispositivo.

* **Nome** -proteger a estação de trabalho do administrador
* **Nome de usuário** - secure-ws-admin@identityitpro.com
* **Função de diretório** - **administrador limitado** e selecione a seguinte função administrativa
   * **Administrador do Intune**
* **Criar**

Vamos criar dois grupos de um para os usuários das estações de trabalho e outro para as estações de trabalho em si. No portal do Azure, navegue até **Azure Active Directory** > **grupos** > **novo grupo**

Primeiro grupo de usuários da estação de trabalho. Você talvez queira configurar [licenciamento baseado em grupo](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) para os usuários nesse grupo para automatizar o provisionamento de licenças aos usuários.

* **Tipo de grupo** -segurança
* **Nome do grupo** -Proteja usuários da estação de trabalho
* **Tipo de associação** - atribuído
* Adicione o usuário de administrador de estação de trabalho segura ao grupo
   * secure-ws-admin@identityitpro.com
* Você pode adicionar outros usuários que gerenciarão estações de trabalho seguras ao grupo
* **Criar**

Segundo grupo de dispositivos de estação de trabalho.

* **Tipo de grupo** -segurança
* **Nome do grupo** -Proteja estações de trabalho
* **Tipo de associação** - atribuído
* **Criar**

### <a name="azure-ad-device-configuration"></a>Configuração de dispositivo do Azure AD

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Especifique quem pode ingressar dispositivos no Azure AD

Configurar os dispositivos de configuração no Active Directory para permitir que seu grupo de segurança administrativa unir dispositivos ao seu domínio. Para configurar essa configuração do portal do Azure, navegue até **Azure Active Directory** > **dispositivos** > **configurações do dispositivo**. Escolher **Selected** sob **os usuários podem unir dispositivos ao Azure AD** e selecione o grupo "Usuários de estação de trabalho segura".

#### <a name="removal-of-local-admin-rights"></a>Remoção de direitos de administrador local

Como parte da distribuição do usuários de estações de trabalho do VIP, DevOps e estações de trabalho de nível seguras terá sem direitos de administrador em seus computadores. Para configurar essa configuração do portal do Azure, navegue até **Azure Active Directory** > **dispositivos** > **configurações do dispositivo**. Selecione **None** sob **administradores locais adicionais no AD do Azure ingressado em dispositivos**.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Exigir a autenticação multifator para unir dispositivos

Para aumentar ainda mais o processo de ingresso de dispositivos para o Azure AD, navegue até **Azure Active Directory** > **dispositivos** > **configurações do dispositivo** escolher **Yes** sob **exigir multi-Factor Auth para unir dispositivos** , em seguida, escolha **salvar**.

#### <a name="configure-mdm"></a>Configurar MDM

No portal do Azure, navegue até **Azure Active Directory** > **mobilidade (MDM e MAM)**  > **Microsoft Intune**. Alterar a configuração **escopo do usuário MDM** à **todos os** e escolha **salvar** conforme permitiremos que qualquer dispositivo seja gerenciado pelo Intune nesse cenário. Mais informações podem ser encontradas no artigo [Intune Quickstart: Configurar o registro automático para dispositivos Windows 10](https://docs.microsoft.com/Intune/quickstart-setup-auto-enrollment). Vamos criar políticas de configuração e a conformidade do Intune em uma etapa futura.

#### <a name="azure-ad-conditional-access"></a>Acesso condicional do Azure AD

Acesso condicional do Azure AD pode ajudar a manter essas tarefas administrativas privilegiadas em dispositivos compatíveis. Os usuários que definimos como membros do **Proteja usuários da estação de trabalho** grupo precisarão realizar a autenticação multifator ao entrar em aplicativos de nuvem. Vamos seguir as diretrizes de práticas recomendadas e excluir nossas contas de acesso de emergência da política. Informações adicionais podem ser encontradas no artigo [gerenciar contas de acesso de emergência no Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)

Para configurar o acesso condicional no portal do Azure, navegue até **Azure Active Directory** > **acesso condicional** > **nova política**.

* **Nome** -seguro de dispositivos necessários política
* Atribuições
   * **Usuários e grupos**
      * Incluir - **usuários e grupos** – selecione o **Proteja usuários da estação de trabalho** grupo criado anteriormente
      * Excluir - **usuários e grupos** -selecionar contas de acesso de emergência da sua organização
   * **Aplicativos na nuvem**
      * Incluir - **todos os aplicativos em nuvem**
* Controles de acesso
   * **Grant** - marque **conceder acesso** botão de opção
      * **Exigir autenticação multifator**
      * **Exigir que o dispositivo seja marcado como em conformidade**
      * Para vários controles - **exigir todos os controles selecionados**
* Habilitar política - **em**

As organizações podem, opcionalmente, criar políticas para países de bloco em que os usuários não acessaria os recursos da empresa. Para obter mais informações sobre as políticas de acesso condicional com base no local de IP podem ser encontradas no artigo [o que é a condição de localização no acesso condicional do Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition)

## <a name="intune-configuration"></a>Configuração do Intune

### <a name="configure-enrollment-status"></a>Configurar o status do registro

Conforme descrito no gerenciamento de cadeia de suprimentos, garantindo que a estação de trabalho segura seja um dispositivo limpo confiável é recomendável que, ao comprar novos dispositivos, que dispositivos ser definida como de fábrica [Windows 10 Pro no modo S](https://docs.microsoft.com/Windows/deployment/Windows-10-pro-in-s-mode), que limita a exposição e vulnerabilidades durante o gerenciamento da cadeia de fornecedores. Depois que um dispositivo é recebido de seu fornecedor, o dispositivo será removido do modo de S usando o Autopilot. As diretrizes a seguir fornecem detalhes sobre como aplicar o processo de transformação.

Queremos garantir que os dispositivos estejam totalmente configurados antes do uso. O Intune fornece um meio **bloquear o uso do dispositivo até que todos os perfis e aplicativos sejam instalados**. 

1. Dos **portal do Azure** navegue até:
1. **Microsoft Intune** > **registro do dispositivo** > **registro do Windows** > **página de Status de registro (visualização)**  >  **Padrão** > **configurações**.
1. Definir **mostrar o progresso de instalação de perfil de aplicativo** à **Sim**.
1. Definir **bloquear o uso do dispositivo até que todos os perfis e aplicativos sejam instalados** à **Sim**.

### <a name="create-an-autopilot-deployment-profile"></a>Criar um perfil de implantação do Autopilot

Depois de criar um grupo de dispositivos, você deve criar um perfil de implantação para que você possa configurar os dispositivos do Autopilot.

1. No Intune no portal do Azure, escolha **registro de dispositivo** > **registro do Windows** > **perfis de implantação**  >   **Criar perfil**.
1. Para o nome, digite **proteger o perfil de implantação de estação de trabalho**. Para obter a descrição, insira **implantação de estações de trabalho seguras**.
1. Conjunto de converter destino todos os dispositivos Autopilot para Sim. Essa configuração garante que todos os dispositivos na lista se registrar com o serviço de implantação do Autopilot.  Permitir a 48 horas para que o registro a ser processado.
1. Para o modo de implantação, escolha **Implantando automaticamente (visualização)** . Dispositivos com esse perfil são associados com o registro do dispositivo de usuário. Credenciais de usuário são necessárias para registrar o dispositivo.
1. Na associação para o Azure AD como caixa, **ingressado no Azure AD** deve ser escolhido e cinza.
1. Selecione a experiência de Out-of-box (OOBE), configure a seguinte opção e deixe outras definido como o padrão e, em seguida, selecione **Okey**:
   1. Tipo de conta de usuário: **Standard**
1. Selecione **Criar** para criar o perfil. O perfil de implantação do Autopilot agora está disponível para atribuir a dispositivos.
1. Escolher **atribuições** > **atribuir ao** > **grupos selecionados**
   1. **Selecionar grupos para incluir** -Proteja usuários da estação de trabalho

### <a name="configure-windows-update"></a>Configurar o Windows Update

Uma das coisas mais importantes que uma organização pode fazer é manter o Windows 10 atualizado. Para manter o Windows 10 em um estado protegido, implantaremos um anel de atualização para gerenciar o ritmo no qual as atualizações são aplicadas às estações de trabalho. Essa configuração pode ser encontrada na **portal do Azure** > **Microsoft Intune** > **atualizações de Software**  >  **Anéis de atualização do Windows 10**.

Iremos **criar** alterado de um novo anel de atualização com as seguintes configurações de padrões.

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

Clique em **criar** , em seguida, no **atribuições** guia adicionar o **estações de trabalho segura** grupo como um grupo incluído.

Informações adicionais sobre as políticas de atualização do Windows podem ser encontradas no [CSP de política - atualização](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-update)

### <a name="windows-defender-atp-intune-integration"></a>Integração com o Windows Defender ATP Intune

Windows Defender ATP e o Microsoft Intune funcionam em conjunto para ajudar a prevenir violações de segurança e ajudam a limitar o impacto de violações. Fornecem os recursos de detecção em tempo real. ATP também fornecerá nosso auditoria extenso de implantação e o registro de dispositivos de ponto de extremidade.

Para configurar a integração com o Windows Defender ATP Intune no portal do Azure, navegue até **Microsoft Intune** > **conformidade do dispositivo** > **do Windows Defender ATP** .

1. Na etapa 1 em **Configurando o Windows Defender ATP**, clique em **conectar-se o Windows Defender ATP no Microsoft Intune na Central de segurança do Windows Defender**.
1. Na Central de segurança do Windows Defender:
   1. Selecione **as configurações** > **recursos avançados**
   1. Para **conexão do Microsoft Intune**, escolha **em**
   1. Selecione **salvar preferências**
1. Depois de uma conexão é estabelecida, retorne ao Intune e clique em **Refresh** na parte superior.
1. Definir **Windows conectar-se a versão de dispositivos 10.0.15063 e acima para o Windows Defender ATP** à **em**.
1. **Salvar**

Informações adicionais podem ser encontradas no artigo [Windows Defender Advanced Threat Protection](https://docs.microsoft.com/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="completing-hardening-of-the-workstation-profile"></a>Concluindo a proteção do perfil da estação de trabalho

Para concluir com êxito a proteção da solução, baixe e execute o script de acordo com o estado desejado **nível de perfil** do gráfico a seguir.

| Perfil | Local de download | Nome de arquivo |
| --- | --- | --- |
| Baixa segurança | N/D |  N/D |
| Segurança aprimorada | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809).ps1 |
| Alta segurança  | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Especializada | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline.ps1 |
| Conformidade especializado * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Protegido | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

Conformidade especializado * é um script que impõe a configuração especializada fornecida no NCSC Windows10 SecurityBaseline.

Depois que o script selecionado for executado com êxito, as atualizações para os perfis e as políticas podem ser feitas no Microsoft Intune. Os scripts para perfis avançado e seguro criam políticas e perfis para você, mas você devem atribuir a política para seus **Proteja estações de trabalho** grupo.

* Perfis de configuração do dispositivo do Intune criados pelos scripts podem ser encontrados na **portal do Azure** > **Microsoft Intune** > **deconfiguraçãododispositivo**  >  **Perfis**.
* Políticas de conformidade do dispositivo do Intune criadas pelos scripts podem ser encontradas na **portal do Azure** > **Microsoft Intune** > **conformidade do dispositivo**  >  **Políticas**.

Para revisar as alterações, você também pode exportar os perfis, e aplicar alterações ao arquivo de exportação, conforme descrito no SECCON documentação com base em e adicional de proteção que é necessária.

Executar os dados do Intune exportar o script `DeviceConfiguration_Export.ps1` do [DeviceConfiguration GiuHub repositório](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) fornecerá a exportação atual de todos os perfis existentes do Intune.

## <a name="additional-configurations-and-hardening-to-consider"></a>Configurações adicionais e proteção a considerar

A orientação fornecida tiver habilitado uma estação de trabalho protegida, controles adicionais também devem ser considerados, como acesso de navegadores alternativos, saídos HTTP permitidos e bloqueados sites e a capacidade de executar o script do PowerShell personalizado.

### <a name="restrictive-inbound-and-outbound-rules-in-firewall-configuration-service-provider-csp"></a>Regras de entrada e saídas restritivas no provedor de serviço de configuração do firewall (CSP)

Gerenciamento adicional de regras de entrada e saídas pode ser atualizado para refletir os pontos de extremidade permitidos e bloqueados. Conforme continuamos a proteger a estação de trabalho segura, podemos mover a restrição para um negar todas as entrada e saída como padrão e adicionar sites permitidos para a saída refletir a sites da web comuns e confiável. As informações de configuração adicional para o provedor de serviço de configuração de Firewall podem ser encontradas no artigo [Firewall CSP](https://docs.microsoft.com/Windows/client-management/mdm/firewall-csp).

Recomendações de padrão restringido são:

* Negar todas as entradas
* Negar todas as saídas

O projeto Spamhaus mantém uma lista de BOM que as organizações podem usar como ponto de partida para o bloqueio de sites conhecidos como [a lista de blocos de domínio (duplo)](https://www.spamhaus.org/dbl/).

### <a name="managing-local-applications"></a>Gerenciando aplicativos locais

Removendo aplicativos locais, incluindo a remoção de aplicativos de produtividade moverá a estação de trabalho segura para um estado totalmente protegido. Em nosso exemplo, adicionaremos Chrome como navegador padrão e restringir a capacidade de modificar o incluindo plug-ins de navegador usando o Intune.

#### <a name="deploy-applications-using-intune"></a>Implantar aplicativos usando o Intune

Em algumas situações, os aplicativos, como o navegador Google Chrome são necessários na estação de trabalho protegida. O exemplo a seguir fornece instruções para instalar o Chrome em dispositivos no grupo de segurança **Proteja estações de trabalho** criado anteriormente.

1. Baixar o instalador offline [pacote Chrome para Windows 64‑bit](https://cloud.google.com/chrome-enterprise/browser/download/)
1. Extraia os arquivos e anote o local do `GoogleChromeStandaloneEnterprise64.msi` instalar usando o Intune
1. No **portal do Azure** procure **Microsoft Intune** > **aplicativos cliente** > **aplicativos**  >  **Adicionar**
1. Sob **tipo de aplicativo**, escolha **Line-of-business**
1. Sob **arquivo de pacote de aplicativo**, selecione o `GoogleChromeStandaloneEnterprise64.msi` no local extraído e clique **Okey**
1. Sob **informações do aplicativo**, forneça uma descrição e o publicador e escolha **Okey**
1. Clique em **Adicionar**
1. Sobre o **atribuições** selecionar **disponível para dispositivos registrados** sob **tipo de atribuição**
1. Sob **grupos incluídos**, adicione o **estações de trabalho seguro** grupo criado anteriormente
1. Clique em **Okey** , em seguida, **salvar**

Orientações adicionais sobre como definir configurações do Chrome podem ser encontradas em seu artigo de suporte [gerenciar o navegador do Chrome com o Microsoft Intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Configurando o portal da empresa para aplicativos personalizados

Em um modo seguro, instalação de aplicativos será restrito ao portal da empresa do Intune. No entanto, a instalação do portal exige acesso ao Microsoft Store. Em nossa solução segura, faremos o portal disponíveis para todos os dispositivos usando o modo offline do portal da empresa.

Instalando uma Intune gerenciados a cópia do [Portal da empresa](https://docs.microsoft.com/Intune/store-apps-company-portal-app) permitirá que a capacidade de enviar por push ferramentas adicionais sob demanda para os usuários das estações de trabalho protegidas.

Algumas organizações podem ser necessárias para instalar aplicativos do Windows de 32 bits ou aplicativos que exigem outros preparativos para implantar. Para esses aplicativos, o [ferramenta de preparação conteúdo do Microsoft win32](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) fornecerá um pronto para usar `.intunewin` arquivo de formato para a instalação.

### <a name="setting-up-custom-settings-using-powershell"></a>Configurar configurações personalizadas usando o PowerShell

Também usaremos um exemplo de uso do PowerShell para fornecer extensibilidade no gerenciamento de host. O script irá configurar um plano de fundo padrão no host. Esse recurso também está disponível em perfis e é usado apenas para ilustrar o recurso.

Na solução, pode haver a necessidade de configurar alguns controles personalizados e configurações em estações de trabalho seguras. Em nosso exemplo, alteraremos o plano de fundo da estação de trabalho usando o Powershell para ser capaz de identificar facilmente o dispositivo como uma estação de trabalho segura pronta para uso. Embora nosso exemplo usará o PowerShell para concluir essa tarefa, ele também pode ser concluído no portal do Azure.

Nosso exemplo usará o seguinte [imagem de fundo genérico livre](https://i.imgur.com/OAJ28zO.png) e o [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) do Microsoft Scripting Center para permitir que o Windows carregar o plano de fundo em Iniciar.

1. Baixe o script em um dispositivo local
1. Atualize o customerXXXX e o local de download do plano de fundo que você pretende para usar no script para refletir o arquivo de plano de fundo e a pasta que você gostaria de usar a implantação. Em nosso exemplo, substituímos customerXXXX a planos de fundo.  
1. Navegue até a **portal do Azure** > **Microsoft Intune** > **configuração do dispositivo** > **PowerShell scripts** > **adicionar**
1. Fornecer um **nome** para o script e especifique a **local do Script** onde você baixou para
1. Selecione **Configurar**
   1. Definir **executar este script usando o fez logon em credenciais**, para **Sim**
   1. Clique em **OK**
1. Clique em **Criar**
1. Selecione **atribuições** > **selecionar grupos**
   1. Adicionar o grupo de segurança **estações de trabalho segura** criado anteriormente e clique em **salvar**

### <a name="using-the-preview-mdm-security-baseline-for-october-2018"></a>Usando a visualização: Linha de base de segurança MDM para outubro de 2018

Microsoft Intune apresentou o recurso de gerenciamento de linha de base de segurança fornecendo aos administradores uma maneira simples de impor uma postura de segurança de linha de base comum. A linha de base fornece um meio semelhante para alcançar um bloqueado para baixo de estação de trabalho de perfil avançado.

Para a estação de trabalho segura, implementação que nessa linha de base não é usada como ela entrará em conflito com a implantação de uma configuração de segurança.

|   |   |   |
| :---: | :---: | :---: |
| Acima do bloqueio | Instalação de dispositivo | Serviços da Área de Trabalho Remota |
| Tempo de execução do aplicativo | Bloqueio de dispositivo | Gerenciamento remoto |
| Gerenciamento de Aplicativos | Serviço de Log de eventos | Chamada de procedimento remoto |
| Reprodução automática | Experiência | Search |
| BitLocker | Exploit Guard | Tela inteligente |
| Navegador | Explorador de Arquivos | Requisito do sistema|
| Conectividade | Internet Explorer | Wi-Fi |
| Delegação de credenciais | Opções de segurança de diretivas locais | Gerenciador de Conexão do Windows |
| Credenciais da interface do usuário | Guia de segurança MS | Windows Defender|
| Proteção de dados | Herdado de MSS | O espaço de trabalho do Windows Ink |
| Device Guard | Potência | Windows PowerShell |

Para obter mais informações sobre esse recurso de visualização podem ser encontradas no artigo [as configurações de linha de base de segurança do Windows para o Intune](https://docs.microsoft.com/Intune/security-baseline-settings-windows).

## <a name="enroll-and-validate-your-first-device"></a>Registrar e validar seu primeiro dispositivo

1. Para registrar seu dispositivo, você precisa ter as seguintes informações:
   * **Número de série** - encontrado no chassi do dispositivo
   * **ID de produto do Windows** - encontrado em **System** > **sobre** no menu de configurações do Windows.
   * Executando [Get-WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) fornecerá um hash de arquivo CSV para registro de dispositivo com todas as informações necessárias. 
      * Executar `Get-WindowsAutoPilotInfo – outputfile device1.csv` para saída de informações como um arquivo CSV que pode ser importado no Intune.

   > [!NOTE]
   > O script criará requerem direitos elevados e execute como remoto conectado. Você pode usar o comando a seguir para permitir que o script seja executado corretamente. `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

   *  Você pode coletar essas informações ao entrar em um dispositivo superior ou o Windows 10 versão 1809 para coletar as informações ou seu revendedor de hardware pode fornecer essas informações quando novos dispositivos de ordenação.
1. Coletar as informações necessárias e retornar para o **portal do Azure**. Navegue até **Microsoft Intune** > **registro de dispositivo** > **registro do Windows** > **dispositivos: Gerenciar dispositivos Windows Autopilot**, selecione **importação**e escolha o arquivo CSV que você criou ou que foram fornecidos.
1. Adicionar o dispositivo agora está registrado para o grupo de segurança **Proteja estações de trabalho** criado anteriormente.
1. No dispositivo Windows 10 você deseja configurar, navegue até **configurações do Windows** > **atualização e segurança** > **recuperação**. Escolher **começar** sob **redefinir esse PC** e siga os prompts para redefinir e reconfigurar o dispositivo usando as políticas de conformidade e de perfil configuradas.

Depois que o dispositivo tiver sido configurado, concluir uma revisão e verifique a configuração. Confirme se que o primeiro dispositivo está configurado corretamente antes de continuar a implantação.

## <a name="assignment-and-monitoring"></a>Atribuição e monitoramento

Atribuir usuários e dispositivos exigirão o mapeamento do [selecionado perfis](https://docs.microsoft.com/intune/device-profile-assign) para a segurança de seu grupo e todos os novos usuários que terá permissão para o serviço serão solicitados a ser adicionado ao grupo de segurança.

Os perfis a serem de monitoramento pode ser feito usando o monitoramento [perfis do Microsoft Intune](https://docs.microsoft.com/intune/device-profile-monitor).

## <a name="next-steps"></a>Próximas etapas

[Microsoft Intune](https://docs.microsoft.com/intune/index) documentação

[Azure AD](https://docs.microsoft.com/azure/active-directory/index) documentação
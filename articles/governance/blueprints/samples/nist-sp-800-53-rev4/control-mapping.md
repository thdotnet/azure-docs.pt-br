---
title: Amostra – blueprint do NIST SP 800-53 R4 – mapeamento de controles
description: Mapeamento de controles da amostra de blueprint NIST SP 800-53 R4 para o Azure Policy e o RBAC.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b887c4e6812d201dc83465a578f71e1742e8e9cf
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343075"
---
# <a name="control-mapping-of-the-nist-sp-800-53-r4-blueprint-sample"></a>Mapeamento de controles da amostra de blueprint NIST SP 800-53 R4

O artigo a seguir fornece detalhes sobre como a amostra de blueprint NIST SP 800-53 R4 do Azure Blueprints é mapeada para os controles do NIST SP 800-53 R4. Para obter mais informações sobre os controles, confira [NIST SP 800-53](https://nvd.nist.gov/800-53).

Os seguintes mapeamentos referem-se aos controles do **NIST SP 800-53 (Rev. 4)** . Use a navegação no lado direito para ir diretamente para um mapeamento de controle específico. Muitos dos controles mapeados são implementados com uma iniciativa do [Azure Policy](../../../policy/overview.md). Para examinar a iniciativa completa, abra **Política** no portal do Azure e selecione a página **Definições**. Em seguida, localize e selecione a iniciativa de política interna **[Visualização]: Auditar controles NIST SP 800-53 R4 e implantar extensões de VM específicas para dar suporte a requisitos de auditoria**.

## <a name="ac-2-account-management"></a>AC-2 Gerenciamento de conta

Este blueprint ajuda a analisar contas que podem não estar em conformidade com os requisitos de gerenciamento de conta de sua organização. Este blueprint atribui cinco definições do Azure Policy que auditam contas externas com permissões de leitura, gravação e proprietário em contas de assinatura a preteridas. Revisando as contas auditadas por essas políticas, você pode adotar as medidas adequadas a fim de garantir que os requisitos de gerenciamento de conta sejam atendidos.

- [Visualização]: Audit deprecated accounts on a subscription
- [Visualização]: Audit deprecated accounts with owner permissions on a subscription
- [Visualização]: Audit external accounts with owner permissions on a subscription
- [Visualização]: Audit external accounts with read permissions on a subscription
- [Visualização]: Audit external accounts with write permissions on a subscription

## <a name="ac-2-7-account-management--role-based-schemes"></a>AC-2 (7) Gerenciamento de conta | Esquemas baseados em função

O Azure implementa o RBAC ([controle de acesso baseado em função](../../../../role-based-access-control/overview.md)) para ajudar você a gerenciar quem tem acesso aos recursos no Azure. Usando o portal do Azure, você pode examinar quem tem acesso aos recursos do Azure e suas permissões. Esse blueprint também atribui duas definições do [Azure Policy](../../../policy/overview.md) para auditar o uso da autenticação do Azure Active Directory para SQL Servers e o Service Fabric. O uso da autenticação do Azure Active Directory permite o gerenciamento simplificado de permissões e o gerenciamento centralizado de identidades dos usuários de banco de dados e de outros serviços da Microsoft.

- Provisionamento de auditoria de um administrador do Azure Active Directory Domain Services para o SQL Server
- Auditar o uso do Azure Active Directory para autenticação de cliente no Service Fabric

## <a name="ac-2-12-account-management--account-monitoring--atypical-usage"></a>AC-2 (12) Gerenciamento de conta | Monitoramento de conta / Uso atípico

O acesso JIT (Just-In-Time) à máquina virtual bloqueia o tráfego de entrada às máquinas virtuais do Azure, reduzindo a exposição a ataques enquanto fornece acesso fácil para se conectar às VMs quando necessário. Todas as solicitações JIT para acessar máquinas virtuais são registradas no Log de Atividades, permitindo que você monitore se há uso atípico. Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que ajuda você a monitorar máquinas virtuais que podem dar suporte ao acesso Just-In-Time, mas ainda não foram configuradas.

- [Visualização]: Monitorar o possível acesso Just In Time (JIT) à rede na Central de Segurança do Azure

## <a name="ac-4-information-flow-enforcement"></a>AC-4 Imposição de fluxo de informações

O CORS (compartilhamento de recurso de origem cruzada) pode permitir que recursos dos Serviços de Aplicativos sejam solicitados de um domínio externo. A Microsoft recomenda que você permita que apenas domínios necessários interajam com sua API, sua função e seus aplicativos Web. Este blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) para ajudá-lo a monitorar as restrições de acesso de recursos do CORS na Central de Segurança do Azure.
Ter noções básicas sobre implementações de CORS pode ajudá-lo a confirmar que os controles de fluxo de informações foram implementados.

- [Visualização]: Audit CORS resource access restrictions for a Web Application

## <a name="ac-5-separation-of-duties"></a>AC-5 Separação de funções

Ter apenas um proprietário de assinatura do Azure não permite a redundância administrativa. Por outro lado, ter muitos proprietários de assinatura do Azure pode aumentar o potencial de uma violação por meio de uma conta de proprietário comprometida. Esse blueprint ajuda você a manter um número apropriado de proprietários de assinatura do Azure por meio da atribuição de duas definições do [Azure Policy](../../../policy/overview.md) que audita o número de proprietários de assinaturas do Azure. Este blueprint também atribui quatro definições do Azure Policy que ajudam a controlar a associação ao grupo de Administradores em Máquinas Virtuais do Windows. O gerenciamento de permissões de proprietário de assinatura e administrador de máquina virtual pode ajudá-lo a implementar uma diferenciação de funções apropriada.

- [Visualização]: Audit maximum number of owners for a subscription
- [Visualização]: Audit minimum number of owners for subscription
- Fazer uma auditoria para confirmar que o grupo de Administradores nas VMs do Windows exclui os membros especificados
- Fazer uma auditoria para confirmar que o grupo de Administradores nas VMs do Windows inclui os membros especificados
- Implantar uma extensão de VM para fazer uma auditoria para confirmar que o grupo de Administradores nas VMs do Windows exclui os membros especificados
- Implantar uma extensão de VM para fazer uma auditoria para confirmar que o grupo de Administradores nas VMs do Windows inclui os membros especificados

## <a name="ac-6-7-least-privilege--review-of-user-privileges"></a>AC-6 (7) Privilégios mínimos | Análise de privilégios do usuário

O Azure implementa o RBAC ([controle de acesso baseado em função](../../../../role-based-access-control/overview.md)) para ajudar você a gerenciar quem tem acesso aos recursos no Azure. Usando o portal do Azure, você pode examinar quem tem acesso aos recursos do Azure e suas permissões. Este blueprint atribui seis definições do [Azure Policy](../../../policy/overview.md) para auditar contas que devem ser priorizadas para análise. Revisar esses indicadores de conta pode ajudar a garantir que controles de privilégios mínimos estejam implementados.

- [Visualização]: Audit maximum number of owners for a subscription
- [Visualização]: Audit minimum number of owners for subscription
- Fazer uma auditoria para confirmar que o grupo de Administradores nas VMs do Windows exclui os membros especificados
- Fazer uma auditoria para confirmar que o grupo de Administradores nas VMs do Windows inclui os membros especificados
- Implantar uma extensão de VM para fazer uma auditoria para confirmar que o grupo de Administradores nas VMs do Windows exclui os membros especificados
- Implantar uma extensão de VM para fazer uma auditoria para confirmar que o grupo de Administradores nas VMs do Windows inclui os membros especificados

## <a name="ac-17-1-remote-access--automated-monitoring--control"></a>AC-17 (1) Acesso remoto | Controle / monitoramento automatizado

Este blueprint ajuda a monitorar e controlar o acesso remoto atribuindo três definições do [Azure Policy](../../../policy/overview.md) para monitorar se a depuração remota de aplicativos do Serviço de Aplicativo do Azure está desativada, bem como duas definições de política que auditam Máquinas Virtuais do Linux que permitem conexões remotas de contas sem senhas. Este blueprint também atribui uma definição do Azure Policy que ajuda você a monitorar o acesso irrestrito a contas de armazenamento. Monitorar esses indicadores pode ajudá-lo a garantir que os métodos de acesso remoto estejam em conformidade com sua política de segurança.

- [Visualização]: Audit remote debugging state for a Function App
- [Visualização]: Audit remote debugging state for a Web Application
- [Visualização]: Audit remote debugging state for an API App
- [Visualização]: Audit that Linux VMs do not allow remote connections from accounts without passwords
- [Visualização]: Deploy VM extension to audit that Linux VMs do not allow remote connections from accounts without passwords
- Auditar o acesso irrestrito à rede para contas de armazenamento

## <a name="au-3-2-content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>AU-3 (2) Conteúdo de Registros de Auditoria | Gerenciamento Centralizado do Conteúdo do Registro de Auditoria Planejada

Dados de log coletados pelo Azure Monitor são armazenados em um workspace do Log Analytics, permitindo o gerenciamento e a configuração centralizada. Este blueprint ajuda a garantir que os eventos sejam registrados em log atribuindo sete definições do [Azure Policy](../../../policy/overview.md) que auditam e impõem a implantação do agente do Log Analytics em máquinas virtuais do Azure.

- [Visualização]: Auditar a implantação do Agente do Log Analytics – imagem de VM (sistema operacional) não listada
- [Visualização]: Auditar a implantação do Agente do Log Analytics no VMSS – imagem de VM (sistema operacional) não listada
- [Visualização]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Versão prévia]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Visualização]: Deploy Log Analytics Agent for Linux VMs
- [Versão prévia]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Visualização]: Deploy Log Analytics Agent for Windows VMs

## <a name="au-5-response-to-audit-processing-failures"></a>AU-5 Resposta a Falhas de Processamento de Auditoria

Este blueprint atribui cinco definições do [Azure Policy](../../../policy/overview.md) que monitoram configurações de auditoria e log de eventos. Monitorar essas configurações pode fornecer um indicador de falha ou de configuração incorreta do sistema de auditoria e ajudá-lo a adotar uma ação corretiva.

- [Visualização]: Monitor unaudited SQL servers in Azure Security Center
- Configuração de diagnóstico de auditoria
- Auditar instâncias gerenciadas de SQL sem Segurança de Dados Avançada
- Auditar configurações de Auditoria de nível do servidor SQL
- Auditar servidores SQL sem Segurança de Dados Avançada

## <a name="au-6-4-audit-review-analysis-and-reporting--central-review-and-analysis"></a>AU-6 (4) Revisão, Análise e Relatório de Auditoria | Revisão e Análise Central

Dados de log coletados pelo Azure Monitor são armazenados em um workspace do Log Analytics, permitindo análises e relatórios centralizados. Este blueprint ajuda a garantir que os eventos sejam registrados em log atribuindo sete definições do [Azure Policy](../../../policy/overview.md) que auditam e impõem a implantação do agente do Log Analytics em máquinas virtuais do Azure.

- [Visualização]: Auditar a implantação do Agente do Log Analytics – imagem de VM (sistema operacional) não listada
- [Visualização]: Auditar a implantação do Agente do Log Analytics no VMSS – imagem de VM (sistema operacional) não listada
- [Visualização]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Versão prévia]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Visualização]: Deploy Log Analytics Agent for Linux VMs
- [Versão prévia]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Visualização]: Deploy Log Analytics Agent for Windows VMs

## <a name="au-12-audit-generation"></a>AU-12 Geração de Auditoria

Esse blueprint ajuda você a garantir que os eventos do sistema sejam registrados em log por meio da atribuição de 15 definições do [Azure Policy](../../../policy/overview.md) que auditam as configurações do log de auditoria em recursos do Azure. Essas definições de política auditam e impõem a implantação do agente do Log Analytics em máquinas virtuais do Azure, bem como as configurações de auditoria para outros tipos de recursos do Azure. Essas definições de política também auditam a configuração dos logs de diagnóstico para fornecer informações sobre as operações executadas em recursos do Azure. Além disso, a auditoria e a Segurança de Dados Avançada são configuradas em servidores SQL.

- [Visualização]: Auditar a implantação do Agente do Log Analytics – imagem de VM (sistema operacional) não listada
- [Visualização]: Auditar a implantação do Agente do Log Analytics no VMSS – imagem de VM (sistema operacional) não listada
- [Visualização]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Versão prévia]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Visualização]: Deploy Log Analytics Agent for Linux VMs
- [Versão prévia]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Visualização]: Deploy Log Analytics Agent for Windows VMs
- [Visualização]: Monitor unaudited SQL servers in Azure Security Center
- Aplicar Configurações de Diagnóstico para Grupos de Segurança de Rede
- Configuração de diagnóstico de auditoria
- Auditar instâncias gerenciadas de SQL sem Segurança de Dados Avançada
- Auditar configurações de Auditoria de nível do servidor SQL
- Auditar servidores SQL sem Segurança de Dados Avançada
- Implantar Segurança de Dados Avançada em servidores SQL
- Implantar Auditoria em servidores SQL

## <a name="cm-7-2-least-functionality--prevent-program-execution"></a>CM-7 (2) Funcionalidade Mínima | Impedir a Execução do Programa

O controle de aplicativos adaptável na Central de Segurança do Azure é uma solução de lista de permissões inteligente, automatizada e de ponta a ponta que pode bloquear ou impedir a execução de softwares específicos em suas máquinas virtuais. O controle de aplicativos pode ser executado no modo de imposição, que proíbe a execução de aplicativos não aprovados. Esse blueprint atribui uma definição do Azure Policy que ajuda você a monitorar máquinas virtuais em que uma lista de permissões de aplicativos é recomendada, mas ainda não foi configurada.

- [Visualização]: Monitor possible app whitelisting in Azure Security Center

## <a name="cm-7-5-least-functionality--authorized-software--whitelisting"></a>CM-7 (5) Funcionalidade Mínima | Um Software autorizado / Lista de Permissões

O controle de aplicativos adaptável na Central de Segurança do Azure é uma solução de lista de permissões inteligente, automatizada e de ponta a ponta que pode bloquear ou impedir a execução de softwares específicos em suas máquinas virtuais. O controle de aplicativo ajuda a criar listas de aplicativos aprovados para suas máquinas virtuais. Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que ajuda você a monitorar máquinas virtuais em que uma lista de permissões de aplicativos é recomendada, mas ainda não foi configurada.

- [Visualização]: Monitor possible app whitelisting in Azure Security Center

## <a name="cm-11-user-installed-software"></a>CM-11 Software Instalado pelo Usuário

O controle de aplicativos adaptável na Central de Segurança do Azure é uma solução de lista de permissões inteligente, automatizada e de ponta a ponta que pode bloquear ou impedir a execução de softwares específicos em suas máquinas virtuais. O controle de aplicativos pode ajudar a impor e a monitorar a conformidade com políticas de restrição de software. Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que ajuda você a monitorar máquinas virtuais em que uma lista de permissões de aplicativos é recomendada, mas ainda não foi configurada.

- [Visualização]: Monitor possible app whitelisting in Azure Security Center

## <a name="cp-7-alternate-processing-site"></a>CP-7 Site de Processamento Alternativo

O Azure Site Recovery replica cargas de trabalho em execução em máquinas virtuais de uma localização primária para uma secundária. Se ocorrer uma interrupção no site primário, a carga de trabalho fará failover para a localização secundária. Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que audita máquinas virtuais sem a recuperação de desastre configurada. Monitorar esse indicador pode ajudá-lo a garantir que os controles de contingência necessários estejam em vigor.

- Auditar máquinas virtuais sem a recuperação de desastre configurada

## <a name="ia-2-1-identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>IA-2 (1) Identificação e Autenticação (Usuários Organizacionais) | Acesso à Rede para Contas com Privilégios

Esse blueprint ajuda a restringir e controlar o acesso privilegiado atribuindo duas definições do [Azure Policy](../../../policy/overview.md) para auditar contas com permissões de gravação e/ou proprietário que não têm a autenticação multifator habilitada. A autenticação multifator ajuda a proteger contas, mesmo que uma única informação de autenticação seja comprometida. Monitorando as contas sem a autenticação multifator habilitada, você pode identificar as contas que têm mais probabilidade de serem comprometidas.

- [Visualização]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Visualização]: Audit accounts with write permissions who are not MFA enabled on a subscription

## <a name="ia-2-2-identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>IA-2 (2) Identificação e Autenticação (Usuários Organizacionais) | Acesso à Rede para Contas sem Privilégios

Esse blueprint ajuda a restringir e controlar o acesso atribuindo uma definição do [Azure Policy](../../../policy/overview.md) para auditar contas com permissões de leitura que não têm a autenticação multifator habilitada. A autenticação multifator ajuda a proteger contas, mesmo que uma única informação de autenticação seja comprometida. Monitorando as contas sem a autenticação multifator habilitada, você pode identificar as contas que têm mais probabilidade de serem comprometidas.

- [Visualização]: Audit accounts with read permissions who are not MFA enabled on a subscription

## <a name="ia-5-authenticator-management"></a>IA-5 Gerenciamento de Autenticador

Esse blueprint atribui cinco definições do [Azure Policy](../../../policy/overview.md) que auditam Máquinas Virtuais do Linux que permitem conexões remotas de contas sem senhas e/ou têm permissões incorretas definidas no arquivo passwd. Esse blueprint também atribui uma definição de política que audita a conjugação do tipo de criptografia de senha para Máquinas Virtuais do Windows. Monitorar esses indicadores ajuda você a garantir que os autenticadores do sistema estejam em conformidade com a política de identificação e autenticação de sua organização.

- [Visualização]: Audit that Linux VMs do not have accounts without passwords
- [Visualização]: Deploy VM extension to audit that Linux VMs do not have accounts without passwords
- [Visualização]: Audit that Linux VMs have the passwd file permissions set to 0644
- [Visualização]: Audit that Windows VMs store passwords using reversible encryption
- [Visualização]: Deploy VM extension to audit that Linux VMs have the passwd file permissions set to 0644

## <a name="ia-5-1-authenticator-management--password-based-authentication"></a>IA-5 (1) Gerenciamento de Autenticador | Autenticação Baseada em Senha

Esse blueprint ajuda você a impor senhas fortes por meio da atribuição de 12 definições do [Azure Policy](../../../policy/overview.md) que auditam Máquinas Virtuais do Windows que não impõem a força mínima e outros requisitos de senha. O reconhecimento de máquinas virtuais que estão violando a política de força da senha ajuda você a tomar ações corretivas para garantir que as senhas de todas as contas de usuário da máquina virtual estejam em conformidade com a política de senha de sua organização.

- [Visualização]: Audit that Windows VMs cannot re-use the previous 24 passwords
- [Visualização]: Audit that Windows VMs have a maximum password age of 70 days
- [Visualização]: Audit that Windows VMs have a minimum password age of 1 day
- [Visualização]: Audit that Windows VMs have the password complexity setting enabled
- [Visualização]: Audit that Windows VMs restrict the minimum password length to 14 characters
- [Visualização]: Audit that Windows VMs store passwords using reversible encryption
- [Visualização]: Deploy VM extension to audit that Windows VMs cannot re-use the previous 24 passwords
- [Visualização]: Deploy VM extension to audit that Windows VMs have a maximum password age of 70 days
- [Visualização]: Deploy VM extension to audit that Windows VMs have a minimum password age of 1 day
- [Visualização]: Deploy VM extension to audit that Windows VMs have the password complexity setting enabled
- [Visualização]: Deploy VM extension to audit that Windows VMs restrict the minimum password length to 14 characters
- [Visualização]: Deploy VM extension to audit that Windows VMs store passwords using reversible encryption

## <a name="ra-5-vulnerability-scanning"></a>RA-5 Verificação de Vulnerabilidade

Esse blueprint ajuda você a gerenciar as vulnerabilidades do sistema de informações por meio da atribuição de quatro definições do [Azure Policy](../../../policy/overview.md) que monitoram vulnerabilidades do sistema operacional, vulnerabilidades do SQL e vulnerabilidades da máquina virtual na Central de Segurança do Azure. A Central de Segurança do Azure fornece funcionalidades de relatórios que permitem ter insights em tempo real sobre o estado de segurança de recursos implantados do Azure. Esse blueprint também atribui três definições de política que auditam e impõem a Segurança de Dados Avançada em servidores SQL. A segurança de dados avançada inclui recursos de avaliação de vulnerabilidade e proteção avançada contra ameaças para ajudá-lo a entender as vulnerabilidades de seus recursos implantados.

- Auditar instâncias gerenciadas de SQL sem Segurança de Dados Avançada
- Auditar servidores SQL sem Segurança de Dados Avançada
- Implantar Segurança de Dados Avançada em servidores SQL
- [Visualização]: Audit OS vulnerabilities on your virtual machine scale sets in Azure Security Center
- [Visualização]: Monitor OS vulnerabilities in Azure Security Center
- [Visualização]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Visualização]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="sc-5-denial-of-service-protection"></a>SC-5 Proteção Contra Negação de Serviço

O nível padrão da proteção contra DDoS (ataque de negação de serviço distribuído) do Azure fornece recursos adicionais e recursos de mitigação com relação à camada de serviço básica. Esses recursos adicionais incluem a integração do Azure Monitor e a capacidade de examinar relatórios de mitigação pós-ataque. Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que audita se o nível padrão da proteção contra DDoS está habilitada. Compreender a diferença de funcionalidade entre as camadas de serviço pode ajudar você a selecionar a melhor solução para tratar das proteções contra negação de serviço para seu ambiente do Azure.

- [Visualização]: Audit standard tier of DDoS protection is enabled for a virtual network

## <a name="sc-7-boundary-protection"></a>SC-7 Proteção de Limite

Esse blueprint ajuda você a gerenciar e controlar o limite do sistema atribuindo uma definição do [Azure Policy](../../../policy/overview.md) que monitora os grupos de segurança de rede com regras permissivas. Regras que são muito permissivas podem permitir o acesso não intencional à rede e devem ser examinadas. Esse blueprint também atribui uma definição de política que monitora recomendações de proteção do grupo de segurança de rede na Central de Segurança do Azure. A Central de Segurança do Azure analisa padrões de tráfego de máquinas virtuais voltadas para a Internet e fornece recomendações de regras de grupo de segurança de rede para reduzir a superfície de ataque potencial.
Além disso, esse blueprint também atribui três definições do Azure Policy que monitoram pontos de extremidade, contas de armazenamento e aplicativos desprotegidos. Os pontos de extremidade e os aplicativos que não estão protegidos por um firewall e as contas de armazenamento com acesso irrestrito podem permitir o acesso não intencional às informações contidas no sistema de informações.

- [Visualização]: Monitor Internet-facing virtual machines for Network Security Group traffic hardening recommendations
- [Visualização]: Monitor permissive network access in Azure Security Center
- [Visualização]: Monitor unprotected network endpoints in Azure Security Center
- [Visualização]: Monitor unprotected web application in Azure Security Center
- Auditar o acesso irrestrito à rede para contas de armazenamento

## <a name="sc-7-3-boundary-protection--access-points"></a>SC-7 (3) Proteção de Limite | Pontos de Acesso

O acesso JIT (Just-In-Time) à máquina virtual bloqueia o tráfego de entrada às máquinas virtuais do Azure, reduzindo a exposição a ataques enquanto fornece acesso fácil para se conectar às VMs quando necessário. O acesso JIT à máquina virtual ajuda a limitar o número de conexões externas a seus recursos no Azure. Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que ajuda você a monitorar máquinas virtuais que podem dar suporte ao acesso Just-In-Time, mas ainda não foram configuradas.

- [Visualização]: Monitorar o possível acesso Just In Time (JIT) à rede na Central de Segurança do Azure

## <a name="sc-7-4-boundary-protection--external-telecommunications-services"></a>SC-7 (4) Proteção de Limite | Serviços de Telecomunicações Externos

O acesso JIT (Just-In-Time) à máquina virtual bloqueia o tráfego de entrada às máquinas virtuais do Azure, reduzindo a exposição a ataques enquanto fornece acesso fácil para se conectar às VMs quando necessário. O acesso JIT à máquina virtual ajuda você a gerenciar exceções à sua política de fluxo de tráfego facilitando os processos de solicitação e aprovação de acesso. Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que ajuda você a monitorar máquinas virtuais que podem dar suporte ao acesso Just-In-Time, mas ainda não foram configuradas.

- [Visualização]: Monitorar o possível acesso Just In Time (JIT) à rede na Central de Segurança do Azure

## <a name="sc-28-1-protection-of-information-at-rest--cryptographic-protection"></a>SC-28 (1) Proteção de Informações de em Repouso | Proteção de Criptografia

Esse blueprint ajuda você a impor sua política sobre o uso de controles de criptografia para proteger informações em repouso por meio da atribuição de 9 definições do [Azure Policy](../../../policy/overview.md) que impõem controles de criptografia específicos e auditam o uso de configurações de criptografia fraca. Entender em que local os recursos do Azure podem ter configurações de criptografia não ideais pode ajudá-lo a tomar ações corretivas para garantir que os recursos sejam configurados de acordo com a política de segurança de informações. Especificamente, as definições de política atribuídas por esse blueprint exigem criptografia para contas do Data Lake Storage; exigem Transparent Data Encryption em bancos de dados SQL e auditam quanto à falta de criptografia em bancos de dados SQL, discos de máquina virtual e variáveis de conta de automação.

- [Visualização]: Monitor unencrypted SQL databases in Azure Security Center
- [Visualização]: Monitor unencrypted VM Disks in Azure Security Center
- Auditar transferência segura para contas de armazenamento
- Auditar instâncias gerenciadas de SQL sem Segurança de Dados Avançada
- Auditar servidores SQL sem Segurança de Dados Avançada
- Auditar o status da Transparent Data Encryption
- Implantar Segurança de Dados Avançada em servidores SQL
- Implantar a Transparent Data Encryption no BD SQL
- Impor a criptografia em contas do Data Lake Storage

## <a name="si-2-flaw-remediation"></a>SI-2 Correção de Falhas

Esse blueprint ajuda você a gerenciar falhas do sistema de informações atribuindo seis definições do [Azure Policy](../../../policy/overview.md) que monitoram atualizações ausentes do sistema, vulnerabilidades do sistema operacional, vulnerabilidades do SQL e vulnerabilidades da máquina virtual na Central de Segurança do Azure. A Central de Segurança do Azure fornece funcionalidades de relatórios que permitem ter insights em tempo real sobre o estado de segurança de recursos implantados do Azure. Esse blueprint também atribui uma definição de política que garante a atualização automática do sistema operacional para conjuntos de dimensionamento de máquinas virtuais.

- [Visualização]: Audit any missing system updates on virtual machine scale sets in Azure Security Center
- [Visualização]: Audit OS vulnerabilities on your virtual machine scale sets in Azure Security Center
- [Visualização]: Monitor missing system updates in Azure Security Center
- [Visualização]: Monitor OS vulnerabilities in Azure Security Center
- [Visualização]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Visualização]: Monitor VM Vulnerabilities in Azure Security Center
- Impor atualização automática do sistema operacional com verificações de integridade do aplicativo em VMSS

## <a name="si-3-malicious-code-protection"></a>SI-3 Proteção contra código mal-intencionado

Esse blueprint ajuda a gerenciar a proteção de ponto de extremidade, incluindo a proteção contra código mal-intencionado, atribuindo três definições do [Azure Policy](../../../policy/overview.md) que monitoram quanto à ausência de proteção de ponto de extremidade em máquinas virtuais na Central de Segurança do Azure e impõem a solução antimalware da Microsoft em Máquinas Virtuais do Windows.

- [Visualização]: Audit the endpoint protection solution on virtual machine scale sets in Azure Security Center
- [Visualização]: Monitor missing Endpoint Protection in Azure Security Center
- Implantar a extensão padrão antimalware de IaaS da Microsoft para Windows Server

## <a name="si-3-1-malicious-code-protection--central-management"></a>SI-3 (1) Proteção contra código mal-intencionado | Gerenciamento central

Esse blueprint ajuda a gerenciar a proteção de ponto de extremidade, incluindo a proteção contra código mal-intencionado, atribuindo duas definições do [Azure Policy](../../../policy/overview.md) que monitoram quanto à ausência de proteção de ponto de extremidade em máquinas virtuais na Central de Segurança do Azure. A Central de Segurança do Azure fornece funcionalidades centralizadas de gerenciamento e relatórios que permitem ter insights em tempo real sobre o estado de segurança de recursos implantados do Azure.

- [Visualização]: Audit the endpoint protection solution on virtual machine scale sets in Azure Security Center
- [Visualização]: Monitor missing Endpoint Protection in Azure Security Center

## <a name="si-4-information-system-monitoring"></a>SI-4 Monitoramento do sistema de informações

Esse blueprint ajuda a monitorar seu sistema por meio da auditoria e da imposição do registro em log e da segurança de dados em todos os recursos do Azure. Especificamente, as políticas atribuídas auditam e impõem a implantação do agente do Log Analytics e de configurações de segurança aprimoradas para bancos de dados SQL, contas de armazenamento e recursos de rede. Esses recursos podem ajudar você a detectar comportamentos anormais e indicadores de ataques para que possa tomar as devidas providências.

- [Visualização]: Auditar a implantação do Agente do Log Analytics – imagem de VM (sistema operacional) não listada
- [Visualização]: Auditar a implantação do Agente do Log Analytics no VMSS – imagem de VM (sistema operacional) não listada
- [Visualização]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Versão prévia]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Visualização]: Deploy Log Analytics Agent for Linux VMs
- [Versão prévia]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Visualização]: Deploy Log Analytics Agent for Windows VMs
- Auditar instâncias gerenciadas de SQL sem Segurança de Dados Avançada
- Auditar servidores SQL sem Segurança de Dados Avançada
- Implantar Segurança de Dados Avançada em servidores SQL
- Implantar a Proteção Avançada contra Ameaças em Contas de Armazenamento
- Implantar Auditoria em servidores SQL
- Implantar o observador de rede quando redes virtuais são criadas
- Implantar a Detecção de Ameaças em servidores SQL

## <a name="si-4-18-information-system-monitoring--analyze-traffic--covert-exfiltration"></a>SI-4 (18) Monitoramento do sistema de informações | Analisar o tráfego/vazamento oculto

A Proteção Avançada contra Ameaças do Armazenamento do Azure detecta tentativas incomuns e potencialmente prejudiciais de acessar ou explorar contas de armazenamento. Os alertas de proteção incluem padrões de acesso anormais, extrações/carregamentos anormais e atividade de armazenamento suspeita. Esses indicadores podem ajudá-lo a detectar vazamento oculto de informações.

- Implantar a Proteção Avançada contra Ameaças em Contas de Armazenamento

## <a name="next-steps"></a>Próximas etapas

Outros artigos sobre blueprints e como usá-los:

- Saiba mais sobre o [ciclo de vida do blueprint](../../concepts/lifecycle.md).
- Saiba como usar [parâmetros estáticos e dinâmicos](../../concepts/parameters.md).
- Saiba como personalizar a [ordem de sequenciamento de blueprint](../../concepts/sequencing-order.md).
- Saiba como usar o [bloqueio de recurso de blueprint](../../concepts/resource-locking.md).
- Saiba como [atualizar atribuições existentes](../../how-to/update-existing-assignments.md).
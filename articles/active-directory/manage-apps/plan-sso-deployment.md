---
title: Planejar uma única do Active Directory do Azure logon implantação
description: Guia para ajudá-lo a planejar, implantar e gerenciar o logon único em sua organização.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd8cebbd7c60715bc90412d9f53458edfee6c56d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108196"
---
# <a name="plan-a-single-sign-on-deployment"></a>Planejar uma implantação de logon única

Logon único (SSO) significa acessar todos os aplicativos e recursos de um usuário precisa entrando em uma única vez usando uma conta de usuário único. Com o SSO, os usuários podem acessar aplicativos necessários tudo sem a exigência para autenticar uma segunda vez.

## <a name="benefits-of-sso"></a>Benefícios do SSO

Logon único (SSO) adiciona segurança e conveniência quando os usuários entram aplicativos no Azure Active Directory (Azure AD). 

Muitas organizações contam com o software como um aplicativo de serviço (SaaS), como o Office 365, Box e Salesforce, para produtividade do usuário final. Historicamente, a equipe de TI necessários para criar e atualizar contas de usuário em cada aplicativo SaaS e os usuários necessários para lembrar uma senha para cada um individualmente.

O Azure Marketplace possui mais de 3.000 aplicativos com conexões pré-integrados de SSO, facilitando a integrá-los em seu locatário.

## <a name="licensing"></a>Licenciamento

- **Licenciamento do AD do Azure** -SSO para aplicativos SaaS pré-integrados é gratuito. No entanto, o número de objetos no seu diretório e os recursos que você deseja implantar pode exigir licenças adicionais. Para obter uma lista completa dos requisitos de licença, consulte [preços do Active Directory do Azure](https://azure.microsoft.com/pricing/details/active-directory/).
- **Licenciamento de aplicativos** -você precisará as licenças apropriadas para seus aplicativos de SaaS atender às suas necessidades de negócios. Trabalhar com o proprietário do aplicativo para determinar se os usuários atribuídos ao aplicativo têm as licenças apropriadas para suas funções dentro do aplicativo. Se o Azure AD gerencia o provisionamento automático com base em funções, as funções atribuídas no Azure AD devem se alinhar com o número de licenças possuídas dentro do aplicativo. Número incorreto de licenças possuídas no aplicativo pode levar a erros durante a provisionamento/atualização de um usuário.

## <a name="plan-your-sso-team"></a>Planejar a sua equipe SSO

- **Entre em contato com os participantes do direito** – quando a falha, dos projetos de tecnologia é normalmente devido a expectativas incompatíveis no impacto, resultados e responsabilidades. Para evitar essas armadilhas [Certifique-se de que você está envolver os participantes do direito](https://aka.ms/deploymentplans) e que os participantes entendam suas funções.
- **Planejar a comunicação** -a comunicação é crítica para o sucesso de qualquer novo serviço. Comunicar-se proativamente para seus usuários limitam como sua experiência será alterado, quando ele será alterado e como obter suporte se enfrentarem problemas. Examine as opções para [aplicativos habilitados para como os usuários finais terão acesso a seu SSO](end-user-experiences.md)e criar suas comunicações de acordo com sua seleção. 

## <a name="plan-your-sso-protocol"></a>Planejar seu protocolo SSO

Uma implementação de SSO baseada em protocolos de Federação aprimora a segurança, confiabilidade, e experiências de usuário final e é mais fácil de implementar. Muitos aplicativos são previamente integrados no Azure AD com o [passo a passo orienta disponíveis](../saas-apps/tutorial-list.md). Você pode encontrá-los em nossa [do Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). Informações detalhadas sobre cada método de SSO podem ser encontradas no artigo [logon único para aplicativos no Azure Active Directory](what-is-single-sign-on.md).

Há duas maneiras principais no qual você pode habilitar seus usuários para logon único para seus aplicativos:

- **Com o logon único federado** do Azure AD autentica o usuário para o aplicativo usando sua conta do Azure AD. Esse método tem suporte para aplicativos que suporte a protocolos como SAML 2.0, WS-Federation ou OpenID Connect e é o melhor modo de logon único. É recomendável usar o SSO federado com o Azure AD quando um aplicativo é compatível com ele, em vez de SSO baseado em senha e ADFS.

- **Com baseado em senha de logon único** os usuários entram no aplicativo com um nome de usuário e senha na primeira vez, eles acessá-lo. Após o primeiro logon, o Azure AD fornece o nome de usuário e a senha ao aplicativo. O logon único baseado em senha permite o armazenamento e a reprodução segura de senhas do aplicativo usando uma extensão de navegador da Web ou aplicativo móvel. Essa opção utiliza o processo de entrada existente fornecido pelo aplicativo, permite que um administrador gerenciar as senhas e não exige que o usuário saiba a senha.

### <a name="considerations-for-federation-based-sso"></a>Considerações para SSO baseado em federação

- **Usando o OAuth e OpenID Connect** - se o aplicativo que você está se conectando dá suporte a ele, use o método OIDC/OAuth 2.0 para habilitar o SSO para esse aplicativo. Esse método requer menos configuração e permite uma experiência de usuário mais rica. Para obter mais informações, consulte [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md), [OpenID Connect 1.0](../develop/v2-protocols-oidc.md), e [guia do desenvolvedor do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).
- **Configurações de ponto de extremidade para que o SSO baseado em SAML** -se você usar SAML, seus desenvolvedores precisará de informações específicas antes de configurar o aplicativo. Para obter mais informações, consulte [configurar opções básicas de SAML](configure-single-sign-on-portal.md).
- **Certificado de gerenciamento para que o SSO baseado em SAML** – quando você habilita o SSO federado para seu aplicativo, o Azure AD cria um certificado que é por padrão, válida por três anos. Você pode personalizar a data de validade para esse certificado, se necessário. Certifique-se de que você tem processos para renovar os certificados antes de sua expiração. Para obter mais informações, consulte [certificados de gerenciamento do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs).

### <a name="considerations-for-password-based-sso"></a>Considerações para SSO baseado em senha

Usando o Azure AD para SSO baseado em senha exige a implantação de uma extensão de navegador que irá recuperar as credenciais com segurança e preencher os formulários de logon. Definir um mecanismo para implantar a extensão em escala com [navegadores com suporte](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction). As opções incluem:

- [Política de grupo para o Internet Explorer ](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)
- [System Center Configuration Manager (SCCM) para o Internet Explorer ](https://docs.microsoft.com/sccm/core/clients/deploy/deploy-clients-to-windows-computers)
- [Usuário controlado por download e configuração para o IE, Firefox, Microsoft Edge ou Chrome ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Para obter mais informações, consulte [como configurar senha logon único](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery).

#### <a name="capturing-login-forms-metadata-for-applications-that-arent-in-the-gallery"></a>Captura de metadados de formulários de logon para aplicativos que não estão na Galeria

A Microsoft oferece suporte a metadados de captura em um aplicativo web para compartimentação de senhas (capturando os campos nome de usuário e senha). Navegue até a URL de logon durante o processo de configuração do aplicativo para capturar os metadados de formulários. Peça ao proprietário do aplicativo para a URL de logon exata. Essas informações são usadas durante o processo de logon, o mapeamento de credenciais do Azure AD para o aplicativo durante o logon.

Para obter mais informações, consulte [o que é o acesso a aplicativos e SSO com o Azure AD? – SSO baseado em senha](https://azure.microsoft.com/documentation/articles/active-directory-appssoaccess-whatis/).

#### <a name="indications-that-metadata-in-forms-needs-to-be-recaptured"></a>Indicações de que os metadados em formulários precisam ser retomou

Quando aplicativos alteram o layout HTML, você precisa recapturar os metadados para ajustar para que as alterações. Sintomas comuns que indicam que o layout HTML tem alteração incluem:

- Usuários que clicar no aplicativo de relatórios obtém ficou "preso" na página de logon
- Usuários que o nome de usuário ou a senha não é preenchida de relatórios

#### <a name="shared-accounts"></a>Contas compartilhadas

Da perspectiva do sinal, aplicativos com contas compartilhadas não são diferentes de um aplicativo de galeria que usa o SSO de senha para usuários individuais. No entanto, há algumas etapas adicionais necessárias ao planejamento e configuração de um aplicativo devem usar contas compartilhadas:

1. Trabalhar com usuários de negócios do aplicativo para documentar o seguinte:
   1. Conjunto de usuários da organização que usará o aplicativo
   1. Conjunto de credenciais no aplicativo associado com o conjunto de usuários existentes 
1. Para cada combinação de conjunto de usuários e credenciais, crie um grupo de segurança na nuvem ou local com base em seus requisitos.
1. Redefina as credenciais compartilhadas. Depois que o aplicativo é implantado no Azure AD, indivíduos não precisam da senha de conta compartilhada. Uma vez que a senha será armazenada no AD do Azure, considere definir que ele seja muito longo e complexo. 
1. Configure substituição automática da senha, se o aplicativo dá suporte a ele. Dessa forma, nem mesmo o quem fez a configuração inicial do administrador será saber a senha da conta compartilhada. 

## <a name="plan-your-authentication-method"></a>Planejar seu método de autenticação

Escolher o método de autenticação correto é uma primeira decisão fundamental na configuração de uma solução de identidade híbrida do Azure AD. Implemente o método de autenticação que é configurado usando o Azure AD Connect, que também provisiona usuários na nuvem.

Para escolher um método de autenticação, é necessário considerar o tempo, a infraestrutura existente, a complexidade e o custo de implementação de sua escolha. Esses fatores são diferentes para cada organização e podem mudar ao longo do tempo. Você deve escolher aquele que mais se aproxima seu cenário específico. Para obter mais informações, consulte [escolha o método de autenticação correto para sua solução de identidade híbrida do Azure Active Directory](https://docs.microsoft.com/azure/security/azure-ad-choose-authn).

## <a name="plan-your-security-and-governance"></a>Planejar a segurança e governança 

Identidade é dinâmico primário novo para segurança atenção e investimentos em porque perímetros de rede têm se tornado cada vez mais porosos e menos eficiente com a explosão de dispositivos BYOD e aplicativos de nuvem. 

### <a name="plan-access-reviews"></a>Revisões de acesso do plano

[Revisões de acesso](https://docs.microsoft.com/azure/active-directory/governance/create-access-review) permitem às organizações gerenciar participações de maneira eficiente, acesso a aplicativos empresariais e atribuições de função. Você deve planejar para revisar o acesso de usuário regularmente para garantir que somente as pessoas corretas têm acesso contínuo.

Alguns dos principais tópicos para planejar durante a configuração revisões de acesso incluem:

1. Identificando uma cadência de revisões de acesso que atenda às suas necessidades de negócios. Isso pode ser tão frequente como uma vez por semana, mensalmente, anualmente ou como um exercício de sob demanda.

1. Crie grupos que representam os revisores dos relatórios de acesso do aplicativo. Você precisará garantir que os participantes mais familiarizados com o aplicativo e seus usuários de destino e casos de uso são participantes em suas revisões de acesso

1. Concluir uma revisão de acesso inclui a possibilidade de eliminar as permissões de acesso do aplicativo aos usuários que não precisam de acesso. Plano para lidar com solicitações de suporte de potencial de usuários negados. Um usuário excluído permanecerá excluído do AD do Azure por 30 dias, período durante o qual eles podem ser restaurados por um administrador se necessário. Após 30 dias, esse usuário será excluído permanentemente. Usando o portal do Azure Active Directory, um Administrador Global pode explicitamente excluir permanentemente um usuário excluído recentemente antes desse período de tempo for atingido.

### <a name="plan-auditing"></a>Plano de auditoria

O Azure AD fornece [relatórios que contêm informações técnicas e comerciais](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/). 

Segurança e relatórios de atividade estão disponíveis. Relatórios de segurança mostram que os usuários sinalizados como risco e entradas arriscadas. Relatórios de atividade de ajudam você a entender o comportamento dos usuários em sua organização, detalhando a atividade de entrada e fornecendo as trilhas de auditoria de todos os logons. Você pode usar relatórios para gerenciar riscos, aumentar a produtividade e monitorar a conformidade.

| Tipo de relatório | Revisão de acesso | Relatórios de segurança | Relatório de entrada |
|-------------|---------------|------------------|----------------|
| Use para examinar | Permissões de aplicativo e o uso. | Contas potencialmente comprometidas | Quem está acessando os aplicativos |
| Ações possíveis | Auditoria de acesso; Revogar permissões | Revogar o acesso; forçar a redefinição de segurança | Revogar o acesso |

Azure AD retém a maioria dos dados de auditoria por 30 dias e disponibiliza os dados por meio do portal de administração do Azure ou por meio de uma API para download em seus sistemas de análise.

### <a name="consider-using-microsoft-cloud-application-security"></a>Considere o uso de segurança de aplicativo do Microsoft Cloud

Segurança de aplicativo de nuvem da Microsoft (MCAS) é uma solução de agente de segurança de acesso de nuvem (CASB). Ele proporciona visibilidade de seus serviços e aplicativos de nuvem, fornece análise sofisticada para identificar e combater ameaças cibernéticas e permite que você controle como os seus dados são transferidos.

Implantar o MCAS permite que você:

- Use o Cloud Discovery para mapear e identificar seu ambiente de nuvem e os aplicativos de nuvem que sua organização está usando.
- Sancionar e Cancelar sanção de aplicativos em sua nuvem
- Use os conectores de fácil de implantar aplicativos que aproveitam APIs do provedor para visibilidade e controle dos aplicativos que você se conectar ao
- Usar a proteção de controle de aplicativo de acesso condicional para obter visibilidade em tempo real e controle de acesso e as atividades dentro de seus aplicativos de nuvem
- Ajuda você a ter controle contínuo de configuração e, em seguida, ajustar continuamente, políticas.

Controle de sessão de segurança de aplicativo de nuvem da Microsoft (MCAS) está disponível para qualquer navegador em qualquer plataforma principal em qualquer sistema operacional. Aplicativos móveis e aplicativos da área de trabalho também podem ser bloqueados ou permitidos. Com a integração nativa com o Azure AD, quaisquer aplicativos que estão configurados com SAML ou Open ID Connect com o logon único do Azure AD podem ter suporte, incluindo [vários aplicativos em destaque](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad).

Para obter informações sobre MCAS, consulte o [visão geral do Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). MCAS é um serviço de assinatura com base no usuário. Você pode examinar os detalhes de licenciamento na [folha de dados de licenciamento do MCAS](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2NXYO).

### <a name="use-conditional-access"></a>Use o acesso condicional

Com acesso condicional, você pode automatizar decisões de controle de acesso com base em critérios para seus aplicativos de nuvem.

Políticas de acesso condicional são impostas após a autenticação multifator primeiro ter sido concluída. Portanto, acesso condicional não foi projetado como uma primeira linha de defesa para cenários, como ataques de (DoS) de negação de serviço, mas pode usar os sinais desses eventos para determinar o acesso. Por exemplo, o nível de risco de entrada, pode ser usado o local da solicitação e assim por diante. Para obter mais informações sobre o acesso condicional, consulte [visão geral](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access) e o [plano de implantação](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access).

## <a name="azure-sso-technical-requirements"></a>Requisitos técnicos de SSO do Azure

A seção a seguir fornece detalhes sobre os requisitos para configurar seu aplicativo específico, incluindo ambientes necessárias, pontos de extremidade, mapeamento de declaração, os atributos necessários, certificados e protocolos usados. Você precisará dessas informações para configurar o SSO na [portal do Azure AD](https://portal.azure.com/).

### <a name="authentication-mechanism-details"></a>Detalhes do mecanismo de autenticação

Para todos os aplicativos SaaS pré-integrados, a Microsoft fornece um tutorial e você não precisará dessas informações. Se o aplicativo não está em nosso marketplace de aplicativos / galeria, talvez você precise coletar as seguintes partes de dados:

- **Provedor de identidade atual, o aplicativo usa para que o SSO, se aplicável** , por exemplo: AD FS, PingFederate, Okta
- **Protocolos com suporte pelo aplicativo de destino** - por exemplo, SAML 2.0, OpenID Connect, OAuth, autenticação baseada em formulários, WS-Fed, WS-Trust
- **Protocolo que está sendo configurado com o Azure AD** - por exemplo, SAML 2.0 ou 1.1, OpenID Connect, OAuth, baseada em formulários, WS-Fed

### <a name="attribute-requirements"></a>Requisitos de atributo

Há um conjunto pré-configurado de atributos e mapeamentos de atributo entre objetos de usuário do Azure AD e objetos de usuário de cada aplicativo SaaS. Alguns aplicativos gerenciam outros tipos de objetos, como grupos. Planejar o mapeamento de atributos de usuário do AD do Azure para seu aplicativo e [personalizar os mapeamentos de atributo padrão](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) acordo com seu negócio precisa.

### <a name="certificate-requirements"></a>Requisitos de certificado

O certificado para o aplicativo deve ser atualizado ou há um risco dos usuários não conseguem acessar o aplicativo. A maioria dos certificados de aplicativo de SaaS são bons para 36 meses. Você alterar essa duração de certificado na folha do aplicativo. Certifique-se de documentar a expiração e saber como você gerenciará sua renovação de certificado. 

Há duas maneiras para gerenciar seus certificados. 

- **Substituição automática de certificado** -a Microsoft suporta [substituição de chave de assinatura do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Enquanto esse é nosso método preferencial para gerenciar certificados, nem todos os ISVS dá suporte a esse cenário.

- **Atualizando manualmente** -cada aplicativo tem seu próprio certificado expira com base em como ele está definido. Antes de vencimento do certificado do aplicativo, crie um novo certificado e enviá-lo para o ISV. Essa informação pode ser extraída dos metadados de Federação. [Leia mais sobre metadados de Federação aqui.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata)

## <a name="implement-sso"></a>Implementar o SSO

Use as seguintes fases planejar e implantar sua solução em sua organização:

### <a name="user-configuration-for-sso"></a>Configuração do usuário para que o SSO

- **Identificar os usuários de teste**

   Entre em contato com o proprietário do aplicativo e peça-lhe para criar um mínimo de usuários de teste de três dentro do aplicativo. Verifique se as informações que você usará como o identificador primário são preenchidas corretamente e corresponde a um atributo que está disponível no Azure AD. Na maioria dos casos, isso será mapeado para "NameID" para aplicativos baseados em SAML. Para os tokens JWT, ele é "preferred_username".
   
   Criar o usuário no Azure AD seja manualmente como um usuário baseado em nuvem ou sincronização do usuário do local usando o mecanismo de sincronização do Azure AD Connect. Verifique se que as informações correspondem as declarações que está sendo enviadas para o aplicativo.

- **Configurar o SSO**

   Dos [lista de aplicativos](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list), localizar e abrir o tutorial SSO para seu aplicativo e siga as etapas deste tutorial no configurar com êxito seu aplicativo de SaaS.

   Se você não conseguir localizar seu aplicativo, consulte [documentação do aplicativo personalizado](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Isso orientará você sobre como adicionar um aplicativo que não está localizado na Galeria do Azure AD.

   Opcionalmente, você pode usar as declarações emitidas no token SAML para o aplicativo empresarial usando [documentação de diretrizes da Microsoft](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping). Certifique-se de que isso é mapeado para o que você espera receber na resposta SAML para seu aplicativo. Se você encontrar problemas durante a configuração, use nossas diretrizes sobre [como a integração de SSO depurar](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging).

Integração de aplicativo personalizado é um recurso de licenças do Azure AD Premium P1 ou P2.

### <a name="provide-sso-change-communications-to-end-users"></a>Fornecer comunicações de alteração SSO para usuários finais

Implemente seu plano de comunicação. Verifique se você está permitindo que os usuários finais saber que uma alteração é proveniente, quando chegou, o que fazer agora e como buscar assistência.

### <a name="verify-end-user-scenarios-for-sso"></a>Verifique se a cenários de usuário final para SSO

Você pode usar os seguintes casos de teste para realização de testes em corporativos e dispositivos pessoais para garantir que suas configurações de SSO estão funcionando conforme o esperado. Os cenários a seguir pressupõem que um usuário está navegando até uma URL de aplicativo e passar por um fluxo de autenticação iniciado pelo provedor de serviço (fluxo de autenticação iniciada pelo SP).

| Cenário | Resultado esperado no fluxo de autenticação iniciada pelo SP pelo usuário |
|----------|---------------------------------------------------|
| Faça logon no aplicativo com o IE enquanto estiver na rede corporativa. | Autenticação integrada do Windows (IWA) ocorre sem prompts adicionais. |
| Faça logon no aplicativo com o IE enquanto estiver fora da rede corporativa com nova tentativa de logon. | Prompt de baseada em formulários no servidor do AD FS. Usuário faz logon com êxito no e o navegador solicita para MFA. |
| Faça logon no aplicativo com o IE enquanto estiver fora da rede corporativa com uma sessão atual e nunca tenha realizado a MFA. | Usuário não receberá o prompt para o primeiro fator. O usuário recebe prompt para MFA. |
| Faça logon no aplicativo com o IE enquanto estiver fora da rede corporativa com uma sessão atual e já tenha realizado a MFA nesta sessão. | Usuário não receberá o prompt para o primeiro fator. Usuário não receberá a MFA. SSOs do usuário no aplicativo. |
| Faça logon no aplicativo com o Firefox/Chrome/Safari, enquanto estiver fora da rede corporativa com uma sessão atual e já tenha realizado a MFA nesta sessão. | Usuário não receberá o prompt para o primeiro fator. Usuário não receberá a MFA. Usuário do SSO no aplicativo. |
| Faça logon no aplicativo com o Firefox/Chrome/Safari, enquanto estiver fora da rede corporativa com nova tentativa de logon. | Prompt de baseada em formulários no servidor do AD FS. Usuário faz logon com êxito no e o navegador solicita para MFA. |
| Faça logon no aplicativo com o Chrome/Firefox enquanto estiver na rede corporativa com uma sessão atual. | Usuário não receberá o prompt para o primeiro fator. Usuário não receberá a MFA. Usuário do SSO no aplicativo. |
| Faça logon no aplicativo com o aplicativo móvel do aplicativo com uma nova tentativa de logon. | Prompt de baseada em formulários no servidor do AD FS. Usuário faz logon com êxito no e cliente ADAL solicita para MFA. |
| Usuário não autorizado tenta fazer logon no aplicativo com a URL de logon. | Prompt de baseada em formulários no servidor do AD FS. Usuário não conseguir fazer logon com o primeiro fator. |
| Usuário autorizado tenta fazer logon, mas digita uma senha incorreta. | Usuário navega para a URL do aplicativo e recebe o erro de nome de usuário/senha inválidos. |
| Usuário autorizado clica no link em um email e já está autenticado. | Usuário clica na URL e está conectado ao aplicativo sem prompts adicionais. |
| Usuário autorizado clica no link em um email e ainda não está autenticado. | Usuário clica na URL e é solicitado a autenticar com o primeiro fator. |
| Autorizado usuário faz logon no aplicativo com o aplicativo móvel do aplicativo (iniciado por SP) com uma nova tentativa de logon. | Prompt de baseada em formulários no servidor do AD FS. Usuário faz logon com êxito no e cliente ADAL solicita para MFA. |
| Usuário não autorizado tenta fazer logon no aplicativo com a URL de logon (iniciado por SP). | Prompt de baseada em formulários no servidor do AD FS. Usuário não conseguir fazer logon com o primeiro fator. |
| Usuário autorizado tenta fazer logon, mas digita uma senha incorreta.| Usuário navega para a URL do aplicativo e recebe o erro de nome de usuário/senha inválidos. |
| Usuário autorizado faça logoff e, em seguida, registra novamente. | Se a URL de saída estiver configurado, o usuário está conectado fora de todos os serviços e o prompt para autenticar. |
| Usuário autorizado faça logoff e, em seguida, registra novamente. | Se a URL de saída não estiver configurado, usuário será automaticamente registrado na usando token existente da sessão de navegador existente do AD do Azure. |
| Usuário autorizado clica no link em um email e já está autenticado. | Usuário clica na URL e está conectado ao aplicativo sem prompts adicionais. |
| Usuário autorizado clica no link em um email e ainda não está autenticado. | Usuário clica na URL e é solicitado a autenticar com o primeiro fator. |

## <a name="manage-sso"></a>Gerenciar logon único

Esta seção descreve os requisitos e recomendações para gerenciar com êxito o SSO.

### <a name="required-administrative-roles"></a>Funções administrativas necessárias

Sempre use a função com o mínimo de permissões disponível para realizar a tarefa necessária no Azure Active Directory. A Microsoft recomenda [examinar as diferentes funções que estão disponíveis](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) e escolher o correto para atender às suas necessidades de cada pessoa para este aplicativo. Algumas funções talvez precise ser aplicados temporariamente e removidos após a implantação foi concluída.

| Persona| Funções | Função do AD do Azure (se necessário) |
|--------|-------|-----------------------------|
| Ajudar o administrador de suporte técnico | Suporte de camada 1 | Nenhum |
| Administrador de identidade | Configurar e depurar o impacto de problemas do Azure AD | Administrador global |
| Administrador de aplicativo | Atestado de usuário no aplicativo, a configuração em que os usuários com permissões | Nenhum |
| Administradores de infraestrutura | Proprietário de substituição do certificado | Administrador global |
| Proprietário/participantes de negócios | Atestado de usuário no aplicativo, a configuração em que os usuários com permissões | Nenhum |

É recomendável usar [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) (PIM) para gerenciar suas funções para fornecer a revisar a auditoria, controle e acesso adicional para os usuários com permissões de diretório.

### <a name="sso-certificate-lifecycle-management"></a>Gerenciamento de ciclo de vida do certificado SSO

É importante identificar as funções certas e a tarefa de gerenciar o ciclo de vida do certificado de autenticação entre o Azure AD de listas de distribuição de email e o aplicativo que está sendo configurado com o logon único. Aqui estão algumas das principais funções, que é recomendável ter em vigor:

- Proprietário para atualizar as propriedades do usuário no aplicativo
- Proprietário da chamada para suporte de reparos do aplicativo
- Lista de distribuição de email monitorado de perto as notificações de alteração relacionadas ao certificado

O tempo de vida máximo de um certificado é de três anos. É recomendável estabelecer um processo de como você vai lidar com uma alteração de certificado entre o Azure AD e o seu aplicativo. Isso pode ajudar a evitar ou minimizar uma interrupção devido à expiração de um certificado ou forçar a substituição do certificado.

### <a name="rollback-process"></a>Processo de reversão

Depois de concluir o teste com base em seus casos de teste, é hora de mover para a produção com o seu aplicativo. Movendo para a produção significa que você irá implementar suas configurações planejadas e testadas em seu locatário de produção e distribua-o a seus usuários. No entanto, é importante planejar o que fazer caso sua implantação não funcionar conforme o planejado. Se a configuração do SSO falhar durante a implantação, você deve compreender como atenuar qualquer interrupção e reduzir o impacto para os usuários.

A disponibilidade dos métodos de autenticação no aplicativo irá determinar sua melhor estratégia. Sempre verifique se que você tiver uma documentação detalhada para os proprietários do aplicativo sobre como obter o estado de configuração do logon original caso sua implantação é executado em problemas.

- **Se seu aplicativo dá suporte a vários provedores de identidade**, por exemplo, LDAP e do AD FS e Ping, não exclua a configuração de SSO existente durante a distribuição. Em vez disso, desabilitá-lo durante a migração caso você precise alterná-lo novamente mais tarde. 

- **Se seu aplicativo não dá suporte a vários IDPs** , mas permite que os usuários fazer logon usando autenticação baseada em formulários (nome de usuário/senha), certifique-se de que os usuários poderão retornar a essa abordagem em caso de falha a nova distribuição de configuração de SSO.

### <a name="access-management"></a>gerenciamento de acesso

É recomendável escolher uma abordagem em escala, ao gerenciar o acesso aos recursos. Abordagens comuns incluem a utilização de grupos locais ao sincronizar por meio do Azure AD Connect, [criando grupos dinâmicos no AD do Azure com base em atributos de usuário](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal), ou criando [grupos de autoatendimento](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) no Azure AD gerenciado pelo proprietário do recurso.

### <a name="monitor-security"></a>Segurança do monitor

Recomendamos configurar uma cadência regular em que você examinar os diferentes aspectos de segurança de aplicativo de SaaS e realize ações corretivas necessárias.

### <a name="troubleshooting"></a>solução de problemas

Os links a seguir apresentam cenários de solução de problemas. Você talvez queira criar um guia específico para sua equipe de suporte que incorpora estes cenários e as etapas para corrigi-los.

#### <a name="consent-issues"></a>Problemas de consentimento

- [Erro de consentimento inesperada](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-prompt)

- [Erro de consentimento do usuário](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-error)

#### <a name="sign-in-issues"></a>Problemas de credenciais

- [Problemas ao entrar meio de um portal personalizado](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-deeplink)

- [Problemas para entrar no painel de acesso](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-access-panel)

- [Erro na página de entrada do aplicativo](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-application-error)

- [Problema ao entrar em um aplicativo da Microsoft](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-first-party-microsoft)

#### <a name="sso-issues-for-applications-listed-in-the-azure-application-gallery"></a>Problemas de SSO para aplicativos listados na Galeria de aplicativos do Azure

- [Problema com o SSO de senha para os aplicativos listados na Galeria de aplicativos do Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery) 

- [Problema com o SSO federado para aplicativos listados na Galeria de aplicativos do Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-gallery)   

#### <a name="sso-issues-for-applications-not-listed-in-the-azure-application-gallery"></a>Problemas de SSO para aplicativos não listados na Galeria de aplicativos do Azure

- [Problema com o SSO de senha para aplicativos não listados na Galeria de aplicativos do Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-non-gallery) 

- [Problema com o SSO federado para aplicativos não listados na Galeria de aplicativos do Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-non-gallery)

## <a name="next-steps"></a>Próximas etapas

[Depurar o SSO baseado em SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

[Mapeamento de declaração para aplicativos por meio do PowerShell](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping)

[Personalizando declarações emitidas no token SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

[Protocolo SAML de logon único](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

[Protocolo SAML de Logout Único](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-out-protocol-reference)

[B2B do AD do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) (para usuários externos, como parceiros e fornecedores)

[Acesso condicional do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)

[Proteção de identidade do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

[Acesso SSO](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

[Tutorial do aplicativo de SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

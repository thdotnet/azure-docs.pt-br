---
title: Nuvem do Azure AD regido gerenciamento para cargas de trabalho local - Azure
description: Este tópico descreve o gerenciamento de nuvem controlado para cargas de trabalho local.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b7e54f6acfe1cbbe6e46fe92d132ebdaa91ff33
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742351"
---
# <a name="how-azure-ad-delivers-cloud-governed-management-for-on-premises-workloads"></a>Como o AD do Azure fornece nuvem regido gerenciamento para cargas de trabalho local

Azure Active Directory (Azure AD) é uma identidade abrangente como uma solução de serviço (IDaaS) utilizada por milhões de organizações que abrangem todos os aspectos de identidade, gerenciamento de acesso e segurança. O Azure AD mantém as identidades dos usuários de mais de um bilhão e ajuda os usuários a entrar e acessar com segurança:

* Recursos externos, como o Microsoft Office 365, o portal do Azure e milhares de outros aplicativos de Software-como um serviço (SaaS).
* Recursos internos, como aplicativos na rede corporativa e a intranet, juntamente com os aplicativos de nuvem desenvolvido pela organização de uma organização.

As organizações podem usar o Azure AD se elas são 'nuvem pura' ou como 'híbrido' implantação se eles tiverem localmente as cargas de trabalho. Uma implantação híbrida do Azure AD pode ser parte de uma estratégia para uma organização para migrar seus ativos de TI para a nuvem, ou para continuar integrar a infraestrutura local existente juntamente com os novos serviços de nuvem.

Historicamente, as organizações de 'híbrido' visto do Azure AD como uma extensão existente de infraestrutura no local. Nessas implantações, a administração de governança de identidade local, Windows Server Active Directory ou outros sistemas de diretório interno, são os pontos de controle e os usuários e grupos são sincronizados nesses sistemas para um diretório na nuvem como o Azure AD. Depois que essas identidades estão na nuvem, eles podem ser disponibilizados para o Office 365, Azure e outros aplicativos.

![Ciclo de vida de identidade](media/cloud-governed-management-for-on-premises//image1.png)

Conforme as organizações mover mais de sua infra-estrutura de TI, juntamente com seus aplicativos para a nuvem, muitos procuram a segurança aprimorada e gerenciamento simplificado de recursos de gerenciamento de identidade como um serviço. Os recursos de IDaaS entregue na nuvem no Azure AD aceleram a transição para o gerenciamento controlado na nuvem, fornecendo as soluções e os recursos que permitem que as empresas a adotar rapidamente e ser movido mais do seu gerenciamento de identidade tradicional no local sistemas do Azure AD, enquanto continua a dar suporte a aplicativos existentes, bem como novos.

Este documento descreve a estratégia da Microsoft para híbrido IDaaS e descreve como as organizações podem usar o Azure AD para seus aplicativos existentes.

## <a name="the-azure-ad-approach-to-cloud-governed-identity-management"></a>A abordagem do Azure AD para a nuvem de gerenciamento de identidades controlados

Quando sua transição para a nuvem, as organizações que precisam de garantias de que eles têm controles sobre seu ambiente completo - mais segurança e mais visibilidade em atividades, com suporte, automação e insights proativos. "**Nuvem regido gerenciamento**" descreve como, as organizações gerenciem e controlem seus usuários, aplicativos, grupos e dispositivos da nuvem.

Neste mundo moderno, as organizações precisam ser capazes de gerenciar com eficiência em grande escala, devido à proliferação de aplicativos SaaS e a função de cada vez maior de colaboração e identidades externas. O novo panorama de risco da nuvem significa que uma organização deve ser mais responsiva – um ator mal-intencionado que compromete a um usuário de nuvem pode afetar os aplicativos de nuvem e locais.

Em particular, híbrido, as organizações precisam poder delegar e automatizar tarefas, que historicamente IT fez manualmente. Para automatizar tarefas, eles precisam de APIs e os processos que orquestrar o ciclo de vida dos diferentes relacionados à identidade recursos (usuários, grupos, aplicativos, dispositivos), portanto, eles podem delegar o gerenciamento diário desses recursos para mais pessoas fora do a equipe de TI do núcleo. AD do Azure lida com esses requisitos por meio do gerenciamento de conta de usuário e autenticação nativa para usuários sem a necessidade de infraestrutura de identidade local. Criação de infraestrutura local não pode se beneficiar as organizações que têm novas comunidades de usuários, como parceiros de negócios, que não se originam em seu diretório local, mas cujo gerenciamento de acesso é fundamental para alcançar os resultados de negócios.

Além disso, o gerenciamento não está completo sem a governança--- e governança neste novo mundo é parte integrante do sistema de identidade em vez de um complemento. Governança de identidade fornece às organizações a capacidade de gerenciar a identidade e acesso do ciclo de vida entre os funcionários, parceiros de negócios e fornecedores e serviços e aplicativos.

Incorporar o controle de identidade torna mais fácil permitir que a organização para fazer a transição para a nuvem de gerenciamento controlado, permite que a TI da escala, aborda novos desafios com convidados e fornece insights e a automação que o que os clientes tinham com mais profunda infraestrutura local. Governança neste novo mundo significa a capacidade para uma organização ter transparência, visibilidade e controles apropriados de acesso a recursos dentro da organização. Com o Azure AD, operações de segurança e auditoria equipes têm acesso de visibilidade sobre quem tem--- e quem deve ter - para quais recursos na organização (em quais dispositivos), o que esses usuários estão fazendo com que o acesso e se a organização tem e usa apropriada controles para remover ou restringir o acesso de acordo com políticas normativas ou da empresa.

O novo modelo de gerenciamento se beneficia as organizações com SaaS e aplicativos do linha de negócios (LOB), já que são mais facilmente capazes de gerenciar e proteger o acesso a esses aplicativos. Integrando aplicativos com o Azure AD, as organizações poderão usar e gerenciar o acesso em nuvem e local se originou identidades consistentemente. Gerenciamento de ciclo de vida do aplicativo se torna mais automatizado e o Azure AD fornece insights avançados sobre o uso do aplicativo que não era facilmente realizável no gerenciamento de identidade local. Por meio do AD do Azure, grupos do Office 365 e os recursos de autoatendimento de equipes, as organizações podem facilmente criar grupos para colaboração e gerenciamento de acesso e adicionar ou remover os usuários na nuvem para habilitar a colaboração e gerenciamento de requisitos de acesso.

Selecionando os recursos da direita do Azure AD para a nuvem, gerenciamento controlado depende dos aplicativos a serem usados e como esses aplicativos serão integrados com o Azure AD. As seções a seguir descrevem as abordagens a serem adotadas para aplicativos integrados ao AD e aplicativos que usam protocolos de Federação (por exemplo, SAML, OAuth ou OpenID Connect).

## <a name="cloud-governed-management-for-ad-integrated-applications"></a>Gerenciamento de nuvem controlado para aplicativos integrados ao AD

AD do Azure melhora o gerenciamento de aplicativos integrados ao Active Directory por meio do acesso remoto seguro e o acesso condicional a esses aplicativos no local uma organização. Além disso, o Azure AD também fornece gerenciamento de ciclo de vida de conta e gerenciamento de credenciais para as contas do usuário existentes do AD, incluindo:

* **Acesso condicional para aplicativos locais e acesso remoto seguro**

Para muitas organizações, a primeira etapa no gerenciamento de acesso da nuvem para web integradas ao AD do local e aplicativos da área de trabalho remotos é implantar o [proxy de aplicativo](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) na frente desses aplicativos para fornecer seguro acesso remoto.

Após um logon único para o Azure AD, os usuários podem acessar aplicativos locais e de nuvem por meio de uma URL externa ou um portal interno do aplicativo. Por exemplo, o Proxy de aplicativo fornece acesso remoto e o logon único para área de trabalho remota, o SharePoint, bem como aplicativos, como Tableau e Qlik e aplicativos de negócios (LOB). Além disso, as políticas de acesso condicional podem incluir exibindo o [termos de uso](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) e [garantir que o usuário concordou-los](https://docs.microsoft.com/azure/active-directory/conditional-access/require-tou) antes de poder acessar um aplicativo.

![Arquitetura de Proxy de aplicativo](media/cloud-governed-management-for-on-premises/image2.png)

* **Gerenciamento automático de ciclo de vida de contas do Active Directory**

Governança de identidade ajuda as organizações a atingir um equilíbrio entre *produtividade* ---quão rapidamente uma pessoa pode ter acesso aos recursos que precisa, como quando ele ingressam na organização? --- e *segurança* ---como o acesso deve alterar ao longo do tempo, como quando o status de emprego dessa pessoa alterar? O gerenciamento do ciclo de vida de identidades é a base para governança de identidade, e governança efetiva em escala requer modernizar a infraestrutura de gerenciamento do ciclo de vida de identidades para aplicativos.

Para muitas organizações, ciclo de vida de identidade para funcionários é vinculado à representação do usuário em um sistema de gerenciamento de capital humano (HCM). Para as organizações que usam o Workday como seus sistemas HCM, AD do Azure pode garantir que contas de usuário no AD fiquem [automaticamente provisionados e desprovisionada para trabalhadores no Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial). Fazer então leva a produtividade do usuário aprimorada por meio da automação de birthright contas e gerencia os riscos, garantindo o aplicativo de acesso é atualizado automaticamente quando um usuário altera as funções ou sai da organização. O provisionamento de usuário controlada no Workday [plano de implantação](https://aka.ms/WorkdayDeploymentPlan) é um guia passo a passo que orienta as organizações por meio da implementação de práticas recomendada de dia de trabalho para a solução de provisionamento de usuário do Active Directory em um processo de cinco etapas.

O Azure AD Premium também inclui o Microsoft Identity Manager, que pode importar registros de outros sistemas HCM locais, incluindo SAP, Oracle eBusiness e Oracle PeopleSoft.

Colaboração Business-to-business cada vez mais exige a concessão do acesso às pessoas fora da sua organização. [B2B do AD do Azure](https://docs.microsoft.com/azure/active-directory/b2b/) colaboração permite que as organizações compartilhem com segurança seus aplicativos e serviços com os usuários convidados e parceiros externos enquanto mantém o controle sobre seus próprios dados corporativos.

Azure AD pode [automaticamente criar contas no AD para usuários convidados](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises) conforme necessário, permitindo que os parceiros de negócios convidados acessar aplicativos no local integrado ao AD sem a necessidade de outra senha. As organizações podem configurar [políticas de autenticação multifator (MFA) para o usuário convidado](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)s para verificações de MFA são feitas durante a autenticação de proxy de aplicativo. Além disso, qualquer [revisões de acesso](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews) que são feitas na nuvem B2B, os usuários se aplicam a usuários locais. Por exemplo, se o usuário de nuvem for excluído por meio de políticas de gerenciamento do ciclo de vida, o usuário local também é excluído.

**Gerenciamento de contas do Active Directory de credenciais** AD do Azure do autoatendimento de redefinição de senha permite aos usuários que tenham esquecido suas senhas para ser reautenticados e redefinir suas senhas, com as senhas alteradas [gravados local do Active Directory](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback). O processo de redefinição de senha também pode usar as políticas de senha do Active Directory no local: Quando um usuário redefine sua senha, ele é verificado para garantir que ele atende a diretiva do Active Directory local antes de confirmá-la nesse diretório. Redefinir a senha de autoatendimento [plano de implantação](https://aka.ms/deploymentplans/sspr) descreve as práticas recomendadas para distribuir a senha de autoatendimento de redefinição aos usuários por meio da web e experiências integradas ao Windows.

![Arquitetura do Azure AD SSPR](media/cloud-governed-management-for-on-premises/image3.png)

Por fim, para organizações que permitem que os usuários alterem suas senhas no AD, AD pode ser configurado para usar a mesma política de senha, pois a organização está usando no Azure AD por meio de [recurso de proteção de senha do Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises), atualmente em visualização pública.

Quando uma organização está pronta para mover um aplicativo integrado ao AD para a nuvem, movendo o sistema operacional que hospeda o aplicativo do Azure, [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) fornece serviços de domínio compatível com o AD (por exemplo, o ingresso no domínio, diretiva de grupo, LDAP e Kerberos/NTLM autenticação). Serviços de domínio do AD do Azure integra-se com o locatário da organização existente do Azure AD, tornando possível para os usuários façam logon usando suas credenciais corporativas. Além disso, as contas de usuário e grupos existentes podem ser usadas para proteger o acesso aos recursos, o garantindo um mais suave 'lift-and-shift' de recursos de local para serviços de infraestrutura do Azure.

![Azure AD Domain Services](media/cloud-governed-management-for-on-premises/image4.png)

## <a name="cloud-governed-management-for-on-premises-federation-based-applications"></a>Gerenciamento de nuvem controlado para aplicativos locais federação

Para uma organização que já usa um provedor de identidade local, movendo aplicativos para o Azure AD permite acesso mais seguro e uma experiência administrativa mais fácil para o gerenciamento de Federação. O Azure AD habilita configurando controles de acesso por aplicativo granulares, incluindo a autenticação multifator, por meio do acesso condicional do Azure AD. Azure AD dá suporte a mais recursos, incluindo certificados de assinatura de token específico do aplicativo e datas de validade do certificado configuráveis. Esses recursos, ferramentas e orientações permitem às organizações desativar seus provedores de identidade local. Do Microsoft IT, para obter um exemplo, foi movida 17,987 aplicativos de interna Active Directory Federation Services da Microsoft (AD FS) para o Azure AD.

![Evolução do AD do Azure](media/cloud-governed-management-for-on-premises/image5.png)

Para começar a migrar aplicativos federados do Azure AD como provedor de identidade, consulte https://aka.ms/migrateapps que inclui links para:

* O white paper [migrando seus aplicativos ao Azure Active Directory](https://aka.ms/migrateapps/whitepaper), que apresenta os benefícios da migração e descreve como planejar a migração em quatro fases descritas claramente: detecção, classificação, migração, e gerenciamento contínuo. Você será guiado pelo processo de pensar sobre o processo e dividir seu projeto em pedaços mais fácil de consumir. Ao longo do documento, haverá links para recursos importantes que irão ajudá-lo durante o processo.

* O guia de solução [migrando a autenticação de aplicativo dos serviços de Federação do Active Directory ao Azure Active Directory](https://aka.ms/migrateapps/adfssolutionguide) explora mais detalhadamente os mesmos quatro fases de planejamento e execução de um projeto de migração do aplicativo . Neste guia, você aprenderá como aplicar essas fases para o objetivo específico de mover um aplicativo dos serviços de Federação do Active Directory (AD FS) para o Azure AD.

* O [Script de preparação de migração de serviços do diretório federação Active Directory](https://aka.ms/migrateapps/adfstools) podem ser executados em servidores de serviços de Federação do Active Directory (AD FS) locais existentes para determinar a prontidão de aplicativos para a migração para o Azure AD.

## <a name="ongoing-access-management-across-cloud-and-on-premises-applications"></a>Gerenciamento de acesso contínuo em aplicativos de nuvem e locais

As organizações precisam de um processo para gerenciar o acesso que seja escalonável. Os usuários continuam se acumular direitos de acesso e acabar com além do que foi inicialmente provisionado para eles. Além disso, as organizações empresariais precisam ser capaz de dimensionar com eficiência para desenvolver e aplicar a política de acesso e controles em uma base contínua.

Em geral, a IT delega as decisões de aprovação de acesso aos tomadores de decisão empresariais. Além disso, a IT pode envolver os próprios usuários. Por exemplo, os usuários que acessam os dados confidenciais do cliente no aplicativo de marketing da empresa na Europa precisam conhecer as políticas da empresa. Usuários convidados também podem não estar cientes dos requisitos de manipulação de dados em uma organização para a qual você foi convidados.

As organizações podem automatizar o processo de ciclo de vida de acesso por meio de tecnologias, como [grupos dinâmicos](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership), combinado com o provisionamento de usuário para [aplicativos SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list), ou [aplicativos integrado usando o sistema de gerenciamento de identidade entre domínios (SCIM](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)) padrão. As organizações também podem controlar quais [os usuários convidados tenham acesso a aplicativos locais](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises). Esses direitos de acesso podem então ser revisados regularmente usando [revisões de acesso do Azure AD](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) recorrentes.

## <a name="future-directions"></a>Perspectivas para o futuro

Em ambientes híbridos, estratégia da Microsoft é permitir implantações em que o **nuvem é o plano de controle de identidade**e em diretórios locais e outros sistemas de identidade, como o Active Directory e outros locais aplicativos, são o destino para o provisionamento de usuários com acesso. Essa estratégia continuará garantir que os direitos, identidades e acesso nesses aplicativos e cargas de trabalho que contam com eles. Nesse estado final, as organizações poderão aumentar a produtividade do usuário final inteiramente na nuvem.

![Arquitetura do Azure AD](media/cloud-governed-management-for-on-premises/image6.png)

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre como começar essa jornada, consulte os planos de implantação do AD do Azure, localizados em <https://aka.ms/deploymentplans> . Eles fornecem orientação de ponta a ponta sobre como implantar os recursos do Azure Active Directory (Azure AD). Cada plano explica o valor de negócios, planejamento considerações, design e procedimentos operacionais necessários para distribuir com sucesso a recursos comuns do AD do Azure. A Microsoft atualiza continuamente os planos de implantação com as melhores práticas aprendidas com implantações de clientes e outros comentários quando adicionamos novos recursos de gerenciamento da nuvem com o Azure AD.

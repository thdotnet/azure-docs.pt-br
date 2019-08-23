---
title: Planos de implantação - Azure Active Directory | Microsoft Docs
description: Fornece orientação de ponta a ponta sobre como implantar recursos do Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6a335bcbb45b1aac48a3aaea282ffeffe1696f4c
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69900077"
---
# <a name="azure-active-directory-deployment-plans"></a>Planos de implantação do Azure Active Directory
Procurando orientação de ponta a ponta sobre a implantação de recursos do Azure Active Directory (Azure AD)? Os planos de implantação do Azure AD orientam você pelo valor de negócios, considerações de planejamento e procedimentos operacionais necessários para implantar com êxito recursos comuns do Azure AD.

Em qualquer uma das páginas do plano, use a funcionalidade imprimir para PDF do navegador para criar uma versão offline atualizada da documentação.
## <a name="include-the-right-stakeholders"></a>Incluir os participantes certos

Ao iniciar o planejamento da implantação para um novo recurso, é importante incluir os principais acionistas em sua organização. Recomendamos que você identifique e documente a pessoa ou as pessoas que atendem a cada uma das funções a seguir e trabalhe com elas para determinar seu envolvimento no projeto.  

As funções podem incluir o seguinte 

|Role |Descrição |
|-|-|
|Usuário final|Um grupo representativo de usuários para os quais a funcionalidade será implementada. Geralmente, visualiza as alterações em um programa piloto.
|Gerente de suporte de ti|O departamento de ti oferece suporte à organização que pode fornecer informações sobre a compatibilidade dessa mudança de uma perspectiva de assistência técnica.  
|Arquiteto de identidade ou administrador global do Azure|Representante da equipe de gerenciamento de identidades responsável pela definição de como essa alteração é alinhada com a infra-estrutura principal de gerenciamento de identidades em sua organização.|
|Proprietário da empresa do aplicativo |O proprietário geral da empresa dos aplicativos afetados, que podem incluir o gerenciamento de acesso.  Também pode fornecer informações sobre a experiência do usuário e a utilidade dessa alteração da perspectiva de um usuário final.
|Proprietário da segurança|Um representante da equipe de segurança que pode se desconectar que o plano atenderá aos requisitos de segurança de sua organização.|
|Gerenciador de conformidade|A pessoa na sua organização responsável por garantir a conformidade com requisitos corporativos, industriais ou governamentais.|

**Os níveis de envolvimento podem incluir:**

- **R**esponsible para implementar o plano e o resultado do projeto 

- **Um**pproval do plano e do resultado do projeto 

- **C**para o plano e o resultado do projeto 

- **Eu**nformed o plano e o resultado do projeto


## <a name="deploy-authentication"></a>Implantar autenticação

| Funcionalidade | Descrição|
| -| -|
| [Autenticação Multifator](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-getstarted)| A Autenticação Multifator do Azure é uma solução de verificação em duas etapas da Microsoft. Usando métodos de autenticação aprovados pelo administrador, o Azure MFA ajuda a proteger o acesso aos seus dados e aplicativos, ao mesmo tempo em que atende à demanda por um processo de entrada simples. |
| [Acesso condicional](https://aka.ms/deploymentplans/ca)| Com o acesso condicional, você pode implementar decisões de controle de acesso automatizadas para quem pode acessar seus aplicativos de nuvem, com base em condições. |
| [Redefinição de senha de autoatendimento](https://aka.ms/deploymentplans/sspr)| A redefinição de senha de autoatendimento ajuda os usuários a redefinir suas senhas sem a intervenção do administrador, quando e onde eles precisam. |

## <a name="deploy-application-management"></a>Implantar o gerenciamento de aplicativos

| Funcionalidade | Descrição|
| -| - |
| [Logon Único](https://aka.ms/deploymentplans/sso)| O logon único ajuda os usuários a acessar os aplicativos e recursos de que precisam para fazer negócios ao entrar apenas uma vez. Depois que eles tiverem entrado, eles poderão ir de Microsoft Office para o SalesForce para o box para aplicativos internos sem precisar inserir as credenciais uma segunda vez. |
| [Painel de acesso](https://aka.ms/deploymentplans/accesspanel)| Oferece a seus usuários um hub simples para descobrir e acessar todos os seus aplicativos. Habilite-os a serem mais produtivos com recursos de autoatendimento, como solicitar acesso a aplicativos e grupos ou gerenciar o acesso a recursos em nome de outros. |


## <a name="deploy-hybrid-scenarios"></a>Implantar cenários híbridos

| Funcionalidade | Descrição|
| -| -|
| [ADFS para sincronização de hash de senha](https://aka.ms/deploymentplans/adfs2phs)| Com a sincronização de hash de senha, os hashes de senhas de usuário são sincronizados do Active Directory local para o Azure AD, permitindo que o Azure AD autentique usuários sem interação com o Active Directory local |
| [ADFS para autenticação de passagem](https://aka.ms/deploymentplans/adfs2pta)| A autenticação de passagem do Azure AD ajuda os usuários a entrar em aplicativos locais e baseados em nuvem usando as mesmas senhas. Esse recurso fornece aos usuários uma experiência melhor-uma senha a menos para lembrar e reduz os custos de assistência técnica de ti, pois os usuários têm menos probabilidade de se esquecer de como entrar. Quando as pessoas entram usando o Azure AD, esse recurso valida as senhas dos usuários diretamente no Active Directory local. |
| [Proxy de Aplicativo do Azure AD](https://aka.ms/deploymentplans/appproxy)| Os funcionários de hoje querem ser produtivos em qualquer lugar, a qualquer momento e com qualquer dispositivo. Eles precisam acessar aplicativos SaaS na nuvem e nos aplicativos corporativos locais. O proxy de aplicativo do Azure AD permite esse acesso robusto sem dispendiosos e complexas redes virtuais privadas (VPNs) ou zonas desmilitarizadas (DMZs). |
| [SSO contínuo](https://aka.ms/SeamlessSSODPDownload)| O SSO Contínuo do Azure AD (Logon Único Contínuo do Azure Active Directory) conecta usuários automaticamente quando estiverem nos respectivos dispositivos corporativos conectados à sua rede corporativa. Com esse recurso, os usuários não precisarão digitar suas senhas para entrar no Azure AD e, em geral, não precisarão inserir seus nomes de usuário. Esse recurso fornece aos usuários autorizados acesso fácil aos seus aplicativos baseados em nuvem sem a necessidade de componentes locais adicionais. |

## <a name="deploy-user-provisioning"></a>Implantar provisionamento de usuário

| Funcionalidade | Descrição|
| -| -|
| [Provisionamento do usuário](https://aka.ms/UserProvisioningDPDownload)| O Azure AD ajuda a automatizar a criação, a manutenção e a remoção de identidades de usuário em aplicativos em nuvem (SaaS) como Dropbox, Salesforce, ServiceNow e outros. |
| [Provisionamento de entrada de usuário controlada pelo Workday](https://aka.ms/WorkdayDeploymentPlan)| O provisionamento de entrada de usuário controlada pelo Workday cria uma base para a governança de identidade contínua e melhora a qualidade dos processos de negócios que dependem de dados de identidade autoritativos. Usando esse recurso, você pode gerenciar diretamente o ciclo de vida de identidade de funcionários e trabalhos contingentes Configurando regras que mapeiam processos de movimentação de portador de União (como novas contratações, término e transferência) para ações de provisionamento de ti (como criar, habilitar, Desativar |

## <a name="deploy-governance-and-reporting"></a>Implantar governança e relatórios

| Funcionalidade | Descrição|
| -| -|
| [Privileged Identity Management](https://aka.ms/deploymentplans/pim)| O Azure AD Privileged Identity Management (PIM) ajuda você a gerenciar funções administrativas com privilégios entre o Azure AD, recursos do Azure e outros Serviços Online da Microsoft. O PIM fornece soluções como acesso just-in-time, solicitação de fluxos de trabalho de aprovação e revisões de acesso totalmente integradas para que você possa identificar, descobrir e impedir atividades maliciosas de funções com privilégios em tempo real. |
| [Relatórios e monitoramento](https://aka.ms/deploymentplans/reporting)| O design de sua solução de monitoramento e relatório do Azure AD depende dos seus requisitos legais, de segurança e operacionais, bem como do ambiente e dos processos existentes. Este artigo apresenta as várias opções de design e orienta você para a estratégia de implantação certa. |

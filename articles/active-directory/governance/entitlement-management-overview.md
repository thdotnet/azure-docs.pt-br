---
title: O que é gerenciamento de direitos do AD do Azure? (Visualização) – o Azure Active Directory
description: Obtenha uma visão geral do gerenciamento de direitos do Active Directory do Azure e como você pode usá-lo para gerenciar o acesso a grupos, aplicativos e sites do SharePoint Online para usuários internos e externos.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/05/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbb4001e9496d31d9c2879721f8cf8e26b74ddf3
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204548"
---
# <a name="what-is-azure-ad-entitlement-management-preview"></a>O que é gerenciamento de direitos do AD do Azure? (Visualização)

> [!IMPORTANT]
> No momento, o gerenciamento de direitos do Azure AD (Azure Active Directory) está em versão prévia pública.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Os funcionários em organizações precisam de acesso a vários grupos de aplicativos e sites para realizar seu trabalho. Gerenciar esse acesso é um desafio. Na maioria dos casos, não há nenhuma lista organizada de todos os recursos que um usuário precisa para um projeto. O gerente de projeto tem uma boa compreensão dos recursos necessários, as pessoas envolvidas e por quanto tempo que durará o projeto. No entanto, o gerente de projeto geralmente não tem permissões para aprovar ou conceder acesso a outras pessoas. Esse cenário se tornar mais complicado quando você tenta trabalhar com pessoas externas ou de empresas.

Gerenciamento de direitos do Active Directory (Azure AD) do Azure pode ajudar você a gerenciar o acesso a grupos, aplicativos e sites do SharePoint Online para usuários internos e também os usuários fora da sua organização.

## <a name="why-use-entitlement-management"></a>Por que usar o gerenciamento de direitos

Organizações empresariais frequentemente enfrentam desafios ao gerenciar o acesso aos recursos, como:

- Os usuários talvez não saibam que eles devem ter de acesso
- Os usuários podem ter dificuldade para localizar os indivíduos certos ou os recursos certos
- Depois que os usuários localizarem e receber acesso a um recurso, eles podem manter para acessar mais do que é necessário para fins comerciais

Esses problemas são agravados para usuários que precisam de acesso de outro diretório, como usuários externos que sejam de organizações de cadeia de suprimentos ou outros parceiros de negócios. Por exemplo:

- As organizações podem não conhecer todas as pessoas específicas em outros diretórios para poder para convidá-los
- Mesmo que as organizações capazes de convidar esses usuários, as organizações podem não lembrar gerencie todo o acesso do usuário consistente

Gerenciamento de direitos do AD do Azure pode ajudar a enfrentar esses desafios.

## <a name="what-can-i-do-with-entitlement-management"></a>O que pode fazer com o gerenciamento de direitos?

Aqui estão alguns dos recursos do gerenciamento de direitos:

- Criar pacotes de recursos relacionados que os usuários podem solicitar
- Definir regras para como a quando o acesso expira e recursos de solicitação
- Controlar o ciclo de vida de acesso para usuários internos e externos
- Delegar o gerenciamento de recursos
- Designar os aprovadores para aprovar solicitações
- Criar relatórios para acompanhar o histórico

Para obter uma visão geral da governança de identidade e gerenciamento de direitos, assista ao seguinte vídeo da conferência Ignite 2018:

>[!VIDEO https://www.youtube.com/embed/aY7A0Br8u5M]

## <a name="what-resources-can-i-manage"></a>Quais recursos pode gerenciar?

Aqui estão os tipos de recursos que você pode gerenciar o acesso com o gerenciamento de direitos:

- Grupos de segurança do Azure AD
- Grupos do Office 365
- Enterprise aplicativos do Azure AD, incluindo o aplicativo SaaS e aplicativos integrados ao personalizados dão suporte à Federação ou provisionamento
- Sites e conjuntos de sites do SharePoint Online

Você também pode controlar o acesso a outros recursos que se baseiam em grupos de segurança do Azure AD ou grupos do Office 365.  Por exemplo:

- Você pode atribuir licenças de usuários para o Microsoft Office 365, usando um grupo de segurança do Azure AD em um pacote de acesso e configurando [licenciamento baseado em grupo](../users-groups-roles/licensing-groups-assign.md) para esse grupo
- Você pode dar aos usuários acesso para gerenciar recursos do Azure usando um grupo de segurança do Azure AD em um pacote de acesso e criando uma [atribuição de função do Azure](../../role-based-access-control/role-assignments-portal.md) para esse grupo

## <a name="what-are-access-packages-and-policies"></a>Quais são os pacotes de acesso e políticas?

Gerenciamento de direitos de apresenta o conceito de um *pacote acesso*. Um pacote de acesso é um pacote de todos os recursos que um usuário precisa para trabalhar em um projeto ou executar seu trabalho. Os recursos incluem acesso a grupos, aplicativos ou sites. Pacotes de acesso são usados para controlar o acesso para seus funcionários internos e também os usuários fora da sua organização. Pacotes de acesso são definidos em contêineres chamados *catálogos*.

Pacotes de acesso também incluem um ou mais *políticas*. Uma política define as regras ou as grades de proteção para acessar um pacote de acesso. Habilitar uma política impõe que somente os usuários certos recebem acesso aos recursos certos e para a quantidade certa de tempo.

![Pacote de acesso e políticas](./media/entitlement-management-overview/elm-overview-access-package.png)

Com um pacote de acesso e as respectivas políticas, o Gerenciador de pacotes de acesso define:

- Recursos
- Funções de usuários necessários para os recursos
- Os usuários internos e usuários externos que estão qualificados para solicitar acesso
- Processo de aprovação e os usuários que podem aprovar ou negar acesso
- Duração do acesso do usuário

O diagrama a seguir mostra um exemplo dos diferentes elementos no gerenciamento de direitos. Ele mostra dois pacotes de exemplo de acesso.

- **Pacote de acesso 1** inclui um único grupo como um recurso. O acesso é definido com uma política que permite que um conjunto de usuários no diretório para solicitar acesso.
- **Pacote de acesso 2** inclui um grupo, um aplicativo e um site do SharePoint Online como recursos. O acesso é definido com duas diretivas diferentes. A primeira diretiva permite que um conjunto de usuários no diretório para solicitar acesso. A segunda política permite que os usuários em um diretório externo solicitar acesso.

![Visão geral do gerenciamento de direitos](./media/entitlement-management-overview/elm-overview.png)

## <a name="external-users"></a>Usuários externos

Ao usar o [do Azure AD business-to-business (B2B)](../b2b/what-is-b2b.md) convidar experiência, você já deve saber os endereços de email dos usuários convidados externo que deseja trazer para o seu diretório de recursos e trabalhar com. Isso funciona muito bem quando você estiver trabalhando em um projeto de curto prazo ou menor e você já conhece todos os participantes, mas isso é mais difícil de gerenciar se você tiver muitos usuários que você deseja trabalhar ou se os participantes alterar ao longo do tempo.  Por exemplo, você pode trabalhar com outra organização e ter um ponto de contato com essa organização, mas ao longo do tempo adicionais que os usuários da organização também precisará acessar.

Com o gerenciamento de direitos, você pode definir uma política que permite que os usuários de organizações que você especificar, e que também estão usando o Azure AD, para poder solicitar um pacote de acesso. Você pode especificar se a aprovação é necessária e uma data de expiração para o acesso. Se a aprovação for necessária, você também pode designar como aprovador um ou mais usuários da organização externa que você anteriormente convidado - uma vez que eles provavelmente sabe quais usuários externos de sua organização precisam de acesso. Depois de configurar o pacote de acesso, você pode enviar um link para o pacote de acesso para sua pessoa de contato na organização externa. Esse contato pode compartilhar com outros usuários na organização externa, e eles podem usar este link para solicitar o acesso de pacote.  Os usuários da organização que já foram convidados para seu diretório também podem usar esse link.

Quando uma solicitação for aprovada, o gerenciamento de direitos de provisionará o usuário com o acesso necessário, o que pode incluir convidar o usuário se eles não ainda estiver em seu diretório. Azure AD criará automaticamente uma conta de B2B para eles.  Observe que um administrador pode ter limitado anteriormente que as organizações são permitidas para a colaboração, definindo uma [B2B permitir ou negar lista](../b2b/allow-deny-list.md) para permitir ou bloquear convites para outras organizações.  Se o usuário não é permitido pela lista de permissões ou bloqueios, em seguida, eles não serão convidados.

Como você não deseja que o acesso do usuário externo durar para sempre, você especificar uma data de expiração na política, como 180 dias. Após 180 dias, se o acesso não for renovado, o gerenciamento de direitos de removerá todo o acesso associado a esse pacote de acesso.  Se o usuário que foi convidado por meio do gerenciamento de direitos não tem outras acesso pacote atribuições, em seguida, quando eles perdem a sua última atribuição, sua conta B2B será impedida de entrar por 30 dias e subsequentemente removida.  Isso impede que a proliferação de contas desnecessárias.  

## <a name="terminology"></a>Terminologia

Para entender melhor o gerenciamento de direitos e sua documentação, você deve revisar os termos a seguir.

| Termo ou conceito | DESCRIÇÃO |
| --- | --- |
| gerenciamento de direitos | Um serviço que atribui, revoga e administra pacotes de acesso. |
| pacote de acesso | Uma coleção de permissões e políticas para recursos que os usuários podem solicitar. Um pacote de acesso sempre está contido em um catálogo. |
| solicitação de acesso | Uma solicitação para acessar um pacote de acesso. Normalmente, uma solicitação passa por um fluxo de trabalho. |
| policy | Um conjunto de regras que define o ciclo de vida de acesso, como como os usuários podem obter acesso, quem pode aprovar e por quanto tempo os usuários têm acesso. Exemplos de políticas incluem o acesso dos funcionários e o acesso externo. |
| catalog | Um contêiner de pacotes de acesso e recursos relacionados. |
| Catálogo geral | Um catálogo interno que está sempre disponível. Para adicionar recursos ao catálogo geral, exige determinadas permissões. |
| resource | Um ativo ou serviço (por exemplo, um grupo, aplicativo ou site) que um usuário pode ser concedido permissões para. |
| Tipo de recurso | O tipo de recurso, que inclui grupos, aplicativos e sites do SharePoint Online. |
| função de recurso | Uma coleção de permissões associadas a um recurso. |
| diretório de recursos | Um diretório que tem um ou mais recursos para compartilhar. |
| usuários atribuídos | Uma atribuição de um pacote de acesso para um usuário ou grupo. |
| enable | O processo de disponibilizar um pacote de acesso para que os usuários solicitem. |

## <a name="license-requirements"></a>Requisitos de licença

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Nuvens especializados, como o Azure governamental, Azure Alemanha e Azure China 21Vianet, não estão atualmente disponíveis para uso nesta visualização.

## <a name="next-steps"></a>Próximas etapas

- [Tutorial: Criar seu primeiro pacote de acesso](entitlement-management-access-package-first.md)
- [Cenários comuns](entitlement-management-scenarios.md)

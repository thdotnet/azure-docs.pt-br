---
title: Mapeamento de controle de exemplo - blueprint UK OFICIAL-
description: Mapeamento de controle de exemplo OFICIAL do Reino Unido especificações técnicas.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/13/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 2eb6e62bc891a147a89107f5e3de7c2b605bbd09
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276971"
---
# <a name="control-mapping-of-the-uk-official-blueprint-sample"></a>Mapeamento de controle de exemplo OFICIAL do Reino Unido blueprint

O artigo a seguir fornece detalhes sobre como o exemplo de especificações técnicas do Reino Unido OFICIAL é mapeada para os controles OFICIAL do Reino Unido.
Para obter mais informações sobre os controles, consulte [OFICIAL do Reino Unido](https://www.gov.uk/government/publications/government-security-classifications).

Os seguintes mapeamentos são para o **OFICIAL do Reino Unido** controles. Use a navegação no lado direito para ir diretamente para um mapeamento de controle específico. Muitos dos controles mapeados são implementados com uma iniciativa do [Azure Policy](../../../policy/overview.md). Para examinar a iniciativa completa, abra **Política** no portal do Azure e selecione a página **Definições**. Em seguida, localize e selecione o  **[visualização] controla e implantar extensões de VM específicas para dar suporte a requisitos de auditoria de auditoria do Reino Unido OFICIAL** iniciativa de política interna.

## <a name="1-data-in-transit-protection"></a>1 dados na proteção de trânsito

O blueprint ajuda você a garantir que a transferência de informações com os serviços do Azure seja segura por meio da atribuição de duas definições do [Azure Policy](../../../policy/overview.md) para auditar conexões não seguras com contas de armazenamento e o Cache Redis.

- Somente conexões seguras para seu Cache Redis devem ser habilitadas
- Transferência segura para contas de armazenamento deve ser habilitada

## <a name="23-data-at-rest-protection"></a>2.3 dados na proteção de rest

Esta especificação técnica o ajuda a impor sua política sobre o uso de controles cryptograph atribuindo 11 [política do Azure](../../../policy/overview.md) definições que impõem controles cryptograph específico e auditoria usam das configurações de criptografia fracas.
Entender em que local os recursos do Azure podem ter configurações de criptografia não ideais pode ajudá-lo a tomar ações corretivas para garantir que os recursos sejam configurados de acordo com a política de segurança de informações. Especificamente, as políticas atribuídas por esta especificação técnica exigem criptografia para contas de armazenamento do data lake; exigir criptografia transparente de dados em bancos de dados SQL; auditoria faltando criptografia em contas de armazenamento, bancos de dados SQL, discos de máquina virtual e variáveis de conta de automação; conexões inseguro de auditoria para contas de armazenamento e Cache Redis; auditar criptografia de senha de máquina de virtual fraco; e a auditoria de comunicação não criptografada do Service Fabric.

- Monitorar bancos de dados SQL não criptografados na Central de segurança do Azure
- Criptografia de disco deve ser aplicada em máquinas virtuais
- Variáveis de conta de automação devem ser criptografadas.
- Transferência segura para contas de armazenamento deve ser habilitada
- Transferência segura para contas de armazenamento deve ser habilitada
- Clusters do Service Fabric devem ter a propriedade ClusterProtectionLevel definida como EncryptAndSign
- Transparent Data Encryption em bancos de dados SQL deve ser habilitado
- Implantar a transparent data encryption do banco de dados SQL
- Exigir criptografia em contas do Data Lake Store
- Locais permitidos (foi codificado para "Sul do Reino Unido" e "Oeste do Reino Unido")
- Locais para grupos de recursos permitidos (foi codificado para "Sul do Reino Unido" e "Oeste do Reino Unido")

## <a name="52-vulnerability-management"></a>5.2 gerenciamento de vulnerabilidades

Esta especificação técnica o ajuda a gerenciar vulnerabilidades do sistema de informações por meio da atribuição cinco [política do Azure](../../../policy/overview.md) definições de monitoram o endpoint protection ausente, atualizações de sistema operacional vulnerabilidades do sistema SQL ausentes vulnerabilidades e vulnerabilidades de máquina virtual. Esses insights fornecem informações em tempo real sobre o estado de segurança dos recursos implantados e podem ajudá-lo a priorizar as ações de correção.

- monitora o Endpoint Protection ausente na Central de Segurança do Azure
- Atualizações do sistema devem ser instaladas em seus computadores
- Vulnerabilidades na configuração de segurança em seus computadores devem ser corrigidas.
- As vulnerabilidades de seus bancos de dados SQL devem ser corrigidas.
- Vulnerabilidades devem ser corrigidas por uma solução de avaliação de vulnerabilidade

## <a name="53-protective-monitoring"></a>5.3 monitoramento de proteção

Esta especificação técnica ajuda a proteger os ativos de informações de sistema, atribuindo um [política do Azure](../../../policy/overview.md) irrestrito de definição que monitora as contas de armazenamento. Esta especificação técnica também atribui uma definição de política do Azure que monitora a atividade de lista de permissões.

- Auditar o acesso irrestrito à rede para contas de armazenamento
- Controles de aplicativo adaptáveis deve ser habilitados em máquinas virtuais
- Implantar a detecção de ameaças em servidores SQL
- Implantar a extensão de IaaS antimalware da Microsoft do padrão para o Windows Server

## <a name="9-secure-user-management--10-identity-and-authentication"></a>Gerenciamento de usuário de seguro de 9 / 10 autenticação e identidade

O Azure implementa o acesso baseado em função de RBAC (controle) para Ajuda que você a gerenciar quem tem acesso aos recursos no Azure. Usando o portal do Azure, você pode examinar quem tem acesso aos recursos do Azure e suas permissões. Esta especificação técnica o ajuda a restringir e controlar os direitos de acesso privilegiado atribuindo seis [política do Azure](../../../policy/overview.md) definições para auditar contas externas com o proprietário e/ou contas com o proprietário e permissões de leitura e/ou gravação de leitura/gravação permissões que não têm a autenticação multifator habilitada.

- MFA deve ser habilitado em contas com permissões de proprietário em sua assinatura
- MFA deve ser habilitadas contas com permissões de gravação em sua assinatura
- MFA deve ser habilitado em contas com permissões de leitura em sua assinatura
- audita contas externas com permissões de proprietário em uma assinatura
- audita contas externas com permissões de gravação em uma assinatura
- audita contas externas com permissões de leitura em uma assinatura

Esta especificação técnica atribui duas definições de política do Azure para auditar o uso de autenticação do Active Directory do Azure para servidores SQL e o Service Fabric. O uso da autenticação do Azure Active Directory permite o gerenciamento simplificado de permissões e o gerenciamento centralizado de identidades dos usuários de banco de dados e de outros serviços da Microsoft.

- Um administrador do Azure Active Directory deve ser provisionado para servidores SQL
- Clusters do Service Fabric só devem usar o Azure Active Directory para autenticação de cliente

Esta especificação técnica também atribui cinco definições de política do Azure para auditar as contas que devem ser priorizadas para revisão, incluindo contas depreciadas e contas externas. Quando necessário, as contas podem ser impedidas de entrar (ou removidas), o que remove imediatamente os direitos de acesso aos recursos do Azure. Esta especificação técnica atribui duas definições de política do Azure à conta de auditoria depreciada que deve ser considerada para remoção.

- Contas preteridas devem ser removidas da sua assinatura
- Contas preteridas com permissões de proprietário devem ser removidas da sua assinatura
- Contas externas com permissões de proprietário devem ser removidas da sua assinatura
- As contas externas com permissões de gravação do proprietário devem ser removidas da sua assinatura

Esta especificação técnica também atribui uma definição de política do Azure que audita a VM do Linux permissões de arquivo de senha para alertar se eles são definidos incorretamente. Esse design permite que você tomar uma ação corretiva para garantir que autenticadores não estão comprometidos.

- [Visualização]: Audit Linux VM /etc/passwd file permissions are set to 0644

Esta especificação técnica o ajuda a impor senhas fortes, atribuindo 10 definições de política do Azure que as VMs do Windows que não impõem o nível mínimo e outros requisitos de senha de auditoria. O reconhecimento de VMs que estejam violando a política de força da senha ajuda você a tomar ações corretivas para garantir que as senhas de todas as contas de usuário da VM estejam em conformidade com a política.

- [Visualização]: Deploy requirements to audit Windows VMs that do not have the password complexity setting enabled
- [Visualização]: Deploy requirements to audit Windows VMs that do not have a maximum password age of 70 days
- [Visualização]: Deploy requirements to audit Windows VMs that do not have a minimum password age of 1 day
- [Visualização]: Deploy requirements to audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Visualização]: Deploy requirements to audit Windows VMs that allow re-use of the previous 24 passwords
- [Visualização]: Audit Windows VMs that do not have the password complexity setting enabled
- [Visualização]: Audit Windows VMs that do not have a maximum password age of 70 days
- [Visualização]: Audit Windows VMs that do not have a minimum password age of 1 day
- [Visualização]: Audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Visualização]: Audit Windows VMs that allow re-use of the previous 24 passwords

Esse plano gráfico também ajuda a controlar o acesso aos recursos do Azure por meio da atribuição sete definições de política do Azure. Essas políticas auditam o uso de tipos de recursos e configurações que podem permitir um acesso mais permissivo aos recursos. Entender os recursos que estão violando essas políticas pode ajudar você a tomar ações corretivas para garantir que o acesso aos recursos do Azure seja restrito a usuários autorizados.

- [Visualização]: Deploy requirements to audit Linux VMs that have accounts without passwords
- [Visualização]: Deploy requirements to audit Linux VMs that allow remote connections from accounts without passwords
- [Visualização]: Audit Linux VMs that have accounts without passwords
- [Visualização]: Audit Linux VMs that allow remote connections from accounts without passwords
- Contas de armazenamento devem ser migradas para novos recursos do Azure Resource Manager
- Máquinas virtuais devem ser migradas para novos recursos do Azure Resource Manager
- Auditar VMs que não usam discos gerenciados

## <a name="11-external-interface-protection"></a>Proteção de Interface externa 11

Além de usar as políticas de mais de 25 para o gerenciamento de usuário segura apropriado, esta especificação técnica ajuda a proteger as interfaces de serviço contra acesso não autorizado, atribuindo um [política do Azure](../../../policy/overview.md) definição irrestrito de monitores contas de armazenamento. Contas de armazenamento com acesso irrestrito podem permitir o acesso involuntário às informações contidas dentro do sistema de informações. Esta especificação técnica também atribui uma política que permite que os controles de aplicativo adaptáveis em máquinas virtuais.

- Auditar o acesso irrestrito à rede para contas de armazenamento
- Controles de aplicativo adaptáveis deve ser habilitados em máquinas virtuais

## <a name="12-secure-service-administration"></a>Administração do serviço de seguro 12

O Azure implementa o acesso baseado em função de RBAC (controle) para Ajuda que você a gerenciar quem tem acesso aos recursos no Azure. Usando o portal do Azure, você pode examinar quem tem acesso aos recursos do Azure e suas permissões. Esta especificação técnica o ajuda a restringir e controlar os direitos de acesso privilegiado atribuindo cinco [política do Azure](../../../policy/overview.md) definições para auditar contas externas com o proprietário e/ou contas e permissões de gravação com o proprietário e/ou permissões de gravação que não têm a autenticação multifator habilitada.

Sistemas usados para a administração de um serviço de nuvem têm acesso altamente privilegiado ao serviço. Um comprometimento teria um impacto significativo, incluindo os meios para ignorar os controles de segurança e roubar ou manipular grandes volumes de dados. Os métodos usados pelos administradores do provedor de serviços para gerenciar o serviço operacionais devem ser projetados para minimizar qualquer risco de exploração que poderia prejudicar a segurança do serviço. Se não for implementado esse princípio, um invasor pode ter os meios para ignorar os controles de segurança e roubar ou manipular grandes volumes de dados.

- MFA deve ser habilitado em contas com permissões de proprietário em sua assinatura
- MFA deve ser habilitadas contas com permissões de gravação em sua assinatura
- Contas externas com permissões de proprietário devem ser removidas da sua assinatura
- As contas externas com permissões de gravação do proprietário devem ser removidas da sua assinatura

Esta especificação técnica atribui duas definições de política do Azure para auditar o uso de autenticação do Active Directory do Azure para servidores SQL e o Service Fabric. O uso da autenticação do Azure Active Directory permite o gerenciamento simplificado de permissões e o gerenciamento centralizado de identidades dos usuários de banco de dados e de outros serviços da Microsoft.

- Um administrador do Azure Active Directory deve ser provisionado para servidores SQL
- Clusters do Service Fabric só devem usar o Azure Active Directory para autenticação de cliente

Esta especificação técnica também atribui quatro definições de política do Azure para auditar as contas que devem ser priorizadas para revisão, incluindo contas depreciadas e contas externas com permissões elevadas. Quando necessário, as contas podem ser impedidas de entrar (ou removidas), o que remove imediatamente os direitos de acesso aos recursos do Azure. Esta especificação técnica atribui duas definições de política do Azure à conta de auditoria depreciada que deve ser considerada para remoção.

- Contas preteridas devem ser removidas da sua assinatura
- Contas preteridas com permissões de proprietário devem ser removidas da sua assinatura
- Contas externas com permissões de proprietário devem ser removidas da sua assinatura
- As contas externas com permissões de gravação do proprietário devem ser removidas da sua assinatura

Esta especificação técnica também atribui uma definição de política do Azure que audita a VM do Linux permissões de arquivo de senha para alertar se eles são definidos incorretamente. Esse design permite que você tomar uma ação corretiva para garantir que autenticadores não estão comprometidos.

- [Visualização]: Audit Linux VM /etc/passwd file permissions are set to 0644

## <a name="13-audit-information-for-users"></a>Informações de auditoria de 13 para os usuários

Esta especificação técnica ajuda a garantir que os eventos de sistema são registrados por meio da atribuição 6 [política do Azure](../../../policy/overview.md) definições de configurações de auditoria log em recursos do Azure. Uma política atribuída também audita se as máquinas virtuais não estão enviando logs para um workspace especificado do Log Analytics.

- Monitorar os servidores SQL não auditados na Central de segurança do Azure
- Configuração de diagnóstico de auditoria
- Auditar configurações de Auditoria de nível do servidor SQL
- [Visualização]: Deploy Log Analytics Agent for Linux VMs
- [Visualização]: Deploy Log Analytics Agent for Windows VMs
- Implantar o observador de rede quando as redes virtuais são criadas

## <a name="next-steps"></a>Próximas etapas

Outros artigos sobre blueprints e como usá-los:

- Saiba mais sobre o [ciclo de vida do blueprint](../../concepts/lifecycle.md).
- Saiba como usar [parâmetros estáticos e dinâmicos](../../concepts/parameters.md).
- Saiba como personalizar a [ordem de sequenciamento de blueprint](../../concepts/sequencing-order.md).
- Saiba como usar o [bloqueio de recurso de blueprint](../../concepts/resource-locking.md).
- Saiba como [atualizar atribuições existentes](../../how-to/update-existing-assignments.md).

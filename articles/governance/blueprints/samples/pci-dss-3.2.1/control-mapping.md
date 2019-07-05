---
title: Mapeamento de controle - diagrama de PCI-DSS v3.2.1 - exemplo
description: Mapeamento de controle de exemplo para a política do Azure e o RBAC blueprint v3.2.1 Payment Card Industry Data Security Standard.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 38b1cc6249da98e11167416c8e18d06de1645679
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540942"
---
# <a name="control-mapping-of-the-pci-dss-v321-blueprint-sample"></a>Mapeamento de controle de exemplo de especificações técnicas do v3.2.1 PCI-DSS

O artigo a seguir fornece detalhes sobre como o exemplo de especificações técnicas do Azure BluePrint PCI-DSS v3.2.1 é mapeada para os controles de v3.2.1 PCI-DSS. Para obter mais informações sobre os controles, consulte [PCI-DSS v3.2.1](https://www.pcisecuritystandards.org/documents/PCI_DSS_v3-2-1.pdf).

Os seguintes mapeamentos são para o **v3.2.1:2018 de PCI-DSS** controles. Use a navegação no lado direito para ir diretamente para um mapeamento de controle específico. Muitos dos controles mapeados são implementados com uma iniciativa do [Azure Policy](../../../policy/overview.md). Para examinar a iniciativa completa, abra **Política** no portal do Azure e selecione a página **Definições**. Em seguida, localize e selecione o  **[visualização] v3.2.1:2018 do PCI de auditoria controla e implantar extensões de VM específicas para dar suporte a requisitos de auditoria** iniciativa de política interna.

## <a name="132-and-134-boundary-protection"></a>1.3.2 e 1.3.4 proteção de limite

Esta especificação técnica o ajuda a gerenciar e controlar redes atribuindo [política do Azure](../../../policy/overview.md) definições que monitora os grupos de segurança de rede com regras permissivos. Regras que são muito permissivas podem permitir o acesso não intencional à rede e devem ser examinadas. Esta especificação técnica atribui um definições de política do Azure que monitora os pontos de extremidade desprotegidos, aplicativos e contas de armazenamento. Os pontos de extremidade e os aplicativos que não estão protegidos por um firewall e as contas de armazenamento com acesso irrestrito podem permitir o acesso não intencional às informações contidas no sistema de informações.

- Auditar o acesso irrestrito à rede para contas de armazenamento
- Acesso por meio do ponto de extremidade voltado para a Internet deve ser restrito

## <a name="34a-41-41g-41h-and-653-cryptographic-protection"></a>3.4.a, 4.1, 4.1.g, 4.1.h e 6.5.3 proteção criptográfica

Esta especificação técnica o ajuda a impor a política com o uso de controles cryptograph atribuindo [política do Azure](../../../policy/overview.md) definições que impõem controles cryptograph específico e auditoria usam configurações de criptografia fraca. Entender em que local os recursos do Azure podem ter configurações de criptografia não ideais pode ajudá-lo a tomar ações corretivas para garantir que os recursos sejam configurados de acordo com a política de segurança de informações. Especificamente, as políticas atribuídas por esta especificação técnica exigem transparent data encryption em bancos de dados SQL; auditoria faltando criptografia em contas de armazenamento e as variáveis de conta de automação. Também há políticas qual endereço conexões inseguras para contas de armazenamento, aplicativos de funções, aplicativo Web, aplicativos de API e o Cache Redis de auditoria e auditar a comunicação não criptografada do Service Fabric.

- O aplicativo de funções deve ser acessível apenas por HTTPS
- Aplicativo Web deve ser acessível somente por HTTPS
- API App só deve ser acessado via HTTPS
- monitora o banco de dados SQL não criptografado na Central de Segurança do Azure
- Criptografia de disco deve ser aplicada em máquinas virtuais
- Variáveis de conta de automação devem ser criptografadas.
- Somente conexões seguras para seu Cache Redis devem ser habilitadas
- Transferência segura para contas de armazenamento deve ser habilitada
- Clusters do Service Fabric devem ter a propriedade ClusterProtectionLevel definida como EncryptAndSign
- Transparent Data Encryption em bancos de dados SQL deve ser habilitado
- Implantar a transparent data encryption do banco de dados SQL

## <a name="51-62-66-and-1121-vulnerability-scanning-and-system-updates"></a>5.1, 6.2, 6.6 e 11.2.1 verificação de vulnerabilidade e as atualizações do sistema

Esta especificação técnica ajuda a gerenciar as vulnerabilidades do sistema de informações por meio da atribuição [política do Azure](../../../policy/overview.md) definições que monitoram as atualizações do sistema, vulnerabilidades do sistema operacional, vulnerabilidades de SQL e máquina virtual ausente vulnerabilidades na Central de segurança do Azure. A Central de Segurança do Azure fornece funcionalidades de relatórios que permitem ter insights em tempo real sobre o estado de segurança de recursos implantados do Azure.

- monitora o Endpoint Protection ausente na Central de Segurança do Azure
- Implantar a extensão da Microsoft IaaSAntimalware padrão para o Windows Server
- Implantar a detecção de ameaças em servidores SQL
- Atualizações do sistema devem ser instaladas em seus computadores
- Vulnerabilidades na configuração de segurança em seus computadores devem ser corrigidas.
- As vulnerabilidades de seus bancos de dados SQL devem ser corrigidas.
- Vulnerabilidades devem ser corrigidas por uma solução de avaliação de vulnerabilidade

## <a name="711-712-and-713-separation-of-duties"></a>7.1.1. 7.1.2 e 7.1.3 separação de funções

Ter apenas um proprietário de assinatura do Azure não permite a redundância administrativa. Por outro lado, ter muitos proprietários de assinatura do Azure pode aumentar o potencial de uma violação por meio de uma conta de proprietário comprometida. Esta especificação técnica o ajuda a manter um número apropriado de proprietários de assinatura do Azure, atribuindo [política do Azure](../../../policy/overview.md) definições que o número de proprietários de assinaturas do Azure de auditoria. O gerenciamento de permissões de proprietário de assinatura pode ajudá-lo a implementar uma diferenciação de direitos apropriada.

- Deve haver mais de um proprietário atribuído à sua assinatura
- Um máximo de 3 proprietários deve ser designado para a sua assinatura 

## <a name="32-721-831a-and-831b-management-of-privileged-access-rights"></a>3.2, 7.2.1, 8.3.1.a e 8.3.1.b gerenciamento de direitos de acesso privilegiado

Esta especificação técnica o ajuda a restringir e controlar os direitos de acesso privilegiado atribuindo [política do Azure](../../../policy/overview.md) definições para auditar contas externas com o proprietário, gravar e/ou permissões de leitura e contas de funcionários com o proprietário e/ou gravação permissões que não têm a autenticação multifator habilitada. O Azure implementa o RBAC (controle de acesso baseado em função) para gerenciar quem tem acesso aos recursos no Azure. Entender o local em que as regras RBAC personalizadas são implementadas pode ajudar você a verificar a necessidade e a implementação apropriada, pois as regras RBAC personalizadas estão sujeitas a erros. Isso também blueprint atribui [política do Azure](../../../policy/overview.md) definições para auditar usam de autenticação do Active Directory do Azure para servidores SQL. Usando o Azure Active Directory autenticação simplifica o gerenciamento de permissão e centraliza o gerenciamento de identidade de usuários de banco de dados e outros produtos da Microsoft  
Serviços.
 
- Contas externas com permissões de proprietário devem ser removidas da sua assinatura
- As contas externas com permissões de gravação do proprietário devem ser removidas da sua assinatura
- Contas externas com permissões de leitura devem ser removidas da sua assinatura
- MFA deve ser habilitado em contas com permissões de proprietário em sua assinatura
- MFA deve ser habilitadas contas com permissões de gravação em sua assinatura
- MFA deve ser habilitado em contas com permissões de leitura em sua assinatura
- Um administrador do Azure Active Directory deve ser provisionado para servidores SQL
- Auditar o uso de regras personalizadas do RBAC

## <a name="812-and-815-least-privilege-and-review-of-user-access-rights"></a>8.1.2 e 8.1.5 menos privilégios e revisão de usuário de direitos de acesso

O Azure implementa o acesso baseado em função de RBAC (controle) para Ajuda que você a gerenciar quem tem acesso aos recursos no Azure. Usando o portal do Azure, você pode examinar quem tem acesso aos recursos do Azure e suas permissões. Esta especificação técnica atribui [política do Azure](../../../policy/overview.md) definições para auditar as contas que devem ser priorizadas para revisão, incluindo contas depreciadas e contas externas com permissões elevadas.

- Contas preteridas devem ser removidas da sua assinatura
- Contas preteridas com permissões de proprietário devem ser removidas da sua assinatura
- Contas externas com permissões de proprietário devem ser removidas da sua assinatura
- As contas externas com permissões de gravação do proprietário devem ser removidas da sua assinatura
- Contas externas com permissões de leitura devem ser removidas da sua assinatura

## <a name="813-removal-or-adjustment-of-access-rights"></a>8.1.3 Remoção de ou ajuste de direitos de acesso

O Azure implementa o controle de acesso baseado em função (RBAC) para ajudá-lo a gerenciar quem tem acesso aos recursos no Azure. Usando o Azure Active Directory e o RBAC, você pode atualizar as funções de usuário para refletir alterações organizacionais. Quando necessário, as contas podem ser impedidas de entrar (ou removidas), o que remove imediatamente os direitos de acesso aos recursos do Azure. Esta especificação técnica atribui [política do Azure](../../../policy/overview.md) definições para auditar conta depreciada que deve ser considerada para remoção.

- Contas preteridas devem ser removidas da sua assinatura
- Contas preteridas com permissões de proprietário devem ser removidas da sua assinatura

## <a name="823ab-824ab-and-825-password-based-authentication"></a>8.2.3.a,b, 8.2.4.a,b e 8.2.5 autenticação baseada em senha

Esta especificação técnica o ajuda a impor senhas fortes, atribuindo [política do Azure](../../../policy/overview.md) definições de auditar VMs do Windows que não impõem o nível mínimo e outros requisitos de senha. O reconhecimento de VMs que estejam violando a política de força da senha ajuda você a tomar ações corretivas para garantir que as senhas de todas as contas de usuário da VM estejam em conformidade com a política.

- [Visualização]: Audit Windows VMs that do not have a maximum password age of 70 days
- [Visualização]: Deploy requirements to audit Windows VMs that do not have a maximum password age of 70 days
- [Visualização]: Audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Visualização]: Deploy requirements to audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Visualização]: Audit Windows VMs that allow re-use of the previous 24 passwords
- [Visualização]: Deploy requirements to audit Windows VMs that allow re-use of the previous 24 passwords

## <a name="103-and-1054-audit-generation"></a>10.3 e 10.5.4 de geração de auditoria

Esta especificação técnica ajuda a garantir que os eventos de sistema são registrados por meio da atribuição [política do Azure](../../../policy/overview.md) definições de configurações de auditoria log em recursos do Azure.
Os logs de diagnóstico fornecem insights sobre operações realizadas em recursos do Azure. Os logs do Azure dependem de relógios internos sincronizados para criar um registro de eventos relacionado à hora em todos os recursos.

- Monitorar os servidores SQL não auditados na Central de segurança do Azure
- Configuração de diagnóstico de auditoria
- Auditar configurações de Auditoria de nível do servidor SQL
- Implantar auditoria em servidores SQL
- Contas de armazenamento devem ser migradas para novos recursos do Azure Resource Manager
- Máquinas virtuais devem ser migradas para novos recursos do Azure Resource Manager

## <a name="1236-and-1237-information-security"></a>12.3.6 e 12.3.7 de segurança de informações

Esta especificação técnica o ajuda a gerenciar e controlar sua rede, atribuindo [política do Azure](../../../policy/overview.md) permitidas definições que os locais de rede aceitáveis e produtos da empresa aprovados de auditoria para o ambiente. Esses são personalizáveis por cada empresa por meio dos parâmetros de política dentro de cada uma dessas políticas.

- Locais permitidos
- Locais para grupos de recursos permitidos

## <a name="next-steps"></a>Próximas etapas

Agora que você analisou o mapeamento de controle do plano gráfico v3.2.1 de PCI DSS, visite os seguintes artigos para saber mais sobre a visão geral e como implantar este exemplo:

> [!div class="nextstepaction"]
> [Diagrama de v3.2.1 de PCI DSS - visão geral](./index.md)
> [v3.2.1 de PCI-DSS blueprint - as etapas de implantação](./deploy.md)

## <a name="addition-articles-about-blueprints-and-how-to-use-them"></a>Outros artigos sobre blueprints e como usá-los:

- Saiba mais sobre o [ciclo de vida do blueprint](../../concepts/lifecycle.md).
- Saiba como usar [parâmetros estáticos e dinâmicos](../../concepts/parameters.md).
- Saiba como personalizar a [ordem de sequenciamento de blueprint](../../concepts/sequencing-order.md).
- Saiba como usar o [bloqueio de recurso de blueprint](../../concepts/resource-locking.md).
- Saiba como [atualizar atribuições existentes](../../how-to/update-existing-assignments.md).

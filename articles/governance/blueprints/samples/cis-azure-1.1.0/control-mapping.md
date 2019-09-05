---
title: Amostra – Blueprint do CIS Microsoft Azure Foundations Benchmark – mapeamento de recomendação
description: Mapeamento de recomendação da amostra de blueprint do CIS Microsoft Azure Foundations Benchmark para o Azure Policy.
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/09/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: d02dca4438b55d51cccb5cc1b55679eb72b73991
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232810"
---
# <a name="recommendation-mapping-of-the-cis-microsoft-azure-foundations-benchmark-blueprint-sample"></a>Mapeamento de recomendação da amostra de blueprint do CIS Microsoft Azure Foundations Benchmark

O artigo a seguir fornece detalhes sobre como a amostra do blueprint do CIS Microsoft Azure Foundations Benchmark do Azure Blueprints é mapeada para as recomendações do CIS Microsoft Azure Foundations Benchmark. Para saber mais sobre as recomendações, confira [CIS Microsoft Azure Foundations Benchmark](https://www.cisecurity.org/benchmark/azure/).

Os mapeamentos a seguir são para as recomendações do **CIS Microsoft Azure Foundations Benchmark v1.1.0**. Use a navegação no lado direito para ir diretamente para um mapeamento de recomendação específico.
Muitas das recomendações mapeadas são implementadas com uma iniciativa do [Azure Policy](../../../policy/overview.md). Para examinar a iniciativa completa, abra **Política** no portal do Azure e selecione a página **Definições**. Em seguida, localize e selecione as recomendações da **\[Versão Prévia\] de Audit CIS Microsoft Azure Foundations Benchmark v1.1.0 e implante Extensões de VM específicas para dar suporte aos requisitos de auditoria** da iniciativa de política interna.

> [!NOTE]
> A amostra completa do blueprint estará disponível em breve. A iniciativa associada do Azure Policy está disponível agora.

## <a name="11-ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>1.1 Garantir que a autenticação multifator esteja habilitada para todos os usuários privilegiados

Este blueprint atribui definições do [Azure Policy](../../../policy/overview.md) que se alinham com esta recomendação de CIS.

- O MFA deve ser habilitado em contas com permissões de proprietário em sua assinatura
- O MFA deve ser habilitado em contas com permissões de gravação em sua assinatura

## <a name="12-ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>1.2 Garantir que a autenticação multifator esteja habilitada para todos os usuários não privilegiados

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- O MFA deve ser habilitado em contas com permissões de leitura em sua assinatura

## <a name="13-ensure-that-there-are-no-guest-users"></a>1.3 Garantir que não haja usuários convidados

Este blueprint atribui definições do [Azure Policy](../../../policy/overview.md) que se alinham com esta recomendação de CIS.

- As contas externas com permissões de proprietário devem ser removidas de sua assinatura
- As contas externas com permissões de leitura devem ser removidas de sua assinatura
- As contas externas com permissões de gravação devem ser removidas de sua assinatura

## <a name="23-ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>2.3 Garantir que a configuração padrão da política do ASC "Monitorar Atualizações do Sistema" não esteja "Desabilitada"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- As atualizações do sistema devem ser instaladas em suas máquinas

## <a name="24-ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>2.4 Garantir que a configuração padrão da política do ASC "Monitorar Vulnerabilidades do SO" não esteja "Desabilitada"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- As vulnerabilidades da configuração de segurança nas máquinas devem ser corrigidas

## <a name="25-ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>2.5 Garantir que a configuração padrão da política do ASC "Monitorar a Proteção do Ponto de Extremidade" não esteja "Desabilitada"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- monitora o Endpoint Protection ausente na Central de Segurança do Azure

## <a name="26-ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>2.6 Garantir que a configuração padrão da política do ASC "Monitorar Criptografia de Disco" não esteja "Desabilitada"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- A criptografia de disco deve ser aplicada em máquinas virtuais

## <a name="28-ensure-asc-default-policy-setting-monitor-web-application-firewall-is-not-disabled"></a>2.8 Garantir que a configuração padrão da política do ASC "Monitorar o Firewall do Aplicativo Web" não esteja "Desabilitada"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- As regras NSGs para aplicativos Web em IaaS devem ser protegidas

## <a name="210-ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>2.10 Garantir que a configuração padrão da política do ASC "Monitorar Avaliação de Vulnerabilidade" não esteja "Desabilitada"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- As vulnerabilidades devem ser corrigidas por uma solução de Avaliação de Vulnerabilidades

## <a name="212-ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>2.12 Garantir que a configuração padrão da política do ASC "Monitorar o Acesso à Rede JIT" não esteja "Desabilitada"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- O controle de acesso à rede just-in-time deve ser aplicado em máquinas virtuais

## <a name="215-ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>2.15 Garantir que a configuração padrão da política do ASC "Monitorar a Criptografia do SQL" não esteja "Desabilitada"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- A Transparent Data Encryption em bancos de dados SQL deve ser habilitada

## <a name="31-ensure-that-secure-transfer-required-is-set-to-enabled"></a>3.1 Garantir que "Transferência segura necessária" esteja definido como "Habilitado"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- A transferência segura para contas de armazenamento deve ser habilitada

## <a name="37-ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>3.7 Garantir que a regra de acesso de rede padrão para Contas de Armazenamento estejam definidas como negar

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- Auditar o acesso irrestrito à rede para contas de armazenamento

## <a name="41-ensure-that-auditing-is-set-to-on"></a>4.1 Garantir que "Auditoria" esteva definido como "Ativado"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- A auditoria deve ser habilitada nas configurações de segurança de dados avançada no SQL Server

## <a name="42-ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>4.2 Garantir que "AuditActionGroups" na política de "auditoria" de um SQL Server esteja definido corretamente

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- As configurações de Auditoria do SQL devem ter grupos de ações configurados para capturar atividades críticas

## <a name="43-ensure-that-auditing-retention-is-greater-than-90-days"></a>4.3 Garantir que a retenção de "Auditoria" seja "maior que 90 dias"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- Os servidores SQL devem ser configurados com um período de retenção de auditoria superior a 90 dias.

## <a name="44-ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>4.4 Garantir que a "Segurança de Dados Avançada" em um SQL Server esteja definida como "Ativado"

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- A Segurança de Dados Avançada deve ser habilitada nos servidores SQL

## <a name="45-ensure-that-threat-detection-types-is-set-to-all"></a>4.5 Garantir que 'Tipos de Detecção de Ameaças' esteja definido como 'Todos'

Este blueprint atribui definições do [Azure Policy](../../../policy/overview.md) que se alinham com esta recomendação de CIS.

- Os tipos de Proteção Avançada contra Ameaças devem estar definidos como “Todos” nas configurações de Segurança de Dados Avançada do SQL Server
- Os tipos de Proteção Avançada contra Ameaças devem estar definidos como “Todos” nas configurações de Segurança de Dados Avançada da instância gerenciada de SQL

## <a name="46-ensure-that-send-alerts-to-is-set"></a>4.6 Garantir que 'Enviar alertas para' esteja configurado

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- As configurações da Segurança de Dados Avançada para o SQL Server devem conter um endereço de email para receber alertas de segurança

## <a name="47-ensure-that-email-service-and-co-administrators-is-enabled"></a>4.7 Garantir que 'Serviço de email e coadministradores' esteja 'Habilitado'

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- As configurações da Segurança de Dados Avançada para a instância gerenciada do SQL devem conter um endereço de email para receber alertas de segurança

## <a name="48-ensure-that-azure-active-directory-admin-is-configured"></a>4.8 Garantir que o administrador do Azure Active Directory esteja configurado

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- Um administrador do Azure Active Directory deve ser provisionado para servidores SQL

## <a name="49-ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>4.9 Garantir que a 'Criptografia de Dados' esteja definida como 'Ativado' em um Banco de Dados SQL

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- A Transparent Data Encryption em bancos de dados SQL deve ser habilitada

## <a name="410-ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>4.10 Garantir que o protetor de TDE do SQL Server seja criptografado com BYOK (Bring Your Own Key)

Este blueprint atribui definições do [Azure Policy](../../../policy/overview.md) que se alinham com esta recomendação de CIS.

- O protetor TDE do SQL Server deve ser criptografado com sua própria chave
- O protetor TDE da instância gerenciada do SQL deve ser criptografado com sua própria chave

## <a name="517-ensure-that-logging-for-azure-keyvault-is-enabled"></a>5.1.7 Garantir que o log do Azure Key Vault esteja 'Habilitado'

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- Os logs de diagnóstico no Key Vault deve estar habilitados

## <a name="71-ensure-that-os-disk-are-encrypted"></a>7.1 Garantir que o 'Disco do SO' seja criptografado

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- A criptografia de disco deve ser aplicada em máquinas virtuais

## <a name="72-ensure-that-data-disks-are-encrypted"></a>7.2 Garantir que os 'Discos de dados' sejam criptografados

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- A criptografia de disco deve ser aplicada em máquinas virtuais

## <a name="75-ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>7.5 Garantir que os Patches de SO mais recentes para todas as máquinas virtuais sejam aplicados

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- As atualizações do sistema devem ser instaladas em suas máquinas

## <a name="76-ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>7.6 Garantir que a proteção de ponto de extremidade para todas as máquinas virtuais esteja instalada

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- monitora o Endpoint Protection ausente na Central de Segurança do Azure

## <a name="85-enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>8.5 Habilitar o RBAC (controle de acesso baseado em função) nos Serviços de Kubernetes do Azure

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- \[Versão Prévia\]: O RBAC (controle de acesso baseado em função) deve ser usado nos Serviços de Kubernetes

## <a name="92-ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>9.2 Garantir que o aplicativo Web redirecione todo o tráfego HTTP para HTTPS no Serviço de Aplicativo do Azure

Esse blueprint atribui uma definição do [Azure Policy](../../../policy/overview.md) que se alinha com esta recomendação de CIS.

- Aplicativo Web deve ser acessível somente por HTTPS

## <a name="next-steps"></a>Próximas etapas

Agora que você examinou o mapeamento de controle do blueprint CIS Microsoft Azure Foundations Benchmark, visite o seguinte artigo para saber mais sobre o blueprint ou visite o Azure Policy no portal do Azure para atribuir a iniciativa:

> [!div class="nextstepaction"]
> [Blueprint CIS Microsoft Azure Foundations Benchmark – Visão geral](./index.md)
> [Portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Definitions)

Outros artigos sobre blueprints e como usá-los:

- Saiba mais sobre o [ciclo de vida do blueprint](../../concepts/lifecycle.md).
- Saiba como usar [parâmetros estáticos e dinâmicos](../../concepts/parameters.md).
- Saiba como personalizar a [ordem de sequenciamento de blueprint](../../concepts/sequencing-order.md).
- Saiba como usar o [bloqueio de recurso de blueprint](../../concepts/resource-locking.md).
- Saiba como [atualizar atribuições existentes](../../how-to/update-existing-assignments.md).
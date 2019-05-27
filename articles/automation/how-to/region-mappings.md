---
title: Mapeamentos de espaço de trabalho de automação e o Log Analytics do Azure
description: Este artigo descreve os mapeamentos permitidos entre uma conta de automação e um espaço de trabalho do Log Analytics para dar suporte à solução
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/20/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8afe8fed8912dd365071f1c4c5560c9f5578dcd8
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66133112"
---
# <a name="workspace-mappings"></a>Mapeamentos de espaço de trabalho

Ao habilitar soluções como o gerenciamento de atualizações, controle de alterações e inventário ou iniciar/parar VMs durante a solução de fora do horário comercial, somente determinadas regiões têm suporte para vincular um espaço de trabalho do Log Analytics e uma conta de automação. Esse mapeamento se aplica somente à conta de automação e o espaço de trabalho do Log Analytics. Os recursos de emissão de relatórios para seu espaço de trabalho de conta de automação ou o Log Analytics podem residir em outras regiões.

## <a name="supported-mappings"></a>Mapeamentos com suporte

A tabela abaixo mostra os mapeamentos com suporte:

|**Região do Workspace do Log Analytics**|**Região da Automação do Azure**|
|---|---|
|**EUA**||
|EastUS<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS<sup>2</sup>|WestCentralUS<sup>2</sup>|
|**Canadá**||
|Canadá Central|Canadá Central|
|**Pacífico Asiático**||
|AustraliaSoutheast|AustraliaSoutheast|
|SoutheastAsia|SoutheastAsia|
|CentralIndia|CentralIndia|
|JapanEast|JapanEast|
|**Europa**||
|UKSouth|UKSouth|
|WestEurope|WestEurope|
|**US Gov**||
|USGovVirginia|USGovVirginia|

<sup>1</sup> EastUS mapeamento para espaços de trabalho do Log Analytics para contas de automação não é um mapeamento de região para região exato, mas é o mapeamento correto.

<sup>2</sup> devido a restrições de capacidade a região não estiver disponível durante a criação de novos recursos. Isso inclui contas de automação e o Log Analytics para espaços de trabalho. No entanto, os recursos vinculados pré-existentes na região devem continuam a funcionar.

## <a name="unlink-workspace"></a>Desvincular o workspace

Se você decidir que você não deseja mais integrar sua conta de automação com um espaço de trabalho do Log Analytics, você pode desvincular sua conta diretamente do portal do Azure. Antes de prosseguir, primeiro você precisa remover o gerenciamento de atualizações, controle de alterações e inventário ou iniciar/parar VMs durante soluções fora do horário comercial, se você estiver usando-os. Se você não removê-los, esse processo será impedido de continuar. Examine o artigo sobre a solução específica que você importou para entender as etapas necessárias para removê-la.

Depois de remover essas soluções, você poderá executar as etapas a seguir para desvincular sua conta de Automação.

> [!NOTE]
> Algumas soluções, incluindo versões anteriores da solução de monitoramento do Azure SQL, podem ter criado ativos de automação e também podem precisar ser removidas antes de desvincular o workspace.

1. No portal do Azure, abra sua conta da Automação e, na página da conta da Automação, selecione **Workspace vinculado** na seção **Recursos Relacionados** à esquerda.

2. Na página Desvincular o workspace, clique em **Desvincular o workspace**. Você receberá um prompt perguntando se que deseja continuar.

3. Enquanto a Automação do Azure tenta desvincular a conta do seu espaço de trabalho do Log Analytics, você pode acompanhar o progresso no menu **Notificações**.

Se você tiver usado a solução Gerenciamento de Atualizações, como opção, convém remover os itens a seguir que não serão mais necessários após a remoção da solução.

* Atualizar agendas - Cada uma terá nomes que correspondam às implantações de atualizações que você criou)

* Grupos de trabalho híbrido criados para a solução - Cada um receberá um nome semelhante a machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8).

Se você tiver usado a solução Iniciar/parar VMs durante os horários fora de pico, como opção, convém remover os itens a seguir que não serão mais necessários após a remoção da solução.

* Iniciar e parar agendas de runbook da VM
* Iniciar e parar runbooks da VM
* variáveis

Como alternativa, você também pode desvincular seu espaço de trabalho de sua conta de automação do espaço de trabalho do Log Analytics. No espaço de trabalho, selecione **conta de automação** sob **recursos relacionados**. Na página conta de automação, selecione **desvincular conta**.

## <a name="next-steps"></a>Próximas etapas

Saiba como integrar as soluções a seguir:

Gerenciamento de atualizações e controle de alterações e inventário:

* De um [máquina virtual](../automation-onboard-solutions-from-vm.md)
* De seu [conta de automação](../automation-onboard-solutions-from-automation-account.md)
* Quando [navegação várias máquinas](../automation-onboard-solutions-from-browse.md)
* De um [runbook](../automation-onboard-solutions.md)

Iniciar/Parar VMs durante os horários inativos

* [Implantar iniciar/parar VMs fora do horário comercial](../automation-solution-vm-management.md)
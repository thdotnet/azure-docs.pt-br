---
title: Habilitar o Azure Monitor para máquinas virtuais usando o Azure Policy | Microsoft Docs
description: Este artigo descreve como habilitar os Azure Monitor para máquinas virtuais para várias máquinas virtuais ou conjuntos de dimensionamento de máquina virtual usando a política do Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: magoedte
ms.openlocfilehash: 9664ad5272ffeb55bd85e3d4c4f4b70d05e97702
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524139"
---
# <a name="enable-azure-monitor-for-vms-preview-using-azure-policy"></a>Habilitar o Azure Monitor para máquinas virtuais usando a política do Azure (visualização)

Este artigo explica como habilitar o Azure Monitor para máquinas virtuais (versão prévia) para máquinas virtuais ou conjuntos de dimensionamento de máquina virtual usando a política do Azure. No final deste processo, você terá com êxito configurado a habilitação do Log Analytics e agentes de dependência e identificamos máquinas virtuais que não são compatíveis.

Para descobrir, gerenciar e habilitar o Azure Monitor para máquinas virtuais para todas as suas máquinas virtuais ou conjuntos de dimensionamento de máquina virtual, você pode usar a política do Azure ou Azure PowerShell. A política do Azure é o método recomendado, pois você pode gerenciar as definições de política para controlar efetivamente suas assinaturas para garantir a conformidade consistente e habilitação automática de recentemente provisionado VMs. Essas definições de política:

* Implantam o agente do Log Analytics e o Dependency Agent.
* Relatam os resultados da conformidade.
* Corrigem VMs fora de conformidade.

Se você estiver interessado em realizar isso com o modelo do Azure PowerShell ou do Azure Resource Manager, consulte [habilitar o uso do modelo do Azure PowerShell ou do Resource Manager](vminsights-enable-at-scale-powershell.md). 

## <a name="manage-policy-coverage-feature-overview"></a>Gerenciar visão geral do recurso de cobertura de política

Originalmente a experiência com o Azure Policy para gerenciar e implantar as definições de política para o Azure Monitor para VMs foi executada exclusivamente do Azure Policy. Com o **Gerenciar política de cobertura** recurso, ela torna mais simples e mais fácil de descobrir, gerenciar e habilitar em grande escala a **habilitar o Azure Monitor para VMs** iniciativa, que inclui as definições de política mencionado anteriormente. Você pode acessar esse novo recurso do **começar** guia no Azure Monitor para máquinas virtuais selecionando **Gerenciar política de cobertura**. Ele abre a página de cobertura de política de VMs. 

![O Azure Monitor na guia máquinas virtuais começar](./media/vminsights-enable-at-scale-policy/get-started-page-01.png)

Aqui você pode verificar e gerenciar cobertura para a iniciativa por grupos de gerenciamento e assinaturas, bem como entender quantas VMs existem em cada um dos grupos de gerenciamento, assinaturas e seus status de conformidade.   

![O Azure Monitor para a página de política de gerenciar VMs](./media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Essas informações são úteis para planejar e executar seu cenário de governança para o Azure Monitor para VMs de um local central. Enquanto a política do Azure fornece uma exibição de conformidade quando uma política/iniciativa é atribuída a um escopo, com essa nova página você pode descobrir onde a iniciativa de política/não está atribuído e atribuí-lo no local. Todas as ações (atribuir, exibir, editar) redirecionar a política do Azure diretamente. O Azure Monitor para a página de cobertura de política de VMs é uma experiência de expandida e integrada para apenas a iniciativa **habilitar o Azure Monitor para VMs**. 

Nesta página você também pode configurar seu espaço de trabalho do Log Analytics para o Azure Monitor para VMs, que executa o seguinte:

- Instala as soluções de mapa do serviço de instalação e Insights de infraestrutura
- Permite que os contadores de desempenho do sistema operacional usados pelos gráficos de desempenho, pastas de trabalho e suas consultas de log personalizado e alertas.

![Configurar o Azure Monitor para VMs do espaço de trabalho](./media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Essa opção não está relacionada a qualquer ação de política e está disponível para fornecer uma maneira fácil de atender a [pré-requisitos](vminsights-enable-overview.md) necessário para habilitar o Azure Monitor para VMs.  

### <a name="what-information-is-available-on-this-page"></a>Quais informações estão disponíveis nesta página?
A tabela a seguir fornece uma divisão de quais informações são apresentadas na página de cobertura de política e como interpretá-lo.

| Função | Descrição | 
|----------|-------------| 
| **Escopo** | Grupo de gerenciamento e assinaturas que você possui ou herdada de acesso para com capacidade de fazer uma busca detalhada por meio do gerenciamento de grupo de hierarquia.|
| **Função** | Sua função ao escopo, pode ser o proprietário de leitor ou colaborador. Em alguns casos, pode aparecer em branco para indicar que você pode ter acesso à assinatura, mas não para o grupo de gerenciamento pertence a. Informações de outras colunas irão variar dependendo da sua função conforme ele é a chave para determinar quais dados você pode ver e ações que você pode executar em termos de atribuição de iniciativa/diretiva (proprietário), editá-los ou exibir a conformidade. |
| **Total de VMs** | Número de VMs sob o escopo. Para um grupo de gerenciamento é uma soma de VMs aninhado sob o grupo de gerenciamento filho e/ou assinaturas. |
| **Cobertura de atribuição** | Porcentagem de VMs que são cobertas pela iniciativa/política. |
| **Status de atribuição** | Sob essa coluna, você pode encontrar informações sobre o status da sua política / iniciativa atribuição. |
| **VMs compatíveis** | Número de VMs que são compatíveis com a iniciativa de política /. Para a iniciativa **habilitar o Azure Monitor para VMs** este é o número de VMs que têm o agente do Log Analytics e o agente de dependência. Em alguns casos, pode aparecer em branco devido a nenhuma atribuição, ou nenhuma VM ou não há permissões suficientes. Informações são fornecidas em estado de conformidade. |
| **Conformidade** | O número da conformidade geral é a soma dos recursos distintos que estão em conformidade dividida pela soma de todos os recursos distintos. |
| **Estado de conformidade** | Informações sobre o estado de conformidade para sua política / iniciativa atribuição.|

Quando você atribui a iniciativa/política, o escopo selecionado na atribuição pode ser o escopo listados ou um subconjunto dela. Por exemplo, você pode ter criado uma atribuição para uma assinatura (escopo de política) e não um grupo de gerenciamento (escopo de cobertura). Nesse caso, o valor de **cobertura de atribuição** indicaria que o escopo de VMs na política (ou iniciativa) dividido por máquinas virtuais no escopo de cobertura. Em outro caso, você pode ter excluído algumas VMs, grupos de recursos ou uma assinatura do escopo de política. Se o valor estiver em branco, ele indica que a iniciativa de política/não existe ou você não tem permissão (informações são fornecidas no Status de atribuição).

## <a name="enable-using-azure-policy"></a>Habilitar usando o Azure Policy

Para habilitar o Azure Monitor para VMs usando o Azure Policy no locatário:

- Atribua a iniciativa a um escopo: grupo de gerenciamento, assinatura ou grupo de recursos
- Examinar e corrigir os resultados de conformidade

Para obter mais informações sobre como atribuir o Azure Policy, confira [Visão geral do Azure Policy](../../governance/policy/overview.md#policy-assignment) e examine a [Visão geral dos grupos de gerenciamento](../../governance/management-groups/index.md) antes de continuar.

### <a name="policies-for-azure-vms"></a>Políticas para VMs do Azure

As definições de política para uma VM do Azure estão listadas na tabela a seguir:

|NOME |Descrição |Type |
|-----|------------|-----|
|[Visualização]: Habilitar o Azure Monitor para VMs |Habilite o Azure Monitor para as VMs (Máquinas Virtuais) no escopo especificado (grupo de gerenciamento, assinatura ou grupo de recursos). Usa o espaço de trabalho do Log Analytics como parâmetro. |Iniciativa |
|[Visualização]: Auditar a Implantação do Dependency Agent – Imagem de VM (sistema operacional) não listada |Relata as VMs como fora de conformidade se a Imagem de VM (sistema operacional) não está definida na lista e o agente não está instalado. |Política |
|[Visualização]: Auditar a Implantação do Agente do Log Analytics – Imagem de VM (sistema operacional) não listada |Relata as VMs como fora de conformidade se a Imagem de VM (sistema operacional) não está definida na lista e o agente não está instalado. |Política |
|[Visualização]: Implantar o Dependency Agent para VMs do Linux |Implante o Dependency Agent para VMs do Linux se a Imagem de VM (sistema operacional) estiver definida na lista e o agente não estiver instalado. |Política |
|[Visualização]: Implantar o Dependency Agent para VMs do Windows |Implante o Dependency Agent para VMs do Windows se a Imagem de VM (sistema operacional) estiver definida na lista e o agente não estiver instalado. |Política |
|[Visualização]: Implantar o Agente do Log Analytics para VMs do Linux |Implante o Agente do Log Analytics para VMs do Linux se a Imagem de VM (sistema operacional) estiver definida na lista e o agente não estiver instalado. |Política |
|[Visualização]: Implantar o Agente do Log Analytics para VMs do Windows |Implante o Agente do Log Analytics para VMs do Windows se a Imagem de VM (sistema operacional) estiver definida na lista e o agente não estiver instalado. |Política |

### <a name="policies-for-azure-virtual-machine-scale-sets"></a>Conjuntos de dimensionamento de políticas para a máquina virtual do Azure

As definições de política para um conjunto de dimensionamento de máquina virtual do Azure estão listadas na tabela a seguir:

|NOME |Descrição |Type |
|-----|------------|-----|
|[Visualização]: Habilitar o Azure Monitor para conjuntos de dimensionamento VM (VMSS) |Habilite o Azure Monitor para os conjuntos de dimensionamento de máquinas virtuais no escopo especificado (grupo gerenciamento, assinatura ou grupo de recursos). Usa o espaço de trabalho do Log Analytics como parâmetro. Observação: se seu upgradePolicy do conjunto de dimensionamento é definido como Manual, você precisará aplicar a extensão para todas as VMs no conjunto ao chamar o upgrade nelas. Na CLI, isso seria az vmss update-instances. |Iniciativa |
|[Visualização]: Implantação do agente de dependência em VMSS – imagem (sistema operacional da VM) removido da lista de auditoria |Definido como não compatível se a imagem de VM (SO) não está definido na lista e o agente de dimensionamento de máquinas virtuais de relatórios não está instalado. |Política |
|[Visualização]: Implantação do agente do Log Analytics em VMSS – imagem (sistema operacional da VM) removido da lista de auditoria |Definido como não compatível se a imagem de VM (SO) não está definido na lista e o agente de dimensionamento de máquinas virtuais de relatórios não está instalado. |Política |
|[Visualização]: Implantar agente de dependência para conjuntos de dimensionamento de VM do Linux (VMSS) |Implante o agente de dependência para Linux, conjuntos de dimensionamento de máquina virtual se a imagem de VM (SO) é definido na lista e o agente não está instalado. |Política |
|[Visualização]: Implantar agente de dependência para conjuntos de dimensionamento VM do Windows (VMSS) |Implante a dependência de agente para Windows conjuntos de dimensionamento de máquina virtual se a imagem de VM (SO) é definido na lista e o agente não está instalado. |Política |
|[Visualização]: Implantar o Agente do Log Analytics no VMSS (Conjuntos de Dimensionamento de VMs) do Linux |Implante o agente do Log Analytics para Linux, conjuntos de dimensionamento de máquina virtual se a imagem de VM (SO) é definido na lista e o agente não está instalado. |Política |
|[Visualização]: Implantar o Agente do Log Analytics no VMSS (Conjuntos de Dimensionamento de VMs) do Windows |Implante o Log Analytics agente para Windows conjuntos de dimensionamento de máquina virtual se a imagem de VM (SO) é definido na lista e o agente não está instalado. |Política |

A política independente (não incluída com a iniciativa) é descrita aqui:

|NOME |Descrição |Type |
|-----|------------|-----|
|[Visualização]: Auditar o espaço de trabalho do Log Analytics para a VM – Relatar incompatibilidade |Relate as VMs como fora de conformidade se elas não estiverem fazendo logon no espaço de trabalho do Log Analytics especificado na atribuição de política/iniciativa. |Política |

### <a name="assign-the-azure-monitor-initiative"></a>Atribuir a iniciativa do Azure Monitor
Para criar a atribuição de política do Monitor para a página de cobertura de política de VMs do Azure, execute as etapas a seguir. Para entender como concluir essas etapas, confira  [Criar uma atribuição de política no portal do Azure](../../governance/policy/assign-policy-portal.md).

Quando você atribui a iniciativa/política, o escopo selecionado na atribuição pode ser o escopo listados aqui ou um subconjunto dela. Por exemplo, você pode ter criado atribuição para a assinatura (escopo de política) e não o grupo de gerenciamento (escopo de cobertura) na qual % de cobertura do caso indicaria o escopo de VMs na política (ou iniciativa) dividido por máquinas virtuais no escopo de cobertura. Em alguns casos, você pode ter excluída algumas VMs ou RGs ou assinatura do escopo da política.  Caso ele esteja em branco, indica que a política da / iniciativa não existe ou você não tem permissões (informações fornecidas no status de atribuição)  

1. Entre no [Portal do Azure](https://portal.azure.com).

2. No portal do Azure, selecione **Monitor**. 

3. Escolha **Máquinas Virtuais (versão prévia)** na seção **Insights**.
 
4. Selecione o **começar** guia e na página selecionar **Gerenciar política de cobertura**.

5. Selecione um grupo de gerenciamento ou a assinatura da tabela e selecione **escopo** clicando-se a elipse (...).      Em nosso exemplo, um escopo limita a atribuição de política a um agrupamento de máquinas virtuais para imposição.

6. Sobre o **atribuição de política do Azure** página é previamente preenchido com a iniciativa **habilitar o Azure Monitor para VMs**. 
    O **nome da atribuição** caixa é preenchida automaticamente com o nome da iniciativa, mas você pode alterá-lo. Adicione também uma descrição opcional. A caixa **Atribuído por** é populada automaticamente com base em quem está conectado e esse valor é opcional.

7. (Opcional) Para remover um ou mais recursos do escopo, selecione **Exclusões**.

8. Na lista suspensa **Espaço de Trabalho do Log Analytics** da região com suporte, selecione um espaço de trabalho.

   > [!NOTE]
   > Se o workspace estiver fora do escopo da atribuição, conceda as permissões *Colaborador do Log Analytics* à ID de Entidade de Segurança da atribuição de política. Se você não fizer isso, poderá ver uma falha de implantação, como: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` Para conceder acesso, examine [Como configurar manualmente a identidade gerenciada](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  A caixa de seleção **Identidade Gerenciada** está marcada, pois a iniciativa que está sendo atribuída inclui uma política com o efeito *deployIfNotExists*.
    
9. Na lista suspensa **Gerenciar localização de identidade**, selecione a região apropriada.

10. Selecione **Atribuir**.

Depois de criar a atribuição, o Azure Monitor para a página de cobertura de política de VMs atualizará a cobertura de atribuição, Status de atribuição, VMs de conformidade e estado de conformidade para refletir as alterações. 

A matriz a seguir mapeia cada estado de conformidade possíveis para a iniciativa.  

| Estado de Conformidade | Descrição | 
|------------------|-------------|
| **Em conformidade** | Todas as VMs no escopo têm os agentes de Log Analytics e dependência implantados para eles.|
| **Não está em conformidade** | Nem todas as VMs no escopo tem o Log Analytics e agentes de dependência implantada para eles e podem exigir a correção.|
| **Não foi iniciado** | Uma atribuição de nova foi adicionada. |
| **Bloqueio** | Você não tem privilégios suficientes para o grupo de gerenciamento. <sup>1</sup> | 
| **Em branco** | Nenhuma política foi atribuída. | 

<sup>1</sup> se você não tiver acesso ao grupo de gerenciamento, peça ao proprietário para fornecer acesso ou exibir a conformidade e gerenciar atribuições por meio de grupos de gerenciamento filhos ou assinaturas. 

A tabela a seguir mapeia cada status de atribuição possíveis para a iniciativa.

| Status da atribuição | Descrição | 
|------------------|-------------|
| **Êxito** | Todas as VMs no escopo têm os agentes de Log Analytics e dependência implantados para eles.|
| **Aviso** | A assinatura não está em um grupo de gerenciamento.|
| **Não foi iniciado** | Uma atribuição de nova foi adicionada. |
| **Bloqueio** | Você não tem privilégios suficientes para o grupo de gerenciamento. <sup>1</sup> | 
| **Em branco** | Nenhuma VM existe ou a política não atribuído. | 
| **Ação** | Atribuir política ou editar atribuição | 

<sup>1</sup> se você não tiver acesso ao grupo de gerenciamento, peça ao proprietário para fornecer acesso ou exibir a conformidade e gerenciar atribuições por meio de grupos de gerenciamento filhos ou assinaturas. 

## <a name="review-and-remediate-the-compliance-results"></a>Revisar e corrigir os resultados de conformidade

O exemplo a seguir é uma VM do Azure, mas também se aplica aos conjuntos de dimensionamento de máquina virtual. Você pode aprender como analisar os resultados de conformidade, lendo [identificar resultados de não conformidade](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). No Monitor do Azure para a página de cobertura de política de VMs, selecione um grupo de gerenciamento ou a assinatura da tabela e selecione **exibir a conformidade** clicando-se a elipse (...).   

![Conformidade com a política para máquinas virtuais do Azure](./media/vminsights-enable-at-scale-policy/policy-view-compliance-01.png)

Com base nos resultados das políticas incluídas com a iniciativa, as VMs são relatadas como fora de conformidade nos seguintes cenários:

* O Log Analytics ou o Dependency Agent não está implantado.  
    Esse cenário é típico para um escopo com as VMs existentes. Para atenuar isso, implante os agentes necessários [criando tarefas de correção](../../governance/policy/how-to/remediate-resources.md) em uma política de não conformidade.  
    - [Visualização]: Deploy Dependency Agent for Linux VMs
    - [Visualização]: Deploy Dependency Agent for Windows VMs
    - [Visualização]: Deploy Log Analytics Agent for Linux VMs
    - [Visualização]: Deploy Log Analytics Agent for Windows VMs

* A Imagem de VM (sistema operacional) não está identificada na definição de política.  
    Os critérios da política de implantação incluem apenas as VMs implantadas com base em imagens de VM conhecidas do Azure. Verifique a documentação para ver se o sistema operacional da VM é compatível. Caso ele não seja, duplique a política de implantação e atualize-a ou modifique-a para fazer com que a imagem fique em conformidade.  
    - [Visualização]: Auditar a Implantação do Dependency Agent – Imagem de VM (sistema operacional) não listada
    - [Visualização]: Auditar a Implantação do Agente do Log Analytics – Imagem de VM (sistema operacional) não listada

* As VMs não estão fazendo logon no espaço de trabalho do Log Analytics especificado.  
    É possível que algumas VMs no escopo da iniciativa estejam fazendo logon em um espaço de trabalho do Log Analytics diferente daquele especificado na atribuição de política. Essa política é uma ferramenta para identificar quais VMs estão relatando para um espaço de trabalho fora de conformidade.  
    - [Visualização]: Audit Log Analytics Workspace for VM - Report Mismatch

## <a name="edit-initiative-assignment"></a>Editar atribuição de iniciativa

A qualquer momento depois de atribuir uma iniciativa para um grupo de gerenciamento ou a assinatura, você pode editá-lo para modificar as propriedades a seguir:

- Nome da atribuição
- Descrição
- Atribuída por
- Espaço de trabalho do Log Analytics
- Exceções

## <a name="next-steps"></a>Próximas etapas

Agora que o monitoramento está habilitado para suas máquinas virtuais, essas informações estão disponíveis para análise com o Azure Monitor para VMs. Para saber como usar o recurso Integridade, confira [Exibir a Integridade do Azure Monitor para VMs](vminsights-health.md). Para exibir as dependências de aplicativos descobertas, confira [Exibir o Mapa do Azure Monitor para VMs](vminsights-maps.md). Para identificar gargalos e a utilização geral com o desempenho de VMs, consulte [Exibir o desempenho da VM do Azure](vminsights-performance.md), ou para exibir dependências de aplicativos descobertos, consulte [Exibir o Azure Monitor para mapa de VMs](vminsights-maps.md).
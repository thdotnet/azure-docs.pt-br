---
title: Habilitar o Azure Monitor para visão geral de máquinas virtuais (versão prévia) | Microsoft Docs
description: Este artigo descreve como implantar e configurar o Azure Monitor para VMs e os requisitos de sistema necessários.
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
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: 4eb07b29f68c5f495bdbe5e23f5b226480b66661
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524064"
---
# <a name="enable-azure-monitor-for-vms-preview-overview"></a>Habilitar o Azure Monitor para visão geral de máquinas virtuais (versão prévia)

Este artigo fornece uma visão geral das opções disponíveis para instalação do Azure Monitor para máquinas virtuais monitorar a integridade, desempenho e descobrir dependências de aplicativo em execução em máquinas virtuais do Azure e conjuntos de dimensionamento de máquinas virtuais, máquinas virtuais locais ou máquinas virtuais hospedadas no outro ambiente de nuvem.  

Habilite o Azure Monitor para VMs usando um dos seguintes métodos:

* Habilitar uma única máquina virtual do Azure ou o dimensionamento de máquinas virtuais, conjunto, selecionando **Insights (visualização)** diretamente do VM ou máquina virtual conjunto de dimensionamento.
* Habilitar dois ou mais VMs do Azure e máquinas virtuais conjuntos de dimensionamento usando a política do Azure. Usando esse método, as dependências necessárias de VMs novas e existentes e conjuntos de dimensionamento são instaladas e configuradas corretamente. Não compatível VMs e conjuntos de dimensionamento são relatados, para que você possa decidir se deseja habilitá-los e como você deseja corrigi-los.
* Habilite duas ou mais VMs do Azure ou conjuntos de dimensionamento de máquinas virtuais em uma assinatura ou um grupo de recursos especificado usando o PowerShell.
* Habilite para monitorar as máquinas virtuais ou computadores físicos, hospedados em sua rede corporativa ou de outro ambiente de nuvem.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, verifique se você compreende as informações descritas nas seções a seguir.

### <a name="log-analytics"></a>Log Analytics

O Azure Monitor para VMs oferece suporte a um espaço de trabalho do Log Analytics nas seguintes regiões:

- Centro-Oeste dos EUA
- Leste dos EUA
- Canada Central<sup>1</sup>
- Sul do Reino Unido<sup>1</sup>
- Europa Ocidental
- Sudeste Asiático<sup>1</sup>

<sup>1</sup> Atualmente, essa região não dá suporte ao recurso Integridade do Azure Monitor para VMs.

>[!NOTE]
>As máquinas virtuais do Azure podem ser implantadas de qualquer região e não são limitadas para as regiões com suporte para o espaço de trabalho do Log Analytics.
>

Caso não tenha um workspace, crie um com um dos seguintes métodos:
* [A CLI do Azure](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [O portal do Azure](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Se você estiver habilitando o monitoramento para uma única VM do Azure ou o dimensionamento de máquinas virtuais definido no portal do Azure, você pode criar um espaço de trabalho durante esse processo.

Para o cenário em escala usando a política do Azure, Azure PowerShell ou modelos do Azure Resource Manager, o seu espaço de trabalho do Log Analytics precisa primeiro os seguintes itens configurados:

* Instale as soluções ServiceMap e InfrastructureInsights. Você pode concluir a instalação usando um modelo do Azure Resource Manager que é fornecido ou usando o **configurar o espaço de trabalho** opção encontrado na **começar** guia.
* Configure o espaço de trabalho do Log Analytics para coletar contadores de desempenho.

Para configurar seu espaço de trabalho para o cenário em escala, você pode configurá-lo usando um dos seguintes métodos:

* [PowerShell do Azure](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace)
* Dos **configurar o espaço de trabalho** opção no Monitor do Azure para VMs [cobertura de política](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) página

### <a name="supported-operating-systems"></a>Sistemas operacionais com suporte

A tabela a seguir lista os sistemas operacionais Windows e Linux compatíveis com o Azure Monitor para VMs. Uma lista completa que fornece detalhes das versões principais e secundárias do kernel com suporte e de lançamento do sistema operacional Linux é fornecida mais adiante nesta seção.

|Versão do SO |Desempenho |Mapas |Integridade |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X | |
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| X |
|Ubuntu 14.04, 16.04, 18.04 | X | X | X |
|CentOS Linux 6, 7 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 11, 12 | X | X | X |
|Debian 8, 9.4 | X<sup>1</sup> | | X |

<sup>1</sup> O recurso Desempenho do Azure Monitor para VMs está disponível somente no Azure Monitor. Ele não está disponível quando você o acessa diretamente no painel esquerdo da VM do Azure.

>[!NOTE]
>As informações a seguir aplicam-se ao suporte do sistema operacional Linux:
> - Somente as versões de kernel padrão e Linux SMP têm suporte.
> - Não há suporte para nenhuma distribuição do Linux em versões de kernel não padrão, como PAE (Extensão do Endereço Físico) e Xen. Por exemplo, não há suporte para um sistema com a cadeia de caracteres de versão *2.6.16.21-0.8-xen*.
> - Não há suporte para kernels personalizados, incluindo recompilações de kernels padrão.
> - Há suporte para o kernel CentOSPlus.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Versão do SO | Versão do kernel |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Versão do SO | Versão do kernel |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

### <a name="centosplus"></a>CentOSPlus
| Versão do SO | Versão do kernel |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| Versão do SO | Versão do kernel |
|:--|:--|
| Ubuntu 18.04 | kernel 4.15.* |
| Ubuntu 16.04.3 | kernel 4.15.* |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="suse-linux-11-enterprise-server"></a>SUSE Linux 11 Enterprise Server

| Versão do SO | Versão do kernel
|:--|:--|
|11 SP4 | 3.0.* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Versão do SO | Versão do kernel
|:--|:--|
|12 SP2 | 4.4.* |
|12 SP3 | 4.4.* |

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency Agent

O recurso Mapa do Azure Monitor para VMs obtém seus dados do Microsoft Dependency Agent. O Dependency Agent depende do agente do Log Analytics para suas conexões com o Log Analytics. Portanto, o sistema precisa ter o agente do Log Analytics instalado e configurado com o Dependency Agent.

Se você habilitar o Azure Monitor para VMs para uma VM individual do Azure ou usar o método de implantação em escala, você precisará usar a extensão Dependency Agent da VM do Azure para instalar o agente como parte da experiência.

Em um ambiente híbrido, você pode baixar e instalar o agente de dependência em qualquer uma das duas maneiras: manualmente ou usando um método de implantação automatizado para máquinas virtuais que são hospedados fora do Azure.

A tabela a seguir descreve as fontes conectadas para as quais o recurso Mapa dá suporte em um ambiente híbrido.

| Fonte conectada | Suportado | Descrição |
|:--|:--|:--|
| Agentes do Windows | Sim | Além do [Agente do Log Analytics](../../azure-monitor/platform/log-analytics-agent.md), os agentes do Windows exigem o Microsoft Dependency Agent. Para obter uma lista completa das versões de sistema operacional, confira os [sistemas operacionais compatíveis](#supported-operating-systems). |
| Agentes do Linux | Sim | Além do [Agente do Log Analytics](../../azure-monitor/platform/log-analytics-agent.md), os agentes do Linux exigem o Microsoft Dependency Agent. Para obter uma lista completa das versões de sistema operacional, confira os [sistemas operacionais compatíveis](#supported-operating-systems). |
| Grupo de gerenciamento do System Center Operations Manager | Não  | |

O Dependency Agent pode ser baixado nos seguintes locais:

| Arquivo | SO | Versão | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) |  Windows | 9.8.1 | 622C99924385CBF539988D759BCFDC9146BB157E7D577C997CDD2674E27E08DD |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.8.1 | 3037934A5D3FB7911D5840A9744AE9F980F87F620A7F7B407F05E276FE7AE4A8 |

## <a name="role-based-access-control"></a>Controle de acesso baseado em função

Para habilitar e acessar os recursos do Azure Monitor para VMs, você precisará receber as seguintes funções de acesso:

- Para habilitá-lo, você deve ter o *Colaborador do Log Analytics* função.

- Para exibir os dados de desempenho, integridade e mapa, você precisará ter a função *Leitor de Monitoramento* na VM do Azure. O espaço de trabalho do Log Analytics precisa ser configurado para o Azure Monitor para VMs.

Para obter mais informações sobre como controlar o acesso a um espaço de trabalho do Log Analytics, veja [Gerenciar espaços de trabalho](../../azure-monitor/platform/manage-access.md).

## <a name="how-to-enable-azure-monitor-for-vms-preview"></a>Como habilitar o Azure Monitor para máquinas virtuais (versão prévia)

Habilitar o Azure Monitor para VMs usando um dos métodos a seguir, descritos na tabela a seguir.

| Estado de implantação | Método | Descrição |
|------------------|--------|-------------|
| Conjunto de dimensionamento de VM do Azure ou máquina virtual único | [Diretamente da VM](vminsights-enable-single-vm.md) | Você pode habilitar uma única máquina virtual do Azure, selecionando **Insights (visualização)** diretamente do VM ou máquina virtual conjunto de dimensionamento. |
| Várias VMs do Azure ou conjuntos de dimensionamento de máquina virtual | [Azure Policy](vminsights-enable-at-scale-policy.md) | Você pode habilitar várias VMs do Azure usando a política do Azure e definições de política disponíveis. |
| Várias VMs do Azure ou conjuntos de dimensionamento de máquina virtual | [Modelos do Azure PowerShell ou Azure Resource Manager](vminsights-enable-at-scale-powershell.md) | Você pode habilitar vários conjuntos de dimensionamento de VMs do Azure ou máquina virtual em um grupo de recursos ou assinatura especificado usando modelos do Azure PowerShell ou do Azure Resource Manager. |
| Nuvem híbrida | [Habilitar para o ambiente híbrido](vminsights-enable-hybrid-cloud.md) | Você pode implantar máquinas virtuais ou computadores físicos que são hospedados em seu datacenter ou em outros ambientes de nuvem. |

## <a name="performance-counters-enabled"></a>Contadores de desempenho habilitados

O Azure Monitor para VMs configura um espaço de trabalho do Log Analytics para coletar os contadores de desempenho que ele usa. A tabela a seguir lista os objetos e contadores que são coletados a cada 60 segundos.

### <a name="windows-performance-counters"></a>Contadores de desempenho do Windows

|Nome do objeto |Nome do contador |
|------------|-------------|
|LogicalDisk |% de Espaço Livre |
|LogicalDisk |Média de segundos/Leitura do Disco |
|LogicalDisk |Média de segundos/Transferência do Disco |
|LogicalDisk |Média de segundos/Gravação do Disco |
|LogicalDisk |Bytes de Disco/s |
|LogicalDisk |Bytes Lidos no Disco/s  |
|LogicalDisk |Leituras de Disco/s  |
|LogicalDisk |Transferências de Disco/s |
|LogicalDisk | Bytes Gravados no Disco/s |
|LogicalDisk | Gravações de Disco/s |
|LogicalDisk |Megabytes Livres |
|Memória |MBytes Disponíveis |
|Adaptador de rede |Bytes Recebidos/s |
|Adaptador de rede |Bytes Enviados/s |
|Processador |% Tempo do Processador |

### <a name="linux-performance-counters"></a>Contadores de desempenho do Linux

|Nome do objeto |Nome do contador |
|------------|-------------|
|Disco Lógico |% de Espaço Usado |
|Disco Lógico |Bytes Lidos no Disco/s  |
|Disco Lógico |Leituras de Disco/s  |
|Disco Lógico |Transferências de Disco/s |
|Disco Lógico | Bytes Gravados no Disco/s |
|Disco Lógico | Gravações de Disco/s |
|Disco Lógico |Megabytes Livres |
|Disco Lógico |Bytes de Disco Lógico/s |
|Memória |MBytes de Memória Disponíveis |
|Rede |Total de Bytes Recebidos |
|Rede |Total de Bytes Transmitidos |
|Processador |% Tempo do Processador |

## <a name="diagnostic-and-usage-data"></a>Dados de uso e de diagnóstico

A Microsoft coleta automaticamente dados de uso e de desempenho por meio do uso do serviço Azure Monitor. A Microsoft utiliza esses dados para fornecer e aprimorar a qualidade, a segurança e a integridade do serviço. Para fornecer funcionalidades precisas e eficientes de solução de problemas, os dados do recurso Mapa incluem informações sobre a configuração de seu software, como sistema operacional e versão, endereço IP, nome DNS e nome da estação de trabalho. A Microsoft não coleta nomes, endereços nem outras informações de contato.

Para obter mais informações sobre a coleta e uso de dados, consulte a [Política de privacidade do Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Próximas etapas

Agora que o monitoramento está habilitado para a máquina virtual, essas informações ficarão disponíveis para análise com o Azure Monitor para VMs. Para saber como usar o recurso Integridade, confira [Exibir a Integridade do Azure Monitor para VMs](vminsights-health.md). Para exibir as dependências de aplicativos descobertas, confira [Exibir o Mapa do Azure Monitor para VMs](vminsights-maps.md).

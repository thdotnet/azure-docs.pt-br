---
title: Habilitar o Azure Monitor (versão prévia) para um ambiente híbrido | Microsoft Docs
description: Este artigo descreve como habilitar os Azure Monitor para as VMs para um ambiente de nuvem híbrida que contém uma ou mais máquinas virtuais.
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
ms.date: 06/07/2019
ms.author: magoedte
ms.openlocfilehash: bc26cc0654aac9416bf31ffccf426648e3a8b8d2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122567"
---
# <a name="enable-azure-monitor-for-vms-preview-for-a-hybrid-environment"></a>Habilitar o Azure Monitor para máquinas virtuais (versão prévia) para um ambiente híbrido

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Este artigo explica como habilitar o Azure Monitor para máquinas virtuais (versão prévia) para máquinas virtuais ou computadores físicos, hospedados no datacenter ou em outro ambiente de nuvem. No final deste processo, você será com êxito começaram a suas máquinas virtuais em seu ambiente de monitoramento e saber se estão tendo problemas de desempenho ou disponibilidade. 

Antes de começar, certifique-se de examinar a [pré-requisitos](vminsights-enable-overview.md) e verificar se sua assinatura e seus recursos atendem aos requisitos. Examine os requisitos e os métodos de implantação do [agente do Linux e do Windows do Log Analytics](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

>[!NOTE]
>O Dependency Agent de Mapa do Azure Monitor para VMs não transmite dados por conta própria e não exige alterações em firewalls ou portas. Os dados do mapa sempre são transmitidos pelo agente do Log Analytics para o serviço do Azure Monitor, diretamente ou por meio de [gateway do Operations Management Suite](../../azure-monitor/platform/gateway.md) se suas políticas de segurança não permitem que computadores da rede Conecte-se à internet.

As etapas para concluir essa tarefa são resumidas da seguinte maneira:

1. Instale o agente do Log Analytics para Windows ou Linux. Antes de instalar o agente, examine os [visão geral do agente do Log Analytics](../platform/log-analytics-agent.md) artigo para compreender os pré-requisitos de sistema e métodos de implantação.

2. Baixar e instalar o Dependency Agent de Mapa do Azure Monitor para VMs para [Windows](https://aka.ms/dependencyagentwindows) ou [Linux](https://aka.ms/dependencyagentlinux).

3. Habilitar a coleta de contadores de desempenho.

4. Implantar o Azure Monitor para VMs.

## <a name="install-the-dependency-agent-on-windows"></a>Instalar o Dependency Agent no Windows
Instale o Dependency Agent manualmente em computadores Windows executando `InstallDependencyAgent-Windows.exe`. Se você executar esse arquivo executável sem opções, ele iniciará um assistente de instalação que você poderá seguir para instalar o agente de forma interativa.

>[!NOTE]
>São necessários privilégios de *Administrador* para instalar ou desinstalar o agente.

A tabela a seguir destaca os parâmetros compatíveis com a instalação do agente por meio da linha de comando.

| Parâmetro | DESCRIÇÃO |
|:--|:--|
| /? | Retorna uma lista das opções de linha de comando. |
| /S | Realiza uma instalação silenciosa sem a interação do usuário. |

Por exemplo, para executar o programa de instalação com o `/?` parâmetro, insira **InstallDependencyAgent-Windows.exe /?** .

Os arquivos do Dependency Agent do Windows são instalados em *C:\Arquivos de Programas\Microsoft Dependency Agent* por padrão. Se o agente de dependência não for iniciado após a conclusão da instalação, verifique os logs para obter informações de erro detalhadas. O diretório de log é *%Programfiles%\Microsoft Dependency Agent\logs*.

## <a name="install-the-dependency-agent-on-linux"></a>Instale o Agente de Dependência no Linux
O Dependency Agent é instalado em servidores Linux por meio de *InstallDependencyAgent-Linux64.bin*, um script de shell com um binário de extração automática. Você pode executar o arquivo usando `sh` ou adicionar permissões de execução ao próprio arquivo.

>[!NOTE]
> O acesso root é necessário para instalar ou configurar o agente.
>

| Parâmetro | DESCRIÇÃO |
|:--|:--|
| -help | Obtenha uma lista das opções de linha de comando. |
| -s | Realize uma instalação silenciosa sem solicitações ao usuário. |
| --check | Verifica as permissões e o sistema operacional, mas não instala o agente. |

Por exemplo, para executar o programa de instalação com o `-help` parâmetro, insira **InstallDependencyAgent-Linux64.bin-ajuda**.

Instale o agente de dependência do Linux como raiz, executando o comando `sh InstallDependencyAgent-Linux64.bin`.

Se o Agente de Dependência não for iniciado, verifique os logs para obter informações de erro detalhadas. Em agentes do Linux, o diretório de log é */var/opt/microsoft/dependency-agent/log*.

Os arquivos do Agente de Dependência são colocados nos diretórios a seguir:

| Arquivos | Local padrão |
|:--|:--|
| Arquivos de núcleo | /opt/microsoft/dependency-agent |
| Arquivos de log | /var/opt/microsoft/dependency-agent/log |
| Arquivos de configuração | /etc/opt/microsoft/dependency-agent/config |
| Arquivos executáveis de serviço | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Arquivos de armazenamento binário | /var/opt/microsoft/dependency-agent/storage |

## <a name="enable-performance-counters"></a>Habilitar contadores de desempenho
Se o espaço de trabalho do Log Analytics referenciado pela solução ainda não estiver configurado para coletar os contadores de desempenho necessários para a solução, você precisará habilitá-los. Você pode fazer isso de duas maneiras:
* Manualmente, conforme descrito em [Fontes de dados de desempenho do Windows e do Linux no Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Baixando e executando um script do PowerShell que está disponível a partir de [da Galeria do PowerShell do Azure](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

## <a name="deploy-azure-monitor-for-vms"></a>Implantar o Azure Monitor para VMs
Esse método inclui um modelo JSON que especifica a configuração para habilitar os componentes da solução no espaço de trabalho do Log Analytics.

Se você não souber como implantar recursos usando um modelo, consulte:
* [Implantar recursos com modelos do Resource Manager e o Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Implantar recursos com modelos do Resource Manager e a CLI do Azure](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Para usar a CLI do Azure, primeiro você precisa instalar e usar a CLI localmente. Você deve estar executando a CLI do Azure versão 2.0.27 ou posterior. Para identificar sua versão, execute `az --version`. Para instalar ou atualizar a CLI do Azure, consulte [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="create-and-execute-a-template"></a>Criar e executar um modelo

1. Copie e cole a seguinte sintaxe JSON em seu arquivo:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                            "promotionCode": ""
                        }
                    },
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },
                        "plan": {
                            "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Salve esse arquivo como *installsolutionsforvminsights.json* em uma pasta local.

1. Capture os valores de *WorkspaceName*, *ResourceGroupName* e *WorkspaceLocation*. O valor para *WorkspaceName* é o nome do seu espaço de trabalho do Log Analytics. O valor de *WorkspaceLocation* é a região na qual o workspace foi definido.

1. Você está pronto para implantar esse modelo usando o seguinte comando do PowerShell:

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    A alteração de configuração pode levar alguns minutos para ser concluído. Quando ele for concluído, uma mensagem é exibida que é semelhante à seguinte e inclui o resultado:

    ```powershell
    provisioningState       : Succeeded
    ```
   Depois de habilitar o monitoramento, poderão ser necessários cerca de 10 minutos antes da exibição do estado de integridade e das métricas para o computador híbrido.

## <a name="next-steps"></a>Próximas etapas

Agora que o monitoramento está habilitado para suas máquinas virtuais, essas informações estão disponíveis para análise com o Azure Monitor para VMs.
 
- Para saber como usar o recurso de integridade, consulte [modo de exibição do Azure Monitor de integridade de VMs](vminsights-health.md).
- Para exibir as dependências de aplicativos descobertas, confira [Exibir o Mapa do Azure Monitor para VMs](vminsights-maps.md).
- Para identificar gargalos e a utilização geral do desempenho da sua VM, consulte [desempenho da VM do Azure de modo de exibição](vminsights-performance.md).
- Para exibir as dependências de aplicativos descobertas, confira [Exibir o Mapa do Azure Monitor para VMs](vminsights-maps.md).
---
title: Como integrar o esquema de alerta comum com aplicativos lógicos
description: Saiba como criar um aplicativo lógico que utiliza o esquema de alerta comum para lidar com todos os alertas.
author: ananthradhakrishnan
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/27/2019
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: b51b9f08819a4c496e051d375f6d52aaa985c8e6
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394125"
---
# <a name="how-to-integrate-the-common-alert-schema-with-logic-apps"></a>Como integrar o esquema de alerta comum com aplicativos lógicos

Este artigo mostra como criar um aplicativo lógico que utiliza o esquema de alerta comum para lidar com todos os alertas.

## <a name="overview"></a>Visão geral

O [esquema de alerta comum](https://aka.ms/commonAlertSchemaDocs) fornece um esquema JSON padronizado e extensível em todos os diferentes tipos de alerta. O esquema de alerta comum é mais útil quando aproveitados por meio de programação – por meio de webhooks, runbooks e os aplicativos lógicos. Neste artigo, demonstraremos como um único aplicativo lógico pode ser criado para lidar com todos os alertas. Os mesmos princípios podem ser aplicados a outros métodos de programação. O aplicativo lógico descrito neste artigo cria variáveis bem definidas para o [campos 'essenciais'](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#essentials-fields)e também descreve como você pode manipular [tipo de alerta]('https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields') lógica específica.


## <a name="prerequisites"></a>Pré-requisitos 

Este artigo pressupõe que o leitor esteja familiarizado com o 
* Configurando regras de alerta ([métrica](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric), [log](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log), [log de atividades](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log))
* Configurando [grupos de ação](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* Habilitando a [esquema de alerta comum](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema#how-do-i-enable-the-common-alert-schema) de dentro de grupos de ação

## <a name="create-a-logic-app-leveraging-the-common-alert-schema"></a>Criar um aplicativo lógico aproveitando o esquema de alerta comuns

1. Siga as [as etapas descritas para criar seu aplicativo lógico](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups-logic-app). 

1.  Selecionar o gatilho: **Quando uma solicitação HTTP for recebida**.

    ![Gatilhos do Aplicativo Lógico](media/action-groups-logic-app/logic-app-triggers.png "Gatilhos do Aplicativo Lógico")

1.  Selecione **Editar** para alterar o gatilho de solicitação HTTP.

    ![HTTP solicita gatilhos](media/action-groups-logic-app/http-request-trigger-shape.png "HTTP solicita gastilhos")


1.  Copie e cole o seguinte esquema:

    ```json
        {
            "type": "object",
            "properties": {
                "schemaId": {
                    "type": "string"
                },
                "data": {
                    "type": "object",
                    "properties": {
                        "essentials": {
                            "type": "object",
                            "properties": {
                                "alertId": {
                                    "type": "string"
                                },
                                "alertRule": {
                                    "type": "string"
                                },
                                "severity": {
                                    "type": "string"
                                },
                                "signalType": {
                                    "type": "string"
                                },
                                "monitorCondition": {
                                    "type": "string"
                                },
                                "monitoringService": {
                                    "type": "string"
                                },
                                "alertTargetIDs": {
                                    "type": "array",
                                    "items": {
                                        "type": "string"
                                    }
                                },
                                "originAlertId": {
                                    "type": "string"
                                },
                                "firedDateTime": {
                                    "type": "string"
                                },
                                "resolvedDateTime": {
                                    "type": "string"
                                },
                                "description": {
                                    "type": "string"
                                },
                                "essentialsVersion": {
                                    "type": "string"
                                },
                                "alertContextVersion": {
                                    "type": "string"
                                }
                            }
                        },
                        "alertContext": {
                            "type": "object",
                            "properties": {}
                        }
                    }
                }
            }
        }
    ```

1. Selecione **+** **Nova Etapa** e, em seguida, **Adicione uma ação**.

    ![Adicione uma ação](media/action-groups-logic-app/add-action.png "Adicione uma ação")

1. Nesse estágio, você pode adicionar uma variedade de conectores (Microsoft Teams, Slack, Salesforce, etc.) com base em suas necessidades específicas. Você pode usar o 'campos essenciais' out-of-the-box. 

    ![Campos essenciais](media/alerts-common-schema-integrations/logic-app-essential-fields.png "campos essenciais")
    
    Como alternativa, você pode criar lógica condicional com base no tipo de alerta usando a opção 'Expression'.

    ![Expressão de aplicativo lógico](media/alerts-common-schema-integrations/logic-app-expressions.png "expressão de aplicativo lógico")
    
     O [campo 'monitoringService']('https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields') permite identificar exclusivamente o tipo de alerta com base no qual você pode criar a lógica condicional.

    
    Por exemplo, o abaixo de trecho de código verifica se o alerta é um alerta de log do Application Insights com base e, nesse caso, imprime os resultados da pesquisa. Caso contrário, ele imprime "NA".

    ```text
      if(equals(triggerBody()?['data']?['essentials']?['monitoringService'],'Application Insights'),triggerBody()?['data']?['alertContext']?['SearchResults'],'NA')
    ```
    
     Saiba mais sobre [escrevendo expressões de aplicativo de lógica](https://docs.microsoft.com/azure/logic-apps/workflow-definition-language-functions-reference#logical-comparison-functions).

    


## <a name="next-steps"></a>Próximas etapas

* [Saiba mais sobre grupos de ação](../../azure-monitor/platform/action-groups.md).
* [Saiba mais sobre o esquema de alerta comuns](https://aka.ms/commonAlertSchemaDocs).


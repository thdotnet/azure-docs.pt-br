---
title: Criar, exibir e gerenciar alertas do log de atividades no Azure Monitor
description: Crie alertas do log de atividade usando o portal do Azure, um modelo do Azure Resource Manager e do PowerShell do Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: vinagara
ms.openlocfilehash: 71e61228fcdbd52abedbc1f1205baa60b1aea923
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705291"
---
# <a name="create-view-and-manage-activity-log-alerts-by-using-azure-monitor"></a>Criar, exibir e gerenciar alertas do log de atividades usando o Azure Monitor  

## <a name="overview"></a>Visão geral
Alertas do log de atividades são os alertas ativados quando ocorre um novo evento de log de atividades que corresponde às condições especificadas no alerta.

Esses alertas são para recursos do Azure e podem ser criados usando um modelo do Azure Resource Manager. Eles também podem ser criados, atualizados ou excluídos no portal do Azure. Normalmente, você cria atividades alertas do log para receber notificações quando ocorrerem alterações específicas a recursos em sua assinatura do Azure. Alertas geralmente estão no escopo para recursos ou grupos de recurso específico. Por exemplo, você talvez queira ser notificado quando qualquer máquina virtual no grupo de recursos de amostra **myProductionResourceGroup** é excluído. Ou, você talvez queira ser notificado se quaisquer funções novas forem atribuídas a um usuário em sua assinatura.

> [!IMPORTANT]
> Não não possível criar alertas de notificação de integridade do serviço por meio da interface para criação de alerta de log de atividade. Para saber mais sobre como criar e usar as notificações de integridade do serviço, consulte [receber alertas de log de atividades em notificações de integridade do serviço](alerts-activity-log-service-notifications.md).

Quando você cria regras de alerta, verifique o seguinte:

- A assinatura no escopo não é diferente da assinatura em que o alerta foi criado.
- Os critérios devem ser o nível, status, chamador, ID de recurso, grupo de recursos ou categoria de evento de tipo de recurso no qual o alerta é configurado.
- Não há nenhuma condição "anyOf" ou condições aninhadas na configuração de alerta JSON. Basicamente, apenas uma condição de "allOf" é permitida com nenhuma adicional "allOf" ou "anyOf" condições.
- Quando a categoria for "administrativa", você deve especificar pelo menos um dos critérios anteriores no seu alerta. Você não pode criar um alerta que seja ativado sempre que um evento for criado nos logs de atividades.


## <a name="azure-portal"></a>Portal do Azure

Você pode usar o portal do Azure para criar e modificar regras de alerta de log de atividade. A experiência é integrada com um log de atividades do Azure para garantir que a criação contínua de alerta para eventos específicos de interesse.

### <a name="create-with-the-azure-portal"></a>Criar com o portal do Azure

Use o procedimento a seguir.

1. No portal do Azure, selecione **Monitor** > **alertas**.
2. Selecione **nova regra de alerta** no canto superior esquerdo do **alertas** janela.

     ![Nova regra de alerta](media/alerts-activity-log/AlertsPreviewOption.png)

     A janela **Criar regra** é exibida.

      ![novas opções de regra de alerta](media/alerts-activity-log/create-new-alert-rule-options.png)

3. Sob **definir condição de alerta**, forneça as seguintes informações e selecione **feito**:

   - **Destino do alerta:** Para exibir e selecione o destino do novo alerta, use **filtrar por assinatura** / **filtrar por tipo de recurso**. Na lista exibida, selecione o recurso ou grupo de recursos.

     > [!NOTE]
     > 
     > Você pode selecionar um recurso, grupo de recursos ou uma assinatura inteira para um sinal de log de atividade.

     **Exibição de exemplo do destino de alerta**

     ![Selecionar o destino](media/alerts-activity-log/select-target.png)

   - Sob **critérios de destino**, selecione **adicionar critérios**. Todos os sinais disponíveis para o destino são exibidos, que incluem aqueles de várias categorias de **Log de atividades**. O nome da categoria é acrescentado para o **serviço do Monitor** nome.

   - Selecione o sinal na lista exibida com as várias operações possíveis para o tipo **Log de Atividades**.

     É possível selecionar a linha do tempo do histórico de log e a lógica de alerta correspondente para esse sinal de destino:

     **Tela Adicionar critérios**

     ![Adicionar critérios](media/alerts-activity-log/add-criteria.png)

     - **Tempo de histórico**: Eventos disponíveis para a operação selecionada podem ser plotados nas últimas 6, 12 ou 24 horas ou na última semana.

     - **Lógica de Alerta**:

       - **Nível de evento**: O nível de severidade do evento: Detalhado, _Informativo_, _Aviso_, _Erro_ ou _Crítico_.
       - **Status**: O status do evento: Iniciado, _Com falha_ ou _Bem-sucedido_.
       - **Evento iniciado por**: Também conhecido como o chamador. O endereço de email ou o identificador do Azure Active Directory do usuário que realizou a operação.

       Este gráfico de exemplo de sinal tem a lógica de alerta aplicada:

       ![critérios selecionados](media/alerts-activity-log/criteria-selected.png)

4. Sob **definir detalhes do alerta**, forneça os seguintes detalhes:

    - **Nome da regra de alerta**: O nome da nova regra de alerta.
    - **Descrição**: A descrição para a nova regra de alerta.
    - **Salvar alerta para o grupo de recursos**: Selecione o grupo de recursos onde você deseja salvar essa nova regra.

5. Em **Grupo de ações**, no menu suspenso, especifique o grupo de ações que deseja atribuir à nova regra de alerta. Ou, [criar um novo grupo de ação](../../azure-monitor/platform/action-groups.md) e atribuí-lo para a nova regra. Para criar um novo grupo, selecione **+ novo grupo**.

6. Para habilitar as regras após criá-los, selecione **Yes** para o **Habilitar regra após a criação** opção.
7. Selecione **criar regra de alerta**.

    A nova regra de alerta para o log de atividades é criada e uma mensagem de confirmação é exibida no canto superior direito da janela.

    É possível habilitar, desabilitar, editar ou excluir uma regra. Saiba mais sobre como gerenciar regras de log de atividades.


Uma analogia simple para condições de Noções básicas sobre em que as regras de alerta podem ser criadas em um log de atividades é para explorar ou filtrar eventos por meio de [log de atividades no portal do Azure](activity-log-view.md#azure-portal). No **do Azure Monitor - log de atividades** tela, você pode filtrar ou localizar o evento necessário e, em seguida, criar um alerta usando o **adicionar alerta do log de atividades** botão. Em seguida, siga as etapas 4 a 7 como anteriormente mostradas.
    
 ![Adicionar alerta do log de atividades](media/alerts-activity-log/add-activity-log.png)
    

### <a name="view-and-manage-in-the-azure-portal"></a>Exibir e gerenciar no portal do Azure

1. No portal do Azure, selecione **Monitor** > **alertas**. Selecione **gerenciar regras de alerta** no canto superior esquerdo da janela.

    ![Gerenciar regras de alerta](media/alerts-activity-log/manage-alert-rules.png)

    A lista das regras disponíveis é exibida.

2. Pesquise a regra de log de atividades modificar.

    ![Pesquisar regras de alerta de log de atividade](media/alerts-activity-log/searth-activity-log-rule-to-edit.png)

    Você pode usar os filtros disponíveis, _assinatura_, _grupo de recursos_, _recurso_, _tipo de sinal_, ou _Status_ , para localizar a regra de atividade que você deseja editar.

   > [!NOTE]
   > 
   > Você pode editar apenas **descrição**, **critérios de destino**, e **grupos de ação**.

3. Selecione a regra e clique duas vezes para editar as opções de regra. Faça as alterações necessárias e, em seguida, selecione **salvar**.

   ![Gerenciar regras de alerta](media/alerts-activity-log/activity-log-rule-edit-page.png)

4. Você pode habilitar, desabilitar ou excluir uma regra. Selecione a opção apropriada na parte superior da janela depois de selecionar a regra conforme descrito na etapa 2.


## <a name="azure-resource-manager-template"></a>Modelo do Azure Resource Manager
Para criar um alerta do log de atividades usando um modelo do Azure Resource Manager, você cria um recurso do tipo `microsoft.insights/activityLogAlerts`. Em seguida, você preencherá todas as propriedades relacionadas. Aqui está um modelo que cria um alerta do log de atividades:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
O exemplo anterior JSON pode ser salvas como, por exemplo, sampleActivityLogAlert.json para fins deste passo a passo e pode ser implantada por meio [do Azure Resource Manager no portal do Azure](../../azure-resource-manager/resource-group-template-deploy-portal.md).

> [!NOTE]
> Pode levar até 5 minutos para a regra de alerta de log novo do atividades se torne ativa.

## <a name="rest-api"></a>API REST 
O [API de alertas do Log de atividade do Azure Monitor](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) é uma API REST. Ele é totalmente compatível com a API de REST do Azure Resource Manager. Ele pode ser usado por meio do PowerShell usando o cmdlet do Gerenciador de recursos ou a CLI do Azure.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="deploy-the-resource-manager-template-with-powershell"></a>Implantar o modelo do Resource Manager com o PowerShell
Para usar o PowerShell para implantar o modelo do Resource Manager de exemplo mostrado anteriormente na [modelo do Resource Manager](#azure-resource-manager-template) seção, use o seguinte comando:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```

onde o sampleActivityLogAlert.parameters.json contém os valores fornecidos para os parâmetros necessários para a criação de regra de alerta.

### <a name="use-activity-log-powershell-cmdlets"></a>Usar cmdlets do PowerShell do log de atividades

Os alertas do log de atividades têm cmdlets do PowerShell dedicados disponíveis:

- [Set-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Set-AzActivityLogAlert): Cria um novo alerta de log de atividade ou atualiza um alerta do log de atividades existente.
- [Get-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Get-AzActivityLogAlert): Obtém a atividade de um ou mais recursos de alerta do log.
- [Enable-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Enable-AzActivityLogAlert): Permite que um alerta do log de atividades existente e define suas marcas.
- [Disable-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Disable-AzActivityLogAlert): Desabilita um alerta do log de atividades existente e define suas marcas.
- [Remove-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Remove-AzActivityLogAlert): Remove um alerta do log de atividades.

## <a name="azure-cli"></a>CLI do Azure

Os comandos da CLI do Azure dedicados sob o conjunto [az monitor activity-log alert](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert) estão disponíveis para gerenciar regras de alerta do log de atividades.

Para criar uma regra de alerta de log de atividades de novo, use os comandos a seguir nesta ordem:

1. [az monitor activity-log alert create](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-create): Crie um novo recurso de regra de alerta do log de atividade.
1. [az monitor activity-log alert scope](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/scope): Adicione escopo para a regra de alerta de log de atividade criado.
1. [az monitor activity-log alert action-group](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/action-group): Adicione um grupo de ação para a regra de alerta de log de atividades.

Para recuperar um recurso de regra de alerta de log de atividades, use o comando da CLI do Azure [Mostrar alertas do log de atividades az monitor](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-show
). Para exibir todos os recursos de regra de alerta de log de atividade em um grupo de recursos, use [lista de alertas de log de atividades do az monitor](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list).
Recursos de regra de alerta do log de atividade podem ser removidos usando o comando de CLI do Azure [exclusão do alerta de log de atividades do monitor az](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-delete).

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre [esquema de webhook para os logs de atividade](../../azure-monitor/platform/activity-log-alerts-webhook.md).
- Leia uma [visão geral dos logs de atividade](../../azure-monitor/platform/activity-log-alerts.md).
- Saiba mais sobre [grupos de ação](../../azure-monitor/platform/action-groups.md).  
- Saiba mais sobre as [notificações de integridade do serviço](../../azure-monitor/platform/service-notifications.md).

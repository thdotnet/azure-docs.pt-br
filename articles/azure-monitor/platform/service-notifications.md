---
title: Exibir as notificações de integridade do serviço usando o Portal do Azure
description: As notificações de integridade do serviço permitem exibir mensagens de integridade do serviço publicadas pelo Microsoft Azure.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2018
ms.author: dukek
ms.subservice: logs
ms.openlocfilehash: 8cf8c3eb86f55b5595ef9a1af83ea50580bf638b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069347"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Exibir as notificações de integridade do serviço usando o Portal do Azure

Notificações de integridade do serviço são publicadas pela infraestrutura do Azure. Eles contêm informações sobre os recursos em sua assinatura. Essas notificações são uma subclasse de atividade de log de eventos e isso também podem ser encontradas no log de atividades. As notificações de integridade do serviço podem ser informativas ou acionáveis, dependendo da classe.

Para obter mais informações sobre as várias classes de notificações de integridade do serviço, consulte [as propriedades de notificações de integridade do serviço](../../service-health/service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Exibindo as notificações de integridade do serviço no Portal do Azure

1. No [portal do Azure](https://portal.azure.com), selecione **Monitor**.

    ![Captura de tela do menu do Portal do Azure, com a opção Monitor selecionada](./media/service-notifications/home-monitor.png)

    Este Azure Monitor reúne todas as suas configurações e dados de monitoramento em uma exibição consolidada. Ela abre primeiro na seção **Log de atividades** .

1. Selecione **Alertas**.

    ![Captura do log de atividades do Monitor, com a opção Alertas selecionada](./media/service-notifications/service-health-summary.png)

1. Selecione **+Adicionar alerta do log de atividades** e configure um alerta para garantir que você receba notificações de serviço futuras. Para obter mais informações, consulte [Criar alertas do log de atividades em notificações de serviço](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Próximas etapas

* Receber [notificações de alerta sempre que uma notificação de integridade do serviço](../../azure-monitor/platform/alerts-activity-log-service-notifications.md) é postada.  
* Saiba mais sobre os [alertas do log de atividades](../../azure-monitor/platform/activity-log-alerts.md).

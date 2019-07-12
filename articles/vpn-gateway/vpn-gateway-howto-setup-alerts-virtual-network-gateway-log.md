---
title: Configurar alertas em eventos de log de diagnóstico do Gateway de VPN do Azure
description: Etapas para configurar alertas em eventos de log de diagnóstico do Gateway de VPN
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: alzam
ms.openlocfilehash: 4f18581b9ca5770b89be8ca37529c09d635dfb25
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607108"
---
# <a name="set-up-alerts-on-diagnostic-log-events-from-vpn-gateway"></a>Configurar alertas em eventos de log de diagnóstico do Gateway de VPN

Este artigo ajuda você a configurar alertas com base em eventos de log de diagnóstico do Gateway de VPN do Azure usando o Azure Log Analytics. 

Os logs a seguir estão disponíveis no Azure:

|***Nome*** | ***Descrição*** |
|---        | ---               |
|GatewayDiagnosticLog | Contém os logs de diagnóstico para eventos de configuração de gateway, principais alterações e eventos de manutenção |
|TunnelDiagnosticLog | Contém eventos de alteração de estado do túnel. Túnel conectar/desconectar eventos tem um motivo resumido para que a alteração de estado, se aplicável |
|RouteDiagnosticLog | Logs de alterações em rotas estáticas e eventos BGP que ocorrem no gateway |
|IKEDiagnosticLog | Registra mensagens de controle de IKE e eventos no gateway |
|P2SDiagnosticLog | Registra mensagens de controle de ponto a site e eventos no gateway |

## <a name="setup"></a>Configurar alertas

As etapas de exemplo a seguir criará um alerta para um evento de desconexão que envolve um túnel VPN site a site:


1. No portal do Azure, pesquise **do Log Analytics** sob **todos os serviços** e selecione **espaços de trabalho do Log Analytics**.

   ![Seleções para ir para os espaços de trabalho do Log Analytics](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "criar")

2. Selecione **Create** sobre o **Log Analytics** página.

   ![Página de análise de log com o botão Criar](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "selecione")

3. Selecione **criar novo** e preencha os detalhes.

   ![Detalhes para criar um espaço de trabalho do Log Analytics](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "selecione")

4. Encontre seu gateway de VPN na **Monitor** > **as configurações de diagnóstico** folha.

   ![Seleções para localizar o gateway de VPN em configurações de diagnóstico](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "selecione")

5. Para ativar o diagnóstico, clique duas vezes o gateway e, em seguida, selecione **ativar o diagnóstico**.

   ![Seleções para ativar o diagnóstico](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "selecione")

6. Preencha os detalhes e certifique-se de que **enviar para Log Analytics** e **TunnelDiagnosticLog** estão selecionados. Escolha o espaço de trabalho de análise de Log que você criou na etapa 3.

   ![As caixas de seleção](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "selecione")

7. Vá para a visão geral do recurso de gateway de rede virtual e selecione **alertas** da **monitoramento** guia. Em seguida, crie uma nova regra de alerta ou editar uma regra de alerta existente.

   ![Seleções para criar uma nova regra de alerta](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "selecione")

   ![Point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "selecione")
8. Selecione o espaço de trabalho do Log Analytics e o recurso.

   ![Seleções de espaço de trabalho e os recursos](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "selecione")

9. Selecione **pesquisa de logs personalizada** como a lógica de sinal sob **Adicionar condição**.

   ![Seleções para uma pesquisa de log personalizado](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "selecione")

10. Insira a consulta a seguir na caixa de texto **Consulta de pesquisa**. Substitua os valores português (Brasil), conforme apropriado.

     `AzureDiagnostics |
     where Category  == "TunnelDiagnosticLog" and ResourceId == toupper("<RESOURCEID OF GATEWAY>") and TimeGenerated > ago(5m) and
     remoteIP_s == "<REMOTE IP OF TUNNEL>" and status_s == "Disconnected"`

    Defina o valor de limite como 0 e selecione **feito**.

    ![Inserindo uma consulta e selecionando um limite](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "selecione")

11. Sobre o **Create rule** página, selecione **criar novo** sob o **grupos de ação** seção. Preencha os detalhes e selecione **Okey**.

    ![Detalhes para um novo grupo de ação](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "selecione")

12. No **Create rule** da página, preenchimento nos detalhes do **Personalizar ações** e certifique-se de que o nome correto aparece na **nome do grupo de ação** seção. Selecione **criar regra de alerta** para criar a regra.

    ![Seleções para criar uma regra](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "selecione")

## <a name="next-steps"></a>Próximas etapas

Para configurar alertas em métricas de túnel, consulte [configurar alertas em métricas do Gateway de VPN](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).

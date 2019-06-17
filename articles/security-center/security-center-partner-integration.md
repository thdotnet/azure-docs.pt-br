---
title: Integrar soluções de segurança na Central de Segurança do Azure | Microsoft Docs
description: Saiba como a Central de Segurança do Azure se integra aos parceiros para aprimorar a segurança geral dos recursos do Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: d94567800a9fd020784c9cb07b2c6824cd032509
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064286"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integrar soluções de segurança na Central de Segurança do Azure
Este documento ajuda você a gerenciar soluções de segurança já conectadas à Central de Segurança do Azure e a adicionar novas.

> [!NOTE]
> Um subconjunto de soluções de segurança será desativado em 31 de julho de 2019. Para obter mais informações e serviços alternativos, consulte [recursos de desativação da Central de segurança (julho de 2019)](security-center-features-retirement-july2019.md#menu_solutions).

## <a name="integrated-azure-security-solutions"></a>Soluções de segurança integradas do Azure
A Central de Segurança facilita a criação de soluções de segurança integradas no Azure. Os benefícios incluem:

- **Implantação simplificada**: A Central de Segurança oferece provisionamento simplificado das soluções integradas de parceiros. Para as soluções como antimalware e avaliação de vulnerabilidades, a Central de Segurança pode provisionar o agente necessário em suas máquinas virtuais e para dispositivos de firewall, a Central de Segurança pode cuidar de grande parte da configuração de rede necessária.
- **Detecções integradas**: Os eventos de segurança das soluções de parceiro são automaticamente coletados, agregados e exibidos como parte dos incidentes e alertas da Central de Segurança. Esses eventos também são combinados com detecções de outras fontes para fornecer funcionalidades de detecção avançada de ameaças.
- **Monitoramento e gerenciamento de integridade unificados**: Os clientes podem usar eventos de integridade integrados para monitorar todas as soluções de parceiros em um relance. O gerenciamento básico está disponível com acesso fácil à configuração avançada usando a solução de parceiro.

Atualmente, soluções de segurança integradas incluem a avaliação de vulnerabilidade por [Qualys](https://www.qualys.com/public-clouds/microsoft-azure/) e [Rapid7](https://www.rapid7.com/products/insightvm/) e firewall do aplicativo Web de Gateway de aplicativo do Microsoft.

> [!NOTE]
> A Central de Segurança não instala o Microsoft Monitoring Agent em soluções de virtualização de parceiro porque a maioria dos fornecedores de segurança proíbe agentes externos em execução em suas soluções.
>
>

## <a name="how-security-solutions-are-integrated"></a>Como as soluções de segurança são integradas
As soluções de segurança do Azure implantadas da Central de Segurança serão conectadas automaticamente. Você também pode se conectar a outras fontes de dados de segurança, incluindo computadores que executam no local ou em outras nuvens.

![Integração de soluções de parceiros](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Gerenciar soluções de segurança integrada do Azure e outras fontes de dados

1. Entre no [Portal do Azure](https://azure.microsoft.com/features/azure-portal/).

2. No menu **Microsoft Azure**, selecione **Central de Segurança**. **Central de Segurança - Visão geral** é aberto.

3. No menu da Central de Segurança, selecione **Soluções de segurança**.

   ![Visão geral da Central de Segurança](./media/security-center-partner-integration/overview.png)

Em **Soluções de segurança**, você pode exibir informações sobre a integridade da solução integrada de segurança do Azure e executar tarefas básicas de gerenciamento. Você também pode conectar a outros tipos de fontes de dados de segurança, como logs de firewall e alertas do Azure Active Directory Identity Protection no CEF (Formato de Evento Comum).

### <a name="connected-solutions"></a>Soluções conectadas

A seção **Soluções conectadas** inclui soluções de segurança atualmente conectadas à Central de Segurança e informações sobre o status de integridade de cada solução.  

![Soluções conectadas](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

O status de uma solução de parceiro pode ser:

* Íntegro (verde) - não há qualquer problema de integridade.
* Não íntegro (vermelho) - há um problema de integridade que requer atenção imediata
* Problemas de integridade (laranja) - a solução interrompeu o envio de relatórios sobre sua integridade.
* Não relatado (cinza) - a solução não relatou nada ainda. O status da solução pode não ser relatado se ela tiver sido conectada recentemente e se ainda estiver sendo implantada, ou se não houver nenhum dado de integridade disponível.

> [!NOTE]
> Se os dados de status de integridade não estiverem disponíveis, a Central de Segurança mostrará a data e a hora do último evento recebido para indicar se a solução está reportando ou não. Se não houver dados de integridade disponíveis e se nenhum alerta for tiver sido recebido nos últimos 14 dias, a Central de Segurança indica que a solução não está íntegra ou não está reportando.
>
>

1. Selecione **EXIBIR** para obter informações e opções adicionais, o que inclui:

   - **Console da solução**. Abre a experiência de gerenciamento para esta solução.
   - **Vincular VM**. Abre a folha Vincular Aplicativos. Nela, você pode conectar recursos à solução de parceiro.
   - **Excluir solução**.
   - **Configurar**.

   ![Detalhes da solução de parceiro](./media/security-center-partner-solutions/partner-solutions-detail.png)

### <a name="discovered-solutions"></a>Soluções descobertas

A Central de Segurança detecta automaticamente as soluções de segurança em execução no Azure, mas não conectadas à Central de Segurança, e exibe as soluções na seção **Soluções descobertas**. Isso inclui soluções do Azure, como o [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), bem como soluções de parceiros.

> [!NOTE]
> A camada Padrão da Central de Segurança é necessária no nível de assinatura para o recurso de descoberta de soluções. Confira os [Preços](security-center-pricing.md) para saber mais sobre os tipos de preço de Segurança.
>
>

Selecione **CONNECT** em uma solução para integrá-la à Central de Segurança e ser notificado sobre alertas de segurança.

![Soluções descobertas](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

A Central de Segurança também detecta soluções implantadas na assinatura que podem encaminhar logs em CEF (formato de eventos comuns). Saiba como [conectar a uma solução de segurança](quick-security-solutions.md) que usa os logs em CEF à Central de Segurança.

### <a name="add-data-sources"></a>Adicionar fontes de dados

A seção **Adicionar fontes de dados** inclui outras fontes de dados disponíveis que podem ser conectadas. Para obter instruções sobre como adicionar dados de qualquer uma dessas fontes, clique em **ADICIONAR**.

![Fontes de dados](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)

## <a name="exporting-data-to-a-siem"></a>Exportar dados para um SIEM

Eventos processados produzidos pela Central de Segurança do Azure são publicados no [log de atividades](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) do Azure, um log de tipos disponíveis por meio do Azure Monitor. O Azure Monitor oferece um pipeline consolidado para qualquer um dos seus dados de monitoramentos de roteamento para uma ferramenta do SIEM. Isso é feito transmitindo dados para um Hub de eventos onde ele pode então ser extraído para uma ferramenta de parceiro.

Este pipe usa o [monitoramento do Azure único pipeline](../azure-monitor/platform/stream-monitoring-data-event-hubs.md) para obter acesso aos dados de monitoramento de seu ambiente do Azure. Isso permite que você configure facilmente SIEMs e ferramentas de monitoramento para consumir os dados.

As seções a seguir descrevem como você pode configurar os dados para serem transmitidos para um hub de eventos. As etapas pressupõem que você já tem a Central de Segurança do Azure configurado na sua assinatura do Azure.

Visão geral de alto nível

![Visão geral de alto nível](media/security-center-export-data-to-siem/overview.png)

### <a name="what-is-the-azure-security-data-exposed-to-siem"></a>Quais são os dados de segurança do Azure expostos para SIEM?

Nesta versão, expomos os [alertas de segurança.](../security-center/security-center-managing-and-responding-alerts.md) Em versões futuras, enriqueceremos o conjunto de dados com as recomendações de segurança.

### <a name="how-to-setup-the-pipeline"></a>Como configurar o pipeline

#### <a name="create-an-event-hub"></a>Criar um Hub de Evento

Antes de começar, você precisa [criar um namespace dos Hubs de Eventos](../event-hubs/event-hubs-create.md). Esse namespace e Hub de Eventos é o destino para todos os seus dados de monitoramento.

#### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Transmissão do Log de Atividades do Azure para os Hubs de Eventos

Consulte o seguinte artigo [log de atividades de fluxo para Hubs de Eventos](../azure-monitor/platform/activity-logs-stream-event-hubs.md)

#### <a name="install-a-partner-siem-connector"></a>Instalar um conector do SIEM parceiro 

Rotear dados de monitoramento para um Hub de Eventos com o Azure Monitor permite fácil integração com as ferramentas de monitoramento e o SIEM de parceiro.

Consulte o link a seguir para ver a lista de [SIEMs de suporte](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)

### <a name="example-for-querying-data"></a>Exemplo de consulta de dados 

Eis algumas consultas Splunk que você pode usar para extrair dados de alerta:

| **Descrição da consulta:** | **Consulta** |
|----|----|
| Todos os Alertas| index=main Microsoft.Security/locations/alerts|
| Resumir contagem de operações por seu nome| index=main sourcetype="amal:security" \| tabela operationName \| estatísticas contam por operationName|
| Obter informações de alertas: Hora, Nome, Estado, ID e Assinatura | index=main Microsoft.Security/locations/alerts \| table \_time, properties.eventName, State, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Próximas etapas

Neste artigo, você aprendeu a integrar as soluções de parceiro à Central de Segurança. Para saber mais sobre a Central de Segurança, confira estes artigos:

* [Monitoramento da integridade de segurança na Central de Segurança](security-center-monitoring.md). Saiba como monitorar a integridade dos recursos do Azure.
* [Perguntas frequentes sobre a Central de Segurança do Azure](security-center-faq.md). Obtenha respostas para perguntas frequentes sobre como usar a Central de Segurança.
* [Blog de Segurança do Azure](https://blogs.msdn.com/b/azuresecurity/). Encontre postagens no blog sobre a conformidade e segurança do Azure.

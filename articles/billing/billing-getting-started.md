---
title: Evitar custos inesperados e gerenciar a cobrança no Azure
description: Saiba como evitar encargos inesperados em sua conta do Azure. Use recursos de gerenciamento e de acompanhamento de custos para uma assinatura do Azure.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: b64e84c3fff27675029ff35f27972a4aca014ec3
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "68612023"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Evite cobranças inesperadas com o gerenciamento de custo e a cobrança do Azure

Ao se inscrever no Azure, há várias coisas que podem ser feitas para você ter uma ideia melhor de seus gastos:

- A [calculadora de preços](https://azure.microsoft.com/pricing/calculator/) pode fornecer uma estimativa de custos antes de você criar um recurso do Azure. 

- O [portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) fornece uma previsão e a análise de custos atual de sua assinatura. 

- Se desejar agrupar e entender os custos de diferentes projetos ou equipes, examine a [marcação de recursos](../azure-resource-manager/resource-group-using-tags.md). Se sua organização tiver um sistema de relatórios que você prefira usar, confira as [APIs de cobrança](billing-usage-rate-card-overview.md).

- Se a assinatura tiver sido criada com base em um EA (Contrato Enterprise), será possível exibir seus custos no portal do Azure. Se a assinatura for feita por meio de um CSP (Provedor de Soluções na Nuvem) ou pelo Azure Sponsorship, alguns dos recursos a seguir poderão não se aplicar ao seu caso. Para obter mais informações, confira [Recursos adicionais do EA, CSP e Sponsorship](#other-offers).

- Se a assinatura for uma oferta de avaliação gratuita, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), AIO (Azure via Open) ou BizSpark, ela será desabilitada automaticamente quando todos os créditos forem usados. Saiba mais sobre os [limites de gastos](#spending-limit) para evitar que sua assinatura desabilitada de forma inesperada.

- Se tiver se inscrito em uma [conta gratuita do Azure](https://azure.microsoft.com/free/), [poderá usar alguns dos serviços do Azure mais populares gratuitamente por 12 meses](billing-create-free-services-included-free-account.md). Juntamente com as recomendações listadas abaixo, consulte [Evitar ser cobrado na conta gratuita](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Obter custos estimados antes de adicionar serviços do Azure

Veja algumas informações adicionais sobre estimar custos usando as seguintes ferramentas:
- Calculadora de preços do Azure
- Portal do Azure
- Limite de gastos

As imagens nas seções a seguir mostram os preços de exemplo em dólares americanos.

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Estimar custo online usando a calculadora de preços

Confira a [calculadora de preços](https://azure.microsoft.com/pricing/calculator/) e para obter uma estimativa do custo mensal do serviço em que você está interessado. Você pode adicionar qualquer recurso interno do Azure para obter um custo estimado. Na calculadora de preços, você pode alterar o tipo de moeda.

![Captura de tela do menu de calculadora de preços](./media/billing-getting-started/pricing-calc.png)

Por exemplo, na calculadora de preços, uma VM (Máquina Virtual) do Windows A1 terá estimativa de custar um determinado valor/mês em horas de computação se você deixá-la em execução o tempo todo:

![Captura de tela da calculadora de preços mostrando um custo estimado de VM Windows A1 por mês](./media/billing-getting-started/pricing-calcvm.png)

Para obter mais informações sobre preços, confira as [Perguntas frequentes sobre Preços](https://azure.microsoft.com/pricing/faq/). Se desejar conversar com um vendedor do Azure, ligue para o número de telefone mostrado na parte superior da página de Perguntas frequentes.

### <a name="review-estimated-costs-in-the-azure-portal"></a>Examinar os custos estimados no portal do Azure

Normalmente, quando você adiciona um serviço no portal do Azure, há um modo de exibição que mostra um custo estimado por mês na moeda cobrada. Por exemplo, quando você escolhe o tamanho de sua VM do Windows, vê o custo mensal estimado para as horas de computação:

![Exemplo: uma VM Windows A1 mostrando o custo estimado por mês](./media/billing-getting-started/vm-size-cost.png)

### <a name="spending-limit"></a>Verifique se você tem um limite de gastos ativado

Se você tiver uma assinatura que usa crédito, o limite de gastos é ativado para você por padrão. Dessa forma, quando você gasta todo o seu crédito, não há cobrança a mais no seu cartão de crédito. Consulte a [lista completa de ofertas do Azure e a disponibilidade do limite de gastos](https://azure.microsoft.com/support/legal/offer-details/).

No entanto, quando você atingir o limite de gastos, os serviços serão desabilitados. Isso significa que suas VMs serão desalocadas. Para evitar o tempo de inatividade do serviço, você deve desabilitar o limite de gastos. O excedente é cobrado em seu cartão de crédito cadastrado.

Para ver se você já tem um limite de gastos ativado, vá para o [modo de exibição Assinaturas no Centro de Contas](https://account.windowsazure.com/Subscriptions). Uma faixa será exibida se o limite de gastos estiver ativado, semelhante ao seguinte:

![Captura de tela que mostra um aviso sobre a ativação do limite de gastos no Centro de Contas](./media/billing-getting-started/spending-limit-banner.png)

Clique na faixa e siga os prompts para remover o limite de gastos. Se você não inseriu informações de cartão de crédito na inscrição, deverá inseri-las para remover o limite de gastos. Para saber mais, confira [Limite de gastos do Azure – Como funciona e como habilitá-lo ou removê-lo](https://azure.microsoft.com/pricing/spending-limits/).

## <a name="use-budgets-and-cost-alerts"></a>Usar orçamentos e alertas de custo

É possível criar [orçamentos](../cost-management/tutorial-acm-create-budgets.md) para gerenciar custos e criar [alertas](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) que notificam automaticamente os stakeholders de anomalias de gastos e risco de gastos em excesso. Os alertas são baseados nos gastos comparados com os limites de orçamento e de custo.

## <a name="monitor-costs-when-using-azure-services"></a>Monitorar custos ao usar os serviços do Azure
É possível monitorar custos com as seguintes ferramentas:

- Marcas
- Detalhamento de custos e taxa de gravação
- Análise de custo

### <a name="tags"></a> Adicionar marcas aos recursos para agrupar os dados de cobrança

Você pode usar marcas para agrupar os dados de cobrança no caso de serviços com suporte. Por exemplo, se você executa várias VMs para equipes diferentes, pode usar marcas para categorizar os custos por centro de custo (por exemplo: RH, marketing, finanças) ou ambiente (pré-produção, produção, teste).

![Captura de tela que mostra a configuração de marcas no portal](./media/billing-getting-started/tags.png)

As marcas aparecem em vários modos de exibição de relatório diferentes. Por exemplo, elas estão visíveis no [modo de exibição de análise de custo](#costs) imediatamente e no CSV de uso detalhado após o primeiro período de cobrança.

Para obter mais informações, veja [Usando marcas para organizar os recursos do Azure](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a> Monitorar detalhamento de custos e taxa de gravação

Depois que seus serviços do Azure estiverem em execução, verifique regularmente os encargos. É possível ver o gasto atual e a taxa de gravação no portal do Azure.

1. Acesse [Assinaturas no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) e selecione uma assinatura.

2. Se houver suporte para a sua assinatura, você verá o detalhamento de custos e a taxa de gravação.

    ![Captura de tela da taxa de gravação e do detalhamento no portal do Azure](./media/billing-getting-started/burn-rate.PNG)

3. Clique em [Análise de custo](../cost-management/quick-acm-cost-analysis.md) na lista à esquerda para ver o detalhamento de custos por recurso. Após adicionar um serviço, aguarde 24 horas para que os dados sejam exibidos.

    ![Captura de tela do modo de exibição de análise de custo no portal do Azure](./media/billing-getting-started/cost-analysis.png)

4. Você pode filtrar por propriedades diferentes, como [marcas](#tags), grupo de recursos e período de tempo. Clique em **Aplicar** para confirmar os filtros e em **Baixar** se desejar exportar o modo de exibição para um arquivo .csv (valores separados por vírgulas).

5. Além disso, clique em um recurso para ver seu histórico de gastos diário e quanto o recurso custa por dia.

    ![Captura de tela da exibição do histórico de gastos no portal do Azure](./media/billing-getting-started/costhistory.png)

Compare os custos que você vê com as estimativas que você viu quando selecionou os serviços. Se os custos forem significativamente diferentes das estimativas, confira o plano de preços que você selecionou para seus recursos.

## <a name="optimize-and-reduce-costs"></a>Otimizar e reduzir custos
Se você não estiver familiarizado com os princípios de gerenciamento de custos, leia [Como otimizar seu investimento em nuvem com o Gerenciamento de Custos do Azure](../cost-management/cost-mgt-best-practices.md).

No portal do Azure, também é possível otimizar e reduzir custos do Azure com desligamento automático para VMs e recomendações do Assistente.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>Considere recursos de redução de custos, como o desligamento automático para VMs

Dependendo do cenário, você pode configurar o desligamento automático para suas VMs no portal do Azure. Para saber mais, confira [Desligamento automático de máquinas virtuais usando o Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Captura de tela da opção de desligamento automático no portal](./media/billing-getting-started/auto-shutdown.png)

O desligamento automático não é igual a desligar a VM com as opções de energia padrão. O desligamento automático interrompe e desaloca suas VMs para parar as cobranças adicionais. Para saber mais, confira os estados de VM nas Perguntas frequentes sobre preços de [VMs Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) e [VMs Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).

Para obter mais recursos de redução de custos para os ambientes de desenvolvimento e teste, confira [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Ativar e examinar recomendações do Assistente do Azure

[O Assistente do Azure](../advisor/advisor-overview.md) ajuda a reduzir os custos identificando recursos com baixa utilização. Visite o assistente no portal do Azure:

![Captura de tela do botão Azure Advisor no portal do Azure](./media/billing-getting-started/advisor-button.png)

É possível obter recomendações acionáveis na guia **Custo** no painel do Assistente:

![Captura de tela de exemplo de recomendação de custo do Advisor](./media/billing-getting-started/advisor-action.png)

Examine o tutorial [Otimizar custos de recomendações](../cost-management/tutorial-acm-opt-recommendations.md) para obter um tutorial guiado sobre recomendações do Assistente para economia de custos.

## <a name="review-costs-against-your-latest-invoice"></a>Examinar custos em sua última fatura

No fim do período de cobrança, sua fatura mais recente estará disponível. Também é possível [baixar faturas e arquivos de detalhes de uso](billing-download-azure-invoice-daily-usage-date.md) para verificar se você foi cobrado corretamente. Para obter mais informações sobre como comparar seu uso diário com sua fatura, consulte [Entenda sua fatura do Microsoft Azure](billing-understand-your-bill.md).

### <a name="billing-api"></a>API de Cobrança

Use a API de cobrança do Azure para obter programaticamente dados de uso. Use a API RateCard junto com a API de Uso para obter seu uso cobrado. Para saber mais, confira [Obtenha informações sobre o consumo de recursos do Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Casos especiais e recursos adicionais

### <a name="ea-csp-and-sponsorship-customers"></a>Clientes do EA, CSP e Sponsorship
Fale com seu gerente de conta ou parceiro do Azure para começar.

| Oferta | Recursos |
|-------------------------------|-----------------------------------------------------------------------------------|
| EA (Enterprise Agreement) | [Portal EA](https://ea.azure.com/), [documentos de ajuda](https://ea.azure.com/helpdocs) e [relatório do Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Programa do CSP (Provedor de Soluções na Nuvem) | Fale com seu provedor |
| Azure Sponsorship | [Portal do Sponsorship](https://www.microsoftazuresponsorships.com/) |

Se estiver gerenciando TI em uma grande organização, recomendamos a leitura de [Azure enterprise scaffold](/azure/architecture/cloud-adoption-guide/subscription-governance) e [enterprise IT white paper](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (baixe o .pdf, somente em inglês).

#### <a name="EA"></a> Visualizações de custo do Contrato Enterprise no portal do Azure

As visualizações de custo Enterprise estão, no momento, em Visualização Pública. Itens a serem observados:

- Os custos de assinatura são baseados no uso e não incluem valores pré-pagos, excedentes, quantidades incluídas, ajustes e impostos. Os encargos reais são calculados no nível de registro.
- Os valores mostrados no portal do Azure podem ser diferentes daqueles no Enterprise Portal. Atualizações no Enterprise Portal podem levar alguns minutos antes de serem mostradas no portal do Azure.
- Se você não está vendo os custos, isso pode ser por um dos seguintes motivos:
    - Você não tem permissões no nível da assinatura. Para ver as exibições de custo empresarial, você precisa ser um Leitor de Cobrança, Leitor, Colaborador ou Proprietário no nível da assinatura.
    - Você é um Proprietário de Conta e seu Administrador de registro desabilitou a configuração “Encargos de exibição do AO”.  Contate o Administrador de registro para obter acesso aos custos.
    - Você é um administrador de departamento e seu administrador de registro desabilitou a configuração **Encargos de exibição do DA**.  Entre em contato com o Administrador de registro para obter acesso.
    - Você comprou o Azure por meio de um parceiro de canal e o parceiro não liberou informações sobre preços.  
- Se você atualizar as configurações relacionadas ao custo de acesso no Enterprise Portal, haverá um atraso de alguns minutos antes que as alterações sejam mostradas no portal do Azure.
- Limite de gastos e diretrizes de fatura não se aplicam às assinaturas do Contrato Enterprise.

### <a name="check-your-subscription-and-access"></a>Verificar assinatura e acesso

Para exibir os custos, você deve ter [acesso no nível das assinaturas a informações de cobrança](billing-manage-access.md). Somente o Administrador da Conta pode acessar o [Centro de Contas](https://account.azure.com/Subscriptions), alterar informações de cobrança e gerenciar assinaturas. O Administrador da Conta é a pessoa que passou pelo processo de inscrição. Para gerenciar essas funções, consulte [Adicionar ou alterar as funções de administrador do Azure que gerenciam a assinatura ou serviços](billing-add-change-azure-subscription-administrator.md).

Para ver se você é o administrador da Conta, acesse [Assinaturas no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Exiba a lista de assinaturas e encontre **Minha função**. Se *Administrador da Conta* for a função, você terá privilégios completos. Se estiver indicado outra coisa, como *Proprietário*, você não terá todos os privilégios.

![Captura de tela de sua função no modo de exibição Assinaturas no portal do Azure](./media/billing-getting-started/sub-blade-view.PNG)

Para gerenciar assinaturas e alterar informações de cobrança, [localize o Administrador da Conta](billing-subscription-transfer.md#whoisaa). Peça ao Administrador da Conta para concluir as tarefas ou [transferir a assinatura para você](billing-subscription-transfer.md).

Se o administrador da sua conta não estiver mais na sua organização e você precisar gerenciar o faturamento, [entre em contato conosco](https://go.microsoft.com/fwlink/?linkid=2083458).


### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>Solicitar um crédito do Contrato de Nível de Serviço para um incidente de serviço

O SLA (contrato de nível de serviço) descreve os compromissos da Microsoft com relação ao tempo de atividade e à conectividade. Um incidente de serviço é relatado quando os serviços do Azure enfrentam um problema que afeta o tempo de atividade ou a conectividade, geralmente denominado *interrupção*. Se não alcançarmos e mantivermos os Níveis de Serviço para cada serviço, conforme descrito no SLA, você poderá ser qualificado para um crédito para uma parte de seus valores de serviço mensais.

Para solicitar um crédito:

1. Entre no [portal do Azure](https://portal.azure.com/). Se você tiver várias contas, use a que foi afetada pelo tempo de inatividade do Azure. 
2. Crie uma solicitação de suporte.
3. Em **Tipo de problema**, selecione **Cobrança**.
4. Em **Tipo de problema**, selecione **Solicitação de Reembolso**.
5. Adicione detalhes para especificar que você está solicitando um crédito SLA, mencione a data/hora/fuso horário, além dos serviços afetados (VMs, sites, etc.)
6. Verifique seus detalhes de contato e selecione **Criar** para enviar sua solicitação.

Os limites de SLA variam por serviço. Por exemplo, a Camada da Web do SQL tem um SLA de 99,9%, as VMs têm um SLA de 99,95% e a Camada Padrão do SQL tem um SLA de 99,99%.

Para alguns serviços, há pré-requisitos para o SLA aplicar. Por exemplo, as máquinas virtuais devem ter duas ou mais instâncias implantadas no mesmo conjunto de disponibilidade.

Para obter mais informações, confira [Contratos de Nível de Serviço](https://azure.microsoft.com/support/legal/sla/) e a documentação [Resumo do SLA para serviços do Azure](https://azure.microsoft.com/support/legal/sla/summary/).

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Caso tenha dúvidas ou precise de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre como usar [limites de gastos](billing-spending-limit.md) para impedir o gasto excessivo.
- Comece [analisando os custos do Azure](../cost-management/quick-acm-cost-analysis.md).

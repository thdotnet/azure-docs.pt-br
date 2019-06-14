---
title: Escala e hospedagem no Azure Functions | Microsoft Docs
description: Saiba como escolher entre o plano de consumo do Azure Functions e o plano Premium.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, funções, plano de consumo, plano premium, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 03/27/2019
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3253cc7e379ae63880d533f14bc76e7af5a4425a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050567"
---
# <a name="azure-functions-scale-and-hosting"></a>Escala e hospedagem no Azure Functions

Quando você cria um aplicativo de funções no Azure, você deve escolher um plano de hospedagem para seu aplicativo. Existem três planos de hospedagem disponíveis para o Azure Functions: [Plano de consumo](#consumption-plan), [plano Premium](#premium-plan), e [plano de serviço de aplicativo](#app-service-plan).

O plano de hospedagem escolhido determina os seguintes comportamentos:

* Como seu aplicativo de funções é dimensionado.
* Os recursos disponíveis para cada instância do aplicativo de função.
* Suporte para recursos avançados, como conectividade de rede virtual.

Planos de consumo e Premium adicionam automaticamente o poder de computação quando seu código está em execução. Seu aplicativo é escalado horizontalmente quando necessário para manipular a carga e reduzido verticalmente quando código será interrompido. Para o plano de consumo, você também não precisa pagar por VMs ociosas ou reservar capacidade com antecedência.  

O plano Premium fornece recursos adicionais, como conectividade de rede virtual, a capacidade de manter instâncias passiva indefinidamente e instâncias de computação premium.

Plano de serviço de aplicativo permite que você aproveite a infraestrutura dedicada, o que você gerencia. Seu aplicativo de função não é dimensionada com base em eventos, que significa que nunca é escalas para zero. (Requer que [Always on](#always-on) está habilitada.)

> [!NOTE]
> Você pode alternar entre planos de consumo e Premium, alterando a propriedade de plano de recurso de aplicativo de funções.

## <a name="hosting-plan-support"></a>Suporte à hospedagem de plano

Suporte ao recurso se enquadra em duas categorias a seguir:

* _Disponibilidade geral (GA)_ : totalmente compatível e aprovados para uso em produção.
* _Visualização_: ainda não foram totalmente suportados e aprovados para uso em produção.

A tabela a seguir indica o nível atual de suporte para os três planos de hospedagem, quando em execução no Windows ou Linux:

| | Plano de consumo | Plano Premium | Plano dedicado |
|-|:----------------:|:------------:|:----------------:|
| Windows | GA | preview | GA |
| Linux | preview | N/D | GA |

## <a name="consumption-plan"></a>Plano de consumo

Quando você estiver usando o plano de consumo, instâncias do host do Azure Functions dinamicamente são adicionadas e removidas com base no número de eventos de entrada. Esse plano sem servidor escala automaticamente, e você é cobrado pelos recursos de computação apenas durante a execução de suas funções. Em um plano de consumo, a execução de uma função expire após um período configurável.

A cobrança baseia-se no número de execuções, no tempo de execução e na memória usada. A cobrança é agregada entre todas as funções em um aplicativo de funções. Para saber mais, confira a [página de preços do Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

O plano de Consumo é o plano de hospedagem padrão e oferece os seguintes benefícios:

* Pague apenas quando suas funções forem executadas
* Escale horizontalmente de forma automática, mesmo durante períodos de carga alta

Aplicativos de funções na mesma região podem ser atribuídos para o mesmo plano de consumo. Não há nenhuma desvantagem ou o impacto de ter vários aplicativos em execução no mesmo plano de consumo. Atribuir vários aplicativos para o mesmo plano de consumo não tem nenhum impacto na resiliência, escalabilidade e confiabilidade de cada aplicativo.

## <a name="premium-plan"></a>Plano Premium (versão prévia)

Quando você estiver usando o plano Premium, instâncias do host do Azure Functions são adicionadas e removidas com base no número de eventos de entrada assim como o plano de consumo.  O plano Premium suporta os seguintes recursos:

* Instâncias perpetuamente passivos para evitar qualquer inicialização a frio
* Conectividade de VNet
* Duração da execução ilimitado
* Tamanhos de instância Premium (um núcleo, dois núcleos e quatro instâncias de núcleo)
* Preços mais previsível
* Alocação de alta densidade do aplicativo para os planos com vários aplicativos de funções

Informações sobre como você pode configurar essas opções podem ser encontradas na [documentos de plano premium do Azure Functions](functions-premium-plan.md).

Em vez de cobrança por execução e a memória consumida, a cobrança para o plano Premium é baseada no número de núcleos por segundo, o tempo de execução e a memória usada em instâncias reservadas e necessárias.  Pelo menos uma instância deve ser at passiva todos os momentos. Isso significa que há um custo mensal fixo por plano ativo, independentemente do número de execuções.

Considere o plano premium do Azure Functions nas seguintes situações:

* Os aplicativos de funções executam continuamente ou quase continuamente.
* Você precisa de mais opções de CPU ou memória, que é fornecido pelo plano de consumo.
* Seu código precisa executar por mais tempo do que o [tempo de execução máximo permitido](#timeout) no plano de consumo.
* Você exige recursos que só estão disponíveis em um plano Premium, como conectividade de VNET/VPN.

> [!NOTE]
> A visualização de plano premium atualmente suporta apenas funções do Azure no Windows.

Ao executar as funções de JavaScript em um plano Premium, você deve escolher uma instância com menos vCPUs. Para obter mais informações, consulte o [escolha planos Premium de núcleo único](functions-reference-node.md#considerations-for-javascript-functions).  

## <a name="app-service-plan"></a>Plano de dedicado (serviço de aplicativo)

Seus aplicativos de função também podem executar em VMs dedicadas mesmas como outros aplicativos de serviço de aplicativo (Basic, Standard, Premium e os SKUs isolados).

Considere um plano do serviço de aplicativo nas seguintes situações:

* Você tem VMs subutilizadas que já estão executando outras instâncias do Serviço de Aplicativo.
* Você deseja fornecer uma imagem personalizada para executar suas funções.

Você paga o mesmo para aplicativos de funções em um plano de serviço de aplicativo como faria para outros recursos do serviço de aplicativo, como aplicativos web. Para obter detalhes sobre como o plano do Serviço de Aplicativo funciona, consulte [Visão geral detalhada de planos de Serviço de Aplicativo do Azure](../app-service/overview-hosting-plans.md).

Com um plano de serviço de aplicativo, você pode escalar horizontalmente manualmente adicionando mais instâncias VM. Você também pode habilitar o dimensionamento automático. Para saber mais, confira [Dimensionar a contagem de instâncias manual ou automaticamente](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). Você também pode escalar verticalmente escolhendo um plano do Serviço de Aplicativo diferente. Para obter mais informações, consulte [Escalar verticalmente um aplicativo no Azure](../app-service/web-sites-scale.md). 

Ao executar funções JavaScript em um plano do Serviço de Aplicativo, você deve escolher um plano que tenha menos vCPUs. Para obter mais informações, consulte [escolher planos do serviço de aplicativo de núcleo único](functions-reference-node.md#choose-single-vcpu-app-service-plans). 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

### <a name="always-on"></a> Sempre ativo

Se executar em um plano do Serviço de Aplicativo, você deverá habilitar a configuração **Always On** para que o aplicativo de funções execute corretamente. Em um plano do Serviço de Aplicativo, o tempo de execução das funções ficará ocioso após alguns minutos de inatividade, portanto, apenas gatilhos HTTP "despertarão" suas funções. Always On está disponível apenas em um plano de Serviço de Aplicativo. Em um plano de Consumo, a plataforma ativa automaticamente os aplicativos de função.

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]


Mesmo com Always On habilitado, o tempo limite de execução para funções individuais é controlado pela configuração `functionTimeout` no arquivo de projeto [host.json](functions-host-json.md#functiontimeout).

## <a name="determine-the-hosting-plan-of-an-existing-application"></a>Determinar o plano de hospedagem de um aplicativo existente

Para determinar o plano de hospedagem usado pelo aplicativo de funções, consulte **plano do Serviço de Aplicativo/tipo de preço** na guia **Visão geral** do aplicativo de funções no [portal do Azure](https://portal.azure.com). Para planos do Serviço de Aplicativo, o tipo de preço também é indicado.

![Exibir o plano de dimensionamento no portal](./media/functions-scale/function-app-overview-portal.png)

Também é possível usar a CLI do Azure para determinar o plano, da seguinte maneira:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Quando a saída desse comando for `dynamic`, o aplicativo de funções estará no plano de Consumo. Quando a saída desse comando é `ElasticPremium`, seu aplicativo de função está no plano Premium. Todos os outros valores indicam diferentes camadas de um plano do serviço de aplicativo.

## <a name="storage-account-requirements"></a>Requisitos da conta de armazenamento

Em qualquer plano, um aplicativo de funções requer uma conta de armazenamento do Azure geral, que dá suporte ao armazenamento de BLOBs do Azure, fila, arquivos e tabela. Isso ocorre porque as funções de dependem do armazenamento do Azure para operações como gerenciamento de gatilhos e log de execuções de função, mas algumas contas de armazenamento não dão suporte a filas e tabelas. Essas contas, que incluem contas de armazenamento somente blob (incluindo armazenamento Premium) e contas de armazenamento para uso geral com replicação de armazenamento com redundância de zona, são filtradas das seleções da **Conta de Armazenamento** existente quando você cria um aplicativo de funções.

<!-- JH: Does using a Premium Storage account improve perf? -->

Para saber mais sobre os tipos de conta de armazenamento, consulte [Apresentando os serviços de armazenamento do Azure](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-and-premium-plans-work"></a>Como funcionam os planos de consumo e premium

No consumo e planos premium, a infraestrutura do Azure Functions é dimensionado a recursos de CPU e memória adicionando outras instâncias do host de funções, com base no número de eventos nos quais suas funções são disparadas. Cada instância do host de funções no plano de consumo é limitada a 1,5 GB de memória e uma CPU.  Uma instância do host é o aplicativo de função inteira, que significa que todas as funções dentro de um recurso de compartilhamento do aplicativo de funções em uma instância e a escala ao mesmo tempo. Aplicativos de funções que compartilham o mesmo plano de consumo são dimensionados de forma independente.  O plano premium, o tamanho do plano determinará a CPU e memória disponível para todos os aplicativos nesse plano nessa instância.  

Arquivos de código de função são armazenados em compartilhamentos de arquivos do Azure na conta de armazenamento principal da função. Quando você exclui a conta de armazenamento principal do aplicativo de funções, os arquivos de código de função são excluídos e não podem ser recuperados.

> [!NOTE]
> Quando estiver usando um gatilho de blob em um plano de consumo, poderá haver um atraso de até 10 minutos no processamento de novos blobs. Esse atraso ocorre quando um aplicativo de funções torna-se ocioso. Depois que o aplicativo de funções estiver em execução, os blobs serão processados imediatamente. Para evitar esse atraso de inicialização a frio, use o plano Premium ou use o [gatilho de grade de eventos](functions-bindings-event-grid.md). Para obter mais informações, consulte [o artigo de referência de associação de gatilho de blob](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Escalonamento de tempo de execução

O Azure Functions usa um componente chamado *controlador de escala* para monitorar a taxa de eventos e determinar se deve aumentar ou reduzir. O controlador de escala usa heurística para cada tipo de gatilho. Por exemplo, ao usar um gatilho do armazenamento de Filas do Azure, ele escala com base no tamanho da fila e na idade da mensagem em fila mais antiga.

A unidade de escala para o Azure Functions é o aplicativo de funções. Quando o aplicativo de funções é dimensionado na horizontal, mais recursos são alocados para executar várias instâncias do host do Azure Functions. Em contrapartida, quando a demanda por computação é reduzida, o controlador de escala remove as instâncias do host de função. O número de instâncias é eventualmente reduzido a zero quando nenhuma função está em execução em um aplicativo de funções.

![Controlador de escala monitorando eventos e criando instâncias](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Noções básicas dos comportamentos de dimensionamento

O dimensionamento pode variar em uma série de fatores e ser diferente com base no gatilho e na linguagem selecionada. Há algumas complicações dos comportamentos de dimensionamento a serem consideradas:

* Um único aplicativo de funções será escalado verticalmente somente para um máximo de 200 instâncias. Uma única instância pode processar mais de uma mensagem ou solicitação por vez, portanto, não há um limite definido de número de execuções simultâneas.
* Para gatilhos HTTP, novas instâncias só serão alocadas no máximo uma vez a cada 1 segundo.
* Para os gatilhos não HTTP, novas instâncias só serão alocadas no máximo uma vez a cada 30 segundos.

Gatilhos diferentes também podem ter diferentes limites de dimensionamento, como documentado abaixo:

* [Hub de Evento](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Melhores práticas e padrões para aplicativos escalonáveis

Há muitos aspectos de um aplicativo de funções que afetarão a qualidade da escala, incluindo a configuração do host, o espaço de tempo de execução e a eficiência dos recursos.  Para obter mais informações, consulte a [seção de escalabilidade do artigo sobre considerações de desempenho](functions-best-practices.md#scalability-best-practices). Adicionalmente, é necessário que você saiba como as conexões se comportam na medida em que o aplicativo de funções é dimensionado. Para saber mais, confira [Como gerenciar conexões no Azure Functions](manage-connections.md).

### <a name="billing-model"></a>Modelo de cobrança

Cobrança para os diferentes planos é descrita em detalhes sobre o [página de preços do Azure Functions](https://azure.microsoft.com/pricing/details/functions/). O uso é agregado no nível do aplicativo de funções e conta apenas o tempo durante o qual o código de função é executado. Veja a seguir as unidades de cobrança:

* **Consumo de recursos em GB/s (gigabyte por segundo)** . Calculado como uma combinação do tamanho da memória e o tempo de execução para todas as funções dentro de um aplicativo de Funções. 
* **Execuções**. Contadas toda vez que uma função é executada em resposta a um gatilho de evento.

Consultas úteis e informações sobre como entender sua fatura de consumo podem ser encontradas [sobre as perguntas frequentes sobre cobrança](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ).

[Azure Functions pricing page]: https://azure.microsoft.com/pricing/details/functions

## <a name="service-limits"></a>Limites de serviço

A tabela a seguir indica os limites que se aplicam a aplicativos de funções durante a execução em vários planos de hospedagem:

[!INCLUDE [functions-limits](../../includes/functions-limits.md)]

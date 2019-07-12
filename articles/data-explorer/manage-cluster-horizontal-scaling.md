---
title: Escalar horizontalmente um cluster do Gerenciador de dados do Azure
description: Este artigo descreve as etapas para escalar horizontalmente e escalar em um cluster do Gerenciador de dados do Azure com base na demanda de alteração.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: 29bfcc42462a667850f0b2e1bbda3d29cd1597ab
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571511"
---
# <a name="manage-cluster-horizontal-scaling-to-accommodate-changing-demand"></a>Gerenciar o dimensionamento horizontal de cluster para acomodar a demanda de alteração

Dimensionar um cluster apropriadamente é essencial para o desempenho do Azure Data Explorer. Mas por demanda em um cluster não pode ser prevista com exatidão absoluta. Um tamanho de cluster estático pode levar a subutilização ou superutilização, nenhum deles é ideal.

Uma abordagem melhor é a *escala* de um cluster, adicionando e removendo a capacidade confirme as mudanças na demanda. Há dois fluxos de trabalho para o dimensionamento: 
* Escala horizontal, também chamado e redução de escala.
* Dimensionamento vertical, também chamado de aumento e redução.

Este artigo explica o fluxo de trabalho de colocação em escala horizontal.

Escala horizontal permite que você dimensione a contagem de instâncias automaticamente com base em regras e agendamentos predefinidos. Especifique as configurações de dimensionamento automático para seu cluster no portal do Azure, conforme descrito neste artigo.

## <a name="steps-to-configure-horizontal-scaling"></a>Etapas para configurar o dimensionamento horizontal

No portal do Azure, vá para o recurso de cluster do Gerenciador de dados. Sob o **as configurações** título, selecione **expandir**. 

Selecione seu método de dimensionamento automático desejado: **Escala manual**, **com otimização de dimensionamento automático** ou **personalizado AutoEscala**.

### <a name="manual-scale"></a>Dimensionamento manual

Escala manual é a configuração padrão com a criação do cluster. Isso significa que o cluster tem uma capacidade de cluster estático não será alterado automaticamente. Você pode escolher a capacidade estática usando a barra, e não será alterado até que a próxima vez em que você irá alterar a configuração de expansão do cluster.

   ![Método escala manual](media/manage-cluster-horizontal-scaling/manual-scale-method.png)

### <a name="optimized-autoscale"></a>Dimensionamento automático otimizado

AutoEscala otimizada é o método recomendado de AutoEscala. Etapas para configurar a AutoEscala otimizado:

1. Selecionado a opção de dimensionamento automático otimizado e escolher um limite inferior e um limite superior à quantidade de instâncias do cluster, o dimensionamento automático será feito entre esses limites.
2. Clique em Salvar.

   ![Método de dimensionamento automático otimizado](media/manage-cluster-horizontal-scaling/optimized-autoscale-method.png)

Depois de clicar em Salvar o mecanismo de dimensionamento automático otimizado começará a trabalhar e é ações estarão visíveis no log de atividades do cluster. Esse método de dimensionamento automático está otimizando o desempenho do cluster e os custos: se o cluster será iniciado chegar a um estado de subutilização ele irá ser dimensionada-in que deixar os mesmos e os custos de desempenho, e se o cluster será iniciado chegar a um estado de superutilização, ele será expandido para garantir que ele tem um bom desempenho

### <a name="custom-autoscale"></a>Dimensionamento automático personalizado

Método de dimensionamento automático personalizado permite que você dimensionar o cluster dinamicamente com base nas métricas que você especificar. O gráfico a seguir mostra o fluxo e as etapas para configurar o dimensionamento automático de personalizado. Mais detalhes o gráfico a seguir.

1. No **nome da configuração de AutoEscala** caixa, forneça um nome, como *Scale-out: utilização de cache*. 

   ![Regra de escala](media/manage-cluster-horizontal-scaling/custom-autoscale-method.png)

2. Para **modo de escala**, selecione **dimensionar com base em uma métrica**. Esse modo oferece dimensionamento dinâmico. Você também pode selecionar **dimensionar para uma contagem de instância específica**.

3. Selecione **+ adicionar uma regra**.

4. Na seção **Regra de escala** à direita, informe os valores para cada configuração.

    **Critérios**

    | Configuração | Descrição e valor |
    | --- | --- |
    | **Agregação de tempo** | Selecione um critério de agregação, como **Média**. |
    | **Nome da métrica** | Selecione a métrica na qual a operação de escala se baseará, como **Utilização de cache**. |
    | **Estatística de intervalo de agregação** | Escolha entre **Média**, **Mínima**, **Máxima** e **Soma**. |
    | **Operador** | Escolha a opção apropriada, como **Maior que ou igual a**. |
    | **Limite** | Escolha um valor apropriado. Por exemplo, para a utilização do cache, 80 por cento é um bom ponto de partida. |
    | **Duração (em minutos)** | Escolha um período apropriado para o sistema retroceder ao calcular métricas. Comece com o padrão de dez minutos. |
    |  |  |

    **Ação**

    | Configuração | Descrição e valor |
    | --- | --- |
    | **Operação** | Escolha a opção apropriada para reduzir ou escalar horizontalmente. |
    | **Contagem de instâncias** | Escolha o número de nós ou instâncias que você deseja adicionar ou remover quando uma condição de métrica for atendida. |
    | **Tempo de resfriamento (minutos)** | Escolha um intervalo de tempo apropriado de espera entre as operações de escala. Comece com o padrão de cinco minutos. |
    |  |  |

5. Selecione **Adicionar**.

6. Na seção **Limites de instância** à esquerda, informe os valores para cada configuração.

    | Configuração | Descrição e valor |
    | --- | --- |
    | **Mínimo** | É o número de instâncias para baixo do qual o cluster não será reduzido, independentemente da utilização. |
    | **Máximo** | É o número de instâncias para acima do qual o cluster não será aumentado, independentemente da utilização. |
    | **Padrão** | O número padrão de instâncias. Essa configuração é usada se houver problemas com as métricas de recursos de leitura. |
    |  |  |

7. Clique em **Salvar**.

Agora, você configurou uma operação de expansão para o cluster do Azure Data Explorer. Adicione outra regra para uma operação de redução. Se precisar de ajuda com problemas de dimensionamento de cluster [abrir uma solicitação de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) no portal do Azure.

## <a name="next-steps"></a>Próximas etapas

* [Monitorar o desempenho, integridade e uso com métricas do Data Explorer do Azure](using-metrics.md)
* [Gerenciar o dimensionamento vertical do cluster](manage-cluster-vertical-scaling.md) para o dimensionamento apropriado de um cluster.

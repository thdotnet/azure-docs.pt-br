---
title: Diagnosticar problemas de desempenho de gráficos na área de trabalho remota - Azure
description: Este artigo descreve como usar contadores de gráficos do RemoteFX em sessões de protocolo de área de trabalho remota para diagnosticar problemas de desempenho com gráficos na área de trabalho Virtual do Windows.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 05/23/2019
ms.author: v-chjenk
ms.openlocfilehash: a139542bf9272336784ac96d667d65caa1ed96ff
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607341"
---
# <a name="diagnose-graphics-performance-issues-in-remote-desktop"></a>Diagnosticar problemas de desempenho de gráficos na área de trabalho remota

Quando o sistema não executar conforme o esperado, é importante identificar a origem do problema. Este artigo ajuda você a identificar e corrigir gargalos de desempenho relacionadas a elementos gráficos durante as sessões do protocolo de área de trabalho remota (RDP).

## <a name="find-your-remote-session-name"></a>Localizar o nome da sessão remota

Você precisará de seu nome de sessão remota para identificar os contadores de desempenho de gráficos. Siga as instruções nesta seção para identificar o nome da sessão remota de visualização de área de trabalho Virtual do Windows.

1. Abra o prompt de comando do Windows da sua sessão remota.
2. Execute o **qwinsta** comando.
    - Se a sessão estiver hospedada em uma sessão de várias VM (máquina virtual): O sufixo para cada nome de contador é o mesmo sufixo em seu nome de sessão, como "rdp-tcp 37."
    - Se a sessão estiver hospedada em uma VM que dá suporte a unidades de processamento de gráficos virtual (vGPU): Os contadores são armazenados no servidor, em vez de em sua VM. As instâncias do contador incluem o nome da VM em vez do número no nome da sessão, como "Win8 Enterprise VM."

>[!NOTE]
> Enquanto contadores com RemoteFX em seus nomes, eles incluem da área de trabalho remota de gráficos no vGPU cenários também.

## <a name="access-performance-counters"></a>Contadores de desempenho de acesso

Contadores de desempenho nos gráficos do RemoteFX ajudarão-lo a detectar gargalos, ajudando a controlar coisas como o quadro de tempo de codificação e ignorados quadros.

Depois de determinar o nome da sessão remota, siga estas instruções para coletar os contadores de desempenho de gráficos do RemoteFX para sua sessão remota.

1. Selecione **inicie** > **ferramentas administrativas** > **Monitor de desempenho**.
2. No **Monitor de desempenho** diálogo caixa, expanda **ferramentas de monitoramento**, selecione **Monitor de desempenho**e, em seguida, selecione **adicionar**.
3. No **adicionar contadores** caixa de diálogo, da **contadores disponíveis** lista, expanda o objeto de contador de desempenho para elementos gráficos do RemoteFX.
4. Selecione os contadores a serem monitorados.
5. No **objeto instâncias de selecionado** , selecione as instâncias específicas a serem monitoradas para os contadores selecionados e, em seguida, selecione **Add**. Para selecionar todas as instâncias de contadores disponíveis, selecione **todas as instâncias**.
6. Depois de adicionar os contadores, selecione **Okey**.

Os contadores de desempenho selecionados serão exibidos na tela do Monitor de desempenho.

>[!NOTE]
>Cada sessão ativa em um host tem sua própria instância de cada contador de desempenho.

## <a name="diagnosis"></a>Diagnóstico

Problemas de desempenho relacionados a gráficos geralmente se enquadram em quatro categorias:

- Baixa taxa de quadros
- Vagas aleatórias
- Alta latência de entrada
- Qualidade ruim do quadro

Iniciar o endereçamento de baixa taxa de quadros, vagas aleatórias e alta latência de entrada. A próxima seção informará quais contadores de desempenho medem cada categoria.

### <a name="performance-counters"></a>contadores de desempenho

Esta seção ajuda você a identificar gargalos.

Primeiro, verifique o contador de Frames/Second de saída. Ele mede o número de quadros que estarão disponíveis para o cliente. Se esse valor for menor que o contador de Frames/Second de entrada, os quadros são sendo ignorados. Para identificar o gargalo, use os contadores ignorados por segundo de quadros.

Há três tipos de quadros ignorados/segundo contadores:

- Quadros ignorados/segundo (recursos de rede insuficientes)
- Quadros ignorados/segundo (recursos insuficientes do cliente)
- Quadros ignorados/segundo (recursos de servidor insuficientes)

Um valor alto para qualquer um dos quadros ignorados/segundo contadores implica que o problema está relacionado ao recurso que o contador acompanha. Por exemplo, se o cliente não decodificar e quadros presentes na mesma taxa do servidor fornece os quadros, o contador de quadros ignorados/segundo (cliente recursos insuficientes) será alto.

Se o contador de saída Frames/Second corresponder o contador de entrada Frames/Second, ainda você ainda terá latência incomuns ou atrasando, o problema poderá ser o tempo médio de codificação. Codificação é um processo síncrono que ocorre no servidor no cenário em uma sessão única (vGPU) e na VM no cenário de várias sessões. Tempo médio de codificação deve ser em até 33 ms. Se o tempo médio de codificação é em 33 ms, mas você ainda tiver problemas de desempenho, pode haver um problema com o aplicativo ou sistema operacional que você está usando.

Para obter mais informações sobre como diagnosticar problemas relacionados ao aplicativo, consulte [contadores de desempenho de atraso de entrada do usuário](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters).

Como RDP dá suporte a um tempo médio de codificação de 33 ms, ele dá suporte a uma taxa de quadros de entrada até 30 quadros por segundo. Observe que a 33 ms é a taxa de quadros com suporte máximo. Em muitos casos, a taxa de quadros experiente pelo usuário será menor, dependendo de quantas vezes um quadro é fornecido para RDP pela origem. Por exemplo, tarefas, como assistir a um vídeo exigem uma taxa de quadros de entrada total de 30 quadros por segundo, mas menos tarefas com uso intenso de recursos, como com pouca frequência editando um documento do word não exigem uma alta taxa de quadros de entrada por segundo para uma boa experiência do usuário.

Use o contador de qualidade de quadro para diagnosticar problemas de qualidade de quadro. Esse contador expressa a qualidade do quadro de saída como uma porcentagem da qualidade de quadro de origem. A perda de qualidade pode ser devido a RemoteFX ou podem ser inerente à fonte de gráficos. Se o RemoteFX causado a perda de qualidade, o problema poderá ser uma falta de recursos de rede ou de servidor para enviar conteúdo de maior fidelidade.

## <a name="mitigation"></a>Redução

Se os recursos de servidor estão causando o gargalo, tente um dos seguintes itens para melhorar o desempenho:

- Reduza o número de sessões por host.
- Aumente a memória e recursos no servidor de computação.
- Descarte a resolução da conexão.

Se os recursos de rede estão causando o gargalo, tente um dos seguintes itens para melhorar a disponibilidade de rede por sessão:

- Reduza o número de sessões por host.
- Descarte a resolução da conexão.
- Use uma maior largura de banda rede.

Se os recursos de cliente estão causando o gargalo, siga um ou ambos destes procedimentos para melhorar o desempenho:

- Instale o cliente de área de trabalho remota mais recente.
- Aumente a memória e computação recursos no computador cliente.

> [!NOTE]
> Atualmente não damos suporte o contador Frames/Second do código-fonte. Por enquanto, o contador de origem Frames/Second sempre será definido como 0.

## <a name="next-steps"></a>Próximas etapas

- Para criar uma máquina virtual do Azure GPU otimizada, consulte [configurar a aceleração de GPU (unidade) para o ambiente de visualização de área de trabalho Virtual do Windows de processamento gráfico](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu).
- Para uma visão geral de faixas de solução de problemas e escalonamento de bloqueios, consulte [solução de problemas de visão geral, comentários e suporte](https://docs.microsoft.com/azure/virtual-desktop/troubleshoot-set-up-overview).
- Para saber mais sobre o serviço de visualização, consulte [ambiente de visualização de área de trabalho do Windows](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).

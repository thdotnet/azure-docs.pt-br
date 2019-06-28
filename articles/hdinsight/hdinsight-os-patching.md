---
title: Configurar o agendamento de aplicação de patch no SO para clusters Azure HDInsight baseados em Linux
description: Saiba como configurar o agendamento de aplicação de patch no SO para clusters HDInsight baseados em Linux.
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: 3fad8869a31688e9e2413abb350eccf1f871f7dd
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330712"
---
# <a name="os-patching-for-hdinsight"></a>Aplicação de patch no HDInsight 

> [!IMPORTANT]
> Imagens do Ubuntu ficam disponíveis para a nova criação do cluster HDInsight em três meses de que está sendo publicado. A partir de janeiro de 2019, os clusters em execução **não** contam com patch automaticamente. Os clientes devem usar ações de script ou outros mecanismos para aplicar o patch de um cluster em execução. Clusters recém-criados sempre terão as atualizações mais recentes disponíveis, incluindo os mais recentes patches de segurança.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Como configurar o agendamento de aplicação de patch no SO para clusters HDInsight baseados em Linux
As máquinas virtuais em um cluster HDInsight precisam ser reinicializadas ocasionalmente para que os patches de segurança importantes possam ser instalados. 

Usando as ações de script descritas neste artigo, você pode modificar a aplicação de patch no agendamento da seguinte maneira:
1. Instale todas as atualizações ou instalar kernel + somente atualizações de segurança ou instalar somente atualizações de kernel.
2. Reinicialização imediata ou agenda uma reinicialização na VM.

> [!NOTE]  
> Essas ações de script só funcionará com clusters HDInsight baseados em Linux criados após 1º de agosto de 2016. Os patches terão efeito somente após a reinicialização das VMs. Esses scripts não serão aplicadas automaticamente atualizações para todos os ciclos de atualização futura. Execute os scripts que sempre que novas atualizações precisam ser aplicadas para instalar as atualizações e reinicie a VM.

## <a name="how-to-use-the-script"></a>Como usar o script 

Usando esse script requer as seguintes informações:
1. Localização do script install-atualizações-agenda-reinicializações: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   O HDInsight usa esse URI para localizar e executar o script em todas as máquinas virtuais no cluster. Esse script fornece opções para instalar atualizações e reinicie a VM.
  
2. Localização do script as reinicializações de agenda: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   O HDInsight usa esse URI para localizar e executar o script em todas as máquinas virtuais no cluster. Esse script reinicia a VM.
  
3. Os tipos de nós do cluster ao qual o script é aplicado: nó de cabeçalho, nó de trabalho, zookeeper. Esse script deve ser aplicado a todos os tipos de nó do cluster. Se não for aplicado a um tipo de nó, em seguida, as máquinas virtuais para esse tipo de nó não serão atualizadas ou reinicializadas.

4. Parâmetro: O script install-atualizações de agenda reinicializações aceita dois parâmetros numéricos:

    | Parâmetro | Definição |
    | --- | --- |
    | Instalar somente atualizações de kernel / instalar todas as atualizações/instalar kernel + segurança atualiza apenas |0, 1 ou 2. Um valor de 0 instala atualizações de kernel apenas, enquanto 1 instala todas as atualizações e o kernel 2 instalações + segurança atualiza apenas. Se nenhum parâmetro for fornecido, o padrão é 0. |
    | Nenhuma reinicialização imediata do reinicialização/habilitar agendamento reinicialização/habilitar |0, 1 ou 2. Um valor 0 desativa a reinicialização, enquanto 1 habilita a reinicialização de agendamento e 2 permite que a reinicialização imediata. Se nenhum parâmetro for fornecido, o padrão é 0. O usuário deve inserir o parâmetro 1 para 2 de parâmetro de entrada. |
   
 5. Parâmetro: O script de reinicializações de agendamento aceita um parâmetro numérico:

    | Parâmetro | Definição |
    | --- | --- |
    | Habilitar a reinicialização imediata de reinicialização/habilitar agendamento |1 ou 2. Um valor de 1 permite a realização de agendamento (reinicialização é agendada nas próximas 24 a 12 horas) enquanto 2 habilita o imediata reinicializar (em 5 minutos). Se nenhum parâmetro for fornecido, o padrão é 1. |  

> [!NOTE] 
> Você deve marcar o script como persistente ao aplicar a um cluster existente. Caso contrário, quaisquer nós novos criado por meio de operações de dimensionamento usarão o agendamento de aplicação de patch padrão.  Se você aplicar o script como parte do processo de criação de cluster, ele será persistido automaticamente.


## <a name="next-steps"></a>Próximas etapas

Para etapas específicas sobre como usar a ação de script, consulte as seções a seguir [HDInsight baseado em Linux personalizar clusters usando a ação de script](hdinsight-hadoop-customize-cluster-linux.md):

* [Usar uma ação de script durante a criação do cluster](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Aplicar uma ação de script em um cluster em execução](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)

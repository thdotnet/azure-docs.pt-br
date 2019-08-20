---
title: Solucionar problemas do HBase usando o Azure HDInsight
description: Obtenha respostas para perguntas comuns sobre como trabalhar com o HBase e o Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive, seodec18
ms.topic: troubleshooting
ms.date: 08/14/2019
ms.openlocfilehash: 595acdc46aca76a86038acebdb9a7581c51e3688
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69573948"
---
# <a name="troubleshoot-apache-hbase-by-using-azure-hdinsight"></a>Solucionar problemas do Apache HBase usando o Azure HDInsight

Saiba mais sobre os principais problemas e suas soluções ao trabalhar com cargas de Apache HBase no Apache Ambari.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Como fazer para executar relatórios do comando hbck com várias regiões não atribuídas?

Uma mensagem de erro comum que pode ser executada ao usar o comando `hbase hbck` é: "várias regiões sem atribuição ou espaços na cadeia de regiões."

Na interface do usuário do HBase Master você pode exibir o número de regiões desbalanceadas em todos os servidores da região. Depois, você pode executar o comando `hbase hbck` para ver os espaços na cadeia de regiões.

Os espaços podem ser causados pelas regiões offline, portanto, corrija as atribuições primeiro. 

Execute estas etapas para levar as regiões não atribuídas de volta a um estado normal:

1. Entre no cluster do HDInsight HBase usando SSH.
2. Para conectar-se com o shell do Apache ZooKeeper, execute o comando `hbase zkcli`.
3. Execute o comando `rmr /hbase/regions-in-transition` ou o comando `rmr /hbase-unsecure/regions-in-transition`.
4. Para sair do shell do `hbase zkcli`, use o comando `exit`.
5. Abra a interface do usuário do Apache Ambari e reinicie o serviço Active HBase Master.
6. Execute o comando `hbase hbck` novamente (sem opções). Verifique a saída desse comando para garantir a atribuição de todas as regiões.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Como fazer para corrigir problemas de tempo limite usando comandos hbck para atribuições de região?

### <a name="issue"></a>Problema

Uma possível causa de problemas do tempo limite, quando você usa o comando `hbck`, é que várias regiões se encontram no estado "em transição" por um longo tempo. Você pode ver essas regiões como offline na interface do usuário do HBase Master. Devido ao grande número de regiões tentando fazer a transição, o HBase Master pode atingir o tempo limite e não conseguir colocar essas regiões no estado online novamente.

### <a name="resolution-steps"></a>Etapas de resolução

1. Entre no cluster do HDInsight HBase usando SSH.
2. Para conectar-se com o shell do Apache ZooKeeper, execute o comando `hbase zkcli`.
3. Execute o comando `rmr /hbase/regions-in-transition` ou `rmr /hbase-unsecure/regions-in-transition`.
4. Para sair do shell do `hbase zkcli`, use o comando `exit`.
5. Na interface do usuário do Ambari, reinicie o serviço Active HBase Master.
6. Execute o comando `hbase hbck -fixAssignments` novamente.

## <a name="next-steps"></a>Próximas etapas

Se você não encontrou seu problema ou não conseguiu resolver seu problema, visite um dos seguintes canais para obter mais suporte:

* Obtenha respostas de especialistas do Azure por meio do [suporte da Comunidade do Azure](https://azure.microsoft.com/support/community/).

* Conecte- [@AzureSupport](https://twitter.com/azuresupport) se com a conta de Microsoft Azure oficial para melhorar a experiência do cliente. Conectando a Comunidade do Azure aos recursos certos: respostas, suporte e especialistas.

* Se precisar de mais ajuda, você poderá enviar uma solicitação de suporte do [portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecione **suporte** na barra de menus ou abra o Hub **ajuda + suporte** . Para obter informações mais detalhadas, consulte [como criar uma solicitação de suporte do Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). O acesso ao gerenciamento de assinaturas e ao suporte de cobrança está incluído na sua assinatura do Microsoft Azure, e o suporte técnico é fornecido por meio de um dos [planos de suporte do Azure](https://azure.microsoft.com/support/plans/).

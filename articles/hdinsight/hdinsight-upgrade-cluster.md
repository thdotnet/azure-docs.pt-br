---
title: Atualizar o cluster HDInsight para uma versão mais recente - Azure
description: Saiba como atualizar o cluster HDInsight para uma versão mais nova.
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/04/2017
ms.openlocfilehash: 4d391c095495a3d0e34a2111d7b4564e6f4f2b8d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072004"
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>Atualizar o cluster HDInsight para uma versão mais recente
Para aproveitar os novos recursos do HDInsight, é recomendável que os clusters HDInsight sejam atualizados para a versão mais recente. Siga as diretrizes apresentadas abaixo para atualizar as versões do cluster HDInsight.

> [!NOTE]  
> Para obter mais informações sobre versões com suporte do HDInsight, consulte [Versões de componente do HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).

## <a name="upgrade-tasks"></a>Tarefas de atualização
O fluxo de trabalho para atualizar o cluster HDInsight serão apresentadas a seguir.

![Diagrama de fluxo de trabalho de atualização](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Leia cada seção deste documento para entender as alterações que podem ser necessárias ao atualizar o cluster HDInsight.
2. Crie um cluster como um ambiente de teste/garantia de qualidade. Para saber mais sobre como criar um cluster, consulte [Saiba como criar clusters HDInsight baseados em Linux](hdinsight-hadoop-provision-linux-clusters.md)
3. Copie trabalhos, fontes de dados e coletores existentes para o novo ambiente.
4. Execute testes de validação para garantir que os trabalhos funcionem conforme o esperado no novo cluster.

Depois de verificar se tudo está funcionando conforme o esperado, agende o tempo de inatividade para a migração. Durante esse tempo de inatividade, execute as tarefas a seguir:

1.  Faça backup de dados transitórios armazenados localmente em nós do cluster. Por exemplo, se você tiver dados armazenados diretamente em um nó principal.
2.  Exclua o cluster existente.
3.  Crie um cluster na mesma sub-rede VNET com a versão do HDI mais recente (ou com suporte) que usa o mesmo armazenamento de dados padrão utilizado pelo cluster anterior. Isso permite que o novo cluster continue trabalhando nos dados de produção existentes.
4.  Importe o backup de todos os dados transitórios.
5.  Inicie os trabalhos/continue processando usando o novo cluster.

## <a name="next-steps"></a>Próximas etapas
* [Saiba como criar clusters HDInsight baseados em Linux](hdinsight-hadoop-provision-linux-clusters.md)
* [Conectar-se ao HDInsight usando o SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Gerenciar um cluster baseado em Linux usando o Apache Ambari](hdinsight-hadoop-manage-ambari.md)


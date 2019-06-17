---
title: Diretrizes de recuperação de desastre do Azure Data Lake Analytics
description: Saiba como planejar a recuperação de desastres para suas contas do Azure Data Lake Analytics.
services: data-lake-analytics
author: MikeRys
ms.author: mrys
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ea1d4020aa9be23b4839690ae0b386d35bce8a23
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66498883"
---
# <a name="disaster-recovery-guidance-for-azure-data-lake-analytics"></a>Diretrizes de recuperação de desastre do Azure Data Lake Analytics

O Azure Data Lake Analytics é um serviço de análise sob demanda que simplifica big data. Em vez de implantar, configurar e ajustar o hardware, você escreve consultas para transformar seus dados e extrair informações importantes. O serviço de análise pode manipular trabalhos de qualquer escala de maneira instantânea, simplesmente configurando o controle para a quantidade de potência necessária. Você só paga pelo trabalho quando ele está em execução, o que o torna econômico. Este artigo fornece orientação sobre como proteger seus trabalhos contra as raras interrupções de toda a região ou contra exclusões acidentais.

## <a name="disaster-recovery-guidance"></a>Guia de recuperação de desastres

Ao usar o Azure Data Lake Analytics, é fundamental para preparar seu próprio plano de recuperação de desastre. Este artigo ajuda a guiá-lo para criar um plano de recuperação de desastres. Há recursos adicionais que podem ajudar você a criar seu próprio plano:
- [Recuperação de desastre e falha para aplicativos do Azure](/azure/architecture/reliability/disaster-recovery)
- [Orientações técnicas de resiliência do Azure](/azure/architecture/reliability)

## <a name="best-practices-and-scenario-guidance"></a>Práticas recomendadas e diretrizes de cenário

Você pode executar um trabalho recorrente do U-SQL em uma conta do ADLA em uma região que lê e grava as tabelas de U-SQL, bem como dados não estruturados.  Prepare para um desastre executando as seguintes etapas:

1. Crie contas do ADLA e ADLS na região secundária que será usado durante uma interrupção.

   > [!NOTE]
   > Uma vez que os nomes de conta são globalmente exclusivos, use um esquema de nomenclatura consistente que indica qual conta é secundária.

2. Para dados não estruturados, referenciar [diretrizes de recuperação de desastres para dados no Azure Data Lake armazenamento Gen1](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)

3. Para dados estruturados armazenados em bancos de dados e tabelas do ADLA, crie cópias dos artefatos de metadados, como bancos de dados, tabelas, funções com valor de tabela e assemblies. Você precisa ser ressincronizado periodicamente esses artefatos, quando as alterações ocorrem na produção. Por exemplo, os dados recém-inseridos tem que ser replicadas para a região secundária, copiando os dados e inserção na tabela secundária.

   > [!NOTE]
   > Esses nomes de objeto têm o escopo para a conta secundária e não são globalmente exclusivos, para que eles possam ter os mesmos nomes de conta de produção principal.

Durante uma interrupção, você precisa atualizar seus scripts para que os caminhos de entrada apontem para o ponto de extremidade secundário. Em seguida, os usuários enviam seus trabalhos para a conta do ADLA na região secundária. A saída do trabalho, em seguida, será gravada para a conta do ADLA e ADLS na região secundária.

## <a name="next-steps"></a>Próximas etapas

[Diretrizes de recuperação de desastres para dados no Azure Data Lake armazenamento Gen1](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)

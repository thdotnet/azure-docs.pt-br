---
title: 'Unir dados: Referência de módulo'
titleSuffix: Azure Machine Learning service
description: Saiba como usar a módulo de unir os dados no serviço de Azure Machine Learning de junção para mesclar conjuntos de dados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterclu
ms.date: 06/01/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7e814f5ea4bd47ceb0697e860c946039ce39ae1f
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67518008"
---
# <a name="join-data"></a>Unir dados

Este artigo descreve como usar o **unir os dados** módulo em que a interface visual do serviço de Azure Machine Learning para mesclar dois conjuntos de dados usando uma operação de junção de estilo de banco de dados.  

## <a name="how-to-configure-join-data"></a>Como configurar a junção de dados

Para executar uma junção em dois conjuntos de dados, eles devem estar relacionados por uma coluna de chave. Também há suporte para chaves compostas usando várias colunas. 

1. Adicionar os conjuntos de dados que você deseja combinar e, em seguida, arraste a **ingressar dados** módulo em seu teste. 

    Você pode encontrar o módulo na **Data Transformation** categoria, em **manipulação**.

1. Conectar-se os conjuntos de dados para o **ingressar dados** módulo. 
 
1. Selecione **iniciar seletor de coluna** para escolher colunas de chave. Lembre-se de escolher colunas para entradas esquerda e direita.

    Para uma única chave:

    Selecione uma única coluna de chave para ambas as entradas.
    
    Para uma chave composta:

    Selecione todas as colunas de chave na entrada à esquerda e à direita na mesma ordem. O **unir os dados** módulo unirá as tabelas quando correspondem a todas as colunas de chave. Marque a opção **Permitir duplicatas e preservar a ordem das colunas na seleção** se a ordem das colunas não é o mesmo que a tabela original. 

    ![column-selector](media/module/join-data-column-selector.png)


1. Selecione o **diferenciar maiusculas de minúsculas** opção se você quiser preservar as maiusculas e minúsculas em uma associação de coluna de texto. 
   
1. Use o **tipo de junção** lista suspensa para especificar como os conjuntos de dados devem ser combinados.  
  
    * **Junção interna**: Uma *junção interna* é a operação de junção mais comuns. Ele retorna as linhas combinadas somente quando os valores das colunas de chave corresponderem.  
  
    * **Junção externa esquerda**: Um *junção externa esquerda* retorna linhas Unidas para todas as linhas da tabela esquerda. Quando uma linha na tabela esquerda não possui linhas correspondentes na tabela direita, a linha retornada contém valores ausentes para todas as colunas que vêm da tabela direita. Você também pode especificar um valor de substituição para valores ausentes.  
  
    * **Full Outer Join**: Um *junção externa completa* retorna todas as linhas da tabela esquerda (**table1**) e da tabela direita (**table2**).  
  
         Para cada uma das linhas em uma das tabelas que não têm linhas correspondentes no outro, o resultado inclui uma linha que contém valores ausentes.  
  
    * **Deixado Semijunção**: Um *semi-junção esquerda* retorna apenas os valores da tabela esquerda quando os valores das colunas de chave corresponderem.  

1. Para a opção **manter as colunas de chave à direita na tabela unida**:

    * Selecione esta opção para exibir as chaves de ambas as tabelas de entrada.
    * Anule a seleção para retornar somente as colunas de chave da entrada à esquerda.

1. Execute o experimento, ou selecione o módulo de unir os dados e selecionou **executar selecionado** ao executar a junção.

1. Para exibir os resultados, clique com botão direito do **dados ingressar** > **conjunto de dados de resultados** > **visualizar**.

## <a name="next-steps"></a>Próximas etapas

Consulte a [conjunto de módulos disponíveis](module-reference.md) ao serviço de Azure Machine Learning. 
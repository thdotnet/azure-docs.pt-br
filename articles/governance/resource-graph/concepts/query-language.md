---
title: Noções básicas da linguagem de consulta
description: Descreve os operadores disponíveis do Kusto e funções a ser usadas com o gráfico de recursos do Azure.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/22/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: dcb21a6aedf16b034fad4f0822e22758dda03c33
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65800501"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Noções básicas sobre a linguagem de consulta do Azure Resource Graph

A linguagem de consulta para o Azure Resource Graph dá suporte a diversos operadores e funções. Cada trabalho e operação com base no [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md).

A melhor maneira de saber mais sobre a linguagem de consulta usada pelo gráfico de recursos é iniciar com documentação para a Consulta de Linguagem do[Azure Data Explorer](/azure/kusto/query/index). Ele fornece uma compreensão sobre como a linguagem é estruturada e como os vários operadores e funções suportados trabalham juntos.

## <a name="supported-tabular-operators"></a>Operadores de tabela compatíveis

Aqui está a lista de operadores de tabela compatíveis no Resource Graph:

- [count](/azure/kusto/query/countoperator)
- [distinct](/azure/kusto/query/distinctoperator)
- [extend](/azure/kusto/query/extendoperator)
- [limit](/azure/kusto/query/limitoperator)
- [order by](/azure/kusto/query/orderoperator)
- [project](/azure/kusto/query/projectoperator)
- [project-away](/azure/kusto/query/projectawayoperator)
- [sample](/azure/kusto/query/sampleoperator)
- [sample-distinct](/azure/kusto/query/sampledistinctoperator)
- [sort by](/azure/kusto/query/sortoperator)
- [summarize](/azure/kusto/query/summarizeoperator)
- [take](/azure/kusto/query/takeoperator)
- [início](/azure/kusto/query/topoperator)
- [top-nested](/azure/kusto/query/topnestedoperator)
- [top-hitters](/azure/kusto/query/tophittersoperator)
- [where](/azure/kusto/query/whereoperator)

## <a name="supported-functions"></a>Funções compatíveis

Aqui está a lista de funções compatíveis no Resource Graph:

- [ago()](/azure/kusto/query/agofunction)
- [buildschema()](/azure/kusto/query/buildschema-aggfunction)
- [strcat()](/azure/kusto/query/strcatfunction)
- [isnotempty()](/azure/kusto/query/isnotemptyfunction)
- [tostring()](/azure/kusto/query/tostringfunction)
- [zip()](/azure/kusto/query/zipfunction)

## <a name="escape-characters"></a>Caracteres de escape

Alguns nomes de propriedade, como aquelas que incluem uma `.` ou `$`, deve ser encapsulado ou escape na consulta ou a propriedade nome é interpretado incorretamente e não fornece os resultados esperados.

- `.` -Wrap como tal, o nome da propriedade: `['propertyname.withaperiod']`
  
  Exemplo de consulta que encapsula a propriedade _OData. Type_:

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$` -Escape do caractere no nome da propriedade. O caractere de escape usado depende do shell em que gráfico de recursos é executado.

  - **bash** - `\`

    Exemplo de consulta que ignora a propriedade  _\$tipo_ no bash:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** -não saiam de `$` caracteres.

  - **PowerShell** - ``` ` ```

    Exemplo de consulta que ignora a propriedade  _\$tipo_ no PowerShell:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>Próximas etapas

- Consulte o idioma em uso no [consultas Starter](../samples/starter.md)
- Ver usos avançados em [Consultas avançadas](../samples/advanced.md)
- Aprender a [explorar recursos](explore-resources.md)
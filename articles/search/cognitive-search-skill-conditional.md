---
title: A habilidade de pesquisa cognitiva condicional (Azure Search) | Microsoft Docs
description: A habilidade de condicional permite filtragem, Criando padrões e mesclando valores.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: luisca
ms.openlocfilehash: 149b701d4a1700787656448e2bdd0d92d2a93844
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65791511"
---
#   <a name="conditional-skill"></a>Habilidade condicional

O *habilidade condicional* permite cenários de Azure Search que exigem uma operação booliana para determinar os dados para atribuir a uma saída. Esses cenários incluem a filtragem, atribuindo um valor padrão e mesclando dados com base em uma condição.

O pseudocódigo a seguir demonstra o que realiza a habilidade de condicional:

```
if (condition) 
    { output = whenTrue } 
else 
    { output = whenFalse } 
```

> [!NOTE]
> Essa habilidade não está associada a uma API de serviços Cognitivos do Azure, e você não será cobrado por usá-lo. No entanto, você deve ainda [anexar a um recurso de serviços Cognitivos](cognitive-search-attach-cognitive-services.md) para substituir a opção de recurso "Gratuito" que limita a um pequeno número de aprimoramentos de por dia.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ConditionalSkill


## <a name="evaluated-fields"></a>Campos avaliados

Essa habilidade é especial porque suas entradas são campos avaliados.

Os seguintes itens são valores válidos de uma expressão:

-   Caminhos de anotação (caminhos em expressões devem ser delimitados por "$(" and ")")
 <br/>
    Exemplos:
    ```
        "= $(/document)"
        "= $(/document/content)"
    ```

-  Literais (cadeias de caracteres, números, true, false, null) <br/>
    Exemplos:
    ```
       "= 'this is a string'"   // string (note the single quotation marks)
       "= 34"                   // number
       "= true"                 // Boolean
       "= null"                 // null value
    ```

-  Expressões que usam operadores de comparação (= =,! =, > =, >, < =, <) <br/>
    Exemplos:
    ```
        "= $(/document/language) == 'en'"
        "= $(/document/sentiment) >= 0.5"
    ```

-   Expressões que usam operadores boolianos (& &, | |,!, ^) <br/>
    Exemplos:
    ```
        "= $(/document/language) == 'en' && $(/document/sentiment) > 0.5"
        "= !true"
    ```

-   Expressões que usam operadores numéricos (+, -, \*, /, %) <br/>
    Exemplos: 
    ```
        "= $(/document/sentiment) + 0.5"         // addition
        "= $(/document/totalValue) * 1.10"       // multiplication
        "= $(/document/lengthInMeters) / 0.3049" // division
    ```

Como a habilidade de condicional dá suporte à avaliação, você pode usá-lo em cenários de transformação minor. Por exemplo, consulte [definição de habilidades 4](#transformation-example).

## <a name="skill-inputs"></a>Entradas de habilidades
As entradas diferenciam maiúsculas de minúsculas.

| Entrada   | Descrição |
|-------------|-------------|
| condição   | Essa entrada é um [avaliado campo](#evaluated-fields) que representa a condição a ser avaliada. Essa condição deve ser avaliada como um valor booliano (*verdadeira* ou *falso*).   <br/>  Exemplos: <br/> "= true" <br/> "= $(/document/language) =='fr'" <br/> "= $(/document/pages/\*/language) == $(/document/expectedLanguage)" <br/> |
| whenTrue    | Essa entrada é um [avaliado o campo](#evaluated-fields) que representa o valor a ser retornado se a condição é avaliada como *verdadeiro*. Cadeias de caracteres constantes devem ser retornadas entre aspas simples ('e'). <br/>Valores de exemplo: <br/> "= 'contract'"<br/>"= $(/ documentos/contractType)" <br/> "= $(/documentos/entidades/\*)" <br/> |
| whenFalse   | Essa entrada é um [avaliado o campo](#evaluated-fields) que representa o valor a ser retornado se a condição é avaliada como *falso*. <br/>Valores de exemplo: <br/> "= 'contract'"<br/>"= $(/ documentos/contractType)" <br/> "= $(/documentos/entidades/\*)" <br/>

## <a name="skill-outputs"></a>Saídas de habilidades
Há uma única saída é simplesmente chamada de "saída". Ele retorna o valor *whenFalse* se a condição for falsa ou *whenTrue* se a condição for verdadeira.

## <a name="examples"></a>Exemplos

### <a name="sample-skill-definition-1-filter-documents-to-return-only-french-documents"></a>Definição da habilidade do exemplo 1: Filtrar os documentos para retornar apenas os documentos francês

A saída a seguir retorna uma matriz de frases ("/ documentos/frenchSentences") se o idioma do documento for o francês. Se o idioma não for o francês, o valor é definido como *nulo*.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == 'fr'" },
        { "name": "whenTrue", "source": "/document/sentences" },
        { "name": "whenFalse", "source": "= null" }
    ],
    "outputs": [ { "name": "output", "targetName": "frenchSentences" } ]
}
```
Se "/ documentos/frenchSentences" é usado como o *contexto* de outra habilidade, essa habilidade é executado somente se "/ documentos/frenchSentences" não está definida como *nulo*.


### <a name="sample-skill-definition-2-set-a-default-value-for-a-value-that-doesnt-exist"></a>Definição da habilidade do exemplo 2: Definir um valor padrão para um valor que não existe

A saída a seguir cria uma anotação ("/ documentos/languageWithDefault") que é definida para o idioma do documento ou "es", se o idioma não está definido.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == null" },
        { "name": "whenTrue", "source": "= 'es'" },
        { "name": "whenFalse", "source": "= $(/document/language)" }
    ],
    "outputs": [ { "name": "output", "targetName": "languageWithDefault" } ]
}
```

### <a name="sample-skill-definition-3-merge-values-from-two-fields-into-one"></a>Definição de habilidade de exemplo 3: Mesclar valores de dois campos em um

Neste exemplo, algumas frases têm uma *frenchSentiment* propriedade. Sempre que o *frenchSentiment* propriedade for nula, queremos usar o *englishSentiment* valor. Podemos atribuir a saída a um membro que é chamado *sentimento* ("/ documentos/sentimento / * / sentimento").

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= $(/document/sentences/*/frenchSentiment) == null" },
        { "name": "whenTrue", "source": "/document/sentences/*/englishSentiment" },
        { "name": "whenFalse", "source": "/document/sentences/*/frenchSentiment" }
    ],
    "outputs": [ { "name": "output", "targetName": "sentiment" } ]
}
```

## <a name="transformation-example"></a>Exemplo de transformação
### <a name="sample-skill-definition-4-data-transformation-on-a-single-field"></a>Definição da habilidade do exemplo 4: Transformação de dados em um único campo

Neste exemplo, recebemos uma *sentimento* que está entre 0 e 1. Queremos transformá-lo para ficar entre -1 e 1. Podemos usar a habilidade de condicional para fazer essa transformação secundária.

Neste exemplo, não usamos o aspecto condicional da habilidade porque a condição é sempre *verdadeira*.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= true" },
        { "name": "whenTrue", "source": "= $(/document/sentences/*/sentiment) * 2 - 1" },
        { "name": "whenFalse", "source": "= 0" }
    ],
    "outputs": [ { "name": "output", "targetName": "normalizedSentiment" } ]
}
```

## <a name="special-considerations"></a>Considerações especiais
Alguns parâmetros são avaliados, portanto, você precisa para se tomar bastante cuidado para seguir o padrão documentado. Expressões devem começar com um sinal de igual. Um caminho deve ser delimitado por "$(" and ")". Certifique-se de colocar cadeias de caracteres entre aspas. Que ajuda o avaliador de distinguir entre cadeias de caracteres e caminhos reais e operadores. Além disso, certifique-se de colocar o espaço em branco em torno de operadores (por exemplo, um "*" em um caminho significa algo diferente de multiplicação).


## <a name="next-steps"></a>Próximas etapas

+ [Habilidades predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de qualificações](cognitive-search-defining-skillset.md)

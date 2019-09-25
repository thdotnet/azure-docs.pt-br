---
title: Anexar um recurso dos Serviços Cognitivos a um conjunto de habilidades – Azure Search
description: Instruções para anexar uma assinatura All-in-One de serviços cognitivas a um pipeline de enriquecimento avançado no Azure Search.
manager: nitinme
author: LuisCabrer
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: luisca
ms.openlocfilehash: 113286f829b628d4740fbba34e7279741a934aef
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265922"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Anexar um recurso dos Serviços Cognitivos a um conjunto de habilidades no Azure Search 

Os algoritmos de ia orientam os [pipelines de indexação cognitiva](cognitive-search-concept-intro.md) usados para o enriquecimento de documentos no Azure Search. Esses algoritmos são baseados nos recursos de serviços cognitivas do Azure, incluindo [Pesquisa Visual computacional](https://azure.microsoft.com/services/cognitive-services/computer-vision/) para análise de imagem e OCR (reconhecimento óptico de caracteres) e [análise de texto](https://azure.microsoft.com/services/cognitive-services/text-analytics/) para reconhecimento de entidade, extração de frases-chave e outros aprimoramentos . Conforme usado pelo Azure Search para fins de enriquecimento de documentos, os algoritmos são encapsulados dentro de uma *habilidade*, colocados em um configurador de *qualificações*e referenciados por um *indexador* durante a indexação.

Você pode enriquecer um número limitado de documentos gratuitamente. Ou então, você pode anexar um recurso de serviços de cognitiva Faturável a um *conconhecimento* para cargas de trabalho maiores e mais frequentes. Neste artigo, você aprenderá a anexar um recurso de serviços de cognitiva Faturável para enriquecer documentos durante a [indexação](search-what-is-an-index.md) de Azure Search.

> [!NOTE]
> Os eventos faturáveis incluem chamadas para API de Serviços Cognitivos e extração de imagem como parte do estágio de quebra de documento no Azure Search. Não há nenhum encargo para a extração de texto de documentos ou para habilidades que não chamam serviços cognitivas.
>
> A execução de habilidades faturáveis é o [preço pago pelo uso dos serviços cognitivas](https://azure.microsoft.com/pricing/details/cognitive-services/). Para obter os preços de extração de imagem, consulte a [página de preços do Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="same-region-requirement"></a>Requisito de mesma região

Exigimos que Azure Search e os serviços cognitivas do Azure existam na mesma região. Caso contrário, você receberá essa mensagem em tempo de execução:`"Provided key is not a valid CognitiveServices type key for the region of your search service."` 

Não é possível mover um serviço entre regiões. Se você receber esse erro, deverá criar um novo recurso de serviços cognitivas na mesma região que Azure Search.

> [!NOTE]
> Algumas habilidades internas se baseiam em serviços cognitivas não regionais (por exemplo, a [habilidade de tradução de texto](cognitive-search-skill-text-translation.md)). Lembre-se de que, se você adicionar qualquer uma dessas habilidades ao seu skillr de que seus dados não têm garantia de permanecer na mesma região que o Azure Search ou o recurso de serviços cognitivas. Consulte a [página status do serviço](https://aka.ms/allinoneregioninfo) para obter mais detalhes.

## <a name="use-free-resources"></a>Usar recursos gratuitos

Você pode usar uma opção de processamento limitado e gratuito para concluir os exercícios do início rápido e o tutorial de pesquisa cognitiva.

Recursos gratuitos (aprimoramentos limitados) são restritos a 20 documentos por dia, por assinatura.

1. Abra o assistente de importação de dados:

   ![Abrir o assistente de importação de dados](media/search-get-started-portal/import-data-cmd.png "Abrir o assistente de importação de dados")

1. Escolha uma fonte de dados e continue a **Adicionar pesquisa cognitiva (opcional)** . Para obter uma explicação passo a passo deste assistente, consulte [importar, indexar e consultar usando as ferramentas do portal](search-get-started-portal.md).

1. Expanda **anexar serviços cognitivas** e, em seguida, selecione **gratuito (aprimoramentos limitados)** :

   ![Seção de serviços cognitivas de anexo expandida](./media/cognitive-search-attach-cognitive-services/attach1.png "Seção de serviços cognitivas de anexo expandida")

1. Prossiga para a próxima etapa, **Adicionar aprimoramentos**. Para obter uma descrição das habilidades disponíveis no portal, consulte [etapa 2: Adicione habilidades](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) cognitivas no início rápido da pesquisa cognitiva.

## <a name="use-billable-resources"></a>Usar recursos faturáveis

Para cargas de trabalho que criam mais de 20 aprimoramentos por dia, lembre-se de anexar um recurso de serviços cognitivas cobráveis. É recomendável que você sempre anexe um recurso de serviços de cognitiva Faturável, mesmo que você nunca pretenda chamar API de Serviços Cognitivos. A anexação de um recurso substitui o limite diário.

Você é cobrado apenas por habilidades que chamam o API de Serviços Cognitivos. Você não é cobrado por [habilidades personalizadas](cognitive-search-create-custom-skill-example.md)ou habilidades como [fusão de texto](cognitive-search-skill-textmerger.md), [divisor de texto](cognitive-search-skill-textsplit.md)e [modelador](cognitive-search-skill-shaper.md), que não são baseados em API.

1. Abra o assistente para importar dados, escolha uma fonte de dados e continue a **Adicionar pesquisa cognitiva (opcional)** .

1. Expanda **anexar serviços cognitivas** e, em seguida, selecione **criar novo recurso de serviços cognitivas**. Uma nova guia é aberta para que você possa criar o recurso:

   ![Criar um recurso de Serviços Cognitivos](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Criar um recurso de Serviços Cognitivos")

1. Na lista **local** , selecione a região onde o serviço de Azure Search está localizado. Certifique-se de usar essa região por motivos de desempenho. O uso dessa região também anula os encargos de largura de banda de saída entre regiões.

1. Na lista **tipo de preço** , selecione **S0** para obter a coleção All-in-One de recursos de serviços cognitivas, incluindo os recursos de visão e linguagem que retornam as habilidades predefinidas usadas pelo Azure Search.

   Para a camada S0, você pode encontrar taxas para cargas de trabalho específicas na [página de preços de serviços cognitivas](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + Na lista **selecionar oferta** , verifique se **Serviços cognitivas** está selecionado.
   + Em recursos de **linguagem** , as taxas para **análise de texto Standard** se aplicam à indexação de ia.
   + Em recursos de **visão** , as taxas para **Pesquisa Visual computacional S1** se aplicam.

1. Selecione **criar** para provisionar o novo recurso de serviços cognitivas.

1. Retorne à guia anterior, que contém o assistente de importação de dados. Selecione **Atualizar** para mostrar o recurso serviços cognitivas e, em seguida, selecione o recurso:

   ![Selecione o recurso serviços cognitivas](./media/cognitive-search-attach-cognitive-services/attach2.png "Selecione o recurso serviços cognitivas")

1. Expanda a seção **Adicionar aprimoramentos** para selecionar as habilidades cognitivas específicas que você deseja executar em seus dados. Conclua o restante do assistente. Para obter uma descrição das habilidades disponíveis no portal, consulte [etapa 2: Adicione habilidades](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) cognitivas no início rápido da pesquisa cognitiva.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Anexar um conjunto de habilidades existente a um recurso de Serviços Cognitivos

Se você tiver um conjunto de habilidades existente, poderá anexá-lo a um recurso de Serviços Cognitivos novo ou diferente.

1. Na página **visão geral do serviço** , selecione **habilidades**:

   ![Guia Conjuntos de Habilidades](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Guia Conjuntos de Habilidades")

1. Selecione o nome do conconhecedor e, em seguida, selecione um recurso existente ou crie um novo. Selecione **OK** para confirmar as alterações.

   ![Lista de recursos do conjunto de habilidades](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Lista de recursos do conjunto de habilidades")

   Lembre-se de que a opção **gratuito (aprimoramentos limitados)** limita-se a 20 documentos diariamente, e que você pode usar **criar novos recursos de serviços cognitivas** para provisionar um novo recurso faturável. Se você criar um novo recurso, selecione **Atualizar** para atualizar a lista de recursos dos Serviços cognitivos e, em seguida, selecione o recurso.

## <a name="attach-cognitive-services-programmatically"></a>Anexar Serviços Cognitivos de modo programático

Ao definir o conjunto de habilidades de forma programática, adicione uma seção `cognitiveServices` ao conjunto de habilidades. Nessa seção, inclua a chave do recurso de serviços cognitivas que você deseja associar ao skillset. Lembre-se de que o recurso deve estar na mesma região que o recurso de Azure Search. Também inclua `@odata.type`e defina como `#Microsoft.Azure.Search.CognitiveServicesByKey`.

O exemplo a seguir mostra esse padrão. Observe a `cognitiveServices` seção no final da definição.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Exemplo: Estimar custos

Para estimar os custos associados à indexação de pesquisa cognitiva, comece com uma ideia da aparência de um documento médio para que você possa executar alguns números. Por exemplo, você pode aproximar:

+ 1\.000 PDFs.
+ Seis páginas cada.
+ Uma imagem por página (imagens de 6.000).
+ 3\.000 caracteres por página.

Suponha um pipeline que consiste na quebra de documento de cada PDF, extração de imagem e texto, OCR (reconhecimento óptico de caracteres) de imagens e reconhecimento de entidades de organizações.

Os preços mostrados neste artigo são hipotéticos. Eles são usados para ilustrar o processo de estimativa. Seus custos podem ser menores. Para obter os preços reais das transações, consulte Confira [preços de serviços cognitivas](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Para decifração de documento com conteúdo de texto e imagem, a extração de texto atualmente é gratuita. Para imagens 6.000, suponha $1 para cada 1.000 imagens extraídas. Esse é um custo de $6 para esta etapa.

2. Para OCR de 6.000 imagens em inglês, a habilidade cognitiva do OCR usa o melhor algoritmo (DescribeText). Supondo um custo de US$ 2,50 por 1.000 imagens a serem analisadas, pagaríamos US$ 15,00 para essa etapa.

3. Para extração de entidade, você teria um total de três registros de texto por página. Cada registro tem 1.000 caracteres. Três registros de texto por página multiplicados por 6.000 páginas são iguais a 18.000 registros de texto. Supondo US$ 2,00 por 1.000 registros de texto, essa etapa custaria US$36,00.

Juntando tudo isso, você pagaria cerca de $57 para ingerir documentos PDF de 1.000 desse tipo com o conjunto de qualificações descrito.

## <a name="next-steps"></a>Próximas etapas
+ [Página de preços do Azure Search](https://azure.microsoft.com/pricing/details/search/)
+ [Como definir um conjunto de qualificações](cognitive-search-defining-skillset.md)
+ [Criar conjunto de qualificações (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Como mapear campos enriquecidos](cognitive-search-output-field-mapping.md)

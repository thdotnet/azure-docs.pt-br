---
title: Conectar-se ao repositório de conhecimento com o Power BI – Azure Search
description: Crie um repositório de conhecimento usando o assistente para importar dados no portal do Azure e, em seguida, conecte-se com o Power BI para análise e exploração.
author: heidisteen
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: tutorial
ms.date: 07/30/2019
ms.author: heidist
ms.openlocfilehash: 1c7f297092760f2a92f524347a3c1a5e353d0965
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69635545"
---
# <a name="create-an-azure-search-knowledge-store-and-connect-using-power-bi"></a>Criar um repositório de conhecimento do Azure Search e conectar-se usando o Power BI

> [!Note]
> O armazenamento de dados de conhecimento está em versão prévia e não é destinado a uso em produção. A [API REST do Azure Search versão 2019-05-06 versão prévia](search-api-preview.md) fornece esse recurso. Não há suporte para SDK do .NET no momento.
>

O repositório de conhecimento é um recurso do Azure Search que persiste a saída de um pipeline de enriquecimento de IA para análise subsequente ou outro processamento downstream. Um pipeline aprimorado por IA aceita arquivos de imagem ou arquivos de texto não estruturados de uma fonte de dados compatível, envia-os para indexação pelo Azure Search, aplica os aprimoramentos de IA dos Serviços Cognitivos (tais como análise de imagem e processamento de linguagem natural) e, em seguida, salva os resultados em um repositório de conhecimento no Armazenamento do Azure. Na loja de conhecimento, você pode anexar ferramentas como o Power BI ou o Gerenciador de Armazenamento para explorar os resultados.

Neste artigo, crie uma loja de conhecimento no portal e, em seguida, conecte-se e explore usando o Power Query no Power BI Desktop. 

Este passo a passo usa um conjunto de dados que consiste em análises e avaliações de clientes, pontuação de sentimentos dos Serviços Cognitivos e, em seguida, Power Query para consultar seus documentos. Os dados são provenientes de dados de resenhas do hotel no Kaggle.com. 

## <a name="prerequisites"></a>Pré-requisitos

+ [Baixar arquivo CSV de resenhas de hotel (HotelReviews_Free.csv)](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Este conjunto de dados contém registros de comentários do cliente sobre hotéis.

+ [Power BI Desktop](https://powerbi.microsoft.com/downloads/)

+ [Azure Search](search-create-service-portal.md). É possível usar um serviço gratuito para este passo a passo. 

+ [Armazenamento do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Verifique se a conta é de "uso geral", ou seja, *StorageV2 (uso geral V2*), que é o padrão, ou *Storage (uso geral V1*). Escolha a mesma região que o Azure Search.
 
## <a name="prepare-data"></a>Preparar dados 

Carregue o arquivo. csv no Armazenamento de Blobs do Azure para que ele possa ser acessado por um indexador do Azure Search.

1. [Entre no portal do Azure](https://portal.azure.com), navegue até sua conta de Armazenamento do Azure, clique em **Blobs** e, em seguida, clique em **+ Contêiner**.

1. [Crie um contêiner de Blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) para conter dados de exemplo: 

   1. Dê ao contêiner o nome de `hotel-reviews`. 
   
   1. Defina o Nível de Acesso Público para qualquer um de seus valores válidos. Nós usamos o padrão.

1. Depois que o contêiner for criado, abra-o e selecione **Carregar** na barra de comandos.

1. Navegue até a pasta que contém o **HotelReviews-Free.csv**, selecione o arquivo e clique em **Carregar**.

   ![Carregar o arquivo .csv](media/knowledge-store-howto-powerbi/hotel-reviews-blob-container.png "Carregar o arquivo .csv")

1. Enquanto você estiver no armazenamento do Azure, obtenha o nome de contêiner e a cadeia de conexão.  Você precisará de ambas essas cadeias de caracteres ao criar um objeto de fonte de dados:

   1. Na página de visão geral, clique em **Chaves de Acesso** e copie uma *cadeia de conexão*. Ela começa com `DefaultEndpointsProtocol=https;` e termina com `EndpointSuffix=core.windows.net`. O nome e a chave da sua conta estão entre as duas. 

   1. O nome do contêiner deve ser `hotel-reviews` ou qualquer nome que você tenha atribuído. 

## <a name="create-and-run-ai-enrichments"></a>Criar e executar aprimoramentos de IA

Use o assistente para importar dados para criar o repositório de conhecimento. Você importará o conjunto de dados, escolherá aprimoramentos, configurará um repositório de conhecimento e um índice e, em seguida, executará.

1. [Encontre o serviço de pesquisa](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) no portal do Azure.

1. Clique em **Importar dados** na barra de comandos.

1. Crie o objeto de fonte de dados na primeira página do assistente.

   - Selecione **Armazenamento de Blobs do Azure**.

   - Dê um nome à fonte de dados, tal como *hotel-reviews-ds*.

   - Como os dados são. csv, selecione **Texto delimitado** para o modo de análise, selecione a **Primeira Linha Contém o Cabeçalho** e verifique se o caractere delimitador é uma vírgula.

   - A cadeia de conexão para sua conta de Armazenamento do Azure pode ser obtida no portal, em **Chaves de acesso**.

   - O nome do contêiner também pode ser obtido no portal, na sua lista de Blobs do Armazenamento do Azure.

      ![Criar um objeto de fonte de dados](media/knowledge-store-howto-powerbi/hotel-reviews-ds.png "Criar um objeto de fonte de dados")

1. Adicione aprimoramentos e configure um repositório de conhecimento na segunda página do assistente.

   - Em **Anexar Serviços Cognitivos**, você pode usar o recurso gratuito que permite até 20 transações por dia. O número de registros em HotelReviews-Free.csv é menor que 20.

   - Em **Adicionar Aprimoramentos**, nomeie o conjunto de habilidades *hotel-reviews-ss*, escolha o campo *reviews_text*, escolha um nível de granularidade de *Páginas (partes de 5.000 caracteres)* e, em seguida, selecione estas três habilidades cognitivas: *Extrair frases-chave*, *Detectar o idioma*, *Detectar sentimento*.

      ![Criar um conjunto de habilidades](media/knowledge-store-howto-powerbi/hotel-reviews-ss.png "Criar um conjunto de habilidades")

   - Em **Salvar aprimoramentos no repositório de conhecimento**, forneça a cadeia de conexão para sua conta de Armazenamento do Azure de uso geral. Uma loja de conhecimento é criada no armazenamento de tabelas do Azure quando você seleciona as opções *projetos de tabela do Azure* nesta seção.

      ![Configurar repositório de conhecimento](media/knowledge-store-howto-powerbi/hotel-reviews-ks.png "Configurar repositório de conhecimento")
   
   Clique em **Avançar: Personalize o índice de destino** para continuar para a próxima etapa.

1. Configure um índice para consultas de pesquisa de texto completo opcionais no Azure Search. Embora o foco deste passo a passo seja o Power BI conectado ao Armazenamento de Tabelas do Azure, o assistente **Importar dados** também pode criar um índice usado para pesquisa de texto completo no Azure Search. 

   O assistente obtém amostras da fonte de dados para inferir campos e tipos de dados, portanto, tudo o que você precisa fazer é selecionar os atributos necessários para obter os comportamentos desejados. Por exemplo, *Recuperável* significa que o conteúdo do campo pode ser recuperado do serviço, enquanto *Pesquisável* permite a pesquisa de texto completo nos campos selecionados.

   - Dê um nome ao índice, tal como *hotel-reviews-idx*.
   - Defina todos os campos como **Recuperável**.
   - Defina *cidade*, *nome*, *reviews_text*, *idioma*, *frases-chave* como **Pesquisáveis**.
   - Defina *sentimento*, *idioma*, *frases-chave* como **Filtrável** e **Com faceta**. 
   
    O índice deve ser semelhante à imagem a seguir.

     ![Configurar um índice](media/knowledge-store-howto-powerbi/hotel-reviews-idx.png "Configurar um índice")

   Clique em **Avançar: Crie um indexador** para continuar para a próxima etapa.

1. Configure um indexador dando a ele um nome e uma cadência de execução. Para esta explicação, use *hotel-reviews-idxr* como o nome do indexador e use agenda **Once** padrão para executar o indexador imediatamente.

   A execução do indexador coloca todas as configurações anteriores em andamento. Extração, processamento e ingestão: tudo acontece aqui nesta etapa.

1. Monitore a indexação na fila de notificações no portal. A execução leva vários minutos para ser concluída.

## <a name="connect-with-power-bi"></a>Conectar com Power BI

1. Inicie o Power BI Desktop e selecione **Obter dados**.

1. Em obter dados, selecione **Azure** e, em seguida, **Armazenamento de Tabelas do Azure**. Clique em **Conectar**.

1. Em Nome da Conta ou URL, cole o nome da conta de Armazenamento do Azure (a URL completa é resolvida para você).

1. Insira a chave de conta.

1. Selecione Documento, Frases-chave e Páginas. Essas são as tabelas criadas pelo Assistente de importação de dados quando você seleciona os itens de mesmo nome na configuração do repositório de conhecimento. Clique em **Carregar**.

1. Abra o Power Query clicando no comando **Editar Consultas**.

   ![Abrir Power Query](media/knowledge-store-howto-powerbi/powerbi-edit-queries.png "Abrir Power Query")

1. Para Documentos:

   - Remova as colunas PartitionKey, RowKey e Timestamp criadas pelo Armazenamento de Tabelas do Azure. O repositório de conhecimento fornece relações usadas nesta análise.

   - Expanda a coluna de conteúdo.

     ![Editar tabelas](media/knowledge-store-howto-powerbi/powerbi-edit-table.png "Editar tabelas")

1. Selecione todos os campos e, em seguida, desmarque aqueles começando com "metadados".

1. Corrija o tipo de dados para as colunas a seguir usando o ícone ABC-123 em cada coluna:

   - As colunas de data devem ser **DateTime**
   - *Latitude* deve ser **Número Decimal**
   - *Longitude* deve ser **Número Decimal**

1. Repita as etapas anteriores para Frases-chave, removendo PartitionKey e outras colunas, expandindo colunas de conteúdo, definindo *SentimentScore* para **Número Decimal**.

1. Repita novamente para páginas, removendo PartitionKey e outras colunas, expandindo colunas de conteúdo. Não há nenhuma modificação de tipo de dados para esta tabela.

1. Clique em **Fechar e Aplicar** na extrema esquerda da barra de comandos do Power Query.

1. Valide se o Power BI reconhece as relações que o repositório de conhecimento criou em seus dados. Clique no bloco de relações no painel de navegação à esquerda. Todas as três tabelas devem estar relacionadas. Edite as relações e verifique se a “Direção entre filtros” foi definida como ambos; com isso, todos os seus visuais serão atualizados quando um filtro for aplicado.

   ![Validar relações](media/knowledge-store-howto-powerbi/powerbi-relationships.png "Validar relações")

## <a name="try-with-larger-data-sets"></a>Experimentar com conjuntos de dados maiores

Nós mantivemos o conjunto de dados pequeno intencionalmente para evitar encargos para um passo a passo de demonstração. Para uma experiência mais realista, você pode criar e anexar um recurso de Serviços Cognitivos faturável para habilitar um número maior de transações em relação ao analisador de sentimentos, à extração de frases-chave e às habilidades do detector de idiomas.

Crie novos contêineres no Armazenamento de Blobs do Azure e carregue cada arquivo CSV em seu próprio contêiner. Especifique um desses contêineres na etapa de criação da fonte de dados no Assistente de importação de dados.

| DESCRIÇÃO | Link |
|-------------|------|
| Camada gratuita   | [HotelReviews_Free.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Pequeno (500 registros) | [HotelReviews_Small.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Small.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Médio (6.000 registros)| [HotelReviews_Medium.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Medium.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D)
| Grande (35.000 registros de conjuntos de dados completos) | [HotelReviews_Large.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Large.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Lembre-se de que conjuntos de dados muito grandes são caros de processar. Esse específico custa aproximadamente 1.000 dólares americanos.|

Na etapa de enriquecimento do assistente, anexe um recurso de [Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) faturável criado na camada *S0*, na mesma região que o Azure Search, para usar conjuntos de dados maiores. 

  ![Criar um recurso de Serviços Cognitivos](media/knowledge-store-howto-powerbi/create-cognitive-service.png "Criar um recurso de Serviços Cognitivos")

## <a name="next-steps"></a>Próximas etapas

Se você quiser repetir este exercício ou fazer outro passo a passo de enriquecimento por IA, exclua o indexador *hotel-reviews-idx* que você acabou de criar. A exclusão do indexador redefine o contador de transações diárias gratuitas de volta para zero. 

Para obter mais instruções passo a passo demonstrando o repositório de conhecimento, continue com o próximo artigo, que mostra como criar um repositório de conhecimento usando as APIs REST e o Postman.

> [!div class="nextstepaction"]
> [Introdução ao repositório de conhecimento](knowledge-store-howto.md)

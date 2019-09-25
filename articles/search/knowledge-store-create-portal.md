---
title: Criar um repositório de conhecimento no portal do Azure – Azure Search
description: Crie um repositório de conhecimento do Azure Search para persistir aprimoramentos do pipeline de pesquisa cognitiva, usando o assistente de Importação de dados no portal do Azure.
author: lisaleib
services: search
ms.service: search
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: v-lilei
ms.openlocfilehash: fb979a7ff4144694aecad0985c5bce9be2de05bd
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265205"
---
# <a name="create-an-azure-search-knowledge-store-in-the-azure-portal"></a>Criar um repositório de conhecimento do Azure Search no portal do Azure

> [!Note]
> O armazenamento de dados de conhecimento está em versão prévia e não é destinado a uso em produção. A [API REST do Azure Search versão 2019-05-06 versão prévia](search-api-preview.md) fornece esse recurso. Não há suporte para SDK do .NET no momento.
>

O repositório de conhecimento é um recurso do Azure Search que persiste a saída de um pipeline de enriquecimento de IA para análise posterior ou outro processamento downstream. Um pipeline aprimorado por IA aceita arquivos de imagem ou arquivos de texto não estruturados, indexa-os usando o Azure Search, aplica os aprimoramentos de IA dos Serviços Cognitivos (tais como análise de imagem e processamento de linguagem natural) e, em seguida, salva os resultados em um repositório de conhecimento no Armazenamento do Azure. É possível usar ferramentas como o Power BI ou o Gerenciador de Armazenamento para explorar o repositório de conhecimento.

Neste artigo, você usará o assistente de Importação de Dados no portal do Azure para ingerir, indexar e aplicar os aprimoramentos de AI a um conjunto de resenhas de hotéis. As resenhas de hotéis são importadas para o Armazenamento de Blobs do Azure e os resultados são salvos como um repositório de conhecimento no Armazenamento de Tabelas do Azure.

Depois de criar o repositório de conhecimento, será possível aprender a acessá-lo usando o Gerenciador de Armazenamento ou o Power BI.

## <a name="prerequisites"></a>Pré-requisitos

+ [Crie um serviço Azure Search](search-create-service-portal.md) ou [localizar um serviço existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) na assinatura atual. Você pode usar um serviço gratuito para este tutorial.

+ [Crie uma conta de Armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) para armazenar dados de exemplo e o repositório de conhecimento. Sua conta de armazenamento deve usar a mesma localização (como US-WEas seu serviço do Azure Search e o *Tipo de conta* deve ser *Armazenamento V2 (Uso Geral V2)* (padrão) ou *Armazenamento (Uso Geral V1)* .

## <a name="load-the-data"></a>Carregar os dados

Carregue o arquivo CSV de resenhas de hotéis no Armazenamento de Blobs do Azure para que ele possa ser acessado por um indexador do Azure Search e alimentado por meio do pipeline de aprimoramento de IA.

### <a name="create-an-azure-blob-container-with-the-data"></a>Criar um contêiner de Blobs do Azure com os dados

1. [Baixe os dados de resenhas de hotel salvos em um arquivo CSV (HotelReviews_Free.csv)](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Esses dados são originados do Kaggle.com e contêm comentários do cliente sobre hotéis.
1. [Entre no portal do Azure](https://portal.azure.com) e navegue até sua conta de armazenamento do Azure.
1. [Criar um contêiner de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) Para fazer isso, na barra de navegação esquerda de sua conta de armazenamento, clique em **Blobs** e, em seguida, clique em **+ Contêiner** na barra de comandos.
1. Para o novo contêiner **Nome**, insira `hotel-reviews`.
1. Selecione qualquer **Nível de Acesso Público**. Nós usamos o padrão.
1. Clique em **OK** para criar o contêiner de Blobs do Azure.
1. Abra o novo contêiner `hotels-review`, clique em **Upload** e selecione o arquivo **HotelReviews-Free.csv** que você baixou na primeira etapa.

    ![Fazer upload dos dados](media/knowledge-store-create-portal/upload-command-bar.png "Carregar as resenhas de hotéis")

1. Clique em **Upload** para importar o arquivo CSV no Armazenamento de Blobs do Azure. O novo contêiner será exibido.

    ![Criar o contêiner de Blobs do Azure](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "o contêiner de Blobs do Azure")

### <a name="get-the-azure-storage-account-connection-string"></a>Obter a cadeia de conexão da conta de Armazenamento do Azure

1. No portal, navegue até sua conta de Armazenamento do Azure.
1. Na navegação esquerda do serviço, clique em **Chaves de acesso**.
1. Em **chave 1**, copie e salve a *Cadeia de conexão*. A cadeia de caracteres começa com `DefaultEndpointsProtocol=https`. O nome e a chave da sua conta de armazenamento estão inseridos na cadeia de caracteres. Mantenha essa cadeia de caracteres acessível. Você precisará dela em etapas futuras.

## <a name="create-and-run-ai-enrichments"></a>Criar e executar aprimoramentos de IA

Use o assistente para importar dados para criar o repositório de conhecimento. Você criará uma fonte de dados, escolherá aprimoramentos, configurará um repositório de conhecimento e um índice e, em seguida, executará.

### <a name="start-the-import-data-wizard"></a>Iniciar o assistente Importar dados

1. No portal do Azure, [Encontre seu serviço de pesquisa](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).

1. Na barra de comandos, clique em **Importar dados** para iniciar o assistente de importação.

### <a name="connect-to-your-data-import-data-wizard"></a>Conectar-se aos seus dados (Assistente de importação de dados)

Nesta etapa do assistente, você criará uma fonte de dados do Blob do Azure com os dados dos seus hotéis.

1. Na lista **Fonte de Dados**, selecione **Armazenamento de Blobs do Azure**.
1. Para o **Nome**, insira `hotel-reviews-ds`.
1. Para **Modo de análise**, selecione **Texto delimitado** e, em seguido, marque a caixa de seleção **A Primeira Linha Contém Cabeçalho**. Verifique se o **Caractere delimitador** é uma vírgula (,).
1. Insira a **Cadeia de Conexão** do serviço de armazenamento salva em uma etapa anterior.
1. Para **Nome do contêiner**, insira `hotel-reviews`.
1. Clique em **Avançar: adicionar pesquisa cognitiva (Opcional)** .

      ![Criar um objeto de fonte de dados](media/knowledge-store-create-portal/hotel-reviews-ds.png "Criar um objeto de fonte de dados")

## <a name="add-cognitive-search-import-data-wizard"></a>Adicionar pesquisa cognitiva (Assistente de importação de dados)

Nesta etapa do assistente, você criará um conjunto de habilidades com aprimoramentos de habilidades cognitivas. As habilidades usadas neste modelo extrairão frases-chave e detectarão o idioma e o sentimento. Esses aprimoramentos serão “projetados” em um repositório de conhecimento como tabelas do Azure.

1. Expanda **Anexar Serviços Cognitivos**. **Gratuito (Aprimoramentos limitados)** é selecionado por padrão. É possível usar esse recurso porque o número de registros em HotelReviews-Free.csv é 19 e esse recurso gratuito permite até 20 transações por dia.
1. Expanda **Adicionar Aprimoramentos**.
1. Em **Nome do conjunto de habilidades**, insira `hotel-reviews-ss`.
1. Em **Campo de dados de origem**, selecione **reviews_text*.
1. Para **Nível de granularidade do aprimoramento**, selecione **Páginas (5 mil partes de caracteres)**
1. Selecione estas habilidades cognitivas:
    + **Extraia frases-chave**
    + **Detectar o idioma**
    + **Detectar sentimento**

      ![Criar um conjunto de habilidades](media/knowledge-store-create-portal/hotel-reviews-ss.png "Criar um conjunto de habilidades")

1. Expanda **Salvar aprimoramentos no repositório de conhecimento**.
1. Insira a **Cadeia de Conexão da conta de armazenamento** salva em uma etapa anterior.
1. Selecione estas **projeções de tabela do Azure**:
    + **Documentos**
    + **Páginas**
    + **Frases-chave**

    ![Configurar repositório de conhecimento](media/knowledge-store-create-portal/hotel-reviews-ks.png "Configurar repositório de conhecimento")

1. Clique em **Avançar: personalizar o índice de destino**.

### <a name="import-data-import-data-wizard"></a>Importar dados (Assistente de importação de dados)

Nesta etapa do assistente, você configurá um índice para consultas de pesquisa de texto completo opcionais. O assistente criará um exemplo de sua fonte de dados para inferir campos e tipos de dados. Só é necessário selecionar os atributos do seu comportamento desejado. Por exemplo, o atributo **Recuperável** permitirá que o serviço de pesquisa retorne um valor de campo enquanto o **Pesquisável** habilitará a pesquisa de texto completo no campo.

1. Em **Nome do índice**, insira `hotel-reviews-idx`.
1. Para atributos, faça estas seleções:
    + Selecione **Recuperável** para todos os campos.
    + Selecione **Filtrável** e **Com faceta** para estes campos: *Sentimento*, *Idioma*, *Palavras-chave*
    + Selecione **Pesquisável** para estes campos: *cidade*, *nome*, *reviews_text*, *idioma*, *Palavras-chave*

    O índice deve ser semelhante à imagem a seguir. Como a lista é longa, nem todos os campos estão visíveis na imagem.

    ![Configurar um índice](media/knowledge-store-create-portal/hotel-reviews-idx.png "Configurar um índice")

1. Clique em **Avançar: criar um indexador**.

### <a name="create-an-indexer"></a>Criar um indexador

Nesta etapa do assistente, você configurará um indexador que reunirá a fonte de dados, o conjunto de habilidades e o índice definidos nas etapas anteriores do assistente.

1. Para **Nome**, insira `hotel-reviews-idxr`.
1. Para **Agenda**, mantenha o padrão **Uma vez**.
1. Clique em **Enviar** para executar o indexador. A extração de dados, a indexação e a aplicação de habilidades cognitivas acontecem nesta etapa.

### <a name="monitor-the-notifications-queue-for-status"></a>Monitorar a fila de Notificações para ver o status

1. No portal do Azure, monitore o log de atividades de Notificações para obter um link de status de **notificação do Azure Search** clicável. A execução pode levar vários minutos para ser concluída.

## <a name="next-steps"></a>Próximas etapas

Agora que você aprimorou seus dados usando os serviços cognitivos e projetou os resultados em um repositório de conhecimento, é possível usar o Gerenciador de Armazenamento ou o Power BI para explorar seu conjunto de dados aprimorado.

Para saber como explorar esse repositório de conhecimento usando o Gerenciador de Armazenamento, confira o seguinte passo a passo.

> [!div class="nextstepaction"]
> [Exibir com o Gerenciador de Armazenamento](knowledge-store-view-storage-explorer.md)

Para saber como conectar esse repositório de conhecimento ao Power BI, confira o seguinte passo a passo.

> [!div class="nextstepaction"]
> [Conectar com o Power BI](knowledge-store-connect-power-bi.md)

Se desejar repetir esse exercício ou experimentar um passo a passo de aprimoramento de IA diferente, exclua o indexador *hotel-reviews-idx*. A exclusão do indexador redefine o contador de transações diárias gratuito para zero.

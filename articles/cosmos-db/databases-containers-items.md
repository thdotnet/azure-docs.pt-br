---
title: Trabalhar com bancos de dados, contêineres e itens no Azure Cosmos DB
description: Este artigo descreve como criar e usar bancos de dados, contêineres e itens no Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 574dd9fd6189b6d0f1e5d455146d6d083ad7ff77
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389462"
---
# <a name="work-with-databases-containers-and-items-in-azure-cosmos-db"></a>Trabalhar com bancos de dados, contêineres e itens no Azure Cosmos DB

Depois de criar uma [conta do Azure Cosmos DB](account-overview.md) na sua assinatura do Azure, você pode gerenciar dados em sua conta criando bancos de dados, contêineres e itens. Este artigo descreve cada uma dessas entidades. 

A imagem a seguir mostra a hierarquia de entidades diferentes em uma conta do Azure Cosmos DB:

![Entidades de conta do Azure Cosmos](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Bancos de dados do Azure Cosmos

Você pode criar um ou vários bancos de dados Cosmos do Azure em sua conta. Um banco de dados é análogo a um namespace. Um banco de dados é a unidade de gerenciamento para um conjunto de contêineres do Cosmos do Azure. A tabela a seguir mostra como um banco de dados do Azure Cosmos é mapeado para várias entidades específicas a uma API:

| Entidade de Cosmos do Azure | API do SQL | API Cassandra | API do Azure Cosmos DB para MongoDB | API do Gremlin | API de Tabela |
| --- | --- | --- | --- | --- | --- |
|Banco de dados do Azure Cosmos | Banco de dados | Keyspace | Banco de dados | Banco de dados | ND |

> [!NOTE]
> Com contas da API de tabela, ao criar sua primeira tabela, um banco de dados padrão é criado automaticamente em sua conta do Cosmos do Azure.

### <a name="operations-on-an-azure-cosmos-database"></a>Operações em um banco de dados do Azure Cosmos

Você pode interagir com um banco de dados Cosmos do Azure com APIs do Azure Cosmos conforme descrito na tabela a seguir:

| Operação | CLI do Azure | API do SQL | API Cassandra | API do Azure Cosmos DB para MongoDB | API do Gremlin | API de Tabela |
| --- | --- | --- | --- | --- | --- | --- |
|Enumerar todos os bancos de dados| Sim | Sim | Sim (o banco de dados é mapeado para um keyspace) | Sim | ND | ND |
|Ler banco de dados| Sim | Sim | Sim (o banco de dados é mapeado para um keyspace) | Sim | ND | ND |
|Criar novo banco de dados| Sim | Sim | Sim (o banco de dados é mapeado para um keyspace) | Sim | ND | ND |
|Atualizar banco de dados| Sim | Sim | Sim (o banco de dados é mapeado para um keyspace) | Sim | ND | ND |


## <a name="azure-cosmos-containers"></a>Contêineres do Azure Cosmos

Um contêiner do Cosmos do Azure é a unidade de escalabilidade para armazenamento e taxa de transferência provisionada. Um contêiner é particionado horizontalmente e, em seguida, é replicado em várias regiões. Os itens que você adiciona ao contêiner e a taxa de transferência que provisiona nesse contêiner são ambos automaticamente distribuídos em um conjunto de partições lógicas baseado na chave de partição. Para saber mais sobre particionamento e chaves de partição, consulte [particionar dados](partition-data.md). 

Quando você cria um contêiner do Azure Cosmos, você configurar a taxa de transferência em um dos seguintes modos:

* **Modo de taxa de transferência provisionada dedicada**: A taxa de transferência provisionada em um contêiner é exclusivamente reservada para o contêiner e ele é feito por SLAs. Para obter mais informações, consulte [como provisionar a taxa de transferência em um contêiner do Azure Cosmos](how-to-provision-container-throughput.md).

* **Modo de taxa de transferência provisionada compartilhado**: Esses contêineres compartilham a taxa de transferência provisionada com outros contêineres no mesmo banco de dados (exceto os contêineres que foram configurados com taxa de transferência provisionada dedicada). Em outras palavras, a taxa de transferência provisionada no banco de dados é compartilhada entre todos os contêineres de "taxa de transferência compartilhada". Para obter mais informações, consulte [como provisionar a taxa de transferência em um banco de dados do Azure Cosmos](how-to-provision-database-throughput.md).

> [!NOTE]
> Você pode configurar a taxa de transferência dedicada e compartilhada somente ao criar o banco de dados e o contêiner. Para alternar do modo de taxa de transferência dedicada para o modo de taxa de transferência compartilhada (e vice-versa) depois que o contêiner é criado, você precisa criar um novo contêiner e migrar os dados para o novo contêiner. Você pode migrar os dados usando a recurso de feed de alterações do Azure Cosmos DB.

Um contêiner do Azure Cosmos pode dimensionar elasticamente, quer você crie contêineres usando os modos de taxa de transferência provisionada compartilhados ou dedicados.

Um contêiner do Azure Cosmos é um contêiner de itens independente de esquema. Itens em um contêiner podem ter esquemas arbitrários. Por exemplo, um item que representa uma pessoa e um item que representa um automóvel podem ser colocados na *mesmo contêiner*. Por padrão, todos os itens que você adiciona a um contêiner são indexados automaticamente sem a necessidade de índice explícito ou gerenciamento de esquema. Você pode personalizar o comportamento de indexação Configurando a [política de indexação](index-overview.md) em um contêiner. 

Você pode definir [tempo de vida (TTL)](time-to-live.md) nos itens selecionados em um contêiner do Azure Cosmos ou para todo o contêiner normalmente limpar esses itens do sistema. O Azure Cosmos DB exclui automaticamente os itens quando eles expiram. Ela também garante que uma consulta executada no contêiner não retorna os itens expirados dentro de um limite fixo. Para obter mais informações, consulte [configurar o TTL em seu contêiner](how-to-time-to-live.md).

Você pode usar [o feed de alterações](change-feed.md) para inscrever-se ao log de operações que é gerenciado para cada partição lógica do seu contêiner. Feed de alterações fornece o log de todas as atualizações realizadas no contêiner, juntamente com as imagens dos itens anterior e posterior. Para obter mais informações, consulte [crie aplicativos reativos usando o feed de alterações](serverless-computing-database.md). Você também pode configurar a duração de retenção para que a alteração de feed usando a política no contêiner de feed de alterações. 

Você pode registrar [procedimentos armazenados, disparadores, funções definidas pelo usuário (UDFs)](stored-procedures-triggers-udfs.md), e [procedimentos de mesclagem](how-to-manage-conflicts.md) para seu contêiner do Azure Cosmos. 

Você pode especificar uma [restrição de chave exclusiva](unique-keys.md) em seu contêiner do Azure Cosmos. Ao criar uma política de chave exclusiva, você garante a exclusividade de um ou mais valores por chave de partição lógica. Se você criar um contêiner usando uma política de chave exclusiva, não há itens novos ou atualizados com valores que o especificado pela restrição de chave exclusiva de valores duplicam podem ser criados. Para obter mais informações, confira [Restrições de chave exclusivas](unique-keys.md).

Um contêiner do Azure Cosmos é especializado em entidades específicas de API, conforme mostrado na tabela a seguir:

| Entidade de Cosmos do Azure | API do SQL | API Cassandra | API do Azure Cosmos DB para MongoDB | API do Gremlin | API de Tabela |
| --- | --- | --- | --- | --- | --- |
|Contêiner do Azure Cosmos | Coleção | Tabela | Coleção | Grafo | Tabela |

### <a name="properties-of-an-azure-cosmos-container"></a>Propriedades de um contêiner do Azure Cosmos

Um contêiner do Azure Cosmos tem um conjunto de propriedades definidas pelo sistema. Dependendo de qual API você usar, algumas propriedades podem não ser diretamente expostas. A tabela a seguir descreve a lista de propriedades definidas pelo sistema:

| Propriedade definida pelo sistema | Configurável pelo usuário ou gerados pelo sistema | Finalidade | API do SQL | API Cassandra | API do Azure Cosmos DB para MongoDB | API do Gremlin | API de Tabela |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_ID | Gerados pelo sistema | Identificador exclusivo do contêiner | Sim | Não | Não | Não | Não  |
|\_etag | Gerados pelo sistema | Marca da entidade usada para controle de simultaneidade otimista | Sim | Não | Não | Não | Não  |
|\_ts | Gerados pelo sistema | Último carimbo de data/hora atualizado do contêiner | Sim | Não | Não | Não | Não |
|\_Self | Gerados pelo sistema | URI endereçável do contêiner | Sim | Não | Não | Não | Não  |
|ID | Configurável pelo usuário | Nome exclusivo do contêiner definido pelo usuário | Sim | sim | sim | sim | Sim |
|indexingPolicy | Configurável pelo usuário | Fornece a capacidade de alterar o caminho de índice, o tipo de índice e o modo de índice | Sim | Não | Não | Não  | Sim |
|TimeToLive | Configurável pelo usuário | Fornece a capacidade de excluir itens automaticamente de um contêiner após um período de tempo definido. Para obter detalhes, consulte [tempo de vida](time-to-live.md). | Sim | Não | Não | Não  | Sim |
|changeFeedPolicy | Configurável pelo usuário | Usado para ler as alterações feitas a itens em um contêiner. Para obter detalhes, consulte [o feed de alterações](change-feed.md). | Sim | Não | Não | Não  | Sim |
|uniqueKeyPolicy | Configurável pelo usuário | Usado para garantir a exclusividade de um ou mais valores em uma partição lógica. Para obter mais informações, consulte [restrições de chave exclusivas](unique-keys.md). | Sim | Não | Não | Não  | Sim |

### <a name="operations-on-an-azure-cosmos-container"></a>Operações em um contêiner do Azure Cosmos

Um contêiner do Cosmos do Azure suporta as seguintes operações ao usar qualquer uma das APIs do Azure Cosmos:

| Operação | CLI do Azure | API do SQL | API Cassandra | API do Azure Cosmos DB para MongoDB | API do Gremlin | API de Tabela |
| --- | --- | --- | --- | --- | --- | --- |
| Enumerar os contêineres em um banco de dados | Sim | sim | sim | Sim | ND | ND |
| Ler um contêiner | Sim | sim | sim | Sim | ND | ND |
| Criar um novo contêiner | Sim | sim | sim | Sim | ND | ND |
| Atualizar um contêiner | Sim | sim | sim | Sim | ND | ND |
| Excluir um contêiner | Sim | sim | sim | Sim | ND | ND |

## <a name="azure-cosmos-items"></a>Itens do Azure Cosmos

Dependendo de qual API você usar, um item de Cosmos do Azure pode representar a um documento em uma coleção, uma linha em uma tabela, ou um nó ou borda em um gráfico. A tabela a seguir mostra o mapeamento de entidades da API específica a um item do Azure Cosmos:

| Entidade de cosmos | API do SQL | API Cassandra | API do Azure Cosmos DB para MongoDB | API do Gremlin | API de Tabela |
| --- | --- | --- | --- | --- | --- |
|Item do Azure Cosmos | Documento | Linha | Documento | Nó ou borda | item |

### <a name="properties-of-an-item"></a>Propriedades de um item

Cada item Cosmos do Azure tem as seguintes propriedades definidas pelo sistema. Dependendo de qual API você usar, alguns deles podem não ser diretamente exposto.

| Propriedade definida pelo sistema | Configurável pelo usuário ou gerados pelo sistema| Finalidade | API do SQL | API Cassandra | API do Azure Cosmos DB para MongoDB | API do Gremlin | API de Tabela |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_ID | Gerados pelo sistema | Identificador exclusivo do item | Sim | Não | Não | Não | Não  |
|\_etag | Gerados pelo sistema | Marca da entidade usada para controle de simultaneidade otimista | Sim | Não | Não | Não | Não  |
|\_ts | Gerados pelo sistema | Carimbo de hora da última atualização do item | Sim | Não | Não | Não | Não  |
|\_Self | Gerados pelo sistema | URI endereçável do item | Sim | Não | Não | Não | Não |
|ID | Você pode usar o | Nome exclusivo definido pelo usuário em uma partição lógica. Se o usuário não especifica a ID, o sistema gera automaticamente um. | Sim | sim | sim | sim | Sim |
|Propriedades arbitrárias definidas pelo usuário | Definido pelo usuário | Propriedades definidas pelo usuário, representadas na representação de API nativo (incluindo JSON, BSON e CQL) | Sim | sim | sim | sim | Sim |

### <a name="operations-on-items"></a>Operações em itens

Itens do Cosmos do Azure dão suporte para as operações a seguir. Você pode usar qualquer uma das APIs do Azure Cosmos para executar as operações.

| Operação | CLI do Azure | API do SQL | API Cassandra | API do Azure Cosmos DB para MongoDB | API do Gremlin | API de Tabela |
| --- | --- | --- | --- | --- | --- | --- |
| Inserir, Substituir, Excluir, Upsert, Ler | Não | sim | sim | sim | sim | Sim |

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre esses conceitos e tarefas:

* [Provisionar a produtividade em um banco de dados Cosmos do Azure](how-to-provision-database-throughput.md)
* [Provisionar a produtividade em um contêiner do Azure Cosmos](how-to-provision-container-throughput.md)
* [Trabalhar com partições lógicas](partition-data.md)
* [Configurar o TTL em um contêiner do Azure Cosmos](how-to-time-to-live.md)
* [Criar aplicativos reativos, usando o feed de alterações](change-feed.md)
* [Configurar uma restrição de chave exclusiva em seu contêiner do Azure Cosmos](unique-keys.md)

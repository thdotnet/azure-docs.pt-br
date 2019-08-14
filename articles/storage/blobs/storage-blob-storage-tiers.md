---
title: Camadas de acesso quentes, frias e de arquivo para BLOBs-armazenamento do Azure
description: Camadas de acesso quentes, frias e de arquivo para contas de armazenamento do Azure.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/23/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: clausjor
ms.openlocfilehash: 8f180308133ffba12cc3bffb19130aa7a129da9f
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952886"
---
# <a name="azure-blob-storage-hot-cool-and-archive-access-tiers"></a>Armazenamento de BLOBs do Azure: camadas de acesso quentes, frias e de arquivo

O armazenamento do Azure oferece diferentes níveis de acesso, que permitem armazenar dados de objeto de BLOB da maneira mais econômica. As camadas de acesso disponíveis incluem:

- Otimizado para o armazenamento de dados que são acessados com frequência.
- Muito otimizado para armazenar dados que são acessados com pouca frequência e armazenados por pelo menos 30 dias.
- **Arquivado** -otimizado para armazenar dados que raramente são acessados e armazenados por pelo menos 180 dias com requisitos de latência flexíveis (na ordem de horas).

As seguintes considerações se aplicam às diferentes camadas de acesso:

- Somente as camadas de acesso quentes e frias podem ser definidas no nível da conta. A camada de acesso de arquivamento não está disponível no nível da conta.
- Camadas quentes, frias e de arquivo podem ser definidas no nível do blob.
- Os dados na camada de acesso frio podem tolerar uma disponibilidade ligeiramente menor, mas ainda exigem alta durabilidade, latência de recuperação e características de taxa de transferência semelhantes aos dados ativos. Para dados legais, um SLA (contrato de nível de serviço) de disponibilidade ligeiramente menor e custos de acesso mais altos em comparação com os dados ativos são compensações aceitáveis para reduzir os custos de armazenamento.
- O armazenamento de arquivos armazena dados offline e oferece os custos de armazenamento mais baixos, mas também os mais altos reidratar de dados e custos de acesso.

Os dados armazenados na nuvem aumentam em ritmo exponencial. Para gerenciar os custos de suas necessidades cada vez maiores de armazenamento, é útil organizar seus dados com base em atributos como frequência de acesso e período de retenção planejado para otimizar os custos. Os dados armazenados na nuvem podem ser diferentes em termos de como são gerados, processados e acessados durante seu tempo de vida. Alguns dados são ativamente acessados e modificados durante seu ciclo de vida. Alguns dados são acessados com frequência no início do seu tempo de vida, mas esse acesso cai drasticamente à medida que os dados envelhecem. Alguns dados permanecem ociosos na nuvem e raramente são acessados depois de armazenados.

Cada um desses cenários de acesso a dados se beneficia de uma camada de acesso diferente que é otimizada para um padrão de acesso específico. Com as camadas de acesso frequente, esporádica e de arquivo morto, o armazenamento de BLOBs do Azure atende a essa necessidade de camadas de acesso diferenciado com modelos de preços separados.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="storage-accounts-that-support-tiering"></a>Contas de armazenamento que dão suporte a camadas

A camada de dados de armazenamento de objetos em alta, esporádica ou arquivo morto só tem suporte em contas de armazenamento de BLOB e Uso Geral v2 (GPv2). Contas de Uso geral v1 (GPv1) não dão suporte a camadas. No entanto, os clientes podem converter facilmente suas contas de Armazenamento de Blob ou de GPv1 existentes para contas de GPv2 por meio de um processo simples de um clique no Portal do Azure. O GPv2 fornece uma nova estrutura de preços para BLOBs, arquivos e filas, além de acesso a uma variedade de novos recursos de armazenamento. Além disso, mais adiante, alguns novos recursos e cortes de preços serão oferecidos somente em contas GPv2. Portanto, os clientes devem avaliar o uso de contas do GPv2 após a revisão abrangente do novo preço, já que algumas cargas de trabalho podem ser mais caras no GPv2 do que o GPv1. Para saber mais, confira [Visão geral da conta de armazenamento do Azure](../common/storage-account-overview.md).

As contas de armazenamento de BLOBs e GPv2 expõem o atributo **camada de acesso** no nível da conta. Esse atributo permite que você especifique a camada de acesso padrão como frequente ou fria para qualquer blob na conta de armazenamento que não tenha uma camada explícita definida no nível do objeto. Para objetos com a camada definida no nível do objeto, a camada da conta não será aplicada. A camada de arquivo pode ser aplicada somente no nível do objeto. Você pode alternar entre essas camadas de acesso a qualquer momento.

## <a name="premium-performance-block-blob-storage"></a>Armazenamento de blobs de blocos de desempenho premium

O armazenamento de blobs de blocos de desempenho premium disponibiliza dados acessados com frequência por meio de hardware de alto desempenho. Os dados nesse nível de desempenho são armazenados em SSDs (unidades de estado sólido), que são otimizados para latência baixa e consistente. O SSDs fornece taxas transacionais e taxa de transferência maiores em comparação com os discos rígidos tradicionais.

O armazenamento de blobs de blocos de desempenho Premium é ideal para cargas de trabalho que exigem tempos de resposta rápidos e consistentes. É melhor para cargas de trabalho que executam muitas transações pequenas, como a captura de dados de telemetria, mensagens e transformação de dados. Os dados que envolvem usuários finais, como edição interativa de vídeo, conteúdo da Web estático e transações online também são bons candidatos.

O armazenamento de blobs de blocos de desempenho Premium está disponível apenas por meio do tipo de conta de armazenamento de blobs de blocos e atualmente não dá suporte a camadas para camadas de acesso frequente, esporádica ou de arquivamento.

## <a name="hot-access-tier"></a>Camada de acesso quente

A camada de acesso quente tem custos de armazenamento maiores do que as camadas frias e de arquivo morto, mas os custos de acesso mais baixos. Os cenários de uso de exemplo para a camada de acesso quente incluem:

- Dados que estão em uso ativo ou que devem ser acessados (lidos e gravados) com frequência.
- Dados que são preparados para processamento e migração eventual para a camada de acesso fria.

## <a name="cool-access-tier"></a>Camada de acesso frio

A camada de acesso frio reduz os custos de armazenamento e os custos de acesso mais altos em comparação com o armazenamento dinâmico. Essa camada destina-se aos dados que permanecerão na camada esporádica por pelo menos 30 dias. Os cenários de uso de exemplo para a camada de acesso frio incluem:

- Conjuntos de dados de recuperação de desastre e de backup de curto prazo.
- Conteúdo de mídia mais antigo que não é mais exibido frequentemente, mas ainda deve estar disponível imediatamente quando acessado.
- Grandes conjuntos de dados que precisam ser armazenados de forma econômica enquanto mais dados estão sendo obtidos para processamento futuro. (*por exemplo*, armazenamento de longo prazo de dados científicos; dados brutos de telemetria de uma instalação de manufatura)

## <a name="archive-access-tier"></a>Camada de acesso ao arquivo

A camada de acesso de arquivamento tem o menor custo de armazenamento e os maiores custos de recuperação de dados em comparação com as camadas quentes e frias. Essa camada destina-se aos dados que podem tolerar várias horas de latência de recuperação e permanecerão na camada de arquivos por pelo menos 180 dias.

Enquanto um blob está no armazenamento de arquivo morto, os dados de blob ficam offline e não podem ser lidos, copiados, substituídos ou modificados. Não é possível tirar instantâneos de um blob no armazenamento de arquivos. No entanto, os metadados de blob permanecem online e disponíveis, permitindo que você liste o blob e suas propriedades. Para BLOBs no arquivo morto, as únicas operações válidas são getblobproperties, GetBlobMetadata, ListBlobs, SetBlobTier e DeleteBlob.

Os cenários de uso de exemplo para a camada de acesso de arquivamento incluem:

- Backup de longo prazo, backup secundário e conjuntos de dados de arquivamento
- Dados originais (brutos) que devem ser preservados, mesmo após serem processados em formato utilizável final. (*por exemplo*, arquivos de mídia brutos após transcodificação em outros formatos)
- Dados de conformidade e arquivamento que precisam ser armazenados por um longo tempo e quase nunca são acessados. (*Por exemplo*, vídeo de câmara de segurança, raios X/MRIs para empresas de saúde, gravações de áudio e transcrições de chamadas de clientes para serviços financeiros)

### <a name="blob-rehydration"></a>Reidratação de blob

[!INCLUDE [storage-blob-rehydrate-include](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="account-level-tiering"></a>Camadas em nível de conta

Os BLOBs em todas as três camadas de acesso podem coexistir dentro da mesma conta. Qualquer blob que não tenha uma camada atribuída explicitamente herda a camada da configuração de nível de acesso da conta. Se a camada de acesso for inferida da conta, você verá a propriedade de blob **Camada de Acesso Inferida** definida como "true", e a propriedade de blob **Camada de Acesso** corresponde à camada da conta. No portal do Azure, a propriedade _camada de acesso deduzida_ é exibida com a camada de acesso de blob como **ativa (inferida)** ou **fria (inferida)** .

A alteração da camada de acesso da conta aplica-se a todos os objetos inferidos da _camada de acesso_ armazenados na conta que não têm uma camada explícita definida. Caso altere a camada de conta de frequente para esporádico, você será cobrado por operações de gravação (por 10.000) para todos os blobs sem uma camada de conjunto apenas em contas de GPv2. Não há nenhum encargo para essa alteração em contas de Armazenamento de Blobs. Você será cobrado por operações de leitura (por 10.000) e por recuperação de dados (por GB) se alternar de fria para quente no armazenamento de BLOBs ou em contas de GPv2.

## <a name="blob-level-tiering"></a>Camada no nível do blob

As camadas no nível do blob permitem que você altere a camada de seus dados no nível de objeto usando uma única operação chamada [Definir Nível de Blob](/rest/api/storageservices/set-blob-tier). Você pode alterar facilmente o nível de acesso de um blob entre as camadas quente, fria ou de arquivo morto como alteração de padrões de uso, sem a necessidade de mover dados entre contas. Todas as alterações de camadas acontecem imediatamente. No entanto, reidratar um blob dos arquivos pode levar várias horas.

A hora da última alteração na camada de blob é exposta por meio da propriedade do blob **Acessar Hora de Alteração da Camada**. Se um blob estiver na camada de arquivo morto, ele não poderá ser substituído, portanto, carregar o mesmo BLOB não é permitido neste cenário. Você pode substituir um blob em uma camada quente ou fria; nesse caso, o novo BLOB herda a camada do blob que foi substituído.

> [!NOTE]
> O armazenamento de arquivos e as camadas no nível do blob só oferecem suporte aos blobs de bloco. Você também não pode alterar atualmente a camada de um blob de blocos que tenha instantâneos.

### <a name="blob-lifecycle-management"></a>Gerenciamento de ciclo de vida de blob

O gerenciamento do ciclo de vida do armazenamento de BLOBs oferece uma política avançada baseada em regras que você pode usar para fazer a transição de seus dados para a melhor camada de acesso e para expirar os dados no final do ciclo de vida. Ver [gerenciar o ciclo de vida de armazenamento de BLOBs do Azure](storage-lifecycle-management-concepts.md) para saber mais.  

> [!NOTE]
> Os dados armazenados em uma conta de armazenamento de blob de blocos (desempenho Premium) atualmente não podem ser colocados em camadas para quente, frio ou arquivo morto usando [Set Blob Tier](/rest/api/storageservices/set-blob-tier) ou usando o gerenciamento de ciclo de vida do armazenamento de BLOBs do Azure.
> Para mover dados, você deve copiar blobs de forma síncrona da conta de armazenamento de blobs de blocos para a camada de acesso quente em uma conta diferente usando o [bloco Put da API de URL](/rest/api/storageservices/put-block-from-url) ou uma versão do AzCopy que dê suporte a essa API.
> A API *Colocar bloco pela URL* copia dados sincronicamente no servidor, o que significa que a chamada é concluída apenas depois que todos os dados são movidos do local do servidor de origem para o local de destino.

### <a name="blob-level-tiering-billing"></a>Cobrança da camada no nível do blob

Quando um blob é movido para uma camada mais esporádica (frequente -> esporádico, frequente -> arquivos ou esporádico -> arquivos), a operação é cobrada como uma operação de gravação na camada de destino, em que os encargos pela operação de gravação (por 10.000) e gravação de dados (por GB) da camada de destino são aplicados.

Quando um blob é movido para uma camada mais quente (arquivo morto-> frio, > quente ou frio > quente), a operação é cobrada como uma leitura da camada de origem, em que a operação de leitura (por 10.000) e os encargos de recuperação de dados (por GB) da camada de origem se aplicam. Encargos de exclusão antecipada para qualquer blob tirado das camadas esporádica ou de arquivo também podem incorrer. A tabela a seguir resume como as alterações na camada são cobradas.

| | **Encargos de gravação (operação + acesso)** | **Encargos de leitura (operação + acesso)**
| ---- | ----- | ----- |
| **Direção de SetBlobTier** | frio de > quente,<br> arquivo de > quente,<br> arquivo de > frio | arquivo morto-> frio,<br> arquivo morto-> quente,<br> frio-> quente

### <a name="cool-and-archive-early-deletion"></a>Exclusão antecipada esporádica e de arquivo

Além dos encargos por GB e por mês, qualquer blob que seja colocado na camada esporádica (apenas para contas de GPv2) está sujeito a um período inicial de exclusão moderada de 30 dias, e qualquer blob que seja colocado na camada de arquivo está sujeito a um período de exclusão antecipada do arquivo de 180 dias. A cobrança é proporcional. Por exemplo, se um blob for movido para o arquivo e depois for excluído ou movido para a camada frequente após 45 dias, será cobrada um valor pela exclusão antecipada equivalente a 135 dias (180 menos 45) de armazenamento de blob no arquivo.

Você pode calcular a exclusão antecipada usando a propriedade BLOB, **Last-Modified**, se não houver nenhuma alteração na camada de acesso. Caso contrário, você pode usar quando a camada de acesso foi modificada pela última vez para fria ou arquivo morto exibindo a propriedade blob: **acesso-camada-alteração-tempo**. Para obter mais informações sobre as propriedades de blob, confira [Obter Propriedades de Blob](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties).

## <a name="comparing-block-blob-storage-options"></a>Comparando opções de armazenamento de blobs de blocos

A tabela a seguir mostra uma comparação do armazenamento de blobs de blocos de desempenho premium e as camadas de acesso frequente, fria e de arquivo.

|                                           | **Desempenho premium**   | **Camada quente** | **Camada fria**       | **Camada de arquivo morto**  |
| ----------------------------------------- | ------------------------- | ------------ | ------------------- | ----------------- |
| **Disponibilidade**                          | 99,9%                     | 99,9%        | 99%                 | Offline           |
| **Disponibilidade** <br> **(Leituras de RA-GRS)**  | N/D                       | 99,99%       | 99,9%               | Offline           |
| **Encargos de uso**                         | Custos de armazenamento mais altos, menor acesso e custo da transação | Custos de armazenamento maiores, custos de acesso e de transações menores | Custos de armazenamento menores, custos de acesso e de transações maiores | Custos de armazenamento mais baixos, custos de acesso e de transações mais altos |
| **Tamanho mínimo do objeto**                   | N/D                       | N/D          | N/D                 | N/D               |
| **Duração mínima de armazenamento**              | N/D                       | N/D          | 30 dias<sup>1</sup> | 180 dias
| **Latência** <br> **(Tempo até o primeiro byte)** | Milissegundos de dígito único | milissegundos | milissegundos        | horas<sup>2</sup> |

<sup>1</sup> os objetos na camada fria em contas GPv2 têm uma duração de retenção mínima de 30 dias. As contas de armazenamento de BLOBs não têm uma duração de retenção mínima para a camada fria.

<sup>2</sup> armazenamento de arquivos atualmente dá suporte a duas reidratar prioridades, alta e padrão, que oferecem latências de recuperação diferentes. Para obter mais informações, consulte [blob reidratação](#blob-rehydration).

> [!NOTE]
> As contas de armazenamento de BLOBs dão suporte às mesmas metas de desempenho e escalabilidade que as contas de armazenamento de uso geral v2. Para saber mais, consulte [Metas de desempenho e escalabilidade do Armazenamento do Azure](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="quickstart-scenarios"></a>Cenários de início rápido

Nesta seção, os cenários a seguir são demonstrados usando o Portal do Azure:

- Como alterar a camada de acesso de conta padrão de uma conta de Armazenamento de Blobs ou de GPv2.
- Como alterar a camada de um blob em uma conta de Armazenamento de Blobs ou de GPv2.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>Alterar a camada de acesso de conta padrão de uma conta de Armazenamento de Blobs ou de GPv2

1. Entre no [Portal do Azure](https://portal.azure.com).

1. Para navegar até sua conta de armazenamento, selecione Todos os Recursos e sua conta de armazenamento.

1. Na folha Configurações, clique em **Configuração** para exibir e/ou alterar a configuração da conta.

1. Selecione a camada de acesso certa para suas necessidades: Defina a **Camada de acesso** como **Esporádico** ou **Frequente**.

1. Clique em **Salvar** na parte superior da folha.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>Alterar a camada de um blob em uma conta de armazenamento de BLOBs ou de GPv2

1. Entre no [Portal do Azure](https://portal.azure.com).

1. Para navegar até o blob em sua conta de armazenamento, selecione **todos os recursos**, selecione sua conta de armazenamento, selecione o contêiner e, em seguida, selecione o blob.

1. Na folha **Propriedades do blob** , selecione o menu suspenso **camada de acesso** para selecionar a camada de acesso **quente**, **fria**ou de **arquivamento** .

1. Clique em **Salvar** na parte superior da folha.

## <a name="pricing-and-billing"></a>Preços e cobrança

Todas as contas de armazenamento usam um modelo de preços para armazenamento de blobs de blocos com base na camada de cada blob. Tenha em mente as seguintes considerações de cobrança:

- **Custos de armazenamento**: Além da quantidade de dados armazenados, o custo de armazenamento de dados varia dependendo da camada de acesso. O custo por gigabyte diminui conforme a camada fica mais esporádica.
- **Custos de acesso a dados**: os encargos de acesso a dados aumentam conforme a camada fica mais esporádica. Para dados na camada de acesso fria e de arquivo morto, você será cobrado por um encargo de acesso a dados por gigabyte para leituras.
- **Custos de transação**: há um encargo por transação para todas as camadas que aumenta à medida que a camada fica mais esporádica.
- **Custos de transferência de dados com replicação geográfica**: isso só se aplica a contas com replicação geográfica configurada, incluindo GRS e RA-GRS. A transferência de dados de replicação geográfica acarreta um encargo por gigabyte.
- **Custos de transferência de dados de saída**: transferências de dados de saída (dados que são transferidos para fora de uma região do Azure) acarretam a cobrança por uso de largura de banda por gigabyte, de forma consistente com as contas de armazenamento de finalidade geral.
- **Alterando a camada de acesso**: Alterar a camada de acesso da conta resultará em encargos de alteração de camada para BLOBs de _camada de acesso_ inferidos armazenados na conta que não têm uma camada explícita definida. Para obter informações sobre como alterar a camada de acesso de um único BLOB, consulte [cobrança em camadas no nível do blob](#blob-level-tiering-billing).

> [!NOTE]
> Para obter mais informações sobre os preços para BLOBs de blocos, consulte a página de [preços do armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/) . Para saber mais informações sobre os encargos de transferência de dados de saída, confira a página [Detalhes de preços de transferências de dados](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="faq"></a>Perguntas Frequentes

**Devo usar contas de Armazenamento de Blobs ou de GPv2 se quiser colocar meus dados em camadas?**

Recomendamos que você use o GPv2 em vez de contas de Armazenamento de Blobs para camadas. O GPv2 dá suporte a todos os recursos aos quais as contas de Armazenamento de Blobs dão suporte e muito mais. O preço entre o Armazenamento de Blobs e o GPv2 são quase idênticos, mas alguns novos recursos e cortes de preços só estarão disponíveis em contas de GPv2. Contas de GPv1 não dão suporte a camadas.

A estrutura de preços entre contas de GPv1 e GPv2 é diferente, e os clientes devem avaliar cuidadosamente antes de decidir usar contas de GPv2. Você pode converter facilmente uma conta existente de Armazenamento de Blobs ou de GPv1 em GPv2 por meio de um processo simples de um clique. Para saber mais, confira [Visão geral da conta de armazenamento do Azure](../common/storage-account-overview.md).

**Posso armazenar objetos em todas as três camadas de acesso (quentes, frias e de arquivamento) na mesma conta?**

Sim. O atributo de **camada de acesso** definido no nível da conta é a camada de conta padrão que se aplica a todos os objetos nessa conta sem uma camada de conjunto explícita. No entanto, camadas no nível do blob permitem que você defina a camada de acesso no nível de objeto, independentemente da configuração de camada de acesso da conta. Os BLOBs em qualquer uma das três camadas de acesso (quente, fria ou arquivo) podem existir na mesma conta.

**Posso alterar a camada de acesso padrão do meu BLOB ou da conta de armazenamento do GPv2?**

Sim, você pode alterar a camada de conta padrão definindo o atributo de **camada de acesso** na conta de armazenamento. A alteração da camada de conta aplica-se a todos os objetos armazenados na conta que não têm um conjunto de camadas explícita (por exemplo, **quente (inferido)** ou **frio (inferido)** ). Alternar a camada de conta de operações de gravação quentes para frias (por 10.000) para todos os BLOBs sem uma camada de conjunto somente nas contas GPv2 e alternar de fria para quente incorre em operações de leitura (por 10.000) e de recuperação de dados (por GB) para todos os BLOBs no armazenamento de BLOBs e contas de GPv2.

**Posso configurar minha camada de acesso de conta padrão para arquivar?**

Nº Somente as camadas de acesso quente e fria podem ser definidas como a camada de acesso de conta padrão. A camada arquivo só pode ser definida no nível do objeto.

**Em quais regiões os níveis de acesso quente, frio e arquivo estão disponíveis?**

As camadas de acesso quente e fria juntamente com camadas no nível do blob estão disponíveis em todas as regiões. O armazenamento de arquivo estará disponível, inicialmente, apenas em regiões selecionadas. Para obter uma lista completa, consulte [Produtos do Azure por região](https://azure.microsoft.com/regions/services/).

**Os BLOBs na camada de acesso frio se comportam de forma diferente daqueles na camada de acesso quente?**

Os BLOBs na camada de acesso quente têm a mesma latência que os BLOBs nas contas de armazenamento GPv1, GPv2 e BLOB. Os BLOBs na camada de acesso frio têm uma latência semelhante (em milissegundos) como BLOBs nas contas de armazenamento GPv1, GPv2 e BLOB. Os BLOBs na camada de acesso de arquivamento têm várias horas de latência em contas de armazenamento de GPv1, GPv2 e blobs.

Os BLOBs na camada de acesso frio têm um SLA (nível de serviço) de disponibilidade ligeiramente menor do que os BLOBs armazenados na camada de acesso quente. Para obter mais informações, veja [SLA para armazenamento](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**As operações entre as camadas frequente, esporádica e de arquivo são a mesma?**

Todas as operações entre frequente e esporádica são 100% consistentes. Todas as operações de arquivamento válidas, incluindo getblobproperties, GetBlobMetadata, ListBlobs, SetBlobTier e DeleteBlob, são de 100% consistentes com a quente e a fria. Os dados de BLOB não podem ser lidos ou modificados enquanto estiverem na camada de arquivo até a operação; somente as operações de leitura de metadados de blob têm suporte durante o arquivamento.

**Ao reidratar um blob da camada de arquivo para a camada frequente ou esporádica, como vou saber quando a reidratação estiver concluída?**

Durante a reidratação, você pode usar a operação de obtenção de propriedades de blob para sondar o atributo **Status de Arquivo** para confirmar quando a alteração de camada estiver concluída. O status exibe "reidratação pendentes para camada quente" ou "reidratação pendente para camada fria" dependendo da camada de destino. Após a conclusão, a propriedade status do arquivo é removida, e a propriedade de blob de **Nível de Acesso** reflete a nova camada frequente ou esporádica.  

**Depois de definir o nível de um blob, quando vai começar a ser cobrada a taxa apropriada?**

Cada blob é sempre cobrado de acordo com a camada indicada pelo propriedade **Camada de Acesso** do blob. Quando você define uma nova camada para um blob, a propriedade **Camada de acesso** reflete imediatamente a nova camada para todas as transições. No entanto, reidratar um blob da camada dos arquivos para uma camada frequente ou esporádica pode levar várias horas. Nesse caso, são cobradas tarifas de arquivos até a conclusão da reidratação, ponto no qual a propriedade **Camada de Acesso** reflete a nova camada. Nesse ponto, você é cobrado por esse blob na taxa do armazenamento frequente ou esporádico.

**Como determino se vou incorrer um custo de exclusão antecipada ao excluir ou tirar um blob da camada esporádica ou de arquivo?**

Qualquer blob que for excluído ou tirado da camada esporádica (somente para contas de GPv2) ou de arquivo morto antes de 30 e 180 dias respectivamente incorrerá em uma carga de exclusão antecipada proporcional. É possível determinar quanto tempo um blob está na camada esporádica ou de arquivos verificando a propriedade **Hora da Alteração da Camada de Acesso** do blob, que fornece um selo da última alteração da camada. Se a camada do blob nunca foi alterada, você poderá verificar a propriedade de blob **modificada pela última vez** . Para obter mais informações, consulte [exclusão antecipada de frio e arquivo](#cool-and-archive-early-deletion).

**Quais ferramentas e SDKs do Azure dão suporte a armazenamento de camada no nível do blob e de arquivo?**

As ferramentas do portal do Azure, do PowerShell e da CLI e as bibliotecas de cliente .NET, Java, Python e Node.js, todas dão suporte ao armazenamento de camada no nível do blob e de arquivo.  

**Qual a quantidade de dados que posso armazenar nas camadas frequente, esporádica ou de arquivo?**

O armazenamento de dados junto com outros limites é definido no nível da conta e não por camada de acesso. Portanto, é possível optar por usar todo o seu limite em uma camada ou em todas as três camadas. Para saber mais, confira [Metas de desempenho e escalabilidade do Armazenamento do Azure](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Próximas etapas

### <a name="evaluate-hot-cool-and-archive-in-gpv2-and-blob-storage-accounts"></a>Avaliar a quente, a frio e o arquivo em contas de armazenamento de BLOBs e GPv2

[Verificar a disponibilidade de frequente, esporádica e de arquivo por região](https://azure.microsoft.com/regions/#services)

[Gerenciar o ciclo de vida de armazenamento de BLOBs do Azure](storage-lifecycle-management-concepts.md)

[Avaliar o uso de suas contas de armazenamento atuais, habilitando as métricas do Armazenamento do Azure](../common/storage-enable-and-view-metrics.md)

[Verificar o preço de frequente, esporádica e de arquivo em contas de Armazenamento de Blobs e GPv2 por região](https://azure.microsoft.com/pricing/details/storage/)

[Verificar os preços de transferências de dados](https://azure.microsoft.com/pricing/details/data-transfers/)

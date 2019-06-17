---
title: Copiar dados do Azure Data Lake Storage Gen1 para Gen2 com o Azure Data Factory
description: Usar o Azure Data Factory para copiar dados do Azure Data Lake Storage Gen1 para Gen2
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: jingwang
ms.openlocfilehash: d13dad6e87bd6c821b497138ad75d7ae2b9a052d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068984"
---
# <a name="copy-data-from-azure-data-lake-storage-gen1-to-gen2-with-azure-data-factory"></a>Copiar dados do Azure Data Lake Storage Gen1 para Gen2 com o Azure Data Factory

Gen2 de armazenamento do Azure Data Lake é um conjunto de recursos dedicado para análise de big data está incorporada [armazenamento de BLOBs do Azure](../storage/blobs/storage-blobs-introduction.md). Você pode usá-lo para fazer interface com seus dados usando ambos os paradigmas de armazenamento de sistema e o objeto de arquivo.

Se você usar o Azure Data Lake armazenamento Gen1 no momento, você pode avaliar Gen2 de armazenamento do Azure Data Lake, copiando os dados do Data Lake armazenamento Gen1 para Gen2 usando o Azure Data Factory.

O Azure Data Factory é um serviço de integração de dados baseado em nuvem completamente gerenciado. Você pode usar o serviço para popular o lago com os dados de um conjunto avançado de locais e armazenamentos de dados de baseado em nuvem e economizam tempo quando você cria suas soluções de análise. Para obter uma lista de conectores com suporte, consulte a tabela de [armazenamentos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).

O Azure Data Factory oferece uma solução de movimentação de dados gerenciados de expansão. Por causa da arquitetura de expansão do Data Factory, ele pode incluir dados em uma alta taxa de transferência. Para obter mais informações, consulte [desempenho da atividade de cópia](copy-activity-performance.md).

Este artigo mostra como usar a ferramenta Copiar dados do Data Factory para copiar dados do Azure Data Lake armazenamento Gen1 no armazenamento do Azure Data Lake Gen2. Você pode seguir as etapas semelhantes para copiar dados de outros tipos de armazenamentos de dados.

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.
* Conta do Azure Data Lake Store Gen1 contendo dados.
* Conta de armazenamento do Azure com o Data Lake armazenamento Gen2 habilitado. Se você não tiver uma conta de armazenamento [criar uma conta](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM).

## <a name="create-a-data-factory"></a>Criar um data factory

1. No menu à esquerda, selecione **criar um recurso** > **dados + análise** > **Data Factory**.
   
   ![No novo painel de seleção de Data Factory](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. Sobre o **novo data factory** , forneça valores para os campos que são mostrados na imagem a seguir: 
      
   ![Nova página do Data factory](./media/load-azure-data-lake-storage-gen2-from-gen1/new-azure-data-factory.png)
 
    * **Nome**: Insira um nome globalmente exclusivo para o Azure Data Factory. Se você receber o erro "Nome do Data factory \"LoadADLSDemo\" não está disponível," digite um nome diferente para o data factory. Por exemplo, use o nome _**seunome**_ **ADFTutorialDataFactory**. Crie o data factory novamente. Para ver as regras de nomenclatura de artefatos do Data Factory, confira [Regras de nomenclatura do Data Factory](naming-rules.md).
    * **Assinatura**: Selecione a assinatura do Azure para criar o Data Factory. 
    * **Grupo de recursos**: Selecione um grupo de recursos existente na lista suspensa. Você também pode selecionar o **criar novo** opção e digite o nome de um grupo de recursos. Para saber mais sobre grupos de recursos, confira [Usar grupos de recursos para gerenciar recursos do Azure](../azure-resource-manager/resource-group-overview.md). 
    * **Versão**: Selecione **V2**.
    * **Localização**: Selecione o local para o data factory. Somente os locais com suporte são exibidos na lista suspensa. Os armazenamentos de dados que são usados pelo data factory podem ser em outros locais e regiões. 

3. Selecione **Criar**.
4. Após a criação, vá para seu data factory. Você verá a home page **Data Factory** conforme mostrado na imagem a seguir: 
   
   ![Página inicial do data factory](./media/load-azure-data-lake-storage-gen2-from-gen1/data-factory-home-page.png)

5. Selecione o **autor & Monitor** lado a lado para iniciar o aplicativo de integração de dados em uma guia separada.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Carregar dados no Azure Data Lake Store Gen2

1. Sobre o **começar** página, selecione o **copiar dados** lado a lado para iniciar a ferramenta Copiar dados. 

   ![Bloco ferramenta Copy data](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-tile.png)
2. Sobre o **propriedades** , especifique **CopyFromADLSGen1ToGen2** para o **nome da tarefa** campo. Selecione **Avançar**.

    ![Página Propriedades](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-properties-page.png)
3. Sobre o **armazenamento de dados de origem** página, selecione **+ criar nova conexão**.

    ![Página Armazenamento de dados de origem](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page.png)
    
4. Selecione **Gen1 de armazenamento do Azure Data Lake** na Galeria do conector e selecione **continuar**.
    
    ![Página de armazenamento de dados de origem do Azure Data Lake Store Gen1](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page-adls-gen1.png)
    
5. Sobre o **conexão de especificar a Gen1 de armazenamento do Azure Data Lake** página, siga estas etapas:

   a. Selecione o Data Lake Storage Gen1 para o nome da conta e especifique ou valide o **Locatário**.
  
   b. Selecione **Testar conexão** para validar as configurações. Em seguida, selecione **Concluir**.
  
   c. Você ver que uma conexão nova foi criado. Selecione **Avançar**.
   
   > [!IMPORTANT]
   > Neste passo a passo, você pode usar uma identidade gerenciada para recursos do Azure para autenticar seu Gen1 de armazenamento do Azure Data Lake. Para conceder as permissões apropriadas no Azure Data Lake armazenamento Gen1 de identidade gerenciada, siga [estas instruções](connector-azure-data-lake-store.md#managed-identity).
   
   ![Especifique a conta do Azure Data Lake Store Gen1](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen1-account.png)
      
6. Sobre o **escolha o arquivo de entrada ou a pasta** página, navegue até a pasta e o arquivo que você deseja copiar. Selecione o arquivo ou pasta e, em seguida, selecione **Choose**.

    ![Escolha a pasta ou arquivo de entrada](./media/load-azure-data-lake-storage-gen2-from-gen1/choose-input-folder.png)

7. Especifique o comportamento de cópia, selecionando o **copiar arquivos recursivamente** e **cópia binária** opções. Selecione **Avançar**.

    ![Especifique a pasta de saída](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-binary-copy.png)
    
8. Sobre o **armazenamento de dados de destino** página, selecione **+ criar nova conexão** > **Gen2 de armazenamento do Azure Data Lake**  >   **Continuar**.

    ![Página Armazenamento de dados de destino](./media/load-azure-data-lake-storage-gen2-from-gen1/destination-data-storage-page.png)

9. Sobre o **conexão especificar o Azure Data Lake armazenamento Gen2** página, siga estas etapas:

   a. Selecione sua conta de Data Lake armazenamento Gen2 com capacidade do **nome da conta de armazenamento** lista suspensa.
   
   b. Selecione **Concluir** para criar a conexão. Em seguida, selecione **Avançar**.
   
   ![Especificar a conta do Azure Data Lake Storage Gen2](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-account.png)

10. Sobre o **escolha o arquivo de saída ou a pasta** página, insira **copyfromadlsgen1** como o nome da pasta de saída e selecione **próxima**. Data Factory cria correspondente do Azure Data Lake armazenamento Gen2 sistema de arquivos e subpastas durante a cópia se não existirem.

    ![Especifique a pasta de saída](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-path.png)

11. Sobre o **configurações** página, selecione **próxima** para usar as configurações padrão.

12. Sobre o **resumo** página, examine as configurações e selecione **próxima**.

    ![Página Resumo](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-summary.png)
13. Sobre o **página de implantação**, selecione **Monitor** para monitorar o pipeline.

    ![Página Implantação](./media/load-azure-data-lake-storage-gen2-from-gen1/deployment-page.png)
14. Observe que a guia **Monitor** à esquerda é selecionada automaticamente. A coluna **Ações** inclui links para exibir detalhes da execução da atividade e executar o pipeline novamente.

    ![Monitorar execuções de pipeline](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-pipeline-runs.png)

15. Para exibir as execuções de atividade associadas com a execução do pipeline, selecione o link **Exibir as Execuções de Atividade** na coluna **Ações**. Há apenas uma atividade (atividade de cópia) no pipeline, assim, você vê apenas uma entrada. Para voltar à exibição de execuções de pipeline, selecione o link **Pipelines** na parte superior. Selecione **Atualizar** para atualizar a lista. 

    ![Monitorar execuções de atividade](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-runs.png)

16. Para monitorar os detalhes de execução de cada atividade de cópia, selecione o link **Detalhes** (imagem de óculos) em **Ações** na exibição de monitoramento de atividade. Você pode monitorar detalhes como o volume de dados copiados da origem para o coletor, taxa de transferência de dados, as etapas de execução com duração correspondente e as configurações usadas.

    ![Monitorar detalhes da execução da atividade](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-run-details.png)

17. Verifique se que os dados são copiados em sua conta de armazenamento do Azure Data Lake Gen2.

## <a name="best-practices"></a>Práticas recomendadas

Para avaliar a atualização do Gen1 de armazenamento do Azure Data Lake para o armazenamento do Azure Data Lake Gen2 em geral, consulte [atualizar suas soluções de análise de big data do Gen1 de armazenamento do Azure Data Lake para o armazenamento do Azure Data Lake Gen2](../storage/blobs/data-lake-storage-upgrade.md). As seções a seguir apresentam as práticas recomendadas para uso da fábrica de dados para uma atualização de dados do Data Lake armazenamento Gen1 para Data Lake armazenamento Gen2.

### <a name="data-partition-for-historical-data-copy"></a>Partição de dados para cópia de dados históricos

- Se o tamanho total dos dados no Data Lake armazenamento Gen1 é menor que 30 TB e o número de arquivos for menor que 1 milhão, você pode copiar todos os dados em uma execução de atividade de cópia única.
- Se você tiver uma quantidade maior de dados a serem copiados, ou você quiser flexibilidade para gerenciar a migração de dados em lotes e fazer com que cada um deles concluída dentro de um período de tempo específico, particione os dados. Também é o particionamento reduz o risco de qualquer problema inesperado.

Use uma prova de conceito para verificar a solução de ponta a ponta e testar a taxa de transferência de cópia em seu ambiente. Principais etapas de prova de conceito: 

1. Crie um pipeline do Data Factory com uma atividade de cópia única para copiar vários TBs de dados do Data Lake armazenamento Gen1 para Data Lake armazenamento Gen2 para obter uma linha de base de desempenho de cópia. Iniciar com [unidades de integração de dados (DIUs)](copy-activity-performance.md#data-integration-units) como 128. 
2. Com base na taxa de transferência de cópia que obter na etapa 1, calcule o tempo estimado necessário para a migração de dados inteiro. 
3. (Opcional) Criar uma tabela de controle e definir o filtro de arquivo para particionar os arquivos a serem migrados. É a forma de particionar os arquivos: 

    - Partição por nome de pasta ou nome de pasta com um filtro curinga. É recomendável que esse método.
    - Partição por um arquivo de hora da última modificação.

### <a name="network-bandwidth-and-storage-io"></a>S de armazenamento e largura de banda de rede 

Você pode controlar a simultaneidade de trabalhos de cópia do Data Factory que ler dados do Data Lake armazenamento Gen1 e gravar dados no Data Lake armazenamento Gen2. Dessa forma, você pode gerenciar o uso em que o armazenamento e/s para não afetar o trabalho de negócios normais no Data Lake armazenamento Gen1 durante a migração.

### <a name="permissions"></a>Permissões 

No Data Factory, o [conector do Data Lake armazenamento Gen1](connector-azure-data-lake-store.md) identidade principal e gerenciada para as autenticações de recursos do Azure dá suporte de serviço. O [conector do Data Lake armazenamento Gen2](connector-azure-data-lake-storage.md) dá suporte a conta chave, entidade de serviço e uma identidade gerenciada para autenticações de recursos do Azure. Para tornar o Data Factory capaz de navegar e copiar todos os arquivos ou acessar listas de controle (ACLs) que você precisa, conceder alta permissões suficientes para a conta que você fornece para acessar, ler, ou escrever todos os arquivos e definir ACLs se você optar por. Conceda uma função de superusuário ou proprietário durante o período de migração. 

### <a name="preserve-acls-from-data-lake-storage-gen1"></a>Preservar as ACLs do Data Lake Storage Gen1

Se você deseja replicar as ACLs, juntamente com os arquivos de dados quando você atualiza do Data Lake armazenamento Gen1 para Gen2 de armazenamento do Data Lake, consulte [preservar ACLs do Data Lake armazenamento Gen1](connector-azure-data-lake-storage.md#preserve-acls-from-data-lake-storage-gen1). 

### <a name="incremental-copy"></a>Cópia incremental 

Você pode usar várias abordagens para carregar somente os arquivos novos ou atualizados do Data Lake armazenamento Gen1:

- Carrege arquivos novos ou atualizados por nome de arquivo ou pasta de particionada de tempo. Um exemplo é de 2019/05/13 / *.
- Carrege arquivos novos ou atualizados por LastModifiedDate.
- Identificar arquivos novos ou atualizados por qualquer ferramenta de terceiros ou uma solução. Em seguida, passe o nome de arquivo ou pasta para o pipeline do Data Factory por meio do parâmetro ou um arquivo ou tabela. 

A frequência apropriada para fazer o carregamento incremental depende do número total de arquivos no Azure Data Lake armazenamento Gen1 e o volume de arquivos novos ou atualizados para ser carregado sempre. 

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Visão geral da atividade de cópia](copy-activity-overview.md)
> [conector do Azure Data Lake armazenamento Gen1](connector-azure-data-lake-store.md)
> [conector do armazenamento do Azure Data Lake Gen2](connector-azure-data-lake-storage.md)
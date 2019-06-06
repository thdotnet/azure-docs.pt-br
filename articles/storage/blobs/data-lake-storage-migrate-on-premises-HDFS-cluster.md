---
title: Use o Azure Data Box para migrar dados do local HDFS armazenar no armazenamento do Azure
description: Migrar dados de um repositório do HDFS local para o armazenamento do Azure
services: storage
author: normesta
ms.service: storage
ms.date: 06/05/2019
ms.author: normesta
ms.topic: article
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 9a42135df38cde91cc6626a3f7d0328334af0a5d
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729058"
---
# <a name="use-azure-data-box-to-migrate-data-from-an-on-premises-hdfs-store-to-azure-storage"></a>Usar o Azure Data Box para migrar dados de um repositório do HDFS local para o armazenamento do Azure

Você pode migrar dados de um repositório do HDFS no local do seu cluster do Hadoop no armazenamento do Azure (armazenamento de BLOBs ou Data Lake armazenamento Gen2) usando um dispositivo Data Box. Você pode escolher entre uma caixa de 80 TB de dados ou uma pesada de caixa de dados de 770 TB.

Este artigo ajuda você a concluir essas tarefas:

:heavy_check_mark: Copie seus dados em uma caixa de dados ou um dispositivo de caixa de dados pesados.

:heavy_check_mark: Envie o dispositivo para a Microsoft.

:heavy_check_mark: Mova os dados em sua conta de armazenamento do Data Lake armazenamento Gen2.

## <a name="prerequisites"></a>Pré-requisitos

Você precisa dessas coisas para concluir a migração.

* Conta de armazenamento do Azure que **não** tem namespaces hierárquicos habilitados nele.

* Se você deseja usar a conta de armazenamento do Azure Data Lake Gen2 (uma conta de armazenamento que **faz** tem namespaces hierárquicos habilitados nele), em seguida, você talvez queira criá-lo neste momento.

* Um cluster de Hadoop local que contém os dados de origem.

* Uma [dispositivo Azure Data Box](https://azure.microsoft.com/services/storage/databox/).

    - [Solicitar o Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-ordered) ou [Data Box pesado](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-ordered). Durante a ordenação de seu dispositivo, lembre-se de escolher uma conta de armazenamento que **não** tem namespaces hierárquicos habilitados nele. Isso ocorre porque os dispositivos da caixa de dados ainda não suportam a ingestão direto no armazenamento do Azure Data Lake Gen2. Você precisará copiar para uma conta de armazenamento e, em seguida, fazer uma segunda cópia para a conta do ADLS Gen2. As instruções para isso são fornecidas nas etapas abaixo.
    - Cabo e conecte-se sua [Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) ou [caixa de dados pesados](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-set-up) a uma rede local.

Se você estiver pronto, vamos começar.

## <a name="copy-your-data-to-a-data-box-device"></a>Copiar os dados para um dispositivo Data Box

Para copiar os dados do repositório local de HDFS em um dispositivo Data Box, você vai configurar algumas coisas e, em seguida, usar o [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) ferramenta.

Se a quantidade de dados que você está copiando é mais do que a capacidade de uma única caixa de dados ou que um único nó na caixa de dados pesados, divida seu conjunto de dados em tamanhos que cabem em seus dispositivos.

Siga estas etapas para copiar dados via o REST APIs do Blob/armazenamento de objeto para o dispositivo Data Box. A interface REST API fará o dispositivo aparecer como um repositório de HDFS para seu cluster. 


1. Antes de copiar os dados por meio de REST, identifique os primitivos de segurança e conexão para conectar-se à interface REST na caixa de dados ou dados caixa pesada. Entrar para a interface do usuário da caixa de dados da web local e vá para **conectar e copiar** página. O armazenamento do Azure em relação a conta para seu dispositivo, em **configurações de acesso**, localize e selecione **REST**.

    ![Página de "Conectar e copiar"](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connect-rest.png)

2. Na conta de armazenamento de acesso e a caixa de diálogo dados de carregamento, copie o **ponto de extremidade de serviço Blob** e o **chave de conta de armazenamento**. Do ponto de extremidade de serviço blob, omita o `https://` e a barra à direita.

    Nesse caso, é o ponto de extremidade: `https://mystorageaccount.blob.mydataboxno.microsoftdatabox.com/`. A parte de host do URI que você usará é: `mystorageaccount.blob.mydataboxno.microsoftdatabox.com`. Por exemplo, consulte como [conectar ao REST sobre http](/azure/databox/data-box-deploy-copy-data-via-rest). 

     ![Caixa de diálogo "Acessar a conta de armazenamento e carregar dados"](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connection-string-http.png)

3. Adicionar o ponto de extremidade e o endereço IP de nó de caixa de dados ou dados caixa pesada para `/etc/hosts` em cada nó.

    ```    
    10.128.5.42  mystorageaccount.blob.mydataboxno.microsoftdatabox.com
    ```
    Se você estiver usando algum outro mecanismo para DNS, você deve garantir que a caixa de dados do ponto de extremidade pode ser resolvido.
    
4. Defina uma variável do shell `azjars` para apontar para o `hadoop-azure` e o `microsoft-windowsazure-storage-sdk` arquivos jar. Esses arquivos estão sob o diretório de instalação do Hadoop (você pode verificar se esses arquivos existem usando este comando `ls -l $<hadoop_install_dir>/share/hadoop/tools/lib/ | grep azure` onde `<hadoop_install_dir>` é o diretório onde você instalou o Hadoop) usa os caminhos completos. 
    
    ```
    # azjars=$hadoop_install_dir/share/hadoop/tools/lib/hadoop-azure-2.6.0-cdh5.14.0.jar
    # azjars=$azjars,$hadoop_install_dir/share/hadoop/tools/lib/microsoft-windowsazure-storage-sdk-0.6.0.jar
    ```

5. Crie o contêiner de armazenamento que você deseja usar para cópia de dados. Você também deve especificar uma pasta de destino como parte desse comando. Isso pode ser uma pasta de destino fictício neste momento.

    ```
    # hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -mkdir -p  wasb://[container_name]@[blob_service_endpoint]/[destination_folder]
    ```

6. Execute um comando de lista para garantir que seu contêiner e a pasta foram criadas.

    ```
    # hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -ls -R  wasb://[container_name]@[blob_service_endpoint]/
    ```

7. Copie dados do HDFS do Hadoop para o armazenamento de blobs de caixa de dados no contêiner que você criou anteriormente. Se a pasta que você estiver copiando para não for encontrada, o comando cria automaticamente.

    ```
    # hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -strategy dynamic -m 4 -update \
    [source_directory] \
           wasb://[container_name]@[blob_service_endpoint]/[destination_folder]       
    ```
   O `-libjars` opção é usada para fazer a `hadoop-azure*.jar` e o dependente `azure-storage*.jar` arquivos disponíveis para `distcp`. Isso já pode ocorrer por alguns clusters.
   
   A exemplo a seguir mostra como o `distcp` comando é usado para copiar dados.
   
   ```
   # hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.mystorageaccount.blob.mydataboxno.microsoftdatabox.com=myaccountkey \
    -strategy dynamic -m 4 -update \
    /data/testfiles \
    wasb://hdfscontainer@mystorageaccount.blob.mydataboxno.microsoftdatabox.com/testfiles
   ```
  
Para melhorar a velocidade de cópia:
- Tente alterar o número de mapeadores. (O exemplo acima usa `m` = 4 mapeadores.)
- Tente executar vários `distcp` em paralelo.
- Lembre-se de que arquivos grandes executam melhor do que arquivos pequenos.
    
## <a name="ship-the-data-box-to-microsoft"></a>Envie a caixa de dados à Microsoft

Siga estas etapas para preparar e enviar o dispositivo Data Box para a Microsoft.

1. Depois que a cópia de dados for concluída, execute:
    
    - [Preparar para o envio em sua caixa de dados ou dados caixa pesada](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest).
    - Após concluir a preparação do dispositivo, baixe os arquivos BOM. Você usa esses BOM ou arquivos posteriormente para verificar os dados carregados para o Azure de manifesto. 
    - Desligar o dispositivo e remova os cabos.
2.  Agende uma retirada com a UPS. Siga as instruções para:

    - [Envie o Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up) 
    - [Envie sua caixa de dados com uso intenso](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-picked-up).
3.  Depois que a Microsoft recebe seu dispositivo, ele está conectado à rede de datacenter e os dados são carregados para a conta de armazenamento especificada (com namespaces hierárquicos desabilitados) quando você fez o pedido de dispositivo. Verifique se que todos os seus dados é carregado no Azure em relação aos arquivos BOM. Agora você pode mover esses dados para uma conta de armazenamento do Data Lake armazenamento Gen2.


## <a name="move-the-data-onto-your-data-lake-storage-gen2-storage-account"></a>Mover os dados em sua conta de armazenamento do Data Lake armazenamento Gen2

Essa etapa é necessária se você estiver usando o Azure Data Lake armazenamento Gen2 como repositório de dados. Se você estiver usando apenas uma conta de armazenamento de blob sem namespace hierárquico como seu armazenamento de dados, você não precisará realizar esta etapa.

Você pode fazer isso de duas maneiras.

- Use [Azure Data Factory para mover dados para o ADLS Gen2](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2). Você terá que especificar **armazenamento de BLOBs do Azure** como a origem.

- Use seu cluster do Hadoop baseada no Azure. Você pode executar esse comando DistCp:

    ```bash
    hadoop distcp -Dfs.azure.account.key.{source_account}.dfs.windows.net={source_account_key} abfs://{source_container} @{source_account}.dfs.windows.net/[path] abfs://{dest_container}@{dest_account}.dfs.windows.net/[path]
    ```

Este comando copia os dados e metadados de sua conta de armazenamento em sua conta de armazenamento do Data Lake armazenamento Gen2.

## <a name="next-steps"></a>Próximas etapas

Saiba como o Data Lake armazenamento Gen2 funciona com clusters do HDInsight. Consulte [Usar o Azure Data Lake Storage Gen2 com clusters de HDInsight do Azure](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md).

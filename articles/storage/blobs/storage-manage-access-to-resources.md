---
title: Habilitar o acesso de leitura público para contêineres e blobs no Armazenamento de Blobs do Azure | Microsoft Docs
description: Saiba como disponibilizar os contêineres e blobs para acesso anônimo e como acessá-los programaticamente.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/30/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: 6293fc84969c4e246c05da4482f76142263db230
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68985554"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Gerenciar o acesso de leitura anônimo aos contêineres e blobs

Você pode habilitar o acesso de leitura anônimo, público para um contêiner e seus blobs no Armazenamento de Blobs do Azure. Ao fazer isso, você pode conceder acesso somente leitura a esses recursos sem compartilhar a chave da conta e sem exigir uma SAS (assinatura de acesso compartilhado).

O acesso de leitura público é ideal para cenários em que você quer que determinados blobs sempre estejam disponíveis para acesso de leitura anônimo. Para ter um controle mais refinado, você pode criar uma assinatura de acesso compartilhado. As assinaturas de acesso compartilhado permitem fornecer acesso restrito usando permissões diferentes, por um período específico. Para saber mais sobre como criar assinaturas de acesso compartilhado, veja [Usando SAS (Assinaturas de Acesso Compartilhado) no Armazenamento do Azure](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Conceder permissões de usuários anônimos a contêineres e blobs

Por padrão, um contêiner e todos os BLOBs dentro dele podem ser acessados somente por um usuário que tenha recebido as permissões apropriadas. Para conceder aos usuários anônimos acesso de leitura a um contêiner e seus BLOBs, você pode definir o nível de acesso público do contêiner. Quando você concede acesso público a um contêiner, os usuários anônimos podem ler BLOBs dentro de um contêiner publicamente acessível sem autorizar a solicitação.

Você pode configurar um contêiner com as seguintes permissões:

* **Sem acesso de leitura público:** O contêiner e seus BLOBs podem ser acessados somente pelo proprietário da conta de armazenamento. Esse é o padrão para todos os novos contêineres.
* **Acesso de leitura público somente para BLOBs:** Os BLOBs dentro do contêiner podem ser lidos por solicitação anônima, mas os dados do contêiner não estão disponíveis. Os clientes não podem enumerar os blobs no contêiner.
* **Acesso de leitura público para contêiner e seus BLOBs:** Todos os dados de contêiner e BLOB podem ser lidos por solicitação anônima. Os clientes podem enumerar os blobs no contêiner por meio solicitação anônima, mas não podem enumerar os contêineres na conta de armazenamento.

Você pode usar os seguintes recursos para definir permissões de contêiner:

* [Portal do Azure](https://portal.azure.com)
* [PowerShell do Azure](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [CLI do Azure](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* De modo programático, usando uma das bibliotecas de cliente de armazenamento ou a API REST

### <a name="set-container-public-access-level-in-the-azure-portal"></a>Definir o nível de acesso público do contêiner no portal do Azure

No [portal do Azure](https://portal.azure.com), você pode atualizar o nível de acesso público para um ou mais contêineres:

1. Navegue até sua conta de armazenamento no portal do Azure.
1. Em **serviço blob** na folha do menu, selecione **BLOBs**.
1. Selecione os contêineres para os quais você deseja definir o nível de acesso público.
1. Use o botão **alterar nível de acesso** para exibir as configurações de acesso público.
1. Selecione o nível de acesso público desejado na lista suspensa **nível de acesso público** e clique no botão OK para aplicar a alteração aos contêineres selecionados.

A captura de tela a seguir mostra como alterar o nível de acesso público para os contêineres selecionados.

![Captura de tela mostrando como definir o nível de acesso público no portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

> [!NOTE]
> Você não pode alterar o nível de acesso público para um blob individual. O nível de acesso público é definido somente no nível de contêiner.

### <a name="set-container-public-access-level-with-net"></a>Definir o nível de acesso público do contêiner com .NET

A fim de definir permissões para um contêiner usando C# e a Biblioteca de Cliente de Armazenamento para .NET, primeiro recupere as permissões existentes do contêiner chamando o método **GetPermissions**. Em seguida, defina a propriedade **PublicAccess** para o objeto **BlobContainerPermissions** que é retornado pelo método **GetPermissions**. Por fim, chame o método **SetPermissions** com as permissões atualizadas.

O exemplo a seguir define as permissões do contêiner para acesso de leitura público completo. Para definir as permissões como acesso de leitura público somente para os blobs, defina a propriedade **PublicAccess** como **BlobContainerPublicAccessType.Blob**. Para remover todas as permissões para usuários anônimos, defina a propriedade como **BlobContainerPublicAccessType.Off**.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Acessar contêineres e blobs anonimamente

Um cliente que acessa contêineres e blobs anonimamente pode usar construtores que não necessitam de credenciais. Os exemplos a seguir mostram algumas maneiras diferentes de referenciar contêineres e blobs anonimamente.

### <a name="create-an-anonymous-client-object"></a>Criar um objeto de cliente anônimo

Você pode criar um novo objeto de cliente de serviço para acesso anônimo fornecendo o ponto de extremidade de armazenamento de BLOBs para a conta. No entanto, você também precisa saber o nome de um contêiner dessa conta que esteja disponível para acesso anônimo.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob storage endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a>Fazer referência a um contêiner anonimamente

Se tiver a URL para um contêiner que está disponível de forma anônima, você pode usá-lo para fazer referência diretamente ao contêiner.

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a>Fazer referência a um blob anonimamente

Se tiver a URL para um blob que está disponível para acesso anônimo, você pode fazer referência ao blob diretamente usando esta URL:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a>Recursos disponíveis para usuários anônimos

A tabela a seguir mostra quais operações podem ser chamadas anonimamente quando um contêiner é configurado para acesso público.

| Operação REST | Acesso de leitura público ao contêiner | Acesso de leitura público apenas aos blobs |
| --- | --- | --- |
| Listar contêineres | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Create Container | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Get Container Properties | Solicitações anônimas permitidas | Somente solicitações autorizadas |
| Get Container Metadata | Solicitações anônimas permitidas | Somente solicitações autorizadas |
| Set Container Metadata | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Get Container ACL | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Set Container ACL | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Delete Container | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Listar Blobs | Solicitações anônimas permitidas | Somente solicitações autorizadas |
| Put Blob | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Get Blob | Solicitações anônimas permitidas | Solicitações anônimas permitidas |
| Get Blob Properties | Solicitações anônimas permitidas | Solicitações anônimas permitidas |
| Set Blob Properties | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Obter Metadados do Blob | Solicitações anônimas permitidas | Solicitações anônimas permitidas |
| Set Blob Metadata | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Put Block | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Obter lista de blocos (somente blocos confirmados) | Solicitações anônimas permitidas | Solicitações anônimas permitidas |
| Obter lista de blocos (somente blocos não confirmados ou todos os blocos) | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Put Block List | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Delete Blob | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Copiar blob | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Blob de instantâneo | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Lease Blob | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Put Page | Somente solicitações autorizadas | Somente solicitações autorizadas |
| Get Page Ranges | Solicitações anônimas permitidas | Solicitações anônimas permitidas |
| Acrescentar Blob | Somente solicitações autorizadas | Somente solicitações autorizadas |

## <a name="next-steps"></a>Próximas etapas

* [Autorização para os serviços de armazenamento do Azure](https://docs.microsoft.com/rest/api/storageservices/authorization-for-the-azure-storage-services)
* [Usando assinaturas de acesso compartilhado (SAS)](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
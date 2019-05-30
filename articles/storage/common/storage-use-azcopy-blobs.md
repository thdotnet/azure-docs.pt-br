---
title: Transferir dados do armazenamento de BLOBs do Azure usando AzCopy v10 | Microsoft Docs
description: Este artigo contém uma coleção de AzCopy exemplo comandos que ajudam você a criam contêineres, copiam arquivos e sincronizar pastas entre sistemas de arquivos locais e contêineres.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 98e33f838ee9b6f506bf1dc01e1dd61ad587aa05
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299402"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>Transferir dados com o armazenamento de BLOBs e AzCopy

O AzCopy é um utilitário de linha de comando que você pode usar para copiar dados para, de ou entre contas de armazenamento. Este artigo contém comandos de exemplo que funcionam com o armazenamento de BLOBs.

## <a name="get-started"></a>Introdução

Consulte a [começar com o AzCopy](storage-use-azcopy-v10.md) artigo para baixar o AzCopy e saiba mais sobre as maneiras que você pode fornecer as credenciais de autorização para o serviço de armazenamento.

> [!NOTE]
> Os exemplos neste artigo presumem que você foi autenticado sua identidade usando o `AzCopy login` comando. O AzCopy, em seguida, usa sua conta do Azure AD para autorizar o acesso aos dados no armazenamento de BLOBs.
>
> Se você preferir usar um token SAS para autorizar o acesso a dados de blob, você pode acrescentar esse token para a URL de recurso em cada comando AzCopy.
>
> Por exemplo: `https://<storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>"`.

## <a name="create-a-container"></a>Criar um contêiner

Você pode usar o AzCopy `make` comando para criar um contêiner. Os exemplos nesta seção criam um contêiner chamado `mycontainer`.

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy make "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>` |
| **Exemplo** | `azcopy make "https://mystorageaccount.blob.core.windows.net/mycontainer"` |
| **Exemplo** (namespace hierárquico) | `azcopy make "https://mystorageaccount.dfs.core.windows.net/mycontainer"` |

## <a name="upload-files"></a>Carregar arquivos

Você pode usar o AzCopy `copy` comando para carregar arquivos e pastas do seu computador local.

Esta seção contém os seguintes exemplos:

> [!div class="checklist"]
> * Carregar um arquivo
> * Carregar uma pasta
> * Carregar arquivos usando curingas

> [!NOTE]
> O AzCopy não calcular e armazenar o código de hash md5 do arquivo automaticamente. Se você quiser AzCopy para fazer isso, em seguida, acrescente o `--put-md5` sinalizador para cada comando de cópia. Dessa forma, quando o blob é baixado, o AzCopy calcula um hash MD5 para dados baixados e verifica que o hash MD5 armazenado no blob de `Content-md5` propriedade corresponda ao hash calculado.

### <a name="upload-a-file"></a>Carregar um arquivo

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>"` |
| **Exemplo** | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |
| **Exemplo** (namespace hierárquico) | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt"` |

> [!NOTE]
> Por padrão, o AzCopy carrega dados em blobs de blocos. Para carregar arquivos como Blobs de acréscimo ou Blobs de página, use o sinalizador `--blob-type=[BlockBlob|PageBlob|AppendBlob]`.

### <a name="upload-a-folder"></a>Carregar uma pasta

Este exemplo copia uma pasta (e todos os arquivos nessa pasta) para um contêiner de blob. O resultado é uma pasta no contêiner com o mesmo nome.

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Exemplo** | `azcopy copy "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Exemplo** (namespace hierárquico) | `azcopy copy "C:\myFolder" "https://mystorageaccount.dfs.core.windows.net/mycontainer" --recursive` |

Para copiar para uma pasta dentro do contêiner, especifique o nome da pasta em sua cadeia de caracteres de comando.

|    |     |
|--------|-----------|
| **Exemplo** | `azcopy copy "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder" --recursive` |
| **Exemplo** (namespace hierárquico) | `azcopy copy "C:\myFolder" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder" --recursive` |

Se você especificar o nome de uma pasta que não existe no contêiner, o AzCopy cria uma nova pasta com esse nome.

### <a name="upload-the-contents-of-a-folder"></a>Carregar o conteúdo de uma pasta

Você pode carregar o conteúdo de uma pasta sem copiar a pasta que contém em si, usando o símbolo de caractere curinga (*).

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "<local-folder-path>\*" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<folder-path>` |
| **Exemplo** | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder"` |
| **Exemplo** (namespace hierárquico) | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder"` |

> [!NOTE]
> Acrescente a `--recursive` sinalizador para carregar arquivos em todas as subpastas.

## <a name="download-files"></a>Baixar arquivos

Você pode usar o AzCopy `copy` comando para baixar blobs, pastas e contêineres em seu computador local.

Esta seção contém os seguintes exemplos:

> [!div class="checklist"]
> * Baixar um arquivo
> * Uma pasta de download
> * Baixar arquivos usando curingas

> [!NOTE]
> Se o `Content-md5` valor da propriedade de um blob contém um hash, o AzCopy calcula um hash MD5 para dados baixados e verifica que o hash MD5 armazenado no blob de `Content-md5` propriedade corresponda ao hash calculado. Se esses valores não corresponderem, o download falhar, a menos que você substituir esse comportamento por meio do acréscimo `--check-md5=NoCheck` ou `--check-md5=LogOnly` para o comando Copiar.

### <a name="download-a-file"></a>Baixar um arquivo

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>" "<local-file-path>"` |
| **Exemplo** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "C:\myFolder\myTextFile.txt"` |
| **Exemplo** (namespace hierárquico) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt" "C:\myFolder\myTextFile.txt"` |

### <a name="download-a-folder"></a>Uma pasta de download

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<folder-path>" "<local-folder-path>" --recursive` |
| **Exemplo** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder "C:\myFolder"  --recursive` |
| **Exemplo** (namespace hierárquico) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder "C:\myFolder"  --recursive` |

Este exemplo resulta em uma pasta chamada `C:\myFolder\myBlobFolder` que contém todos os arquivos baixados.

### <a name="download-the-contents-of-a-folder"></a>Baixar o conteúdo de uma pasta

Você pode baixar o conteúdo de uma pasta sem copiar a pasta que contém em si, usando o símbolo de caractere curinga (*).

> [!NOTE]
> Atualmente, esse cenário tem suporte apenas para contas que não têm um namespace hierárquico.

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-folder-path>/"` |
| **Exemplo** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder/*" "C:\myFolder"` |

> [!NOTE]
> Acrescente a `--recursive` sinalizador para baixar os arquivos em todas as subpastas.

## <a name="copy-blobs-between-storage-accounts"></a>Copiar blobs entre contas de armazenamento

Você pode usar o AzCopy para copiar blobs para outras contas de armazenamento. A operação de cópia é síncrona, portanto, quando o comando é retornado, que indica que todos os arquivos foram copiados.

> [!NOTE]
> Atualmente, esse cenário tem suporte apenas para contas que não têm um namespace hierárquico.

O AzCopy usa a [colocar bloco From URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API, portanto, os dados são copiados diretamente entre os servidores de armazenamento. Essas operações de cópia não usam a largura de banda de rede do seu computador.

Esta seção contém os seguintes exemplos:

> [!div class="checklist"]
> * Copiar um blob para outra conta de armazenamento
> * Copiar uma pasta para outra conta de armazenamento
> * Copiar um contêiner para outra conta de armazenamento
> * Copiar todos os contêineres, pastas e arquivos para outra conta de armazenamento

### <a name="copy-a-blob-to-another-storage-account"></a>Copiar um blob para outra conta de armazenamento

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>"` |
| **Exemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |

### <a name="copy-a-folder-to-another-storage-account"></a>Copiar uma pasta para outra conta de armazenamento

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<folder-path>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<folder-path>" --recursive` |
| **Exemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobFolder" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myBlobFolder" --recursive` |

### <a name="copy-a-containers-to-another-storage-account"></a>Copiar um contêiner para outra conta de armazenamento

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>" --recursive` |
| **Exemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer" "https://mydestinationaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="copy-all-containers-folders-and-files-to-another-storage-account"></a>Copiar todos os contêineres, pastas e arquivos para outra conta de armazenamento

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/" "https://<destination-storage-account-name>.blob.core.windows.net/" --recursive"` |
| **Exemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net" "https://mydestinationaccount.blob.core.windows.net" --recursive` |

## <a name="synchronize-files"></a>Sincronizar arquivos

Você pode sincronizar o conteúdo de um sistema de arquivos local para um contêiner de blob. Você também pode sincronizar um contêiner de blob para um sistema de arquivos local em seu computador. A sincronização é unidirecional. Em outras palavras, você escolhe qual dos dois pontos de extremidade é a origem e qual é o destino.

> [!NOTE]
> A versão atual do AzCopy não sincronizar entre outras origens e destinos (por exemplo: O armazenamento de arquivos ou recipientes do Amazon Web Services (AWS) S3).

O `sync` comando compara nomes de arquivo e carimbos de hora da última modificação. Defina as `--delete-destination` sinalizador opcional para um valor de `true` ou `prompt` excluir arquivos na pasta de destino se esses arquivos deixará de existir na pasta de origem.

Se você definir a `--delete-destination` sinalizador como `true` AzCopy exclui arquivos sem fornecer um prompt. Se você quiser um prompt para aparecer antes do AzCopy exclui um arquivo, defina as `--delete-destination` sinalizador como `prompt`.

> [!NOTE]
> Para impedir exclusões acidentais, certifique-se de habilitar o [exclusão reversível](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) recurso antes de usar o `--delete-destination=prompt|true` sinalizador.

### <a name="synchronize-a-container-to-a-local-file-system"></a>Sincronizar um contêiner para um sistema de arquivos local

Nesse caso, o sistema de arquivos local se torna a origem e o contêiner é o destino.

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy sync "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Exemplo** | `azcopy sync "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Exemplo** (namespace hierárquico) | `azcopy sync "C:\myFolder" "https://<storage-account-name>.dfs.core.windows.net/mycontainer" --recursive` |


### <a name="synchronize-a-local-file-system-to-a-container"></a>Sincronizar um sistema de arquivos local para um contêiner

Nesse caso, o contêiner se torna a origem e o sistema de arquivos local é o destino.

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy sync "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" "C:\myFolder" --recursive` |
| **Exemplo** | `azcopy sync "https://mystorageaccount.blob.core.windows.net/mycontainer" "C:\myFolder" --recursive` |
| **Exemplo** (namespace hierárquico) | `azcopy sync "https://mystorageaccount.dfs.core.windows.net/mycontainer" "C:\myFolder" --recursive` |

## <a name="next-steps"></a>Próximas etapas

Encontre mais exemplos em qualquer um dos seguintes artigos:

- [Introdução ao AzCopy](storage-use-azcopy-v10.md)

- [Tutorial: Migrar dados de locais para armazenamento em nuvem usando o AzCopy](storage-use-azcopy-migrate-on-premises-data.md)

- [Transferir dados com o armazenamento de arquivos e AzCopy](storage-use-azcopy-files.md)

- [Transferir dados com o AzCopy e o Amazon S3 buckets](storage-use-azcopy-s3.md)

- [Configurar, otimizar e solucionar problemas de AzCopy](storage-use-azcopy-configure.md)
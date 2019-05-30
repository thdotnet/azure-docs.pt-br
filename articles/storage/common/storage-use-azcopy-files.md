---
title: Transferir dados para ou dos arquivos do Azure usando AzCopy v10 | Microsoft Docs
description: Transferir dados com o armazenamento de arquivos e o AzCopy.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 5d11d5c0da350b9abf2e2b06a11a1690cf3f6922
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247103"
---
# <a name="transfer-data-with-azcopy-and-file-storage"></a>Transferir dados com o armazenamento de arquivos e AzCopy 

O AzCopy é um utilitário de linha de comando que você pode usar para copiar blobs ou arquivos de ou para uma conta de armazenamento. Este artigo contém comandos de exemplo que funcionam com arquivos do Azure.

Antes de começar, consulte a [começar com o AzCopy](storage-use-azcopy-v10.md) artigo para baixar o AzCopy e familiarizar-se com a ferramenta.

## <a name="create-file-shares"></a>Criar compartilhamentos de arquivos

Você pode usar o AzCopy `make` comando para criar um compartilhamento de arquivos. O exemplo nesta seção cria um compartilhamento de arquivos chamado `myfileshare`.

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy make "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>"` |
| **Exemplo** | `azcopy make "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

## <a name="upload-files"></a>Carregar arquivos

Você pode usar o AzCopy `copy` comando para carregar arquivos e pastas do seu computador local.

Esta seção contém os seguintes exemplos:

> [!div class="checklist"]
> * Carregar um arquivo
> * Carregar uma pasta
> * Carregar arquivos usando curingas

> [!NOTE]
> O AzCopy não calcular e armazenar o código de hash md5 do arquivo automaticamente. Se você quiser AzCopy para fazer isso, em seguida, acrescente o `--put-md5` sinalizador para cada comando de cópia. Dessa forma, quando o arquivo é baixado, AzCopy calcula um hash MD5 para dados baixados e verifica que o hash MD5 armazenado no arquivo de `Content-md5` propriedade corresponda ao hash calculado.

### <a name="upload-a-file"></a>Carregar um arquivo

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-name>?<SAS-token>"` |
| **Exemplo** | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

### <a name="upload-a-folder"></a>Carregar uma pasta

Este exemplo copia uma pasta (e todos os arquivos nessa pasta) para um compartilhamento de arquivos. O resultado é uma pasta no compartilhamento de arquivos com o mesmo nome.

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "<local-folder-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>" --recursive` |
| **Exemplo** | `azcopy copy "C:\myFolder" "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Para copiar para uma pasta no compartilhamento de arquivo, especifique o nome da pasta em sua cadeia de caracteres de comando.

|    |     |
|--------|-----------|
| **Exemplo** | `azcopy copy "C:\myFolder" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Se você especificar o nome de uma pasta que não existe no compartilhamento de arquivos, o AzCopy cria uma nova pasta com esse nome.

### <a name="upload-the-contents-of-a-folder"></a>Carregar o conteúdo de uma pasta

Você pode carregar o conteúdo de uma pasta sem copiar a pasta que contém em si, usando o símbolo de caractere curinga (*).

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "<local-folder-path>/*" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<folder-path>?<SAS-token>` |
| **Exemplo** | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

> [!NOTE]
> Acrescente a `--recursive` sinalizador para carregar arquivos em todas as subpastas.

## <a name="download-files"></a>Baixar arquivos

Você pode usar o AzCopy `copy` compartilhamentos de comando para baixar arquivos, pastas e arquivos em seu computador local.

Esta seção contém os seguintes exemplos:

> [!div class="checklist"]
> * Baixar um arquivo
> * Uma pasta de download
> * Baixar arquivos usando curingas

> [!NOTE]
> Se o `Content-md5` valor de propriedade de um arquivo contém um hash, o AzCopy calcula um hash MD5 para dados baixados e verifica que o hash MD5 armazenado no arquivo de `Content-md5` propriedade corresponda ao hash calculado. Se esses valores não corresponderem, o download falhar, a menos que você substituir esse comportamento por meio do acréscimo `--check-md5=NoCheck` ou `--check-md5=LogOnly` para o comando Copiar.

### <a name="download-a-file"></a>Baixar um arquivo

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path>?<SAS-token>" "<local-file-path>"` |
| **Exemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myFolder\myTextFile.txt"` |

### <a name="download-a-folder"></a>Uma pasta de download

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<folder-path>?<SAS-token>" "<local-folder-path>" --recursive` |
| **Exemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myFolder"  --recursive` |

Este exemplo resulta em uma pasta chamada `C:\myFolder\myFileShareFolder` que contém todos os arquivos baixados.

### <a name="download-the-contents-of-a-folder"></a>Baixar o conteúdo de uma pasta

Você pode baixar o conteúdo de uma pasta sem copiar a pasta que contém em si, usando o símbolo de caractere curinga (*).

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/*?<SAS-token>" "<local-folder-path>/"` |
| **Exemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder/*?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myFolder"` |

> [!NOTE]
> Acrescente a `--recursive` sinalizador para baixar os arquivos em todas as subpastas.

## <a name="next-steps"></a>Próximas etapas

Encontre mais exemplos em qualquer um dos seguintes artigos:

- [Introdução ao AzCopy](storage-use-azcopy-v10.md)

- [Transferir dados com o armazenamento de BLOBs e AzCopy](storage-use-azcopy-blobs.md)

- [Transferir dados com o AzCopy e o Amazon S3 buckets](storage-use-azcopy-s3.md)

- [Configurar, otimizar e solucionar problemas de AzCopy](storage-use-azcopy-configure.md)
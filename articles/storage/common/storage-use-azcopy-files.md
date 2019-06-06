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
ms.openlocfilehash: 69d7136396c3d989e63b8956d3e703cc7f9666c8
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687938"
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

Você pode usar o AzCopy `copy` comando para carregar arquivos e diretórios do seu computador local.

Esta seção contém os seguintes exemplos:

> [!div class="checklist"]
> * Carregar um arquivo
> * Carregar um diretório
> * Carregar arquivos usando curingas

> [!NOTE]
> O AzCopy não calcular e armazenar o código de hash md5 do arquivo automaticamente. Se você quiser AzCopy para fazer isso, em seguida, acrescente o `--put-md5` sinalizador para cada comando de cópia. Dessa forma, quando o arquivo é baixado, AzCopy calcula um hash MD5 para dados baixados e verifica que o hash MD5 armazenado no arquivo de `Content-md5` propriedade corresponda ao hash calculado.

### <a name="upload-a-file"></a>Carregar um arquivo

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-name>?<SAS-token>"` |
| **Exemplo** | `azcopy copy "C:\myDirectory\myTextFile.txt" "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

### <a name="upload-a-directory"></a>Carregar um diretório

Este exemplo copia um diretório (e todos os arquivos nesse diretório) para um compartilhamento de arquivos. O resultado é um diretório no compartilhamento de arquivos com o mesmo nome.

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "<local-directory-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>" --recursive` |
| **Exemplo** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Para copiar um diretório dentro do compartilhamento de arquivos, especifique o nome do diretório em sua cadeia de caracteres de comando.

|    |     |
|--------|-----------|
| **Exemplo** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Se você especificar o nome de um diretório que não existe no compartilhamento de arquivos, o AzCopy cria um novo diretório com esse nome.

### <a name="upload-the-contents-of-a-directory"></a>Carregar o conteúdo de um diretório

Você pode carregar o conteúdo de um diretório sem copiar o diretório que contém em si, usando o símbolo de caractere curinga (*).

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "<local-directory-path>/*" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path>?<SAS-token>` |
| **Exemplo** | `azcopy copy "C:\myDirectory\*" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

> [!NOTE]
> Acrescente a `--recursive` sinalizador para carregar arquivos em todos os subdiretórios.

## <a name="download-files"></a>Baixar arquivos

Você pode usar o AzCopy `copy` compartilhamentos de comando para baixar arquivos, diretórios e arquivos em seu computador local.

Esta seção contém os seguintes exemplos:

> [!div class="checklist"]
> * Baixar um arquivo
> * Baixe um diretório
> * Baixar arquivos usando curingas

> [!NOTE]
> Se o `Content-md5` valor de propriedade de um arquivo contém um hash, o AzCopy calcula um hash MD5 para dados baixados e verifica que o hash MD5 armazenado no arquivo de `Content-md5` propriedade corresponda ao hash calculado. Se esses valores não corresponderem, o download falhar, a menos que você substituir esse comportamento por meio do acréscimo `--check-md5=NoCheck` ou `--check-md5=LogOnly` para o comando Copiar.

### <a name="download-a-file"></a>Baixar um arquivo

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path>?<SAS-token>" "<local-file-path>"` |
| **Exemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myDirectory\myTextFile.txt"` |

### <a name="download-a-directory"></a>Baixe um diretório

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path>?<SAS-token>" "<local-directory-path>" --recursive` |
| **Exemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myDirectory"  --recursive` |

Este exemplo resulta em um diretório chamado `C:\myDirectory\myFileShareDirectory` que contém todos os arquivos baixados.

### <a name="download-the-contents-of-a-directory"></a>Baixar o conteúdo de um diretório

Você pode baixar o conteúdo de um diretório sem copiar o diretório que contém em si, usando o símbolo de caractere curinga (*).

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/*?<SAS-token>" "<local-directory-path>/"` |
| **Exemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory/*?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myDirectory"` |

> [!NOTE]
> Acrescente a `--recursive` sinalizador para baixar os arquivos em todos os subdiretórios.

## <a name="next-steps"></a>Próximas etapas

Encontre mais exemplos em qualquer um dos seguintes artigos:

- [Introdução ao AzCopy](storage-use-azcopy-v10.md)

- [Transferir dados com o AzCopy e o Armazenamento de Blobs](storage-use-azcopy-blobs.md)

- [Transferir dados com o AzCopy e os buckets do Amazon S3](storage-use-azcopy-s3.md)

- [Configurar, otimizar e solucionar problemas do AzCopy](storage-use-azcopy-configure.md)
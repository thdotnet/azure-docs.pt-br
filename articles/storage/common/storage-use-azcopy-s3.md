---
title: Transferir dados para o armazenamento do Azure de buckets do Amazon S3 usando AzCopy v10 | Microsoft Docs
description: Transferir dados com o AzCopy e o Amazon S3 buckets
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 04/23/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: b18c4c039b615c7c88268b6e668df9f7fec9fabf
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687917"
---
# <a name="copy-data-from-amazon-s3-buckets-by-using-azcopy"></a>Copiar dados do Amazon S3 buckets usando o AzCopy

O AzCopy é um utilitário de linha de comando que você pode usar para copiar blobs ou arquivos de ou para uma conta de armazenamento. Este artigo ajuda você a copiar objetos, diretórios e buckets do Amazon Web Services (AWS) S3 para o armazenamento de BLOBs do Azure usando o AzCopy.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Escolha como você irá fornecer as credenciais de autorização

* Para autorizar com o armazenamento do Azure, use o Azure Active Directory (AD) ou um token de assinatura de acesso compartilhado (SAS).

* Para autorizar com o AWS S3, use uma chave de acesso do AWS e uma chave de acesso secreta.

### <a name="authorize-with-azure-storage"></a>Autorizar com o armazenamento do Azure

Consulte a [começar com o AzCopy](storage-use-azcopy-v10.md) do artigo para baixar o AzCopy e, em seguida, escolha como você irá fornecer as credenciais de autorização para o serviço de armazenamento.

> [!NOTE]
> Os exemplos neste artigo presumem que você foi autenticado sua identidade usando o `AzCopy login` comando. O AzCopy, em seguida, usa sua conta do Azure AD para autorizar o acesso aos dados no armazenamento de BLOBs.
>
> Se você preferir usar um token SAS para autorizar o acesso a dados de blob, você pode acrescentar esse token para a URL de recurso em cada comando AzCopy.
>
> Por exemplo: `https://mystorageaccount.blob.core.windows.net/mycontainer?<SAS-token>`.

### <a name="authorize-with-aws-s3"></a>Autorizar com o AWS S3

Reunir a sua chave de acesso do AWS e a chave de acesso secreta e, em seguida, defina essas variáveis de ambiente:

| Sistema operacional | Comando  |
|--------|-----------|
| **Windows** | `set AWS_ACCESS_KEY_ID=<access-key>`<br>`set AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **Linux** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **MacOS** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>`|

## <a name="copy-objects-directories-and-buckets"></a>Copiar objetos, diretórios e buckets

O AzCopy usa a [colocar bloco From URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API, portanto, os dados são copiados diretamente entre o AWS S3 e servidores de armazenamento. Essas operações de cópia não usam a largura de banda de rede do seu computador.

### <a name="copy-an-object"></a>Copiar um objeto

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>/<object-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>/<blob-name>"` |
| **Exemplo** | `azcopy cp "https://s3.amazonaws.com/mybucket/myobject" "https://mystorageaccount.blob.core.windows.net/mycontainer/myblob"` |

> [!NOTE]
> Exemplos neste artigo usam URLs de estilo de caminho para buckets AWS S3 (por exemplo: `http://s3.amazonaws.com/<bucket-name>`). 
>
> Você também pode usar o virtuais URLs de estilo hospedado (por exemplo: `http://bucket.s3.amazonaws.com`). 
>
> Para saber mais sobre hospedagem virtual de buckets, consulte [Virtual de hospedagem de Buckets]] (https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html).

### <a name="copy-a-directory"></a>Copiar um diretório

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>/<directory-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>" --recursive=true` |
| **Exemplo** | `azcopy cp "https://s3.amazonaws.com/mybucket/mydirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory" --recursive=true` |

### <a name="copy-a-bucket"></a>Copiar uma partição de memória

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>" --recursive=true` |
| **Exemplo** | `azcopy cp "https://s3.amazonaws.com/mybucket" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive=true` |

### <a name="copy-all-buckets-in-all-regions"></a>Copiar todos os buckets em todas as regiões

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "https://s3.amazonaws.com/" "https://<storage-account-name>.blob.core.windows.net" --recursive=true` |
| **Exemplo** | `azcopy cp "https://s3.amazonaws.com" "https://mystorageaccount.blob.core.windows.net" --recursive=true` |

### <a name="copy-all-buckets-in-a-specific-s3-region"></a>Copiar todos os buckets em uma região específica do S3

|    |     |
|--------|-----------|
| **Sintaxe** | `azcopy cp "https://s3-<region-name>.amazonaws.com/" "https://<storage-account-name>.blob.core.windows.net" --recursive=true` |
| **Exemplo** | `azcopy cp "https://s3-rds.eu-north-1.amazonaws.com" "https://mystorageaccount.blob.core.windows.net" --recursive=true` |

## <a name="handle-differences-in-object-naming-rules"></a>Lidar com diferenças nas regras de nomenclatura de objeto

AWS S3 tem um conjunto diferente de convenções de nomenclatura para nomes de bucket em comparação com os contêineres de blob do Azure. Você pode ler sobre elas [aqui](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules). Se você optar por copiar um grupo de buckets para uma conta de armazenamento do Azure, a operação de cópia poderá falhar devido a diferenças de nomenclatura.

AzCopy lida com dois dos problemas mais comuns que podem surgir; buckets que contêm períodos e buckets que contêm hifens consecutivos. Nomes do AWS S3 bucket podem conter hifens consecutivos e pontos, mas não de um contêiner no Azure. AzCopy substitui os pontos com hifens consecutivos com um número que representa o número de hifens consecutivos e hifens (por exemplo: um bucket denominado `my----bucket` torna-se `my-4-bucket`. 

Além disso, como o AzCopy copia arquivos, ele verifica se há conflitos de nomenclatura e tenta resolvê-los. Por exemplo, se houver buckets com o nome `bucket-name` e `bucket.name`, o AzCopy resolve uma partição de memória denominada `bucket.name` primeiro para `bucket-name` e, em seguida, para `bucket-name-2`.

## <a name="handle-differences-in-object-metadata"></a>Lidar com diferenças nos metadados de objeto

AWS S3 e o Azure permitem que diferentes conjuntos de caracteres nos nomes de chaves de objeto. Você pode ler sobre os caracteres que usa o AWS S3 [aqui](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys). No lado do Azure, chaves de objeto de blob aderem às regras de nomenclatura [ C# identificadores](https://docs.microsoft.com/dotnet/csharp/language-reference/).

Como parte de um AzCopy `copy` de comando, você pode fornecer um valor para opcional a `s2s-invalid-metadata-handle` sinalizador que especifica como você deseja lidar com arquivos em que os metadados do arquivo contém os nomes de chave incompatíveis. A tabela a seguir descreve cada valor de sinalizador.

| Valor de sinalizador | DESCRIÇÃO  |
|--------|-----------|
| **ExcludeIfInvalid** | (Opção padrão) Os metadados não está incluído o objeto transferido. AzCopy registra um aviso. |
| **FailIfInvalid** | Os objetos não são copiados. AzCopy registra um erro e inclui esse erro na contagem de falhas que aparece no resumo da transferência.  |
| **RenameIfInvalid**  | AzCopy resolve a chave de metadados inválido e copia o objeto para o Azure usando o par de valor de chave de metadados resolvido. Para saber exatamente quais etapas AzCopy leva para renomear as chaves de objeto, consulte a [como o AzCopy renomeia as chaves de objeto](#rename-logic) seção abaixo. Se o AzCopy é não é possível renomear a chave, o objeto não será copiado. |

<a id="rename-logic" />

### <a name="how-azcopy-renames-object-keys"></a>Como o AzCopy renomeia as chaves de objeto

O AzCopy executa estas etapas:

1. Substitui caracteres inválidos com '_'.

2. Adiciona a cadeia de caracteres `rename_` para o início de uma nova chave válido.

   Essa chave será usada para salvar os metadados originais **valor**.

3. Adiciona a cadeia de caracteres `rename_key_` para o início de uma nova chave válido.
   Essa chave será usada para salvar os metadados originais inválido **chave**.
   Você pode usar essa chave para tentar recuperar os metadados no lado do Azure, pois a chave de metadados é preservado como um valor no serviço de armazenamento de Blob.

## <a name="next-steps"></a>Próximas etapas

Encontre mais exemplos em qualquer um dos seguintes artigos:

- [Introdução ao AzCopy](storage-use-azcopy-v10.md)

- [Transferir dados com o AzCopy e o Armazenamento de Blobs](storage-use-azcopy-blobs.md)

- [Transferir dados com o AzCopy e o Armazenamento de Arquivos](storage-use-azcopy-files.md)

- [Configurar, otimizar e solucionar problemas do AzCopy](storage-use-azcopy-configure.md)
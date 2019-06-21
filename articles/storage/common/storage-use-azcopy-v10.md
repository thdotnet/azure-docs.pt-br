---
title: Copiar ou mover dados para o armazenamento do Azure usando o AzCopy v10 | Microsoft Docs
description: O AzCopy é um utilitário de linha de comando que você pode usar para copiar dados para, de ou entre contas de armazenamento. Este artigo ajuda você a baixar o AzCopy, conecte-se à sua conta de armazenamento e, em seguida, transferir arquivos.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 722097f1a61a10cd45c0c330e998021cd1abf0c8
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147959"
---
# <a name="get-started-with-azcopy"></a>Introdução ao AzCopy

O AzCopy é um utilitário de linha de comando que você pode usar para copiar blobs ou arquivos de ou para uma conta de armazenamento. Este artigo ajuda você a baixar o AzCopy, conecte-se à sua conta de armazenamento e, em seguida, transferir arquivos.

> [!NOTE]
> AzCopy **V10** é a versão com suporte no momento do AzCopy.
>
> Se você precisar usar o AzCopy **v8.1**, consulte o [usar a versão anterior do AzCopy](#previous-version) seção deste artigo.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>Baixar o AzCopy

Primeiro, baixe o arquivo executável do AzCopy V10 para qualquer diretório em seu computador. 

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

> [!NOTE]
> Se você deseja copiar dados de e para seus [o armazenamento de tabelas](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) de serviço e, em seguida, instale [AzCopy versão 7.3](https://aka.ms/downloadazcopynet).

## <a name="run-azcopy"></a>Executar o AzCopy

Para sua conveniência, considere adicionar o local do diretório do executável AzCopy ao caminho do sistema para facilidade de uso. Dessa forma, você pode digitar `azcopy` de qualquer diretório em seu sistema.

Se você optar por não adicionar o diretório do AzCopy ao caminho, você terá que altere os diretórios para o local do seu arquivo executável do AzCopy e digite `azcopy` ou `.\azcopy` nos prompts de comando do Windows PowerShell.

Para ver uma lista de comandos, digite `azcopy -h` e, em seguida, pressione a tecla ENTER.

Para saber mais sobre um comando específico, basta incluir o nome do comando (por exemplo: `azcopy list -h`).

![Ajuda embutida](media/storage-use-azcopy-v10/azcopy-inline-help.png)

> [!NOTE] 
> Como um proprietário de sua conta de armazenamento do Azure, você não é automaticamente atribuídas permissões para acessar dados. Antes de poder fazer qualquer coisa significativa com o AzCopy, você precisa decidir como você vai fornecer as credenciais de autorização para o serviço de armazenamento. 

## <a name="choose-how-youll-provide-authorization-credentials"></a>Escolha como você irá fornecer as credenciais de autorização

Você pode fornecer credenciais de autorização usando o Azure Active Directory (AD) ou usando um token de assinatura de acesso compartilhado (SAS).

Use esta tabela como um guia:

| Tipo de armazenamento | Método com suporte no momento de autorização |
|--|--|
|**Armazenamento de Blobs** | Azure AD & SAS |
|**Armazenamento de BLOBs (namespace referiremos)** | Somente AD do Azure |
|**Armazenamento de arquivos** | Somente a SAS |

### <a name="option-1-use-azure-ad"></a>Opção 1: Usar o Azure AD

O nível de autorização que você precisa se baseia se você planeja carregar arquivos ou apenas baixá-los.

Se você quiser baixar arquivos, em seguida, verifique se que o [leitor de dados de Blob de armazenamento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) foi atribuído à sua identidade.

Se você quiser carregar arquivos e, em seguida, verifique se que uma dessas funções foi atribuída à sua identidade:

- [Colaborador de dados de blob de armazenamento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Proprietário de Dados do Blob de Armazenamento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Essas funções podem ser atribuídas à sua identidade em qualquer um desses escopos:

- Contêiner (sistema de arquivos)
- Conta de armazenamento
- Grupo de recursos
- Assinatura

Para saber como verificar e atribuir funções, consulte [conceder acesso a dados do Azure blob e fila com o RBAC no portal do Azure](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Você não precisa ter uma dessas funções atribuídas à sua identidade, se sua identidade é adicionada à lista de controle de acesso (ACL) do contêiner de destino ou diretório. Na ACL, sua identidade precisa de permissão de gravação no diretório de destino e permissão de execução no contêiner e cada diretório pai.

Para obter mais informações, consulte [controle de acesso no armazenamento do Azure Data Lake Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authenticate-your-identity"></a>Autenticar sua identidade

Depois de verificar que sua identidade recebeu o nível de autorização necessária, abra um prompt de comando, digite o seguinte comando e, em seguida, pressione a tecla ENTER.

```azcopy
azcopy login
```

Se você pertencer a mais de uma organização, inclua a ID de locatário da organização à qual pertence a conta de armazenamento.

```azcopy
azcopy login --tenant-id=<tenant-id>
```

Substitua o `<tenant-id>` espaço reservado com a ID de locatário da organização à qual pertence a conta de armazenamento. Para localizar a ID de locatário, selecione **Azure Active Directory > Propriedades > ID de diretório** no portal do Azure.

Esse comando retorna um código de autenticação e a URL de um site. Abra o site, forneça o código e, em seguida, escolha o botão **Avançar**.

![Criar um contêiner](media/storage-use-azcopy-v10/azcopy-login.png)

Uma janela de entrada será exibida. Nessa janela, entre em sua conta do Azure usando suas credenciais de conta do Azure. Depois de entrar com êxito, feche a janela do navegador e comece a usar o AzCopy.

### <a name="option-2-use-a-sas-token"></a>Opção 2: Usar um token SAS

Você pode acrescentar um token SAS para cada URL de origem ou destino que usam em seus comandos de AzCopy.

Recursivamente de comando neste exemplo copia dados de um diretório local para um contêiner de blob. Um token SAS fictício é acrescentado ao final das da URL do contêiner.

```azcopy
azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

Para saber mais sobre tokens SAS e como obtê-lo, consulte [usando assinaturas de acesso (SAS) compartilhado](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="transfer-files"></a>Transferir arquivos

Depois de autenticado sua identidade ou obtido um token SAS, você pode começar a transferência de arquivos.

Para localizar os comandos de exemplo, consulte qualquer um destes artigos.

- [Transferir dados com o AzCopy e o Armazenamento de Blobs](storage-use-azcopy-blobs.md)

- [Transferir dados com o AzCopy e o Armazenamento de Arquivos](storage-use-azcopy-files.md)

- [Transferir dados com o AzCopy e os buckets do Amazon S3](storage-use-azcopy-s3.md)

## <a name="use-azcopy-in-a-script"></a>Use o AzCopy em um script

Ao longo do tempo, o AzCopy [baixar link](#download-and-install-azcopy) apontará para novas versões do AzCopy. Se seu script baixa AzCopy, o script poderá parar de funcionar se uma versão mais recente do AzCopy modifica os recursos que depende de seu script. 

Para evitar esses problemas, obter um link estático (sem alteração) para a versão atual do AzCopy. Dessa forma, seu script baixa a exatamente a mesma versão do AzCopy cada vez que ele é executado.

Para obter o link, execute este comando:

| Sistema operacional  | Comando |
|--------|-----------|
| **Linux** | `curl -v https://aka.ms/downloadazcopy-v10-linux` |
| **Windows** | `(curl https://aka.ms/downloadazcopy-v10-windows -MaximumRedirection 0 -ErrorAction silentlycontinue).RawContent` |

> [!NOTE]
> Para o Linux, `--strip-components=1` sobre o `tar` comando remove a pasta de nível superior que contém o nome da versão e, em vez disso, extrai o binário diretamente para a pasta atual. Isso permite que o script a ser atualizada com uma nova versão do `azcopy` apenas atualizando o `wget` URL.

A URL é exibida na saída desse comando. Seu script, em seguida, pode baixar o AzCopy usando essa URL.

| Sistema operacional  | Comando |
|--------|-----------|
| **Linux** | `wget -O azcopyv10.tar https://azcopyvnext.azureedge.net/release20190301/azcopy_linux_amd64_10.0.8.tar.gz tar -xf azcopyv10.tar --strip-components=1 ./azcopy` |
| **Windows** | `Invoke-WebRequest https://azcopyvnext.azureedge.net/release20190517/azcopy_windows_amd64_10.1.2.zip -OutFile azcopyv10.zip <<Unzip here>>` |

## <a name="use-azcopy-in-storage-explorer"></a>Use o AzCopy no Gerenciador de armazenamento

Se você deseja aproveitar as vantagens de desempenho do AzCopy, mas você prefere usar o Gerenciador de armazenamento em vez de linha de comando para interagir com seus arquivos, em seguida, habilite o AzCopy no Gerenciador de armazenamento. 

No Gerenciador de armazenamento, escolha **versão prévia**->**Use AzCopy para Download e Upload do Blob aprimorado**.

![Habilitar o AzCopy como um mecanismo de transferência no Gerenciador de armazenamento do Azure](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

> [!NOTE]
> Você não precisa habilitar essa configuração se você tiver habilitado um namespace hierárquico em sua conta de armazenamento. Isso ocorre porque o Gerenciador de armazenamento usa automaticamente o AzCopy em contas de armazenamento que têm um namespace hierárquico.  

O Gerenciador de armazenamento usa sua chave de conta para executar operações, portanto, depois de entrar no Gerenciador de armazenamento, você não precisará fornecer credenciais de autorização adicional.

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>Usar a versão anterior do AzCopy

Se você precisar usar a versão anterior do AzCopy (AzCopy v8.1), consulte um dos links a seguir:

- [AzCopy no Windows (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)

- [AzCopy no Linux (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>Configurar, otimizar e solucionar problemas de AzCopy

Consulte [configurar, otimizar e solucionar problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="next-steps"></a>Próximas etapas

Se você tiver dúvidas, problemas ou comentários gerais, enviá-los [no GitHub](https://github.com/Azure/azure-storage-azcopy) página.

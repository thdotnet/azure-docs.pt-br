---
title: Configurar o armazenamento de arquivos do Azure
description: Como configurar e conectar-se aos arquivos do Azure, no contêiner do Windows no serviço de aplicativo.
author: msangapu-msft
manager: gwallace
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 7/01/2019
ms.author: msangapu-msft
ms.openlocfilehash: ea6555e8459b8f372a73d7f943e9a96523d4c791
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789040"
---
# <a name="configure-azure-files-in-a-windows-container-on-app-service"></a>Configurar os arquivos do Azure em um contêiner do Windows no serviço de aplicativo

> [!NOTE]
> Este artigo se aplica aos contêineres do Windows personalizados. Para implantar no serviço de aplicativo no _Linux_, consulte [fornecimento do conteúdo do armazenamento do Azure](./containers/how-to-serve-content-from-azure-storage.md).
>

Este guia mostra como acessar o armazenamento do Azure em contêineres do Windows. Somente [compartilhamentos de arquivos do Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli) e [compartilhamentos de arquivos do Premium](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-premium-fileshare) têm suporte. Você pode usar compartilhamentos de arquivos do Azure neste passo a passo. Os benefícios incluem conteúdo seguro, portabilidade de conteúdo, acesso a vários aplicativos e vários métodos de transferência.

## <a name="prerequisites"></a>Pré-requisitos

- [CLI do Azure](/cli/azure/install-azure-cli) (2.0.46 ou posterior).
- [Um aplicativo de contêiner do Windows existente no serviço de aplicativo do Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-windows-container)
- [Criar compartilhamento de arquivos do Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli)
- [Carregar arquivos no compartilhamento de arquivos do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide)

> [!NOTE]
> Os arquivos do Azure é diferente do padrão de armazenamento e cobrado separadamente, não é incluído com o aplicativo web. Ele não oferece suporte usando a configuração de Firewall devido a limitações da infra-estrutura.
>

## <a name="link-storage-to-your-web-app-preview"></a>Armazenamento de link para seu aplicativo Web (versão prévia)

 Para montar um compartilhamento de arquivos do Azure para um diretório em seu aplicativo de serviço de aplicativo, você deve usar o [ `az webapp config storage-account add` ](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) comando. Tipo de armazenamento deve ser AzureFiles.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory of form c:<directory name> >
```

Você deve fazer isso para quaisquer outros diretórios que você deseja ser vinculado a um arquivos do Azure de compartilhamento.

## <a name="verify"></a>Verificar

Depois que um compartilhamento de arquivos do Azure é vinculado a um aplicativo web, você pode verificar isso executando o seguinte comando:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```


## <a name="next-steps"></a>Próximas etapas

- [Migrar um aplicativo ASP.NET para o serviço de aplicativo do Azure usando um contêiner do Windows (visualização)](app-service-web-tutorial-windows-containers-custom-fonts.md).
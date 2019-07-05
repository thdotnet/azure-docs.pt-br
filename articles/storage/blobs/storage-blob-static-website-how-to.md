---
title: Hospedar um site estático no armazenamento do Azure
description: Saiba como fornecer conteúdo estático (HTML, CSS, JavaScript e arquivos de imagem) diretamente de um contêiner em uma conta de GPv2 de armazenamento do Azure.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.author: normesta
ms.date: 05/28/2019
ms.openlocfilehash: 5ab24a99b22fae172b5308ba7477953f27ecfd44
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67435945"
---
# <a name="host-a-static-website-in-azure-storage"></a>Hospedar um site estático no armazenamento do Azure

Você pode fornecer conteúdo estático (HTML, CSS, JavaScript e arquivos de imagem) diretamente de um contêiner em uma conta de GPv2 de armazenamento do Azure. Para obter mais informações, consulte [hospedagem de site estático no armazenamento do Azure](storage-blob-static-website.md).

Este artigo mostra como habilitar a hospedagem de site estático usando o portal do Azure, a CLI do Azure ou PowerShell.

<a id="portal" />

## <a name="use-the-azure-portal"></a>Use o Portal do Azure

Para obter um tutorial passo a passo, consulte [Tutorial: Hospedar um site estático no Armazenamento de Blobs](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host).

Depois de habilitar a hospedagem de site estático, você pode exibir as páginas do seu site em um navegador usando a URL pública do site.

<a id="portal-find-url" />

### <a name="find-the-website-url-by-using-the-azure-portal"></a>Encontrar a URL do site usando o portal do Azure

No painel que aparece ao lado da página de visão geral da conta de sua conta de armazenamento, selecione **site estático**. A URL do seu site é exibida na **ponto de extremidade primário** campo.

![Métrica de métricas de sites estáticos de Armazenamento do Microsoft Azure](./media/storage-blob-static-website/storage-blob-static-website-url.png)

<a id="cli" />

## <a name="use-the-azure-cli"></a>Usar a CLI do Azure

Você pode habilitar a hospedagem de site estático usando o [Interface de linha de comando do Azure (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

1. Primeiro, abra o [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest), ou se você tiver [instalado](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) a CLI do Azure localmente, abra um aplicativo de console de comando, como o Windows PowerShell.

2. Se sua identidade estiver associada a mais de uma assinatura, em seguida, defina seu ativo a assinatura da conta de armazenamento que irá hospedar seu site estático.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   Substitua o `<subscription-id>` valor de espaço reservado com a ID da sua assinatura.

3. Habilite a hospedagem de site estático.

   ```azurecli-interactive
   az storage blob service-properties update --account-name <storage-account-name> --static-website --404-document <error-document-name> --index-document <index-document-name>
   ```

   * Substitua o valor de espaço reservado `<storage-account-name>` pelo nome da sua conta de armazenamento.

   * Substitua o `<error-document-name>` espaço reservado com o nome do documento de erro que será exibido aos usuários quando um navegador solicita uma página em seu site que não existe.

   * Substitua o `<index-document-name>` espaço reservado com o nome do documento de índice. Este documento geralmente é "index. html".

4. Carregar objetos para o contêiner *$web* de um diretório de origem.

   > [!NOTE]
   > Se você estiver usando o Azure Cloud Shell, certifique-se de adicionar um `\` caractere de escape para se referir ao `$web` contêiner (por exemplo: `\$web`). Se você estiver usando uma instalação local da CLI do Azure, em seguida, você não precisará usar o caractere de escape.

   Este exemplo pressupõe que você está executando comandos da sessão do Azure Cloud Shell.

   ```azurecli-interactive
   az storage blob upload-batch -s <source-path> -d \$web --account-name <storage-account-name>
   ```

   * Substitua o valor de espaço reservado `<storage-account-name>` pelo nome da sua conta de armazenamento.

   * Substitua o `<source-path>` espaço reservado com um caminho para o local dos arquivos que você deseja carregar.

   > [!NOTE]
   > Se você estiver usando uma instalação local da CLI do Azure e, em seguida, você pode usar o caminho para qualquer local no computador local (por exemplo: `C:\myFolder`.
   >
   > Se você estiver usando o Azure Cloud Shell, você precisará fazer referência a um compartilhamento de arquivos que é visível para o Cloud Shell. Esse local pode ser o compartilhamento de arquivos da nuvem compartilham a mesmo ou um compartilhamento de arquivo que você montar do Cloud Shell. Para saber como fazer isso, consulte [persistir arquivos no Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage).

<a id="cli-find-url" />

### <a name="find-the-website-url-by-using-the-azure-cli"></a>Localizar a URL do site usando a CLI do Azure

Você pode exibir o conteúdo de um navegador usando a URL pública do site.

Localize a URL usando o seguinte comando:

```azurecli-interactive
az storage account show -n <storage-account-name> -g <resource-group-name> --query "primaryEndpoints.web" --output tsv
```

* Substitua o valor de espaço reservado `<storage-account-name>` pelo nome da sua conta de armazenamento.

* Substitua o `<resource-group-name>` valor de espaço reservado com o nome do seu grupo de recursos.

<a id="powershell" />

## <a name="use-powershell"></a>Usar o PowerShell

Você pode habilitar a hospedagem de site estático usando o módulo PowerShell do Azure.

1. Abra uma janela de comando do Windows PowerShell.

2. Verifique se que você tenha o módulo Azure PowerShell Az versão 0,7 ou posterior.

   ```powershell
   Get-InstalledModule -Name Az -AllVersions | select Name,Version
   ```

   Se você precisar instalá-lo ou atualizá-lo, confira [Instalar o módulo do Azure PowerShell](/powershell/azure/install-Az-ps).

3. Entre na sua assinatura do Azure com o comando `Connect-AzAccount` e siga as instruções na tela.

   ```powershell
   Connect-AzAccount
   ```

4. Se sua identidade estiver associada a mais de uma assinatura, em seguida, defina seu ativo a assinatura da conta de armazenamento que irá hospedar seu site estático.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   Substitua o `<subscription-id>` valor de espaço reservado com a ID da sua assinatura.

5. Obter o contexto da conta de armazenamento que define a conta de armazenamento que você deseja usar.

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * Substitua o `<resource-group-name>` valor de espaço reservado com o nome do seu grupo de recursos.

   * Substitua o valor de espaço reservado `<storage-account-name>` pelo nome da sua conta de armazenamento.

6. Habilite a hospedagem de site estático.

   ```powershell
   Enable-AzStorageStaticWebsite -Context $ctx -IndexDocument <index-document-name> -ErrorDocument404Path <error-document-name>
   ```

   * Substitua o `<error-document-name>` espaço reservado com o nome do documento de erro que será exibido aos usuários quando um navegador solicita uma página em seu site que não existe.

   * Substitua o `<index-document-name>` espaço reservado com o nome do documento de índice. Este documento geralmente é "index. html".

7. Carregar objetos para o contêiner *$web* de um diretório de origem.

    ```powershell
    # upload a file
    set-AzStorageblobcontent -File "<path-to-file>" `
    -Container `$web `
    -Blob "<blob-name>" `
    -Context $ctx
     ```

   * Substitua os `<path-to-file>` valor de espaço reservado com o caminho totalmente qualificado para o arquivo que você deseja carregar (por exemplo: `C:\temp\index.html`).

   * Substitua os `<blob-name>` valor de espaço reservado com o nome que você deseja dar ao blob resultante (por exemplo: `index.html`).

<a id="powershell-find-url" />

### <a name="find-the-website-url-by-using-powershell"></a>Localizar a URL do site usando o PowerShell

Você pode exibir o conteúdo de um navegador usando a URL pública do site.

Localize a URL usando o seguinte comando:

```powershell
 $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
Write-Output $storageAccount.PrimaryEndpoints.Web
```

* Substitua o `<resource-group-name>` valor de espaço reservado com o nome do seu grupo de recursos.

* Substitua o valor de espaço reservado `<storage-account-name>` pelo nome da sua conta de armazenamento.

<a id="metrics" />

## <a name="enable-metrics-on-static-website-pages"></a>Habilitar as métricas nas páginas do site estático

Depois de habilitar as métricas, as estatísticas em arquivos de tráfego a **$web** contêiner será relatado no painel de métricas.

1. Clique em **as configurações** > **monitoramento** > **métricas**.

   Dados de métricas são gerados pendurando em diferentes APIs de métrica. O portal exibe apenas membros da API usados em um determinado período de tempo para se concentrar apenas em membros que retornam dados. Para garantir que você poderá selecionar o membro de API necessário, a primeira etapa é expandir o intervalo de tempo.

2. Clique no botão de período de tempo e selecione **últimas 24 horas** e, em seguida, clique em **aplicar**.

   ![Intervalo de tempo de métricas de sites estático do Armazenamento do Microsoft Azure](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

3. Selecione **Blob** da *Namespace* lista suspensa.

   ![Namespace de métricas de sites estático do Armazenamento do Microsoft Azure](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

4. Em seguida, selecione a métrica **Egresso**.

   ![Métrica de métricas de sites estáticos de Armazenamento do Microsoft Azure](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

5. Selecione **Somar** do seletor *Agregação*.

   ![Agregação de métricas de sites estáticos de Armazenamento do Microsoft Azure](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

6. Clique o **Adicionar filtro** botão e escolha **nome da API** do *propriedade* seletor.

   ![Nome da API de métricas de sies estáticos do Armazenamento do Microsoft Azure](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

7. a caixa de seleção ao lado **GetWebContent** na *valores* seletor para popular o relatório de métricas.

   ![O GetWebContent de métricas de websites estáticos do Armazenamento do Microsoft Azure](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

## <a name="next-steps"></a>Próximas etapas

* [Hospedagem de site estático no Armazenamento do Azure](storage-blob-static-website.md)
* [Usar a CDN do Azure para acessar blobs com domínios personalizados por HTTPS](storage-https-custom-domain-cdn.md)
* [Configurar um nome de domínio personalizado para seu ponto de extremidade de Blobs ou web](storage-custom-domain-name.md)
* [Funções do Azure](/azure/azure-functions/functions-overview)
* [Serviço de Aplicativo do Azure](/azure/app-service/overview)
* [Compile seu primeiro aplicativo web sem servidor](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Tutorial: Hospede seu domínio no DNS do Azure](../../dns/dns-delegate-domain-azure-dns.md)

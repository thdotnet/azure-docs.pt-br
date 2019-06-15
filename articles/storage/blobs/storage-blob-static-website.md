---
title: Hospedagem de site estático no Armazenamento do Microsoft Azure
description: O Armazenamento do Microsoft Azure agora oferece um site estático hospedand, fornecendo uma solução econômica e dimensionável para hospedar aplicativos web modernos.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.author: normesta
ms.reviewer: seguler
ms.date: 05/29/2019
ms.subservice: blobs
ms.openlocfilehash: 36cc8cebdb567cb9650ad1ad3baf72a0b5478247
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427971"
---
# <a name="static-website-hosting-in-azure-storage"></a>Hospedagem de site estático no Armazenamento do Microsoft Azure

Você pode fornecer o conteúdo estático (HTML, CSS, JavaScript e arquivos de imagem) diretamente de um contêiner de armazenamento denominado *$web*. Hospedando o conteúdo no armazenamento do Azure permite que você use arquiteturas sem servidor que incluem [Azure Functions](/azure/azure-functions/functions-overview) e outra plataforma como um serviço (PaaS) de serviços.

> [!NOTE]
> Se seu site depende do código do lado do servidor, use [serviço de aplicativo do Azure](/azure/app-service/overview) em vez disso.

## <a name="setting-up-a-static-website"></a>Como configurar um site estático

Hospedagem de site estático é um recurso que você precisa ativar a conta de armazenamento.

Para habilitar a hospedagem de site estático, selecione o nome do arquivo padrão e, em seguida, opcionalmente, forneça um caminho para uma página 404 personalizada. Se um contêiner de armazenamento de blob denominado **$web** ainda não existir na conta, ela é criada para você. Adicione os arquivos do seu site para esse contêiner.

Para obter orientação passo a passo, consulte [hospedar um site estático no armazenamento do Azure](storage-blob-static-website-how-to.md).

![Métrica de métricas de sites estáticos de Armazenamento do Microsoft Azure](./media/storage-blob-static-website/storage-blob-static-website-blob-container.png)

Arquivos a **$web** contêiner diferenciam maiusculas de minúsculas, atendidos por meio de solicitações de acesso anônimo e estão disponíveis apenas por meio de operações de leitura.

## <a name="uploading-content"></a>Carregando conteúdo

Você pode usar qualquer uma dessas ferramentas para carregar o conteúdo para o **$web** contêiner:

> [!div class="checklist"]
> * [CLI do Azure](storage-blob-static-website-how-to.md#cli)
> * [Módulo do Azure PowerShell](storage-blob-static-website-how-to.md#powershell)
> * [AzCopy](../common/storage-use-azcopy-v10.md)
> * [Gerenciador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/)
> * [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)
> * [Extensão do Visual Studio Code](https://code.visualstudio.com/tutorials/static-website/getting-started)

## <a name="viewing-content"></a>Exibindo conteúdo

Os usuários podem exibir o conteúdo do site em um navegador usando a URL pública do site. Você pode encontrar a URL usando o portal do Azure, CLI do Azure ou PowerShell. Use essa tabela como um guia.

|Ferramenta| Diretrizes |
|----|----|
|**Portal do Azure** | [Encontrar a URL do site usando o portal do Azure](storage-blob-static-website-how-to.md#portal-find-url) |
|**CLI do Azure** | [Localizar a URL do site usando a CLI do Azure](storage-blob-static-website-how-to.md#cli-find-url) |
|**Módulo do Azure PowerShell** | [Localizar a URL do site usando o PowerShell](storage-blob-static-website-how-to.md#powershell-find-url) |

A URL do seu site contém um código regional. Por exemplo, a URL `https://contosoblobaccount.z22.web.core.windows.net/` contém um código regional `z22`.

Enquanto que o código deve permanecer a URL, é apenas para uso interno e você não precisará usá-lo de qualquer outra forma.

O documento de índice que você especifique quando você habilita a hospedagem de site estático, será exibida quando os usuários, abra o site e não especificam um arquivo específico (por exemplo: `https://contosoblobaccount.z22.web.core.windows.net`).  

Se o servidor retornará um erro 404, e você não especificou um documento de erro quando você habilitou o site, uma página de padrão 404 é retornado ao usuário.

## <a name="impact-of-the-setting-the-public-access-level-of-the-web-container"></a>Impacto da configuração de nível de acesso público do contêiner da web

Você pode modificar o nível de acesso público a **$web** contêiner, mas isso não tem impacto sobre o ponto de extremidade primário do site estático porque esses arquivos são atendidos por meio de solicitações de acesso anônimo. Isso significa que o acesso público de (somente leitura) para todos os arquivos.

Captura de tela a seguir mostra a configuração de nível de acesso público no portal do Azure:

![Captura de tela mostrando como definir o nível de acesso público no portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

Enquanto o ponto de extremidade primário do site estático não é afetado, uma alteração para o nível de acesso público tem impacto sobre o ponto de extremidade de serviço blob primário.

Por exemplo, se você alterar o nível de acesso público do **$web** contêiner a partir **privado (sem acesso anônimo)** para **Blob (acesso de leitura anônimo somente para blobs)** , em seguida, a nível de acesso público para o ponto de extremidade primário do site estático `https://contosoblobaccount.z22.web.core.windows.net/index.html` não é alterado.

No entanto, o ponto de extremidade de serviço de blob o acesso público para o primário `https://contosoblobaccount.blob.core.windows.net/$web/index.html` alterar de privado para público. Agora os usuários podem abrir esse arquivo usando qualquer um dos dois pontos de extremidade.

## <a name="content-delivery-network-cdn-and-secure-socket-layer-ssl-support"></a>Content Delivery Network (CDN) e suporte de Secure Socket Layer (SSL)

Para disponibilizar seus arquivos de site estático em seu domínio personalizado e HTTPS, consulte [usando a CDN do Azure para acessar blobs com domínios personalizados por HTTPS](storage-https-custom-domain-cdn.md). Como parte desse processo, você precisa apontar seu CDN para o primário *site estático* ponto de extremidade em vez do primário *do serviço blob* ponto de extremidade. Talvez você precise aguardar alguns minutos antes que seu conteúdo fique visível, conforme a configuração de CDN não é executada imediatamente.

Quando você atualiza seu site estático, certifique-se de limpar o conteúdo armazenado em cache nos servidores de borda da CDN com uma limpeza de ponto de extremidade CDN. Para obter mais informações, confira [Como limpar um ponto de extremidade do CDN do Azure](../../cdn/cdn-purge-endpoint.md).

> [!NOTE]
> Há suporte para HTTPS nativamente por meio do ponto de extremidade da web de conta, portanto, o ponto de extremidade da web é acessível por meio de HTTP e HTTPS. No entanto, se a conta de armazenamento é configurada para requerer transferência segura via HTTPS, os usuários devem usar o ponto de extremidade HTTPS. Para obter mais informações, consulte [requer transferência segura no armazenamento do Azure](../common/storage-require-secure-transfer.md).
>
> O uso de domínios personalizados por HTTPS requer o uso da CDN do Azure neste momento.

## <a name="custom-domain-names"></a>Nomes de domínio personalizados

Você pode disponibilizar seu site estático por meio de um domínio personalizado. Para obter mais informações, consulte [configurar um nome de domínio personalizado para sua conta de armazenamento do Azure](storage-custom-domain-name.md).

Para obter uma visão detalhada de hospedar seu domínio no Azure, consulte [hospede seu domínio no DNS do Azure](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="pricing"></a>Preços

Você pode habilitar a hospedagem de site estático gratuitamente. Você será cobrado somente para o armazenamento de BLOBs que utiliza o seu site e os custos das operações. Para obter mais detalhes sobre preços para Armazenamento de Blob do Azure, confira [Página de Preços do Armazenamento de Blob do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="metrics"></a>metrics

Você pode habilitar métricas nas páginas do site estático. Depois de habilitar as métricas, as estatísticas em arquivos de tráfego a **$web** contêiner será relatado no painel de métricas.

Para habilitar as métricas em suas páginas do site estático, consulte [habilitar as métricas nas páginas do site estático](storage-blob-static-website-how-to.md#metrics).

## <a name="next-steps"></a>Próximas etapas

* [Hospedar um site estático no armazenamento do Azure](storage-blob-static-website-how-to.md)
* [Usar a CDN do Azure para acessar blobs com domínios personalizados por HTTPS](storage-https-custom-domain-cdn.md)
* [Configurar um nome de domínio personalizado para seu ponto de extremidade de Blobs ou web](storage-custom-domain-name.md)
* [Funções do Azure](/azure/azure-functions/functions-overview)
* [Serviço de Aplicativo do Azure](/azure/app-service/overview)
* [Compile seu primeiro aplicativo web sem servidor](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Tutorial: Hospede seu domínio no DNS do Azure](../../dns/dns-delegate-domain-azure-dns.md)

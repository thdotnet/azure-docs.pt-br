---
title: Criar um Azure HPC Cache
description: Como criar uma instância do Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: tutorial
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: 479adf9419cdd6b04e50fa479d47b56762b2bdc6
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70774619"
---
# <a name="create-an-azure-hpc-cache"></a>Criar um Azure HPC Cache

Use o portal do Azure para criar seu cache.

![captura de tela de uma visão geral do cache no portal do Azure, com botão de criação na parte inferior](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Definir os detalhes básicos

![captura de tela da página de detalhes do projeto no portal do Azure](media/create-1.png)

Em **Detalhes do Projeto**, selecione a assinatura e o grupo de recursos que hospedarão o Azure HPC Cache. Verifique se a assinatura está na lista de [acesso de visualização](hpc-cache-prereqs.md#azure-subscription).

Em **Detalhes do Serviço**, defina o nome do cache e estes outros atributos:

* Localização – selecione uma das [regiões com suporte](hpc-cache-overview.md#region-availability).
* Rede virtual – você pode selecionar uma existente ou criar uma nova rede virtual.
* Sub-rede – escolha ou crie uma sub-rede com pelo menos 64 endereços IP (/24) que serão usados somente para o Azure HPC Cache.

## <a name="set-cache-capacity"></a>Definir a capacidade de cache
<!-- change link in GUI -->

Na página **Cache**, você deve definir a capacidade do Azure HPC Cache. Esse valor determina a quantidade de dados que seu cache pode conter e a rapidez com que ele pode atender às solicitações do cliente. Após o período de versão prévia pública, a capacidade também afetará o custo do cache.

A capacidade do cache é medida em IPOS (operações de entrada/saída por segundo). Escolha a capacidade definindo estes dois valores:

* A taxa máxima de transferência de dados para o cache (taxa de transferência), em GB/segundo
* A quantidade de armazenamento alocada para dados armazenados em cache, em TB

Escolha um dos valores de taxa de transferência disponíveis e tamanhos de armazenamento em cache. A capacidade de IOPS é calculada e mostrada abaixo dos seletores de valor.

Tenha em mente que a taxa de transferência de dados real depende da carga de trabalho, das velocidades de rede e do tipo dos destinos de armazenamento. Se um arquivo não estiver no cache ou estiver marcado como obsoleto, o serviço usará alguma taxa de transferência para obtê-lo do armazenamento de back-end. O valor escolhido define a taxa de transferência máxima para todo o cache e essa quantidade não fica totalmente disponível para solicitações de cliente.

Para o armazenamento em cache, o Azure HPC Cache gerencia quais arquivos são armazenados em cache e pré-carregados para maximizar as tarifas de acesso ao cache. O conteúdo do cache é avaliado continuamente e os arquivos são movidos para o armazenamento de longo prazo quando são acessados com menos frequência. Escolha um tamanho de armazenamento de cache que possa manter confortavelmente o conjunto ativo de arquivos de trabalho com espaço adicional para metadados e outras sobrecargas.

![captura de tela da página de dimensionamento do cache](media/create-cache-iops.png)

## <a name="add-storage-targets"></a>Adicionar destinos de armazenamento

Os destinos de armazenamento são o armazenamento de longo prazo e de back-end para o conteúdo do seu cache.

Você pode definir destinos de armazenamento ao criar o cache, mas também pode adicioná-los posteriormente com o link na seção **Configurar** da página do seu cache no Portal.

![captura de tela da página de destino de armazenamento](media/create-targets.png)

Clique no link **Adicionar destino de armazenamento** para definir seus sistemas de armazenamento de back-end. O armazenamento pode ser feito em contêineres de Blob do Azure ou em sistemas NFS locais.

Você pode definir até dez destinos de armazenamento diferentes.

Para obter instruções detalhadas de como adicionar um destino de armazenamento, leia [Adicionar armazenamento](hpc-cache-add-storage.md). O procedimento é diferente para Armazenamento de Blobs e para exportações de NFS.

Para os dois tipos de armazenamento, você precisa especificar como encontrar o sistema de armazenamento de back-end (um endereço NFS ou um nome de contêiner de Blob) e o caminho do namespace voltado para o cliente.

Ao criar um destino de Armazenamento de Blobs, verifique se o cache tem permissões de acesso à conta de armazenamento, conforme descrito em [Adicionar funções de controle de acesso](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account). Se não tiver certeza de que a configuração de função será bem-sucedida, crie o cache primeiro e, em seguida, adicione o Armazenamento de Blobs.

Ao criar um destino de armazenamento de NFS, especifique um [modelo de uso](hpc-cache-add-storage.md#choose-a-usage-model). A configuração do modelo de uso ajuda o cache a otimizar seu fluxo de trabalho.

## <a name="add-resource-tags-optional"></a>Adicionar marcas de recurso (opcional)

A página **Marcas** permite que você adicione [marcas de recurso](https://go.microsoft.com/fwlink/?linkid=873112) ao Azure HPC Cache. 

## <a name="finish-creating-the-cache"></a>Concluir a criação do cache

Depois de configurar o novo cache, clique na guia **Examinar + criar**. O portal valida suas seleções e permite que você examine suas escolhas. Se tudo estiver correto, clique em **Criar**. 

A criação do cache leva cerca de 10 minutos. Você pode acompanhar o progresso no painel de notificações do portal do Azure. Após a conclusão, uma notificação será exibida com um link para a nova instância do Azure HPC Cache. 

O cache também fica visível na lista **Recursos** da sua assinatura. 

![captura de tela da instância do Azure HPC Cache no portal do Azure](media/finished-hpc-cache.png)

## <a name="next-steps"></a>Próximas etapas

Após seu cache aparecer na lista de **Recursos**, você poderá montá-lo para acesso de cliente, usá-lo para mover seus dados de conjunto de trabalho para um novo destino do Armazenamento de Blobs do Azure ou definir fontes de dados adicionais.

* [Montar o Azure HPC Cache](hpc-cache-mount.md)
* [Mover dados para o Armazenamento de Blobs do Azure para o Azure HPC Cache](hpc-cache-ingest.md)
* [Adicionar destinos de armazenamento](hpc-cache-add-storage.md)

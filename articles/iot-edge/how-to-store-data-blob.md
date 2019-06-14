---
title: Armazenar blob de blocos nos dispositivos – Azure IoT Edge | Microsoft Docs
description: Compreender os recursos de tempo de vida e em camadas, consulte as operações de armazenamento de blob com suporte e se conectar à sua conta de armazenamento de blob.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 396af2dfd9fc53c080163a27e376328c1369d5e1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65991461"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Armazenar dados na borda com o Armazenamento de Blobs do Azure no IoT Edge (versão prévia)

O Armazenamento de Blob do Azure no IoT Edge fornece uma solução de armazenamento de [blob de blocos](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) na borda. Um módulo do armazenamento de blobs no dispositivo do IoT Edge comporta-se como um serviço de blobs de blocos do Azure, mas os blobs de blocos são armazenados localmente no dispositivo do IoT Edge. É possível acessar os blobs usando os mesmos métodos do SDK do armazenamento do Azure ou as chamadas à API do blob de blocos que você já está acostumado.

> [!NOTE]
> O Armazenamento de Blobs do Azure no IoT Edge está em [visualização pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Esse módulo é fornecido com **disposição em camadas** e **time-to-live** recursos.

> [!NOTE]
> Atualmente, as camadas e a funcionalidade de time-to-live só estão disponíveis no Linux AMD64 e Linux ARM32.

**Disposição em camadas** é uma funcionalidade configurável, que permite que você carregar automaticamente os dados de seu armazenamento de BLOBs do local para Azure com suporte à conectividade de internet intermitentes. Ele permite que você:

- Ativar ou desativar o recurso de disposição em camadas
- Escolha a ordem na qual os dados são copiados para o Azure como NewestFirst ou OldestFirst
- Especifique a conta de armazenamento do Azure ao qual você deseja que os dados carregados.
- Especifique os contêineres que você deseja carregar no Azure. Este módulo permite que você especifique os nomes de contêiner de origem e destino.
- Completa a disposição em camadas de blob (usando `Put Blob` operação) e as camadas de nível de bloco (usando `Put Block` e `Put Block List` operações).

Este módulo usa o bloco nível disposição em camadas, quando seu blob consiste em blocos. Aqui estão alguns dos cenários comuns:

- Seu aplicativo atualiza alguns blocos de um blob carregado anteriormente, esse módulo carrega apenas os blocos atualizados e não o blob inteiro.
- O módulo é carregar o blob e conexão de internet desaparece, quando a conectividade está de volta novamente que ele carrega apenas os blocos restantes e não o blob inteiro.

Se ocorrer um encerramento inesperado do processo (como falha de energia) durante o carregamento de um blob, todos os blocos que foram devidos para o upload serão carregados novamente, quando o módulo fica online novamente.

**Time-to-live** (TTL) é uma funcionalidade configurável em que o módulo exclui automaticamente seus blobs do armazenamento local quando expira o período especificado (medido em minutos). TTL permite que você:

- Ativar ou desativar o recurso de disposição em camadas
- Especifique o TTL em minutos

Cenários onde os dados, como vídeos, imagens, dados financeiros, dados hospital ou todos os dados que precisam ser armazenados localmente, mais tarde que pode ser processados localmente ou transferidos para a nuvem são bons exemplos para usar este módulo.

Este artigo fornece instruções para implantar um Armazenamento de Blobs do Azure no contêiner do IoT Edge que executa um serviço blob no dispositivo do IoT Edge.

> [!NOTE]
> Os termos "disposição em camadas automática" e "expiração automática" usada no vídeo foram substituídas por "camadas" e "time-to-live".

Assista ao vídeo para introdução rápida
> [!VIDEO https://www.youtube.com/embed/wkprcfVidyM]

## <a name="prerequisites"></a>Pré-requisitos

Um dispositivo do Azure IoT Edge:

- Você pode usar seu computador de desenvolvimento ou uma máquina virtual como um dispositivo IoT Edge, seguindo as etapas no guia de início rápido para [Linux](quickstart-linux.md) ou [dispositivos Windows](quickstart.md).

- O Armazenamento de Blobs do Azure no módulo do IoT Edge dá suporte às seguintes configurações de dispositivo:

  | Sistema operacional | Arquitetura |
  | ---------------- | ----- | ----- |
  | Ubuntu Server 16.04 | AMD64 |
  | Ubuntu Server 18.04 | AMD64 |
  | Windows 10 IoT Core (atualização de outubro) | AMD64 |
  | Windows 10 IoT Enterprise (atualização de outubro) | AMD64 |
  | Windows Server 2019 | AMD64 |
  | Raspbian-stretch | ARM32 |

Recursos de nuvem:

Um [Hub IoT](../iot-hub/iot-hub-create-through-portal.md) na camada padrão no Azure.

## <a name="tiering-and-time-to-live-properties"></a>Propriedades de tempo de vida e em camadas

Use propriedades desejadas para definir a disposição em camadas e propriedades de tempo de vida. Eles podem ser definidos durante a implantação ou alterados mais tarde editando o gêmeo do módulo sem a necessidade de reimplantar. Recomendamos verificar "Gêmeo do módulo" para `reported configuration` e `configurationValidation` para garantir que os valores sejam propagados corretamente.

### <a name="tiering-properties"></a>Propriedades em camadas

O nome dessa configuração é `tieringSettings`

| Campo | Valores possíveis | Explicação |
| ----- | ----- | ---- |
| tieringOn | verdadeiro, falso | Por padrão, ele é definido como `false`, se você quiser ativá-lo na defini-lo como `true`|
| backlogPolicy | NewestFirst, OldestFirst | Permite que você escolha a ordem na qual os dados são copiados para o Azure. Por padrão, ele é definido como `OldestFirst`. A ordem é determinada pela hora da última modificação do Blob |
| remoteStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` uma cadeia de caracteres de conexão que permite que você especifique a conta de armazenamento do Azure ao qual você deseja que seus dados é carregada. Especificar `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix`. Adicionar apropriado EndpointSuffix do Azure onde os dados serão carregados, varia para Global do Azure, Azure governamental e Microsoft Azure Stack. |
| tieredContainers | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Permite que você especifique os nomes de contêiner que você deseja carregar no Azure. Este módulo permite que você especifique os nomes de contêiner de origem e destino. Se você não especificar o nome do contêiner de destino, ele atribuirá automaticamente o nome do contêiner como `<IoTHubName>-<IotEdgeDeviceName>-<ModuleName>-<ContainerName>`. Você pode criar cadeias de caracteres de modelo para o nome do contêiner de destino, verifique a coluna de valores possíveis. <br>* %h -> nome do Hub IoT (3 a 50 caracteres). <br>* %d -> ID do dispositivo IoT (1 a 129 caracteres). <br>* %m -> nome do módulo (1 a 64 caracteres). <br>* %c -> nome do contêiner de origem (3 a 63 caracteres). <br><br>Tamanho máximo do nome do contêiner é 63 caracteres, enquanto atribuir automaticamente o nome do contêiner de destino se o tamanho do contêiner exceder 63 caracteres, que ele será cortar cada seção (IoTHubName, IotEdgeDeviceName, ModuleName, ContainerName) a 15 caracteres. |

### <a name="time-to-live-properties"></a>Propriedades de tempo de vida

O nome dessa configuração é `ttlSettings`

| Campo | Valores possíveis | Explicação |
| ----- | ----- | ---- |
| ttlOn | verdadeiro, falso | Por padrão, ele é definido como `false`, se você quiser ativá-lo na defini-lo como `true`|
| timeToLiveInMinutes | `<minutes>` | Especifique o TTL em minutos. O módulo excluirá automaticamente seus blobs do armazenamento local quando o TTL expire |

## <a name="configure-log-files"></a>Configurar arquivos de log

Para obter informações sobre como configurar arquivos de log para o seu módulo, consulte estes [práticas recomendadas de produção](https://docs.microsoft.com/azure/iot-edge/production-checklist#set-up-logs-and-diagnostics).

## <a name="connect-to-your-blob-storage-module"></a>Conectar ao módulo do armazenamento de blobs

É possível usar o nome da conta e a chave de conta que você configurou para o módulo para acessar o armazenamento de blobs no seu dispositivo do IoT Edge.

Especifique o dispositivo do IoT Edge como ponto de extremidade do blob para quaisquer solicitações de armazenamento que você faz para esse dispositivo. É possível [Criar uma cadeia de conexão para um ponto de extremidade explícito](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) usando as informações do dispositivo do IoT Edge e do nome da conta que você configurou.

- Para módulos que são implantados no mesmo dispositivo, como onde o armazenamento de BLOBs do Azure no módulo do IoT Edge está em execução, o ponto de extremidade do blob é: `http://<module name>:11002/<account name>`.
- Para módulos que são implantados em um dispositivo diferente onde o armazenamento de BLOBs do Azure no módulo do IoT Edge está em execução, em seguida, dependendo de sua configuração o blob de ponto de extremidade é um de:
  - `http://<device IP >:11002/<account name>`
  - `http://<IoT Edge device hostname>:11002/<account name>`
  - `http://<fully qualified domain name>:11002/<account name>`

## <a name="azure-blob-storage-quickstart-samples"></a>Exemplos de início rápido do armazenamento de BLOBs do Azure

A documentação do Armazenamento de Blobs do Azure inclui guias de início rápido que fornecem códigos de exemplo em várias linguagens. Você pode executar esses exemplos para testar o armazenamento de BLOBs do Azure no IoT Edge, alterando o ponto de extremidade de blob para se conectar ao seu módulo de armazenamento de blob.

As seguintes guias de início rápido também contam com suporte do IoT Edge, assim é possível implantá-las como módulos do IoT Edge juntamente com o módulo do armazenamento de blobs:

- [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
- [Java](../storage/blobs/storage-quickstart-blobs-java.md)
- [Python](../storage/blobs/storage-quickstart-blobs-python.md)
- [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="connect-to-your-local-storage-with-azure-storage-explorer"></a>Conectar-se para o armazenamento local com o Gerenciador de armazenamento do Azure

Você pode usar **Azure Storage Explorer** para se conectar à sua conta de armazenamento local. Isso só está disponível com [Gerenciador de armazenamento do Azure versão 1.5.0](https://github.com/Microsoft/AzureStorageExplorer/releases/tag/v1.5.0).

> [!NOTE]
> Você pode encontrar erros ao executar as etapas a seguir, como adicionar uma conexão a uma conta de armazenamento local, ou criar contêineres na conta de armazenamento local. Ignore e atualizar.

1. Baixe e instale o Gerenciador de armazenamento do Azure

1. Conectar ao armazenamento do Azure usando uma cadeia de caracteres de conexão

1. Forneça a cadeia de caracteres de conexão: `DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`

1. Percorra as etapas para se conectar.

1. Criar contêiner dentro de sua conta de armazenamento local

1. Inicie o upload de arquivos como blobs de blocos.
   > [!NOTE]
   > Esse módulo não oferece suporte a blobs de página.

1. Você pode optar por conectar suas contas de armazenamento do Azure em que você está carregando os dados. Ele oferece uma exibição única para sua conta de armazenamento local e a conta de armazenamento do Azure

## <a name="supported-storage-operations"></a>Operações de armazenamento com suporte

Módulos de armazenamento de blob no IoT Edge usam os mesmos SDKs de armazenamento do Azure e são consistentes com a versão 2017-04-17 da API de armazenamento do Azure para pontos de extremidade de blob de bloco. Versões posteriores são dependentes das necessidades do cliente.

Porque nem todas as operações de armazenamento de BLOBs do Azure têm suporte pelo armazenamento de BLOBs do Azure no IoT Edge, esta seção lista o status de cada.

### <a name="account"></a>Conta

Com suporte:

- Listar contêineres

Sem suporte:

- Obter e definir propriedades do serviço Blob
- Solicitação de blob de simulação
- Obter estatísticas do serviço Blob
- Obter Informações da conta

### <a name="containers"></a>Contêineres

Com suporte:

- Criar e excluir contêiner
- Obter Propriedades do Contêiner
- Listar blobs
- Obter e definir ACL do contêiner
- Definir Metadados do Contêiner

Sem suporte:

- Concessão de contêiner

### <a name="blobs"></a>Blobs

Com suporte:

- Colocar, obter e excluir blob
- Obter e definir propriedades do blob
- Obter e definir Metadados do Blob

Sem suporte:

- Concessão de blob
- Instantâneo do blob
- Copiar e cancelar cópia do blob
- Cancelar exclusão do blob
- Definir camada do blob

### <a name="block-blobs"></a>Blobs de bloco

Com suporte:

- Colocar bloco
- Colocar e obter lista de blocos

Sem suporte:

- Colocar bloco pela URL

## <a name="feedback"></a>Comentários

Seus comentários são importantes para nós para tornar esse módulo e seus recursos úteis e fácil de usar. Compartilhe seus comentários e informe-nos como podemos melhorar.

Você pode entrar em contato conosco em absiotfeedback@microsoft.com

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre [implantar o armazenamento de BLOBs do Azure no IoT Edge](how-to-deploy-blob.md)

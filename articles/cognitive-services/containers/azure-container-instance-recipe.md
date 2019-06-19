---
title: Receita de instância de contêiner do Azure
titleSuffix: Azure Cognitive Services
description: Saiba como implantar contêineres de serviços Cognitivos na instância de contêiner do Azure
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 06/12/2019
ms.author: dapine
ms.openlocfilehash: 5de11b62bb2a2302c247907627e27339400fcf25
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207486"
---
# <a name="deploy-and-run-container-on-azure-container-instance-aci"></a>Implante e execute o contêiner na instância de contêiner do Azure (ACI)

Com as etapas a seguir, dimensionem aplicativos de serviços Cognitivos do Azure na nuvem com facilidade com o Azure [instância de contêiner](https://docs.microsoft.com/azure/container-instances/) (ACI). Essa ajuda você a focar na criação de seus aplicativos em vez de gerenciar a infraestrutura.

## <a name="prerequisites"></a>Pré-requisitos

Essa solução funciona com qualquer contêiner de serviços Cognitivos. O recurso de serviço cognitivo deve ser criado no portal do Azure antes de usar essa receita. Cada serviço cognitivo que dá suporte a contêineres tem um documento de "Como instalar o" especificamente para instalar e configurar o serviço para um contêiner. Como alguns serviços exigem um arquivo ou conjunto de arquivos como entrada para o contêiner, é importante que você compreenda e usou o contêiner com êxito antes de usar essa solução.

* Um recurso de serviço cognitivo, criado no portal do Azure.
* Serviço cognitivo **URL de ponto de extremidade** -examine seu do serviço específico "como instalar o" para o contêiner, para localizar onde a URL de ponto de extremidade é de dentro do portal do Azure e o que um exemplo correto da URL é semelhante. O formato exato pode alterar o serviço.
* Serviço cognitivo **chave** -as chaves são sobre o **chaves** página para o recurso do Azure. Você precisa apenas de uma das duas chaves. A chave é uma cadeia de caracteres de 32 caracteres alfanuméricos.
* Um único cognitivas contêiner de serviços em seu host local (computador). Certifique-se de que você pode:
  * Baixar a imagem com um `docker pull` comando.
  * Execute o contêiner local com êxito com todas as definições de configuração necessárias, com um `docker run` comando.
  * Chame o ponto de extremidade do contêiner, obter uma resposta de 2xx e uma resposta JSON de volta.

Todas as variáveis entre colchetes angulares, `<>`, precisam ser substituídos por seus próprios valores. Essa substituição inclui os colchetes angulares.

## <a name="step-2-launch-your-container-on-azure-container-instances-aci"></a>Etapa 2: Inicie o contêiner em instâncias de contêiner do Azure (ACI)

**Criando o recurso de instância de contêiner do Azure (ACI).**

1. Vá para o [criar](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) página para instâncias de contêiner.

1. Sobre o **Noções básicas de** , insira os detalhes a seguir:

    |Página|Configuração|Value|
    |--|--|--|
    |Noções básicas|Assinatura|Selecione sua assinatura.|
    |Noções básicas|Grupo de recursos|Selecione o grupo de recursos disponíveis ou crie um novo, como `cognitive-services`.|
    |Noções básicas|Nome do contêiner|Insira um nome como `cognitive-container-instance`. Esse nome deve ser inferior em maiusculas.|
    |Noções básicas|Local padrão|Selecione uma região para a implantação.|
    |Noções básicas|Tipo de Imagem|`Public`|
    |Noções básicas|Nome da imagem|Insira o local do contêiner de serviços Cognitivos. Isso pode ser usado no mesmo local do `docker pull` comando, _por exemplo_: <br>`mcr.microsoft.com/azure-cognitive-services/sentiment`|
    |Noções básicas|Tipo do SO|`Linux`|
    |Noções básicas|Tamanho|Alterar o tamanho para as recomendações sugeridas para seu contêiner específico do serviço cognitivo.:<br>2 núcleos<br>4 GB
    ||||
  
1. Sobre o **rede** , insira os detalhes a seguir:

    |Página|Configuração|Value|
    |--|--|--|
    |Rede|Portas|Edite a porta existente para TCP da `80` para `5000`. Isso significa que você está expondo o contêiner na porta 5000.|
    ||||

1. Sobre o **avançado** , insira os detalhes a seguir para passar o contêiner necessário cobrança configurações para o recurso de instância de contêiner:

    |Chave da página Avançado|Valor de página Avançado|
    |--|--|
    |`apikey`|Copiado do **chaves** página do recurso. Você precisa apenas uma das duas chaves. É uma cadeia de caracteres alfanuméricos 32 caracteres sem espaços ou traços, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Copiado do **visão geral** página do recurso. |
    |`eula`|`accept`|

    Se o contêiner tiver outras definições de configuração como montagens de entrada, saída de montagens ou registro em log, essas configurações também precisam ser adicionados.

1. Selecione **examinar e criar**.
1. Após a validação é bem-sucedida, selecione **criar** para concluir o processo de criação.
1. Selecione o ícone de sino no painel de navegação superior. Esta é a janela de notificação. Ele exibirá uma azul **ir para o recurso** botão quando o recurso é criado. Selecione esse botão para acessar o novo recurso.

## <a name="use-the-container-instance"></a>Usar a instância de contêiner

1. Selecione o **visão geral** e copie o endereço IP. Ele será um endereço IP numérico como `55.55.55.55`.
1. Abra uma nova guia do navegador e usar o endereço IP, por exemplo, `http://<IP-address>:5000 (http://55.55.55.55:5000`). Você verá homepage do contêiner, permitindo que você saiba que o contêiner está sendo executado.

1. Selecione **descrição do serviço API** para exibir a página de swagger para o contêiner.

1. Selecione qualquer uma da **POST** APIs e selecione **experimentá-lo**.  Os parâmetros são exibidos, incluindo a entrada. Preencha os parâmetros.

1. Selecione **Execute** para enviar a solicitação à instância de contêiner.

    Você criou e usado os contêineres de serviços Cognitivos na instância de contêiner do Azure com êxito.

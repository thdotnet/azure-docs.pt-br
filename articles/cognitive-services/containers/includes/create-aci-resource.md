---
title: Suporte a contêiner
titleSuffix: Azure Cognitive Services
description: Saiba como criar um recurso de instância (ACI) do contêiner do azure.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: b24a78873c3220b969fc548d2553e465e80773cd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455065"
---
## <a name="create-an-azure-container-instance-aci-resource"></a>Criar um recurso de instância de contêiner do Azure (ACI)

1. Vá para o [criar](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) página para instâncias de contêiner.

2. Sobre o **Noções básicas de** , insira os detalhes a seguir:

    |Configuração|Valor|
    |--|--|
    |Assinatura|Selecione sua assinatura.|
    |Grupo de recursos|Selecione o grupo de recursos disponíveis ou crie um novo, como `cognitive-services`.|
    |Nome do contêiner|Insira um nome como `cognitive-container-instance`. Esse nome deve ser inferior em maiusculas.|
    |Location|Selecione uma região para a implantação.|
    |Tipo de Imagem|`Public`|
    |Nome da imagem|Insira o local do contêiner de serviços Cognitivos. Isso pode ser usado no mesmo local do `docker pull` comando, _por exemplo_: <br>`mcr.microsoft.com/azure-cognitive-services/sentiment`|
    |Tipo do SO|`Linux`|
    |Tamanho|Alterar o tamanho para as recomendações sugeridas para seu contêiner específico do serviço cognitivo.:<br>2 núcleos<br>4 GB

3. Sobre o **rede** , insira os detalhes a seguir:

    |Configuração|Value|
    |--|--|
    |Portas|Defina a porta TCP como `5000`. Expõe o contêiner na porta 5000.|

4. Sobre o **avançado** , insira os detalhes a seguir para passar o contêiner [necessário cobrança configurações](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers#billing-arguments) para o recurso ACI:

    |Chave da página Avançado|Valor de página Avançado|
    |--|--|
    |`apikey`|Copiado do **chaves** página do recurso de análise de texto. É uma cadeia de caracteres alfanuméricos 32 caracteres sem espaços ou traços, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Copiado do **visão geral** página do recurso de análise de texto. Exemplo: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |`eula`|`accept`|

1. Clique em **examinar e criar**
1. Após a validação é bem-sucedida, clique em **criar** para concluir o processo de criação
1. Quando o recurso é implantado com êxito, ele estará pronto para uso
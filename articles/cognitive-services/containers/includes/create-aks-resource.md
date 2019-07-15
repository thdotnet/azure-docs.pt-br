---
title: Criar um recurso de cluster do serviço Kubernetes do Azure
titleSuffix: Azure Cognitive Services
description: Saiba como criar um recurso do serviço de Kubernetes do Azure (AKS).
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: ab7ce8b4a538e6a286a00285069a22878c5d88d9
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877456"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Criar um recurso de cluster do serviço Kubernetes do Azure

1. Vá para [serviço Kubernetes do Azure](https://ms.portal.azure.com/#create/microsoft.aks) e selecione **criar**.

1. Sobre o **Noções básicas de** , insira as seguintes informações:

    |Configuração|Valor|
    |--|--|
    |Assinatura|Selecione a assinatura apropriada|
    |Grupo de recursos|Selecione um grupo de recursos disponíveis|
    |Nome do cluster Kubernetes|Insira um nome (em minúsculas)|
    |Região|Selecione um local próximo|
    |Versão do Kubernetes|1.12.8 (padrão)|
    |Prefixo de nome DNS|Criado automaticamente, mas você pode substituir|
    |Tamanho do nó|Standard DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Contagem de nós|Deixe o controle deslizante com o valor padrão|

1. Sobre o **escala** guia, deixe **nós virtuais** e **(visualização) de conjuntos de dimensionamento de máquina Virtual** definidas para os valores padrão.
1. Sobre o **autenticação** guia, deixe **entidade de serviço** e **RBAC habilitar** definidas para os valores padrão.
1. Sobre o **rede** , insira as seguintes seleções:

    |Configuração|Valor|
    |--|--|
    |Roteamento de aplicativo HTTP|Não|
    |Configuração de rede|Basic|

1. No **Monitoring** guia, certifique-se de que **habilitar o monitoramento de contêiner** é definido como **Sim**e deixar **espaço de trabalho do Log Analytics** como o valor padrão.
1. Sobre o **marcas** guia, deixe os pares nome/valor em branco por enquanto.
1. Selecione **examinar e criar**.
1. Após a validação é bem-sucedida, selecione **criar**.

> [!NOTE]
> Se a validação falhar, pode ser devido a um erro de "Entidade de serviço". Volte para o **autenticação** guia e, em seguida, de volta para **revisar + criar**, onde validação deve executar e, em seguida, passar.

---
title: Suporte a contêiner
titleSuffix: Azure Cognitive Services
description: Saiba como criar um azure kubernetes (AKS) um recurso.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 1d68c08f6dfca74c38973af1686d614f3f10cc28
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455088"
---
## <a name="create-an-azure-kubernetes-service-aks-cluster-resource"></a>Criar um recurso de cluster do serviço de Kubernetes do Azure (AKS)

1. Vá para o [criar](https://ms.portal.azure.com/#create/microsoft.aks) para serviços de Kubernetes.

1. Sobre o **Noções básicas de** , insira os detalhes a seguir:

    |Configuração|Valor|
    |--|--|
    |Assinatura|Selecione a assinatura apropriada|
    |Grupo de recursos|Selecione um grupo de recursos disponíveis|
    |Nome do cluster Kubernetes|Nome desejado (em minúsculas)|
    |Região|Selecione um local próximo|
    |Versão do Kubernetes|1.12.8 (padrão)|
    |Prefixo de nome DNS|Criado automaticamente, mas pode, opcionalmente, substituir|
    |Tamanho do nó|Standard DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Contagem de nós|Deixe o controle deslizante com o valor padrão|

1. Sobre o **dimensionamento** guia, deixe o *nós virtuais* e *conjuntos de dimensionamento de máquina virtual* valores padrão.
1. Sobre o **autenticação** guia, deixe *entidade de serviço* e *RBAC habilitar* valores padrão.
1. Sobre o **rede** , insira as seguintes seleções:

    |Configuração|Value|
    |--|--|
    |Roteamento de aplicativo HTTP|Não|
    |Configuração de rede|Basic|

1. No **monitoramento** guia, certifique-se de que *habilitar o monitoramento de contêiner* está definido como **Sim** e deixe o *espaço de trabalho do Log Analytics* como seu valor padrão
1. Sobre o **marcas** guia, deixe os pares nome/valor em branco por enquanto.
1. Clique em **examinar e criar**.
1. Após a validação é bem-sucedida, clique em **criar**.

> [!NOTE]
> Se a validação falhar, pode ser devido uma *entidade de serviço* erro. Navegue de volta para o *autenticação* guia e de volta aos *revisão + criar* em que a validação deve executar e, em seguida, passar.

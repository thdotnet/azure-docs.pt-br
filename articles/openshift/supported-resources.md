---
title: Recursos com suporte para Azure Red Hat OpenShift | Microsoft Docs
description: Entenda quais regiões do Azure e os tamanhos de máquina virtual são suportados pelo Microsoft Azure Red Hat OpenShift.
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/15/2019
ms.openlocfilehash: 5182a5e325bd7883af1a7d102d3e02b277a5089e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65788709"
---
# <a name="azure-red-hat-openshift-resources"></a>Recursos do Azure do Red Hat OpenShift

Este tópico lista as regiões do Azure e os tamanhos de máquina virtual com suporte pelo serviço do Microsoft Azure Red Hat OpenShift.

## <a name="azure-regions"></a>Regiões do Azure

Ver [produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/?products=openshift&regions=all) para obter uma lista atual de regiões em que você pode implantar o Azure Red Hat OpenShift clusters.

## <a name="virtual-machine-sizes"></a>Tamanhos das máquinas virtuais

Estes são os tamanhos de máquina virtual com suporte que você pode especificar os nós de computação no cluster do Azure Red Hat OpenShift.

> [!Important]
> Cada VM tem um número diferente de unidades que podem ser anexados. Isso pode não ser tão imediatamente claro como memória ou o tamanho da CPU.
> Nem todos os tamanhos de VM estão disponíveis em todas as regiões. Mesmo que a API dá suporte ao tamanho especificado, você poderá receber um erro se o tamanho não está disponível na região em que você especificar.
> Ver [tamanhos de lista atual de VM com suporte por região](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) para obter mais informações.

## <a name="compute-node-sizes"></a>Tamanhos de nós de computação

Os seguintes tamanhos de nó de computação são suportados pela API de REST do Azure Red Hat OpenShift:

|Tamanho|vCPU|RAM|
|-|-|-|
|Standard D4s v3|4|16 GB|
|Standard D8s v3|8|32 GB|
|Standard D16s v3|16|64 GB|
|Standard D32s v3|32|128 GB|
|-|-|-|
|Standard E4s v3|4|32 GB|
|Standard E8s v3|8|64 GB|
|Standard E16s v3|16|128 GB|
|Standard E32s v3|32|256 GB|
|-|-|-|
|F8s v2 Standard|8|16 GB|
|F16s v2 Standard|16|32 GB|
|F32s v2 Standard|32|64 GB|

## <a name="master-node-sizes"></a>Tamanhos de nó mestre

O seguinte mestre / de tamanhos de nós de infraestrutura são compatíveis com a API REST do Azure Red Hat OpenShift:

|Tamanho|vCPU|RAM|
|-|-|-|
|Standard D4s v3|4|16 GB|
|Standard D8s v3|8|32 GB|
|Standard D16s v3|16|64 GB|
|Standard D32s v3|32|128 GB|

## <a name="next-steps"></a>Próximas etapas

Experimente o [criar um cluster do Azure Red Hat OpenShift](tutorial-create-cluster.md) tutorial.
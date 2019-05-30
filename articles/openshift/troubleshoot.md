---
title: Solucionar problemas do Azure Red Hat OpenShift | Microsoft Docs
description: Solucionar problemas e resolver problemas comuns com o Azure Red Hat OpenShift
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.service: container-service
ms.topic: troubleshooting
ms.date: 05/08/2019
ms.openlocfilehash: 7f2bdf643f12671bec3d0c087d8775844099fe9a
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306256"
---
# <a name="troubleshooting-for-azure-red-hat-openshift"></a>Solução de problemas do Azure Red Hat OpenShift

Este artigo fornece detalhes sobre alguns problemas comuns encontrados ao criar ou gerenciar clusters do Microsoft Azure Red Hat OpenShift.

## <a name="retrying-the-creation-of-a-failed-cluster"></a>Repetir a criação de um cluster com falha

Se criar um Azure Red Hat OpenShift cluster usando o `az` falha do comando da CLI, repetir o criar continuará a falhar.
Use `az openshift delete` para excluir o cluster com falha, em seguida, crie um cluster totalmente novo.

## <a name="hidden-azure-red-hat-openshift-cluster-resource-group"></a>Grupo de recursos de cluster do Azure Red Hat OpenShift oculto

Atualmente, o `Microsoft.ContainerService/openShiftManagedClusters` recurso que é criado automaticamente com a CLI do Azure (`az openshift create` comando) está oculta no portal do Azure. No **grupo de recursos** modo de exibição, seleção **Mostrar tipos ocultos** para exibir o grupo de recursos.

![Captura de tela da caixa de seleção no portal do tipo oculto](./media/aro-portal-hidden-type.png)

## <a name="creating-a-cluster-results-in-error-that-no-registered-resource-provider-found"></a>Criando um cluster resultados no erro não encontrado nenhum provedor de recursos registrado

Se criar um cluster resulta em um erro `No registered resource provider found for location '<location>' and API version '2019-04-30' for type 'openShiftManagedClusters'. The supported api-versions are '2018-09-30-preview`, em seguida, foram parte da visualização e agora precisa [instâncias reservadas de máquina virtual do Azure de compra](https://aka.ms/openshift/buy) para usar o produto disponível. Uma reserva reduz seus gastos por pagar previamente para serviços totalmente gerenciados do Azure. Consulte a [ *o que são Azure reservas* ](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) para saber mais sobre reservas e como eles economizar seu dinheiro.

## <a name="next-steps"></a>Próximas etapas

- Experimente o [Centro de Ajuda do Red Hat OpenShift](https://help.openshift.com/) para obter mais sobre OpenShift solução de problemas.

- Encontre respostas para [perguntas frequentes sobre o Azure Red Hat OpenShift](openshift-faq.md).

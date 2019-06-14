---
title: Modelo de custo para arquivos do Azure NetApp | Microsoft Docs
description: Descreve o modelo de custo para arquivos do NetApp do Azure para o gerenciamento de despesas do serviço.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: b-juche
ms.openlocfilehash: b06e3366224b90899dd3f9f9439edf897de82794
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65524214"
---
# <a name="cost-model-for-azure-netapp-files"></a>Modelo de custo para o Azure NetApp Files 

Noções básicas sobre o modelo de custo para arquivos do NetApp do Azure ajuda você a gerenciar seus gastos do serviço.

## <a name="calculation-of-capacity-consumption"></a>Cálculo de consumo da capacidade

Os arquivos do Azure do NetApp é cobrado a capacidade de armazenamento provisionado.  Capacidade provisionada é alocada com a criação de pools de capacidade.  Pools de capacidade são cobrados com base em $/ provisionado-GiB/mês em incrementos por hora. O tamanho mínimo para um pool de capacidade de único é de 4 TiB e pools de capacidade podem ser expandidos posteriormente em incrementos de 1 TiB. Volumes são criados dentro de pools de capacidade.  Cada volume é atribuído a uma cota que decrementa a partir da capacidade provisionada para pools. A cota que pode ser atribuída aos volumes intervalos entre o mínimo de 100 GiB a um máximo de TiB 92.  

Para um volume ativo, o consumo da capacidade em relação à cota é com base na capacidade de (efetiva) lógica.

Se o consumo de capacidade real de um volume exceder sua cota de armazenamento, o volume pode continuar a crescer. Gravações ainda serão permitidas desde que o tamanho do volume real é menor que o limite do sistema (100 TiB).  

A capacidade total utilizada em um pool de capacidade em relação a sua quantidade provisionado é a soma de maior de ambos a cota atribuída ou real consumo de todos os volumes dentro do pool: 

   ![Cálculo de capacidade total utilizada](../media/azure-netapp-files/azure-netapp-files-total-used-capacity.png)

O diagrama a seguir ilustra esses conceitos.  
* Temos um pool de capacidade com 4 TiB de capacidade provisionada.  O pool contém três volumes.  
    * Volume 1 é atribuído a uma cota de 2 TiB e tem 800 GiB de consumo.  
    * Volume 2 é atribuído a uma cota de 1 TiB e tem 100 GiB de consumo.  
    * Volume 3 é atribuído a uma cota de 500 GiB, mas tem 800 GiB de consumo (excedente).  
* O pool de capacidade é limitado para 4 TiB de capacidade (a quantidade provisionada).  
    3,8 TiB de capacidade é consumido (TiB 2 e 1 TiB de cota de Volumes 1 e 2 e 800 GiB de consumo real para o Volume 3). E a 200 GiB de capacidade restante.

   ![Pool de capacidade com três volumes](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-three-vols.png)

## <a name="overage-in-capacity-consumption"></a>Excedente em consumo da capacidade  

Quando o total usado a capacidade de um pool excede sua capacidade provisionada, gravações de dados ainda são permitidas.  Após o período de cortesia (uma hora), se a capacidade utilizada do pool ainda exceder sua capacidade provisionada, em seguida, o tamanho do pool será automaticamente aumentado em incrementos de 1 TiB até que a capacidade provisionada é maior que a capacidade total utilizada.  Por exemplo, na ilustração acima, se o Volume 3 continua a crescer e o consumo real atinge 1,2 TiB, em seguida, após o período de cortesia, o pool de serão automaticamente redimensionado para 5 TiB.  O resultado é que a capacidade do pool provisionada (5 TiB) excede a capacidade utilizada (TiB 4.2).  

## <a name="manual-changes-of-the-pool-size"></a>Alterações manuais do tamanho do pool  

Você pode aumentar ou diminuir o tamanho do pool manualmente. No entanto, as seguintes restrições se aplicam:
* Limites mínimo e máximo de serviço  
    Consulte o artigo [limites de recursos](azure-netapp-files-resource-limits.md).
* Um incremento de 1 TiB após a compra de mínimo de 4 TiB inicial
* Um incremento de cobrança mínimo de uma hora
* O tamanho do pool provisionados não pode ser diminuído para menor do que o total de capacidade usadas no pool.

## <a name="behavior-of-maximum-size-pool-overage"></a>Comportamento de excedente de tamanho máximo de pool   

O tamanho máximo de um pool de capacidade que você pode criar ou redimensionar para é 500 TiB.  Quando o total de capacidade usadas em um pool de capacidade excede 500 TiB, ocorrerão as seguintes situações:
* Gravações de dados ainda serão permitidas (se o volume estiver abaixo do máximo de sistema de 100 TiB).
* Após o período de cortesia de uma hora, o pool será automaticamente redimensionado em incrementos de 1 TiB, até que a capacidade do pool provisionado excede a capacidade total utilizada.
* Provisionado e cobradas a capacidade do pool de que 500 TiB excedendo não pode ser usada para atribuir cota do volume adicional. Ele também não pode ser usado para expandir os limites de QoS de desempenho.  
    Ver [níveis de serviço](azure-netapp-files-service-levels.md) sobre os limites de desempenho e dimensionamento de QoS.

O diagrama a seguir ilustra esses conceitos:
* Temos um pool de capacidade com uma camada de armazenamento Premium e uma capacidade de 500 TiB. O pool contém nove volumes.
    * Volumes de 1 a 8 são atribuídos a uma cota de 60 TiB cada.  A capacidade total utilizada é TiB 480.  
        Cada volume tem um limite de QoS de 3,75 GiB/s de taxa de transferência (TiB 60 * MiB/s 64).  
    * Volume 9 é atribuída uma cota de 20 TiB.  
        Volume 9 tem um limite de QoS de 1,25 GiB/s de taxa de transferência (TiB 60 * MiB/s 64).
* Volume 9 é um cenário de excedente. Ela tem 25 TiB de consumo real.  
    * Após o período de cortesia de uma hora, o pool de capacidade será redimensionado para TiB 505.  
        Ou seja, o total capacidade utilizada = 8 * 60-cota TiB de Volumes de 1 a 8 e 25 TiB de consumo real para o Volume 9.
    * A capacidade cobrada é TiB 505.
    * Cota do volume para o Volume 9 não pode ser aumentada (porque a cota total atribuída para o pool não pode exceder 500 TiB).
    * Taxa de transferência adicional de QoS não pode ser atribuída (pois o QoS total para o pool ainda é baseado em 500 TiB).

   ![Pool de capacidade com nove volumes](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-nine-vols.png)

## <a name="capacity-consumption-of-snapshots"></a>Consumo de capacidade de instantâneos 

O consumo da capacidade de instantâneos em arquivos do NetApp do Azure é cobrado em relação à cota do volume pai.  Como resultado, ele compartilha a mesma taxa de cobrança que o pool de capacidade ao qual pertence o volume.  No entanto, ao contrário do volume ativo, o consumo de instantâneo é medido com base na capacidade incremental consumida.  Instantâneos de arquivos NetApp do Azure são diferenciais por natureza. Dependendo da taxa de alteração dos dados, os instantâneos geralmente consomem muito menos capacidade do que a capacidade de lógica do volume ativo. Por exemplo, suponha que você tenha um instantâneo de um volume de 500 GiB que contém apenas 10 GiB de dados diferenciais. A capacidade que é cobrada em relação à cota do volume do instantâneo seria não 500 GiB de 10 GiB. 

## <a name="next-steps"></a>Próximas etapas

* [Página de preços do Azure os arquivos NetApp](https://azure.microsoft.com/pricing/details/storage/netapp/)
* [Níveis de serviço do Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Limites de recursos do Azure NetApp Files](azure-netapp-files-resource-limits.md)

---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/30/2019
ms.openlocfilehash: a463ac9f9584cb13cadbcf79674d56b2f8e47c2c
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753083"
---
| Destino de computação | Uso | Suporte de GPU | Suporte FPGA | DESCRIÇÃO |
| ----- | ----- | ----- | ----- | ----- |
| [Serviço da web local](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Teste/depuração | Talvez | &nbsp; | BOM para testar e solucionar problemas limitado. Aceleração de hardware depende do uso de bibliotecas no sistema local.
| [AKS (Serviço de Kubernetes do Azure)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Inferência de tipos em tempo real |  [Sim](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [Sim](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Ideal para implantações de produção em grande escala. Fornece o dimensionamento automático e tempos de resposta rápidos.  O AKS é a única opção disponível para a interface visual. |
| [ACI (Instâncias de Contêiner do Azure)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Teste ou desenvolvimento | &nbsp;  | &nbsp; | BOM para escala baixa, com base em CPU cargas de trabalho que exigem < 48 GB de RAM |
| [Computação do Azure Machine Learning](../articles/machine-learning/service/how-to-run-batch-predictions.md) | (Visualização) Inferência de tipos de lote | Sim | &nbsp;  | Execute computação sem servidor de pontuação em lote. Dá suporte a VMs normais e de baixa prioridade. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | (Visualização) Módulo do IoT |  &nbsp; | &nbsp; | Implantar e fornecer modelos de ML em dispositivos IoT. |
| [Borda da caixa de dados do Azure](../articles/databox-online/data-box-edge-overview.md)   | via IoT Edge |  &nbsp; | Sim | Implantar e fornecer modelos de ML em dispositivos IoT. |
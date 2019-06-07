---
title: Destinos de computação
titleSuffix: Azure Machine Learning service
description: Um destino de computação permite que você especifique o recurso de computação em que você executa o script de treinamento ou o host de sua implantação do serviço. Esse local pode ser seu computador local ou um recurso de computação em nuvem.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/30/2019
ms.openlocfilehash: 42c0f5460a63b781aafdd43410761e2d7b17944d
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755344"
---
#  <a name="what-is-a-compute-target-in-azure-machine-learning-service"></a>O que é um destino de computação no serviço do Azure Machine Learning? 

Um destino de computação permite que você especifique o recurso de computação em que você executa o script de treinamento ou o host de sua implantação do serviço. Esse local pode ser seu computador local ou um recurso de computação em nuvem.

Destinos de computação tornam fácil alterar seu ambiente de computação sem alterar seu código.  Um ciclo de vida de desenvolvimento do modelo típico:

* Comece com o dev/experimentação em uma pequena quantidade de dados. Nessa fase, é recomendável usar um ambiente local. Por exemplo, o computador local ou uma VM baseada em nuvem.
* Escalar verticalmente seu treinamento em grandes conjuntos de dados ou distribuída treinamento usando um dos [destinos de treinamento](#train).  
* Implantar em vários ambientes de hospedagem de web ou em dispositivos de IoT usando um dos [destinos de implantação](#deploy).

Os recursos de computação, você pode usar para seus destinos de computação são anexados a um [espaço de trabalho](concept-workspace.md). Recursos que não seja o computador local são compartilhados por usuários do espaço de trabalho de computação.

## <a name="train"></a> Destinos de treinamento

O serviço do Azure Machine Learning tem suporte variado em todos os recursos de computação diferentes.  Também é possível anexar seu próprio recurso de computação, embora o suporte para vários cenários possa variar conforme detalhado abaixo:

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]


## <a name="deploy"></a>Destinos de implantação

Os seguintes recursos de computação podem ser usados para hospedar sua implantação de modelo.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]


## <a name="managed-compute"></a>Computação gerenciada

Um recurso de computação gerenciada é criado e gerenciado pelo serviço do Azure Machine Learning. Essa computação é otimizada para cargas de trabalho de aprendizado de máquina. Computação do Azure Machine Learning é a única computação gerenciada a partir de 30 de maio de 2019. Recursos de computação gerenciados adicionais podem ser adicionados no futuro.

### <a name="amlcompute"></a> Computação de aprendizado de máquina do Azure

Você pode usar a computação do Azure Machine Learning para treinamento e Inferência de lote (versão prévia).  Com esse recurso de computação, você tem:

* Cluster único ou vários node
* É dimensionado automaticamente sempre que enviar uma execução 
* Gerenciamento de cluster automático e o agendamento de trabalho 
* Suporte para recursos de CPU e GPU

Você pode criar instâncias de computação do Azure Machine Learning com qualquer um dos seguintes:

* O Portal do Azure
* O Azure Machine Learning do SDK
* A CLI do Azure

Todos os outros recursos de computação devem ser criados fora do espaço de trabalho e, em seguida, anexados a ele.

## <a name="unmanaged-compute"></a>Computação não gerenciada

É um recurso de computação não gerenciado *não* gerenciado pelo serviço de Azure Machine Learning. Você deve cria esse tipo de computação fora do Azure Machine Learning e anexá-lo ao seu espaço de trabalho. Recursos de computação não gerenciado podem exigir etapas adicionais para você para manter ou melhorar o desempenho para cargas de trabalho de aprendizado de máquina.

## <a name="next-steps"></a>Próximas etapas

* [Configurar destinos de computação para treinamento de modelo](how-to-set-up-training-targets.md)
* [Implantar modelos com o serviço de Azure Machine Learning](how-to-deploy-and-where.md)
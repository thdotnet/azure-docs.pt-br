---
title: 'MLOps: Gerenciar, implantar e monitorar modelos de ML'
titleSuffix: Azure Machine Learning service
description: 'Saiba como usar o serviço Azure Machine Learning para MLOps: implantar, gerenciar e monitorar seus modelos para melhorar continuamente-los. Você pode implantar os modelos que treinou com os Serviços do Azure Machine Learning no computador local ou de outras fontes.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 0eaf48f57c3011222b71a63d703e1ccec7aca001
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692829"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Gerenciar, implantar e monitorar modelos com os Serviços do Azure Machine Learning

Neste artigo, saiba mais sobre como usar o serviço Azure Machine Learning para gerenciar o ciclo de vida de seus modelos. O Azure Machine Learning usa uma abordagem de operações de aprendizado de máquina (MLOps), o que melhora a qualidade e a consistência de soluções de aprendizado de máquina. O serviço do Azure Machine Learning fornece os seguintes recursos de MLOps:

* Integração com os Pipelines do Azure. Defina contínuos integração e implantação de fluxos de trabalho para seus modelos.
* Um registro de modelo que mantém várias versões dos modelos treinados.
* Validação do modelo. Automaticamente validar seus modelos treinados e selecione a configuração ideal para implantá-los em produção.
* Implante seus modelos como um serviço web na nuvem, localmente ou em dispositivos IoT Edge.
* Monitore o desempenho do seu modelo implantado, portanto, é possível conduzir a melhorias na próxima versão do modelo.

Para saber mais sobre os conceitos por trás de MLOps e como elas se aplicam ao serviço de Azure Machine Learning, assista ao vídeo a seguir.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="integration-with-azure-pipelines"></a>Integração com os Pipelines do Azure

Você pode usar Pipelines do Azure para criar um processo de integração contínua que treina um modelo. Em um cenário típico, quando um cientista de dados verifica uma alteração para o repositório de Git para um projeto, o Pipeline do Azure iniciará uma execução de treinamento. Os resultados da execução, em seguida, podem ser inspecionados para ver as características de desempenho do modelo treinado. Você também pode criar um pipeline que implanta o modelo como um serviço web.

O [extensão do Azure Machine Learning](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) torna mais fácil trabalhar com os Pipelines do Azure. Ele fornece os seguintes aprimoramentos para os Pipelines do Azure:

* Habilita a seleção de espaço de trabalho ao definir uma conexão de serviço.
* Permite que pipelines sejam disparados por modelos treinados criados em um pipeline de treinamento de versão.

Para obter mais informações sobre como usar Pipelines do Azure com o Azure Machine Learning, consulte o [integração contínua e implantação de modelos de ML com Pipelines do Azure](/azure/devops/pipelines/targets/azure-machine-learning) artigo e o [MLOps de serviço do Azure Machine Learning](https://aka.ms/mlops) repositório.

## <a name="convert-and-optimize-models"></a>Converter e otimizar os modelos

Convertendo seu modelo [Open Neural Network Exchange](https://onnx.ai) (ONNX) pode melhorar o desempenho. Em média, convertendo em ONNX pode produzir um aumento de desempenho de 2x.

Para obter mais informações sobre ONNX com o serviço Azure Machine Learning, consulte o [criar e acelerar os modelos de ML](concept-onnx.md) artigo.

## <a name="register-models"></a>Registrar os modelos

O registro de modelo permite que você armazene e controle a versão de seus modelos na nuvem do Azure em seu workspace. O registro de modelo torna mais fácil organizar e manter o controle de seus modelos treinados.

> [!TIP]
> Você também pode registrar os modelos treinados fora do serviço de Azure Machine Learning.
 
Modelos registrados são identificados por nome e versão. Cada vez que você registra um modelo com o mesmo nome de um já existente, o Registro incrementa a versão. Você também pode fornecer marcas de metadados adicionais durante o registro, que podem ser usadas ao pesquisar modelos. O serviço de Azure Machine Learning dá suporte a qualquer modelo que pode ser carregado usando o Python 3.5.2 ou superior.

Você não pode excluir modelos que estão sendo usados em uma implantação ativa.

Para obter mais informações, consulte a seção sobre registro de modelos em [Implantar modelos](how-to-deploy-and-where.md#registermodel).

Para obter um exemplo de registro de um modelo armazenado no formato pickle, consulte [Tutorial: treinar um modelo de classificação de imagem](tutorial-deploy-models-with-aml.md).

## <a name="package-and-debug-models"></a>Modelos de pacote e depuração

Antes de implantar um modelo em produção, ele é empacotado em uma imagem do Docker. Na maioria dos casos, criação de imagem são ocorre automaticamente, em segundo plano durante a implantação. Para cenários avançados, você pode especificar manualmente a imagem.

Se você tiver problemas com a implantação, você pode implantar em seu ambiente de desenvolvimento local para solução de problemas e depuração.

Para obter mais informações, consulte [implantar modelos](how-to-deploy-and-where.md#registermodel) e [Solucionando problemas de implantações](how-to-troubleshoot-deployment.md).

## <a name="validate-and-profile-models"></a>Validar e modelos de perfil

O serviço do Azure Machine Learning pode usar a criação de perfil para determinar as configurações de CPU e memória ideais para usar ao implantar seu modelo. Validação de modelo ocorre como parte desse processo, usando dados que você fornecer para o processo de criação de perfil.

## <a name="use-models"></a>Usar modelos

Modelos de aprendizado de máquina treinada pode ser implantada como serviços da web na nuvem ou localmente no seu ambiente de desenvolvimento. Você também pode implantar modelos em dispositivos do Azure IoT Edge. Implantações podem usar a CPU, GPU ou portões programáveis em campo FPGA (arranjo) para inferência. Você também pode usar modelos do Power BI.

Ao usar um modelo como um serviço web ou dispositivo do IoT Edge, você pode fornecer os seguintes itens:

* Os modelos que são usados para pontuar os dados enviados para o serviço/dispositivo.
* Um script de entrada. Esse script aceita solicitações, usa os modelos para pontuar os dados e retornar uma resposta.
* Um arquivo de ambiente do conda que descreve as dependências exigidas pelo script de modelo (s) e de entrada.
* Quaisquer ativos adicionais, como texto, dados, etc. que são necessários para o script de modelo (s) e entrada.

Esses ativos são empacotados em uma imagem do Docker e implantados como um serviço web ou o módulo do IoT Edge.

Opcionalmente, você pode usar os seguintes parâmetros para ajustar ainda mais a implantação:

* Enable GPU: Usado para habilitar o suporte GPU na imagem do Docker. A imagem deve ser usada nos serviços do Microsoft Azure, como instâncias de contêiner do Azure, serviço Kubernetes do Azure, computação do Azure Machine Learning ou máquinas virtuais do Azure.
* Etapas de arquivo do docker extra: Um arquivo que contém as etapas adicionais de Docker para ser executado ao criar a imagem do Docker.
* Imagem de base: Uma imagem personalizada para usar como a imagem base. Se você não usar uma imagem personalizada, a imagem base é fornecida pelo serviço de Azure Machine Learning.

Você também pode fornecer a configuração da plataforma de destino de implantação. Por exemplo, a VM tipo de família, a memória disponível e número de núcleos ao implantar o serviço Kubernetes do Azure.

Quando a imagem é criada, os componentes necessários ao serviço Azure Machine Learning também são adicionadas. Por exemplo, os ativos necessários para executar o serviço web e interagir com o IoT Edge.

> [!NOTE]
> Você não pode modificar ou alterar o servidor web ou componentes do IoT Edge usados na imagem do Docker. Serviço de Machine Learning do Azure usa uma configuração de servidor da web e componentes do IoT Edge que são testados e com suporte pela Microsoft.

### <a name="web-service"></a>Serviço Web

Você pode usar seus modelos no **serviços web** destinos de computação com o seguinte:

* Azure Container Instance
* Serviço de Kubernetes do Azure
* Ambiente de desenvolvimento local

Para implantar o modelo como um serviço web, você deve fornecer os seguintes itens:

* O modelo ou um ensemble de modelos.
* Dependências necessárias para usar o modelo. Por exemplo, um script que aceita solicitações e invoca o modelo, as dependências de conda, etc.
* Configuração de implantação que descreve como e onde implantar o modelo.

Para obter mais informações, consulte [implantar modelos](how-to-deploy-and-where.md).

### <a name="iot-edge-devices"></a>Dispositivos do IoT Edge


Você pode usar modelos com dispositivos IoT por meio **módulos do Azure IoT Edge**. Módulos do IoT Edge são implantados em um dispositivo de hardware, que permite a inferência de tipos ou modelo de pontuação, no dispositivo.

Para obter mais informações, consulte [implantar modelos](how-to-deploy-and-where.md).

### <a name="analytics"></a>Análise

Microsoft Power BI dá suporte a usando modelos de aprendizado de máquina para análise de dados. Para obter mais informações, consulte [integração do Azure Machine Learning no Power BI (visualização)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="monitor-and-collect-data"></a>Monitorar e coletar dados

Monitoramento permite que você entenda quais dados estão sendo enviados para o seu modelo e as previsões que ele retorna.

Essas informações ajudam você a entender como seu modelo está sendo usado. Os dados coletados de entrada também podem ser útil em versões futuras do treinamento do modelo.

Para obter mais informações, consulte [Como habilitar a coleta de dados de modelo](how-to-enable-data-collection.md).

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre [como e em que local você pode implantar modelos](how-to-deploy-and-where.md) com o serviço do Azure Machine Learning. Para obter um exemplo de implantação, consulte [Tutorial: Implantar um modelo de classificação de imagem em instâncias de contêiner do Azure](tutorial-deploy-models-with-aml.md).

Saiba como criar [integração contínua e implantação de modelos de ML com Pipelines do Azure](/azure/devops/pipelines/targets/azure-machine-learning). 

Saiba como criar aplicativos cliente e serviços que [Consomem um modelo implantado como um serviço Web](how-to-consume-web-service.md).

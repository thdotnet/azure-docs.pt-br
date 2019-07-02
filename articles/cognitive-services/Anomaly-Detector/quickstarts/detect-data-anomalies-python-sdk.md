---
title: 'Início Rápido: Detectar anomalias de dados usando a biblioteca do Detector de anomalias e o Python | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Use a API do Detector de anomalias para detectar anormalidades em sua série de dados como um lote ou no fluxo de dados.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 07/01/2019
ms.author: aahi
ms.openlocfilehash: 1d89ed8f40547142d41af9c587fc8fc000fa4dd9
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503670"
---
# <a name="quickstart-anomaly-detector-client-library-for-python"></a>Início Rápido: Biblioteca de cliente do Detector de anomalias para Python

Comece com a biblioteca de cliente do Detector de anomalias para .NET. Siga estas etapas para instalar o pacote e testar o código de exemplo para tarefas básicas. O serviço do Detector de anomalias permite que você localizar anormalidades em seus dados de série temporal usando automaticamente os modelos de melhor ajuste, independentemente do setor, o cenário ou o volume de dados.

## <a name="key-concepts"></a>Principais conceitos

Use a biblioteca de cliente do Detector de anomalias para Python para:

* Detectar anomalias em todo o seu conjunto de dados de série temporal, como uma solicitação de lote
* Detectar o status de anomalias do último ponto de dados da série de tempo

[Documentação de referência de biblioteca](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector?view=azure-python) | [código-fonte biblioteca](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-anomalydetector) | [pacote (PyPi)](https://pypi.org/project/azure-cognitiveservices-anomalydetector/) | [exemplos](https://github.com/Azure-Samples/anomalydetector)

## <a name="prerequisites"></a>Pré-requisitos

* Assinatura do Azure - [criar uma gratuitamente](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)
* O [biblioteca de análise de dados do Pandas](https://pandas.pydata.org/)
 
## <a name="setting-up"></a>Configurando

### <a name="create-an-anomaly-detector-resource"></a>Criar um recurso do Detector de anomalias

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="install-the-client-library"></a>Instalar a biblioteca de cliente

Depois de instalar o Python, você pode instalar a biblioteca de cliente com:

```console
pip install --upgrade azure-cognitiveservices-anomalydetector
```

## <a name="object-model"></a>Modelo de objeto

O cliente do Detector de anomalias é um [AnomalyDetectorClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python) objeto autentica no Azure usando sua chave. O cliente fornece dois métodos de detecção de anomalias: Em um conjunto de dados inteiro usando [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-)e o ponto em que os dados mais recentes usando [Last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-). 

Dados de série temporal são enviados como uma série de [pontos](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point(class)?view=azure-python) em um [solicitar](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request(class)?view=azure-python) objeto. O `Request` objeto contém propriedades para descrever os dados ([granularidade](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) por exemplo) e os parâmetros para a detecção de anomalias. 

A resposta do Detector de anomalias é um [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) ou [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) objeto dependendo do método usado. 

## <a name="getting-started"></a>Introdução

Crie um novo aplicativo Python em seu editor preferido ou IDE. Em seguida, adicione as seguintes declarações de importação ao seu arquivo. 

[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

> [!NOTE]
> Este início rápido pressupõe que você já [criou uma variável de ambiente](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) para a chave do Detector de anomalias, denominado `ANOMALY_DETECTOR_KEY`.

Crie variáveis para sua chave como uma variável de ambiente, o caminho para um arquivo de dados de série de tempo e o local do azure de sua assinatura. Por exemplo: `westus2`. 

[!code-python[Vars for the key, path location and data path](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=initVars)]

## <a name="code-examples"></a>Exemplos de código 

Esses trechos de código mostram como fazer o seguinte com a biblioteca de cliente do Detector de anomalias para .NET:

* [Autenticar o cliente](#authenticate-the-client)
* [Carregar um conjunto de dados de série temporal de um arquivo](#load-time-series-data-from-a-file)
* [Detectar anomalias em todo o conjunto de dados](#detect-anomalies-in-the-entire-data-set) 
* [Detectar o status de anomalias do último ponto de dados](#detect-the-anomaly-status-of-the-latest-data-point)

### <a name="authenticate-the-client"></a>Autenticar o cliente

Adicione sua variável de local do azure para o ponto de extremidade e autenticar o cliente com a sua chave.

[!code-python[Client authentication](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=client)]

### <a name="load-time-series-data-from-a-file"></a>Carregar dados de série temporal de um arquivo

Baixar os dados de exemplo para este início rápido da [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv):
1. Em seu navegador, clique com botão direito **Raw**
2. Clique em **link Salvar como**
3. Salve o arquivo ao seu diretório de aplicativo, como um arquivo. csv.

Esses dados de série de tempo são formatados como um arquivo. csv e serão enviados para a API do Detector de anomalias.

Carregar seu arquivo de dados com a biblioteca de Pandas `read_csv()` e fazer uma variável para armazenar sua série de dados de lista vazia. Iterar por meio do arquivo e acrescentar os dados como uma [ponto](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point%28class%29?view=azure-python) objeto. Este objeto contém o carimbo de hora e o valor numérico das linhas de seu arquivo de dados. csv. 

[!code-python[Load the data file](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=loadDataFile)]

Criar uma [solicitar](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request%28class%29?view=azure-python) objeto com sua série de tempo e o [granularidade](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) (ou periodicidade) dos seus pontos de dados. Por exemplo: `Granularity.daily`.

[!code-python[Create the request object](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=request)]

### <a name="detect-anomalies-in-the-entire-data-set"></a>Detectar anomalias em todo o conjunto de dados 

Chame a API para detectar anomalias por meio do banco de dados de série temporal de inteiro usando o cliente [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-) método. Store retornado [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) objeto. Iterar por meio da resposta `is_anomaly` listar e imprimir o índice de qualquer `true` valores. Esses valores correspondem ao índice de pontos de dados anormais, caso seja encontrado.

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=detectAnomaliesBatch)]

### <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Detectar o status de anomalias do último ponto de dados

Chamar a API do Detector de anomalias para determinar se o seu último ponto de dados é uma anomalia usando o cliente [last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-) método e repositório retornado [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) objeto. A resposta `is_anomaly` valor é um valor booliano que especifica o status de anomalias desse ponto.  

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=latestPointDetection)]

## <a name="run-the-application"></a>Executar o aplicativo

Executar o aplicativo em seu IDE ou na linha de comando com o `python` comando e o nome do arquivo.
 
## <a name="clean-up-resources"></a>Limpar recursos

Se você quiser limpar e remover uma assinatura dos serviços Cognitivos, você pode excluir o recurso ou grupo de recursos. Também é excluir o grupo de recursos exclui todos os outros recursos associados com o grupo de recursos.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI do Azure](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Você também pode executar o seguinte comando do shell de nuvem para remover o grupo de recursos e seus recursos associados. Isso pode levar alguns minutos para ser concluída. 

```azurecli-interactive
az group delete --name example-anomaly-detector-resource-group
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
>[Detecção de anomalias de streaming com o Azure Databricks](../tutorials/anomaly-detection-streaming-databricks.md)

* O que é o [API do Detector de anomalias?](../overview.md)
* [Práticas recomendadas](../concepts/anomaly-detection-best-practices.md) ao usar a API do Detector de anomalias.
* O código-fonte desta amostra pode ser encontrado no [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/csharp-sdk-sample.cs).

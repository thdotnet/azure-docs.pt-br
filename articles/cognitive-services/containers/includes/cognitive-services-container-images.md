---
ms.openlocfilehash: fe9879f9574fe1496ebdf20ac76fe5b759d4ea6a
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051212"
---
### <a name="container-repositories-and-images"></a>Repositórios e imagens de contêiner

As tabelas a seguir são uma lista abrangente das imagens de contêiner disponíveis oferecidas pelos serviços cognitivas do Azure.

#### <a name="public-container-registry-mcrmicrosoftcom"></a>Público (registro de contêiner `mcr.microsoft.com`:)

O registro de contêiner da Microsoft hospeda todos os contêineres de GA (disponibilidade geral) para serviços cognitivas.

| Serviço | Contêiner | Registro de contêiner/repositório/nome da imagem |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Análise de Texto](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Extração de Frases-chave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Análise de Texto](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Detecção de idioma | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Análise de Texto](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Análise de Sentimento | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-preview-container-registry-containerpreviewazurecrio"></a>Visualização pública (registro de contêiner `containerpreview.azurecr.io`:)

O registro de visualização de contêiner hospeda todos os contêineres de "visualização pública" para serviços cognitivas que ainda não foram progredidos para a disponibilidade geral (GA). Esses contêineres exigem uma solicitação formal para acesso a fim de consumi-los.

| Serviço | Contêiner | Registro de contêiner/repositório/nome da imagem |
|--|--|--|
| [Detector de anomalias](../../anomaly-detector/anomaly-detector-container-howto.md) | Detector de Anomalias | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Pesquisa Visual Computacional](../../Computer-vision/computer-vision-how-to-install-containers.md) | Reconhecimento de Texto | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text` |
| [Detecção Facial](../../face/face-how-to-install-containers.md) | Detecção Facial | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Reconhecedor de formulário](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Reconhecimento de Formulários | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [API do Serviço de Fala](../../speech-service/speech-container-howto.md) | Conversão de fala em texto | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [API do Serviço de Fala](../../speech-service/speech-container-howto.md) | Conversão de texto em fala | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |

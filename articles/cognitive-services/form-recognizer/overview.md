---
title: O que é o Reconhecimento de Formulários?
titleSuffix: Azure Cognitive Services
description: Saiba como usar o Reconhecimento de Formulários para analisar dados de formulário e de tabela.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 04/08/2019
ms.author: pafarley
ms.openlocfilehash: 8fb382227c71fce7ebe062057adf5edfb90a1a92
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65601630"
---
# <a name="what-is-form-recognizer"></a>O que é o Reconhecimento de Formulários?

O Reconhecimento de Formulários do Azure é um serviço cognitivo que usa a tecnologia de aprendizado de máquina para identificar e extrair pares chave-valor e dados de tabela de documentos de formulário. Em seguida, ele gera dados estruturados que incluem as relações no arquivo original. Chame seu modelo personalizado do Reconhecimento de Formulários usando uma API REST simples para reduzir a complexidade e integrá-lo com facilidade ao seu fluxo de trabalho ou aplicativo. Para começar, você precisa apenas de cinco documentos de formulário ou um formulário vazio do mesmo tipo como o material de entrada. É possível obter resultados com rapidez e precisão, adaptados ao seu conteúdo específico, sem intervenção manual intensa nem ampla experiência em ciência de dados.

## <a name="request-access"></a>Solicitar acesso
O Reconhecimento de Formulários está disponível em uma versão prévia de acesso limitado. Para obter acesso à versão prévia, preencha e envie o formulário de [Solicitação de acesso ao Reconhecimento de Formulários](https://aka.ms/FormRecognizerRequestAccess). O formulário solicita informações sobre você, sua empresa e o cenário do usuário para o qual usará o Reconhecimento de Formulários. Caso sua solicitação seja aprovada pela equipe dos Serviços Cognitivos do Azure, você receberá um email com instruções para acessar o serviço.

## <a name="what-it-does"></a>O que faz

Quando você envia seus dados de entrada, o algoritmo é treinado neles, agrupa os formulários por tipos, descobre quais chaves e tabelas estão presentes e aprende a associar valores a chaves e entradas a tabelas. O aprendizado sem supervisão permite que o modelo entenda o layout e as relações entre os campos e as entradas sem rotulagem manual de dados nem codificação e manutenção intensivas. Por outro lado, os modelos de machine learning pré-treinados exigem dados padronizados e são menos precisos quando usados com o material de entrada que se desvia de formatos tradicionais, como formulários específicos do setor.

Depois de treinar no modelo, você pode testá-lo, treinar novamente e, por fim, usá-lo para extrair, de forma confiável, dados de mais formulários de acordo com suas necessidades.

## <a name="what-it-includes"></a>O que inclui

O Reconhecimento de Formulários está disponível como uma API REST. Você pode criar, treinar e pontuar um modelo ao chamar a API. Se quiser, você pode executar o modelo em um contêiner de Docker local.

## <a name="input-requirements"></a>Requisitos de entrada

O Reconhecimento de Formulários funciona em documentos de entrada que atendem aos seguintes requisitos:

* O formato deve ser JPG, PNG ou PDF (texto ou digitalizado). PDFs com texto inserido são o melhor porque não há nenhuma possibilidade de erro na extração e na localização de caracteres.
* O tamanho do arquivo precisa ser inferior a 4 MB (megabytes).
* Para imagens, as dimensões devem estar entre 50 x 50 pixels e 4.200 x 4.200 pixels.
* Se documentos em papel forem digitalizados, os formulários deverão ser digitalizações de alta qualidade.
* O texto precisa usar o alfabeto latino (caracteres em português).
* Os dados devem ser impressos (e não manuscritos).
* Os dados precisam conter chaves e valores.
* As chaves podem aparecer acima ou à esquerda dos valores, mas não abaixo nem à direita.

Atualmente o Reconhecimento de Formulários não dá suporte aos seguintes tipos de dados de entrada:

* Tabelas complexas (tabelas aninhadas, células ou cabeçalhos mesclados e assim por diante).
* Caixas de seleção ou botões de opção.
* Documentos PDF com mais de 50 páginas.

## <a name="where-do-i-start"></a>Por onde começo?

**Etapa 1:** Criar um recurso do Reconhecimento de Formulários no portal do Azure.

**Etapa 2:** Experimentar um Início Rápido para obter experiência prática:
* [Início Rápido: treinar em um modelo do Reconhecimento de Formulários e extrair dados de formulário usando a API REST com o cURL](quickstarts/curl-train-extract.md)
* [Início Rápido: treinar em um modelo do Reconhecimento de Formulários e extrair dados de formulário usando a API REST com o Python](quickstarts/python-train-extract.md)

É recomendável que você use o serviço gratuito enquanto estiver aprendendo a tecnologia, mas lembre-se de que o número de páginas livres é limitado a 500 páginas por mês.

**Etapa 3:** revisar as Especificações das APIs REST

Use as APIs a seguir para treinar e extrair dados estruturados de formulários.

| API REST | DESCRIÇÃO |
|-----|-------------|
| Treinar | Treine um novo modelo para analisar seus formulários usando cinco formulários do mesmo tipo ou um formulário vazio.  |
| Analise  |Analise um único documento passado como um fluxo para extrair pares chave-valor e tabelas do formulário com o modelo personalizado.  |

Explore o [documento de referência da API REST](https://aka.ms/form-recognizer/api). 

## <a name="data-privacy-and-security"></a>Segurança e privacidade de dados

O serviço é oferecido como uma [versão prévia](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) de um serviço do Azure de acordo com os [Termos de Serviço Online](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Assim como ocorre com todos os serviços cognitivos, os desenvolvedores que usam o serviço Reconhecimento de Formulários devem estar cientes das políticas da Microsoft referentes aos dados do cliente. Confira a [página de Serviços Cognitivos](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) na Central de Confiabilidade da Microsoft para saber mais.

## <a name="next-steps"></a>Próximas etapas

Complete um [início rápido](quickstarts/curl-train-extract.md) para começar a usar as [APIs do Reconhecimento de Formulários](https://aka.ms/form-recognizer/api).

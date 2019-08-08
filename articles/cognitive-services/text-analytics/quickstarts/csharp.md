---
title: 'Início Rápido: Chamar o serviço de Análise de Texto usando o SDK do Azure para .NET e C#'
titleSuffix: Azure Cognitive Services
description: Informações e exemplos de códigos para ajudá-lo a começar a usar o serviço de Análise de Texto e C#.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/30/2019
ms.author: assafi
ms.openlocfilehash: d12f6b400b270c6ef631d9f503980efef1ae8458
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840386"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Início Rápido: Usar o SDK do .NET e C# para chamar o serviço de Análise de Texto
<a name="HOLTop"></a>

Este início rápido ajuda a começar a usar o SDK do Azure para .NET e C# para analisar a linguagem. Embora a API REST da [Análise de Texto](//go.microsoft.com/fwlink/?LinkID=759711) seja compatível com a maioria das linguagens de programação, o SDK fornece uma maneira fácil de integrar o serviço aos aplicativos.

> [!NOTE]
> O código de demonstração deste artigo usa os métodos síncronos do SDK do .NET de Análise de Texto para manter a simplicidade. No entanto, para cenários de produção, recomendamos usar os métodos assíncronos em lote em seus próprios aplicativos para mantê-los escalonáveis e responsivos. Por exemplo, você pode usar `SentimentBatchAsync` em vez de `Sentiment`.
>
> Uma versão assíncrona em lote deste exemplo pode ser encontrada no [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics).

Para obter detalhes técnicos, confira a [referência da Análise de Texto](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet) do SDK para .NET.

## <a name="prerequisites"></a>Pré-requisitos

* Qualquer edição do Visual Studio 2017 ou posterior
* O [SDK de Análise de Texto para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

## <a name="create-the-visual-studio-solution-and-install-the-sdk"></a>Criar a solução do Visual Studio e instalar o SDK

1. Crie um novo projeto de aplicativo de console (.NET Core). [Acesse o Visual Studio](https://visualstudio.microsoft.com/vs/).
1. Clique com o botão direito do mouse na solução e selecione **Gerenciar Pacotes NuGet para a solução**.
1. Selecione a guia **Procurar**. Pesquise **Microsoft.Azure.CognitiveServices.Language.TextAnalytics**.

## <a name="authenticate-your-credentials"></a>Autenticar suas credenciais

1. Adicione a instrução `using` a seguir ao arquivo de classe principal (que é o Program.cs por padrão).

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net.Http;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Rest;
    ```

2. Crie uma nova classe `ApiKeyServiceClientCredentials` para armazenar as credenciais e adicione-as a cada solicitação.

    ```csharp
    class ApiKeyServiceClientCredentials : ServiceClientCredentials
    {
        private readonly string apiKey;

        public ApiKeyServiceClientCredentials(string apiKey)
        {
            this.apiKey = apiKey;
        }

        public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            if (request == null)
            {
                throw new ArgumentNullException("request");
            }
            request.Headers.Add("Ocp-Apim-Subscription-Key", this.apiKey);
            return base.ProcessHttpRequestAsync(request, cancellationToken);
        }
    }
    ```

3. Atualize a classe `Program`. Adicione um membro constante à sua chave de API de Análise de Texto e outro ao ponto de extremidade de serviço. Lembre-se de usar o local do Azure correto para o seu recurso de Análise de Texto.

    ```csharp
    // Enter your Text Analytics (TA) API Key (available in Azure Portal -> your TA resource -> Keys)
    private const string ApiKey = "enter-your-textanalytics-api-key-here";
    // You can get the resource location from Azure Portal -> your TA resource -> Overview
    // There are two acceptable formats for the endpoint, both
    // require that you omit the `/text/analytics/<version>` suffix:
    // 1. A location based endpoint URL -
    //     "https://<your-location>.api.cognitive.microsoft.com";
    // 2. A resource name based endpoint URL -
    //     "https://<your-resource-name>.cognitiveservices.azure.com";
    private const string Endpoint = "enter-your-base-resource-endpoint-here";
    ```

> [!Tip]
> Para melhorar a segurança de segredos em sistemas de produção, recomendamos usar o [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Criar um cliente de Análise de Texto

Na função `Main` de seu projeto, chame o método de exemplo que você deseja invocar. Passe os parâmetros `Endpoint` e `ApiKey` que você definiu.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(ApiKey);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = Endpoint
        };

        // Change the console encoding to display non-ASCII characters.
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        SentimentAnalysisExample(client);
        // DetectLanguageExample(client);
        // RecognizeEntitiesExample(client);
        // KeyPhraseExtractionExample(client);
        Console.ReadLine();
    }
```

As seções a seguir descrevem como chamar cada recurso de serviço.

## <a name="perform-sentiment-analysis"></a>Executar análise de sentimento

1. Crie uma nova função `SentimentAnalysisExample()` que usa o cliente criado anteriormente.
2. Na mesma função, chame `client.Sentiment()` e obtenha o resultado. O resultado conterá o sentimento `Score` se for bem-sucedido e um `errorMessage` se não. Uma pontuação mais próxima de 0 indica um sentimento negativo, enquanto uma pontuação mais próxima de 1 indica um sentimento positivo.

    ```csharp
    var result = client.Sentiment("I had the best day of my life.", "en");

    // Printing sentiment results
    Console.WriteLine($"Sentiment Score: {result.Score:0.00}");
    ```

### <a name="output"></a>Saída

```console
Sentiment Score: 0.87
```

## <a name="perform-language-detection"></a>Executar a detecção de idioma

1. Crie uma nova função `DetectLanguageExample()` que usa o cliente criado anteriormente.
2. Na mesma função, chame `client.DetectLanguage()` e obtenha o resultado. O resultado conterá a lista de idiomas detectados em `DetectedLanguages` se for bem sucedido e um `errorMessage` se não. Em seguida, imprima o primeiro idioma retornado.

    ```csharp
    var result = client.DetectLanguage("This is a document written in English.");

    // Printing detected languages
    Console.WriteLine($"Language: {result.DetectedLanguages[0].Name}");
    ```

> [!Tip]
> Em alguns casos, pode ser difícil eliminar a ambiguidade de idiomas com base na entrada. Você pode usar o parâmetro `countryHint` para especificar um código de país de duas letras. Por padrão, a API está usando o "US" como o countryHint padrão, para remover esse comportamento, você pode redefinir esse parâmetro configurando esse valor como uma cadeia de caracteres vazia `countryHint = ""`.

### <a name="output"></a>Saída

```console
Language: English
```

## <a name="perform-entity-recognition"></a>Executar reconhecimento de entidade

1. Crie uma nova função `RecognizeEntitiesExample()` que usa o cliente criado anteriormente.
2. Na mesma função, chame `client.Entities()` e obtenha o resultado. Em seguida, itere pelos resultados. O resultado conterá a lista de entidades detectadas em `Entities` se for bem sucedido e um `errorMessage` se não. Para cada entidade detectada, imprima seu tipo, subtipo, nome da Wikipédia (se houver), bem como os locais do texto original.

    ```csharp
    var result = client.Entities("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.");

    // Printing recognized entities
    Console.WriteLine("Entities:");
    foreach (var entity in result.Entities)
    {
        Console.WriteLine($"\tName: {entity.Name},\tType: {entity.Type ?? "N/A"},\tSub-Type: {entity.SubType ?? "N/A"}");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.EntityTypeScore:F3}");
        }
    }
    ```

### <a name="output"></a>Saída

```console
Entities:
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0,      Length: 9,      Score: 1.000
    Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25,     Length: 10,     Score: 1.000
    Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40,     Length: 10,     Score: 0.999
    Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54,     Length: 7,      Score: 0.800
    Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54,     Length: 13,     Score: 0.800
    Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89,     Length: 5,      Score: 0.800
    Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116,    Length: 11,     Score: 0.800
```

## <a name="perform-key-phrase-extraction"></a>Executar uma extração de frases-chave

1. Crie uma nova função `KeyPhraseExtractionExample()` que usa o cliente criado anteriormente.
2. Na mesma função, chame `client.KeyPhrases()` e obtenha o resultado. O resultado conterá a lista de frases-chave detectadas em `KeyPhrases` se for bem sucedido e um `errorMessage` se não. Em seguida, imprima todas as frases-chave detectadas.

    ```csharp
    var result = client.KeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in result.KeyPhrases)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
    ```

### <a name="output"></a>Saída

```console
Key phrases:
    cat
    veterinarian
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Análise de Texto com o Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Visão geral da Análise de Texto](../overview.md)
* [Perguntas frequentes (FAQ)](../text-analytics-resource-faq.md)

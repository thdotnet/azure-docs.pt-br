---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 2f0cda05933aa64b415249b0596bfdfa24ef59f7
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968012"
---
## <a name="prerequisites"></a>Pré-requisitos

Este início rápido requer:

* [Go](https://golang.org/doc/install)
* Uma chave de assinatura do Azure para a Tradução de Texto

## <a name="create-a-project-and-import-required-modules"></a>Criar um projeto e importar os módulos necessários

Crie um novo projeto Go usando seu IDE ou editor favorito. Em seguida, copie esse trecho de código para seu projeto em um arquivo denominado `sentence-length.go`.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>Criar a função principal

Este exemplo tentará ler a chave da sua assinatura de Tradução de Texto da variável de ambiente `TRANSLATOR_TEXT_KEY`. Se você não estiver familiarizado com as variáveis de ambiente,poderá definir `subscriptionKey` como uma cadeia de caracteres e comentar a instrução condicional.

Copie este código em seu projeto:

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables. If so, be sure to delete the "os" import.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our breakSentence function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    breakSentence(subscriptionKey)
}
```

## <a name="create-a-function-to-determine-sentence-length"></a>Criar uma função para determinar o comprimento da frase

Vamos criar uma função para determinar o comprimento da frase. Esta função usará um único argumento, sua chave de assinatura de Tradução de Texto.

```go
func breakSentence(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Em seguida, vamos construir a URL. A URL é criada usando os métodos `Parse()` e `Query()`. Você observará que os parâmetros são adicionados com o método `Add()`.

Copie este código para a função `breakSentence`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0")
q := u.Query()
q.Add("languages", "en")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Para saber mais sobre pontos de extremidade, rotas e parâmetros de solicitação, confira [API de Tradução de Texto 3.0: BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence).

## <a name="create-a-struct-for-your-request-body"></a>Criar um struct para o corpo da solicitação

Em seguida, crie uma estrutura anônima para o corpo da solicitação e codifique-o como JSON com `json.Marshal()`. Adicione este código à função `breakSentence`.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "How are you? I am fine. What did you do today?"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>Criar a solicitação

Agora que você codificou o corpo da solicitação como JSON, pode criar sua solicitação POST e chamar a API de Tradução de Texto.

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers to the request
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

Se estiver usando uma assinatura de vários serviço cognitivos, você também deve incluir o `Ocp-Apim-Subscription-Region` em seus parâmetros de solicitação. [Saiba mais sobre a autenticação com a assinatura de vários serviços](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="handle-and-print-the-response"></a>Processar e imprimir a resposta

Adicione este código à função `breakSentence` para decodificar a resposta JSON e, em seguida, formate e imprima o resultado.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Colocar tudo isso junto

É isso; você montou um programa simples que chamará a API de Tradução de Texto e retornará uma resposta JSON. Agora é hora de executar o programa:

```console
go run sentence-length.go
```

Se você quiser comparar seu código com o nosso, o exemplo completo está disponível no [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Resposta de exemplo

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Próximas etapas

Confira a referência da API para saber tudo o que você pode fazer com a API de Tradução de Texto.

> [!div class="nextstepaction"]
> [Referência de API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

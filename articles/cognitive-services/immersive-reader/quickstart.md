---
title: 'Início Rápido: Criar um aplicativo Web que inicia a Leitura Avançada (C#)'
titlesuffix: Azure Cognitive Services
description: Neste início rápido, você cria um aplicativo Web do zero e adiciona a funcionalidade de Leitura Avançada.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 77d95383c801038c256ccb2bf386ddf06048cf78
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311805"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-c"></a>Início Rápido: Criar um aplicativo Web que inicia a Leitura Avançada (C#)

A [Leitura Avançada](https://www.onenote.com/learningtools) é uma ferramenta projetada de forma inclusiva que implementa técnicas comprovadas para melhorar a compreensão da leitura.

Neste Início Rápido, você cria um aplicativo Web do zero e integra a Leitura Avançada usando o SDK de Leitura Avançada. Um exemplo de funcionamento completo deste Início Rápido está disponível [aqui](https://github.com/Microsoft/immersive-reader-sdk/samples/quickstart-csharp).

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads)
* Uma chave de assinatura para a Leitura Avançada. Obtenha uma seguindo [estas instruções](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).

## <a name="create-a-web-app-project"></a>Criar um projeto do aplicativo Web

Crie um novo projeto no Visual Studio, usando o modelo de aplicativo Web ASP.NET Core com Model-View-Controller interno.

![Novo Projeto](./media/vswebapp.png)

![Novo aplicativo Web ASP.NET Core](./media/vsmvc.png)

## <a name="acquire-an-access-token"></a>Adquirir um token de acesso

Você precisa de sua chave de assinatura e do ponto de extremidade para a próxima etapa. Encontre sua chave de assinatura na página Chaves do recurso Leitura Avançada no portal do Azure. Encontre o ponto de extremidade na página Visão geral.

Clique com botão direito do mouse no projeto no _Gerenciador de Soluções_ e escolha **Gerenciar Segredos do Usuário**. Isso abrirá um arquivo chamado _secrets.json_. Substitua o conteúdo desse arquivo pelo seguinte, fornecendo sua chave de assinatura e ponto de extremidade onde for apropriado.

```json
{
  "SubscriptionKey": YOUR_SUBSCRIPTION_KEY,
  "Endpoint": YOUR_ENDPOINT
}
```

Abra _Controllers\HomeController.cs_ e substitua a classe `HomeController` pelo código a seguir.

```csharp
public class HomeController : Controller
{
    private readonly string SubscriptionKey;
    private readonly string Endpoint;

    public HomeController(Microsoft.Extensions.Configuration.IConfiguration configuration)
    {
        SubscriptionKey = configuration["SubscriptionKey"];
        Endpoint = configuration["Endpoint"];

        if (string.IsNullOrEmpty(Endpoint) || string.IsNullOrEmpty(SubscriptionKey))
        {
            throw new ArgumentNullException("Endpoint or subscriptionKey is null!");
        }
    }

    public IActionResult Index()
    {
        return View();
    }

    [Route("token")]
    public async Task<string> Token()
    {
        return await GetTokenAsync();
    }

    /// <summary>
    /// Exchange your Azure subscription key for an access token
    /// </summary>
    private async Task<string> GetTokenAsync()
    {
        using (var client = new System.Net.Http.HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", SubscriptionKey);
            using (var response = await client.PostAsync(Endpoint, null))
            {
                return await response.Content.ReadAsStringAsync();
            }
        }
    }
}
```

## <a name="add-sample-content"></a>Adicionar o conteúdo de exemplo

Agora, vamos adicionar um conteúdo de exemplo a este aplicativo Web. Abra _Views\Home\Index.cshtml_ e substitua o código gerado automaticamente por este exemplo:

```html
<h1 id='title'>Geography</h1>
<span id='content'>
    <p>The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers. Landforms are sometimes called physical features. It is important for students to know about the physical geography of Earth. The seasons, the atmosphere and all the natural processes of Earth affect where people are able to live. Geography is one of a combination of factors that people use to decide where they want to live.</p>
</span>

<div class='immersive-reader-button' data-button-style='iconAndText' onclick='launchImmersiveReader()'></div>

@section scripts {
<script type='text/javascript' src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js'></script>
<script type='text/javascript' src='https://code.jquery.com/jquery-3.3.1.min.js'></script>
<script type='text/javascript'>
    function getImmersiveReaderTokenAsync() {
        return new Promise((resolve) => {
            $.ajax({
                url: '/token',
                type: 'GET',
                success: token => {
                    resolve(token);
                }
            });
        });
    }

    async function launchImmersiveReader() {
        const content = {
            title: document.getElementById('title').innerText,
            chunks: [ {
                content: document.getElementById('content').innerText,
                lang: 'en'
            } ]
        };

        const token = await getImmersiveReaderTokenAsync();
        ImmersiveReader.launchAsync(token, content, { uiZIndex: 1000000 });
    }
</script>
}
```

## <a name="build-and-run-the-app"></a>Compilar e executar o aplicativo

Na barra de menus, selecione **Depurar > Iniciar Depuração** ou pressione **F5** para iniciar o aplicativo.

Em seu navegador, você deverá ver:

![Aplicativo de exemplo](./media/quickstart-result.png)

Ao clicar no botão "Leitura Avançada", você verá a Leitura Avançada iniciada com o conteúdo na página.

![Leitura Avançada](./media/quickstart-immersive-reader.png)

## <a name="next-steps"></a>Próximas etapas

* Confira o [tutorial](./tutorial.md) para ver o que mais você pode fazer com o SDK da Leitura Avançada
* Explore o [SDK da Leitura Avançada](https://github.com/Microsoft/immersive-reader-sdk) e a [Referência de SDK da Leitura Avançada](./reference.md)
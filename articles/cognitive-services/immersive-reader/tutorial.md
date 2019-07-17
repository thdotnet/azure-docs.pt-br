---
title: 'Tutorial: Iniciar a Leitura Avançada usando Node.js'
titleSuffix: Azure Cognitive Services
description: Neste tutorial, você criará um aplicativo Node.js que inicia a Leitura Avançada.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: f8697042ed46e0ff333f736454346908d76cf039
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718383"
---
# <a name="tutorial-launch-the-immersive-reader-nodejs"></a>Tutorial: Iniciar a Leitura Avançada (Node.js)

Na [visão geral](./overview.md), você aprendeu sobre o que é a Leitura Avançada e como ela implementa técnicas comprovadas para melhorar a compreensão de leitura para aprendizes do idioma, leitores emergentes e estudantes com diferenças de aprendizado. Este tutorial aborda como criar um aplicativo Web Node.js que inicia a Leitura Avançada. Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar um aplicativo Web Node.js com o Express
> * Adquirir um token de acesso
> * Iniciar a Leitura Avançada com o conteúdo de exemplo
> * Especificar o idioma do conteúdo
> * Especifique o idioma da interface de Leitura Avançada
> * Iniciar a Leitura Avançada com o conteúdo de matemática

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de assinatura para a Leitura Avançada. Obtenha uma seguindo [estas instruções](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
* [Node.js](https://nodejs.org/) e [Yarn](https://yarnpkg.com)
* Um IDE como o [Visual Studio Code](https://code.visualstudio.com/)

## <a name="create-a-nodejs-web-app-with-express"></a>Criar um aplicativo Web Node.js com o Express

Crie um aplicativo Web Node.js com a ferramenta `express-generator`.

```bash
npm install express-generator -g
express --view=pug myapp
cd myapp
```

Instale as dependências do yarn e adicione as dependências `request` e `dotenv`, o que será usado posteriormente no tutorial.

```bash
yarn
yarn add request
yarn add dotenv
```

## <a name="acquire-an-access-token"></a>Adquirir um token de acesso

Em seguida, escreva uma API de back-end para recuperar um token de acesso usando a chave de assinatura. Você precisa de sua chave de assinatura e do ponto de extremidade para a próxima etapa. Encontre sua chave de assinatura na página Chaves do recurso Leitura Avançada no portal do Azure. Encontre o ponto de extremidade na página Visão geral.

Quando tiver a chave de assinatura e o ponto de extremidade, crie um novo arquivo chamado _.env_ e cole o código a seguir, substituindo `{YOUR_SUBSCRIPTION_KEY}` e `{YOUR_ENDPOINT}` por sua chave de assinatura e pelo ponto de extremidade, respectivamente.

```text
SUBSCRIPTION_KEY={YOUR_SUBSCRIPTION_KEY}
ENDPOINT={YOUR_ENDPOINT}
```

Certifique-se de não confirmar esse arquivo no controle do código-fonte, uma vez que ele contém segredos que não devem ser tornados públicos.

Em seguida, abra _app.js_ e adicione o seguinte à parte superior do arquivo. Isso carregará a chave de assinatura e o ponto de extremidade como variáveis de ambiente no Nó.

```javascript
require('dotenv').config();
```

Abra o arquivo _routes\index.js_ e o seguinte será importado na parte superior do arquivo:

```javascript
var request = require('request');
```

Em seguida, adicione o código a seguir diretamente abaixo dessa linha. Este código cria um ponto de extremidade de API que adquire um token de acesso usando a chave de assinatura e retorna esse token.

```javascript
router.get('/token', function(req, res, next) {
  request.post({
    headers: {
        'Ocp-Apim-Subscription-Key': process.env.SUBSCRIPTION_KEY,
        'content-type': 'application/x-www-form-urlencoded'
    },
    url: process.env.ENDPOINT
  },
  function(err, resp, token) {
    return res.send(token);
  });
});
```

Esse ponto de extremidade de API deve ser protegido por alguma forma de autenticação (por exemplo, [OAuth](https://oauth.net/2/)); esse trabalho está além do escopo deste tutorial.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Iniciar a Leitura Avançada com o conteúdo de exemplo

1. Abra _views\layout.pug_ e adicione o seguinte código sob a marca `head`, antes da marca `body`. Essas marcas `script` carregam o [SDK de Leitura Avançada](https://github.com/Microsoft/immersive-reader-sdk) e jQuery.

    ```pug
    script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js')
    script(src='https://code.jquery.com/jquery-3.3.1.min.js')
    ```

2. Abra _views\index.pug_ e substitua o conteúdo pelo código a seguir. Esse código preenche a página com conteúdo de exemplo e adiciona um botão que inicia a Leitura Avançada.

    ```pug
    extends layout

    block content
      h2(id='title') Geography
      p(id='content') The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers.
      div(class='immersive-reader-button' data-button-style='iconAndText' data-locale='en-US' onclick='launchImmersiveReader()')
      script.
        function launchImmersiveReader() {
          // First, get a token using our /token endpoint
          $.ajax('/token', { success: token => {
            // Second, grab the content from the page
            const content = {
              title: document.getElementById('title').innerText,
              chunks: [ {
                content: document.getElementById('content').innerText + '\n\n',
                lang: 'en'
              } ]
            };

            // Third, launch the Immersive Reader
            ImmersiveReader.launchAsync(token, content);
          }});
        }
    ```

3. Nosso aplicativo Web está pronto. Inicie o aplicativo executando:

    ```bash
    npm start
    ```

4. Abra o navegador da Web e navegue até _http://localhost:3000_ . Veja o conteúdo acima na página. Clique no botão **Leitura Avançada** para iniciar a Leitura Avançada com seu conteúdo.

## <a name="specify-the-language-of-your-content"></a>Especificar o idioma do conteúdo

A Leitura Avançada oferece suporte para vários idiomas diferentes. Você pode especificar o idioma do seu conteúdo, seguindo as etapas abaixo.

1. Abra _views\index.pug_ e adicione o seguinte código abaixo da marca `p(id=content)` que você adicionou na etapa anterior. Esse código adiciona algum conteúdo em espanhol à sua página.

    ```pug
    p(id='content-spanish') El estudio de las formas terrestres de la Tierra se llama geografía física. Los accidentes geográficos pueden ser montañas y valles. También pueden ser glaciares, lagos o ríos.
    ```

2. No código JavaScript, adicione o seguinte acima da chamada para `ImmersiveReader.launchAsync`. Esse código transmite o conteúdo em espanhol para a Leitura Avançada.

    ```pug
    content.chunks.push({
      content: document.getElementById('content-spanish').innerText + '\n\n',
      lang: 'es'
    });
    ```

3. Navegue até _http://localhost:3000_ novamente. Você verá o texto em espanhol na página e, ao clicar na **Leitura Avançada**, ele também será exibido na Leitura Avançada.

## <a name="specify-the-language-of-the-immersive-reader-interface"></a>Especifique o idioma da interface de Leitura Avançada

Por padrão, o idioma da interface da Leitura Avançada corresponde à configurações de idioma do navegador. Você também pode especificar o idioma da interface da Leitura Avançada com o código a seguir.

1. Em _views\index.pug_, substitua a chamada para `ImmersiveReader.launchAsync(token, content)` pelo código abaixo.

    ```javascript
    const options = {
        uiLang: 'fr',
    }
    ImmersiveReader.launchAsync(token, content, options);
    ```

2. Navegue até _http://localhost:3000_ . Quando você inicia o leitor imersivo, a interface é mostrada em francês.

## <a name="launch-the-immersive-reader-with-math-content"></a>Iniciar a Leitura Avançada com o conteúdo de matemática

Você pode incluir o conteúdo de matemática na Leitura Avançada usando o [MathML](https://developer.mozilla.org/en-US/docs/Web/MathML).

1. Modifique _views\index.pug_ para incluir o seguinte código acima da chamada para `ImmersiveReader.launchAsync`:

    ```javascript
    const mathML = '<math xmlns="https://www.w3.org/1998/Math/MathML" display="block"> \
      <munderover> \
        <mo>∫</mo> \
        <mn>0</mn> \
        <mn>1</mn> \
      </munderover> \
      <mrow> \
        <msup> \
          <mi>x</mi> \
          <mn>2</mn> \
        </msup> \
        <mo>ⅆ</mo> \
        <mi>x</mi> \
      </mrow> \
    </math>';

    content.chunks.push({
      content: mathML,
      mimeType: 'application/mathml+xml'
    });
    ```

2. Navegue até _http://localhost:3000_ . Ao iniciar a Leitura Avançada e rolar para baixo, você verá a fórmula matemática.

## <a name="next-steps"></a>Próximas etapas

* Explore o [SDK da Leitura Avançada](https://github.com/Microsoft/immersive-reader-sdk) e a [Referência de SDK da Leitura Avançada](./reference.md)
* Visualize exemplos de código no [GitHub](https://github.com/microsoft/immersive-reader-sdk/samples/advanced-csharp)
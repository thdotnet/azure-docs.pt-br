---
title: Referência de SDK do leitor de imersão
titlesuffix: Azure Cognitive Services
description: Referência para o leitor de imersão SDK
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: c128608b3c4a8e1155c3ac962bcfd07f589fbf23
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311792"
---
# <a name="immersive-reader-sdk-reference"></a>Referência de SDK do leitor de imersão

O SDK do leitor de imersão é uma biblioteca JavaScript que permite que você integre o leitor de imersão em seu aplicativo web.

## <a name="functions"></a>Funções

O SDK do expõe uma única função, `ImmersiveReader.launchAsync(token, resourceName, content, options)`.

### <a name="launchasync"></a>launchAsync

Inicia o leitor de imersão dentro de um `iframe` em seu aplicativo web.

```typescript
launchAsync(token: string, resourceName: string, content: Content, options?: Options): Promise<HTMLDivElement>;
```

#### <a name="parameters"></a>parâmetros

| NOME | Type | DESCRIÇÃO |
| ---- | ---- |------------ |
| `token` | string | O token de acesso adquirido da chamada para o `issueToken` ponto de extremidade. |
| `resourceName` | string | Reservado. Deve ser definido como `null`. |
| `content` | [Conteúdo](#content) | Um objeto que contém o conteúdo a ser mostrado no leitor de imersão. |
| `options` | [Opções](#options) | Opções de configuração de determinados comportamentos do leitor de imersão. Opcional. |

#### <a name="returns"></a>Retornos

Retorna um `Promise<HTMLDivElement>` que resolve quando o leitor de imersão é carregado. O `Promise` resolve para um `div` elemento cujo único filho é um `iframe` elemento que contém a página de imersão leitor.

#### <a name="exceptions"></a>Exceções

Retornado `Promise` serão rejeitadas com uma [ `Error` ](#error) se o leitor de imersão Falha ao carregar do objeto. Para obter mais informações, consulte o [códigos de erro](#error-codes).

## <a name="types"></a>Tipos

### <a name="content"></a>Conteúdo

Contém o conteúdo a ser mostrado no leitor de imersão.

```typescript
{
    title?: string;      // Title text shown at the top of the Immersive Reader (optional)
    chunks: [ {          // Array of chunks
        content: string; // Plain text string
        lang?: string;   // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
        mimeType?: string; // MIME type of the content (optional). Defaults to 'text/plain' if not specified.
    } ];
}
```

#### <a name="supported-mime-types"></a>Tipos MIME com suporte

| Tipo de MIME | DESCRIÇÃO |
| --------- | ----------- |
| texto/sem formatação | Texto sem formatação. |
| aplicativo/mathml + xml | Linguagem de marcação de matemática (MathML). [Saiba mais](https://developer.mozilla.org/en-US/docs/Web/MathML).

### <a name="options"></a>Opções

Contém propriedades que configurar determinados comportamentos do leitor de imersão.

```typescript
{
    uiLang?: string;   // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;  // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number; // Z-index of the iframe that will be created (default is 1000)
    useWebview?: boolean; // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
}
```

### <a name="error"></a>Erro

Contém informações sobre o erro.

```typescript
{
    code: string;    // One of a set of error codes
    message: string; // Human-readable representation of the error
}
```

#### <a name="error-codes"></a>Códigos do Erro

| Código | DESCRIÇÃO |
| ---- | ----------- |
| BadArgument | O argumento fornecido é inválido, consulte `message` para obter detalhes. |
| Tempo limite | O leitor de imersão Falha ao carregar dentro do tempo limite especificado. |
| TokenExpired| O token fornecido expirou. |

## <a name="launching-the-immersive-reader"></a>Iniciando o leitor de imersão

O SDK fornece o estilo padrão para o botão para iniciar o leitor envolventes. Use o `immersive-reader-button` atributo de classe para habilitar esse estilo.

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>Atributos opcionais

Use os seguintes atributos para configurar a aparência do botão.

| Atributo | DESCRIÇÃO |
| --------- | ----------- |
| `data-button-style` | Define o estilo do botão. Pode ser `icon`, `text` ou `iconAndText`. Assume o padrão de `icon`. |
| `data-locale` | Define a localidade, por exemplo, `en-US`, `fr-FR`. O padrão é inglês. |
| `data-icon-px-size` | Define o tamanho do ícone em pixels. O padrão é 20px. |

## <a name="browser-support"></a>Suporte ao navegador

Use as versões mais recentes dos navegadores a seguir para a melhor experiência com o leitor envolventes.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Próximas etapas

* Explore o [leitor imersivo SDK no GitHub](https://github.com/Microsoft/immersive-reader-sdk)
* [Início Rápido: Criar um aplicativo web que inicia o leitor de imersão (C#)](./quickstart.md)
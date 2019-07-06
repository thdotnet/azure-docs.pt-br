---
title: Speech Synthesis Markup Language (SSML) - serviços de fala
titleSuffix: Azure Cognitive Services
description: Usando o Speech Synthesis Markup Language para controlar pronúncia e prosódia em texto para fala.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 8285a76f8cd07863874f9c8e8eebe96f1cb968dd
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604815"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Linguagem de marcação de síntese de fala (SSML)

Speech Synthesis Markup Language (SSML) é uma linguagem de marcação baseada em XML que permite aos desenvolvedores especificar o texto como entrado é convertida em voz sintetizada usando o serviço de texto em fala. SSML em comparação com texto sem formatação, permite aos desenvolvedores ajustar o tom, a pronúncia, a velocidade da fala, volume e muito mais do que a saída de texto em fala. Pontuação normal, como pausa após um período, ou usando a entonação correta quando uma frase termina com um ponto de interrogação são manipulados automaticamente.

A implementação de serviços de fala de SSML baseia-se na World Wide Web Consortium [fala síntese Markup Language versão 1.0](https://www.w3.org/TR/speech-synthesis).

> [!IMPORTANT]
> Contagem de caracteres do chinês, japonês e coreano como dois caracteres para cobrança. Para saber mais, consulte [Preços](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="standard-neural-and-custom-voices"></a>Vozes neurais, padrão e personalizadas

Escolher entre standard e neurais vozes ou criar sua própria voz personalizada exclusivos ao seu produto ou marca. 75 vozes padrão estão disponíveis em mais de 45 idiomas e localidades e vozes neurais 5 estão disponíveis em 4 de idiomas e localidades. Para obter uma lista completa de vozes (neurais e padrão), localidades e idiomas com suporte, consulte [suporte para idioma](language-support.md).

Para saber mais sobre o padrão, neural e vozes personalizadas, consulte [visão geral de texto em fala](text-to-speech.md).

## <a name="supported-ssml-elements"></a>Elementos SSML com suporte

Cada documento SSML é criado com elementos SSML (ou marcas). Esses elementos são usados para ajustar o tom, métrica, volume e muito mais. As seções a seguir detalham como cada elemento é usado e quando um elemento é obrigatório ou opcional.  

> [!IMPORTANT]
> Não se esqueça de usar aspas duplas em torno de valores de atributo. Padrões para XML bem formado e válido exige valores de atributo a ser colocado entre aspas duplas. Por exemplo, `<prosody volume="90">` é um elemento bem formado e válido, mas `<prosody volume=90>` não é. SSML pode não reconhecer os valores de atributo que não estão entre aspas.

## <a name="create-an-ssml-document"></a>Criar um documento SSML

`speak` é o elemento raiz, estando **necessária** para todos os documentos SSML. O `speak` elemento contém informações importantes, como versão, idioma e a definição de vocabulário de marcação.

**Sintaxe**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Atributos**

| Atributo | DESCRIÇÃO | Obrigatório/Opcional |
|-----------|-------------|---------------------|
| version | Indica a versão da especificação SSML usada para interpretar a marcação do documento. A versão atual é 1.0. | Obrigatório |
| xml:lang | Especifica o idioma do documento raiz. O valor pode conter um código de idioma de letras minúsculas, de duas letras (por exemplo, **en**), ou o código de idioma e país/região em maiusculas (por exemplo, **en-US**). | Obrigatório |
| xmlns | Especifica o URI para o documento que define o vocabulário de marcação (tipos de elemento e nomes de atributo) do documento SSML. O URI atual é https://www.w3.org/2001/10/synthesis. | Obrigatório |

## <a name="choose-a-voice-for-text-to-speech"></a>Escolha uma voz para texto em fala

O `voice` elemento é necessário. Ele é usado para especificar a voz que é usada para texto em fala.

**Sintaxe**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Atributos**

| Atributo | DESCRIÇÃO | Obrigatório/Opcional |
|-----------|-------------|---------------------|
| name | Identifica a voz usada para a saída de texto em fala. Para obter uma lista completa de vozes com suporte, consulte [suporte ao idioma](language-support.md#text-to-speech). | Obrigatório |

**Exemplo**

> [!NOTE]
> Este exemplo usa o `en-US-Jessa24kRUS` voz. Para obter uma lista completa de vozes com suporte, consulte [suporte ao idioma](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Usar várias vozes

Dentro de `speak` elemento, você pode especificar várias vozes para saída de texto em fala. Esses vozes podem estar em diferentes idiomas. Para cada voz, o texto deve ser encapsulado em um `voice` elemento.

**Atributos**

| Atributo | DESCRIÇÃO | Obrigatório/Opcional |
|-----------|-------------|---------------------|
| name | Identifica a voz usada para a saída de texto em fala. Para obter uma lista completa de vozes com suporte, consulte [suporte ao idioma](language-support.md#text-to-speech). | Obrigatório |

**Exemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Good morning!
    </voice>
    <voice  name="en-US-Guy24kRUS">
        Good morning to you too Jessa!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Ajustar os estilos de fala

> [!IMPORTANT]
> Esse recurso só funcionará com as vozes neurais.

Por padrão, o serviço de texto em fala sintetiza texto usando um estilo de fala com neutralidade de vozes padrão e neurais. Com vozes neurais, você pode ajustar o estilo de fala para expressar cheerfulness, empatia ou sentimento com o `<mstts:express-as>` elemento. Isso é um elemento opcional exclusivo aos serviços de fala do Azure.

Atualmente, os ajustes de estilo de fala têm suporte para esses vozes neurais:
* `en-US-JessaNeural`
* `zh-CN-XiaoxiaoNeural`

As alterações são aplicadas no nível de frase e estilo variam por voz. Se não há suporte para um estilo, o serviço retornará fala no padrão com neutralidade de estilo de fala.

**Sintaxe**

```xml
<mstts:express-as type="string"></mstts:express-as>
```

**Atributos**

| Atributo | DESCRIÇÃO | Obrigatório/Opcional |
|-----------|-------------|---------------------|
| type | Especifica o estilo de fala. Atualmente, os estilos de fala são específicos de voz. | Necessário se ajustar ao estilo de fala para uma voz neural. Se usando `mstts:express-as`, em seguida, o tipo deve ser fornecido. Se um valor inválido for fornecido, esse elemento será ignorado. |

Use esta tabela para determinar quais estilos de fala têm suporte para cada voz neural.

| Voz | Type | DESCRIÇÃO |
|-------|------|-------------|
| `en-US-JessaNeural` | type=`cheerful` | Expressa emoção que está feliz e positivo |
| | type=`empathy` | Expressa uma noção de se preocupar e Noções básicas sobre |
| `zh-CN-XiaoxiaoNeural` | type=`newscast` | Expressa um tom formal, semelhante a difusões de notícias |
| | type=`sentiment` | Transmite uma mensagem tocando nas ou uma história |

**Exemplo**

Este trecho de código SSML ilustra como o `<mstts:express-as>` elemento é usado para alterar o estilo de fala para `cheerful`.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-JessaNeural">
        <mstts:express-as type="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Adicionar ou remover uma quebra/pausa

Use o `break` elemento para inserir pausas (ou quebras) entre palavras ou impedir pausas adicionadas automaticamente pelo serviço de texto em fala.

> [!NOTE]
> Use esse elemento para substituir o comportamento padrão de texto em fala (TTS) para uma palavra ou frase, se a voz sintetizada para essa palavra ou frase parece não natural. Definir `strength` para `none` para impedir que uma quebra de prosódica, que é inserida automaticamente pelo serviço de texto em fala.

**Sintaxe**

```xml
<break strength="string" />
<break time="string" />
```

**Atributos**

| Atributo | DESCRIÇÃO | Obrigatório/Opcional |
|-----------|-------------|---------------------|
| intensidade | Especifica a duração relativa de uma pausa usando um dos seguintes valores:<ul><li>Nenhum</li><li>x fraca</li><li>fraco</li><li>Médio (padrão)</li><li>Forte</li><li>x-strong</li></ul> | Opcional |
| time | Especifica a duração absoluta de uma pausa em segundos ou milissegundos. Exemplos de valores válidos são 2s e 500 | Opcional |

| intensidade | DESCRIÇÃO |
|----------|-------------|
| Nenhum, ou se nenhum valor fornecido | 0 ms |
| x fraca | 250 ms |
| fraco | 500 ms |
| média | 750 ms |
| Forte | 1000 ms |
| x-strong | 1250 ms |


**Exemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Especifique os parágrafos e frases

`p` e `s` elementos são usados para denotar os parágrafos e frases, respectivamente. Na ausência desses elementos, o serviço de texto em fala determina automaticamente a estrutura do documento SSML.

O `p` elemento pode conter texto e os elementos a seguir: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `sub`, `mstts:express-as`, e `s`.

O `s` elemento pode conter texto e os elementos a seguir: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `mstts:express-as`, e `sub`.

**Sintaxe**

```XML
<p></p>
<s></s>
```

**Exemplo**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <p>
            <s>Introducing the sentence element.</s>
            <s>Used to mark individual sentences.</s>
        </p>
        <p>
            Another simple paragraph.
            Sentence structure in this paragraph is not explicitly marked.
        </p>
    </voice>
</speak>
```

## <a name="use-phonemes-to-improve-pronunciation"></a>Usar fonemas para melhorar a pronúncia

O `ph` elemento é usado para pronúncia fonética em documentos SSML. O `ph` elemento pode conter apenas texto, nenhum outro elemento. Sempre fornece fala legível como um fallback.

Alfabetos fonéticos são compostos de telefones, que são compostos de letras, números ou caracteres, às vezes, em combinação. Cada telefone descreve um som exclusivo de fala. Isso é diferente do alfabeto latino, onde qualquer letra pode representar vários sons falados. Considere a pronúncia diferente da letra "c" na "interromper" e "balas" palavras ou a pronúncia diferente da combinação de letras "th" a "coisa" palavras e "os".

**Sintaxe**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Atributos**

| Atributo | DESCRIÇÃO | Obrigatório/Opcional |
|-----------|-------------|---------------------|
| alphabet | Especifica o alfabeto fonético para usar quando a pronúncia da cadeia de caracteres para resumir o `ph` atributo. A cadeia de caracteres especificando o alfabeto deve ser especificada em letras minúsculas. A seguir estão os alfabetos possíveis que você pode especificar.<ul><li>IPA &ndash; alfabeto fonético internacional</li><li>SAPI &ndash; Speech API de telefone definido</li><li>no-break &ndash; Universal telefone definido</li></ul>Alfabeto só se aplica a fonema no elemento. Para obter mais informações, consulte [referência do alfabeto fonético](https://msdn.microsoft.com/library/hh362879(v=office.14).aspx). | Opcional |
| ph | Uma cadeia de caracteres que contém os telefones que especificam a pronúncia da palavra no `phoneme` elemento. Se a cadeia de caracteres especificada contém os telefones não reconhecidos, o serviço de texto em fala (TTS) rejeita o documento inteiro do SSML e produz nenhuma saída de fala especificada no documento. | Necessário se estiver usando fonemas. |

**Exemplos**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

## <a name="adjust-prosody"></a>Ajustar a métrica

O `prosody` elemento é usado para especificar alterações em tom, countour, intervalo, a taxa, duração e volume para a saída de texto em fala. O `prosody` elemento pode conter texto e os elementos a seguir: `audio`, `break`, `p`, `phoneme`, `prosody`, `say-as`, `sub`, e `s`.

Como valores de atributo prosódica podem variar em uma ampla gama, o reconhecedor de fala interpreta os valores atribuídos como uma sugestão do que os valores reais prosódico da voz selecionado devem ser. O serviço de texto em fala limita ou substitui valores que não são suportados. Exemplos de valores sem suporte são um tom de 1 MHz ou um volume de 120.

**Sintaxe**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Atributos**

| Atributo | DESCRIÇÃO | Obrigatório/Opcional |
|-----------|-------------|---------------------|
| Densidade | Indica a densidade da linha do texto. Você pode expressar o tom como:<ul><li>Um valor absoluto, expressado como um número seguido por "Hz" (Hertz). Por exemplo, 600Hz.</li><li>Um valor relativo, expressado como um número precedido por "+" ou "-" e seguido por "Hz" ou "st", que especifica um valor para alterar o tom. Por exemplo: +80 Hz ou - 2st. "st" indica que a unidade de alteração é semitom, que é a metade de um tom (uma etapa de meia) na escala diatonic padrão.</li><li>Um valor constante:<ul><li>x-low</li><li>baixa</li><li>média</li><li>alto</li><li>x-high</li><li>padrão</li></ul></li></ul>. | Opcional |
| delimitação | Não há suporte para delimitação para vozes neurais. Delimitação representa as alterações no tom do conteúdo de fala como uma matriz de destinos em posições de tempo especificado na saída de fala. Cada destino é definido por conjuntos de pares de parâmetro. Por exemplo: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>O primeiro valor em cada conjunto de parâmetros Especifica o local da alteração de tom como um percentual da duração do texto. O segundo valor Especifica a quantidade para aumentar ou diminuir o tom, usando um valor relativo ou um valor de enumeração para tom (consulte `pitch`). | Opcional |
| range  | Um valor que representa o intervalo de densidade do texto. Você pode expressar `range` usando os mesmos valores absolutos, valores relativos ou valores de enumeração usado para descrever `pitch`. | Opcional |
| Taxa  | Indica a taxa de fala do texto. Você pode expressar `rate` como:<ul><li>Um valor relativo, expressado como um número que atua como um multiplicador de padrão. Por exemplo, um valor de *1* resulta em nenhuma alteração na taxa. Um valor de *.5* resulta em uma metade da taxa. Um valor de *3* resulta em um triplica da taxa.</li><li>Um valor constante:<ul><li>x-slow</li><li>lento</li><li>média</li><li>Rápida</li><li>x-fast</li><li>padrão</li></ul></li></ul> | Opcional |
| duration  | O período de tempo decorrido durante a fala o serviço de síntese (TTS) lê o texto, em segundos ou milissegundos. Por exemplo, *2s* ou *1800ms*. | Opcional |
| volume  | Indica o nível de volume da voz falada. Você pode expressar o volume como:<ul><li>Um valor absoluto, expressado como um número no intervalo de 0,0 a 100,0 da *quietest* à *mais agudos*. Por exemplo, 75. O padrão é 100,0.</li><li>Um valor relativo, expressado como um número precedido por "+" ou "-" que especifica um valor para alterar o volume. Por exemplo, + 10 ou-5.5.</li><li>Um valor constante:<ul><li>Silenciosa</li><li>x-soft</li><li>soft</li><li>média</li><li>alto</li><li>x-loud</li><li>padrão</li></ul></li></ul> | Opcional |

### <a name="change-speaking-rate"></a>Alterar taxa de fala

Taxa de fala pode ser aplicada a vozes padrão no word ou no nível da frase. Ao passo que a taxa de fala só pode ser aplicada a vozes neurais no nível de frase.

**Exemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        <prosody rate="+30.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-volume"></a>Alterar volume

Alterações de volume podem ser aplicadas à vozes padrão no word ou no nível da frase. Enquanto as alterações de volume só podem ser aplicadas para vozes neurais no nível de frase.

**Exemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody volume="+20.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-pitch"></a>Alterar densidade

Alterações de tom podem ser aplicadas à vozes padrão no word ou no nível da frase. Enquanto as alterações de tom só podem ser aplicadas para vozes neurais no nível de frase.

**Exemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Alterar contorno de densidade

> [!IMPORTANT]
> Alterações de delimitação de densidade não são compatíveis com as vozes neurais.

**Exemplo**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody contour="(80%,+20%) (90%,+30%)" >
            Good morning.
        </prosody>
    </voice>
</speak>
```

## <a name="next-steps"></a>Próximas etapas

* [Suporte de linguagem: vozes, localidades, idiomas](language-support.md)

---
title: Executar operações de imagem – Python
titlesuffix: Azure Cognitive Services
description: Saiba como usar a API da Pesquisa Visual Computacional com o Python usando notebooks do Jupyter. Visualize os resultados usando bibliotecas populares.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: b18f41db772c1c214bdf48c9bb765b1ce7e6d2c2
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604149"
---
# <a name="computer-vision-api-jupyter-notebook"></a>Bloco de anotações Jupyter da API de visão do computador

Este guia mostra como usar a API de visão do computador em Python e como visualizar os resultados usando bibliotecas populares. Você usará o Jupyter para executar o tutorial. Para saber como começar a usar Jupyter Notebooks interativos, confira a [Documentação do Jupyter](https://jupyter.readthedocs.io/en/latest/index.html).

## <a name="prerequisites"></a>Pré-requisitos

- [Python 2.7+ ou 3.5+](https://www.python.org/downloads/)
- Ferramenta [pip](https://pip.pypa.io/en/stable/installing/)
- [Jupyter Notebook](https://jupyter.org/install) instalado

## <a name="open-the-notebook-in-jupyter"></a>Abra o bloco de anotações no Jupyter 

1. Vá para o repositório GitHub de [Python para Visão Cognitiva](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Clique no botão verde para clonar ou baixar o repositório. 
3. Abra um prompt de comando e navegue até a pasta **Cognitive-Vision-Python\Jupyter Notebook**.
1. Verifique se você tem todas as bibliotecas necessárias executando o comando `pip install requests opencv-python numpy matplotlib` no prompt de comando.
1. Inicie o Jupyter executando o comando `jupyter notebook` no prompt de comando.
1. Na janela do Jupyter, clique em _API da Pesquisa Visual Computacional Example.ipynb_ para abrir o notebook do tutorial.

## <a name="run-the-notebook"></a>Executar o notebook

Para usar esse bloco de anotações, você precisará de uma chave de assinatura para a API da Pesquisa Visual Computacional. Visite a [página Assinatura](https://azure.microsoft.com/try/cognitive-services/) para se inscrever. Na página **Entrar**, use sua conta Microsoft para entrar e você poderá assinar e obter chaves gratuitas. Depois de concluir o processo de inscrição, cole a chave na seção `Variables` do notebook (reproduzida abaixo). Tanto a chave primária quanto a secundária funcionarão. Certifique-se de colocar a chave entre aspas para transformá-la em uma cadeia de caracteres.

Você também precisará garantir que o campo `_region` corresponde à região que corresponde à sua assinatura.

```python
# Variables
_region = 'westcentralus'  # Here you enter the region of your subscription
_url = 'https://{}.api.cognitive.microsoft.com/vision/v2.0/analyze'.format(
    _region)
_key = None  # Here you have to paste your primary key
_maxNumRetries = 10
```

Quando você executar o tutorial, você poderá adicionar imagens para analisar, de uma URL e do armazenamento local. O script exibirá as informações de análise e imagens no notebook.

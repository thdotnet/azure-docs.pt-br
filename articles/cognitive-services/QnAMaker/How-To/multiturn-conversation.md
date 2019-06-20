---
title: Ativar várias conversas
titleSuffix: Azure Cognitive Services
description: Usar prompts e contexto para gerenciar o vários ativa, conhecido como várias por sua vez, para o bot de uma pergunta para outro. Vários por sua vez é a capacidade de ter um back e para trás em que o contexto da pergunta anterior influencia a próxima pergunta e resposta de conversa.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/12/2019
ms.author: diberry
ms.openlocfilehash: 1e46c935d298f2fe7ebfa4bce471288c9ab8a606
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67271957"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Prompts de acompanhamento de uso para criar várias sequências de uma conversa

Usar prompts de acompanhamento e o contexto para gerenciar o vários ativa, conhecido como _ativar várias_, para o bot de uma pergunta para outro.

Assista a este vídeo de demonstração para ver como isso é feito.

[![](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample).

## <a name="what-is-a-multi-turn-conversation"></a>O que é uma conversa com vários por sua vez?

Algumas perguntas não podem ser respondidas em uma única rodada. Quando você projeta suas conversas de (bot de bate-papo) do aplicativo cliente, um usuário pode fazer uma pergunta que precisa ser filtrada ou refinados para determinar a resposta correta. Esse fluxo por meio de perguntas é possível, apresentando o usuário com **acompanhamento solicita**.

Quando o usuário faz a pergunta, o QnA Maker retorna a resposta _e_ quaisquer prompts de acompanhamento. Isso permite que você apresente as perguntas de acompanhamento como opções. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Conversa de turno vários de exemplo com o bot de bate-papo

Um chat bot gerencia a conversa com o usuário pergunta a pergunta, para determinar a resposta final.

![Dentro do fluxo de conversação, gerencie o estado da conversa em um sistema de caixa de diálogo de ativar várias fornecendo avisos dentro das respostas apresentados como opções para continuar a conversa.](../media/conversational-context/conversation-in-bot.png)

Na imagem anterior, o usuário inseriu `My account`. A base de Conhecimento tem 3 pares QnA vinculados. O usuário precisa selecionar uma das três opções para refinar a resposta. Na base de dados de Conhecimento, a pergunta (n º 1), tem três avisos de acompanhamento, apresentados no bot bate-papo como três opções (2). 

Quando o usuário seleciona uma opção (3), a lista próxima de refinar escolhas (4) é apresentada. Isso pode continuar (5) até que a resposta correta e final (6) é determinada.

A imagem anterior tem **Ativar Ativar várias** selecionada para exibidos prompts. 

### <a name="using-multi-turn-in-a-bot"></a>Usando vários por sua vez em um bot

Você precisará alterar seu aplicativo cliente para gerenciar a conversa contextual. Você precisará adicionar [código ao seu bot](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-prompting) para ver os prompts.  

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Criar uma conversa com vários por sua vez da estrutura de um documento

Quando você cria uma base de Conhecimento, você verá uma caixa de seleção opcional para habilitar a ativar a extração. 

![Quando você cria uma base de Conhecimento, você verá uma caixa de seleção opcional para habilitar a ativar a extração.](../media/conversational-context/enable-multi-turn.png)

Se você selecionar essa opção, quando você importa um documento, a conversa com vários por sua vez pode implícita da estrutura. Se existir nessa estrutura, o QnA Maker cria os pares de QnA prompt acompanhamento para você. 

Estrutura de folheio de vários só pode ser inferida de DOCX, PDF ou URLs de arquivos. 

A imagem a seguir de um Microsoft Surface [arquivo PDF](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf) destina-se a ser usado como um manual. Devido ao tamanho do arquivo PDF, o recurso do QnA Maker do Azure requer que a pesquisa de tipo de preço do B (15 índices) ou maior. 

![! [Se você importar um documento, contextual conversa pode ser implícita da estrutura. Se essa estrutura, QnA Maker criará os pares de QnA prompt acompanhamento para você, como parte da importação do documento.] (.. / media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

Ao importar o documento PDF, QnA Maker determina prompts de acompanhamento da estrutura para criar o fluxo de conversação. 

1. Na **etapa 1**, selecione **criar uma base de dados de Conhecimento** de navegação superior.
1. Na **etapa 2**, criar ou usar um serviço QnA existente. Certifique-se de usar um serviço QnA com um serviço de pesquisa de B (15 índices) ou superior porque o arquivo PDF do Manual de superfície é muito grande para uma camada menor.
1. Na **etapa 3**, insira um nome para sua base de dados de Conhecimento, como `Surface manual`.
1. Na **etapa 4**, selecione **habilitar ativar a extração de URLs, os arquivos. PDF ou. docx.** Selecione a URL para o superfície Manual

    ```text
    https://github.com/Azure-Samples/cognitive-services-sample-data-files/raw/master/qna-maker/data-source-formats/product-manual.pdf
    ```

1. Selecione o **criar seu KB** botão. 

    Depois de criar o conhecimento, exibe um modo de exibição de pares de pergunta e resposta.

## <a name="show-questions-and-answers-with-context"></a>Mostrar perguntas e respostas com contexto

Reduza os pares de perguntas e respostas exibidos apenas com conversas contextuais. 

1. Selecione **as opções de exibição**, em seguida, selecione **Mostrar contexto (visualização)** . A lista mostra os pares de pergunta e resposta que contém os prompts de acompanhamento. 

    ![Filtrar pergunta e resposta pares por conversas contextuais](../media/conversational-context/filter-question-and-answers-by-context.png)

2. O contexto de vários por sua vez exibe na primeira coluna.

    ![! [Ao importar o documento PDF, QnA Maker determina prompts de acompanhamento da estrutura para criar o fluxo de conversação. ](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

    Na imagem anterior, #1 indica o texto em negrito na coluna, o que significa a pergunta atual. A pergunta pai é o item superior na linha. Quaisquer perguntas abaixo estão os pares de pergunta e resposta vinculados. Esses itens são selecionáveis, portanto, você pode ir imediatamente para os outros itens de contexto. 

## <a name="add-existing-qna-pair-as-follow-up-prompt"></a>Adicionar o par de QnA existente como o prompt de acompanhamento

A pergunta original de `My account` tem os prompts de acompanhamento, como `Accounts and signing in`. 

![A pergunta original de 'Minha conta' retorna corretamente o 'Contas e entrar no' responder e já tem os prompts de acompanhamento vinculados.](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Adicione um prompt de acompanhamento para um par de QnA existente que atualmente não está vinculado. Porque a questão não está vinculada a qualquer par de QnA, a configuração do modo de exibição atual precisa ser alterado.

1. Para vincular um par de QnA existente como um prompt de acompanhamento, selecione a linha para o par de pergunta e resposta. Para a superfície manual, pesquise por `Sign out` para reduzir a lista.
1. Na linha de `Signout`, selecione **prompt de acompanhamento de Add** da **resposta** coluna.
1. No **prompt de acompanhamento (versão prévia)** janela pop-up, insira o seguinte:

    |Campo|Value|
    |--|--|
    |Exibir texto|`Turn off the device`. Isso é texto personalizado que você opte por exibir no prompt de acompanhamento.|
    |Somente contexto|Selecionado. Essa resposta só será retornada se a pergunta Especifica o contexto.|
    |Link para a resposta|Insira `Use the sign-in screen` para localizar o par de QnA existente.|


1.  Uma correspondência é retornada. Selecione essa resposta, como o acompanhamento e selecione **salvar**. 

    ![Link do prompt de acompanhamento para a caixa de diálogo de resposta para uma resposta já existente, usando o texto da resposta de pesquisa.](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. Depois de ter adicionado o prompt de acompanhamento, lembre-se de selecionar **salvar e treinar** no painel de navegação superior.
  
### <a name="edit-the-display-text"></a>Editar o texto de exibição 

Quando um prompt de acompanhamento é criado e um par de QnA existente é selecionado como o **Link para a resposta**, você pode inserir novos **exibir texto**. Esse texto não substitui a pergunta existente e não adiciona uma nova pergunta alternativa. Ele é separado desses valores. 

1. Para editar o texto de exibição, pesquise e selecione a pergunta a **contexto** campo.
1. Na linha dessa pergunta, selecione o prompt de acompanhamento na coluna de resposta. 
1. Selecione o texto de exibição que você deseja editar e, em seguida, selecione **editar**.

    ![Selecione o texto de exibição que você deseja editar e, em seguida, selecione Editar.](../media/conversational-context/edit-existing-display-text.png)

1. O **prompt de acompanhamento** pop-up permite que você altere o texto de exibição existente. 
1. Quando você terminar a edição do texto de exibição, selecione **salvar**. 
1. Lembre-se de selecionar **salvar e treinar** no painel de navegação superior.


<!--

## To find best prompt answer, add metadata to follow-up prompts 

If you have several follow-up prompts for a given QnA pair, but you know as the knowledge base manager, that not all prompts should be returned, use metadata to categorize the prompts in the knowledge base, then send the metadata from the client application as part of the GenerateAnswer request.

In the knowledge base, when a question-and-answer pair is linked to follow-up prompts, the metadata filters are applied first, then the follow-ups are returned.

1. For the two follow-up QnA pairs, add metadata to each one:

    |Question|Add metadata|
    |--|--|
    |`Feedback on an QnA Maker service`|"Feature":"all"|
    |`Feedback on an existing feature`|"Feature":"one"|
    
    ![Add metadata to follow-up prompt so it can be filtered in conversation response from service](../media/conversational-context/add-metadata-feature-to-follow-up-prompt.png) 

1. Save and train. 

    When you send the question `Give feedback` with the metadata filter `Feature` with a value of `all`, only the QnA pair with that metadata will be returned. Both QnA pairs are not returned because they both do not match the filter. 

-->

## <a name="add-new-qna-pair-as-follow-up-prompt"></a>Adicionar novo par de QnA como prompt de acompanhamento

Adicione um novo par de QnA à base de Conhecimento. O par de QnA deve ser vinculado a uma pergunta existente como um prompt de acompanhamento.

1. Na barra de ferramentas da base de Conhecimento, procure e selecione o par de QnA existente para `Accounts and Signing In`. 

1. No **resposta** coluna para essa pergunta, selecione **prompt de acompanhamento de adicionar**. 
1. O **prompt de acompanhamento (versão prévia)** , crie um novo prompt de acompanhamento, inserindo os seguintes valores: 

    |Campo de texto|Value|
    |--|--|
    |**Texto de exibição**|`Create a Windows Account`. Isso é texto personalizado que você opte por exibir no prompt de acompanhamento.|
    |**Context-only**|Selecionado. Essa resposta só será retornada se a pergunta Especifica o contexto.|
    |**Link para responder**|Insira o texto a seguir como a resposta:<br>`[Create](https://account.microsoft.com/) a Windows account with a new or existing email account.`<br>Quando você salva e treinar o banco de dados, esse texto será convertido em |
    |||

    ![Criar novo QnA prompt](../media/conversational-context/create-child-prompt-from-parent.png)


1. Selecione **criar novo** , em seguida, selecione **salvar**. 

    Isso criou um novo par de perguntas e respostas e vinculados a pergunta selecionada como um prompt de acompanhamento. O **contexto** coluna, ambas as perguntas, indica uma relação de prompt de acompanhamento. 

1. Alterar o **as opções de exibição** ao [Mostrar contexto](#show-questions-and-answers-with-context).

    A nova pergunta mostra como ele está vinculado.

    ![Criar um novo prompt de acompanhamento ](../media/conversational-context/new-qna-follow-up-prompt.png)

    A pergunta pai mostra a nova pergunta como uma das suas opções.

    ![! [A coluna de contexto, ambas as perguntas, indica uma relação de prompt de acompanhamento.] (.. / media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Depois de ter adicionado o prompt de acompanhamento, lembre-se de selecionar **salvar e treinar** no painel de navegação superior.

## <a name="enable-multi-turn-when-testing-follow-up-prompts"></a>Ativar várias Ativar quando solicita o acompanhamento de teste

Quando o teste a pergunta com acompanhamento solicitar que no **teste** painel, selecione **Ativar Ativar várias**e digite sua pergunta. A resposta inclui os prompts de acompanhamento.

![Ao testar a pergunta no painel de teste, a resposta inclui os prompts de acompanhamento.](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

Se você não habilitar vários por sua vez, a resposta será retornada mas prompts de acompanhamento não são retornadas.

## <a name="json-request-to-return-initial-answer-and-follow-up-prompts"></a>Solicitação JSON para retornar a resposta inicial e as solicitações de acompanhamento

Use a esvaziar `context` objeto para solicitar a resposta à pergunta do usuário e incluir prompts de acompanhamento. 

```JSON
{
  "question": "accounts and signing in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="json-response-to-return-initial-answer-and-follow-up-prompts"></a>Resposta JSON para retornar a resposta inicial e as solicitações de acompanhamento

A seção anterior solicitou uma resposta e os prompts de acompanhamento para `Accounts and signing in`. A resposta inclui as informações de aviso, localizadas em `answers[0].context`, incluir o texto a ser exibido ao usuário. 

```JSON
{
    "answers": [
        {
            "questions": [
                "Accounts and signing in"
            ],
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way he or she likes. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 100.0,
            "id": 15,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "promptsToAdd": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    },
                    {
                        "displayOrder": 1,
                        "qnaId": 17,
                        "qna": null,
                        "displayText": "Use Windows Hello to sign in"
                    },
                    {
                        "displayOrder": 2,
                        "qnaId": 18,
                        "qna": null,
                        "displayText": "Sign out"
                    },
                    {
                        "displayOrder": 0,
                        "qnaId": 79,
                        "qna": null,
                        "displayText": "Create a Windows Account"
                    }
                ],
                "promptsToDelete":[]
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start , and right-click your name. Then select Sign out. ",
            "score": 38.01,
            "id": 18,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "promptsToAdd": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Turn off the device"
                    }
                ],
                "promptsToDelete":[]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 27.53,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "promptsToAdd": [],
                "promptsToDelete":[]
            }
        }
    ]
}
```

O `promptsToAdd` matriz fornece o texto em de `displayText` propriedade e o `qnaId` fluxo de valor, portanto, você pode mostrar essas respostas como as opções exibidas Avançar na conversa, enviar, em seguida, o valor selecionado para o QnA Maker na solicitação a seguir. 

<!--

The `promptsToDelete` array provides the ...

-->

## <a name="json-request-to-return-non-initial-answer-and-follow-up-prompts"></a>Solicitação JSON para retornar a resposta não inicial e as solicitações de acompanhamento

Preencher o `context` objeto para incluir o contexto anterior.

Na solicitação JSON a seguir, a pergunta atual é `Use Windows Hello to sign in` e a pergunta anterior era `Accounts and signing in`. 

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "qnaId": 17,
  "context": {
    "previousQnAId": 15,
    "previousUserQuery": "accounts and signing in"
  }
}
``` 

##  <a name="json-response-to-return-non-initial-answer-and-follow-up-prompts"></a>Resposta JSON para retornar a resposta não inicial e as solicitações de acompanhamento

O QnA Maker _GenerateAnswer_ resposta JSON inclui os prompts de acompanhamento na `context` propriedade do primeiro item no `answers` objeto:

```JSON
{
    "answers": [
        {
            "questions": [
                "Use Windows Hello to sign in"
            ],
            "answer": "**Use Windows Hello to sign in**\n\nSince Surface Pro 4 has an infrared (IR) camera, you can set up Windows Hello to sign in just by looking at the screen. \n\nIf you have the Surface Pro 4 Type Cover with Fingerprint ID (sold separately), you can set up your Surface sign you in with a touch. \n\nFor more info, see What is Windows Hello? on Windows.com. ",
            "score": 100.0,
            "id": 17,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "promptsToAdd": [],
                "promptsToDelete":[]
            }
        },
        {
            "questions": [
                "Meet Surface Pro 4"
            ],
            "answer": "**Meet Surface Pro 4**\n\nGet acquainted with the features built in to your Surface Pro 4. \n\nHere’s a quick overview of Surface Pro 4 features: \n\n\n\n\n\n\n\nPower button \n\n\n\n\n\nPress the power button to turn your Surface Pro 4 on. You can also use the power button to put it to sleep and wake it when you’re ready to start working again. \n\n\n\n\n\n\n\nTouchscreen \n\n\n\n\n\nUse the 12.3” display, with its 3:2 aspect ratio and 2736 x 1824 resolution, to watch HD movies, browse the web, and use your favorite apps. \n\nThe new Surface G5 touch processor provides up to twice the touch accuracy of Surface Pro 3 and lets you use your fingers to select items, zoom in, and move things around. For more info, see Surface touchscreen on Surface.com. \n\n\n\n\n\n\n\nSurface Pen \n\n\n\n\n\nEnjoy a natural writing experience with a pen that feels like an actual pen. Use Surface Pen to launch Cortana in Windows or open OneNote and quickly jot down notes or take screenshots. \n\nSee Using Surface Pen (Surface Pro 4 version) on Surface.com for more info. \n\n\n\n\n\n\n\nKickstand \n\n\n\n\n\nFlip out the kickstand and work or play comfortably at your desk, on the couch, or while giving a hands-free presentation. \n\n\n\n\n\n\n\nWi-Fi and Bluetooth® \n\n\n\n\n\nSurface Pro 4 supports standard Wi-Fi protocols (802.11a/b/g/n/ac) and Bluetooth 4.0. Connect to a wireless network and use Bluetooth devices like mice, printers, and headsets. \n\nFor more info, see Add a Bluetooth device and Connect Surface to a wireless network on Surface.com. \n\n\n\n\n\n\n\nCameras \n\n\n\n\n\nSurface Pro 4 has two cameras for taking photos and recording video: an 8-megapixel rear-facing camera with autofocus and a 5-megapixel, high-resolution, front-facing camera. Both cameras record video in 1080p, with a 16:9 aspect ratio. Privacy lights are located on the right side of both cameras. \n\nSurface Pro 4 also has an infrared (IR) face-detection camera so you can sign in to Windows without typing a password. For more info, see Windows Hello on Surface.com. \n\nFor more camera info, see Take photos and videos with Surface and Using autofocus on Surface 3, Surface Pro 4, and Surface Book on Surface.com. \n\n\n\n\n\n\n\nMicrophones \n\n\n\n\n\nSurface Pro 4 has both a front and a back microphone. Use the front microphone for calls and recordings. Its noise-canceling feature is optimized for use with Skype and Cortana. \n\n\n\n\n\n\n\nStereo speakers \n\n\n\n\n\nStereo front speakers provide an immersive music and movie playback experience. To learn more, see Surface sound, volume, and audio accessories on Surface.com. \n\n\n\n\n",
            "score": 21.92,
            "id": 3,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "promptsToAdd": [
                    {
                        "displayOrder": 0,
                        "qnaId": 4,
                        "qna": null,
                        "displayText": "Ports and connectors"
                    }
                ],
                "promptsToDelete":[]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 19.04,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "promptsToAdd": [],
                "promptsToDelete":[]
            }
        }
    ]
}
```

## <a name="query-the-knowledge-base-with-the-qna-id"></a>Consultar a base de dados de conhecimento com a ID do QnA

Na resposta da pergunta inicial, os prompts de acompanhamento e seus associados `qnaId` é retornado. Agora que você tem a ID, você pode passar isso no corpo de solicitação do prompt de acompanhamento. Se o corpo da solicitação contém o `qnaId`e o objeto de contexto (que contém as propriedades do QnA anteriores), então GenerateAnswer retornará a pergunta exata por ID, em vez de usar o algoritmo de classificação para encontrar a resposta, o texto da pergunta. 

## <a name="displaying-prompts-and-sending-context-in-the-client-application"></a>Exibindo prompts e enviar o contexto no aplicativo cliente 

Você adicionou prompts em sua base de dados de conhecimento e testado o fluxo no painel de teste. Agora, você precisa usar esses prompts no aplicativo cliente. Para o Bot Framework, os avisos não serão iniciado automaticamente aparecendo nos aplicativos cliente. Você pode mostrar os prompts como botões ou ações sugeridas como parte da resposta à consulta do usuário no cliente de aplicativos, incluindo isso [exemplo de estrutura do Bot](https://aka.ms/qnamakermultiturnsample) em seu código. O aplicativo cliente deverá armazenar a ID do QnA atual e a consulta de usuário e transmiti-los a [objeto de contexto da API GenerateAnswer](#json-request-to-return-non-initial-answer-and-follow-up-prompts) para a próxima consulta de usuário. 

## <a name="display-order-supported-in-api"></a>Ordem de exibição com suporte na API

O [exibir o texto e a ordem de exibição](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto), retornado na resposta JSON, há suporte para edição pela [Update API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). 

<!--

FIX - Need to go to parent, then answer column, then edit answer. 

-->

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre conversas contextuais do [exemplo de caixa de diálogo](https://aka.ms/qnamakermultiturnsample) ou aprender mais [bot conceitual de design para ativar várias conversas](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Migrar uma base de Conhecimento](../Tutorials/migrate-knowledge-base.md)

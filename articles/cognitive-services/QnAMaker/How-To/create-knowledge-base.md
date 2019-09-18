---
title: 'Início Rápido: Criar base de dados de conhecimento – QnA Maker'
titleSuffix: Azure Cognitive Services
description: Use o portal do serviço de API do QnA Maker para criar uma base de dados de conhecimento com bate-papo. Isso torna seu aplicativo envolvente. Adicione um conjunto pré-populado do bate-papo principal à sua base de dados de conhecimento como ponto de partida para o bate-papo do seu bot, economizando o tempo e o custo de escrevê-lo do zero.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 09/04/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 798bbb725d4764c5ec7a1d69770e9508af0fdf5a
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70376208"
---
# <a name="quickstart-create-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Início Rápido: Criar uma base de dados de conhecimento usando portal do Serviço de API do QnA Maker

O portal do Serviço de API do QnA Maker simplifica a adição das fontes de dados existentes ao criar uma base de dados de conhecimento. Você pode criar uma nova base de dados de conhecimento do QnA Maker dos seguintes tipos de documento:

<!-- added for scanability -->
* Páginas de perguntas frequentes
* Manuais de produtos
* Documentos estruturados

Inclua uma personalidade de bate-papo para tornar suas informações mais interessantes para seus usuários.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

## <a name="create-a-new-knowledge-base"></a>Como criar uma nova base de dados de conhecimento

1. Entre no [portal do QnA Maker](https://qnamaker.ai) com suas credenciais do Azure e selecione **Criar uma base de dados de conhecimento**.

1. Se você ainda não criou um serviço do QnA Maker, selecione **Criar um serviço do QnA**. 

1. Selecione seu locatário do Azure, o nome da assinatura do Azure e o nome do recurso do Azure associados ao serviço do QnA Maker nas listas na **Etapa 2** no portal do QnA Maker. Selecione o serviço do QnA Maker do Azure que hospedará a Base de dados de Conhecimento.

    ![Configurar serviço do QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

1. Insira o nome da sua base de dados de conhecimento e as fontes de dados para a nova base de dados de conhecimento.

    ![Definir fontes de dados](../media/qnamaker-how-to-create-kb/set-data-sources.png)

1. Forneça um **nome** ao serviço, como `my first kb`. Nomes duplicados e caracteres especiais são compatíveis.

1. Adicione a página de solução de problemas do QnA Maker como uma URL: `https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/troubleshooting` e, em seguida, selecione `+ Add URL`. Consulte mais informações sobre os tipos de fontes com suporte [aqui](../Concepts/data-sources-supported.md). Para esse início rápido, **não carregue arquivos** para os dados que deseja extrair. Consulte as [informações sobre preços](https://aka.ms/qnamaker-pricing) para ver quantos documentos é possível adicionar.

1. Adicione o Bate-Papo **_Professional_** à sua KB. 

1. Selecione **Criar sua KB**.

    ![Criar uma base de dados de conhecimento](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Pode demorar alguns minutos para os dados serem extraídos.

    ![Extração](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

1. Quando a base de dados de conhecimento for criada com êxito, você será redirecionado para a página **Base de dados de conhecimento**.

## <a name="clean-up-resources"></a>Limpar recursos

Quando terminar de usar a base de dados de conhecimento, remova a base no portal do QnA Maker.

## <a name="next-steps"></a>Próximas etapas

Como medida de economia de custo, você pode [compartilhar](set-up-qnamaker-service-azure.md#share-existing-services-with-qna-maker) alguns, mas não todos os recursos do Azure criados para o QnA Maker.

> [!div class="nextstepaction"]
> [Adicionar perguntas com metadados](../quickstarts/add-question-metadata-portal.md)

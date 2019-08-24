---
title: Criar base de dados de conhecimento-QnA Maker
titleSuffix: Azure Cognitive Services
description: Use o portal de serviço API de QnA Maker para adicionar criar uma base de dados de conhecimento com o Chit-Chat. Isso torna seu aplicativo envolvente. Adicione um conjunto pré-populado do bate-papo principal à sua base de dados de conhecimento como ponto de partida para o bate-papo do seu bot, economizando o tempo e o custo de escrevê-lo do zero.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b2fb7496f16359f01ddbbe6db31b2d047a2ab4df
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70012822"
---
# <a name="quickstart-create-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Início Rápido: Criar uma base de dados de conhecimento usando o portal do serviço API de QnA Maker

O portal do serviço API de QnA Maker simplifica a adição de suas fontes de dados existentes ao criar uma base de conhecimento. Você pode criar uma nova base de dados de conhecimento do QnA Maker dos seguintes tipos de documento:

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

1. Dê um **nome** ao seu serviço, `my first kb`como. Nomes duplicados e caracteres especiais são compatíveis.

1. Adicione a página de solução de problemas QnA Maker como uma `https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/troubleshooting`URL:, `+ Add URL`em seguida, selecione. Consulte mais informações sobre os tipos de fontes com suporte [aqui](../Concepts/data-sources-supported.md). Para este guia de início rápido, **não carregue arquivos** para dados que você deseja extrair. Consulte as [informações sobre preços](https://aka.ms/qnamaker-pricing) para ver quantos documentos é possível adicionar.

1. Adicione o  **_Professional_ Chit-Chat** à sua KB. 

1. Selecione **Criar sua KB**.

    ![Criar uma base de dados de conhecimento](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Pode levar alguns minutos para que os dados sejam extraídos.

    ![Extração](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

1. Quando sua base de dados de conhecimento for criada com êxito, você será redirecionado para a página **da base de dados de conhecimento** .

## <a name="clean-up-resources"></a>Limpar recursos

Quando terminar de usar a base de dados de conhecimento, remova a base no portal do QnA Maker.

## <a name="next-steps"></a>Próximas etapas

Para medidas de economia de custo, você pode [compartilhar](upgrade-qnamaker-service.md?#share-existing-services-with-qna-maker) alguns, mas não todos os recursos do Azure criados para QnA Maker.

> [!div class="nextstepaction"]
> [Adicionar personalidade ao bate-papo](./chit-chat-knowledge-base.md)

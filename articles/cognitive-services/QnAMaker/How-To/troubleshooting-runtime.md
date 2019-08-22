---
title: Solução de problemas – QnAMaker
titleSuffix: Azure Cognitive Services
description: O QnAMaker inclui componentes hospedados na conta do usuário do Azure. A depuração pode exigir que os usuários manipulem os recursos do QnAMaker Azure ou forneçam à equipe de suporte do QnAMaker informações adicionais sobre a configuração.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 08/20/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b58a08c4cfa97356be4064456b2e9ec5cceccfbc
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876334"
---
# <a name="troubleshooting-tips-to-support-the-qna-maker-service-and-runtime"></a>Dicas de solução de problemas para dar suporte ao tempo de execução e ao serviço do QnA Maker

QnAMaker inclui recursos hospedados na assinatura do Azure do usuário. A depuração pode exigir que os usuários manipulem seus recursos de QnAMaker do Azure ou forneçam à equipe de suporte do QnAMaker informações adicionais sobre a configuração.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Como obter as últimas atualizações do tempo de execução do QnAMaker

O tempo de execução do QnAMaker faz parte do serviço Azure App implantado quando você [cria um serviço QnAMaker](./set-up-qnamaker-service-azure.md) no portal do Azure. Atualizações são feitas periodicamente para o tempo de execução. QnA Maker serviço de aplicativo está no modo de atualização automática após a versão da extensão de site de abril de 2019 (versão 5 +). Isso já foi projetado para cuidar do tempo de inatividade ZERO durante as atualizações. 

Você pode verificar a versão atual em https://www.qnamaker.ai/UserSettings. Se sua versão for anterior à versão 5. x, você deverá reiniciar o serviço de aplicativo para aplicar as atualizações mais recentes.

1. Acesse o serviço do QnAMaker (grupo de recursos) no [portal do Azure](https://portal.azure.com)

    ![Grupo de recursos do Azure do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Clique no Serviço de Aplicativo e abra a seção Visão Geral

     ![Serviço de Aplicativo do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. Reinicie o serviço de Aplicativo. Isso deve concluir dentro de alguns segundos. Observe que quaisquer aplicativos dependentes ou bots que usam esse serviço QnAMaker não estarão disponíveis para os usuários finais durante esse período de reinicialização.

    ![Reinício do serviço de aplicativo do QnAMaker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Como obter o nome de host de serviço do QnAMaker
O nome do host de serviço do QnAMaker será útil para finalidades de depuração quando você contatar o Suporte do QnAMaker ou UserVoice. O nome do host é uma URL neste formato: https:// *{hostname}* . azurewebsites.net.
    
1. Acesse o serviço do QnAMaker (grupo de recursos) no [portal do Azure](https://portal.azure.com)

    ![Grupo de recursos do Azure do QnAMaker no portal do Azure](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Selecione o serviço de aplicativo associado ao recurso de QnA Maker. Normalmente, os nomes são os mesmos.

     ![Selecione o Serviço de Aplicativo do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. A URL do nome do host está disponível na seção Visão Geral

    ![Nome do host do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Melhorar as perguntas da base de dados de conhecimento com o aprendizado ativo](./improve-knowledge-base.md)

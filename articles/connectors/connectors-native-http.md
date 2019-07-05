---
title: Conectar-se aos pontos de extremidade HTTP ou HTTPS do aplicativo lógico do Azure
description: Monitorar pontos de extremidade HTTP ou HTTPS em tarefas automatizadas, processos e fluxos de trabalho por meio de aplicativos lógicos do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/05/2019
tags: connectors
ms.openlocfilehash: fa5fd3ef8b144826468f56ea2a14be592cef5dc1
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541282"
---
# <a name="call-http-or-https-endpoints-by-using-azure-logic-apps"></a>Chamar pontos de extremidade HTTP ou HTTPS, usando aplicativos lógicos do Azure

Com o [aplicativos lógicos do Azure](../logic-apps/logic-apps-overview.md) e o conector HTTP interno, você pode automatizar fluxos de trabalho que regularmente para chamar qualquer ponto de extremidade HTTP ou HTTPS, criação de aplicativos lógicos. Por exemplo, você pode monitorar o ponto de extremidade de serviço para seu site, verificando o ponto de extremidade em uma agenda especificada. Quando ocorre um evento específico naquele ponto de extremidade, como o seu site ficar inativo, o evento dispara o fluxo de trabalho do aplicativo lógico e executa as ações especificadas.

Para verificar ou *sondagem* um ponto de extremidade em um agendamento regular, você pode usar o gatilho HTTP como a primeira etapa no fluxo de trabalho. Em cada verificação, o gatilho envia uma chamada ou *solicitação* ao ponto de extremidade. A resposta do ponto de extremidade determina se o fluxo de trabalho do aplicativo lógico é executado. O gatilho passa todo o conteúdo da resposta para as ações no aplicativo lógico.

Você pode usar a ação HTTP como qualquer outra etapa no fluxo de trabalho para chamar o ponto de extremidade quando quiser. A resposta do ponto de extremidade determina como as ações restantes do fluxo de trabalho são executadas.

Com base em capacidade de um ponto de extremidade de destino, o conector HTTP dá suporte à segurança de camada de transporte (TLS) versões 1.0, 1.1 e 1.2. Os aplicativos lógicos negocia com o ponto de extremidade usando a versão com suporte mais alta possível. Portanto, por exemplo, se o ponto de extremidade suportar 1.2, o conector usa 1.2 pela primeira vez. Caso contrário, o conector usa a próxima versão com suporte mais alta.

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, [inscreva-se em uma conta gratuita do Azure](https://azure.microsoft.com/free/).

* A URL para o ponto de extremidade de destino que você deseja chamar

* Conhecimento básico sobre [como criar aplicativos lógicos](../logic-apps/quickstart-create-first-logic-app-workflow.md). Se ainda não estiver familiarizado com os aplicativos lógicos, veja [O que é o Aplicativo Lógico do Azure?](../logic-apps/logic-apps-overview.md)

* O aplicativo lógico de onde você deseja chamar o ponto de extremidade de destino. Para iniciar com o gatilho HTTP [criar um aplicativo lógico em branco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar a ação HTTP, inicie seu aplicativo lógico com qualquer gatilho que você deseja. Este exemplo usa o gatilho HTTP como a primeira etapa.

## <a name="add-an-http-trigger"></a>Adicionar um gatilho HTTP

Esse gatilho internos faz uma chamada HTTP para a URL especificada para um ponto de extremidade e retorna uma resposta.

1. Entre no [Portal do Azure](https://portal.azure.com). Abra seu aplicativo lógico em branco no Designer de aplicativo lógico.

1. No designer, na caixa de pesquisa, digite "http" como filtro. Dos **disparadores** lista, selecione o **HTTP** gatilho.

   ![Selecionar o gatilho HTTP](./media/connectors-native-http/select-http-trigger.png)

   Este exemplo renomeia o gatilho "Gatilho de HTTP", para que a etapa que tenha um nome mais descritivo. Além disso, o exemplo adiciona posteriormente uma ação HTTP, e ambos os nomes devem ser exclusivos.

1. Forneça os valores para o [parâmetros de gatilho HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger) que você deseja incluir na chamada para o ponto de extremidade de destino. Configure a recorrência para a frequência com que você deseja que o gatilho para verificar o ponto de extremidade de destino.

   ![Insira os parâmetros do gatilho HTTP](./media/connectors-native-http/http-trigger-parameters.png)

   Para obter mais informações sobre tipos de autenticação disponíveis para HTTP, consulte [autenticar HTTP gatilhos e ações](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Para adicionar outros parâmetros disponíveis, abra o **adicionar novo parâmetro** lista e, em seguida, selecione os parâmetros que você deseja.

1. Continue criando o fluxo de trabalho do aplicativo lógico com as ações que são executadas quando o gatilho é acionado.

1. Quando tiver terminado, feito, lembre-se de salvar seu aplicativo lógico. Na barra de ferramentas designer, selecione **salvar**.

## <a name="add-an-http-action"></a>Adicionar uma ação de HTTP

Essa ação interna faz uma chamada HTTP para a URL especificada para um ponto de extremidade e retorna uma resposta.

1. Entre no [Portal do Azure](https://portal.azure.com). Abra seu aplicativo lógico no Logic App Designer.

   Este exemplo usa o gatilho HTTP como a primeira etapa.

1. Na etapa onde você deseja adicionar a ação HTTP, selecione **nova etapa**.

   Para adicionar uma ação entre as etapas, mova o ponteiro sobre a seta entre as etapas. Selecione o sinal de adição ( **+** ) que aparece e, em seguida, selecione **adicionar uma ação**.

1. No designer, na caixa de pesquisa, digite "http" como filtro. Dos **ações** lista, selecione o **HTTP** ação.

   ![Selecionar a ação HTTP](./media/connectors-native-http/select-http-action.png)

   Este exemplo renomeia a ação a ser "Ação de HTTP", para que a etapa que tenha um nome mais descritivo.

1. Forneça os valores para o [parâmetros de ação de HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action) que você deseja incluir na chamada para o ponto de extremidade de destino.

   ![Inserir os parâmetros da ação HTTP](./media/connectors-native-http/http-action-parameters.png)

   Para obter mais informações sobre tipos de autenticação disponíveis para HTTP, consulte [autenticar HTTP gatilhos e ações](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Para adicionar outros parâmetros disponíveis, abra o **adicionar novo parâmetro** lista e, em seguida, selecione os parâmetros que você deseja.

1. Quando tiver terminado, lembre-se de salvar seu aplicativo lógico. Na barra de ferramentas designer, selecione **salvar**.

## <a name="connector-reference"></a>Referência de conector

Para obter mais informações sobre parâmetros de gatilho e ação, consulte estas seções:

* [Parâmetros de gatilho HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger)
* [Parâmetros de ação de HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action)

### <a name="output-details"></a>Detalhes de saída

Aqui está a obter mais informações sobre as saídas de um gatilho HTTP ou uma ação, que retorna essas informações:

| Nome da propriedade | Type | DESCRIÇÃO |
|---------------|------|-------------|
| headers | object | Os cabeçalhos da solicitação |
| body | object | Objeto JSON | O objeto com o conteúdo do corpo da solicitação |
| código de status | int | O código de status da solicitação |
|||

| Código de status | DESCRIÇÃO |
|-------------|-------------|
| 200 | OK |
| 202 | Aceita |
| 400 | Solicitação incorreta |
| 401 | Não Autorizado |
| 403 | Proibido |
| 404 | Não encontrado |
| 500 | Erro interno do servidor. Ocorreu um erro desconhecido. |
|||

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre outros [conectores de Aplicativos Lógicos](../connectors/apis-list.md)

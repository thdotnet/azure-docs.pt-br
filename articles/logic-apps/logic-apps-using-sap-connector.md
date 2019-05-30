---
title: Conectar-se aos sistemas SAP – aplicativos lógicos do Azure
description: Acessar e gerenciar recursos do SAP com a automatização de fluxos de trabalho com aplicativos lógicos do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 05/09/2019
tags: connectors
ms.openlocfilehash: bccefec80ef3afd6d312bb1048d3be5d8e708728
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258161"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Conectar aos sistemas SAP a partir do Aplicativos Lógicos do Azure

Este artigo mostra como você pode acessar seus recursos SAP no local de dentro de um aplicativo lógico usando o conector do SAP. O conector funciona com versões clássicas do SAP, como R/3 e sistemas ECC locais. O conector também permite a integração com sistemas SAP HANA com base em mais recentes da SAP como S/4 HANA, independentemente de onde eles estão hospedados - localmente ou na nuvem. O conector SAP dá suporte à integração de dados ou mensagem de e para sistemas baseados no SAP NetWeaver por meio do documento intermediário (IDoc) ou Business Application Programming Interface (BAPI) ou chamada de função remota (RFC).

O conector SAP usa o [biblioteca do SAP .NET Connector (NCo)](https://support.sap.com/en/product/connectors/msnet.html) e fornece essas operações ou ações:

* **Enviar para o SAP**: Enviar IDoc pela tRFC, chamar funções BAPI por RFC ou chamar RFC/tRFC em sistemas SAP.
* **Receber do SAP**: Receber IDoc via tRFC, chamar funções BAPI por tRFC ou chamar RFC/tRFC em sistemas SAP.
* **Gerar esquemas**: Gera esquemas para os artefatos do SAP para IDoc, BAPI ou RFC.

Para todas as operações acima, o conector SAP dá suporte para autenticação Básica por meio de nome de usuário e senha. O conector também suporta [comunicações de rede segura (SNC)](https://help.sap.com/doc/saphelp_nw70/7.0.31/e6/56f466e99a11d1a5b00000e835363f/content.htm?no_cache=true), que pode ser usado para SAP NetWeaver Single Sign-On ou para recursos de segurança adicionais fornecidos por um produto de segurança externas.

O conector SAP integra-se a sistemas SAP locais por meio do [gateway de dados local](../logic-apps/logic-apps-gateway-connection.md). Em cenários de envio, por exemplo, ao enviar uma mensagem de aplicativos lógicos a um sistema SAP, o gateway de dados atua como um cliente do RFC e encaminha as solicitações recebidas de aplicativos lógicos para SAP.
Da mesma forma, em cenários de recebimento, o gateway de dados atua como um servidor RFC que recebe solicitações de SAP e a encaminha para o aplicativo lógico.

Este artigo mostra como criar aplicativos lógicos de exemplo que se integram ao SAP ao abordar os cenários de integração descritos anteriormente.

<a name="pre-reqs"></a>

## <a name="prerequisites"></a>Pré-requisitos

Para seguir com este artigo, são necessário esses itens:

* Uma assinatura do Azure. Caso você ainda não tenha uma assinatura do Azure, [inscreva-se em uma conta gratuita do Azure](https://azure.microsoft.com/free/).

* O aplicativo lógico de onde você deseja acessar seu sistema SAP e um gatilho que inicia o fluxo de trabalho do aplicativo lógico. Se ainda não estiver familiarizado com aplicativo lógicos, consulte [O que são os Aplicativos Lógicos do Azure](../logic-apps/logic-apps-overview.md) e [Início Rápido: criar seu primeiro aplicativo lógico](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Seu [Servidor de Aplicativos SAP](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) ou [Servidor de Mensagens SAP](https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)

* Baixe e instale a versão mais recente do [gateway de dados local](https://www.microsoft.com/download/details.aspx?id=53127) em qualquer computador local. Antes de continuar, verifique se você configurou seu gateway no portal do Azure. O gateway ajuda a com segurança dados de local de acesso e recursos. Para obter mais informações, consulte [Instalar o gateway de dados local para Aplicativos Lógicos do Azure](../logic-apps/logic-apps-gateway-install.md).

* Se você estiver usando SNC com Single Sign-On (SSO), verifique se que o gateway está em execução como um usuário que é mapeado em relação ao usuário do SAP. Para alterar a conta padrão, selecione **alterar conta**e insira as credenciais do usuário.

  ![Alterar a conta de gateway](./media/logic-apps-using-sap-connector/gateway-account.png)

* Se você estiver habilitando SNC com um produto de segurança externa, copie os arquivos no mesmo computador em que o gateway está instalado ou biblioteca SNC. Alguns exemplos de produtos SNC [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos, NTLM, e assim por diante.

* Baixe e instale a biblioteca de cliente SAP mais recente, que é atualmente [conector do SAP (NCo) 3.0.21.0 para o Microsoft .NET Framework 4.0 e Windows de 64 bits (x64)](https://softwaredownloads.sap.com/file/0020000001865512018), no mesmo computador que o gateway de dados local. Instale esta versão ou uma posterior por estes motivos:

  * Versões anteriores do NCo SAP podem se tornar deadlock quando mais de uma mensagem do IDoc é enviada ao mesmo tempo. Essa condição bloqueia todas as mensagens posteriores enviadas para o destino da SAP, fazendo com que as mensagens atinjam o tempo limite.

  * O gateway de dados local é executado apenas em sistemas de 64 bits. Caso contrário, você obtém um erro de “imagem incorreta” porque o serviço de host do gateway de dados não oferece suporte a assemblies de 32 bits.

  * O serviço de host do gateway de dados e o adaptador SAP da Microsoft usam o .NET Framework 4.5. O NCo SAP para o .NET Framework 4.0 funciona com processos que usam o tempo de execução do .NET 4.0 para 4.7.1. O NCo SAP para o .NET Framework 2.0 funciona com processos que usam o tempo de execução do .NET 2.0 para 3.5 e não funciona com o gateway de dados no local mais recente.

* Conteúdo de mensagem que você pode enviar para o servidor SAP, como um arquivo de exemplo IDoc. Este conteúdo deve estar no formato XML e incluir o namespace para a ação SAP que você deseja usar.

<a name="add-trigger"></a>

## <a name="send-to-sap"></a>Enviar para o SAP

Este exemplo usa um aplicativo lógico que pode ser disparado com uma solicitação HTTP. O aplicativo lógico envia um documento intermediário (IDoc) para um servidor SAP e retorna uma resposta ao solicitante que chamou o aplicativo lógico. 

### <a name="add-http-request-trigger"></a>Adicionar gatilho de solicitação HTTP

Nos Aplicativos Lógicos do Azure, cada aplicativo lógico deve começar com um [gatilho](../logic-apps/logic-apps-overview.md#logic-app-concepts), que é disparado quando um evento específico ocorre ou quando uma condição específica é atendida. Cada vez que o gatilho é acionado, o mecanismo de Aplicativos Lógicos cria uma instância de aplicativo lógico e inicia a execução do fluxo de trabalho do aplicativo.

Neste exemplo, você criará um aplicativo lógico com um ponto de extremidade no Azure para seja possível enviar *solicitações HTTP POST* para o aplicativo lógico. Quando seu aplicativo lógico recebe essas solicitações HTTP, o gatilho é acionado e executa a próxima etapa no fluxo de trabalho.

1. No [portal do Azure](https://portal.azure.com), crie um aplicativo lógico em branco, que abre o Designer do Aplicativo Lógico.

1. Na caixa de pesquisa, digite “solicitação http” como filtro. Na lista de gatilhos, selecione este gatilho: **Quando uma solicitação HTTP é recebida**

   ![Adicionar gatilho de solicitação HTTP](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Agora salve seu aplicativo lógico para poder gerar uma URL de ponto de extremidade para seu aplicativo lógico. Clique em **Salvar** na barra de ferramentas do designer.

   O ponto de extremidade de URL agora aparece em seu gatilho, por exemplo:

   ![Gerar a URL do ponto de extremidade](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-sap-action"></a>Adicionar ação SAP

Em Aplicativos Lógicos do Azure, uma [ação](../logic-apps/logic-apps-overview.md#logic-app-concepts) é uma etapa do fluxo de trabalho que segue um gatilho ou outra ação. Caso ainda não tenha adicionado um gatilho ao seu aplicativo lógico e deseje acompanhar esse exemplo [adicione o gatilho descrito nesta seção](#add-trigger).

1. No Designer do aplicativo lógico, no gatilho, escolha **nova etapa**.

   ![Escolha "Nova etapa"](./media/logic-apps-using-sap-connector/add-action.png)

1. Na caixa de pesquisa, insira "sap" como seu filtro. Na lista de ações, selecione esta ação: **Enviar mensagem ao SAP**
  
   ![Selecionar ação de envio do SAP](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Como alternativa, em vez de pesquisar, escolha a guia **Enterprise** e selecione a ação do SAP.

   ![Selecionar ação de envio do SAP na guia Enterprise](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

1. Se forem solicitados os detalhes da conexão, crie sua conexão SAP agora. Caso contrário, se a conexão já existir, continue a próxima etapa para que possa configurar a ação SAP.

   **Criar conexão SAP local**

   1. Forneça as informações de conexão para seu servidor SAP. Para a propriedade **Gateway de Dados**, selecione o gateway de dados que você criou no portal do Azure para a instalação do gateway.

      Se a propriedade **Tipo de Logon** estiver definida como **Servidor de Aplicativos**, estas propriedades, que geralmente aparecem como opcionais, serão obrigatórias:

      ![Criar conexão do servidor de aplicativos SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Se a propriedade **Tipo de Logon** estiver definida como **Grupo**, estas propriedades, que geralmente aparecem como opcionais, serão obrigatórias:

      ![Criar conexão do servidor de mensagens SAP](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Por padrão, a tipagem forte é usado para verificar se há valores inválidos, executando a validação de XML em relação ao esquema. Esse comportamento pode ajudá-lo a detectar problemas anteriormente. O **segura digitando** opção está disponível para compatibilidade com versões anteriores e só verifica o comprimento da cadeia de caracteres. Saiba mais sobre o [ **segura digitando** opção](#safe-typing).

   1. Quando terminar, escolha **Criar**.

      Os Aplicativos Lógicos configuram e testam a conexão, garantindo seu funcionamento correto.

1. Agora localize e selecione uma ação do servidor SAP.

   1. No **ação SAP** , escolha o ícone de pasta. Na lista de arquivos, localize e selecione a mensagem do SAP que você deseja usar. Para navegar pela lista, use as setas.

      Este exemplo seleciona um IDoc com **pedidos** tipo.

      ![Localizar e selecionar a ação de IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Se não encontrar a ação desejada, é possível inserir um caminho manualmente, por exemplo:

      ![Fornecer manualmente o caminho para a ação de IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > Forneça o valor da Ação do SAP por meio do editor de expressão. Dessa forma, é possível usar a mesma ação para diferentes tipos de mensagem.

      Para obter mais informações sobre operações IDoc, consulte [Message schemas for IDOC operations](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations) (Esquemas de mensagens para operações IDOC).

   1. Clique dentro da caixa **Mensagem de entrada** para que seja exibida a lista de conteúdo dinâmica. Nessa lista, em **Quando uma solicitação HTTP é recebida**, selecione o campo **Corpo**.

      Esta etapa inclui o conteúdo do corpo do seu gatilho de solicitação HTTP e envia essa saída para o servidor SAP.

      ![Selecionar o campo “Corpo”](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Ao concluir, sua ação SAP será semelhante a este exemplo:

      ![Concluir ação SAP](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

1. Salve seu aplicativo lógico. Clique em **Salvar** na barra de ferramentas do designer.

<a name="add-response"></a>

### <a name="add-http-response-action"></a>Adicionar ação de resposta HTTP

Agora, adicione uma ação de resposta ao fluxo de trabalho do aplicativo lógico e inclua a saída de uma ação SAP. Dessa forma, seu aplicativo lógico retorna os resultados do seu servidor SAP para o solicitante original.

1. No Designer do aplicativo lógico, na ação SAP, escolha **nova etapa**.

1. Na caixa de pesquisa, insira “resposta” como filtro. Na lista de ações, selecione esta ação: **Resposta**

1. Clique dentro da caixa **Corpo** para que seja exibida a lista de conteúdo dinâmica. Nessa lista, sob **enviar mensagem ao SAP**, selecione o **corpo** campo.

   ![Concluir ação SAP](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

1. Salve seu aplicativo lógico.

### <a name="test-your-logic-app"></a>Como testar o seu aplicativo lógico

1. Se seu aplicativo lógico já não estiver habilitado, no menu do aplicativo lógico, escolha **Visão geral**. Na barra de ferramentas, escolha **Habilitar**.

1. Na barra de ferramentas do Designer do Aplicativo Lógico, escolha **Executar**. Essa etapa inicia manualmente o aplicativo lógico.

1. Dispare o aplicativo lógico, enviando uma solicitação HTTP POST para a URL em seu gatilho de solicitação HTTP e inclua o conteúdo da mensagem na solicitação. Para enviar a solicitação, você pode usar uma ferramenta como [Postman](https://www.getpostman.com/apps).

   Neste artigo, a solicitação envia um arquivo IDoc, que deve estar no formato XML e incluir o namespace para a ação SAP que você está usando, por exemplo:

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

1. Depois de enviar a solicitação HTTP, aguarde a resposta do seu aplicativo lógico.

   > [!NOTE]
   > O aplicativo lógico pode atingir o tempo limite se todas as etapas necessárias para a resposta não forem concluídas dentro do [tempo limite da solicitação](./logic-apps-limits-and-config.md). Se essa condição ocorrer, as solicitações podem ser bloqueadas. Para auxílio para diagnosticar problemas, saiba como você pode [verificar e monitorar seus aplicativos lógicos](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Parabéns, você criou um aplicativo lógico que pode se comunicar com o servidor SAP. Agora que você configurou uma conexão SAP para seu aplicativo lógico, é possível explorar outras ações SAP disponíveis, como BAPI e RFC.

## <a name="receive-from-sap"></a>Receber do SAP

Este exemplo usa um aplicativo lógico que dispara ao receber uma mensagem de um sistema SAP.

### <a name="add-sap-trigger"></a>Adicionar gatilho do SAP

1. No portal do Azure, crie um aplicativo lógico em branco, o que abrirá o Designer do Aplicativo Lógico.

1. Na caixa de pesquisa, insira "sap" como seu filtro. Na lista de gatilhos, selecione este gatilho: **Quando uma mensagem é recebida do SAP**

   ![Adicionar gatilho do SAP](./media/logic-apps-using-sap-connector/add-sap-trigger.png)

   Como alternativa, você pode ir para o **Enterprise** guia e, em seguida, selecione o Gatilho:

   ![Adicionar gatilho SAP da guia Enterprise](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

1. Se forem solicitados os detalhes da conexão, crie sua conexão SAP agora. Caso contrário, se a conexão já existir, continue a próxima etapa para que possa configurar a ação SAP.

   **Criar conexão SAP local**

   1. Forneça as informações de conexão para seu servidor SAP. Para a propriedade **Gateway de Dados**, selecione o gateway de dados que você criou no portal do Azure para a instalação do gateway.

      Se a propriedade **Tipo de Logon** estiver definida como **Servidor de Aplicativos**, estas propriedades, que geralmente aparecem como opcionais, serão obrigatórias:

      ![Criar conexão do servidor de aplicativos SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Se a propriedade **Tipo de Logon** estiver definida como **Grupo**, estas propriedades, que geralmente aparecem como opcionais, serão obrigatórias:

      ![Criar conexão do servidor de mensagens SAP](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Por padrão, a tipagem forte é usado para verificar se há valores inválidos, executando a validação de XML em relação ao esquema. Esse comportamento pode ajudá-lo a detectar problemas anteriormente. O **segura digitando** opção está disponível para compatibilidade com versões anteriores e só verifica o comprimento da cadeia de caracteres. Saiba mais sobre o [ **segura digitando** opção](#safe-typing).

1. Forneça os parâmetros necessários com base na configuração do sistema SAP.

   Também é possível oferecer uma ou mais ações do SAP. Esta lista de ações especifica as mensagens que o gatilho recebe do servidor SAP por meio do gateway de dados. Uma lista vazia especifica que o gatilho recebe todas as mensagens. Se a lista tiver mais de uma mensagem, o gatilho receberá apenas as mensagens especificadas na lista. Quaisquer outras mensagens enviadas do seu servidor SAP são rejeitadas pelo gateway.

   É possível selecionar uma ação do SAP no seletor de arquivos:

   ![Adicionar ação do SAP](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Ou é possível especificar manualmente uma ação:

   ![Inserir manualmente a ação do SAP](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png) 

   Veja um exemplo que mostra como a ação é exibida quando você configura o gatilho para receber mais de uma mensagem.

   ![Exemplo de gatilho](media/logic-apps-using-sap-connector/example-trigger.png)  

   Para obter mais informações sobre a ação do SAP, consulte [Message schemas for IDOC operations](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations) (Esquemas de mensagem para operações IDOC)

1. Agora salve seu aplicativo lógico para poder começar a receber mensagens do seu sistema SAP.
Clique em **Salvar** na barra de ferramentas do designer.

Agora seu aplicativo lógico está pronto para receber mensagens do seu sistema SAP.

> [!NOTE]
> O gatilho do SAP não é um gatilho de sondagem, mas um gatilho baseado em webhook. O gatilho é chamado do gateway somente quando há uma mensagem; portanto, não é necessária nenhuma sondagem.

### <a name="test-your-logic-app"></a>Como testar o seu aplicativo lógico

1. Para disparar seu aplicativo lógico, envie uma mensagem do sistema SAP.

1. No menu do aplicativo lógico, escolha **Visão geral** e analise o **Histórico de execuções** para ver se há novas execuções em seu aplicativo lógico.

1. Abra a execução mais recente, que mostra a mensagem enviada do seu sistema SAP na seção de saídas de gatilho.

## <a name="generate-schemas-for-artifacts-in-sap"></a>Gerar esquemas para artefatos no SAP

Este exemplo usa um aplicativo lógico que pode ser disparado com uma solicitação HTTP. A ação do SAP envia uma solicitação a um sistema SAP para gerar os esquemas para o IDoc (Documento Intermediário) e o BAPI especificados. Os esquemas retornados na resposta são carregados para uma Conta de integração usando o conector do Azure Resource Manager.

### <a name="add-http-request-trigger"></a>Adicionar gatilho de solicitação HTTP

1. No portal do Azure, crie um aplicativo lógico em branco, o que abrirá o Designer do Aplicativo Lógico.

1. Na caixa de pesquisa, digite “solicitação http” como filtro. Na lista de gatilhos, selecione este gatilho: **Quando uma solicitação HTTP é recebida**

   ![Adicionar gatilho de solicitação HTTP](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Agora salve seu aplicativo lógico para poder gerar uma URL de ponto de extremidade para seu aplicativo lógico.
Clique em **Salvar** na barra de ferramentas do designer.

   O ponto de extremidade de URL agora aparece em seu gatilho, por exemplo:

   ![Gerar a URL do ponto de extremidade](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-sap-action-to-generate-schemas"></a>Adicionar ação do SAP para gerar esquemas

1. No Designer do aplicativo lógico, no gatilho, escolha **nova etapa**.

   ![Escolha "Nova etapa"](./media/logic-apps-using-sap-connector/add-action.png)

1. Na caixa de pesquisa, insira "sap" como seu filtro. Na lista de ações, selecione esta ação: **Gerar esquemas**
  
   ![Selecionar ação de envio do SAP](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Como alternativa, também é possível escolher a guia **Enterprise** e selecionar a ação do SAP.

   ![Selecionar ação de envio do SAP na guia Enterprise](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

1. Se forem solicitados os detalhes da conexão, crie sua conexão SAP agora. Caso contrário, se a conexão já existir, continue a próxima etapa para que possa configurar a ação SAP.

   **Criar conexão SAP local**

   1. Forneça as informações de conexão para seu servidor SAP. Para a propriedade **Gateway de Dados**, selecione o gateway de dados que você criou no portal do Azure para a instalação do gateway.

      Se a propriedade **Tipo de Logon** estiver definida como **Servidor de Aplicativos**, estas propriedades, que geralmente aparecem como opcionais, serão obrigatórias:

      ![Criar conexão do servidor de aplicativos SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Se a propriedade **Tipo de Logon** estiver definida como **Grupo**, estas propriedades, que geralmente aparecem como opcionais, serão obrigatórias:

      ![Criar conexão do servidor de mensagens SAP](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Por padrão, a tipagem forte é usado para verificar se há valores inválidos, executando a validação de XML em relação ao esquema. Esse comportamento pode ajudá-lo a detectar problemas anteriormente. O **segura digitando** opção está disponível para compatibilidade com versões anteriores e só verifica o comprimento da cadeia de caracteres. Saiba mais sobre o [ **segura digitando** opção](#safe-typing).

   1. Quando terminar, escolha **Criar**. 
   
      Os Aplicativos Lógicos configuram e testam a conexão, garantindo seu funcionamento correto.

1. Forneça o caminho para o artefato para o qual você deseja gerar o esquema.

   É possível selecionar a ação do SAP no seletor de arquivos:

   ![Adicionar ação do SAP](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Ou é possível inserir manualmente a ação:

   ![Inserir manualmente a ação do SAP](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png)

   Para gerar esquemas para mais de um artefato, forneça os detalhes da ação do SAP para cada artefato, por exemplo:

   ![Selecionar Adicionar novo item](media/logic-apps-using-sap-connector/schema-generator-array-pick.png)

   ![Mostrar dois itens](media/logic-apps-using-sap-connector/schema-generator-example.png)

   Para obter mais informações sobre a ação do SAP, consulte [Message schemas for IDOC operations](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations) (Esquemas de mensagem para operações IDOC).

1. Salve seu aplicativo lógico. Clique em **Salvar** na barra de ferramentas do designer.

### <a name="test-your-logic-app"></a>Como testar o seu aplicativo lógico

1. Na barra de ferramentas do designer, escolha **Executar** para disparar uma execução do seu aplicativo lógico.

1. Abra a execução e verifique as saídas para o **gerar esquemas** ação.

   As saídas mostram os esquemas gerados para a lista de mensagens especificada.

### <a name="upload-schemas-to-integration-account"></a>Carregar esquemas para a conta de integração

Opcionalmente, é possível baixar ou armazenar os esquemas gerados em repositórios, como um blob, armazenamento ou conta de integração. As contas de integração oferecem uma experiência de primeira classe com outras ações de XML; portanto, este exemplo mostra como carregar esquemas para uma conta de integração para o mesmo aplicativo lógico usando o conector do Azure Resource Manager.

1. No Designer de Aplicativos Lógicos, no gatilho, escolha **Nova etapa**.

1. Na caixa de pesquisa, insira "Gerenciador de recursos" como filtro. Selecione esta ação: **Criar ou atualizar um recurso**

   ![Selecionar ação do Azure Resource Manager](media/logic-apps-using-sap-connector/select-azure-resource-manager-action.png)

1. Insira os detalhes da ação, incluindo sua assinatura do Azure, o grupo de recursos do Azure e a conta de integração. Para adicionar os tokens SAP para os campos, clique dentro de caixas para esses campos e selecione na lista de conteúdo dinâmico é exibido.

   1. Abra o **adicionar novo parâmetro** relacionar e selecionar a **local** e **propriedades** campos.

   1. Forneça detalhes para esses novos campos, conforme mostrado neste exemplo.

      ![Insira os detalhes da ação do Azure Resource Manager](media/logic-apps-using-sap-connector/azure-resource-manager-action.png)

   A ação **Gerar esquemas** do SAP gera esquemas como uma coleção, portanto o designer adiciona automaticamente um loop **For each** à ação. Veja um exemplo que mostra como essa ação é exibida:

   ![Ação do Azure Resource Manager com o loop "for each"](media/logic-apps-using-sap-connector/azure-resource-manager-action-foreach.png)  

   > [!NOTE]
   > Os esquemas usam o formato codificado para base64. Para carregar os esquemas para uma conta de integração, eles devem ser decodificados usando a função `base64ToString()`. Veja um exemplo que mostra o código do elemento `"properties"`:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

1. Salve seu aplicativo lógico. Clique em **Salvar** na barra de ferramentas do designer.

### <a name="test-your-logic-app"></a>Como testar o seu aplicativo lógico

1. Na barra de ferramentas do designer, escolha **Executar** para disparar manualmente seu aplicativo lógico.

1. Após o êxito em uma execute, vá para a conta de integração e verifique se os esquemas gerados existem.

## <a name="enable-secure-network-communications-snc"></a>Habilitar as comunicações de rede segura (SNC)

Antes de começar, verifique se você cumpriu listadas anteriormente [pré-requisitos](#pre-reqs):

* O gateway de dados local está instalado em um computador que está na mesma rede que o sistema SAP.

* Para o logon único, o gateway está em execução como um usuário que é mapeado para o usuário do SAP.

* Biblioteca SNC que fornece as funções de segurança adicional foi instalada no mesmo computador que o gateway de dados. Alguns dos exemplos deles incluem [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos, NTLM, e assim por diante.

Para habilitar o SNC para suas solicitações de ou para o sistema SAP, selecione a **Use SNC** caixa de seleção na conexão de SAP e fornecer estas propriedades:

   ![Configurar o SAP SNC na conexão](media/logic-apps-using-sap-connector/configure-sapsnc.png)

   | Propriedade | DESCRIÇÃO |
   |----------| ------------|
   | **Biblioteca SNC** | O nome da biblioteca SNC ou caminho absoluto ou caminho relativo ao local de instalação NCo. Por exemplo, `sapsnc.dll` ou `.\security\sapsnc.dll` ou `c:\security\sapsnc.dll` |
   | **O SNC SSO** | Ao se conectar por meio de SNC, a identidade SNC normalmente é usada para autenticar o chamador. Outra opção é substituir de modo que as informações de usuário e senha podem ser usadas para autenticar o chamador, mas a linha ainda está criptografada. |
   | **SNC meu nome** | Na maioria dos casos, essa propriedade pode ser omitida. Geralmente, a solução SNC instalada sabe seu próprio nome SNC. Apenas para as soluções que dão suporte a "identidades múltiplas", você talvez precise especificar a identidade a ser usado para este destino específico ou servidor. |
   | **Nome do parceiro SNC** | O nome para o back-end SNC |
   | **O SNC qualidade de proteção** | Qualidade de serviço a ser usado para comunicação de SNC deste destino/servidor específico. Valor padrão é definido pelo sistema back-end. O valor máximo é definido pelo produto de segurança usado para SNC. |
   |||

   > [!NOTE]
   > Variáveis de ambiente SNC_LIB e SNC_LIB_64 não devem ser definidas no computador em que você tem o gateway de dados e a biblioteca SNC. Se definido, eles seriam têm precedência sobre o valor de biblioteca SNC passado por meio do conector.

<a name="safe-typing"></a>

## <a name="safe-typing"></a>Digitar seguro

Por padrão, quando você cria sua conexão de SAP, tipagem forte é usado para verificar se há valores inválidos, executando a validação de XML em relação ao esquema. Esse comportamento pode ajudá-lo a detectar problemas anteriormente. O **segura digitando** opção está disponível para compatibilidade com versões anteriores e só verifica o comprimento da cadeia de caracteres. Se você escolher **segura digitando**, o tipo DATS e o tipo de TIMS no SAP são tratados como cadeias de caracteres em vez de como seus equivalentes XML, `xs:date` e `xs:time` onde `xmlns:xs="http://www.w3.org/2001/XMLSchema"`. Digitar seguro afeta o comportamento para todos os geração de esquema, a mensagem de envio para a carga "foi enviado" e "recebido" resposta, tanto o gatilho. 

Quando a tipagem forte é usado (**segura digitando** não habilitada), o esquema mapeia os tipos DATS e TIMS para tipos mais simples de XML:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true" type="xs:date"/>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true" type="xs:time"/>
```

Ao enviar mensagens usando a tipagem forte, a resposta DATS e TIMS está em conformidade com o formato do tipo XML correspondente:

```xml
<DATE>9999-12-31</DATE>
<TIME>23:59:59</TIME>
```

Quando **segura digitando** é habilitado, o esquema mapeia os DATS e tipos TIMS em XML da cadeia de caracteres campos com restrições de comprimento, por exemplo:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="8" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="6" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

Ao enviar mensagens com **segura digitando** habilitado, a resposta DATS e TIMS se parece com esta:

```xml
<DATE>99991231</DATE>
<TIME>235959</TIME>
```


## <a name="known-issues-and-limitations"></a>Problemas e limitações conhecidos

Veja os problemas e limitações atualmente conhecidos para o conector do SAP:

* Apenas uma única chamada ou mensagem Enviar para SAP funciona com o tRFC. Não há suporte para o padrão de confirmação BAPI (Interface de programação de aplicativo de negócios), como realizar várias chamadas de tRFC na mesma sessão.

* O gatilho do SAP não dá suporte ao recebimento de IDocs em lote do SAP. Essa ação poderia resultar em falha na conexão RFC entre seu sistema SAP e o gateway de dados.

* O gatilho do SAP não dá suporte a clusters de gateway de dados. Em alguns casos de failover, o nó do gateway de dados que se comunica com o sistema SAP pode ser diferente do nó ativo, resultando em um comportamento inesperado. Para cenários de Envio, há suporte para clusters de gateway de dados.

* No momento, o conector do SAP não é compatível com cadeia de caracteres do roteador do SAP. O gateway de dados local deve existir na mesma LAN que o sistema SAP que você deseja conectar.

## <a name="connector-reference"></a>Referência de conector

Para obter detalhes técnicos sobre limites, ações e gatilhos, que são descritos por OpenAPI do conector (anteriormente conhecido como Swagger) descrição, examine os [página de referência do conector](/connectors/sap/).

## <a name="next-steps"></a>Próximas etapas

* [Conectar-se a sistemas locais](../logic-apps/logic-apps-gateway-connection.md) por meio de aplicativos lógicos
* Saiba como validar, transformar e outras operação de mensagem com o [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Saiba mais sobre outros [conectores de Aplicativos Lógicos](../connectors/apis-list.md)

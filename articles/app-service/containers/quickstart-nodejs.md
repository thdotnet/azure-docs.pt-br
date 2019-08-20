---
title: Criar um aplicativo Web Node.js
description: Implantação do Node.js nos Serviços de Aplicativos do Azure
author: KarlErickson
ms.author: karler
ms.date: 07/18/2019
ms.topic: quickstart
ms.service: app-service
ms.devlang: javascript
ms.openlocfilehash: ced2977509f16f8dab2abe5546e19b7e05fb2a3d
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68975784"
---
# <a name="create-a-nodejs-app-in-azure"></a>Criar um aplicativo Node.js no Azure

O Serviço de Aplicativo do Azure fornece um serviço de hospedagem Web altamente escalonável e com aplicação automática de patch. Este início rápido mostra como implantar um aplicativo Node.js no Serviço de Aplicativo do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Se você não tiver uma conta do Azure, [inscreva-se hoje mesmo](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension) para obter uma conta gratuita com US$ 200 em créditos do Azure para experimentar uma combinação de serviços.

É preciso que o [Visual Studio Code](https://code.visualstudio.com/) esteja instalado junto com [Node.js e npm](https://nodejs.org/en/download), o gerenciador de pacotes do Node.js.

Será necessário instalar a [extensão do Serviço de Aplicativo do Azure](vscode:extension/ms-azuretools.vscode-azureappservice), que você pode usar para criar, gerenciar e implantar Aplicativos Web Linux na PaaS (Plataforma como Serviço) do Azure.

### <a name="sign-in"></a>Entrar

Depois que a extensão for instalada, faça logon em sua conta do Azure. Na Barra de Atividades, clique no logo do Azure para mostrar o explorador **SERVIÇO DE APLICATIVO DO AZURE**. Clique em **Entrar no Azure...** e siga as instruções.

![entrar no Azure](./media/quickstart-nodejs/sign-in.png)

### <a name="troubleshooting"></a>solução de problemas

Se você vir o erro **"Não é possível encontrar a assinatura com o nome [ID da assinatura]"** , talvez seja porque você está atrás de um proxy e não consegue acessar a API do Azure. Configure as variáveis de ambiente `HTTP_PROXY` e `HTTPS_PROXY` com suas informações de proxy no seu terminal usando o `export`.

```sh
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Se a definição das variáveis de ambiente não corrigir o problema, contate-nos clicando no botão **Encontrei um problema** abaixo.

### <a name="prerequisite-check"></a>Verificação de pré-requisitos

Antes de continuar, verifique se você tem todos os pré-requisitos instalados e configurados.

No VS Code, você deverá ver o endereço de email do Azure na Barra de Status e sua assinatura no explorador **SERVIÇO DE APLICATIVO DO AZURE**.

> [!div class="nextstepaction"]
> [Encontrei um problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=getting-started)

## <a name="create-your-nodejs-application"></a>Crie seu aplicativo Node.js

Em seguida, crie um aplicativo Node.js que possa ser implantado na nuvem. Este início rápido usa um gerador de aplicativos para realizar scaffold rapidamente do aplicativo de um terminal.

> [!TIP]
> Se você já tiver concluído o [tutorial do Node.js](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial), poderá pular para [Implantar o site](#deploy-the-website).

### <a name="install-the-express-generator"></a>Instalar o gerador Express

[Express](https://www.expressjs.com) é uma estrutura popular para criar e executar aplicativos Node.js. Você pode realizar scaffold (criar) de um novo aplicativo Express usando a ferramenta [Gerador Express](https://expressjs.com/en/starter/generator.html). O Gerador Express é enviado como um módulo npm e é instalado usando a ferramenta de linha de comando npm `npm`.

```bash
npm install -g express-generator
```

O comutador `-g` instala o Gerador Express globalmente em seu computador para que você possa executá-lo de qualquer lugar.

### <a name="scaffold-a-new-application"></a>Realizar scaffold de um novo aplicativo

Em seguida, realize scaffold de um novo aplicativo Express chamado `myExpressApp` executando:

```bash
express myExpressApp --view pug --git
```

Os parâmetros `--view pug --git` instruem o gerador a usar o mecanismo de modelo [pug](https://pugjs.org/api/getting-started.html) (anteriormente conhecido como `jade`) e a criar um arquivo `.gitignore`.

Para instalar todas as dependências do aplicativo, vá para a nova pasta e execute `npm install`.

```bash
cd myExpressApp
npm install
```

### <a name="run-the-application"></a>Executar o aplicativo

Em seguida, verifique se o aplicativo é executado. No terminal, inicie o aplicativo usando o comando `npm start` para iniciar o servidor.

```bash
npm start
```

Agora, abra o navegador e navegue até [http://localhost:3000](http://localhost:3000), em que você verá algo assim:

![Executar aplicativo Express](./media/quickstart-nodejs/express.png)

> [!div class="nextstepaction"]
> [Encontrei um problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-the-website"></a>Implantar o site

Nesta seção, você implantará seu site do Node.js usando o VS Code e a extensão do Serviço de Aplicativo do Azure. Este guia de início rápido usa o modelo de implantação mais básico em que seu aplicativo é compactado e implantado em um aplicativo Web do Azure no Linux.

### <a name="deploy-using-azure-app-service"></a>Implantar usando o Serviço de Aplicativo do Azure

Primeiro, abra a pasta do aplicativo no VS Code.

```bash
code .
```

No explorador **SERVIÇO DE APLICATIVO DO AZURE**, clique no ícone de seta azul para cima para implantar seu aplicativo no Azure.

![Implantar no aplicativo Web](./media/quickstart-nodejs/deploy.png)

> [!TIP]
> Você também pode implantar na **Paleta de Comandos** (CTRL + SHIFT + P) digitando “implantar no aplicativo Web” e executando o comando **Serviço de Aplicativo do Azure: Implantar no comando** do aplicativo Web.

1. Escolha o diretório que está aberto no momento, `myExpressApp`.

2. Escolha **Criar Aplicativo Web**.

3. Digite um nome globalmente exclusivo para seu Aplicativo Web e pressione ENTER. Os caracteres válidos para um nome de aplicativo são “a-z”, “0-9” e “-”.

4. Escolha a **versão do Node.js**; LTS é recomendado.

    O canal de notificação mostra os recursos do Azure que estão sendo criados para seu aplicativo.

Clique em **Sim** quando solicitado a atualizar sua configuração para ser executar `npm install` no servidor de destino. Seu aplicativo será implantado.

![Implantação configurada](./media/quickstart-nodejs/server-build.png)

Quando a implantação é iniciada, você deve atualizar o workspace para que futuras implantações direcionem automaticamente o mesmo Aplicativo Web do Serviço de Aplicativo. Escolha **Sim** para que suas alterações sejam implantadas no aplicativo correto.

![Implantação configurada](./media/quickstart-nodejs/save-configuration.png)

> [!TIP]
> Verifique se seu aplicativo está escutando na porta fornecida pela variável de ambiente PORT: `process.env.PORT`.

### <a name="browse-the-website"></a>Procurar no site

Quando a implantação for concluída, clique **Procurar no Site** no prompt para exibir seu site implantado recentemente.

### <a name="troubleshooting"></a>solução de problemas

Se você vir o erro **"Você não tem permissão para exibir este diretório ou página."** , o aplicativo provavelmente terá falhado em iniciar corretamente. Passe para a próxima seção e exiba a saída do log para localizar e corrigir o erro. Se você não conseguir corrigi-lo, contate-nos clicando no botão **Encontrei um problema** abaixo. Temos o prazer de ajudar!

> [!div class="nextstepaction"]
> [Encontrei um problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="updating-the-website"></a>Atualizar o site

Você pode implantar alterações nesse aplicativo usando o mesmo processo e escolhendo o aplicativo existente em vez de criar um.

## <a name="viewing-logs"></a>Exibir logs

Nesta seção, você aprenderá a exibir (ou “seguir”) os logs do site em execução. Chamadas a `console.log` no site são exibidas na janela de saída no Visual Studio Code.

Localize o aplicativo no explorador **SERVIÇO DE APLICATIVO DO AZURE**, clique com o botão direito do mouse no aplicativo e escolha **Exibir logs de streaming**.

Quando solicitado, escolha habilitar o registro em log e reinicie o aplicativo. Depois que o aplicativo for reiniciado, a janela de saída do VS Code será aberta com uma conexão com o fluxo de log.

![Exibir logs de streaming](./media/quickstart-nodejs/view-logs.png)

![Habilitar registro em log e reiniciar](./media/quickstart-nodejs/enable-restart.png)

Após alguns segundos, vocês verá uma mensagem indicando que você está conectado ao serviço de streaming de log.

```bash
Connecting to log-streaming service...
2017-12-21 17:33:51.428 INFO  - Container practical-mustache_2 for site practical-mustache initialized successfully.
2017-12-21 17:33:56.500 INFO  - Container logs
```

Atualize a página algumas vezes no navegador para ver a saída do log.

```bash
2017-12-21 17:35:17.774 INFO  - Container logs
2017-12-21T17:35:14.955412230Z GET / 304 141.798 ms - -
2017-12-21T17:35:15.248930479Z GET /stylesheets/style.css 304 3.180 ms - -
2017-12-21T17:35:15.378623115Z GET /favicon.ico 404 53.839 ms - 995
```

> [!div class="nextstepaction"]
> [Encontrei um problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=tailing-logs)

## <a name="next-steps"></a>Próximas etapas

Parabéns, você concluiu com êxito este início rápido!

Em seguida, confira as outras extensões do Azure.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Funções do Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Ferramentas do Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Ferramentas da CLI do Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Ferramentas do Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

Ou obtenha todas elas instalando o pacote de extensão [Pacote de Nós para Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack).

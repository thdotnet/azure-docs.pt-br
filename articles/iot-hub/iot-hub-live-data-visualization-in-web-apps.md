---
title: Visualização de dados em tempo real dos dados de sensor do hub IoT do Azure em um aplicativo web | Microsoft Docs
description: Use um aplicativo web para visualizar dados de temperatura e umidade que são coletados de um sensor e enviados ao hub Iot.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 05/31/2019
ms.author: robinsh
ms.openlocfilehash: 22b15a95e529d4f09560e9b7e59d9f78f70651bc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66476017"
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-in-a-web-application"></a>Visualizar dados de sensor em tempo real do hub IoT do Azure em um aplicativo web

![Diagrama de ponta a ponta](./media/iot-hub-live-data-visualization-in-web-apps/1_iot-hub-end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>O que você aprenderá

Neste tutorial, você aprenderá como visualizar dados de sensor em tempo real que o hub IoT recebe com um aplicativo web do Node. js em execução no computador local. Depois de executar o aplicativo web localmente, você pode, opcionalmente, siga etapas para hospedar o aplicativo web no serviço de aplicativo do Azure. Se você quiser tentar visualizar os dados em seu Hub IoT usando o Power BI, consulte [Usar o Power BI para visualizar dados de sensor em tempo real do Hub IoT do Azure](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>O que fazer

* Adicionar um grupo de consumidores ao hub IoT que usará o aplicativo web para ler dados de sensor
* Baixe o código do aplicativo web do GitHub
* Examinar o código do aplicativo web
* Configurar variáveis de ambiente para reter o IoT Hub artefatos necessários pelo seu aplicativo web
* Executar o aplicativo web no computador de desenvolvimento
* Abra uma página da web para ver os dados de temperatura e umidade em tempo real do hub IoT
* (Opcional) Usar a CLI do Azure para hospedar seu aplicativo web no serviço de aplicativo do Azure

## <a name="what-you-need"></a>O que você precisa

* Conclua o [simulador online Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md) tutorial ou um dos tutoriais dispositivo; por exemplo, [Raspberry Pi com Node. js](iot-hub-raspberry-pi-kit-node-get-started.md). Estes abrangem os seguintes requisitos:

  * Uma assinatura ativa do Azure
  * Um Hub IoT em sua assinatura
  * Um aplicativo cliente que envia mensagens para o seu Hub IoT

* [Baixar Git](https://www.git-scm.com/downloads)

* As etapas neste artigo pressupõem que um computador de desenvolvimento do Windows; No entanto, você pode executar essas etapas facilmente em um sistema Linux no seu shell preferencial.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Execute o comando a seguir para adicionar a Extensão do Microsoft Azure IoT para a CLI do Azure à instância do Cloud Shell. A Extensão de IoT adiciona comandos específicos do Hub IoT, do IoT Edge e do DPS (Serviço de Provisionamento de Dispositivos IoT) à CLI do Azure.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

## <a name="add-a-consumer-group-to-your-iot-hub"></a>Adicionar um grupo de consumidores ao hub IoT

[Grupos de consumidores](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers) fornecem exibições independentes no fluxo de eventos que permitem que aplicativos e serviços do Azure para consumir independentemente de dados do mesmo ponto de extremidade do Hub de eventos. Nesta seção, você pode adicionar um grupo de consumidores ao ponto de extremidade internos do seu hub IoT que usará o aplicativo web para ler dados do.

Execute o seguinte comando para adicionar um grupo de consumidores ao ponto de extremidade interno de seu hub IoT:

```azurecli-interactive
az iot hub consumer-group create --hub-name YourIoTHubName --name YourConsumerGroupName
```

Observe o nome que você escolher, você precisará dela posteriormente no tutorial.

## <a name="get-a-service-connection-string-for-your-iot-hub"></a>Obter uma cadeia de caracteres de conexão de serviço para o hub IoT

Os hubs IoT são criados com várias políticas de acesso padrão. Essa política é o **serviço** política, que fornece permissões suficientes para um serviço ler e gravar pontos de extremidade do hub IoT. Execute o seguinte comando para obter uma cadeia de caracteres de conexão do hub IoT que segue a política de serviço:

```azurecli-interactive
az iot hub show-connection-string --hub-name YourIotHub --policy-name service
```

A cadeia de caracteres de conexão deve ser semelhante ao seguinte:

```javascript
"HostName={YourIotHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"
```

Anote a cadeia de caracteres de conexão de serviço, você precisará dela posteriormente no tutorial.

## <a name="download-the-web-app-from-github"></a>Baixe o aplicativo web do GitHub

Abra uma janela de comando e insira os seguintes comandos para baixar o exemplo do GitHub e altere para o diretório de exemplo:

```cmd
git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
cd web-apps-node-iot-hub-data-visualization
```

## <a name="examine-the-web-app-code"></a>Examinar o código do aplicativo web

No diretório web-apps-node-iot-hub-data-visualization, abra o aplicativo web em seu editor favorito. O exemplo a seguir mostra a estrutura do arquivo exibida no VS Code:

![Estrutura de arquivos do aplicativo Web](./media/iot-hub-live-data-visualization-in-web-apps/web-app-files.png)

Reserve um tempo para examinar os arquivos a seguir:

* **Server. js** é um script do lado do serviço que inicializa o soquete da web e a classe de wrapper do Hub de eventos. Ele fornece um retorno de chamada para a classe de wrapper do Hub de eventos que a classe usa para transmitir mensagens de entrada para o soquete da web.

* **Evento de hub de reader.js** é um script do lado do serviço que se conecta ao ponto de extremidade internos do hub IoT usando o grupo de consumidores e de cadeia de caracteres de conexão especificada. Ele extrai o DeviceId e EnqueuedTimeUtc de metadados nas mensagens de entrada e, em seguida, transmite a mensagem usando o método de retorno de chamada registrado pelo Server. js.

* **Gráfico de dispositivo de js** é um script do lado do cliente que escuta no soquete da web, mantém o controle de cada DeviceId e armazena os últimos 50 pontos de dados de entrada para cada dispositivo. Em seguida, associa os dados do dispositivo selecionado para o objeto de gráfico.

* **Index. HTML** manipula o layout da interface do usuário para a página da web e referencia os scripts necessários para a lógica do lado do cliente.

## <a name="configure-environment-variables-for-the-web-app"></a>Configurar variáveis de ambiente para o aplicativo web

Para ler dados do hub IoT, o aplicativo web precisa de cadeia de caracteres de conexão do hub IoT e o nome do grupo consumidor que ele deve ser lida por meio de. Ele obtém essas cadeias de caracteres do ambiente de processo nas linhas a seguir em Server. js:

```javascript
const iotHubConnectionString = process.env.IotHubConnectionString;
const eventHubConsumerGroup = process.env.EventHubConsumerGroup;
```

Defina as variáveis de ambiente na janela de comando com os seguintes comandos. Substitua os valores de espaço reservado pela cadeia de conexão de serviço para seu hub IoT e o nome do grupo de consumidores que você criou anteriormente. Não colocar entre aspas as cadeias de caracteres.

```cmd
set IotHubConnectionString=YourIoTHubConnectionString
set EventHubConsumerGroup=YourConsumerGroupName
```

## <a name="run-the-web-app"></a>Executar o aplicativo Web

1. Certifique-se de que seu dispositivo está em execução e enviando dados.

2. Na janela de comando, execute as seguintes linhas para baixar e instalar os pacotes referenciados e iniciar o site:

   ```cmd
   npm install
   npm start
   ```

3. Você deverá ver a saída no console que indica que o aplicativo web se conectou com êxito ao hub IoT e está escutando na porta 3000:

   ![Aplicativo Web iniciado no console](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-start.png)

## <a name="open-a-web-page-to-see-data-from-your-iot-hub"></a>Abra uma página da web para ver os dados de seu hub IoT

Abra um navegador para `http://localhost:3000`.

No **selecione um dispositivo** , selecione o dispositivo para ver um gráfico em execução dos últimos 50 pontos de dados de temperatura e umidade enviados pelo dispositivo ao hub IoT.

![Página de aplicativo Web mostrando a umidade e a temperatura em tempo real](./media/iot-hub-live-data-visualization-in-web-apps/web-page-output.png)

Você também verá a saída no console que mostra as mensagens que seu aplicativo web estiver transmitindo para o cliente de navegador:  

![Aplicativo Web de saída no console de difusão](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-broadcast.png)

## <a name="host-the-web-app-in-app-service"></a>Hospedar o aplicativo web no serviço de aplicativo

O [recurso de aplicativos Web do serviço de aplicativo do Azure](https://docs.microsoft.com/azure/app-service/overview) fornece uma plataforma como serviço (PAAS) para hospedar aplicativos web. Aplicativos Web hospedados no serviço de aplicativo do Azure podem se beneficiar poderosos recursos do Azure, como segurança adicional, o balanceamento de carga, e a escalabilidade como assim como o Azure e a parceria DevOps soluções como a implantação contínua, gerenciamento de pacotes e assim por diante. O serviço de aplicativo do Azure dá suporte a aplicativos da web desenvolvidos em várias linguagens populares e implantados na infraestrutura do Windows ou Linux.

Nesta seção, você pode provisiona um aplicativo web no serviço de aplicativo e implantar seu código usando comandos da CLI do Azure. Você pode encontrar detalhes dos comandos usados na [az webapp](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest) documentação. Antes de começar, verifique se você concluiu as etapas a serem [adicionar um grupo de recursos ao seu hub IoT](#add-a-consumer-group-to-your-iot-hub), [obter uma cadeia de caracteres de conexão de serviço para o hub IoT](#get-a-service-connection-string-for-your-iot-hub), e [baixar o aplicativo web do GitHub](#download-the-web-app-from-github).

1. Uma [plano do serviço de aplicativo](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) define um conjunto de recursos de computação para um aplicativo hospedado no serviço de aplicativo para ser executado. Neste tutorial, usamos a camada de desenvolvedor/gratuito para hospedar o aplicativo web. Com a camada gratuita, seu aplicativo web é executado em recursos compartilhados do Windows com outros aplicativos de serviço de aplicativo, incluindo aplicativos de outros clientes. Azure também oferece que planos de serviço de aplicativo para implantar aplicativos web no Linux os recursos de computação. Você pode ignorar esta etapa se você já tiver um plano do serviço de aplicativo que você deseja usar.

   Para criar um plano do serviço de aplicativo usando a camada gratuita do Windows, execute o comando a seguir. Use o mesmo grupo de recursos em que hub IoT está. O nome do plano de serviço pode conter letras minúsculas e letras, números e hifens.

   ```azurecli-interactive
   az appservice plan create --name <app service plan name> --resource-group <your resource group name> --sku FREE
   ```

2. Agora, provisione um aplicativo web em seu plano do serviço de aplicativo. O `--deployment-local-git` parâmetro permite que o código do aplicativo web ser carregado e implantado a partir de um repositório Git no seu computador local. Nome do aplicativo web deve ser globalmente exclusivo e pode conter letras minúsculas e letras, números e hifens.

   ```azurecli-interactive
   az webapp create -n <your web app name> -g <your resource group name> -p <your app service plan name> --deployment-local-git
   ```

3. Agora, adicione as configurações do aplicativo para as variáveis de ambiente que especificam a cadeia de conexão do hub IoT e o grupo de consumidores do hub de eventos. As configurações individuais são delimitado de espaço a `-settings` parâmetro. Use a cadeia de caracteres de conexão de serviço para o hub IoT e o grupo de consumidores criado anteriormente neste tutorial. Não os valores da cotação.

   ```azurecli-interactive
   az webapp config appsettings set -n <your web app name> -g <your resource group name> --settings EventHubConsumerGroup=<your consumer group> IotHubConnectionString=<your IoT hub connection string>
   ```

4. Habilite os soquetes Web de protocolo para o aplicativo web e defina o aplicativo web para receber HTTPS apenas solicita (solicitações HTTP serão redirecionadas para HTTPS).

   ```azurecli-interactive
   az webapp config set -n <your web app name> -g <your resource group name> --web-sockets-enabled true
   az webapp update -n <your web app name> -g <your resource group name> --https-only true
   ```

5. Para implantar o código no serviço de aplicativo, você usará seu [credenciais de implantação de nível de usuário](https://docs.microsoft.com/azure/app-service/deploy-configure-credentials). Suas credenciais de implantação de nível de usuário são diferentes das suas credenciais do Azure e são usados para implantações de FTP a um aplicativo web e Git local. Uma vez definida, eles são válidos em todos os seus aplicativos de serviço de aplicativo em todas as assinaturas em sua conta do Azure. Se você definiu anteriormente credenciais de implantação de nível de usuário, você pode usá-los.

   Se você ainda não tiver definido anteriormente credenciais de implantação de nível de usuário, ou você não lembra a senha, execute o comando a seguir. Seu nome de usuário de implantação deve ser exclusivo dentro do Azure, e não deve conter o ' @' símbolo para envios por push do Git local. Quando solicitado, insira e confirme sua nova senha. A senha deve ter pelo menos oito caracteres, com dois destes três elementos: letras, números, símbolos.

   ```azurecli-interactive
   az webapp deployment user set --user-name <your deployment user name>
   ```

6. Obtenha a URL do Git para usar para enviar por push o seu código até o serviço de aplicativo.

   ```azurecli-interactive
   az webapp deployment source config-local-git -n <your web app name> -g <your resource group name>
   ```

7. Adicione um remoto para seu clone que referencia o repositório Git para o aplicativo web no serviço de aplicativo. Para \<URL de clone de Git\>, use a URL retornada na etapa anterior. Execute o seguinte comando na janela de comando.

   ```cmd
   git remote add webapp <Git clone URL>
   ```

8. Para implantar o código no serviço de aplicativo, digite o seguinte comando na janela de comando. Se você for solicitado para credenciais, insira as credenciais de implantação de nível de usuário que você criou na etapa 5. Certifique-se de que você enviar por push para a ramificação mestra do serviço de aplicativo remoto.

    ```cmd
    git push webapp master:master
    ```

9. O progresso da implantação será atualizado na janela de comando. Uma implantação bem-sucedida terminará com linhas semelhantes à seguinte saída:

    ```cmd
    remote:
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://contoso-web-app-3.scm.azurewebsites.net/contoso-web-app-3.git
    6b132dd..7cbc994  master -> master
    ```

10. Execute o seguinte comando para consultar o estado do aplicativo web e verifique se que ele está em execução:

    ```azurecli-interactive
    az webapp show -n <your web app name> -g <your resource group name> --query state
    ```

11. Navegue até `https://<your web app name>.azurewebsites.net` em um navegador. Uma página da web semelhante àquele que você viu quando você executou o aplicativo web localmente exibe. Supondo que o dispositivo estiver em execução e envio de dados, você deverá ver um gráfico em execução as 50 mais recentes temperatura e umidade de leituras do enviadas pelo dispositivo.

## <a name="troubleshooting"></a>solução de problemas

Se você se deparar com problemas com este exemplo, tente as etapas nas seções a seguir. Se você ainda tiver problemas, envie-nos comentários na parte inferior deste tópico.

### <a name="client-issues"></a>Problemas do cliente

* Se um dispositivo não aparecer na lista, ou nenhum gráfico está sendo desenhado, verifique se que o código de dispositivo está em execução em seu dispositivo.

* No navegador, abra as ferramentas de desenvolvedor (em muitos navegadores F12 chave será abri-lo) e localize o console. Procure os avisos ou erros impressos lá.

* Você pode depurar o script do lado do cliente no /js/chat-device-data.js.

### <a name="local-website-issues"></a>Problemas de site da Web local

* Assista a saída na janela onde você iniciou o nó de saída do console.

* Depure o código do servidor, especificamente o Server. js e /scripts/event-hub-reader.js.

### <a name="azure-app-service-issues"></a>Problemas de serviço de aplicativo do Azure

* No portal do Azure, vá para seu aplicativo web. Sob **Monitoring** no painel esquerdo, selecione **logs de serviço de aplicativo**. Transforme **log de aplicativo (sistema de arquivos)** , defina **nível** erro e, em seguida, selecione **salvar**. Em seguida, abra **fluxo de Log** (sob **monitoramento**).

* De seu aplicativo web no portal do Azure, sob **ferramentas de desenvolvimento** selecionar **Console** e validar versões de nó e o npm com `node -v` e `npm -v`.

* Se você vir um erro sobre não localizar um pacote, você pode executar as etapas fora de ordem. Quando o site for implantado (com `git push`) o serviço de aplicativo é executado `npm install`, que é executado com base na versão atual do nó tenha configurado. Se o que é alterado na configuração mais tarde, você precisará fazer uma alteração sem sentido no código e enviar por push novamente.

## <a name="next-steps"></a>Próximas etapas

Você usou com êxito seu aplicativo Web para visualizar dados do sensor em tempo real do seu Hub IoT.

Para a outra maneira de visualizar dados do IoT Hub do Azure, consulte [usar o Power BI para visualizar dados de sensor em tempo real do hub IoT](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

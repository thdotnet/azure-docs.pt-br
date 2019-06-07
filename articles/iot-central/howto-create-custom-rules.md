---
title: Estender o Azure IoT Central com regras personalizadas e notificações | Microsoft Docs
description: Como desenvolvedor de soluções, configure um aplicativo de Central da IoT para enviar notificações por email quando um dispositivo interromperá o envio de telemetria. Esta solução usa o Azure Stream Analytics e o Azure Functions.
author: dominicbetts
ms.author: dobett
ms.date: 05/14/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 5248b9546ffe931b72123778d0d23574e5238405
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742407"
---
# <a name="extend-azure-iot-central-with-custom-rules-that-send-notifications"></a>Estender o Azure IoT Central com regras personalizadas que enviam notificações

Este guia de instruções mostra a você como desenvolvedor de soluções, como estender o aplicativo de Central da IoT com regras personalizadas e notificações. O exemplo mostra como enviar uma notificação a um operador quando um dispositivo interromperá o envio de telemetria. A solução usa uma [do Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) consulta para detectar quando um dispositivo parar de enviar a telemetria. Usos do trabalho do Stream Analytics [Azure Functions](https://docs.microsoft.com/azure/azure-functions/) para enviar notificação de emails usando [SendGrid](https://sendgrid.com/docs/for-developers/partners/microsoft-azure/).

Este guia de instruções mostra como estender o IoT Central além do que ele já pode fazer com as regras internas e as ações.

Este guia de instruções, você aprenderá como:

* Stream de telemetria de um aplicativo de IoT Central usando *exportação contínua de dados*.
* Crie uma consulta de Stream Analytics que detecta quando um dispositivo parou de envio de dados.
* Envie uma notificação por email usando o Azure Functions e os serviços do SendGrid.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir as etapas neste guia de instruções, é necessário ter uma assinatura ativa do Azure.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

### <a name="iot-central-application"></a>Aplicativo de IoT Central

Criar um aplicativo de IoT Central dos [do Azure IoT Central - meus aplicativos](https://aka.ms/iotcentral) página com as seguintes configurações:

| Configuração | Value |
| ------- | ----- |
| Plano de Pagamento | Pré-paga |
| Modelo de aplicativo | Exemplo Contoso |
| Nome do aplicativo | Aceite o padrão ou escolha seu próprio nome |
| URL | Aceite o padrão ou escolha seu próprio prefixo de URL exclusivo |
| Diretório | Seu locatário do Azure Active Directory |
| Assinatura do Azure | Sua assinatura do Azure |
| Região | Leste dos EUA |

Os exemplos e capturas de tela neste artigo usam o **Leste dos EUA** região. Escolha um local perto de você e certifique-se de que criar todos os seus recursos na mesma região.

### <a name="resource-group"></a>Grupo de recursos

Use o [portal do Azure para criar um grupo de recursos](https://portal.azure.com/#create/Microsoft.ResourceGroup) chamado **DetectStoppedDevices** para conter outros recursos que você criou. Crie os recursos do Azure no mesmo local do seu aplicativo IoT Central.

### <a name="event-hubs-namespace"></a>Namespace do Hubs de Eventos

Use o [portal do Azure para criar um namespace de Hubs de eventos](https://portal.azure.com/#create/Microsoft.EventHub) com as seguintes configurações:

| Configuração | Valor |
| ------- | ----- |
| NOME    | Escolha seu nome de namespace |
| Tipo de preço | Basic |
| Assinatura | Sua assinatura |
| Grupo de recursos | DetectStoppedDevices |
| Local padrão | Leste dos EUA |
| Unidades de produtividade | 1 |

### <a name="stream-analytics-job"></a>Trabalho do Stream Analytics

Use o [portal do Azure para criar um trabalho do Stream Analytics](https://portal.azure.com/#create/Microsoft.StreamAnalyticsJob) com as seguintes configurações:

| Configuração | Valor |
| ------- | ----- |
| NOME    | Escolha seu nome de trabalho |
| Assinatura | Sua assinatura |
| Grupo de recursos | DetectStoppedDevices |
| Local padrão | Leste dos EUA |
| Ambiente de hospedagem | Nuvem |
| Unidades de transmissão | 3 |

### <a name="function-app"></a>Aplicativo de funções

Use o [portal do Azure para criar um aplicativo de funções](https://portal.azure.com/#create/Microsoft.FunctionApp) com as seguintes configurações:

| Configuração | Value |
| ------- | ----- |
| Nome do aplicativo    | Escolha o nome do seu aplicativo de função |
| Assinatura | Sua assinatura |
| Grupo de recursos | DetectStoppedDevices |
| SO | Windows |
| Plano de Hospedagem | Plano de consumo |
| Local padrão | Leste dos EUA |
| Pilha de tempo de execução | .NET |
| Armazenamento | Criar Novo |

### <a name="sendgrid-account"></a>Conta do SendGrid

Use o [portal do Azure para criar uma conta do SendGrid](https://portal.azure.com/#create/Sendgrid.sendgrid) com as seguintes configurações:

| Configuração | Valor |
| ------- | ----- |
| NOME    | Escolha seu nome de conta do SendGrid |
| Senha | Criar uma senha |
| Assinatura | Sua assinatura |
| Grupo de recursos | DetectStoppedDevices |
| Tipo de preço | F1 Gratuito |
| Informações de contato | Preencha as informações necessárias |

Quando você tiver criado todos os recursos necessários, sua **DetectStoppedDevices** grupo de recursos é semelhante a captura de tela a seguir:

![Detectar o grupo de recursos de dispositivos parado](media/howto-create-custom-rules/resource-group.png)

## <a name="create-an-event-hub"></a>Criar um Hub de Evento

Você pode configurar um aplicativo de IoT Central a exportação contínua de telemetria para um hub de eventos. Nesta seção, você deve criar um hub de eventos para receber telemetria de seu aplicativo IoT Central. O hub de eventos fornece a telemetria para seu trabalho de Stream Analytics para processamento.

1. No portal do Azure, navegue até seu namespace de Hubs de eventos e selecione **+ Hub de eventos**.
1. Nomeie seu hub de eventos **centralexport**e selecione **criar**.

Seu namespace de Hubs de eventos é semelhante a captura de tela a seguir:

![Namespace do Hubs de Eventos](media/howto-create-custom-rules/event-hubs-namespace.png)

## <a name="get-sendgrid-api-key"></a>Obter chave de API do SendGrid

Seu aplicativo de função precisa de uma chave de API do SendGrid para enviar mensagens de email. Para criar uma chave de API do SendGrid:

1. No portal do Azure, navegue até sua conta do SendGrid. Em seguida, escolha **gerenciar** para acessar sua conta do SendGrid.
1. Na sua conta do SendGrid, escolha **as configurações**, em seguida, **chaves de API**. Escolher **criar chave de API**:

    ![Criar chave de API do SendGrid](media/howto-create-custom-rules/sendgrid-api-keys.png)

1. Sobre o **criar chave de API** página, crie uma chave nomeada **AzureFunctionAccess** com **acesso completo** permissões.
1. Anote a chave de API, você precisará dela quando você configurar seu aplicativo de funções.

## <a name="define-the-function"></a>Definir a função

Esta solução usa um aplicativo Azure Functions para enviar uma notificação por email quando o trabalho do Stream Analytics detecta um dispositivo parado. Para criar seu aplicativo de funções:

1. No portal do Azure, navegue até a **serviço de aplicativo** da instância na **DetectStoppedDevices** grupo de recursos.
1. Selecione **+** para criar uma nova função.
1. Sobre o **escolher um ambiente de desenvolvimento** , escolha **no portal** e, em seguida, selecione **continuar**.
1. Sobre o **criar uma função** , escolha **Webhook + API** e, em seguida, selecione **criar**.

O portal cria uma função padrão chamada **HttpTrigger1**:

![Função de gatilho HTTP padrão](media/howto-create-custom-rules/default-function.png)

### <a name="configure-function-bindings"></a>Configurar associações de função

Para enviar emails com SendGrid, você precisa configurar as ligações para sua função da seguinte maneira:

1. Selecione **integrar**, escolha a saída **HTTP ($return)** e, em seguida, selecione **excluir**.
1. Escolher **+ nova saída**, em seguida, escolha **SendGrid**e, em seguida, escolha **selecione**. Escolher **instalar** para instalar a extensão do SendGrid.
1. Quando a instalação for concluída, selecione **usar o valor retornado da função**. Adicionar um válido **endereço** para receber notificações por email.  Adicionar um válido **do endereço** a ser usado como o remetente do email.
1. Selecione **novos** lado **configuração de aplicativo de chave de API do SendGrid**. Insira **SendGridAPIKey** como a chave e a chave de API do SendGrid que você anotou anteriormente como o valor. Em seguida, selecione **Criar**.
1. Escolher **salvar** para salvar as associações do SendGrid para sua função.

As configurações de integrar parecer com a seguinte captura de tela:

![Integrações de aplicativo de função](media/howto-create-custom-rules/function-integrate.png)

### <a name="add-the-function-code"></a>Adicione o código de função

Para implementar sua função, adicione o C# código para analisar a solicitação HTTP de entrada e enviar os emails da seguinte maneira:

1. Escolha o **HttpTrigger1** funcionar no seu aplicativo de funções e substitua o C# código com o código a seguir:

    ```csharp
    #r "Newtonsoft.Json"
    #r "..\bin\SendGrid.dll"

    using System;
    using SendGrid.Helpers.Mail;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;

    public static SendGridMessage Run(HttpRequest req, ILogger log)
    {
        string requestBody = new StreamReader(req.Body).ReadToEnd();
        log.LogInformation(requestBody);
        var notifications = JsonConvert.DeserializeObject<IList<Notification>>(requestBody);

        SendGridMessage message = new SendGridMessage();
        message.Subject = "Contoso device notification";

        var content = "The following device(s) have stopped sending telemetry:<br/><br/><table><tr><th>Device ID</th><th>Time</th></tr>";
        foreach(var notification in notifications) {
            log.LogInformation($"No message received - Device: {notification.deviceid}, Time: {notification.time}");
            content += $"<tr><td>{notification.deviceid}</td><td>{notification.time}</td></tr>";
        }
        content += "</table>";
        message.AddContent("text/html", content);

        return message;
    }

    public class Notification
    {
        public string deviceid { get; set; }
        public string time { get; set; }
    }
    ```

    Você pode ver uma mensagem de erro até que você salve o novo código.

1. Selecione **salvar** para salvar a função.

### <a name="test-the-function-works"></a>Testar a função funciona

Para testar a função no portal, primeiro escolha **Logs** na parte inferior do editor de código. Em seguida, escolha **teste** à direita do editor de código. Use o seguinte JSON como o **corpo da solicitação**:

```json
[{"deviceid":"test-device-1","time":"2019-05-02T14:23:39.527Z"},{"deviceid":"test-device-2","time":"2019-05-02T14:23:50.717Z"},{"deviceid":"test-device-3","time":"2019-05-02T14:24:28.919Z"}]
```

As mensagens de log de função aparecem na **Logs** painel:

![Saída de log de função](media/howto-create-custom-rules/function-app-logs.png)

Depois de alguns minutos, o **para** endereço de email receberão um email com o seguinte conteúdo:

```txt
The following device(s) have stopped sending telemetry:

Device ID   Time
test-device-1   2019-05-02T14:23:39.527Z
test-device-2   2019-05-02T14:23:50.717Z
test-device-3   2019-05-02T14:24:28.919Z
```

## <a name="add-stream-analytics-query"></a>Adicionar consulta do Stream Analytics

Essa solução usa uma consulta do Stream Analytics para detectar quando um dispositivo para enviar telemetria por mais de 120 segundos. A consulta usa a telemetria do hub de eventos como entrada. O trabalho envia os resultados da consulta para o aplicativo de funções. Nesta seção, você deve configurar o trabalho do Stream Analytics:

1. No portal do Azure, navegue até o trabalho do Stream analytics, sob **topologia de trabalhos** selecionar **entradas**, escolha **+ Adicionar entrada de fluxo**e, em seguida, escolha **evento Hub**.
1. Use as informações na tabela a seguir para configurar a entrada usando o hub de eventos que você criou anteriormente e, em seguida, escolha **salvar**:

    | Configuração | Value |
    | ------- | ----- |
    | Alias de entrada | centraltelemetry |
    | Assinatura | Sua assinatura |
    | Namespace do Hub de Eventos | O namespace do Hub de eventos |
    | Nome do Hub de Eventos | Usar existente - **centralexport** |

1. Sob **topologia de trabalhos**, selecione **saídas**, escolha **+ adicionar**e, em seguida, escolha **Azure function**.
1. Use as informações na tabela a seguir para configurar a saída e, em seguida, escolha **salvar**:

    | Configuração | Value |
    | ------- | ----- |
    | Alias de saída | EmailNotification |
    | Assinatura | Sua assinatura |
    | Aplicativo de funções | Seu aplicativo de funções |
    | Função  | HttpTrigger1 |

1. Sob **topologia de trabalhos**, selecione **consulta** e substitua a consulta existente com o SQL a seguir:

    ```sql
    with
    LeftSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid1, 
        EventEnqueuedUtcTime AS time1
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    ),
    RightSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid2, 
        EventEnqueuedUtcTime AS time2
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    )

    SELECT
        LeftSide.deviceid1 as deviceid,
        LeftSide.time1 as time
    INTO
        [emailnotification]
    FROM
        LeftSide
        LEFT OUTER JOIN
        RightSide 
        ON
        LeftSide.deviceid1=RightSide.deviceid2 AND DATEDIFF(second,LeftSide,RightSide) BETWEEN 1 AND 120
        where
        -- Find records where a device didn't send a message 120 seconds
        RightSide.deviceid2 is NULL
    ```

1. Clique em **Salvar**.
1. Para iniciar o trabalho do Stream Analytics, escolha **visão geral**, em seguida, **iniciar**, em seguida, **agora**e, em seguida, **iniciar**:

    ![Stream Analytics](media/howto-create-custom-rules/stream-analytics.png)

## <a name="configure-export-in-iot-central"></a>Configurar a exportação no IoT Central

Navegue até a [aplicativo IoT Central](https://aka.ms/iotcentral) criados a partir do modelo de Contoso. Nesta seção, você deve configurar o aplicativo para transmitir a telemetria de seus dispositivos simulados para seu hub de eventos. Para configurar a exportação:

1. Navegue até a **exportação contínua de dados** página, selecione **+ novo**e então **Hubs de eventos**.
1. Use as configurações a seguir para configurar a exportação e, em seguida, selecione **salvar**:

    | Configuração | Value |
    | ------- | ----- |
    | Nome para exibição | Exportar para Hubs de eventos |
    | Enabled | Por |
    | Namespace do Hubs de Eventos | O nome do namespace de Hubs de eventos |
    | Hub de Eventos | centralexport |
    | Medidas | Por |
    | Dispositivos | Desativar |
    | Modelos de Dispositivo | Desativar |

![Configuração de exportação contínua de dados](media/howto-create-custom-rules/cde-configuration.png)

Aguarde até que seja o status de exportação **executando** antes de continuar.

## <a name="test"></a>Teste

Para testar a solução, você pode desabilitar a exportação contínua de dados da Central da IoT para dispositivos simulados de paradas:

1. Em seu aplicativo de Central da IoT, navegue até a **exportação contínua de dados** página e selecione o **exportar para Hubs de eventos** Exportar configuração.
1. Definir **Enabled** à **Off** e escolha **salvar**.
1. Depois de pelo menos dois minutos, o **para** endereço de email recebe uma ou mais emails que se parecem com o exemplo a seguir conteúdos:

    ```txt
    The following device(s) have stopped sending telemetry:

    Device ID   Time
    7b169aee-c843-4d41-9f25-7a02671ee659    2019-05-09T14:28:59.954Z
    ```

## <a name="tidy-up"></a>Limpar

Para limpar após estas instruções e evitar custos desnecessários, exclua o **DetectStoppedDevices** grupo de recursos no portal do Azure.

Você pode excluir o aplicativo IoT Central a **gerenciamento** página dentro do aplicativo.

## <a name="next-steps"></a>Próximas etapas

Neste guia de instruções, você aprendeu como:

* Stream de telemetria de um aplicativo de IoT Central usando *exportação contínua de dados*.
* Crie uma consulta de Stream Analytics que detecta quando um dispositivo parou de envio de dados.
* Envie uma notificação por email usando o Azure Functions e os serviços do SendGrid.

Agora que você sabe como criar regras personalizadas e notificações, a próxima etapa sugerida é saber como [estender o Azure IoT Central com análises personalizadas](howto-create-custom-analytics.md).

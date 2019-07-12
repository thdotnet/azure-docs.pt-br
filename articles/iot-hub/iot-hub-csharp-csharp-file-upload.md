---
title: Carregar arquivos de dispositivos para o Hub IoT do Azure com .NET | Microsoft Docs
description: Como carregar arquivos de um dispositivo para a nuvem usando o SDK do dispositivo IoT do Azure para .NET. Os arquivos carregados são armazenados em um contêiner de Azure Storage Blob.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: robinsh
ms.openlocfilehash: 79288f2204030790b2308905d90ff8e035fe2dd9
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621858"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a>Carregar arquivos do seu dispositivo para a nuvem com o Hub IoT usando .NET

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Este tutorial se baseia no código a [enviar mensagens da nuvem para dispositivo com IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial para mostrar a você como usar os recursos de upload de arquivo do IoT Hub. Ele mostra como:

* Fornecer com segurança um URI de blob do Azure a um dispositivo para carregamento de um arquivo.

* Usar as notificações de carregamento de arquivo do Hub IoT para disparar o processamento do arquivo no back-end do aplicativo.

O [enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-dotnet.md) guia de início rápido e [enviar mensagens da nuvem para dispositivo com IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial mostram a funcionalidade básica de dispositivo para a nuvem e nuvem para dispositivo mensagens de IoT Hub. O tutorial [Configurar o roteamento de mensagens com o Hub IoT](tutorial-routing.md) descreve uma forma de armazenamento confiável das mensagens do dispositivo para a nuvem no Armazenamento de Blobs do Azure. No entanto, em alguns cenários você não pode mapear facilmente os dados que seus dispositivos enviam em mensagens relativamente menores do dispositivo para a nuvem que o Hub IoT aceita. Por exemplo:

* Arquivos grandes que contêm imagens
* Vídeos
* Dados de vibração amostrados a alta frequência
* Alguma forma de dados pré-processados

Esses arquivos normalmente são processados em lote na nuvem usando ferramentas como o [Azure Data Factory](../data-factory/introduction.md) ou a pilha do [Hadoop](../hdinsight/index.yml). Quando você precisar carregar arquivos de um dispositivo, ainda poderá usar a segurança e a confiabilidade do Hub IoT.

No final deste tutorial, você executará dois aplicativos do console .NET:

* **SimulatedDevice**, uma versão modificada do aplicativo criado na [enviar mensagens da nuvem para dispositivo com IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial. Esse aplicativo carrega um arquivo no armazenamento usando um URI SAS fornecido pelo seu Hub IoT.

* **ReadFileUploadNotification**, que recebe notificações de carregamento de arquivo de seu Hub IoT.

> [!NOTE]
> O Hub IoT é compatível com muitas plataformas de dispositivo e linguagens (incluindo C, Java e Javascript) por meio dos SDKs do dispositivo IoT do Azure. Confira a [Central de Desenvolvedores do IoT do Azure](https://azure.microsoft.com/develop/iot) para obter instruções passo a passo sobre como conectar seu dispositivo ao Hub IoT do Azure.

Para concluir este tutorial, você precisará do seguinte:

* Visual Studio

* Uma conta ativa do Azure. (Se você não tiver uma conta, poderá criar uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) em apenas alguns minutos.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Carregar um arquivo de um aplicativo de dispositivo

Nesta seção, você modificará o aplicativo de dispositivo que você criou na [enviar mensagens da nuvem para dispositivo com IoT Hub](iot-hub-csharp-csharp-c2d.md) para receber mensagens da nuvem para dispositivo do hub IoT.

1. No Visual Studio, clique com o botão direito no projeto **SimulatedDevice**, clique em **Adicionar** e, em seguida, clique em **Item Existente**. Navegue até um arquivo de imagem e inclua-o no projeto. Este tutorial pressupõe que a imagem é denominada `image.jpg`.

1. Clique com o botão direito do mouse na imagem e clique em **Propriedades**. Verifique se **Copiar para o Diretório de Saída** está definido como **Copiar sempre**.

    ![Mostrar onde atualizar a propriedade de imagem para copiar para o diretório de saída](./media/iot-hub-csharp-csharp-file-upload/image-properties.png)

1. No arquivo **Program.cs** , adicione as seguintes instruções na parte superior do arquivo:

    ```csharp
    using System.IO;
    ```

1. Adicione o seguinte método à classe **Programa** :

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    O método `UploadToBlobAsync` usa o nome de arquivo e a fonte de transmissão do arquivo a ser carregado e manipula o upload para o armazenamento. O aplicativo de console exibe o tempo necessário para carregar o arquivo.

1. Adicione o seguinte método ao método **Main**, logo antes da linha `Console.ReadLine()`:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Para simplificar, este tutorial não implementa nenhuma política de repetição. No código de produção, implemente políticas de repetição (como uma retirada exponencial), conforme sugerido no artigo [Tratamento de falhas transitórias](/azure/architecture/best-practices/transient-faults).

## <a name="receive-a-file-upload-notification"></a>Receber uma notificação de upload de arquivo

Nesta seção, você escreverá um aplicativo de console do .NET que recebe mensagens de notificação de upload de arquivo do Hub IoT.

1. Na solução atual do Visual Studio, crie um projeto do Visual C# do Windows usando o modelo de projeto do **Aplicativo do Console** . Nomeie o projeto **ReadFileUploadNotification**.

    ![Novo projeto no Visual Studio](./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png)

2. No Gerenciador de Soluções, clique com o botão direito no projeto **ReadFileUploadNotification** e, em seguida, clique em **Gerenciar Pacotes NuGet...** .

3. Na janela **Gerenciador de Pacotes NuGet**, procure **Microsoft.Azure.Devices**, clique em **Instalar** e aceite os termos de uso.

    Essa ação baixa, instala e adiciona uma referência ao [pacote NuGet do SDK do serviço IoT do Azure](https://www.nuget.org/packages/Microsoft.Azure.Devices/) no projeto **ReadFileUploadNotification**.

4. No arquivo **Program.cs** , adicione as seguintes instruções na parte superior do arquivo:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

5. Adicione os seguintes campos à classe **Program** . Substitua os valores de espaço reservado pela cadeia de conexão do Hub IoT de [Enviar telemetria de um dispositivo para um Hub IoT](quickstart-send-telemetry-dotnet.md):

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

6. Adicione o seguinte método à classe **Programa** :

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload notification: {0}", 
              string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    É importante lembrar que o padrão de recebimento é o mesmo usado para receber mensagens da nuvem para o dispositivo do aplicativo do dispositivo.

7. Por fim, adicione as seguintes linhas ao método **Principal** :

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Executar os aplicativos

Agora você está pronto para executar os aplicativos.

1. No Visual Studio, clique com o botão direito para a solução e selecione **Definir projetos de inicialização**. Selecione **Vários projetos de inicialização**, em seguida, selecione a ação **Iniciar** para **ReadFileUploadNotification** e **SimulatedDevice**.

2. Pressione **F5**. Ambos os aplicativos devem ser iniciados. Você deve ver o carregamento concluído em um aplicativo de console e a mensagem de notificação do carregamento sendo recebida pelo outro aplicativo de console. Você pode usar o [Portal do Azure](https://portal.azure.com/) ou o Gerenciador de Servidores do Visual Studio para verificar a presença do arquivo carregado em sua conta de Armazenamento do Azure.

    ![Captura de tela mostrando a tela de saída](./media/iot-hub-csharp-csharp-file-upload/run-apps1.png)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a usar os recursos de carregamento de arquivo do Hub IoT para simplificar os carregamentos de arquivos de dispositivos. Você pode continuar explorando os recursos e cenários do Hub IoT com os seguintes artigos:

* [Criar um Hub IoT de modo programático](iot-hub-rm-template-powershell.md)

* [Introdução ao SDK C](iot-hub-device-sdk-c-intro.md)

* [SDKs do Azure IoT](iot-hub-devguide-sdks.md)

Para explorar melhor as funcionalidades do Hub IoT, consulte:

* [Implantando o AI em dispositivos de borda com o Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)

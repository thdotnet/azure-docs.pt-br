---
title: Autenticar dispositivos downstream – Azure IoT Edge | Microsoft Docs
description: Como autenticar dispositivos downstream ou dispositivos de folha para o IoT Hub e rotear sua conexão por meio de dispositivos de gateway do IoT Edge do Azure.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5785b0260474bd0eb861236a0bd78066475baacd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082385"
---
# <a name="authenticate-a-downstream-device-to-azure-iot-hub"></a>Autenticar um dispositivo downstream ao IoT Hub do Azure

Em um cenário de gateway transparente, os dispositivos downstream (às vezes chamados de dispositivos de folha ou filho) precisam identidades no IoT Hub, como qualquer outro dispositivo. Este artigo explica as opções para autenticar um dispositivo downstream ao IoT Hub e, em seguida, demonstra como declarar a conexão de gateway.

Há três etapas gerais para configurar uma conexão de gateway transparente com êxito. Este artigo aborda a segunda etapa:

1. O dispositivo de gateway precisa ser capaz de conectar com segurança para os dispositivos downstream, receber comunicações dos dispositivos downstream e rotear mensagens para o destino apropriado. Para obter mais informações, consulte [configurar um dispositivo IoT Edge para atuar como um gateway transparente](how-to-create-transparent-gateway.md).
2. **O dispositivo downstream precisa ter uma identidade do dispositivo para ser capaz de autenticar com o IoT Hub e saber para se comunicar por meio de seu dispositivo de gateway.**
3. O dispositivo downstream precisa ser capaz de se conectar com segurança a seu dispositivo de gateway. Para obter mais informações, consulte [Cnectar um dispositivo downstream a um gateway do Azure IoT Edge](how-to-connect-downstream-device.md).

Os dispositivos downstream podem autenticar com o IoT Hub usando um dos três métodos: chaves simétricas (também conhecidas como chaves de acesso compartilhado), x. 509 os certificados autoassinados ou certificados x. 509 certificados autoassinados de autoridade (CA). As etapas de autenticação são semelhantes às etapas usadas para configurar qualquer dispositivo IoT Edge com o IoT Hub, com pequenas diferenças para declarar a relação de gateway.

As etapas neste artigo mostram aprovisionamento manual, não automático de provisionamento com o serviço de provisionamento de dispositivos do Azure IoT Hub. 

## <a name="symmetric-key-authentication"></a>Autenticação de chave simétrica

Autenticação de chave simétrica ou autenticação de chave de acesso compartilhado, é a maneira mais simples para autenticar com o IoT Hub. Com autenticação de chave simétrica, uma chave de base64 está associada com a ID do dispositivo IoT no IoT Hub. Você pode incluir essa chave em seus aplicativos de IoT para que seu dispositivo possa apresentar quando ele se conecta ao IoT Hub. 

### <a name="create-the-device-identity"></a>Criar a identidade do dispositivo 

Adicione um novo dispositivo de IoT em seu hub IoT, usando o portal do Azure, CLI do Azure ou a extensão de IoT para Visual Studio Code. Lembre-se de que os dispositivos downstream devem ser identificados no IoT Hub como dispositivo de IoT regular, não os dispositivos IoT Edge. 

Quando você cria a nova identidade do dispositivo, forneça as seguintes informações: 

* Crie uma ID para seu dispositivo.

* Selecione **chave simétrica** como o tipo de autenticação. 

* Opcionalmente, optar por **definir um dispositivo pai** e selecione o dispositivo de gateway do IoT Edge que este dispositivo downstream irão se conectar por meio do. Esta etapa é opcional para autenticação de chave simétrica, mas é recomendável porque a definição de um dispositivo pai habilita [recursos offline](offline-capabilities.md) para seu dispositivo downstream. Você sempre pode atualizar os detalhes do dispositivo para adicionar ou alterar o pai mais tarde. 

   ![Criar ID do dispositivo com autenticação de chave simétrica no portal](./media/how-to-authenticate-downstream-device/symmetric-key-portal.png)

Você pode usar o [extensão de IoT para CLI do Azure](https://github.com/Azure/azure-iot-cli-extension) para concluir a mesma operação. O exemplo a seguir cria um novo dispositivo IoT com autenticação de chave simétrica e atribui a um dispositivo pai: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID}
```

Para obter mais informações sobre os comandos de CLI do Azure para a criação de dispositivo e o gerenciamento de pai/filho, consulte o conteúdo de referência para [identidade de dispositivo de hub de iot de az](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) comandos.

### <a name="connect-to-iot-hub-through-a-gateway"></a>Conectar ao IoT Hub por meio de um gateway

O mesmo processo é usado para autenticar IoT regular dispositivos no IoT Hub com chaves simétricas também se aplica a dispositivos downstream. A única diferença é que você precisa adicionar um ponteiro para o dispositivo de gateway para rotear a conexão ou, em cenários offline, para lidar com a autenticação em nome do IoT Hub. 

Para autenticação de chave simétrica, não há nenhuma etapa adicional que você precisa levar em seu dispositivo para ele autenticar com o IoT Hub. Você ainda precisa de certificados em vigor para que o dispositivo downstream pode se conectar ao seu dispositivo de gateway, conforme descrito em [conectar um dispositivo downstream a um gateway do Azure IoT Edge](how-to-connect-downstream-device.md).

Depois de criar uma identidade de dispositivo de IoT no portal, você pode recuperar suas chaves primárias ou secundárias. Uma dessas chaves deve ser incluído na cadeia de conexão que você incluir em qualquer aplicativo que se comunica com o IoT Hub. Para autenticação de chave simétrica, o IoT Hub fornece a cadeia de caracteres de conexão totalmente formadas nos detalhes do dispositivo para sua conveniência. Você precisa adicionar informações adicionais sobre o dispositivo de gateway para a cadeia de caracteres de conexão. 

Cadeias de caracteres de conexão de chave simétrica para dispositivos downstream precisam dos seguintes componentes: 

* O hub IoT que o dispositivo se conecta ao: `Hostname={iothub name}.azure-devices.net`
* A ID do dispositivo registrado com o hub: `DeviceID={device ID}`
* A chave primária ou secundária: `SharedAccessKey={key}`
* O dispositivo de gateway que o dispositivo se conecta por meio do. Forneça a **hostname** valor do arquivo de config. YAML do dispositivo de gateway do IoT Edge: `GatewayHostName={gateway hostname}`

Tudo isso, uma cadeia de caracteres de conexão completa se parece com:

``` 
HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
```

Se você estabelecer uma relação de pai/filho para este dispositivo downstream, você pode simplificar a cadeia de caracteres de conexão chamando o gateway diretamente como o host da conexão. Por exemplo: 

```
HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
```

## <a name="x509-authentication"></a>Autenticação X.509 

Há duas maneiras de autenticar um dispositivo de IoT usando certificados X.509. Forma que você optar por autenticar as etapas para conectar seu dispositivo ao IoT Hub são os mesmos. Escolha os certificados autoassinados ou assinados pela AC para autenticação e continuar a aprender como se conectar ao IoT Hub. 

Para obter mais informações sobre como o IoT Hub usa autenticação X.509, consulte os seguintes artigos: 
* [Autenticação de dispositivo usando certificados de autoridade de certificação X.509](../iot-hub/iot-hub-x509ca-overview.md)
* [Entendimento conceitual de certificados de autoridade de certificação X.509 no setor de IoT](../iot-hub/iot-hub-x509ca-concept.md)

### <a name="create-the-device-identity-with-x509-self-signed-certificates"></a>Criar a identidade do dispositivo com certificados autoassinados de x. 509

Para a autenticação autoassinada x. 509, também conhecida como autenticação de impressão digital, você precisará criar novos certificados para colocar em seu dispositivo IoT. Esses certificados têm uma impressão digital que você compartilha com o IoT Hub para autenticação. 

A maneira mais fácil de testar esse cenário é usar o mesmo computador que você usou para criar certificados no [configurar um dispositivo IoT Edge para atuar como um gateway transparente](how-to-create-transparent-gateway.md). Essa máquina deve já ser configurada com a ferramenta à direita, o certificado de autoridade de certificação raiz e o certificado de autoridade de certificação intermediário para criar os certificados de dispositivo de IoT. Você pode copiar os certificados finais e suas chaves privadas sobre seu dispositivo downstream posteriormente. Seguindo as etapas no artigo de gateway, você deve configurar openssl em seu computador e clonou o repositório do IoT Edge para scripts de criação de certificado de acesso. Em seguida, você fez um diretório de trabalho que chamamos  **\<WRKDIR >** para manter os certificados. Os certificados de padrão destinam-se para desenvolvimento e teste, portanto, apenas os últimos 30 dias. Você deve ter criado um certificado de autoridade de certificação raiz e um certificado intermediário. 

1. Navegue até seu diretório de trabalho em uma janela do PowerShell ou bash. 

2. Crie dois certificados (primários e secundários) para o dispositivo downstream. Forneça o nome do dispositivo e, em seguida, o rótulo primário ou secundário. Essas informações são usadas para nomear os arquivos de forma que você pode manter controle de certificados para vários dispositivos. 

   ```PowerShell
   New-CACertsDevice "<device name>-primary"
   New-CACertsDevice "<device name>-secondary"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device name>-primary"
   ./certGen.sh create_device_certificate "<device name>-secondary"
   ```

3. Recupere a impressão digital de SHA1 (chamada de uma impressão digital na interface do IoT Hub) de cada certificado, que é uma cadeia de 40 caracteres hexadecimais. Use o seguinte comando openssl para exibir o certificado e encontrar a impressão digital:

   ```PowerShell/bash
   openssl x509 -in <WORKDIR>/certs/iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

4. Navegue até seu hub IoT no portal do Azure e criar uma nova identidade do dispositivo IoT com os seguintes valores: 

   * Selecione **X.509 autoassinado** como o tipo de autenticação.
   * Cole as cadeias de caracteres hexadecimais que você copiou a partir dos certificados primários e secundários do seu dispositivo.
   * Selecione **definir um dispositivo pai** e escolher dispositivo de gateway que este dispositivo downstream se conectarão por meio de IoT Edge. Um dispositivo pai é necessário para autenticação de X.509 de um dispositivo downstream. 

   ![Criar ID do dispositivo com autenticação X.509 autoassinada no portal](./media/how-to-authenticate-downstream-device/x509-self-signed-portal.png)

5. Copie os seguintes arquivos para qualquer diretório em seu dispositivo downstream:

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device name>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device name>*.key.pem`

   Você vai fazer referência a esses arquivos nos aplicativos de dispositivo de folha que se conectam ao IoT Hub. Você pode usar um serviço como [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) ou uma função como [cópia segura Protocol](https://www.ssh.com/ssh/scp/) para mover os arquivos de certificado.

Você pode usar o [extensão de IoT para CLI do Azure](https://github.com/Azure/azure-iot-cli-extension) para concluir a mesma operação de criação do dispositivo. O exemplo a seguir cria um novo dispositivo IoT com a autenticação de X.509 autoassinada e atribui a um dispositivo pai: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_thumbprint --ptp {primary thumbprint} --stp {secondary thumbprint}
```

Para obter mais informações sobre os comandos de CLI do Azure para a criação do dispositivo, a geração de certificado e gerenciamento de pai e filho, consulte o conteúdo de referência para [identidade de dispositivo de hub de iot de az](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) comandos.

### <a name="create-the-device-identity-with-x509-ca-signed-certificates"></a>Criar o dispositivo de identidade com a autoridade de certificação X.509 assinado certificados

X. 509 autoridade (CA) assinada para autenticação de certificado, você precisa de um certificado de autoridade de certificação raiz registrado no IoT Hub que você usa para assinar certificados para seu dispositivo IoT. Qualquer dispositivo usando um certificado que foi problemas pelo certificado de autoridade de certificação raiz ou qualquer um dos seus certificados intermediários terão permissão para se autenticar. 

Esta seção se baseia nas instruções detalhadas no artigo de IoT Hub [configurar a segurança X.509 no hub IoT do Azure](../iot-hub/iot-hub-security-x509-get-started.md). Siga as etapas nesta seção para saber quais valores usar para configurar um dispositivo downstream que se conecta por meio de um gateway. 

A maneira mais fácil de testar esse cenário é usar o mesmo computador que você usou para criar certificados no [configurar um dispositivo IoT Edge para atuar como um gateway transparente](how-to-create-transparent-gateway.md). Essa máquina deve já ser configurada com a ferramenta à direita, o certificado de autoridade de certificação raiz e o certificado de autoridade de certificação intermediário para criar os certificados de dispositivo de IoT. Você pode copiar os certificados finais e suas chaves privadas sobre seu dispositivo downstream posteriormente. Seguindo as etapas no artigo de gateway, você deve configurar openssl em seu computador e clonou o repositório do IoT Edge para scripts de criação de certificado de acesso. Em seguida, você fez um diretório de trabalho que chamamos  **\<WRKDIR >** para manter os certificados. Os certificados de padrão destinam-se para desenvolvimento e teste, portanto, apenas os últimos 30 dias. Você deve ter criado um certificado de autoridade de certificação raiz e um certificado intermediário. 

1. Siga as instruções na [certificados de autoridade de certificação de X.509 registrar ao hub IoT](../iot-hub/iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) seção *configurar a segurança X.509 no hub IoT do Azure*. Nesta seção, você deve executar as seguintes etapas: 

   1. Carrega um certificado de autoridade de certificação raiz. Se você estiver usando os certificados que você criou o artigo de gateway transparente, carregue  **\<WRKDIR > /certs/azure-iot-test-only.root.ca.cert.pem** como o arquivo de certificado raiz. 
   2. Verifique se que você possui esse certificado de autoridade de certificação raiz. Você pode verificar a posse com as ferramentas de certificado no \<WRKDIR >. 

      ```powershell
      New-CACertsVerificationCert "<verification code from Azure portal>"
      ```

      ```bash
      ./certGen.sh create_verification_certificate <verification code from Azure portal>"
      ```

2. Siga as instruções na [criar um dispositivo X.509 para o hub IoT](../iot-hub/iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub) seção *configurar a segurança X.509 no hub IoT do Azure*. Nesta seção, você deve executar as seguintes etapas: 

   1. Adicione um novo dispositivo. Forneça um nome em minúsculas para **ID do dispositivo**e escolha o tipo de autenticação **assinado pela AC X.509**. 
   2. Defina um dispositivo pai. Para dispositivos downstream, selecione **definir um dispositivo pai** e escolha o IoT Edge de dispositivo de gateway que fornecerá a conexão ao IoT Hub. 

3. Crie uma cadeia de certificados para o seu dispositivo downstream. Use o mesmo certificado de autoridade de certificação raiz que você carregou para o IoT Hub para fazer esta cadeia. Use a mesma ID de dispositivo em minúsculas que você atribuiu à sua identidade de dispositivo no portal.

   ```powershell
   New-CACertsDevice "<device id>"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device id>"
   ```

4. Copie os seguintes arquivos para qualquer diretório em seu dispositivo downstream: 

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device id>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device id>*.key.pem`

   Você vai fazer referência a esses arquivos nos aplicativos de dispositivo de folha que se conectam ao IoT Hub. Você pode usar um serviço como [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) ou uma função como [cópia segura Protocol](https://www.ssh.com/ssh/scp/) para mover os arquivos de certificado.

Você pode usar o [extensão de IoT para CLI do Azure](https://github.com/Azure/azure-iot-cli-extension) para concluir a mesma operação de criação do dispositivo. O exemplo a seguir cria um novo dispositivo IoT com a autenticação de AC X.509 assinada e atribui a um dispositivo pai: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_ca
```

Para obter mais informações sobre os comandos de CLI do Azure para a criação de dispositivo e o gerenciamento de pai/filho, consulte o conteúdo de referência para [identidade de dispositivo de hub de iot de az](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) comandos.


### <a name="connect-to-iot-hub-through-a-gateway"></a>Conectar ao IoT Hub por meio de um gateway

Cada SDK de IoT do Azure lida com a autenticação de x. 509 um pouco diferente. No entanto, o mesmo processo é usado para autenticar IoT regular dispositivos no IoT Hub com certificados x. 509 também se aplica a dispositivos downstream. A única diferença é que você precisa adicionar um ponteiro para o dispositivo de gateway para rotear a conexão ou, em cenários offline, para lidar com a autenticação em nome do IoT Hub. Em geral, você pode seguir as mesmas etapas de autenticação de x. 509 para todos os dispositivos do IoT Hub e simplesmente substituir o valor de **Hostname** na cadeia de conexão para ser o nome do host do seu dispositivo de gateway. 

As seções a seguir mostram alguns exemplos para idiomas diferentes do SDK. 

>[!IMPORTANT]
>Os exemplos a seguir demonstram como os SDKs do Hub IoT usar certificados para autenticar dispositivos. Em uma implantação de produção, você deve armazenar todos os segredos, como privadas ou chaves SAS em um módulo de segurança de hardware (HSM). 

#### <a name="net"></a>.NET

Para obter um exemplo de um C# programa de autenticação no IoT Hub com certificados X.509, consulte [configurar a segurança X.509 no hub IoT do Azure](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates). Alguns dos principais linhas do exemplo estão incluídos aqui para demonstrar o processo de autenticação.

Ao declarar o nome do host para sua instância de DeviceClient, use o nome de host do dispositivo de gateway do IoT Edge. O nome do host pode ser encontrado no arquivo de config. YAML do dispositivo de gateway. 

Se você estiver usando os certificados de teste fornecidos pelo repositório de git do IoT Edge, a chave para os certificados é **1234**.

```csharp
try
{
    var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
    var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
    var deviceClient = DeviceClient.Create("<gateway hostname>", auth, TransportType.Amqp_Tcp_Only);

    if (deviceClient == null)
    {
        Console.WriteLine("Failed to create DeviceClient!");
    }
    else
    {
        Console.WriteLine("Successfully created DeviceClient!");
        SendEvent(deviceClient).Wait();
    }

    Console.WriteLine("Exiting...\n");
}
catch (Exception ex)
{
    Console.WriteLine("Error in sample: {0}", ex.Message);
}
```

#### <a name="c"></a>C

Para obter um exemplo de um programa C autenticação no IoT Hub com certificados X.509, consulte o SDK de IoT de C [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample) exemplo. Alguns dos principais linhas do exemplo estão incluídos aqui para demonstrar o processo de autenticação.

Ao definir a cadeia de caracteres de conexão para seu dispositivo downstream, use o nome de host do dispositivo de gateway do IoT Edge para o **HostName** parâmetro. O nome do host pode ser encontrado no arquivo de config. YAML do dispositivo de gateway. 

```C
// If your downstream device uses X.509 authentication (self signed or X.509 CA) then
// resulting connection string should look like the following:
// "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
static const char* connectionString = "[Downstream device IoT Edge connection string]";

// Path to the Edge "owner" root CA certificate
static const char* edge_ca_cert_path = "[Path to root CA certificate]";

// When the downstream device uses X.509 authentication, a certificate and key 
// in PRM format must be provided.
static const char * x509_device_cert_path = "[Path to primary or secondary device cert]";
static const char * x509_device_key_path = "[Path to primary or secondary device key]";

int main(void)
{
    // Create the iothub handle here
    device_handle = IoTHubDeviceClient_CreateFromConnectionString(connectionString, protocol);

    // Provide the Azure IoT device client with the same root
    // X509 CA certificate that was used to set up the IoT Edge gateway runtime
    if (edge_ca_cert_path != NULL)
    {
        cert_string = obtain_edge_ca_certificate();
        (void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
    }

    if ((x509_device_cert_path != NULL) && (x509_device_key_path != NULL))
    {
        const char *x509certificate = obtain_file_contents(x509_device_cert_path);
        const char *x509privatekey = obtain_file_contents(x509_device_key_path);
        if ((IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_CERT, x509certificate) != IOTHUB_CLIENT_OK) ||
            (IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_PRIVATE_KEY, x509privatekey) != IOTHUB_CLIENT_OK)
            )
        {
            printf("failure to set options for x509, aborting\r\n");
            exit(1);
        }
    }
}
```

#### <a name="nodejs"></a>Node.js

Para obter um exemplo de um programa do Node. js autenticação no IoT Hub com certificados X.509, consulte o SDK de IoT de Node. js [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js) exemplo. Alguns dos principais linhas do exemplo estão incluídos aqui para demonstrar o processo de autenticação.

Ao definir a cadeia de caracteres de conexão para seu dispositivo downstream, use o nome de host do dispositivo de gateway do IoT Edge para o **HostName** parâmetro. O nome do host pode ser encontrado no arquivo de config. YAML do dispositivo de gateway. 

Se você estiver usando os certificados de teste fornecidos pelo repositório de git do IoT Edge, a chave para os certificados é **1234**.

```node
// String containing Hostname and Device Id in the following format:
//  "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
var connectionString = '<DEVICE CONNECTION STRING WITH x509=true>';
var certFile = '<PATH-TO-CERTIFICATE-FILE>';
var keyFile = '<PATH-TO-KEY-FILE>';
var passphrase = '<KEY PASSPHRASE IF ANY>';

// fromConnectionString must specify a transport constructor, coming from any transport package.
var client = Client.fromConnectionString(connectionString, Protocol);

var options = {
   cert : fs.readFileSync(certFile, 'utf-8').toString(),
   key : fs.readFileSync(keyFile, 'utf-8').toString(),
   passphrase: passphrase
 };

// Calling setOptions with the x509 certificate and key (and optionally, passphrase) will configure the client transport to use x509 when connecting to IoT Hub
client.setOptions(options);
```

#### <a name="python"></a>Python

Para obter um exemplo de um programa de Python autenticação no IoT Hub com certificados X.509, consulte o SDK de IoT do Java [iothub_client_sample_x509.py](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py) exemplo. Alguns dos principais linhas do exemplo estão incluídos aqui para demonstrar o processo de autenticação.

Ao definir a cadeia de caracteres de conexão para seu dispositivo downstream, use o nome de host do dispositivo de gateway do IoT Edge para o **HostName** parâmetro. O nome do host pode ser encontrado no arquivo de config. YAML do dispositivo de gateway. 

```python
# String containing Hostname, Device Id in the format:
# "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
CONNECTION_STRING = "[Device Connection String]"

X509_CERTIFICATE = (
    "-----BEGIN CERTIFICATE-----""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "...""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXX""\n"
    "-----END CERTIFICATE-----"
)

X509_PRIVATEKEY = (
    "-----BEGIN RSA PRIVATE KEY-----""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "...""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    "-----END RSA PRIVATE KEY-----"
)

def iothub_client_init():
    # prepare iothub client
    client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    # this brings in x509 privateKey and certificate
    client.set_option("x509certificate", X509_CERTIFICATE)
    client.set_option("x509privatekey", X509_PRIVATEKEY)

    return client
```

#### <a name="java"></a>Java

Para obter um exemplo de um programa Java autenticação no IoT Hub com certificados X.509, consulte o SDK de IoT do Java [SendEventX509.java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py) exemplo. Alguns dos principais linhas do exemplo estão incluídos aqui para demonstrar o processo de autenticação.

Ao definir a cadeia de caracteres de conexão para seu dispositivo downstream, use o nome de host do dispositivo de gateway do IoT Edge para o **HostName** parâmetro. O nome do host pode ser encontrado no arquivo de config. YAML do dispositivo de gateway. 

```java
//PEM encoded representation of the public key certificate
private static String publicKeyCertificateString =
    "-----BEGIN CERTIFICATE-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END CERTIFICATE-----\n";

//PEM encoded representation of the private key
private static String privateKeyString =
    "-----BEGIN EC PRIVATE KEY-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END EC PRIVATE KEY-----\n";

DeviceClient client = new DeviceClient(connectionString, protocol, publicKeyCertificateString, false, privateKeyString, false);
```

## <a name="next-steps"></a>Próximas etapas

Ao concluir este artigo, você deve ter um dispositivo IoT Edge funcionando como um gateway transparente e um dispositivo downstream registrado com um hub IoT. Em seguida, você precisará configurar seus dispositivos downstream para o dispositivo de gateway de confiança e enviar mensagens para ela. Para obter mais informações, consulte [Cnectar um dispositivo downstream a um gateway do Azure IoT Edge](how-to-connect-downstream-device.md).

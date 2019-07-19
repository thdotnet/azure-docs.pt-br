---
title: Instalar o Azure IoT Edge no Linux ARM32 | Microsoft Docs
description: Instruções de instalação do Azure IoT Edge no Linux em dispositivos ARM32, como um Raspberry PI
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: kgremban
ms.openlocfilehash: 72349597a2831a64cb11ff66b30e6948f34cc921
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68224703"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Instalar o tempo de execução do Azure IoT Edge no Linux (ARM32v7 / armhf)

O tempo de execução do Azure IoT Edge é o que transforma um dispositivo em um dispositivo do IoT Edge. O tempo de execução pode ser implantado em dispositivos pequenos como um Raspberry Pi ou grandes como um servidor industrial. Após um dispositivo ser configurado com o tempo de execução do IoT Edge, você poderá começar a implantar a lógica de negócios da nuvem nele. 

Para saber mais sobre como funciona o tempo de execução do IoT Edge e quais componentes estão incluídos, veja [Entender o tempo de execução do Azure IoT Edge e sua arquitetura](iot-edge-runtime.md).

Este artigo lista as etapas para instalar o Azure IoT Edge Runtime em um dispositivo Linux ARM32v7/armhf IoT Edge. Essas etapas funcionariam, por exemplo, para dispositivos Raspberry Pi. Para obter uma lista de sistemas operacionais ARM32 com suporte, consulte [Azure IOT Edge sistemas com suporte](support.md#operating-systems). 

>[!NOTE]
>Os pacotes nos repositórios de software do Linux estão sujeitos aos termos de licença localizados em cada pacote (/usr/share/doc/ *package-name* ). Leia os termos da licença antes de usar o pacote. Sua instalação e uso do pacote constitui sua aceitação desses termos. Se você não concorda com os termos de licença, não utilize o pacote.

## <a name="install-the-latest-version"></a>Instalar a versão mais recente

Use as seções a seguir para instalar a versão mais recente do serviço de Azure IoT Edge em seus dispositivos ARM do Linux. 

### <a name="install-the-container-runtime"></a>Instalar o tempo de execução de contêiner

O Azure IoT Edge depende de um tempo de execução de contêiner [compatível com OCI](https://www.opencontainers.org/). Para cenários de produção, é altamente recomendável usar o mecanismo [baseado em Moby](https://mobyproject.org/) fornecido abaixo. É o único mecanismo de contêiner oficialmente com suporte do Azure IoT Edge. Imagens de contêiner do Docker CE/EE são compatíveis com o tempo de execução baseado em Moby.

Os comandos a seguir instalam o mecanismo baseado em Moby e a CLI (interface de linha de comando). A CLI é útil para desenvolvimento, mas é opcional para implantações de produção.

```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f
```

### <a name="install-the-iot-edge-security-daemon"></a>Instalar o daemon de segurança do IoT Edge

O **daemon de segurança do IOT Edge** fornece e mantém os padrões de segurança no dispositivo IOT Edge. O daemon é iniciado a cada inicialização e inicializa o dispositivo iniciando o restante do tempo de execução do IoT Edge. 


```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

Quando IoT Edge for instalado com êxito, a saída solicitará que você atualize o arquivo de configuração. Siga as etapas na seção [Configurar o daemon de segurança Azure IOT Edge](#configure-the-azure-iot-edge-security-daemon) para concluir o provisionamento do seu dispositivo. 

## <a name="install-a-specific-version"></a>Instalar uma versão específica

Se você quiser instalar uma versão específica do Azure IoT Edge, poderá direcionar os arquivos de componente diretamente do repositório IoT Edge GitHub. Use os mesmos `curl` comandos listados nas seções anteriores para obter todos os componentes de IOT Edge em seu dispositivo: o mecanismo Moby e a CLI, o libiothsm e, finalmente, o daemon de segurança IOT Edge. A única diferença é que você substitui as URLs de **aka.ms** por links que apontam diretamente para a versão de cada componente que você deseja usar.

Navegue até o [Azure IOT Edge versões](https://github.com/Azure/azure-iotedge/releases)e localize a versão de lançamento que você deseja direcionar. Expanda a seção **ativos** para a versão e escolha os arquivos que correspondem à arquitetura do dispositivo IOT Edge. Cada versão de IoT Edge contém arquivos **iotedge** e **libiothsm** . Nem todas as versões incluem **Moby-Engine** ou **Moby-CLI**. Se você ainda não tiver o mecanismo de contêiner do Moby instalado, examine as versões mais antigas até encontrar uma que inclua os componentes do Moby. 

Quando IoT Edge for instalado com êxito, a saída solicitará que você atualize o arquivo de configuração. Siga as etapas na próxima seção para concluir o provisionamento do seu dispositivo. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configurar o daemon de segurança do Azure IoT Edge

Configure o tempo de execução do IoT Edge para vincular seu dispositivo físico com uma identidade do dispositivo existente em um Hub IoT do Azure. 

O daemon pode ser configurado usando o arquivo de configuração no `/etc/iotedge/config.yaml`. O arquivo é protegido por gravação por padrão, portanto, talvez você precise de permissões elevadas para editá-lo.

Um único dispositivo Azure IoT Edge pode ser provisionado manualmente usando uma cadeia de caracteres de conexões de dispositivo fornecida pelo Hub IoT. Ou, você pode usar o serviço de provisionamento de dispositivo para provisionar automaticamente os dispositivos, que é útil quando você tem muitos dispositivos para provisionar. Dependendo de sua escolha de provisionamento, escolha o script de instalação apropriado. 

### <a name="option-1-manual-provisioning"></a>Opção 1: provisionamento manual

Para provisionar manualmente um dispositivo, você precisa fornecê-lo com uma [cadeia de conexão de dispositivo](how-to-register-device-portal.md) que você pode criar registrando um novo dispositivo IOT Edge no Hub IOT.

Abra o arquivo de configuração. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Localize as configurações de provisionamento do arquivo e remova a marca de comentário da seção **configuração de provisionamento manual** . Atualizar o valor de **device_connection_string** com a cadeia de caracteres de conexão do dispositivo IoT Edge. Verifique se qualquer outra seção de provisionamento foi comentada.

```yaml
# Manual provisioning configuration
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
# DPS TPM provisioning configuration
# provisioning:
#   source: "dps"
#   global_endpoint: "https://global.azure-devices-provisioning.net"
#   scope_id: "{scope_id}"
#   attestation:
#     method: "tpm"
#     registration_id: "{registration_id}"
```

Salve e feche o arquivo. 

`CTRL + X`, `Y`, `Enter`

Depois de inserir as informações de provisionamento no arquivo de configuração, reinicie o daemon:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Opção 2: provisionamento automático

Para provisionar um dispositivo automaticamente, [configure o Serviço de provisionamento de dispositivos e recupere a ID de registro do dispositivo](how-to-auto-provision-simulated-device-linux.md). Há vários mecanismos de atestado com suporte pelo IoT Edge ao usar o provisionamento automático, mas os requisitos de hardware também afetam suas escolhas. Por exemplo, os dispositivos Raspberry Pi não vêm com um chip Trusted Platform Module (TPM) por padrão.

Abra o arquivo de configuração. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Localize as configurações de provisionamento do arquivo e remova a marca de comentário da seção apropriada para seu mecanismo de atestado. Ao usar o atestado de TPM, por exemplo, atualize os valores de **scope_id** e **registration_id** com os valores de seu serviço de provisionamento de dispositivos no Hub IoT e seu dispositivo IOT Edge com TPM, respectivamente.

```yaml
   # Manual provisioning configuration
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # DPS TPM provisioning configuration
   provisioning:
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     attestation:
       method: "tpm"
       registration_id: "{registration_id}"
```

Salve e feche o arquivo. 

`CTRL + X`, `Y`, `Enter`

Depois de inserir as informações de provisionamento no arquivo de configuração, reinicie o daemon:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Verifique se a instalação bem-sucedida

Se você tiver usado as etapas de **configuração manual** na seção anterior, o tempo de execução do IoT Edge deve ser provisionado e estar em execução no seu dispositivo com êxito. Ou, se você usou as etapas de **configuração automática** , precisará concluir algumas etapas adicionais para que o tempo de execução possa registrar seu dispositivo com o Hub IOT em seu nome. Para as próximas etapas, consulte [criar e provisionar um dispositivo IOT Edge de TPM simulado em uma máquina virtual do Linux](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Você pode verificar o status do Daemon de borda IoT usando:

```bash
systemctl status iotedge
```

Examine os logs do daemon usando:

```bash
journalctl -u iotedge --no-pager --no-full
```

Além disso, lista de módulos com em execução:

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>Dicas e sugestões

Você precisa de privilégios elevados para executar comandos `iotedge`. Após instalar o tempo de execução, saia da sua máquina virtual e entre novamente para atualizar suas permissões automaticamente. Até lá, use **sudo** na frente de qualquer um dos comandos `iotedge`.

Em dispositivos com restrição de recursos, é altamente recomendável que a variável de ambiente *OptimizeForPerformance* seja definida como *false* de acordo com as instruções no [guia de solução de problemas](troubleshoot.md#stability-issues-on-resource-constrained-devices).

Se a rede tem um servidor proxy, siga as etapas em [Configurar o dispositivo do IoT Edge para comunicar-se por meio de um servidor proxy](how-to-configure-proxy-support.md).

## <a name="uninstall-iot-edge"></a>Desinstalar o IoT Edge

Para remover a instalação do IoT Edge do dispositivo Linux, use o seguinte comando em uma linha de comando. 

Remova o tempo de execução do IoT Edge. 

```bash
sudo apt-get remove --purge iotedge
```

Quando o tempo de execução do IoT Edge for removido, o contêiner criado por ele será interrompido, mas ainda existará no seu dispositivo. Exiba todos os contêineres para ver quais deles permanecem. 

```bash
sudo docker ps -a
```

Exclua os contêineres do seu dispositivo, incluindo dois contêineres de tempo de execução. 

```bash
sudo docker rm -f <container name>
```

Por fim, remova o tempo de execução do contêiner do seu dispositivo. 

```bash 
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Próximas etapas

Agora que você tem um dispositivo IoT Edge provisionado com o tempo de execução instalado, é possível [implantar os módulos do IoT Edge](how-to-deploy-modules-portal.md).

Se você estiver tendo problemas com o tempo de execução do IoT Edge sendo instalado corretamente, consulte a página de [solução de problemas](troubleshoot.md#stability-issues-on-resource-constrained-devices) .

Para atualizar uma instalação existente para a versão mais recente do IoT Edge, consulte [Atualizar o IoT Edge de segurança e o tempo de execução do IoT Edge](how-to-update-iot-edge.md).

---
title: Instalar o Azure IoT Edge no Linux | Microsoft Docs
description: Instruções de instalação do Azure IoT Edge em dispositivos Linux no AMD64 que executam o Ubuntu
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: bbab0d8d0947c18cf8e6c178d12fdbd7b335d2b6
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485893"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Instalar o tempo de execução do Azure IoT Edge no Linux (x64)

O tempo de execução do Azure IoT Edge é o que transforma um dispositivo em um dispositivo do IoT Edge. O tempo de execução pode ser implantado em dispositivos pequenos como um Raspberry Pi ou grandes como um servidor industrial. Após um dispositivo ser configurado com o tempo de execução do IoT Edge, você poderá começar a implantar a lógica de negócios da nuvem nele.

Para obter mais informações, consulte [entender o tempo de execução do Azure IoT Edge e sua arquitetura](iot-edge-runtime.md).

Este artigo lista as etapas para instalar o tempo de execução do Azure IoT Edge no seu Ubuntu Linux x64 (Intel/AMD) dispositivo IoT Edge. Consulte a [sistemas com suporte do Azure IoT Edge](support.md#operating-systems) para obter uma lista dos sistemas operacionais de AMD64.

> [!NOTE]
> Os pacotes nos repositórios de software do Linux estão sujeitos aos termos de licença localizados em cada pacote (/usr/share/doc/ *package-name* ). Leia os termos da licença antes de usar o pacote. Sua instalação e uso do pacote constitui sua aceitação desses termos. Se você não concorda com os termos de licença, não utilize o pacote.

## <a name="install-the-latest-version"></a>Instalar a versão mais recente

Use as seções a seguir para instalar a versão mais recente do serviço do Azure IoT Edge em seus dispositivos. 

### <a name="register-microsoft-key-and-software-repository-feed"></a>Registrar a chave da Microsoft e o feed do repositório de software

Prepare o dispositivo do IoT Edge instalação do tempo de execução.


Instale a configuração do repositório. Escolha o **16.04** ou **18.04** trecho de código conforme apropriado para sua versão do Ubuntu:

> [!NOTE]
> Certifique-se de que escolher o trecho de código da caixa de código correto para sua versão do Ubuntu.

* Para **Ubuntu 16.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   ```

* Para **Ubuntu 18.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
   ```
   
Copie lista gerada.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Instale a chave pública do Microsoft GPG

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-the-container-runtime"></a>Instalar o tempo de execução de contêiner

O Azure IoT Edge depende de um tempo de execução de contêiner [compatível com OCI](https://www.opencontainers.org/). Para cenários de produção, é recomendável que você use o [com base em Moby](https://mobyproject.org/) mecanismo fornecido abaixo. É o único mecanismo de contêiner oficialmente com suporte do Azure IoT Edge. Imagens de contêiner do docker CE/EE são compatíveis com o tempo de execução Moby.

Execute atualização apt.

   ```bash
   sudo apt-get update
   ```

Instale o mecanismo de Moby.

   ```bash
   sudo apt-get install moby-engine
   ```

Instale a interface de linha de comando Moby (CLI). A CLI é útil para desenvolvimento, mas é opcional para implantações de produção.

   ```bash
   sudo apt-get install moby-cli
   ```

#### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>Verifique se o kernel do Linux para compatibilidade Moby

Muitos fabricantes de dispositivos inseridos enviar imagens de dispositivo que contêm os kernels do Linux personalizados que podem estar faltando recursos necessários para a compatibilidade de tempo de execução do contêiner. Se você encontrar problemas ao instalar o recomendada [Moby](https://github.com/moby/moby) tempo de execução do contêiner, você talvez possa solucionar problemas de sua configuração de kernel Linux usando o [seleção-config](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh) script fornecido no oficial [repositório Moby Github](https://github.com/moby/moby) executando os comandos a seguir no dispositivo.

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Isso fornecerá uma saída detalhada que contém o status dos recursos de kernel que são usadas pelo tempo de execução Moby. Você desejará garantir que todos os itens sob `Generally Necessary` e `Network Drivers` estão habilitados para garantir que seu kernel é totalmente compatível com o tempo de execução Moby.  Se você tiver identificado os recursos ausentes, você pode habilitá-las recompilar seu kernel da fonte e selecionando os módulos associados para inclusão no config kernel apropriado.  Da mesma forma, se você estiver usando um gerador de configuração de kernel como defconfig ou menuconfig, você precisará localizar e ativar os respectivos recursos e recompilar seu kernel adequadamente.  Depois que você implantou seu kernel modificado mais recentemente, execute o script de configuração de verificação novamente para verificar se seus recursos identificados foi habilitados com êxito.

### <a name="install-the-azure-iot-edge-security-daemon"></a>Instalar o daemon de segurança do Azure IoT Edge

O **daemon de segurança do IoT Edge** fornece e mantém os padrões de segurança no dispositivo do IoT Edge. O daemon é iniciado a cada inicialização e inicializa o dispositivo iniciando o restante do tempo de execução do IoT Edge.

O comando de instalação também instala a versão padrão a **libiothsm** se ainda não existir.

Execute atualização apt.

   ```bash
   sudo apt-get update
   ```

Instale o daemon de segurança. O pacote está instalado em `/etc/iotedge/`.

   ```bash
   sudo apt-get install iotedge
   ```

Depois do IoT Edge é instalado com êxito, a saída solicitará que você atualize o arquivo de configuração. Siga as etapas a [configurar a segurança do Azure IoT Edge daemon](#configure-the-azure-iot-edge-security-daemon) seção para concluir o provisionamento do seu dispositivo. 

## <a name="install-a-specific-version"></a>Instalar uma versão específica

Se você quiser instalar uma versão específica do Azure IoT Edge, você pode direcionar os arquivos de componente diretamente do repositório do GitHub do IoT Edge. Use as etapas a seguir para obter todo o IoT Edge componentes em seu dispositivo: o mecanismo Moby e da CLI, o libiothsm e, finalmente, o daemon de segurança do IoT Edge.

1. Navegue até a [do Azure IoT Edge libera](https://github.com/Azure/azure-iotedge/releases)e encontrar a versão de lançamento que você deseja direcionar. 

2. Expanda o **ativos** seção para essa versão.

3. Pode haver ou não ser atualizações para o mecanismo Moby em qualquer versão especificada. Se você vir os arquivos que começam com **moby mecanismo** e **moby-cli**, use os seguintes comandos para atualizar esses componentes. Se você não vir todos os arquivos Moby e você ainda não tiver Moby instalado em seu dispositivo, volte pelos ativos versão mais antigos até que você encontrá-los. 

   1. Localizar o **moby mecanismo** arquivo que corresponda à arquitetura do seu dispositivo IoT Edge. Clique com botão direito no link de arquivo e copie o endereço do link.

   2. Use o link copiado no comando a seguir para instalar a versão do mecanismo de Moby: 

      ```bash
      curl -L <moby-engine link> -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb
      ```

   3. Localizar o **moby-cli** arquivo que corresponda à arquitetura do seu dispositivo IoT Edge. A CLI Moby é um componente opcional, mas podem ser úteis durante o desenvolvimento. Clique com botão direito no link de arquivo e copie o endereço do link. 

   4. Use o link copiado no comando a seguir para instalar a versão da CLI Moby: 

      ```bash
      curl -L <moby-cli link> -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb
      ```

4. Cada versão deve ter novos arquivos para o daemon de segurança do IoT Edge e o hsmlib. Use os comandos a seguir para atualizar esses componentes. 

   1. Localizar o **libiothsm std** arquivo que corresponda à arquitetura do seu dispositivo IoT Edge. Clique com botão direito no link de arquivo e copie o endereço do link. 

   2. Use o link copiado no comando a seguir para instalar a versão do hsmlib:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```
   
   3. Localizar o **iotedge** arquivo que corresponda à arquitetura do seu dispositivo IoT Edge. Clique com botão direito no link de arquivo e copie o endereço do link. 

   4. Use o link copiado no comando a seguir para instalar a versão do daemon de segurança do IoT Edge. 

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

Depois do IoT Edge é instalado com êxito, a saída solicitará que você atualize o arquivo de configuração. Siga as etapas na próxima seção para concluir o provisionamento do seu dispositivo. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configurar o daemon de segurança do Azure IoT Edge

Configure o tempo de execução do IoT Edge para vincular seu dispositivo físico com uma identidade do dispositivo existente em um Hub IoT do Azure.

O daemon pode ser configurado usando o arquivo de configuração no `/etc/iotedge/config.yaml`. O arquivo está protegido contra gravação por padrão, permissões elevadas talvez sejam necessárias para editá-lo.

Um único dispositivo Azure IoT Edge pode ser provisionado manualmente usando uma cadeia de caracteres de conexões de dispositivo fornecida pelo Hub IoT. Ou, você pode usar o serviço de provisionamento de dispositivo para provisionar automaticamente os dispositivos, que é útil quando você tem muitos dispositivos para provisionar. Dependendo de sua escolha de provisionamento, escolha o script de instalação apropriado.

### <a name="option-1-manual-provisioning"></a>Opção 1: provisionamento manual

Para provisionar um dispositivo manualmente, é necessário fornecer a ele uma [cadeia de conexão do dispositivo](how-to-register-device-portal.md), que poderá ser criada registrando um novo dispositivo no Hub IoT.

Abra o arquivo de configuração.

```bash
sudo nano /etc/iotedge/config.yaml
```

Localize a seção de provisionamento do arquivo. Remova os **manual** modo de provisionamento e verifique se o modo de provisionamento de dps é comentado. Atualizar o valor de **device_connection_string** com a cadeia de caracteres de conexão do dispositivo IoT Edge.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning:
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

Salve e feche o arquivo.

   `CTRL + X`, `Y`, `Enter`

Depois de inserir as informações de provisionamento no arquivo de configuração, reinicie o daemon:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Opção 2: provisionamento automático

Para provisionar um dispositivo automaticamente, [configure o Serviço de provisionamento de dispositivos e recupere a ID de registro do dispositivo](how-to-auto-provision-simulated-device-linux.md). O provisionamento automático funciona apenas com dispositivos que têm um chip TPM (Trusted Platform Module). Por exemplo, dispositivos Raspberry Pi não são fornecidos com o TPM por padrão.

Abra o arquivo de configuração.

```bash
sudo nano /etc/iotedge/config.yaml
```

Localize a seção de provisionamento do arquivo. Remova os **dps** modo e comente a seção manual de provisionamento. Atualize os valores de **scope_id** e **registration_id** com os valores de seu Serviço de Provisionamento de Dispositivos no Hub IoT e o dispositivo do IoT Edge com o TPM.

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning:
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Salve e feche o arquivo.

   `CTRL + X`, `Y`, `Enter`

Depois de inserir as informações de provisionamento no arquivo de configuração, reinicie o daemon:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Verifique se a instalação bem-sucedida

Se você tiver usado as etapas de **configuração manual** na seção anterior, o tempo de execução do IoT Edge deve ser provisionado e em execução no seu dispositivo com êxito. Se você tiver usado as etapas de **configuração automática**, você precisará concluir algumas etapas adicionais para que o tempo de execução pode registrar seu dispositivo no hub IoT em seu nome. Para as próximas etapas, consulte [criar e provisionar um dispositivo simulado do IoT Edge do TPM em uma máquina virtual Linux](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

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

Em dispositivos com restrição de recursos, é altamente recomendável que a variável de ambiente *OptimizeForPerformance* seja definida como *false* de acordo com as instruções no [guia de solução de problemas](troubleshoot.md).

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

Se você estiver tendo problemas com o tempo de execução do IoT Edge instalado corretamente, confira a [solução de problemas](troubleshoot.md) página.

Para atualizar uma instalação existente para a versão mais recente do IoT Edge, consulte [Atualizar o IoT Edge de segurança e o tempo de execução do IoT Edge](how-to-update-iot-edge.md).

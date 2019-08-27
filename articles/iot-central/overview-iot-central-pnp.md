---
title: O que é Azure IoT Central | Microsoft Docs
description: O Azure IoT Central é uma solução de SaaS de ponta a ponta que pode ser usada para compilar e gerenciar sua solução de IoT personalizada. Este artigo fornece uma visão geral dos recursos do Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 07/06/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 0675be988e7e9560560dd07338563c9fa22fcb4c
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69881645"
---
# <a name="what-is-azure-iot-central-preview-features"></a>O que é o Azure IoT Central (versão prévia dos recursos)?

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

> [!WARNING]
> As funcionalidades do [IoT Plug and Play](https://aka.ms/iot-pnp-docs) no Azure IoT Central estão atualmente em versão prévia pública. Não use um aplicativo do IoT Central habilitado para o IoT Plug and Play em cargas de trabalho de produção. Para ambientes de produção, use um aplicativo do IoT Central criado com base em um modelo de aplicativo atual, em disponibilidade geral.

O Azure IoT Central é uma solução de software como um serviço de IoT totalmente gerenciado que facilita a criação de produtos que conectam os mundos físicos e digitais. É possível dar vida à sua visão de produto conectado:

- Derivando novos insights de dispositivos conectados para possibilitar produtos e experiências melhores para seus clientes.
- Criando novas oportunidades de negócios para sua organização.

Azure IoT Central, em comparação com um projeto de IoT típico:

- Reduz a sobrecarga de gerenciamento.
- Reduz as sobrecargas e os custos operacionais.
- Facilita a personalização do seu aplicativo e trabalha com:
  - Tecnologias líderes do setor, como [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) e [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/).
  - Recursos de segurança de nível empresarial, como a criptografia de ponta a ponta.

O vídeo a seguir apresenta uma visão geral do Azure IoT Central:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Microsoft-IoT-Central-intro-walkthrough/Player]

Este artigo descreve os seguintes tópicos do Azure IoT Central:

- As personas típicas associadas a um projeto.
- Como criar seu aplicativo.
- Como conectar seus dispositivos ao seu aplicativo
- Como gerenciar seu aplicativo.

## <a name="known-issues"></a>Problemas conhecidos

> [!Note]
> Esses problemas conhecidos só se aplicam ao aplicativo do IoT Central Versão prévia.

- As regras não dão suporte a todas as ações (somente email).
- Para tipos complexos – regras, análise e grupos de dispositivos não são compatíveis.
- A exportação de dados contínua não é compatível com o formato Avro.
- Os dispositivos simulados não são compatíveis com todos os tipos complexos.
- Atualmente, não há compatibilidade com GeoJSON.
- Atualmente, não há compatibilidade com peças de mapa.
- Os trabalhos não são compatíveis com tipos complexos.
- Não há compatibilidade com tipos de esquema de matriz.
- Não há compatibilidade com exportação de modelo de aplicativo nem com cópia de aplicativo.
- Somente o SDK do dispositivo C e os SDKs do serviço e do dispositivo Node.js são compatíveis.
- Ele só está disponível em regiões selecionadas.

## <a name="personas"></a>Personas

A documentação do Azure IoT Central refere-se a quatro personas que interagem com um aplicativo do Azure IoT Central:

- Um _construtor_ é responsável pela definição dos tipos de dispositivos que se conectam ao aplicativo e pela personalização do aplicativo para o operador.
- Um _operador_ gerencia os dispositivos conectados ao aplicativo.
- Um _administrador_ é responsável por tarefas administrativas, como gerenciar usuários e funções dentro do aplicativo.
- Um _desenvolvedor de dispositivo_ cria o código que é executado em um dispositivo conectado ao seu aplicativo.

## <a name="create-your-azure-iot-central-application"></a>Criar seu aplicativo do Azure IoT Central

Como um construtor, você usa o Azure IoT Central para criar uma solução de IoT personalizada e hospedada na nuvem para sua organização. Uma solução de IoT personalizada normalmente consiste em:

- Um aplicativo baseado em nuvem que recebe telemetria de seus dispositivos e permite o gerenciamento dos dispositivos.
- Vários dispositivos executando o código personalizado conectado ao seu aplicativo baseado em nuvem.

É possível implantar rapidamente um novo aplicativo do Azure IoT Central e personalizá-lo no navegador de acordo com seus requisitos específicos. Como um construtor, você usa as ferramentas baseadas na web para criar um _modelo de dispositivo_ para os dispositivos que se conectam ao seu aplicativo. Um modelo de dispositivo é um blueprint que define as características e o comportamento de um tipo de dispositivo, como:

- A telemetria que ele envia.
- As propriedades de negócios que um operador pode modificar.
- As propriedades do dispositivo que são definidas por um dispositivo e são somente leitura no aplicativo.
- Propriedades definidas por um operador que determina o comportamento do dispositivo.

Este modelo de dispositivo inclui:

- Um _modelo de funcionalidade de dispositivo_ que descreve as funcionalidades que um dispositivo deve implementar, tais como a telemetria que ele envia e as propriedades que ele relata.
- Propriedades de nuvem que não são armazenadas no dispositivo.
- Personalizações, painéis e formulários que fazem parte do aplicativo do IoT Central.

### <a name="create-device-templates"></a>Criar modelos de dispositivo

O [IoT Plug and Play](https://aka.ms/iot-pnp-docs) permite que o IoT Central integre dispositivos sem que você escreva nenhum código de dispositivo inserido. No núcleo do IoT Plug and Play está um esquema de modelo de funcionalidade do dispositivo que descreve os recursos do dispositivo. Em um aplicativo do IoT Central Versão prévia, os modelos de dispositivo usam esses modelos de funcionalidade do dispositivo de IoT Plug and Play.

Como um criador, você tem várias opções para criar modelos de dispositivo:

- Crie o modelo de dispositivo no IoT Central e, em seguida, implemente o respectivo modelo de funcionalidade do dispositivo no código do dispositivo.
- Importe um modelo de funcionalidade do dispositivo do [catálogo de dispositivos Azure Certified para IoT](https://aka.ms/iotdevcat) e adicione todas as propriedades de nuvem, personalizações e painéis de que seu aplicativo do IoT Central precisa.
- Crie um modelo de funcionalidade do dispositivo usando o Visual Studio Code. Implemente o código do dispositivo do modelo e conecte o dispositivo ao aplicativo do IoT Central. O IoT Central localiza o modelo de funcionalidade do dispositivo em um repositório e cria um modelo de dispositivo simples para você.
- Crie um modelo de funcionalidade do dispositivo usando o Visual Studio Code. Implemente o código do dispositivo do modelo. Importe manualmente o modelo de funcionalidade do dispositivo para o aplicativo do IoT Central e, em seguida, adicione todas as propriedades de nuvem, personalizações e painéis de que seu aplicativo IoT central precisa.

Como um criador, você pode usar o IoT Central para gerar código para que dispositivos de teste validem seus modelos de dispositivo.

### <a name="customize-the-ui"></a>Personalizar a interface do usuário

Como um construtor, você também pode personalizar o aplicativo do Azure IoT Central da interface do usuário para os operadores responsáveis pelo uso diário do aplicativo. As personalizações que um construtor pode fazer incluem:

- A definição do layout de propriedades e configurações em um modelo do dispositivo.
- A configuração de painéis personalizados para ajudar os operadores a descobrirem insights e resolverem problemas mais rapidamente.
- A configuração da análise personalizada para explorar os dados de série temporal de seus dispositivos conectados.

## <a name="connect-your-devices"></a>Conecte seus dispositivos

Depois que o construtor define os tipos de dispositivos que podem se conectar ao aplicativo, um desenvolvedor de dispositivo cria o código para executar os dispositivos. Como um desenvolvedor de dispositivo, você usa os [SDKs do Azure IoT](https://github.com/Azure/azure-iot-sdks) de código aberto da Microsoft para criar seu código de dispositivo. Esses SDKs têm amplo suporte a linguagem, plataforma e protocolo para atender às suas necessidades de conexão de dispositivos ao seu aplicativo do Azure IoT Central. Os SDKs ajudam a implementar os seguintes recursos do dispositivo:

- Criar uma conexão segura.
- Enviar telemetria.
- Relatar o status.
- Receber atualizações periódicas.

Para obter mais informações, consulte a postagem de blog [Os benefícios de usar os SDKs do Azure IoT e as armadilhas a serem evitadas caso não os use](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/).

## <a name="manage-your-application"></a>Gerenciar seu aplicativo

Aplicativos do Azure IoT Central são totalmente hospedados pela Microsoft, o que reduz a sobrecarga de administração do gerenciamento de seus aplicativos.

Como um operador, você pode usar o aplicativo do Azure IoT Central para gerenciar os dispositivos em sua solução do Azure IoT Central. Operadores realizam tarefas como:

- Monitorar os dispositivos conectados ao aplicativo.
- Solucionar e corrigir problemas com dispositivos.
- Provisionar novos dispositivos.

Como desenvolvedor, você pode definir regras e ações personalizadas que operem em um fluxo de dados dos dispositivos conectados. Um operador pode habilitar ou desabilitar essas regras no nível do dispositivo para controlar e automatizar tarefas dentro do aplicativo.

Os administradores gerenciam o acesso ao seu aplicativo [com funções de usuário e permissões](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="next-steps"></a>Próximas etapas

Agora que você tem uma visão geral do Azure IoT Central, aqui estão as próximas etapas sugeridas:

- Entender as diferenças entre [Azure IoT Central e aceleradores de solução do Azure IoT](overview-iot-options.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
- Familiarizar-se com a [interface do usuário do Azure IoT Central](overview-iot-central-tour-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
- Comece com a [criação de um aplicativo do Azure IoT Central](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
- Siga uma sequência de tutoriais que mostram:
  - [Como um construtor, como criar um modelo de dispositivo](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
  - [Como um construtor, como adicionar regras para automatizar a sua solução](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
  - [Como um operador, como monitorar seus dispositivos](tutorial-monitor-devices-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
  - [Como um operador, como adicionar um dispositivo à sua solução](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
- Saiba mais sobre o [IoT Plug and Play](https://aka.ms/iot-pnp-docs)

---
title: Definir um novo tipo de dispositivo no Azure IoT Central | Microsoft Docs
description: Este tutorial mostra a você, como um construtor, como definir um novo tipo de dispositivo no seu aplicativo Azure IoT Central. Você define a telemetria, estado, propriedades e comandos para o seu tipo.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 660b2ead146695657ae13444cb7936eff8224f3a
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70099510"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definir um novo tipo de dispositivo no aplicativo do Azure IoT Central (versão prévia dos recursos)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Este tutorial mostra a você, como um construtor, como usar um modelo de dispositivo para definir um novo tipo de dispositivo no seu aplicativo Microsoft Azure IoT Central. Um modelo de dispositivo define as funcionalidades do seu dispositivo. Os recursos incluem telemetria que o dispositivo envia, as propriedades do dispositivo e os comandos aos quais um dispositivo responde.

Neste tutorial, você criará um modelo de dispositivo **Sensor de Ambiente**. Um dispositivo de sensor ambiental:

* Envia telemetria como a temperatura.
* Relata propriedades específicas do dispositivo, como o nível de brilho.
* Responde a comandos como ativar e desativar.
* Relata propriedades genéricas de dispositivo como a versão de firmware e o número de série.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar um novo modelo de dispositivo.
> * Importar um modelo de funcionalidade do dispositivo.
> * Criar propriedades da nuvem.
> * Definir uma visualização para a telemetria do dispositivo.
> * Publicar seu modelo de dispositivo.
> * Criar um dispositivo simulado para seu modelo de dispositivo.
> * Exibir seu dispositivo simulado.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, você precisa de um aplicativo Azure IoT Central. Se você concluiu o início rápido [Criar um aplicativo Azure IoT Central](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json), você pode reutilizar o aplicativo criado no início rápido. Caso contrário, conclua as seguintes etapas para criar um aplicativo Azure IoT Central vazio:

1. Navegue até o site do [Gerenciador de aplicativos do Azure IoT Central](https://aka.ms/iotcentral).

1. Se você tiver uma assinatura do Azure, entre com as credenciais usadas para acessá-lo, caso contrário, entre usando uma conta Microsoft:

    ![Insira a conta da sua organização](./media/tutorial-define-device-type-pnp/sign-in.png)

1. Para começar a criar um novo aplicativo Azure IoT Central, selecione **Novo Aplicativo**.

Para criar um novo aplicativo de IoT Central do Azure que usa versão prévia dos recursos, incluindo o IoT Plug and Play:

1. Escolha **Versão de avaliação**. Você não precisa de uma assinatura do Azure para criar um aplicativo de avaliação.

    Para obter mais informações sobre assinaturas e diretórios, consulte o [criar um início rápido de aplicativo](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

1. Escolha **Aplicativo de visualização**.

1. Opcionalmente, você pode escolher um nome amigável do aplicativo, como **Ar-condicionados Contoso**. O Azure IoT Central gera um prefixo de URL exclusivo para você. Você pode alterar esse prefixo de URL para algo mais fácil de lembrar.

1. Se você estiver criando um aplicativo de avaliação, precisará fornecer seus detalhes de contato.

1. Selecione **Criar**.

    ![Página Criar aplicativo do Azure IoT Central](./media/tutorial-define-device-type-pnp/iotcentralcreate.png)

    Para obter mais informações, consulte o [Criar um início rápido de aplicativo](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

Você também precisa de uma cópia local do arquivo **EnvironmentalSensorInline. capabilitymodel.json** que contém o modelo de funcionalidade do dispositivo [IoT Plug and Play](../iot-pnp/overview-iot-plug-and-play.md). Você pode baixá-lo [aqui](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json). Clique com o botão direito do mouse na página e selecione **Salvar como**.

Depois de baixar o arquivo, abra-o em um editor de texto e substitua as duas instâncias de `<YOUR_COMPANY_NAME_HERE>` pelo seu nome. Use somente os caracteres a-z, A-Z, 0-9 e sublinhado.

## <a name="create-a-template"></a>Criar um modelo

Como um construtor, você pode criar e editar os modelos de dispositivo no seu aplicativo. Depois de publicar um modelo de dispositivo, você pode gerar um dispositivo simulado ou conectar dispositivos reais que implementam o modelo de dispositivo. Dispositivos simulados permitem testar o comportamento do aplicativo antes de você conectar um dispositivo real.

Para adicionar um novo modelo de dispositivo ao seu aplicativo, vá até a página **Modelos de Aplicativo**. Para fazer isso, selecione a guia **Modelos de Dispositivo** no menu de navegação esquerdo.

![Página Modelos de Dispositivo](./media/tutorial-define-device-type-pnp/devicedefinitions.png)

### <a name="add-a-device-capability-model"></a>Adicionar um modelo de funcionalidade do dispositivo

Há várias opções para a criação de um modelo de funcionalidade do dispositivo no IoT Central. Você pode optar por criar um modelo personalizado do zero, importar de um arquivo, selecionar no catálogo do dispositivo ou conectar um dispositivo IoT Plug and Play por meio de uma conexão do dispositivo-primeiro em que o modelo de funcionalidade do dispositivo foi publicado no repositório público. Neste tutorial, você importará um modelo de funcionalidade do dispositivo de um arquivo.

As etapas a seguir mostram como importar o modelo de funcionalidade para um dispositivo de **Sensor Ambiental**. Esses dispositivos enviam a telemetria, como a temperatura, para seu aplicativo:

1. Para adicionar um novo modelo de dispositivo, selecione **+ Novo** na página **Modelos de Aplicativo**.

1. Escolha **Personalizado** na lista de modelos.

1. Insira **Sensor Ambiental** como nome do seu modelo de dispositivo.

1. Escolha **Importar Modelo de Funcionalidade** para criar um modelo de funcionalidade do dispositivo de um arquivo JSON. Navegue até a pasta em que você salvou o arquivo **EnvironmentalSensorInline. capabilitymodel.json** em seu computador local. Selecione o arquivo **EnvironmentalSensorInline. capabilitymodel. json** e, em seguida, selecione **Abrir**. O modelo de funcionalidade do sensor de ambiente inclui as interfaces do **Sensor Ambiental** e de **Informações do Dispositivo**:

    ![Modelo de funcionalidade do dispositivo do sensor ambiental](./media/tutorial-define-device-type-pnp/newdevicecapabilitymodel.png)

    Essas interfaces definem os recursos de um dispositivo de **Sensor ambiental**. Os recursos incluem a telemetria que o dispositivo envia, as propriedades que o dispositivo relata e os comandos aos quais um dispositivo responde.

### <a name="add-cloud-properties"></a>Adicionar propriedades da nuvem

Um modelo de dispositivo pode incluir propriedades de nuvem. As propriedades de nuvem existem somente no aplicativo IoT Central e nunca são enviadas ou recebidas de um dispositivo.

1. Selecione **Propriedades da Nuvem** e, em seguida, **+ Adicionar Propriedade de nuvem**. Use as informações na tabela a seguir para adicionar uma propriedade de nuvem ao seu modelo de dispositivo.

    | Nome de exibição      | Tipo Semântico | Esquema |
    | ----------------- | ------------- | ------ |
    | Data do Último Serviço | Nenhum          | Data   |
    | Nome do cliente     | Nenhum          | Cadeia de caracteres |

1. Selecione **Salvar** para salvar as alterações:

    ![Propriedades da Nuvem](media/tutorial-define-device-type-pnp/cloudproperties.png)

### <a name="add-customizations"></a>Adicionar personalizações

Use personalizações quando precisar modificar uma interface ou adicionar recursos específicos de IoT Central a uma funcionalidade que não exige uma nova versão do seu modelo de funcionalidade do dispositivo. Você pode personalizar os campos quando o modelo de funcionalidade estiver em um rascunho ou em um estado publicado. Você só pode personalizar campos que não interrompem a compatibilidade de interface. Por exemplo, você pode:

- Personalizar o nome de exibição e as unidades de uma funcionalidade.
- Adicionar uma cor padrão a ser usada quando o valor for exibido em um gráfico.
- Especificar os valores iniciais, mínimos e máximos, para uma propriedade.

Você não pode personalizar o nome da funcionalidade ou o tipo de funcionalidade.


## <a name="create-views"></a>Criar exibições

Como um construtor, você pode personalizar o aplicativo para exibir informações relevantes sobre o dispositivo de sensor ambiental para um operador. Suas personalizações habilitam o operador a gerenciar os dispositivos de sensor ambiental conectados ao aplicativo. Você pode criar dois tipos de modos de exibição para que um operador use para interagir com dispositivos:

* Formulários para exibir e editar propriedades de dispositivo e de nuvem.
* Painéis para visualizar dispositivos.

### <a name="generate-default-views"></a>Gerar modos de exibição padrão

Gerar modos de exibição padrão é uma maneira rápida de começar a visualizar as informações importantes do dispositivo. Você pode ter até três modos de exibição padrão geradas para seu modelo de dispositivo:

* O modo de exibição **Comandos** permite que o operador despache comandos para seu dispositivo.
* O modo de exibição **Visão Geral** usa gráficos e métricas para exibir a telemetria do dispositivo.
* O modo de exibição **Sobre** exibe as propriedades do dispositivo.

Depois de selecionar **Gerar modos de exibição padrão**, eles serão automaticamente adicionados na seção **Modos de Exibição** do seu modelo de dispositivo.

### <a name="configure-a-view-to-visualize-devices"></a>Configurar um modo de exibição para visualizar dispositivos

Um painel de dispositivos permite que um operador visualize um dispositivo usando gráficos e métricas. Como um construtor, você pode definir quais informações são exibidas em um painel do dispositivo. Você pode definir vários painéis para os dispositivos. Para criar um painel para visualizar a telemetria do sensor ambiental, selecione **Modo de exibição** e, em seguida, **Visualizando o dispositivo**:

1. Todas as propriedades do dispositivo, propriedades de nuvem, telemetria e opções estáticas são listadas em **Propriedades**. Você pode arrastar e soltar qualquer um desses itens para a exibição. Arraste a propriedade **Nível de Brilho** para a exibição. Você pode configurar o bloco usando o ícone de engrenagem.

1. Para adicionar um gráfico que plota telemetria, selecione **Umidade** e **Temperatura**. Em seguida, selecione **Combinar**. Para exibir esse gráfico em um formato diferente, como um gráfico de pizza ou um gráfico de barras, selecione o botão **Alterar Visualização** na parte superior do bloco.

1. Selecione **Salvar** para salvar sua exibição:

Você pode adicionar mais blocos que mostram outras propriedades ou valores de telemetria. Você também pode adicionar texto estático, links e imagens. Para mover ou redimensionar um bloco no painel, mova o ponteiro do mouse sobre o bloco e solte-o para uma nova localização ou para redimensioná-lo.

### <a name="add-a-device-form"></a>Adicionar um formulário de dispositivo

Um formulário de dispositivo permite que um operador edite as propriedades de dispositivo graváveis e as propriedades de nuvem. Como um construtor, você pode definir vários formulários e escolher quais propriedades de dispositivo e de nuvem mostrar em cada formulário. Você também pode exibir propriedades de dispositivo somente leitura em um formulário.

Para criar um formulário para exibir e editar as propriedades do sensor ambiental:

1. Navegue até **Exibições** no modelo **Sensor Ambiental**. Selecione o bloco **Editando dados de Dispositivo e de Nuvem** para adicionar uma nova exibição.

1. Insira o nome do formulário **Propriedades do Sensor Ambiental**.

1. Arraste as propriedades da nuvem **Nome do cliente** e **Última data de serviço** até a seção existente no formulário.

1. Selecione o **Nível do brilho** e as propriedades do dispositivo **Estado do dispositivo**. Em seguida, selecione **Adicionar Seção**. Edite o título da seção para ser **Propriedades do Sensor**. Escolha **Aplicar**.

1. Selecione as propriedades **Modelo do dispositivo**, **Versão de software**, **Fabricante** e **Fabricante do processador** do dispositivo. Em seguida, selecione **Adicionar Seção**. Edite o título da seção para ser **Propriedades do Dispositivo**. Escolha **Aplicar**.

1. Selecione **Salvar** para salvar sua exibição.

## <a name="publish-device-template"></a>Publicar um modelo de dispositivo

Antes de criar um sensor ambiental simulado ou conectar um sensor ambiental real, você precisa publicar o modelo do dispositivo.

Para publicar um modelo de dispositivo:

1. Vá para seu modelo de dispositivo na página **Modelos de Dispositivo**.

1. Selecione **Publicar**.

1. Na caixa de diálogo **Publicar um Modelo de Dispositivo** escolha **Publicar**:

    ![Modelo de publicação](media/tutorial-define-device-type-pnp/publishedmodel.png)

Depois que um modelo de dispositivo é publicado, ele fica visível na página **Dispositivos** e para o operador. Em um modelo de dispositivo publicado, você não pode editar um modelo de funcionalidade do dispositivo sem criar uma nova versão. No entanto, você pode fazer atualizações nas propriedades de nuvem, nas personalizações e nos modos de exibição, em um modelo de dispositivo publicado sem controle de versão. Depois de fazer qualquer alteração, selecione **Publicar** para enviar por push essas alterações para o operador.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

* Criar um novo modelo de dispositivo
* Importar um modelo de funcionalidade do dispositivo.
* Criar propriedades da nuvem.
* Criar personalizações.
* Definir uma visualização para a telemetria do dispositivo.
* Publicar seu modelo de dispositivo.

Agora que você criou um modelo de dispositivo no seu aplicativo do Azure IoT Central, estas são as próximas etapas sugeridas:

> [!div class="nextstepaction"]
> [Adicionar dispositivo](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

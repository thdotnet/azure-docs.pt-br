---
title: Monitorar seus dispositivos no Azure IoT Central | Microsoft Docs
description: Como um operador, use o seu aplicativo Azure IoT Central para monitorar seus dispositivos.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: cf50fd80bdbce5895bd1da39700d1c6e4cdcc230
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878777"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices-preview-features"></a>Tutorial: Use o Azure IoT Central para monitorar seus dispositivos (versão prévia dos recursos)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Este tutorial mostra a você, como um operador, como usar o seu aplicatio Azure IoT Central para monitorar seus dispositivos e alterar as configurações.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Receber uma notificação
> * Investigar um problema
> * Resolver um problema

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, o construtor deve concluir os três tutorial para construtores para criar o aplicativo Azure IoT Central:

* [Definir um novo tipo de dispositivo](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
* [Adicionar um dispositivo](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
* [Configurar regras e ações para o seu dispositivo](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

## <a name="receive-a-notification"></a>Receber uma notificação

O Azure IoT Central envia notificações sobre dispositivos como mensagens de email. O construtor adicionou uma regra para enviar uma notificação quando a temperatura em um ar-condicionado conectado excede um limite. Verifique os emails enviados para a conta escolhida pelo construtor para receber notificações.

Abra a mensagem de email recebida no final do tutorial [Configurar regras e ações para o seu dispositivo](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json). No email, selecione o link para o dispositivo:

![Email de notificação de alerta](media/tutorial-monitor-devices-pnp/email.png)

Exibição **Painel** para o dispositivo simulado que você criou nos tutoriais anteriores abre no navegador:

![Dispositivo que disparou a mensagem de email com a notificação](media/tutorial-monitor-devices-pnp/dashboard.png)

## <a name="investigate-an-issue"></a>Investigar um problema

Como um operador, você pode exibir informações sobre o dispositivo nas páginas **Painel**, **Propriedades do Sensor Ambiental** e **Comandos**. O construtor personalizou as páginas **Painel** e **Propriedades do Sensor Ambiental** para exibir informações importantes sobre um dispositivo de sensor ambiental conectado.

Escolha o modo de exibição de **Painel** para ver informações sobre o dispositivo.

O gráfico no painel mostra um gráfico de temperatura do dispositivo. Você também pode ver a temperatura de destino atual para o dispositivo no bloco **Propriedades do destino**. Você decide se a temperatura de destino está muito alta.

## <a name="remediate-an-issue"></a>Resolver um problema

Para fazer uma alteração no dispositivo, use a página **Propriedades do Sensor Ambiental**:

1. Escolha **Propriedades do Sensor Ambiental**. Altere o **Nível de Brilho** para 10. Escolha **Salvar** para atualizar o dispositivo. Quando o dispositivo confirma as alterações de configuração, o status da propriedade muda para **sincronizado**:

    ![Atualizar configurações](media/tutorial-monitor-devices-pnp/change-settings.png)

2. Escolha **Painel** e verifique o novo valor da propriedade:

    ![Painel do dispositivo atualizado](media/tutorial-monitor-devices-pnp/new-settings.png)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

* Receber uma notificação
* Investigar um problema
* Resolver um problema

Agora que você sabe como monitorar o dispositivo, a próxima etapa sugerida é:

> [!div class="nextstepaction"]
> [Configurar regras e ações para o seu dispositivo](tutorial-configure-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
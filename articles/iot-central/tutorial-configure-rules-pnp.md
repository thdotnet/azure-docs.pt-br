---
title: Configurar regras e ações no Azure IoT Central | Microsoft Docs
description: Este tutorial mostra a você, como um construtor, como configurar regras e ações baseadas em telemetria em seu aplicativo do Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: c2aa6edfe7ce9b670ea1015e2da72f3ecc48c9ec
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878827"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central-preview-features"></a>Tutorial: Configurar regras e ações para o dispositivo no Azure IoT Central (versão prévia dos recursos)

*Este artigo aplica-se a operadores, construtores e administradores.*

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Neste tutorial, você criará uma regra que envia um email quando a temperatura em um dispositivo de sensor ambiental exceder 90&deg; F.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar uma regra baseada em telemetria
> * Adicionar uma ação

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, você deverá concluir o tutorial [Definir um novo tipo de dispositivo no aplicativo](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) para criar o modelo de dispositivo **Sensor Ambiental** que será usado.

## <a name="create-a-telemetry-based-rule"></a>Criar uma regra baseada em telemetria

1. Para adicionar uma nova regra baseada em telemetria ao aplicativo, no menu de navegação à esquerda, selecione **Regras**.

1. Para criar uma regra, selecione **+Novo**. Em seguida, escolha **Telemetria**.

1. Insira **Temperatura ambiental** como o nome da regra.

1. Na seção **Escopo**, selecione o **Sensor Ambiental** como o modelo de dispositivo. O escopo de dispositivos ao qual esta regra se aplica. Adicione mais critérios de filtragem usando **+Filtro**.

1. Na seção **Condição**, defina o que dispara a regra. Use as seguintes informações para definir uma condição com base na telemetria de temperatura:

    | Campo                                        | Valor                             |
    | -------------------------------------------- | ------------------------------    |
    | Medida                                  | Temperatura                       |
    | Operador                                     | é maior que                   |
    | Valor                                        | 90                                |

    Para adicionar mais condições, selecione **+Condição**.

    ![Criar uma condição de regra](./media/tutorial-configure-rules-pnp/condition.png)

1. Para adicionar uma ação a ser executada quando a regra for disparada, selecione **+Ação** e escolha **Email**.

1. Use as informações da seguinte tabela para definir a ação:

    | Configuração   | Valor                                             |
    | --------- | ------------------------------------------------- |
    | Para        | Seu endereço de email                                |
    | Observações     | A temperatura ambiental excedeu o limite. |

    > [!NOTE]
    > Para receber uma notificação por email, o endereço de email deve ser uma [ID de usuário no aplicativo](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json), e esse usuário deve entrar no aplicativo pelo menos uma vez.

    ![Criar uma ação de regra](./media/tutorial-configure-rules-pnp/action.png)

1. Clique em **Salvar**. A regra é listada na página **Regras**.

## <a name="test-the-rule"></a>Teste a regra

Logo depois de salvar a regra, ela passa a valer. Quando as condições definidas na regra são atendidas, o aplicativo envia uma mensagem para o endereço de email especificado na ação.

> [!NOTE]
> Depois que os testes forem concluídos, desligue a regra para interromper o recebimento de alertas na caixa de entrada.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

* Criar uma regra baseada em telemetria
* Adicionar uma ação

Agora que você definiu uma regra baseada em limite, a próxima etapa sugerida é:

> [!div class="nextstepaction"]
> [Criar uma regra de evento e configurar as notificações](howto-create-event-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

---
title: Usar modelos de aplicativo no Azure IoT Central | Microsoft Docs
description: Como um operador, saiba como usar conjuntos de dispositivos no aplicativo Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 05/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a26f70c5a61f3855a3de991072a7e84103e87b69
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499213"
---
# <a name="use-application-templates"></a>Usar modelos de aplicativo

Este artigo descreve como como um Gerenciador de soluções, criar e usar modelos de aplicativo.

Quando você cria um aplicativo do Azure IoT Central, você tem a opção de modelos de exemplo internos. Você também pode criar seus próprios modelos de aplicativo de aplicativos existentes do IoT Central. Em seguida, você pode usar seus próprios modelos de aplicativo quando você cria novos aplicativos.

Quando você cria um modelo de aplicativo, ele inclui os seguintes itens de seu aplicativo existente:

- O aplicativo painel padrão, incluindo o layout do dashboard e todos os blocos que você definiu.
- Modelos de dispositivo, incluindo medidas, configurações, propriedades, comandos e painel de controle.
- Regras. Todas as definições de regra são incluídas. No entanto, ações, exceto para as ações de email, não serão incluídas.
- Conjuntos de dispositivos, incluindo suas condições e os painéis.

> [!WARNING]
> Se um painel inclui blocos que exibem informações sobre dispositivos específicos, esses blocos mostram **o recurso solicitado não foi encontrado** no novo aplicativo. Você deve reconfigurar esses blocos para exibir informações sobre os dispositivos em seu novo aplicativo.

Quando você cria um modelo de aplicativo, ele não inclui os seguintes itens:

- Dispositivos
- Usuários
- Definições de trabalho
- Definições de exportação contínua de dados

Adicione esses itens manualmente todos os aplicativos criados a partir de um modelo de aplicativo.

## <a name="create-an-application-template"></a>Criar um modelo de aplicativo

Para criar um modelo de aplicativo de um aplicativo de IoT Central existente:

1. Vá para o **administração** seção em seu aplicativo.
1. Selecione **exportação do modelo de aplicativo**.
1. Sobre o **exportação de modelo de aplicativo** página, insira um nome e descrição para o modelo.
1. Selecione o **exportar** botão para criar o modelo de aplicativo. Agora você pode copiar o **Link compartilhável** que permita a alguém criar um novo aplicativo do modelo:

![Criar um modelo de aplicativo](media/howto-use-app-templates/create-template.png)

## <a name="use-an-application-template"></a>Usar um modelo de aplicativo

Para usar um modelo de aplicativo para criar um novo aplicativo de IoT Central, você precisa criado anteriormente **Link compartilhável**. Cole a **Link compartilhável** na barra de endereços do navegador. O **criar um aplicativo** página é exibida com o modelo de aplicativo personalizado selecionado:

![Criar um aplicativo de um modelo](media/howto-use-app-templates/create-app.png)

Selecione o plano de pagamento e preencha os outros campos no formulário. Em seguida, selecione **criar** para criar um novo aplicativo de IoT Central do modelo de aplicativo.

## <a name="manage-application-templates"></a>Gerenciar modelos de aplicativo

Sobre o **exportação do modelo de aplicativo** página, você pode excluir ou atualizar o modelo de aplicativo.

Se você excluir um modelo de aplicativo, você não pode usar o link compartilhável gerado anteriormente para criar novos aplicativos.

Para atualizar seu modelo de aplicativo, altere o nome do modelo ou a descrição sobre o **exportação do modelo de aplicativo** página. Em seguida, selecione a **exportar** novamente no botão. Essa ação gera uma nova **link compartilhável** e invalida quaisquer anterior **link compartilhável** URL.

## <a name="next-steps"></a>Próximas etapas

Agora que você aprendeu como usar modelos de aplicativo, a próxima etapa sugerida é saber como [gerenciar IoT Central do portal do Azure](howto-manage-iot-central-from-portal.md)

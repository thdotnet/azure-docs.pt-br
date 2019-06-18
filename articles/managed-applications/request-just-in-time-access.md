---
title: Habilitar e solicitar o acesso just-in-time para aplicativos gerenciados do Azure
description: Descreve como os editores de aplicativos gerenciados do Azure para solicitar acesso just-in-time para um aplicativo gerenciado.
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: ea933f5382cb42c01de523326b094c1813401132
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66481768"
---
# <a name="enable-and-request-just-in-time-access-for-azure-managed-applications"></a>Habilitar e solicitar o acesso just-in-time para aplicativos gerenciados do Azure

Os consumidores do seu aplicativo gerenciado poderão ficar relutantes em conceder acesso permanente ao grupo de recursos gerenciado. Como um publicador de um aplicativo do Gerenciador, você pode preferir que os consumidores saibam exatamente quando você precisar acessar os recursos gerenciados. Para fornecer aos clientes maior controle sobre como conceder acesso aos recursos gerenciados, os aplicativos gerenciados do Azure fornece um recurso chamado acesso de (JIT) just-in-time, que está atualmente em visualização.

Acesso JIT permite que você solicite acesso elevado para recursos de um aplicativo gerenciado para solução de problemas ou manutenção. Você sempre tem acesso somente leitura aos recursos, mas por um período de tempo específico, você pode ter maior acesso.

O fluxo de trabalho de concessão de acesso é:

1. Adicionar um aplicativo gerenciado no Marketplace e especificar que o acesso JIT está disponível.

1. Durante a implantação, o consumidor permite o acesso JIT para aquela instância do aplicativo gerenciado.

1. Após a implantação, o consumidor pode alterar as configurações para o acesso JIT.

1. Você enviar uma solicitação de acesso quando você precisar solucionar problemas ou atualizar os recursos gerenciados.

1. O consumidor aprova sua solicitação.

Este artigo aborda as ações editores para habilitar o acesso JIT e enviar solicitações. Para saber mais sobre como aprovar solicitações de acesso JIT, consulte [aprovar o acesso just-in-time em aplicativos gerenciados do Azure](approve-just-in-time-access.md).

## <a name="add-jit-access-step-to-ui"></a>Adicionar etapa de acesso JIT à interface do usuário

O arquivo Createuidefinition JSON é exatamente como o arquivo de interface do usuário criado para acesso permanente, exceto que você deve incluir uma etapa que permite que os consumidores de habilitar o acesso JIT. Para saber mais sobre como publicar seu aplicativo gerenciado primeiro oferta no Azure Marketplace, consulte [aplicativos gerenciados do Azure no Marketplace](publish-marketplace-app.md).

Para dar suporte à funcionalidade JIT para sua oferta, adicione o seguinte conteúdo ao seu arquivo Createuidefinition JSON:

Em "etapas":

```json
{
    "name": "jitConfiguration",
    "label": "JIT Configuration",
    "subLabel": {
        "preValidation": "Configure JIT settings for your application",
        "postValidation": "Done"
    },
    "bladeTitle": "JIT Configuration",
    "elements": [
        {
          "name": "jitConfigurationControl",
          "type": "Microsoft.Solutions.JitConfigurator",
          "label": "JIT Configuration"
        }
    ]
}
```
 
Em "saídas":

```json
"jitAccessPolicy": "[parse(concat('{\"jitAccessEnabled\":', string(steps('jitConfiguration').jitConfigurationControl.jitEnabled), ',\"jitApprovalMode\":\"', steps('jitConfiguration').jitConfigurationControl.jitApprovalMode, '\",\"maximumJitAccessDuration\":\"', steps('jitConfiguration').jitConfigurationControl.maxAccessDuration, '\",\"jitApprovers\":', string(steps('jitConfiguration').jitConfigurationControl.approvers), '}'))]"
```

> [!NOTE]
> Acesso JIT está em visualização. O esquema de configuração de JIT pode mudar em futuras iterações.

## <a name="enable-jit-access"></a>Habilitar o acesso JIT

Ao definir sua oferta no marketplace, certifique-se de que habilitar o acesso JIT.

1. Entrar para o [portal de publicação do Cloud Partner](https://cloudpartner.azure.com).

1. Forneça valores para publicar seu aplicativo gerenciado no marketplace. Selecione **Yes** para **habilitar o acesso JIT?**

   ![Habilitar acesso Just-In-Time](./media/request-just-in-time-access/marketplace-enable.png)

Você adicionou uma etapa de configuração de JIT à sua interface do usuário e tiver habilitado o acesso JIT na oferta do marketplace. Quando os consumidores de implantar seu aplicativo gerenciado, eles podem [ativar o acesso JIT para sua instância](approve-just-in-time-access.md#enable-during-deployment).

## <a name="request-access"></a>Solicitar acesso

Quando você precisa acessar os recursos gerenciados do consumidor, você envia uma solicitação para uma função específica, o tempo e duração. O consumidor deve, em seguida, aprovar a solicitação.

Para enviar uma solicitação de acesso JIT:

1. Selecione **acesso JIT** para o aplicativo gerenciado, você precisa acessar.

1. Selecione **funções qualificadas**e selecione **ativar** na coluna de ação para a função que você deseja.

   ![Ativar a solicitação de acesso](./media/request-just-in-time-access/send-request.png)

1. Sobre o **ativar função** de formulário, selecione uma hora de início e a duração de sua função como ativa. Selecione **ativar** para enviar a solicitação.

   ![Ativar o acesso](./media/request-just-in-time-access/activate-access.png) 

1. Exiba as notificações para ver se a nova solicitação JIT é enviada com êxito para o consumidor.

   ![Notificação](./media/request-just-in-time-access/in-progress.png)

   Agora, você deve aguardar o consumidor [aprovar sua solicitação](approve-just-in-time-access.md#approve-requests).

1. Para exibir o status de todas as solicitações JIT de um aplicativo gerenciado, selecione **acesso JIT** e **histórico de solicitação**.

   ![Exibir status](./media/request-just-in-time-access/view-status.png)

## <a name="known-issues"></a>Problemas conhecidos

A ID da entidade da conta que solicita acesso JIT deve ser incluída explicitamente na definição de aplicativo gerenciado. A conta só não pode ser incluída por meio de um grupo que é especificado no pacote. Essa limitação será corrigida em uma versão futura.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre como aprovar as solicitações de acesso JIT, consulte [aprovar o acesso just-in-time em aplicativos gerenciados do Azure](approve-just-in-time-access.md).

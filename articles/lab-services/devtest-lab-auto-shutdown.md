---
title: Gerenciar políticas de autoshutdown no Azure DevTest Labs | Microsoft Docs
description: Saiba como definir a política de autoshutdown para um laboratório para que as máquinas virtuais serão desligadas automaticamente quando eles não estiverem em uso.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2019
ms.author: spelluru
ms.openlocfilehash: 9adf8dd4a5a3c469ed130b29308a0d828aee40bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65873984"
---
# <a name="manage-autoshutdown-policies-for-a-lab-in-azure-devtest-labs"></a>Gerenciar políticas de autoshutdown para um laboratório no Azure DevTest Labs
O Azure DevTest Labs permite que você controle o custo e minimize o desperdício nos laboratórios gerenciando políticas (configurações) para cada laboratório. Este artigo mostra como configurar a política de autoshutdown para uma conta de laboratório e definir configurações de autoshutdown para um laboratório na conta de laboratório. Para exibir como definir cada política de laboratório, confira [Definir políticas de laboratório no Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="set-auto-shutdown-policy-for-a-lab"></a>Definir a política de desligamento automático para um laboratório
Como proprietário de um laboratório, você pode configurar uma programação de desligamento para todas as VMs em seu laboratório. Ao fazer isso, você pode economizar custos com a execução de máquinas que não estão sendo usadas (ociosas). Você pode impor uma política de desligamento em todas as suas VMs de laboratório de forma centralizada, mas também salvar os usuários de seu laboratório com o esforço de configurar um cronograma para suas máquinas individuais. Esse recurso permite que você defina a política em sua programação de laboratório, desde a não oferta de controle a controle total, até seus usuários de laboratório. Como proprietário de um laboratório, você pode configurar essa política seguindo as etapas a seguir:

1. Na home page do seu laboratório, selecione **Configuração e políticas**.
2. Selecione **Política de desligamento automático** na seção **Horários** do menu à esquerda.
3. Selecione uma das opções. As seções a seguir oferecem mais detalhes sobre essas opções: Definir política se aplica somente a novas VMs criadas no laboratório e não para as VMs já existentes. 

    ![Auto desligar as opções de política](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

## <a name="configure-auto-shutdown-settings"></a>Definir as configurações de desligamento automático
A política de autoshutdown ajuda a minimizar o desperdício de laboratório, permitindo que você especifique a hora em que as VMs de serão desse laboratório desligadas.

Para exibir (e alterar) as políticas de um laboratório, siga estas etapas:

1. Entre no [Portal do Azure](https://portal.azure.com).
2. Selecione **Todos os serviços** e selecione **Laboratórios de Desenvolvimento/Teste** na lista.
3. Na lista de laboratórios, selecione o laboratório desejado.   
4. Selecione **Configuração e políticas**.

    ![Painel de configurações de política](./media/devtest-lab-set-lab-policy/policies-menu.png)
5. O laboratório **configuração e políticas** painel, selecione **autodesligamento** sob **agendas**.
   
    ![Desligamento automático da](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
6. Selecione **Ativado** para habilitar essa política e **Desativado** para desabilitá-la.
7. Se você habilitar essa política, especifique a hora (e fuso horário) para desligar todas as VMs no laboratório atual.
8. Especificar **Yes** ou **não** para a opção de enviar uma notificação 15 minutos antes do tempo de autoshutdown especificado. Se escolher **Sim**, insira um ponto de extremidade de URL de webhook ou endereço de email especificando onde você deseja que a notificação seja postada ou enviada. O usuário recebe a notificação e recebe a opção para atrasar o desligamento. Para obter mais informações, consulte o [notificações](#notifications) seção. 
9. Clique em **Salvar**.

    Por padrão, uma vez habilitada, essa política se aplicará a todas as VMs do laboratório atual. Para remover essa configuração de uma VM específica, abra o painel de gerenciamento da VM e altere seu **desligamento automático da** configuração.

### <a name="user-sets-a-schedule-and-can-opt-out"></a>O usuário define um agendamento e pode recusar
Se você definir seu laboratório para essa política, os usuários do laboratório poderão substituir ou desativar a programação do laboratório. Essa opção concede aos usuários de laboratório controle total sobre o cronograma de desligamento automático de suas VMs. Os usuários de laboratório não veem nenhuma alteração na página de programação de desligamento automático da VM.

![Auto desligar a opção de política - 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>O usuário define um agendamento e não pode recusar
Se você definir seu laboratório para essa política, os usuários do laboratório poderão substituir a programação do laboratório. No entanto, eles não podem desativar a política de desligamento automático. Essa opção garante que todas as máquinas do seu laboratório estejam em um cronograma de desligamento automático. Os usuários de laboratório podem atualizar o cronograma de desligamento automático de suas VMs e configurar notificações de desligamento.

![Auto desligar a opção de política - 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>O usuário não tem controle sobre o cronograma definido pelo administrador do laboratório
Se você definir seu laboratório para essa política, os usuários do laboratório não poderão substituir ou recusar a programação do laboratório. Essa opção oferece ao administrador do laboratório o controle completo da programação de cada máquina no laboratório. Os usuários de laboratório só podem configurar notificações de desligamento automático para suas VMs.

![Auto desligar a opção de política - 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="notifications"></a>Notificações
Depois que o desligamento automático da configurado pelo proprietário de laboratório, notificações serão enviadas para os usuários do laboratório 15 minutos antes do desligamento automático da disparado se qualquer uma das suas VMs serão afetadas. Essa opção dá a oportunidade de salvar seu trabalho antes do desligamento de usuários do laboratório. A notificação também fornece links para cada VM para as seguintes ações:

- Ignorar o desligamento automático neste momento
- Adiar o desligamento automático para uma hora ou de 2 horas, para que eles podem continuar trabalhando na VM.

Notificação é enviada pelo ponto de extremidade de gancho da web configurado ou um endereço de email especificado através de proprietários de laboratório nas configurações do autoshutdown. Webhooks permitem que você criar ou configurar integrações que se inscrever em determinados eventos. Quando um desses eventos é disparado, o DevTest Labs enviará uma carga de HTTP POST para URL configurado de forma do webhook. Para saber mais sobre os webhooks, veja [Criar um webhook ou uma função da API do Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

Recomendamos que você use ganchos da web porque eles são extensivamente suportados por vários aplicativos (por exemplo, Slack, aplicativos lógicos do Azure e assim por diante.) e permite que você implemente sua própria maneira de envio de notificações. Por exemplo, este artigo o orienta como obter notificação autoshutdown de emails usando aplicativos lógicos do Azure. Primeiro, vamos rapidamente percorrer as etapas básicas para habilitar a notificação de autoshutdown em seu laboratório.   

## <a name="create-a-logic-app-that-receives-email-notifications"></a>Criar um aplicativo lógico que recebe notificações por email
[Aplicativos lógicos do Azure](../logic-apps/logic-apps-overview.md) fornece muitos conectores out-of-the-box que torna mais fácil para integram um serviço com outros clientes, como o Office 365 e o twitter. Em alto nível, as etapas para configurar um aplicativo lógico para a notificação de email podem ser divididas em quatro fases: 

- Crie um aplicativo lógico. 
- Configure o modelo interno.
- Integrar com o cliente de email
- Obtenha a URL do Webhook.

### <a name="create-a-logic-app"></a>Criar um aplicativo lógico
Para começar, crie um aplicativo lógico em sua assinatura do Azure usando as seguintes etapas:

1. Selecione **+ criar um recurso** no menu à esquerda, selecione **integração**e selecione **aplicativo lógico**. 

    ![Novo menu do aplicativo lógico](./media/devtest-lab-auto-shutdown/new-logic-app.png)
2. Sobre o **aplicativo lógico - criar** página, siga estas etapas: 
    1. Insira um **nome** para o aplicativo lógico.
    2. Selecione sua **assinatura**do Azure.
    3. Crie um novo **grupo de recursos** ou selecione um grupo de recursos existente. 
    4. Selecione uma **local** para o aplicativo lógico. 

        ![Novo aplicativo lógico - configurações](./media/devtest-lab-auto-shutdown/new-logic-app-page.png)
3. No **notificações**, selecione **ir para o recurso** na notificação. 

    ![Ir para o recurso](./media/devtest-lab-auto-shutdown/go-to-resource.png)
4. Selecione **designer de aplicativos lógicos** sob **ferramentas de implantação** categoria.

    ![Selecione a solicitação/resposta HTTP](./media/devtest-lab-auto-shutdown/select-http-request-response-option.png)
5. Sobre o **de solicitação-resposta HTTP** página, selecione **usar este modelo**. 

    ![Selecione usar essa opção de modelo](./media/devtest-lab-auto-shutdown/select-use-this-template.png)
6. Copie o JSON a seguir para o **esquema JSON do corpo da solicitação** seção: 

    ```json
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "properties": {
            "delayUrl120": {
                "type": "string"
            },
            "delayUrl60": {
                "type": "string"
            },
            "eventType": {
                "type": "string"
            },
            "guid": {
                "type": "string"
            },
            "labName": {
                "type": "string"
            },
            "owner": {
                "type": "string"
            },
            "resourceGroupName": {
                "type": "string"
            },
            "skipUrl": {
                "type": "string"
            },
            "subscriptionId": {
                "type": "string"
            },
            "text": {
                "type": "string"
            },
            "vmName": {
                "type": "string"
            }
        },
        "required": [
            "skipUrl",
            "delayUrl60",
            "delayUrl120",
            "vmName",
            "guid",
            "owner",
            "eventType",
            "text",
            "subscriptionId",
            "resourceGroupName",
            "labName"
        ],
        "type": "object"
    }
    ```
    
    ![Esquema JSON do corpo de solicitação](./media/devtest-lab-auto-shutdown/request-json.png)
7. Selecione **+ nova etapa** no designer e siga estas etapas:
    1. Pesquise **Office 365 Outlook – enviar um email**. 
    2. Selecione **enviar um email** partir **ações**. 
    
        ![Enviar email de opção](./media/devtest-lab-auto-shutdown/select-send-email.png)
    3. Selecione **entrar** para entrar em sua conta de email. 
    4. Selecione **TO** campo e, em seguida, escolha o proprietário.
    5. Selecione **assunto**e um assunto do email de notificação de entrada. Por exemplo:  "Desligamento de máquina vmName de laboratório: labName."
    6. Selecione **corpo**e definir o conteúdo do corpo de notificação por email. Por exemplo: "vmName está programado para desligar em 15 minutos. Ignore esse desligamento, clicando em: URL. Desligamento de atraso para uma hora: delayUrl60. Desligamento de atraso de 2 horas: delayUrl120. "

        ![Esquema JSON do corpo de solicitação](./media/devtest-lab-auto-shutdown/email-options.png)
1. Selecione **Salvar** na barra de ferramentas. Agora, você pode copiar o **URL de HTTP POST**. Selecione o botão de cópia para copiar a URL para a área de transferência. 

    ![URL do WebHook](./media/devtest-lab-auto-shutdown/webhook-url.png)

## <a name="next-steps"></a>Próximas etapas
Para saber como definir todas as políticas, consulte [definir políticas de laboratório no Azure DevTest Labs](devtest-lab-set-lab-policy.md).

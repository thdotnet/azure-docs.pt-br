---
title: Criar um host bastião do Azure | Microsoft Docs
description: Neste artigo, saiba como criar um host bastião do Azure
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 5fbd64d2f001a0fbe91de2d954889816a1819479
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67339445"
---
# <a name="create-an-azure-bastion-host-preview"></a>Criar um host de bastiões do Azure (visualização)

Este artigo mostra como criar um host bastião do Azure. Depois que você provisiona o serviço de bastiões do Azure em sua rede virtual, a experiência perfeita de RDP/SSH está disponível para todas as VMs na mesma rede virtual. Essa implantação é feita por rede virtual, e não por assinatura/conta nem por máquina virtual.

Há duas maneiras que você pode criar um recurso de host de bastiões:

* Crie um recurso de bastiões usando o portal do Azure.
* Crie um recurso de bastiões no portal do Azure, usando as configurações de VM existentes.

> [!IMPORTANT]
> Essa versão prévia pública é fornecida sem um SLA e não deve ser usada para cargas de trabalho de produção. Determinados recursos podem não ter suporte, podem ter restrição ou podem não estar disponíveis em todos os locais do Azure. Veja os [Termos de Uso Adicionais para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter detalhes.
>

## <a name="before-you-begin"></a>Antes de começar

A visualização pública é limitada a regiões pública do Azure a seguir:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

Para participar desta versão prévia, você precisa registrar. Use estas etapas para se registrar para a visualização:

[!INCLUDE [register](../../includes/bastion-preview-register-include.md)]

## <a name="createhost"></a>Criar um host bastião

Esta seção ajuda você a criar um novo recurso de bastiões do Azure do portal do Azure.

1. Na home page na [ portal do Azure – versão prévia](https://aka.ms/BastionHost), clique em **+ criar um recurso**. Verifique se que você usa o link fornecido para acessar o portal para essa visualização, não o portal do Azure regular.

1. No **New** página, no *pesquisar no Marketplace* , digite **bastiões**, em seguida, clique em **Enter** para obter os resultados da pesquisa.

1. Nos resultados, clique em **bastião (visualização)** . Verifique se o publicador está *Microsoft* e a categoria é *Networking*.

1. No **bastião (versão prévia)** , clique em **Create** para abrir o **criar um bastiões** página.

1. Sobre o **criar um bastiões** página, configure um novo recurso de bastiões. Especifique as definições de configuração para o recurso de bastiões.

    ![criar um bastiões](./media/bastion-create-host-portal/settings.png)

    * **Assinatura**: A assinatura do Azure que você deseja usar para criar um novo recurso de bastiões.
    * **Grupo de recursos**: O grupo de recursos do Azure no qual o novo recurso de bastiões será criado em. Se você não tiver um grupo de recursos, você pode criar um novo.
    * **Nome**: O nome do novo recurso de bastiões
    * **Região**: A região pública do Azure que o recurso será criado em.
    * **Rede virtual**: A rede virtual na qual o recurso de bastiões será criado em. Você pode criar uma nova rede virtual no portal durante esse processo, caso você não tiver ou não quiser usar uma rede virtual existente. Se você estiver usando uma rede virtual existente, verifique se que a rede virtual existente tem suficiente espaço livre do endereço para acomodar os requisitos de sub-rede de bastiões.
    * **Sub-rede**: A sub-rede em sua rede virtual no qual o novo recurso de host de bastiões será implantado. Você deve criar uma sub-rede usando o valor de nome **AzureBastionSubnet**. Esse valor permite que o Azure para implantar os recursos de bastiões para qual sub-rede. Isso é diferente de uma sub-rede de Gateway. É altamente recomendável que você use pelo menos um/27 ou sub-rede maior (/ 27, / 26 e assim por diante). Criar o **AzureBastionSubnet** sem nenhum grupo de segurança de rede, rotear tabelas ou as delegações.
    * **Endereço IP público**: O IP público do recurso de bastiões em que o RDP/SSH será acessado (pela porta 443). Criar um novo IP público, ou usar um existente. O endereço IP público deve estar na mesma região que o recurso de bastiões que você está criando.
    * **Nome do endereço IP público**: O nome do recurso de endereço IP público.
    * **Endereço IP público SKU**: Previamente preenchido por padrão para **padrão**.
    * **Atribuição**: Previamente preenchido por padrão para **estático**.

1. Quando tiver terminado de especificar as configurações, clique em **revisar + criar**. Isso valida os valores. Depois que a validação é bem-sucedida, você pode começar o processo de criação.
1. Em criar uma página de bastiões, clique em **criar**.
1. Você verá uma mensagem informando que sua implantação está em andamento. Status serão exibidas nesta página conforme os recursos são criados. Demora cerca de 5 minutos para o recurso de bastiões seja criado e implantado.

## <a name="createvmset"></a>Criar um host bastião usando as configurações da VM

Se você criar um host bastião no portal usando uma VM existente, várias configurações automaticamente padrão serão correspondente à sua máquina virtual e/ou rede virtual.

1. No [versão prévia do portal](https://aka.ms/BastionHost), navegue até sua máquina virtual e clique **Connect**.

    ![Conectar-se de VM](./media/bastion-create-host-portal/vmsettings.png)

1. Na barra lateral direita, clique em **bastiões**, em seguida, **uso bastiões**.

    ![Bastion](./media/bastion-create-host-portal/vmbastion.png)

1. Na página de bastiões, preencha os campos de configurações a seguir:

    * **Nome**: O nome do host bastião que você deseja criar.
    * **Sub-rede**: A sub-rede dentro de sua rede virtual à qual bastiões recurso será implantado. A sub-rede deve ser criada com o nome **AzureBastionSubnet**. Isso permite que o Azure para implantar o recurso de bastiões para qual sub-rede. Isso é diferente de uma sub-rede de Gateway. Clique em **configuração de sub-rede gerenciar** para criar a sub-rede de bastiões do Azure. É altamente recomendável que você use pelo menos um/27 ou sub-rede maior (/ 27, / 26, etc.). Criar o **AzureBastionSubnet** sem nenhum grupo de segurança de rede, rotear tabelas ou as delegações. Clique em **criar** para criar a sub-rede, continue com as configurações de Avançar.

      ![Bastion](./media/bastion-create-host-portal/subnet.png)
      
    * **Endereço IP público**: O IP público do recurso de bastiões em que o RDP/SSH será acessado (pela porta 443). Criar um novo IP público, ou usar um existente. O endereço IP público deve estar na mesma região que o recurso de bastiões que você está criando.
    * **Nome do endereço IP público**: O nome do recurso de endereço IP público.
1. Na tela de validação, clique em **criar**. Aguarde cerca de 5 minutos para o recurso de bastiões seja criado e implantado.

## <a name="next-steps"></a>Próximas etapas

Leia o [bastiões perguntas Frequentes](bastion-faq.md)
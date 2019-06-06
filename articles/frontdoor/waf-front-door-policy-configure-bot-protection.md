---
title: Configure a proteção de bot do firewall do aplicativo web com porta frontal do Azure (visualização)
description: Saiba o firewall do aplicativo web (WAF).
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: tyao;kumud
ms.openlocfilehash: af92f3b9049862fc19c69965f979b7dfe8c62526
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481618"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Configure a proteção de bot do firewall do aplicativo web (visualização)
Este artigo mostra como configurar regra de proteção de bot no firewall do aplicativo web do Azure (WAF) para frente usando o modelo do Azure Resource Manager, Azure PowerShell ou CLI do Azure.

Um conjunto de regras de proteção de Bot gerenciado pode ser habilitado para seu WAF executar ações personalizadas nas solicitações de endereços IP mal-intencionados conhecidos. Os endereços IP são originados de inteligência de ameaças da Microsoft do feed. [Gráfico de segurança inteligente](https://www.microsoft.com/security/operations/intelligence) impulsiona a inteligência contra ameaças da Microsoft e é usado por vários serviços, incluindo a Central de segurança do Azure.

> [!IMPORTANT]
> Conjunto de regras de proteção de bot está atualmente em visualização pública e é fornecido com um contrato de nível de serviço de visualização. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.  Veja os [Termos de Uso Adicionais para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter detalhes.

## <a name="prerequisites"></a>Pré-requisitos

Criar uma diretiva básica do WAF para frente, seguindo as instruções descritas em [criar uma política de WAF para frente do Azure usando o portal do Azure](waf-front-door-create-portal.md).

## <a name="enable-bot-protection-rule-set"></a>Habilitar o conjunto de regras de proteção do bot

1. Na página de política básica que você criou na seção anterior, sob **as configurações**, clique em **regras**.
2. Na página de detalhes, sob o **gerenciar regras** seção, no menu suspenso, selecione a caixa de seleção na frente a regra **BotProtection de visualização de 0,1**e, em seguida, selecione **salvar**acima.
    
   ![Regra de proteção do bot](./media/waf-front-door-configure-bot-protection/botprotect2.png)

## <a name="next-steps"></a>Próximas etapas

- Saiba como [monitorar o WAF](waf-front-door-monitor.md).

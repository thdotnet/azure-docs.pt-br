---
title: Novidades no Gateway de Aplicativo do Azure
description: Conheça as novidades do Gateway de Aplicativo do Azure, como as últimas notas sobre a versão, problemas conhecidos, correções de bug, funcionalidades preteridas e alterações futuras.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 4/30/2019
ms.author: victorh
ms.openlocfilehash: cdf2a1a730be657b41c7a4b2daf2f178661394b4
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64947119"
---
# <a name="whats-new-in-azure-application-gateway"></a>Novidades no Gateway de Aplicativo do Azure?

O Gateway de Aplicativo do Azure é atualizado continuamente. Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece informações sobre:

- As versões mais recentes
- Problemas conhecidos
- Correções de bug
- Funcionalidades preteridas

## <a name="new-features"></a>Novos recursos

|Recurso  |DESCRIÇÃO  |Data de adição  |
|---------|---------|---------|
|Disponibilidade geral de dimensionamento automático, redundância de zona e suporte ao VIP estático |Disponibilidade geral para a SKU de v2, que dá suporte a dimensionamento automático, redundância de zona, melhoria de desempenho, VIPs estáticos, Key Vault, cabeçalho de regravação. Confira [Documentação de dimensionamento automático do Gateway de Aplicativo](application-gateway-autoscaling-zone-redundant.md). |Abril de 2019 |
|Integração do Key Vault |Agora, o Gateway de Aplicativo dá suporte à integração com o Key Vault (em versão prévia) para certificados de servidor que estejam anexados a ouvintes habilitados para HTTPS. Confira [Terminação SSL com certificados do Key Vault](key-vault-certs.md). |Abril de 2019 |
|Cabeçalho CRUD/reescritas     |Agora é possível reescrever cabeçalhos HTTP. Consulte [Tutorial: Criar um gateway de aplicativo e reescrever cabeçalhos HTTP](tutorial-http-header-rewrite-powershell.md) para saber mais.|Dezembro de 2018|
|Configuração do WAF e lista de exclusões     |Adicionamos mais opções para ajudar você a configurar o WAF e reduzir falsos positivos. Confira [Web application firewall request size limits and exclusion lists](application-gateway-waf-configuration.md) (Limites de tamanho de solicitação e listas de exclusões do firewall do aplicativo Web) para saber mais.|Dezembro de 2018|
|Dimensionamento automático, redundância de zona e suporte ao VIP estático      |Com o SKU v2, há muitas melhorias como o Dimensionamento automático, melhor desempenho e mais. Confira [O que é o Gateway de Aplicativo do Azure?](overview.md) para saber mais.|Setembro de 2018|
|Descarregamento de conexão     |O descarregamento de conexão permite que você remova membros normalmente dos seus pools de back-end. Para saber mais, confira [Connection draining](overview.md#connection-draining) (Descarregamento de conexão).|Setembro de 2018|
|Páginas de erro personalizadas     |Com páginas de erro personalizadas, é possível criar uma página de erro dentro do formato do restante dos seus sites. Para habilitar isso, confira [Criar páginas de erro personalizadas do Gateway de Aplicativo](custom-error.md).|Setembro de 2018|
|Melhorias de métricas     |É possível obter uma melhor exibição do estado do seu Gateway de Aplicativo com métricas aprimoradas. Para habilitar as métricas no seu Gateway de Aplicativo, confira [Back-end health, diagnostic logs, and metrics for Application Gateway](application-gateway-diagnostics.md) (Integridade do back-end, logs de diagnóstico e métricas do Gateway de Aplicativo).|Junho de 2018|

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o Gateway de Aplicativo, confira [O que é o Gateway de Aplicativo do Azure?](overview.md)
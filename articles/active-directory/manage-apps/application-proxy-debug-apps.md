---
title: Depurar aplicativos de Proxy de aplicativo - Azure Active Directory | Microsoft Docs
description: Depure problemas com aplicativos de Proxy de aplicativo do Azure Active Directory (Azure AD).
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: japere
ms.openlocfilehash: d0a12bde119e9dae3f950603fac4bce060bb5f91
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172259"
---
# <a name="debug-application-proxy-application-issues"></a>Depurar problemas de aplicativos de Proxy de aplicativo 

Este artigo ajuda você a solucionar problemas com aplicativos de Proxy de aplicativo do Azure Active Directory (Azure AD). Se você estiver usando o serviço de Proxy de aplicativo para acesso remoto a um aplicativo da web local, mas se você estiver tendo problemas para se conectar ao aplicativo, use este fluxograma para depurar problemas de aplicativos. 

## <a name="before-you-begin"></a>Antes de começar

Ao solucionar problemas de Proxy de aplicativo, é recomendável que você comece com os conectores. Primeiro, siga o fluxo de solução de problemas [conector do Proxy de aplicativo depurar emite](application-proxy-debug-connectors.md) para certificar-se de conectores de Proxy de aplicativo estão configurados corretamente. Se você ainda estiver tendo problemas, retorne a este artigo para solucionar problemas do aplicativo.  

Para obter mais informações sobre o Proxy de aplicativo, consulte:

- [Acesso remoto a aplicativos locais por meio do Proxy de aplicativo](application-proxy.md)
- [Conectores de Proxy de aplicativo](application-proxy-connectors.md)
- [Instalar e registrar um conector](application-proxy-add-on-premises-application.md)
- [Solucionar problemas e mensagens de erro do Application Proxy](application-proxy-troubleshoot.md)

## <a name="flowchart-for-application-issues"></a>Fluxograma de problemas de aplicativos

Este fluxograma orienta você pelas etapas para depuração de alguns dos problemas mais comuns com a conexão para o aplicativo. Para obter detalhes sobre cada etapa, consulte a tabela o fluxograma a seguir.

![Fluxograma mostrando as etapas para depurar um aplicativo](media/application-proxy-debug-apps/application-proxy-apps-debugging-flowchart.png)

|  | Ação | Descrição | 
|---------|---------|---------|
|1 | Abra um navegador, acessar o aplicativo e insira suas credenciais | Tente usar suas credenciais para entrar no aplicativo e verificação de erros relacionados ao usuário, como [este aplicativo corporativo não pode ser acessado](application-proxy-sign-in-bad-gateway-timeout-error.md). |
|2 | Verifique se a atribuição de usuário para o aplicativo | Verifique se sua conta de usuário tem permissão para acessar o aplicativo de dentro da rede corporativa e, em seguida, entrar no aplicativo, seguindo as etapas de teste [teste o aplicativo](application-proxy-add-on-premises-application.md#test-the-application). Se os problemas de conexão persistirem, consulte [como solucionar problemas de erros de entrada](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-troubleshoot-sign-in-errors).  |
|3 | Abra um navegador e tente acessar o aplicativo | Se um erro será exibida imediatamente, verifique se o Proxy de aplicativo está configurado corretamente. Para obter detalhes sobre mensagens de erro específicas, consulte [problemas de solucionar problemas de Proxy de aplicativo e mensagens de erro](application-proxy-troubleshoot.md).  |
|4 | Verifique a configuração do domínio personalizado ou solucionar o erro | Se a página não exibe nada, certifique-se de seu domínio personalizado está configurado corretamente examinando [trabalhando com domínios personalizados](application-proxy-configure-custom-domain.md).<br></br>Se a página não carrega e será exibida uma mensagem de erro, corrija o erro fazendo referência à [problemas de solucionar problemas de Proxy de aplicativo e mensagens de erro](application-proxy-troubleshoot.md). <br></br>Se demorar mais de 20 segundos para uma mensagem de erro aparecer, pode haver problema de conectividade. Vá para o [conectores de Proxy de aplicativo depurar](application-proxy-debug-connectors.md) artigo de solução de problemas.  |
|5 | Se o problema persistir, vá para a depuração de conector | Pode haver um problema de conectividade entre o proxy e o conector ou entre o conector e o back-end. Vá para o [conectores de Proxy de aplicativo depurar](application-proxy-debug-connectors.md) artigo de solução de problemas. |
|6 | Publicar todos os recursos, marque ferramentas de desenvolvedor do navegador e corrigir os links | Verifique se que o caminho de publicação inclui as imagens necessárias, scripts e folhas de estilo para seu aplicativo. Para obter detalhes, consulte [adicionar um aplicativo local ao Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). <br></br>Usar ferramentas de desenvolvedor (ferramentas de F12 no Internet Explorer ou Microsoft Edge) e a seleção do navegador para problemas de publicação, conforme descrito em [página de aplicativo não será exibido corretamente](application-proxy-page-appearance-broken-problem.md). <br></br>Examine as opções para resolver links desfeitos no [Links na página não funcionam](application-proxy-page-links-broken-problem.md). |
|7 | Verificação de latência de rede | Se a página for carregada lentamente, saiba mais sobre as maneiras de minimizar a latência de rede no [considerações para redução da latência](application-proxy-network-topology.md#considerations-for-reducing-latency). | 
|8 | Consulte a Ajuda de solução de problemas adicionais | Se o problema persistir, localize artigos de solução de problemas adicionais no [documentação de solução de problemas de Proxy de aplicativo](application-proxy-page-appearance-broken-problem.md). |

## <a name="next-steps"></a>Próximas etapas


* [Publicar aplicativos em redes e locais separados usando grupos de conectores](application-proxy-connector-groups.md)
* [Trabalhar com servidores proxy locais existentes](application-proxy-configure-connectors-with-proxy-servers.md)
* [Solucionar erros do conector e do Proxy de Aplicativo](application-proxy-troubleshoot.md)
* [Como fazer uma instalação silenciosa do Conector de Proxy de Aplicativo Azure AD](application-proxy-register-connector-powershell.md)

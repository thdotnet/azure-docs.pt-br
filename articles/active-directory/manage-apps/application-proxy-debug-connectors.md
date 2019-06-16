---
title: Depurar conectores de Proxy de aplicativo - Azure Active Directory | Microsoft Docs
description: Depure problemas com conectores de Proxy de aplicativo do Azure Active Directory (Azure AD).
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
ms.openlocfilehash: c3088ae777fe1a64be218105d36fdb9e01d7b798
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66172229"
---
# <a name="debug-application-proxy-connector-issues"></a>Depurar problemas do conector de Proxy de aplicativo 

Este artigo ajuda você a solucionar problemas com conectores de Proxy de aplicativo do Azure Active Directory (Azure AD). Se você estiver usando o serviço de Proxy de aplicativo para acesso remoto a um aplicativo da web local, mas se você estiver tendo problemas para se conectar ao aplicativo, use este fluxograma para depurar problemas do conector. 

## <a name="before-you-begin"></a>Antes de começar

Este artigo pressupõe que você instalou o conector de Proxy de aplicativo e está enfrentando um problema. Ao solucionar problemas de Proxy de aplicativo, é recomendável que você comece com este fluxo de solução de problemas para determinar se os conectores de Proxy de aplicativo estão configurados corretamente. Se você ainda estiver tendo problemas para se conectar ao aplicativo, siga o fluxo de solução de problemas [problemas de aplicativos de Proxy de aplicativo de depuração](application-proxy-debug-apps.md).  


Para obter mais informações sobre o Proxy de aplicativo e usando seus conectores, consulte:

- [Acesso remoto a aplicativos locais por meio do Proxy de aplicativo](application-proxy.md)
- [Conectores de Proxy de aplicativo](application-proxy-connectors.md)
- [Instalar e registrar um conector](application-proxy-add-on-premises-application.md)
- [Solucionar problemas e mensagens de erro do Application Proxy](application-proxy-troubleshoot.md)

## <a name="flowchart-for-connector-issues"></a>Fluxograma de problemas do conector

Este fluxograma orienta você pelas etapas para depuração de alguns dos problemas mais comuns do conector. Para obter detalhes sobre cada etapa, consulte a tabela o fluxograma a seguir.

![Fluxograma mostrando as etapas para depurar um conector](media/application-proxy-debug-connectors/application-proxy-connector-debugging-flowchart.png)

|  | Ação | DESCRIÇÃO | 
|---------|---------|---------|
|1 | Localize o grupo de conectores atribuído ao aplicativo | Você provavelmente tem um conector instalado em vários servidores, caso em que os conectores devem ser [atribuídas a grupos de conector](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups). Para saber mais sobre os grupos de conectores, confira [Publicar aplicativos em redes e locais separados usando grupos de conector](application-proxy-connector-groups.md). |
|2 | Instalar o conector e atribuir um grupo | Se você não tiver um conector instalado, consulte [instalar e registrar um conector](application-proxy-add-on-premises-application.md#install-and-register-a-connector).<br></br>Se o conector não está atribuído a um grupo, consulte [atribuir o conector a um grupo](application-proxy-connector-groups.md#create-connector-groups).<br></br>Se o aplicativo não está atribuído a um grupo de conectores, consulte [atribuir o aplicativo a um grupo de conectores](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups).|
|3 | Executar um teste de porta no servidor do conector | No servidor de conector, execute um teste de porta por meio [telnet](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) ou outro teste de porta de ferramentas para verificar se as portas 443 e 80 estão abertas.|
|4 | Configurar as portas e domínios | [Certifique-se de que as portas e domínios estão configuradas corretamente](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment) para o conector funcione corretamente, há determinadas portas que devem ser abertas e as URLs que o servidor deve ser capaz de acessar. |
|5 | Verifique se um proxy de back-end está em uso | Verifique se os conectores estão usando servidores proxy de back-end ou ignorando-los. Para obter detalhes, consulte [solucionar problemas de proxy do conector e problemas de conectividade do serviço](application-proxy-configure-connectors-with-proxy-servers.md#troubleshoot-connector-proxy-problems-and-service-connectivity-issues). |
|6 | Atualizar o conector e o atualizador de usar o proxy de back-end | Se um proxy de back-end estiver em uso, você desejará certificar-se de que o conector está usando o mesmo proxy. Para obter detalhes sobre a solução de problemas e configurar conectores para trabalhar com servidores proxy, consulte [trabalhar com existentes locais servidores proxy](application-proxy-configure-connectors-with-proxy-servers.md). |
|7 | Carregar a URL do aplicativo interno no servidor do conector | No servidor do conector, carregar a URL do aplicativo interno. |
|8 | Verifique a conectividade de rede interna | Há um problema de conectividade na sua rede interna que este fluxo de depuração é possível diagnosticar. O aplicativo deve ser acessível internamente para os conectores trabalhar. Você pode habilitar e exibir logs de eventos do conector, conforme descrito em [conectores de Proxy de aplicativo](application-proxy-connectors.md#under-the-hood). |
|9 | Aumentar o valor de tempo limite no back-end | No **configurações adicionais** para seu aplicativo, altere o **tempo limite do aplicativo de back-end** definir como **longo**. Ver [adicionar um aplicativo local ao Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). |
|10 | Se o problema persistir, direcionar problemas de fluxo específico, revisão de aplicativo e depuração de fluxos SSO | Use o [problemas de aplicativos de Proxy de aplicativo depurar](application-proxy-debug-apps.md) Solucionando problemas de fluxo. |

## <a name="next-steps"></a>Próximas etapas


* [Publicar aplicativos em redes e locais separados usando grupos de conectores](application-proxy-connector-groups.md)
* [Trabalhar com servidores proxy locais existentes](application-proxy-configure-connectors-with-proxy-servers.md)
* [Solucionar erros do conector e do Proxy de Aplicativo](application-proxy-troubleshoot.md)
* [Como fazer uma instalação silenciosa do Conector de Proxy de Aplicativo Azure AD](application-proxy-register-connector-powershell.md)

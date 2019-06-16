---
title: Compreender e solucionar problemas de CORS de Proxy de aplicativo do AD do Azure
description: Fornece uma compreensão do CORS no Proxy de aplicativo do Azure AD e como identificar e solucionar problemas CORS.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: 2b6adcf4231aa44a4f28d277e963efa16de8af81
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399340"
---
# <a name="understand-and-solve-azure-active-directory-application-proxy-cors-issues"></a>Compreender e solucionar problemas do Azure Active Directory Application Proxy CORS

[Recursos entre origens (CORS) compartilhamento](http://www.w3.org/TR/cors/) , às vezes, pode apresentar desafios para os aplicativos e APIs que você publica por meio do Proxy de aplicativo do Azure Active Directory. Este artigo discute problemas de CORS de Proxy de aplicativo do AD do Azure e as soluções.

Segurança do navegador geralmente impede que uma página da web faça solicitações AJAX para outro domínio. Essa restrição é chamada de *política de mesma origem*e impede que um site mal-intencionado lendo dados confidenciais de outro site. No entanto, às vezes, talvez queira permitir que outros sites chamar sua API da web. O CORS é um padrão W3C que permite que um servidor relaxar a política de mesma origem e permitir algumas solicitações entre origens enquanto rejeita outras.

## <a name="understand-and-identify-cors-issues"></a>Compreender e identificar problemas CORS

Duas URLs têm a mesma origem se eles têm esquemas idênticas, hosts e portas ([6454 RFC](https://tools.ietf.org/html/rfc6454)), tais como:

-   http:\//contoso.com/foo.html
-   http:\//contoso.com/bar.html

As URLs a seguir têm diferentes origens que o anterior dois:

-   http:\//contoso.net - domínio diferente
-   http:\//contoso.com:9000/foo.html - porta diferente
-   https:\//contoso.com/foo.html - esquema diferente
-   http:\//www.contoso.com/foo.html - subdomínio diferente

Política de mesma origem impede que aplicativos acessem recursos de outras origens, a menos que eles usam os cabeçalhos de controle de acesso corretas. Se os cabeçalhos de CORS estiverem ausente ou incorreto, falharem solicitações entre origens. 

Você pode identificar problemas CORS usando as ferramentas de depuração do navegador:

1. Iniciar o navegador e navegue até o aplicativo web.
1. Pressione **F12** para abrir o console de depuração.
1. Tente reproduzir a transação e examine a mensagem de console. Uma violação de CORS produz um erro de console sobre a origem.

Na seguinte captura de tela, selecionando o **Experimente** botão gerou uma mensagem de erro CORS que https:\//corswebclient-contoso.msappproxy.net não foi encontrado no cabeçalho Access-Control-Allow-Origin.

![Problema do CORS](./media/application-proxy-understand-cors-issues/image3.png)

## <a name="cors-challenges-with-application-proxy"></a>Desafios CORS com Proxy de aplicativo

O exemplo a seguir mostra um típico cenário de CORS de Proxy de aplicativo do AD do Azure. Os hosts de servidor interno uma **CORSWebService** controlador de API da web e uma **CORSWebClient** que chama **CORSWebService**. Existe uma solicitação AJAX do **CORSWebClient** à **CORSWebService**.

![Solicitação de mesma origem local](./media/application-proxy-understand-cors-issues/image1.png)

O aplicativo CORSWebClient funciona quando você hospedá-lo no local, mas o falhar ao carregar ou erros quando publicados por meio do Proxy de aplicativo do Azure AD. Se você publicou o CORSWebClient e CORSWebService aplicativos separadamente como diferentes aplicativos por meio do Proxy de aplicativo, os dois aplicativos são hospedados em diferentes domínios. Uma solicitação AJAX de CORSWebClient para CORSWebService é uma solicitação entre origens e ele falhar.

![Solicitação de CORS de Proxy de aplicativo](./media/application-proxy-understand-cors-issues/image2.png)

## <a name="solutions-for-application-proxy-cors-issues"></a>Soluções para problemas de CORS de Proxy de aplicativo

Você pode resolver o problema do CORS anterior em qualquer uma das várias maneiras.

### <a name="option-1-set-up-a-custom-domain"></a>Opção 1: Configurar um domínio personalizado

Usar um Proxy de aplicativo do Azure AD [domínio personalizado](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) publicar da mesma origem, sem precisar fazer nenhuma alteração para origens de aplicativo, código ou cabeçalhos. 

### <a name="option-2-publish-the-parent-directory"></a>Opção 2: Publicar o diretório pai

O diretório pai de ambos os aplicativos de publicação. Essa solução funciona especialmente bem se você tiver apenas dois aplicativos no servidor web. Em vez de publicar cada aplicativo separadamente, você pode publicar o diretório pai comum, o que resulta na mesma origem.

Os exemplos a seguir mostram o página do Proxy de aplicativo do Azure AD para o aplicativo CORSWebClient do portal.  Quando o **URL interna** é definido como *contoso.com/CORSWebClient*, o aplicativo não pode fazer solicitações com êxito para o *contoso.com/CORSWebService* diretório, porque eles estão entre origens. 

![Publicar o aplicativo individualmente](./media/application-proxy-understand-cors-issues/image4.png)

Em vez disso, defina a **URL interna** para publicar o diretório pai, que inclui ambos os *CORSWebClient* e *CORSWebService* diretórios:

![Publicar o diretório pai](./media/application-proxy-understand-cors-issues/image5.png)

As URLs do aplicativo resultante efetivamente resolverem o problema do CORS:

- https:\//corswebclient-contoso.msappproxy.net/CORSWebService
- https:\//corswebclient-contoso.msappproxy.net/CORSWebClient

### <a name="option-3-update-http-headers"></a>Opção 3: Atualizar os cabeçalhos HTTP

Adicione um cabeçalho de resposta HTTP personalizado no serviço da web para comparar a solicitação de origem. Para sites em execução no Internet Information Services (IIS), use o Gerenciador do IIS para modificar o cabeçalho:

![Adicionar cabeçalho de resposta personalizada no Gerenciador do IIS](./media/application-proxy-understand-cors-issues/image6.png)

Essa modificação não exige nenhuma alteração de código. Você pode verificá-lo nos rastreamentos do Fiddler:

**Após a adição de cabeçalho**\
HTTP/1.1 200 OK\
Cache-Control: não-cache\
Pragma: no-cache\
Content-Type: text/plain; charset=utf-8\
Expira em:-1 \
Variam de acordo: Encoding\ aceitar
Servidor:  Microsoft-IIS/8.5 Microsoft-HTTPAPI/2.0\
**Access-Control-Allow-Origin: https://corswebclient-contoso.msappproxy.net** \
X-AspNet-Version: 4.0.30319\
X-Powered-By: ASP.NET\
Content-Length: 17

### <a name="option-4-modify-the-app"></a>Opção 4: Modifique o aplicativo

Você pode alterar seu aplicativo para dar suporte a CORS, adicionando o cabeçalho Access-Control-Allow-Origin, com valores apropriados. A maneira de adicionar o cabeçalho depende do idioma de código do aplicativo. Alterar o código é a opção menos recomendada, porque ele requer maior esforço.

### <a name="option-5-extend-the-lifetime-of-the-access-token"></a>Opção 5: Estender o tempo de vida do token de acesso

Alguns problemas CORS não podem ser resolvidos, como quando seu aplicativo redireciona para *login.microsoftonline.com* para autenticar, e o token de acesso expira. Os CORS chamar em seguida, falhar. Uma solução alternativa para esse cenário é estender o tempo de vida do token de acesso, para impedir que ela expire durante uma sessão do usuário. Para obter mais informações sobre como fazer isso, consulte [tempos de vida de token configuráveis no Azure AD](../develop/active-directory-configurable-token-lifetimes.md).

## <a name="see-also"></a>Consulte também
- [Tutorial: Adicionar um aplicativo local para acesso remoto por meio do Proxy de aplicativo no Azure Active Directory](application-proxy-add-on-premises-application.md) 
- [Planejar uma implantação do Proxy de aplicativo do Azure AD](application-proxy-deployment-plan.md) 
- [Acesso remoto a aplicativos locais por meio do Proxy de aplicativo do Active Directory do Azure](application-proxy.md) 

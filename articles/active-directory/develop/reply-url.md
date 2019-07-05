---
title: Limitações – plataforma de identidade da Microsoft e restrições de URL de resposta/URI de redirecionamento
description: Limitações e restrições de URls de redirecionamento/URLs de resposta
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 06/29/2019
ms.topic: article
ms.subservice: develop
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07be7d0c70193fec88782fea681e33d6b4cf4b40
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486227"
---
# <a name="redirect-urireply-url-restrictions-and-limitations"></a>Limitações e restrições de URL de resposta/URI de redirecionamento

Um URI ou URL de resposta de redirecionamento é o local que o servidor de autorização enviará o usuário a vez o aplicativo foi autorizado com êxito e um código de autorização ou acesso token. O código ou o token está contida no redirecionamento de URI ou resposta token então é importante que você registre o local correto como parte do processo de registro de aplicativo.

## <a name="maximum-number-of-redirect-uris"></a>Número máximo de URIs de redirecionamento

A tabela a seguir mostra o número máximo de redirecionamento de URIs que você pode adicionar ao registrar seu aplicativo. 

| Contas que está sendo conectadas | Número máximo de URIs de redirecionamento | DESCRIÇÃO |
|--------------------------|---------------------------------|-------------|
| Microsoft ou de estudante contas no locatário do Azure Active Directory (Azure AD) de qualquer organização | 256 | `signInAudience` o campo no manifesto do aplicativo é definido como *AzureADMyOrg* ou *AzureADMultipleOrgs* |
| Pessoal da Microsoft e contas de trabalho e contas corporativas | 100 | `signInAudience` o campo no manifesto do aplicativo é definido como *AzureADandPersonalMicrosoftAccount* |

## <a name="maximum-uri-length"></a>Comprimento máximo do URI

Você pode usar um máximo de 256 caracteres para cada URI de redirecionamento que você adicionar a um registro de aplicativo.

## <a name="restrictions-using-a-wildcard-in-uris"></a>Restrições usando um caractere curinga em URIs

Wildcard URIs, como `https://*.contoso.com`, são convenientes, mas deve ser evitado. Usando caracteres curinga no redirecionamento de URI tem implicações de segurança. De acordo com a especificação do OAuth 2.0 ([seção 3.1.2 da RFC 6749](https://tools.ietf.org/html/rfc6749#section-3.1.2)), um URI do ponto de extremidade de redirecionamento deve ser um URI absoluto. 

O modelo de aplicativo do Azure AD não dá suporte a curinga URIs para aplicativos que são configurados para entrar em contas pessoais da Microsoft e de trabalho ou escolares. No entanto, URIs de curinga são permitidos para aplicativos que são configurados para entrar no trabalho ou de estudante contas no locatário do Azure AD da organização hoje mesmo. 
 
> [!NOTE]
> O novo [registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) experiência não permite que os desenvolvedores adicionem curingas URIs da interface do usuário. Adicionar o caractere curinga URI para aplicativos que entrar no trabalho ou de estudante contas somente é suportada pelo editor de manifesto de aplicativo. Novos aplicativos no futuro, não será possível usar curingas no URI de redirecionamento. No entanto, aplicativos mais antigos que contêm caracteres curinga no redirecionamento de URIs continuará a funcionar.

Se seu cenário exigir que mais URIs de redirecionamento que o limite máximo permitido, em vez de adicionar um curinga URI de redirecionamento, considere uma das abordagens a seguir.

### <a name="use-a-state-parameter"></a>Usar um parâmetro de estado

Se você tiver um número de subdomínios, e se seu cenário exigir redirecionar usuários após a autenticação bem-sucedida para a mesma página de início, usando um parâmetro de estado pode ser útil. 

Nessa abordagem:

1. Crie um URI de redirecionamento "compartilhado" por aplicativo para processar os tokens de segurança que você receber o ponto de extremidade de autorização.
1. Seu aplicativo pode enviar parâmetros específicos do aplicativo (por exemplo, a URL onde o usuário que originou ou qualquer coisa, como informações de identidade visual do subdomínio) no parâmetro state. Ao usar um parâmetro de estado, proteger contra a proteção de CSRF conforme especificado na [seção 10.12 da RFC 6749](https://tools.ietf.org/html/rfc6749#section-10.12)). 
1. Os parâmetros específicos do aplicativo incluirá todas as informações necessárias para o aplicativo para processar o correto experiência do usuário, ou seja, construa o estado do aplicativo apropriado. As faixas de ponto de extremidade de autorização do Azure AD HTML do parâmetro de estado portanto, verifique se você não estiver passando HTML conteúdo nesse parâmetro.
1. Quando o Azure AD envia uma resposta para o URI de redirecionamento "compartilhada", ele enviará o parâmetro de estado para o aplicativo.
1. O aplicativo pode usar o valor no parâmetro de estado para determinar qual URL de envio ainda mais o usuário. Certifique-se de que validar para a proteção de CSRF.

> [!NOTE]
> Essa abordagem permite que um cliente comprometido modificar os parâmetros adicionais enviados no parâmetro de estado, assim, redirecionar o usuário para uma URL diferente, que é o [abra a ameaça de redirecionador](https://tools.ietf.org/html/rfc6819#section-4.2.4) descrito no RFC 6819. Portanto, o cliente deve proteger esses parâmetros por criptografar o estado ou verificá-lo por outros meios, como validação de nome de domínio no URI de redirecionamento em relação ao token.

### <a name="add-redirect-uris-to-service-principals"></a>Adicionar URIs de redirecionamento para entidades de serviço

Outra abordagem é adicionar redirecionar URIs para o [entidades de serviço](app-objects-and-service-principals.md#application-and-service-principal-relationship) que representam o registro do seu aplicativo em qualquer locatário do AD do Azure. Você pode usar essa abordagem quando você não pode usar um parâmetro de estado ou o seu cenário exige que você adicionar novos URIs de redirecionamento para o registro do aplicativo para cada novo locatário em que você oferece suporte. 

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [manifesto do aplicativo](reference-app-manifest.md)

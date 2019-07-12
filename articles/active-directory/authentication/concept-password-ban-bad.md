---
title: Senhas - Azure Active Directory banidas dinamicamente
description: Proibir senhas fracas do seu ambiente com senhas proibidas dinamicamente pelo Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: c043b2ed1a626e362d7edd1a83429aa14046f8ac
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703071"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Eliminar senhas incorretas na organização

Líderes do setor que você não deve usar a mesma senha em vários lugares, para tornar isso complexo e não fazer simples como "Password123". Como as organizações podem garantir que seus usuários estão seguindo as orientações para práticas recomendadas? Como eles ter certeza de que os usuários não estiverem usando senhas fracas, ou até mesma variações senhas fracas?

A etapa inicial que senhas mais fortes é fornecer orientações para seus usuários. A diretriz atual da Microsoft sobre este tópico pode ser encontrada no seguinte link:

[Diretrizes de Senha da Microsoft](https://www.microsoft.com/research/publication/password-guidance)

É importante, mas até mesmo com o que sabemos que muitos usuários acabarão escolhendo senhas fracas ter boas diretrizes. A proteção de senha do AD do Azure protege sua organização, detectar e bloquear senhas fracas conhecidas e suas variantes, bem como, opcionalmente, bloqueando termos fracos adicionais que são específicos para sua organização.

Para obter mais informações sobre os esforços atuais de segurança, consulte o [Relatório de inteligência de segurança da Microsoft](https://www.microsoft.com/security/operations/security-intelligence-report).

## <a name="global-banned-password-list"></a>Lista de senhas proibidas globalmente

A equipe do Azure AD Identity Protection constantemente analisa a telemetria de segurança do Azure AD dados procurando para senhas fracas ou comprometidas comumente usadas, ou, mais especificamente, o mais fraco base termos que geralmente são usados como base para senhas fracas. Quando esses termos de baixa segurança forem encontrados, eles são adicionados à lista de senhas banidas global. O conteúdo da lista global de senhas banidas não se baseiam em qualquer fonte de dados externa. A lista global de senhas banidas baseia-se totalmente nos resultados contínuos de telemetria de segurança do Azure AD e a análise.

Sempre que uma nova senha for alterada ou redefinida para qualquer usuário em qualquer locatário do AD do Azure, a versão atual da lista de senhas banidas global é usada como a chave de entrada ao validar a força da senha. Essa validação resulta em senhas muito mais fortes para todos os clientes do AD do Azure.

> [!NOTE]
> Cibercriminosos também usam estratégias semelhantes em seus ataques. Portanto, Microsoft não publica o conteúdo desta lista publicamente.

## <a name="custom-banned-password-list"></a>Lista personalizada de senhas banidas

Algumas organizações podem desejar melhorar ainda mais a segurança, adicionando suas próprias personalizações na parte superior da lista de senhas banidas global em que a Microsoft chama de lista de senhas banidas personalizado. A Microsoft recomenda que adicionada à lista de termos estão concentrados principalmente em termos de organizacionais específicos, como:

- Nomes de marca
- Nomes de produto
- Locais (por exemplo, como a sede da empresa)
- Termos de internos específico da empresa
- Abreviações que têm o que significa específica da empresa.

Depois que termos são adicionados à lista de senhas banidas personalizados, eles serão adicionados à lista global de senhas banidas quando a validação de senhas.

> [!NOTE]
> A lista de senhas banidas personalizado é limitada a ter um máximo de termos de 1000. Ele não foi projetado para ser extremamente grandes listas de senhas de bloqueio. Para aproveitar totalmente os benefícios da lista de senhas banidas personalizada, a Microsoft recomenda que você primeiro examinar e entender o algoritmo de avaliação de senha (veja [como as senhas são avaliadas](concept-password-ban-bad.md#how-are-passwords-evaluated)) antes de adicionar novos termos para o lista proibida personalizada. Noções básicas sobre como funciona o algoritmo permitirá que sua empresa detectar e bloquear o grande número de senhas fracas e suas variantes de maneira eficiente.

Por exemplo: considere um cliente chamado "Contoso", que se baseia em Londres, e isso faz com que um produto chamado "Widget". Para o cliente, ele seria um desperdício, bem como menos seguro para tentar bloquear as variações específicas desses termos, como:

- "Contoso! 1"
- "Contoso@London"
- "ContosoWidget"
- "! Contoso"
- "LondonHQ"
- ...etcetera

Em vez disso, é muito mais eficiente e seguro para bloquear os principais termos de base:

- "Contoso"
- "Londres"
- "Widget"

O algoritmo de validação de senha, em seguida, bloqueará automaticamente variantes fracos e combinações de opções acima.

A lista de senhas proibidas personalizada e a capacidade de habilitar a integração do Active Directory local são gerenciadas usando o portal do Azure.

![Modificar a lista de senhas banidas personalizado em métodos de autenticação](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="password-spray-attacks-and-third-party-compromised-password-lists"></a>Ataques de senha de spray e listas de senhas comprometidas de terceiros

É uma chave de benefício de proteção de senha do AD do Azure para ajudá-lo a se defender contra ataques de senha de spray. A maioria dos ataques de senha de spray não tentar atacar qualquer determinada conta individual mais de algumas vezes desde que tal comportamento aumenta significativamente a probabilidade de detecção de, por meio do bloqueio de conta ou outros meios. Portanto, a maioria dos ataques de senha de spray contar enviando apenas um pequeno número de senhas mais fracas conhecidos em relação a cada uma das contas em uma empresa. Essa técnica permite que o invasor localizar rapidamente uma conta podem ser facilmente comprometida enquanto ao mesmo tempo evitando possíveis limites de detecção.

Proteção por senha do AD do Azure é projetada para bloquear com eficiência todas as senhas fracas conhecidas que podem ser usadas em ataques de spray de senha, com base nos dados de telemetria de segurança do mundo real, como visto pelo AD do Azure.  A Microsoft está ciente dos sites de terceiros que enumerar milhões de senhas que foram comprometidas em violações de segurança publicamente conhecidos anteriores. É comum para produtos de validação de senha de terceiros deve se basear a comparação de força bruta contra esses milhões de senhas. Microsoft sentem que essas técnicas não são a melhor maneira de melhorar a força da senha geral considerando as estratégias típica usadas pelos invasores de spray de senha.

> [!NOTE]
> Microsoft global senhas banidas lista não é com base em fontes de depende de quaisquer dados de terceiros, incluindo listas de senhas comprometidas.

Embora a lista proibida global da Microsoft é pequena em comparação com algumas listas em massa de terceiros, seus efeitos de segurança são amplificados pelo fato de que ele é originado de telemetria de segurança do mundo real de ataques de senha real de irrigação, além do fato de que a Microsoft algoritmo de validação de senha usa técnicas de correspondência difusa inteligentes. O resultado final é que ele com eficiência detectar e bloquear milhões de senhas fracas mais comuns do que está sendo usado em sua empresa. Os clientes que optam por adicionar termos específicos da organização à lista de senhas banidas personalizado também aproveitar o mesmo algoritmo.

## <a name="on-premises-hybrid-scenarios"></a>Cenários híbridos locais

A proteção de contas somente na nuvem é útil, mas muitas organizações mantêm cenários híbridos, incluindo o Windows Server Active Directory local. É possível instalar a proteção por senha do Azure AD para Windows Server Active Directory agentes locais estender as as listas de senhas em sua infraestrutura existente. Agora, os usuários e administradores que alteram, definem ou redefinem senhas locais precisam obedecer à mesma política de senha que os usuários somente na nuvem.

## <a name="how-are-passwords-evaluated"></a>Como as senhas são avaliadas

Sempre que um usuário altera ou redefine a senha, a nova senha é verificada quanto à intensidade e à complexidade validando-a com relação à lista de senhas banidas personalizadas e globais (se estas últimas estiverem configuradas).

Mesmo que a senha do usuário contenha uma senha proibida, ela ainda poderá ser aceita se a senha geral for forte o suficiente em outros aspectos. Uma senha configurada recentemente passará pelas etapas a seguir para avaliar a força geral e determinar se ela deverá ser aceita ou rejeitada.

### <a name="step-1-normalization"></a>Etapa 1: Normalização

Primeiro, uma nova senha passa por um processo de normalização. Essa técnica permite que um pequeno conjunto de senhas banidas ser mapeado para um conjunto muito maior do que senhas fracas potencialmente.

A normalização tem duas partes.  Primeiros, todas as letras maiúsculas são alteradas para letras minúsculas.  Por exemplo, as substituições de caractere comuns são realizadas, como:  

| Letra original  | Letra substituída |
| --- | --- |
| '0'  | 'o' |
| '1'  | 'l' |
| '$'  | 's' |
| '\@'  | 'a' |

Exemplo: suponha que a senha "em branco" seja proibida e um usuário tente alterar a senha para “Bl@nK”. Mesmo que “Bl@nk” não seja especificamente proibida, o processo de normalização converterá essa senha como "em branco", que é uma senha banida.

### <a name="step-2-check-if-password-is-considered-banned"></a>Etapa 2: Verificar se a senha é considerada proibida

#### <a name="fuzzy-matching-behavior"></a>Comportamento da correspondência difusa

Correspondência difusa é usada na senha normalizada para identificar se ela contém uma senha encontrada na lista de senhas banidas global ou personalizada. O processo de correspondência baseia-se em uma distância de edição de comparação de um (1).  

Exemplo: suponha que a senha “abcdef” seja proibida e um usuário tente alterar a senha para uma dos seguintes:

‘abcdeg’    *(último caractere foi alterado de ‘f’ para ‘g’)* ‘abcdefg’   *’(g’ acrescentado ao final)* ‘abcde’     *(‘f’ à direita excluído do fim)*

Cada uma das senhas acima não corresponde especificamente à senha banida "abcdef". No entanto, uma vez que cada exemplo é dentro de uma distância de edição de 1 do termo 'abcdef' proibido, eles são considerados como uma correspondência para "abcdef".

#### <a name="substring-matching-on-specific-terms"></a>Correspondência de subcadeia de caracteres (em termos específicos)

A correspondência de subcadeia de caracteres é usada na senha normalizada para verificar o nome e o sobrenome do usuário, bem como o nome do locatário (observe que a correspondência de nome de locatário não é feita ao validar senhas em um controlador de domínio do Active Directory).

Exemplo: suponha que temos um usuário, Pol, o que deseja redefinir sua senha para "P0l123fb". Após a normalização, essa senha seria "pol123fb". Correspondência de subcadeia de caracteres localiza a senha contém o nome do usuário "Pol". Mesmo que "P0l123fb" não foi especificamente em qualquer uma das listas de senhas banidas, correspondência de subcadeia de caracteres encontrada "Pol" na senha. Portanto, essa senha deve ser rejeitada.

#### <a name="score-calculation"></a>Cálculo de pontuação

A próxima etapa é identificar todas as instâncias de senhas banidas na nova senha normalizada do usuário. Em seguida:

1. Cada senha banida encontrada na senha do usuário recebe um ponto.
2. Cada caractere exclusivo restante recebe um ponto.
3. Uma senha deve ter pelo menos cinco (5) pontos para que ela seja aceita.

Para os próximos dois exemplos, vamos supor que a Contoso esteja usando a proteção de senha do Azure AD e tenha "contoso" em suas listas personalizadas. Vamos supor também que "em branco" está na lista global.

Exemplo: um usuário altera sua senha para “C0ntos0Blank12”

Após a normalização, essa senha se torna “contosoblank12”. O processo de correspondência localiza que essa senha contém duas senhas banidas: contoso e em branco. Essa senha então recebe uma pontuação:

[contoso] + [em branco] + [1] + [2] = 4 pontos como essa senha é pontos em cinco (5), ele será rejeitado.

Exemplo: um usuário altera a senha para "ContoS0Bl@nkf9!".

Após a normalização, essa senha se torna “contosoblankf9!”. O processo de correspondência localiza que essa senha contém duas senhas banidas: contoso e em branco. Essa senha então recebe uma pontuação:

[contoso] + [em branco] + [f] + [9] + [!] = 5 pontos, pois essa senha é pontos de pelo menos cinco (5), ela é aceita.

   > [!IMPORTANT]
   > Observe que o algoritmo de senhas banidas junto com a lista global pode mudar e muda a qualquer momento no Azure com base em análise e pesquisa de segurança contínuas. Para o serviço de agente do controlador de domínio local, os algoritmos atualizados só terão efeito depois que o software do agente de controlador de domínio for reinstalado.

## <a name="license-requirements"></a>Requisitos de licença

|   | Proteção por senha do AD do Azure com a lista de senhas banidas global | Proteção por senha do AD do Azure com a lista de senhas banidas personalizado|
| --- | --- | --- |
| Usuários somente na nuvem | AD do Azure Gratuito | O Azure AD Premium P1 ou P2 |
| Os usuários sincronizados no local Windows Server Active Directory | O Azure AD Premium P1 ou P2 | O Azure AD Premium P1 ou P2 |

> [!NOTE]
> Os usuários do Windows Server Active Directory local que não são sincronizados ao Azure Active Directory também avaliar os benefícios da proteção por senha do Azure AD com base em licenciamento existentes para os usuários sincronizados.

Informações adicionais sobre licenciamento, incluindo custos, podem ser encontradas no [site de preços do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>O que os usuários veem

Quando um usuário tentar redefinir uma senha para alguma que seria proibida, a seguinte mensagem de erro será exibida:

Infelizmente, sua senha contém uma palavra, uma frase ou um padrão que pode ser facilmente adivinhado. Tente novamente com uma senha diferente.

## <a name="next-steps"></a>Próximas etapas

- [Configurar a lista de senhas proibidas personalizada](howto-password-ban-bad.md)
- [Habilitar agentes de proteção por senha do Azure AD local](howto-password-ban-bad-on-premises-deploy.md)

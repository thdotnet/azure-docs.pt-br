---
title: Linha de base política bloco autenticação herdados (visualização) – Azure Active Directory
description: Política de acesso condicional para bloquear protocolos de autenticação herdados
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: d21b54c3bea98a9a1499dc75890f75f28f2f9dc0
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655718"
---
# <a name="baseline-policy-block-legacy-authentication-preview"></a>Política de linha de base: Autenticação herdada do bloco (visualização)

Para fornecer aos usuários acesso fácil aos aplicativos na nuvem, o Azure AD (Azure Active Directory) dá suporte a uma ampla variedade de protocolos de autenticação, incluindo a autenticação herdada. Autenticação é um termo que se refere a uma solicitação de autenticação feita por:

* Clientes do Office mais antigos que não usam autenticação moderna (por exemplo, o cliente do Office 2010)
* Qualquer cliente que usa protocolos de email herdados, como IMAP/SMTP/POP3

Atualmente, a maioria de todos os comprometer tentativas de entrada provenientes de autenticação herdados. Autenticação herdados não oferece suporte a autenticação multifator (MFA). Mesmo se você tiver uma política de MFA habilitada no seu diretório, um ator mal-intencionado pode autenticar usando um protocolo herdado e ignorar o MFA.

A melhor maneira de proteger sua conta das solicitações de autenticação mal-intencionada feitas pelos protocolos herdados é bloquear essas tentativas completamente. Em um esforço para tornar mais fácil de proteger seu ambiente, criamos esta política de linha de base para o bloco de autenticação herdados.

**Autenticação herdados do bloco** é um [política de linha de base](concept-baseline-protection.md) que bloqueia todas as solicitações de autenticação feitas de protocolos herdados. Autenticação moderna deve ser usada para entrar com êxito para todos os usuários. Usado em conjunto com outras políticas de linha de base, todas as solicitações provenientes de protocolos herdados serão bloqueadas e todos os usuários serão necessários para a MFA sempre que for necessário. Essa política não bloqueie o Exchange ActiveSync.

## <a name="identify-legacy-authentication-use"></a>Identificar o uso de autenticação herdados

Antes de você pode bloquear autenticação herdados em seu diretório, você precisa primeiro entender se os usuários têm aplicativos que usam autenticação herdados e como ela afeta seu diretório geral. Logs de entrada do AD do Azure podem ser usados para entender se você estiver usando autenticação herdados.

1. Navegue até a **portal do Azure** > **do Azure Active Directory** > **entradas**.
1. Adicionar a coluna de aplicativo cliente se ele não for exibido ao clicar no **colunas** > **aplicativo cliente**.
1. Filtrar por **aplicativo cliente** > **outros clientes** e clique em **aplicar**.

Filtragem somente para mostrar a você entrar tentativas feitas por protocolos de autenticação herdados. Clicar em cada tentativa de logon individual mostrará detalhes adicionais. O **aplicativo cliente** campo sob o **informações básicas de** guia indicará qual protocolo de autenticação herdada foi usado.

Esses logs indica que os usuários ainda dependem de autenticação herdados e quais aplicativos estão usando os protocolos herdados para fazer solicitações de autenticação. Para usuários que não aparecem nesses logs e confirmados que não esteja usando autenticação herdados, implemente uma política de acesso condicional ou habilitar o **política de linha de base: autenticação herdados do bloco** para apenas esses usuários.

## <a name="moving-away-from-legacy-authentication"></a>Se distanciando de autenticação herdados

Depois de ter uma ideia melhor de quem está usando autenticação herdados em seu diretório e os aplicativos que dependem dele, a próxima etapa é atualizar seus usuários para usar a autenticação moderna. Autenticação moderna é um método de gerenciamento de identidade que oferece autorização e autenticação de usuário mais segura. Se você tiver uma política de MFA em vigor no seu diretório, a autenticação moderna garante que o usuário é solicitado para MFA quando necessário. É a alternativa mais segura para protocolos de autenticação herdados.

Esta seção fornece uma visão geral passo a passo sobre como atualizar seu ambiente para autenticação moderna. Ler as etapas abaixo antes de habilitar uma autenticação herdados em sua organização a política de bloqueio.

### <a name="step-1-enable-modern-authentication-in-your-directory"></a>Etapa 1: Habilitar a autenticação moderna em seu diretório

A primeira etapa na habilitação da autenticação moderna é verificar se o que seu diretório dá suporte à autenticação moderna. Autenticação moderna está habilitada por padrão para diretórios criados em ou após 1º de agosto de 2017. Se o diretório foi criado antes dessa data, você precisará habilitar manualmente a autenticação moderna para seu diretório usando as seguintes etapas:

1. Verifique se o diretório já dá suporte à autenticação moderna executando `Get-CsOAuthConfiguration` do [Skype para módulo do PowerShell de negócios Online](https://docs.microsoft.com/office365/enterprise/powershell/manage-skype-for-business-online-with-office-365-powershell).
1. Se o comando retornará um `OAuthServers` propriedade e, em seguida, para autenticação moderna está desabilitado. Atualizar a configuração para habilitar a autenticação moderna usando `Set-CsOAuthConfiguration`. Se seu `OAuthServers` propriedade contém uma entrada, você está pronto para começar.

Certifique-se de concluir esta etapa antes de prosseguir. É fundamental que suas configurações de diretório são alteradas pela primeira vez, porque elas ditam o protocolo que será usado por todos os clientes do Office. Mesmo se você estiver usando clientes do Office que dão suporte à autenticação moderna, eles usarão como padrão para usar protocolos herdados se a autenticação moderna estiver desabilitada em seu diretório.

### <a name="step-2-office-applications"></a>Etapa 2: Aplicativos do Office

Depois que você habilitou a autenticação moderna em seu diretório, você pode iniciar a atualização de aplicativos, habilitando a autenticação moderna para clientes do Office. Office 2016 ou clientes de versões posteriores suportam autenticação moderna por padrão. Nenhuma etapa adicional é necessária.

Se você estiver usando clientes do Windows do Office 2013 ou anterior, é recomendável atualizar para o Office 2016 ou posterior. Até mesmo depois de concluir a etapa antes de habilitar a autenticação moderna em seu diretório, os aplicativos do Office mais antigos continuarão a usar protocolos de autenticação herdados. Se você estiver usando clientes do Office 2013 e não é possível atualizar imediatamente para o Office 2016 ou posterior, siga as etapas no artigo a seguir para [habilitar autenticação moderna do Office 2013 em dispositivos Windows](https://docs.microsoft.com/office365/admin/security-and-compliance/enable-modern-authentication). Para ajudar a proteger sua conta, enquanto você estiver usando a autenticação herdada, é recomendável usar senhas fortes em seu diretório. Fazer check-out [proteção por senha do Azure AD](../authentication/concept-password-ban-bad.md) proibir o uso de senhas fracas em seu diretório.

Office 2010 não dá suporte à autenticação moderna. Você precisará atualizar todos os usuários com o Office 2010 para uma versão mais recente do Office. É recomendável atualizar para o Office 2016 ou posterior, como, por padrão, ele bloqueia autenticação herdados.

Se você estiver usando o MacOS, é recomendável atualizar para o Office para Mac 2016 ou posterior. Se você estiver usando o cliente de email nativo, você precisará ter MacOS versão 10.14 ou posterior em todos os dispositivos.

### <a name="step-3-exchange-and-sharepoint"></a>Etapa 3: Exchange e SharePoint

Para os clientes do Outlook com base em Windows usam autenticação moderna, o Exchange Online deve ser autenticação moderna habilitada também. Se a autenticação moderna está desabilitada para Exchange Online, os clientes do Outlook com base em Windows que dão suporte à autenticação moderna (Outlook 2013 ou posterior) usará autenticação básica para se conectar a caixas de correio do Exchange Online.

SharePoint Online está habilitado para o padrão de autenticação moderna. Para diretórios criados após 1º de agosto de 2017, a autenticação moderna está habilitada por padrão no Exchange Online. No entanto, se você tiver desabilitado anteriormente autenticação moderna, ou você está usando um diretório criado antes dessa data, siga as etapas no artigo a seguir para [habilitar a autenticação moderna no Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online).

### <a name="step-4-skype-for-business"></a>Etapa 4: Skype for Business

Para impedir que solicitações de autenticação herdados feitas pelo Skype for Business, é necessário habilitar a autenticação moderna para o Skype for Business Online. Para diretórios criados após 1º de agosto de 2017, a autenticação moderna para o Skype for Business é habilitada por padrão.

Sugerimos que você transição para o Microsoft Teams, que dá suporte à autenticação moderna por padrão. No entanto, se você não conseguir migrar neste momento, você precisará habilitar a autenticação moderna para o Skype for Business Online para que o Skype para clientes de negócios começar a usar a autenticação moderna. Siga as etapas neste artigo [Skype para topologias de negócios com autenticação moderna](https://docs.microsoft.com/skypeforbusiness/plan-your-deployment/modern-authentication/topologies-supported), para habilitar a autenticação moderna para o Skype for Business.

Além de habilitar a autenticação moderna para o Skype for Business Online, é recomendável habilitar a autenticação moderna para o Exchange Online, ao habilitar a autenticação moderna para o Skype for Business. Esse processo ajudará a sincronizar o estado da autenticação moderna no Exchange Online e Skype for Business online e evitar vários prompts de entrada para o Skype para clientes de negócios.

### <a name="step-5-using-mobile-devices"></a>Etapa 5: Usando dispositivos móveis

Aplicativos em seu dispositivo móvel precisam bloquear a autenticação herdada. É recomendável usar o Outlook para dispositivos móveis. O Outlook para dispositivos móveis dá suporte à autenticação moderna por padrão e atender a outras políticas de proteção de linha de base MFA.

Para usar o cliente de email do iOS nativo, você precisará estar executando o iOS versão 11.0 ou posterior para garantir que o cliente de email foi atualizado para o bloco de autenticação herdados.

### <a name="step-6-on-premises-clients"></a>Etapa 6: Clientes locais

Se você for um cliente híbrida usando o Exchange Server local e o Skype para Business local, os dois serviços precisará ser atualizado para habilitar a autenticação moderna. Ao usar a autenticação moderna em um ambiente híbrido, você ainda estiver autenticando usuários locais. A história de autorizar o acesso às alterações de recursos (arquivos ou emails).

Antes de começar a habilitar a autenticação moderna no local, certifique-se de que os pré-requisitos foram atendidos.
Agora você está pronto para habilitar a autenticação moderna no local.

Etapas para habilitar a autenticação moderna podem ser encontradas nos seguintes artigos:

* [Como configurar o Exchange Server local para usar a autenticação moderna híbrida](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
* [Como usar a autenticação moderna (ADAL) com o Skype for Business](https://docs.microsoft.com/skypeforbusiness/manage/authentication/use-adal)

## <a name="enable-the-baseline-policy"></a>Habilitar a política de linha de base

A política **política de linha de base: Autenticação herdada do bloco (visualização)** vem pré-configurada e aparecerá na parte superior quando você navega até a folha de acesso condicional no portal do Azure.

Para habilitar essa política e proteger sua organização:

1. Entrar para o **portal do Azure** como Administrador Global, administrador de segurança ou administrador de acesso condicional.
1. Navegue até **do Azure Active Directory** > **acesso condicional**.
1. Na lista de políticas, selecione **política de linha de base: Autenticação herdada do bloco (visualização)** .
1. Definir **habilitar política** à **usar a política imediatamente**.
1. Clique em **salvar**.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte:

* [Políticas de proteção de linha de base de acesso condicional](concept-baseline-protection.md)
* [Cinco etapas para proteger a infraestrutura de identidade](../../security/azure-ad-secure-steps.md)
* [O que é o Acesso Condicional no Azure Active Directory?](overview.md)

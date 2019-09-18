---
title: Versões no fluxo de usuário no Azure Active Directory B2C | Microsoft Docs
description: Saiba mais sobre as versões de fluxos de usuário disponíveis no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c7b3ce6a4e72b90c6fd642b92d4a92eb02f4c92c
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71063192"
---
# <a name="user-flow-versions-in-azure-active-directory-b2c"></a>Versões no fluxo de usuário no Azure Active Directory B2C

>[!IMPORTANT]
> Qualquer fluxo de usuário listado neste artigo é considerado em visualização pública, a menos que identificado como **Recomendado**. Você somente deve usar fluxos recomendados ao usuário para seus aplicativos de produção.

O usuário flui em Azure Active Directory B2C (Azure AD B2C) ajuda a configurar políticas comuns que descrevem totalmente [as](active-directory-b2c-reference-policies.md) experiências de identidade do cliente. Essas experiências incluem inscrição, entrada, redefinição de senha ou edição de perfil. No Azure Active Directory B2C, você pode selecionar uma coleção de fluxos de usuário recomendada e fluxos de usuário de visualização.

Novos fluxos de usuário são adicionados como novas versões. Como fluxos dos usuários se tornam estáveis, serão recomendados para uso. Os fluxos dos usuários são marcados como **Recomendados** se tiver sido totalmente testadas. Os fluxos dos usuários serão considerados na visualização até que sejam marcados como recomendado. Use um fluxo de usuário recomendado para qualquer aplicativo de produção, mas escolha de outras versões para testar a nova funcionalidade assim que estiverem disponíveis. Você não deve usar as versões mais antigas dos fluxos recomendados ao usuário.

## <a name="v1"></a>V1

| Fluxo de usuário | Recomendado | Descrição |
| --------- | ----------- | ----------- |
| Redefinição de senha | Sim | Permite que um usuário escolha uma nova senha após verificar seu email. Usando este fluxo de usuário, você pode configurar: <ul><li>[Autenticação multifator](active-directory-b2c-reference-mfa.md)</li><li>Configurações de compatibilidade do token</li><li>[Requisitos de complexidade de senha](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Edição de perfil | Sim | Permite que o usuário configure seus atributos de usuário. Usando este fluxo de usuário, você pode configurar: <ul><li>[Tempo de vida do token](active-directory-b2c-reference-tokens.md)</li><li>Configurações de compatibilidade do token</li><li>Comportamento da sessão</li></ul> |
| Proprietário do recurso | Não | Permite que um usuário com uma conta local entre diretamente em aplicativos nativos (nenhum navegador necessário). Usando este fluxo de usuário, você pode configurar: <ul><li>[Tempo de vida do token](active-directory-b2c-reference-tokens.md)</li><li>Configurações de compatibilidade do token</li></ul> |
| Entrar | Não | Permite que um usuário entre em sua conta. Usando este fluxo de usuário, você pode configurar: <ul><li>[Autenticação multifator](active-directory-b2c-reference-mfa.md)</li><li>[Tempo de vida do token](active-directory-b2c-reference-tokens.md)</li><li>Configurações de compatibilidade do token</li><li>Comportamento da sessão</li><li>Entrar no bloco</li><li>Forçar redefinição de senha</li><li>Manter-me conectado (KMSI)</ul><br>Não é possível personalizar a interface do usuário com este fluxo de usuário. |
| Entrar | Não | Permite que um usuário crie uma conta. Usando este fluxo de usuário, você pode configurar: <ul><li>[Autenticação multifator](active-directory-b2c-reference-mfa.md)</li><li>[Tempo de vida do token](active-directory-b2c-reference-tokens.md)</li><li>Configurações de compatibilidade do token</li><li>Comportamento da sessão</li><li>[Requisitos de complexidade de senha](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Inscrever-se e entrar | Sim | Permite que um usuário crie uma conta ou entre em sua conta. Usando este fluxo de usuário, você pode configurar: <ul><li>[Autenticação multifator](active-directory-b2c-reference-mfa.md)</li><li>[Tempo de vida do token](active-directory-b2c-reference-tokens.md)</li><li>Configurações de compatibilidade do token</li><li>Comportamento da sessão</li><li>[Requisitos de complexidade de senha](active-directory-b2c-reference-password-complexity.md)</li></ul>|

## <a name="v2"></a>V2

| Fluxo de usuário | Recomendado | Descrição |
| --------- | ----------- | ----------- |
| Redefinição de senha v2 | Não | Permite que um usuário escolha uma nova senha após verificar seu email. Usando este fluxo de usuário, você pode configurar: <ul><li>[Autenticação multifator](active-directory-b2c-reference-mfa.md)</li><li>Configurações de compatibilidade do token</li><li>[Restrição de idade](basic-age-gating.md)</li><li>[requisitos de complexidade de senha](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Entrar v2 | Não | Permite que um usuário entre em sua conta. Usando este fluxo de usuário, você pode configurar: <ul><li>[Autenticação multifator](active-directory-b2c-reference-mfa.md)</li><li>[Tempo de vida do token](active-directory-b2c-reference-tokens.md)</li><li>Configurações de compatibilidade do token</li><li>Comportamento da sessão</li><li>[Restrição de idade](basic-age-gating.md)</li><li>Personalização da página de entrada</li></ul> |
| Inscrever-se v2 | Não | Permite que um usuário crie uma conta. Usando este fluxo de usuário, você pode configurar: <ul><li>[Autenticação multifator](active-directory-b2c-reference-mfa.md)</li><li>[Tempo de vida do token](active-directory-b2c-reference-tokens.md)</li><li>Configurações de compatibilidade do token</li><li>Comportamento da sessão</li><li>[Restrição de idade](basic-age-gating.md)</li><li>[Requisitos de complexidade de senha](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Inscrever-se e entrar v2 | Não | Permite que um usuário crie uma conta ou entre em sua conta. Usando este fluxo de usuário, você pode configurar: <ul><li>[Autenticação multifator](active-directory-b2c-reference-mfa.md)</li><li>[Restrição de idade](basic-age-gating.md)</li><li>[Requisitos de complexidade de senha](active-directory-b2c-reference-password-complexity.md)</li></ul> |

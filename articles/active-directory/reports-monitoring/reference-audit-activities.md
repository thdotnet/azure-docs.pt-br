---
title: Referência de atividades de auditoria do Azure AD (Azure AD) | Microsoft Docs
description: Obtenha uma visão geral das atividades de auditoria que podem ser registradas em log nos seus logs de auditoria do Azure AD (Azure Active Directory).
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 01/24/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: aaa60a7737b7781a21e23516d139332f10bdf448
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70305990"
---
# <a name="azure-ad-audit-activity-reference"></a>Referência das atividades de auditoria do Azure AD

Com os relatórios do Azure AD (Azure Active Directory), é possível obter as informações necessárias para determinar o desempenho do ambiente.

A arquitetura de relatório no Azure AD consiste nos seguintes componentes:

- **Relatórios de atividades** 
    - [Entradas](concept-sign-ins.md) – Fornece informações sobre o uso de aplicativos gerenciados e atividades de entrada do usuário
    - [Logs de auditoria](concept-audit-logs.md) – Permitem o rastreio de todas as alterações feitas por vários recursos no Azure AD por meio de logs. 
    
- **Relatórios de segurança** 
    - [Entradas arriscadas](concept-risky-sign-ins.md) - uma entrada arriscada é um indicador para uma tentativa de logon que pode ter sido realizada por alguém que não é o proprietário legítimo de uma conta de usuário. 
    - [Usuários sinalizados para riscos](concept-user-at-risk.md) - um usuário arriscado é um indicador de uma conta de usuário que pode ter sido comprometida. 

Este artigo lista as atividades de auditoria que podem ser registradas nos logs de auditoria.

## <a name="access-reviews"></a>Revisões de acesso

|Auditar categoria|Atividade|
|---|---|
|Revisões de Acesso|Revisão de acesso encerrada|
|Revisões de Acesso|Adicionar aprovador para a aprovação de solicitação|
|Revisões de Acesso|Adicionar revisor para a revisão de acesso|
|Revisões de Acesso|Aplicar revisão do acesso|
|Revisões de Acesso|Criar revisão do acesso|
|Revisões de Acesso|Criar programa|
|Revisões de Acesso|Criar solicitação de aprovação|
|Revisões de Acesso|Excluir a revisão de acesso|
|Revisões de Acesso|Excluir programa|
|Revisões de Acesso|Vincular controle do programa|
|Revisões de Acesso|Integrar com as Revisões de Acesso do Azure AD|
|Revisões de Acesso|Remover revisor da revisão de acesso|
|Revisões de Acesso|Solicitar interrupção da análise|
|Revisões de Acesso|Solicitação de aplicação do resultado da revisão|
|Revisões de Acesso|Examinar associação de função RBAC|
|Revisões de Acesso|Revisar atribuição do aplicativo|
|Revisões de Acesso|Revisar associação ao grupo|
|Revisões de Acesso|Revisar solicitação de aprovação de solicitação|
|Revisões de Acesso|Desvincular controle do programa|
|Revisões de Acesso|Atualizar revisão de acesso|
|Revisões de Acesso|Atualizar o status de Integração das Revisões de Acesso do Azure AD|
|Revisões de Acesso|Atualizar configurações de notificação de email da revisão de acesso|
|Revisões de Acesso|Atualizar a configuração de contagem de recorrências da revisão de acesso|
|Revisões de Acesso|Atualizar a duração de recorrência da revisão de acesso na configuração de dias|
|Revisões de Acesso|Atualizar a configuração do tipo final de recorrência da revisão de acesso|
|Revisões de Acesso|Atualizar a configuração do tipo de recorrência da revisão de acesso|
|Revisões de Acesso|Atualizar configurações de lembrete de análise de acesso|
|Revisões de Acesso|Atualizar programa|
|Revisões de Acesso|Atualizar aprovação de solicitação|
|Revisões de Acesso|Usuário desabilitado|

## <a name="account-provisioning"></a>Provisionamento de conta

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Aplicativos|Recuperar concessões de permissões de aplicativo V2|
|Gerenciamento de Aplicativos|Recuperar entidades de serviço de aplicativo V2 no locatário atual|
|Gerenciamento de Aplicativos|Atualizar aplicativo V1|
|Gerenciamento de Aplicativos|Atualizar aplicativo V2|
|Gerenciamento de Aplicativos|Atualizar concessão de permissão de aplicativo V2|
|Gerenciamento de Aplicativos|Adicionar Oauth2Permissiongrant|
|Gerenciamento de Aplicativos|Adicionar atribuição de função de aplicativo à entidade de serviço|

## <a name="application-proxy"></a>Proxy do aplicativo

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Aplicativos|Adicionar aplicativo|
|Gerenciamento de Aplicativos|Adicionar proprietário ao aplicativo|
|Gerenciamento de Aplicativos|Adicionar proprietário à entidade de serviço|
|Gerenciamento de Aplicativos|Adicionar política à entidade de serviço|
|Gerenciamento de Diretório|Adicionar entidade de serviço|
|Gerenciamento de Diretório|Adicionar credenciais de entidade de serviço|
|Gerenciamento de Diretório|Autorizar aplicativo|
|Gerenciamento de Diretório|Excluir aplicativo|
|Gerenciamento de Diretório|Excluir aplicativo irreversivelmente|
|Gerenciamento de Diretório|Remover OAuth2PermissionGrant|
|Gerenciamento de Diretório|Remover a atribuição de função de aplicativo da entidade de serviço|
|Gerenciamento de Diretório|Remover proprietário do aplicativo|
|Recurso|Remover proprietário da entidade de serviço|
|Recurso|Remover política da entidade de serviço|
|Recurso|Remover entidade de serviço|


## <a name="automated-password-rollover"></a>Substituição de senha automática

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Aplicativos|Remover credenciais de entidade de serviço|


## <a name="b2c"></a>B2C

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Aplicativos|Restaurar aplicativo|
|Gerenciamento de Aplicativos|Revogar autorização|
|Gerenciamento de Aplicativos|Atualizar aplicativo|
|Gerenciamento de Aplicativos|Atualizar segredos externos|
|Gerenciamento de Aplicativos|Atualizar entidade de serviço|
|Gerenciamento de Aplicativos|Emitir um token de acesso para o aplicativo|
|Gerenciamento de Aplicativos|Emitir um código de autorização para o aplicativo|
|Gerenciamento de Aplicativos|Emitir um id_token para o aplicativo|
|Gerenciamento de Aplicativos|Validar credenciais de conta local|
|Gerenciamento de Aplicativos|Validar autenticação de usuário|
|Gerenciamento de Aplicativos|Adicionar permissões da aplicativo V2|
|Gerenciamento de Aplicativos|Adicionar uma chave baseada em segredo ASCII a um contêiner de chave CPIM|
|Gerenciamento de Aplicativos|Adicionar uma chave a um contêiner de chave CPIM|
|Gerenciamento de Aplicativos|AdminPolicyDatas-SetResources|
|Gerenciamento de Aplicativos|AdminUserJourneys-GetResources|
|Gerenciamento de Aplicativos|AdminUserJourneys-RemoveResources|
|Autenticação|AdminUserJourneys-SetResources|
|Autenticação|Criar IdentityProvider|
|Autenticação|Criar aplicativo V1|
|Autenticação|Criar aplicativo V2|
|Autenticação|Criar um domínio personalizado no locatário|
|Autorização|Criar um novo AdminUserJourney|
|Autorização|Criar json de recurso localizado|
|Autorização|Criar novo IDP personalizado|
|Autorização|Criar novo IDP|
|Autorização|Criar ou atualizar um recurso de diretório do B2C|
|Autorização|Criar política|
|Autorização|Criar política trustFramework|
|Autorização|Criar política trustFramework com prefixo configurável|
|Autorização|Criar atributo de usuário|
|Autorização|CreateTrustFrameworkPolicy|
|Autorização|Cria ou atualiza um novo AdminUserJourney|
|Autorização|Excluir IDP|
|Autorização|Excluir IdentityProvider|
|Autorização|Excluir aplicativo V1|
|Autorização|Excluir aplicativo V2|
|Autorização|Excluir concessão de permissão de aplicativo V2|
|Autorização|Excluir um recurso do diretório do B2C|
|Autorização|Excluir um contêiner de chave CPIM|
|Autorização|Excluir política trustFramework|
|Autorização|Excluir atributo de usuário|
|Autorização|Habilitar recurso do B2C|
|Autorização|Obter os recursos de diretório do B2C em uma assinatura|
|Autorização|Obter IDP personalizado|
|Autorização|Obter IDP|
|Autorização|Obter aplicativos V1 e V2|
|Autorização|Obter aplicativo V1|
|Autorização|Obter aplicativos V1|
|Autorização|Obter aplicativo V2|
|Autorização|Obter aplicativos V2|
|Autorização|Obter recurso do Active Directory B2C|
|Autorização|Obter uma lista de domínios personalizados no locatário|
|Autorização|Obter um Percurso do Usuário|
|Autorização|Obter declarações do aplicativo permitidas para o Percurso do Usuário|
|Autorização|Obter declarações autodeclaradas permitidas para o Percurso do Usuário|
|Autorização|Obter declarações autodeclaradas permitidas de política|
|Autorização|Obter lista de declarações de saída disponíveis|
|Autorização|Obter definições de conteúdo para o Percurso do Usuário|
|Autorização|Obter IDPs para um fluxo específico do administrador|
|Autorização|Obter metadados de chave ativa do contêiner de chave em JWK|
|Autorização|Obter lista de todos os fluxos de administrador|
|Autorização|Obter lista de marcas de todos os fluxos de administrador para todos os usuários|
|Autorização|Obter lista de locatários de um usuário|
|Autorização|Obter declarações autodeclaradas das contas locais|
|Autorização|Obter json de recurso localizado|
|Autorização|Obter operações do provedor de recursos Microsoft.AzureActiveDirectory|
|Autorização|Obter políticas|
|Autorização|Obter política|
|Autorização|Obter propriedades de recurso de um locatário|
|Autorização|Obter lista de IDP com suporte|
|Autorização|Obter lista de IDP com suporte do Percurso do Usuário|
|Autorização|Obter informações do locatário|
|Autorização|Obter recursos permitidos para o locatário|
|Autorização|Obter lista de IDPs personalizadas definida pelo locatário|
|Autorização|Obter lista de IDPs definida pelo locatário|
|Autorização|Obter lista de IDPs locais definida pelo locatário|
|Autorização|Obter detalhes do locatário de um usuário para a criação de recursos|
|Autorização|Obter lista de locatários|
|Autorização|Obter tenantDomains|
|Autorização|Obter a cultura com suporte padrão para CPIM|
|Autorização|Obter os detalhes de um fluxo de administrador|
|Autorização|Obter a lista de UserJourneys para o locatário|
|Autorização|Obter o conjunto de culturas com suporte disponíveis para CPIM|
|Autorização|Obter política trustFramework|
|Autorização|Obter política trustFramework como xml|
|Autorização|Editar atributo de usuário|
|Autorização|Obter atributos de usuário|
|Autorização|Obter lista de Percurso do Usuário|
|Autorização|GetIEFPolicies|
|Autorização|GetIdentityProviders|
|Autorização|GetTrustFrameworkPolicy|
|Autorização|Obtém um contêiner de chave CPIM em formato jwk|
|Autorização|Obtém uma lista de contêineres de chave no locatário|
|Autorização|Obtém o tipo de locatário|
|Autorização|MigrateTenantMetadata|
|Autorização|Fazer patch de IdentityProvider|
|Autorização|PutTrustFrameworkPolicy|
|Autorização|PutTrustFrameworkpolicy|
|Autorização|Remover um Percurso do Usuário|
|Autorização|Restaurar um backup de contêiner de chave CPIM|
|Autorização|Recuperar concessões de permissões de aplicativo V2|
|Autorização|Recuperar entidades de serviço de aplicativo V2 no locatário atual|
|Autorização|Atualizar IDP personalizado|
|Autorização|Atualizar IDP|
|Autorização|Atualizar IDP Local|
|Autorização|Atualizar aplicativo V1|
|Autorização|Atualizar aplicativo V2|
|Autorização|Atualizar concessão de permissão de aplicativo V2|
|Autorização|Atualizar política|
|Autorização|Atualizar atributo de usuário|
|Autorização|Carregar uma chave CPIM criptografada|
|Autorização|Autorização do usuário: A API está desabilitada para o featureset do locatário|
|Autorização|Autorização do usuário: Acesso concedido ao usuário como 'Admin Locatário'|
|Autorização|Autorização do usuário: O usuário recebeu os direitos de acesso 'Usuários Autenticados'|
|Autorização|Verificar se o recurso B2C está habilitado|
|Autorização|Verificar se o recurso está habilitado|
|Autorização|Criar programa|
|Autorização|Excluir programa|
|Autorização|Vincular controle do programa|
|Autorização|Integrar com as Revisões de Acesso do Azure AD|
|Autorização|Desvincular controle do programa|
|Autorização|Atualizar programa|
|Autorização|Desabilitar SSO da Área de Trabalho|
|Autorização|Desabilitar SSO da Área de Trabalho para um domínio específico|
|Autorização|Desabilitar proxy de aplicativo|
|Autorização|Desabilitar autenticação de passagem|
|Autorização|Habilitar SSO da Área de Trabalho|
|Gerenciamento de diretórios|Habilitar SSO da Área de Trabalho para um domínio específico|
|Gerenciamento de diretórios|Habilitar proxy de aplicativo|
|Gerenciamento de diretórios|Habilitar autenticação de passagem|
|Gerenciamento de Diretório|Criar um domínio personalizado no locatário|
|Gerenciamento de Diretório|Habilitar recurso do B2C|
|Gerenciamento de Diretório|Obter uma lista de domínios personalizados no locatário|
|Gerenciamento de Diretório|Obter propriedades de recurso de um locatário|
|Gerenciamento de Diretório|Obter informações do locatário|
|Gerenciamento de diretórios|Obter recursos permitidos para o locatário|
|Gerenciamento de Diretório|Obter tenantDomains|
|Chave|Obtém o tipo de locatário|
|Chave|Verificar se o recurso B2C está habilitado|
|Chave|Verificar se o recurso está habilitado|
|Chave|Adicionar um parceiro à empresa|
|Chave|Adicionar domínio não verificado|
|Chave|Adicionar domínio verificado|
|Chave|Criar empresa|
|Chave|Criar configurações da empresa|
|Chave|Excluir configurações da empresa|
|Chave|Rebaixar parceiro|
|Chave|Diretório excluído|
|Outros|Diretório excluído permanentemente|
|Outros|Diretório agendado para exclusão|
|Recurso|Promover empresa à condição de parceira|
|Recurso|Limpar propriedades de gerenciamento de direitos|
|Recurso|Remover parceiro da empresa|
|Recurso|Remover domínio não verificado|
|Recurso|Remover domínio verificado|
|Recurso|Definir informações da empresa|
|Recurso|Definir recurso DirSync|
|Recurso|Definir sinalizador DirSyncEnabled|
|Recurso|Definir parceria|
|Recurso|Definir limite de exclusão acidental|
|Recurso|Definir local de dados permitido da empresa|
|Recurso|Definir recurso multinacional de empresa como habilitado|
|Recurso|Definir recurso de diretório no locatário|
|Recurso|Definir a autenticação de domínio|
|Recurso|Definir configurações de federação no domínio|
|Recurso|Definir política de senha|
|Recurso|Definir propriedades de gerenciamento de direitos|
|Recurso|Atualizar empresa|
|Recurso|Atualizar configurações da empresa|
|Recurso|Domínio de atualização|
|Recurso|Verificar domínio|
|Recurso|Verificar domínio por email|
|Recurso|Integração|
|Recurso|Atualizar configurações de alerta|
|Recurso|Atualizar configurações de resumo semanal|
|Recurso|Desabilitar write-back de senha para o diretório|
|Recurso|Habilitar write-back de senha para o diretório|
|Recurso|Adicionar atribuição de função de aplicativo ao grupo|
|Recurso|Adicionar grupo|
|Recurso|Adicionar membro ao grupo|
|Recurso|Adicionar proprietário ao grupo|
|Recurso|Criar configurações de grupo|
|Recurso|Excluir grupo|
|Recurso|Excluir configurações de grupo|
|Recurso|Concluir a aplicação de licença com base em grupo aos usuários|
|Recurso|Excluir grupo irreversivelmente|
|Recurso|Remover atribuição de função de aplicativo do grupo|
|Recurso|Remover membro do grupo|
|Recurso|Remover proprietário do grupo|
|Recurso|Restaurar grupo|
|Recurso|Definir licença de grupo|
|Recurso|Definir grupo a ser gerenciado pelo usuário|
|Recurso|Iniciar a aplicação de licença com base em grupo aos usuários|
|Recurso|Disparar recálculo de licença do grupo|
|Recurso|Atualizar grupo|
|Recurso|Atualizar configurações de grupo|
|Recurso|Adicionar Membro|
|Recurso|Criar Grupo|
|Recurso|Excluir Grupo|
|Recurso|Remover Membro|
|Recurso|Atualizar Grupo|
|Recurso|Aprovar uma solicitação pendente para ingressar em um grupo|
|Recurso|Cancelar uma solicitação pendente para ingressar em um grupo|
|Recurso|Criar política de gerenciamento de ciclo de vida|
|Recurso|Excluir uma solicitação pendente para ingressar em um grupo|
|Recurso|Rejeitar uma solicitação pendente para ingressar em um grupo|
|Recurso|Renovar o grupo|
|Recurso|Solicitar ingresso em um grupo|
|Recurso|Definir propriedades de grupo dinâmico|
|Recurso|Atualizar política de gerenciamento de ciclo de vida|
|Recurso|Adicionar uma chave baseada em segredo ASCII a um contêiner de chave CPIM|
|Recurso|Adicionar uma chave a um contêiner de chave CPIM|
|Recurso|Excluir um contêiner de chave CPIM|
|Recurso|Excluir contêiner de chave|
|Recurso|Obter metadados de chave ativa do contêiner de chave em JWK|
|Recurso|Obter metadados do contêiner de chave|
|Recurso|Obtém um contêiner de chave CPIM em formato jwk|
|Recurso|Obtém uma lista de contêineres de chave no locatário|
|Recurso|Restaurar um backup de contêiner de chave CPIM|
|Recurso|Salvar contêiner de chave|
|Recurso|Carregar uma chave CPIM criptografada|
|Recurso|Emitir um código de autorização para o aplicativo|
|Recurso|Emitir um id_token para o aplicativo|


## <a name="core-directory"></a>Diretório principal

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de unidades administrativas|Baixar um tipo de detecção de risco único|
|Gerenciamento de unidades administrativas|Baixar administradores e status de aceitação do resumo semanal|
|Gerenciamento de unidades administrativas|Baixar todos os tipos de detecção de riscos|
|Gerenciamento de unidades administrativas|Baixar detecções de risco de usuário gratuitas|
|Gerenciamento de unidades administrativas|Baixar usuários sinalizados por risco|
|Gerenciamento de Aplicativos|Convites do Lote processados|
|Gerenciamento de Aplicativos|Convites do Lote carregados|
|Gerenciamento de Aplicativos|Adicionar proprietário à política|
|Gerenciamento de Aplicativos|Adicionar política|
|Gerenciamento de Aplicativos|Excluir política|
|Gerenciamento de Aplicativos|Remover credenciais de política|
|Gerenciamento de Aplicativos|Atualizar política|
|Gerenciamento de Aplicativos|Definir política de registro MFA|
|Gerenciamento de Aplicativos|Definir política de risco de entrada|
|Gerenciamento de Aplicativos|Definir política de risco do usuário|
|Gerenciamento de Aplicativos|Aceitar os Termos de Uso|
|Gerenciamento de Aplicativos|Criar Termos de Uso|
|Gerenciamento de Aplicativos|Recusar Termos de Uso|
|Gerenciamento de Aplicativos|Excluir Termos de Uso|
|Gerenciamento de Aplicativos|Editar Termos de Uso|
|Gerenciamento de Aplicativos|Publicar Termos de Uso|
|Gerenciamento de Aplicativos|Cancelar a publicação dos Termos de Uso|
|Gerenciamento de Aplicativos|Adicionar certificado SSL do aplicativo|
|Gerenciamento de Aplicativos|Excluir associação SSL|
|Gerenciamento de Aplicativos|Registrar conector|
|Gerenciamento de Aplicativos|AdminPolicyDatas-RemoveResources|
|Gerenciamento de Aplicativos|AdminPolicyDatas-SetResources|
|Gerenciamento de Aplicativos|AdminUserJourneys-GetResources|
|Gerenciamento de Diretório|AdminUserJourneys-RemoveResources|
|Gerenciamento de Diretório|AdminUserJourneys-SetResources|
|Gerenciamento de Diretório|Criar IdentityProvider|
|Gerenciamento de Diretório|Criar um novo AdminUserJourney|
|Gerenciamento de Diretório|Criar json de recurso localizado|
|Gerenciamento de Diretório|Criar novo IDP personalizado|
|Gerenciamento de Diretório|Criar novo IDP|
|Gerenciamento de Diretório|Criar ou atualizar um recurso de diretório do B2C|
|Gerenciamento de Diretório|Criar política|
|Gerenciamento de Diretório|Criar política trustFramework|
|Gerenciamento de Diretório|Criar política trustFramework com prefixo configurável|
|Gerenciamento de Diretório|Criar atributo de usuário|
|Gerenciamento de Diretório|CreateTrustFrameworkPolicy|
|Gerenciamento de Diretório|Excluir IDP|
|Gerenciamento de Diretório|Excluir IdentityProvider|
|Gerenciamento de Diretório|Excluir um recurso do diretório do B2C|
|Gerenciamento de Diretório|Excluir política trustFramework|
|Gerenciamento de Diretório|Excluir atributo de usuário|
|Gerenciamento de Diretório|Obter recursos de diretório do B2C em um grupo de recursos|
|Gerenciamento de Diretório|Obter os recursos de diretório do B2C em uma assinatura|
|Gerenciamento de Diretório|Obter IDP personalizado|
|Gerenciamento de Diretório|Obter IDP|
|Gerenciamento de Diretório|Obter recurso do Active Directory B2C|
|Gerenciamento de Diretório|Obter um Percurso do Usuário|
|Gerenciamento de Diretório|Obter declarações do aplicativo permitidas para o Percurso do Usuário|
|Gerenciamento de Diretório|Obter declarações autodeclaradas permitidas para o Percurso do Usuário|
|Gerenciamento de Diretório|Obter declarações autodeclaradas permitidas de política|
|Gerenciamento de Diretório|Obter lista de declarações de saída disponíveis|
|Gerenciamento de Diretório|Obter definições de conteúdo para o Percurso do Usuário|
|Gerenciamento de Diretório|Obter IDPs para um fluxo específico do administrador|
|Gerenciamento de Diretório|Obter lista de todos os fluxos de administrador|
|Gerenciamento de Diretório|Obter lista de marcas de todos os fluxos de administrador para todos os usuários|
|Gerenciamento de Grupo|Membros do grupo de download em massa-iniciados|
|Gerenciamento de Grupo|Membros do grupo de download em massa-concluído|
|Gerenciamento de Grupo|Membros do grupo de importação em massa-iniciados|
|Gerenciamento de Grupo|Membros do grupo de importação em massa-concluídos|
|Gerenciamento de Grupo|Remoção de membros do grupo em massa-iniciada|
|Gerenciamento de Grupo|Remoção de membros do grupo em massa – concluída|
|Gerenciamento de Grupo|Grupos de download em massa-iniciados|
|Gerenciamento de Grupo|Grupos de download em massa-concluídos|
|Gerenciamento de Grupo|Obter lista de locatários de um usuário|
|Gerenciamento de Grupo|Obter declarações autodeclaradas das contas locais|
|Gerenciamento de Grupo|Obter json de recurso localizado|
|Gerenciamento de Grupo|Obter operações do provedor de recursos Microsoft.AzureActiveDirectory|
|Gerenciamento de Grupo|Obter políticas|
|Gerenciamento de Grupo|Obter política|
|Gerenciamento de Grupo|Obter lista de IDP com suporte|
|Gerenciamento de Grupo|Obter lista de IDP com suporte do Percurso do Usuário|
|Gerenciamento de Grupo|Obter lista de IDPs personalizadas definida pelo locatário|
|Gerenciamento de Grupo|Obter lista de IDPs definida pelo locatário|
|Gerenciamento de Grupo|Obter lista de IDPs locais definida pelo locatário|
|Gerenciamento de Grupo|Obter detalhes do locatário de um usuário para a criação de recursos|
|Gerenciamento de Grupo|Obter a cultura com suporte padrão para CPIM|
|Gerenciamento de Grupo|Obter os detalhes de um fluxo de administrador|
|Gerenciamento de Grupo|Obter a lista de UserJourneys para o locatário|
|Gerenciamento de Grupo|Obter o conjunto de culturas com suporte disponíveis para CPIM|
|Gerenciamento de Grupo|Obter política trustFramework|
|Gerenciamento de Grupo|Obter política trustFramework como xml|
|Gerenciamento de Grupo|Editar atributo de usuário|
|Gerenciamento de Política|Obter atributos de usuário|
|Gerenciamento de Política|Obter lista de Percurso do Usuário|
|Gerenciamento de Política|GetIEFPolicies|
|Gerenciamento de Política|GetIdentityProviders|
|Gerenciamento de Política|GetTrustFrameworkPolicy|
|Recurso|MigrateTenantMetadata|
|Recurso|Mover recursos|
|Recurso|Fazer patch de IdentityProvider|
|Recurso|PutTrustFrameworkPolicy|
|Recurso|PutTrustFrameworkpolicy|
|Recurso|Remover um Percurso do Usuário|
|Recurso|Atualizar IDP personalizado|
|Recurso|Atualizar IDP|
|Recurso|Atualizar IDP Local|
|Recurso|Exibir um recurso de diretório do B2C|
|Recurso|Atualizar política|
|Recurso|Atualizar status da assinatura|
|Gerenciamento de funções|Atualizar atributo de usuário|
|Gerenciamento de funções|Validar movimentação de recursos|
|Gerenciamento de funções|Adicionar dispositivo|
|Gerenciamento de funções|Adicionar configuração do dispositivo|
|Gerenciamento de funções|Adicionar proprietário registrado ao dispositivo|
|Gerenciamento de funções|Adicionar usuários registrados ao dispositivo|
|Gerenciamento de funções|Excluir dispositivo|
|Gerenciamento de funções|Excluir configuração de dispositivo|
|Gerenciamento de funções|O dispositivo não é mais compatível|
|Gerenciamento de funções|O dispositivo não é mais gerenciado|
|Gerenciamento do Usuário|AccessReview_Review|
|Gerenciamento do Usuário|AccessReview_Update|
|Gerenciamento do Usuário|ActivationAborted|
|Gerenciamento do Usuário|ActivationApproved|
|Gerenciamento do Usuário|ActivationCanceled|
|Gerenciamento do Usuário|ActivationRequested|
|Gerenciamento do Usuário|Adicionar membro qualificado à função|
|Gerenciamento do Usuário|Adicionar membro à função|
|Gerenciamento do Usuário|Adicionar atribuição de função à definição de função|
|Gerenciamento do Usuário|Adicionar função originada no modelo|
|Gerenciamento do Usuário|Adicionar membro no escopo à função|
|Gerenciamento do Usuário|Added|
|Gerenciamento do Usuário|Assign|
|Gerenciamento do Usuário|Criação de usuários em massa-iniciada|
|Gerenciamento do Usuário|Criação de usuários em massa – concluída|
|Gerenciamento do Usuário|Exclusão de usuários em massa – iniciada|
|Gerenciamento do Usuário|Exclusão de usuários em massa concluída|
|Gerenciamento do Usuário|Usuários do download em massa-iniciados|
|Gerenciamento do Usuário|Usuários do download em massa-concluídos|
|Gerenciamento do Usuário|Restauração em massa de usuários excluídos-iniciado|
|Gerenciamento do Usuário|Restauração em massa de usuários excluídos-concluído|
|Gerenciamento do Usuário|Usuários de convite em massa-iniciados|
|Gerenciamento do Usuário|Usuários de convite em massa-concluídos|
|Gerenciamento do Usuário|Remover proprietário registrado do dispositivo|
|Gerenciamento do Usuário|Remover usuários registrados do dispositivo|
|Gerenciamento do Usuário|Remover membro qualificado da função|
|Gerenciamento do Usuário|Remover o membro da função|
|Gerenciamento do Usuário|Remover atribuição de função da definição de função|
|Gerenciamento do Usuário|Remover membro no escopo da função|
|Gerenciamento do Usuário|Atualizar dispositivo|
|Gerenciamento do Usuário|Atualizar configuração do dispositivo|
|Gerenciamento do Usuário|Atualizar função|






## <a name="identity-protection"></a>Identity Protection

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Diretório|Elevar|
|Gerenciamento de Diretório|Removido|
|Gerenciamento de Diretório|Alterações na configuração de função|
|Outros|ScanAlertsNow|
|Outros|Inscrição|
|Outros|Unelevate|
|Outros|UpdateAlertSettings|
|Outros|UpdateCurrentState|
|Gerenciamento de Política|Revisão de acesso encerrada|
|Gerenciamento de Política|Adicionar aprovador para a aprovação de solicitação|
|Gerenciamento de Política|Adicionar revisor para a revisão de acesso|
|Gerenciamento do Usuário|Aplicar revisão do acesso|
|Gerenciamento do Usuário|Criar revisão do acesso|


## <a name="invited-users"></a>Usuários convidados

|Auditar categoria|Atividade|
|---|---|
|Outros|Criar solicitação de aprovação|
|Outros|Excluir a revisão de acesso|
|Gerenciamento do Usuário|Remover revisor da revisão de acesso|
|Gerenciamento do Usuário|Solicitação de aplicação do resultado da revisão|
|Gerenciamento do Usuário|Solicitar interrupção da análise|
|Gerenciamento do Usuário|Revisar atribuição do aplicativo|
|Gerenciamento do Usuário|Revisar associação ao grupo|
|Gerenciamento do Usuário|Examinar associação de função RBAC|


## <a name="microsoft-identity-manager-mim"></a>MIM (Microsoft Identity Manager)

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Grupo|Revisar solicitação de aprovação de solicitação|
|Gerenciamento de Grupo|Atualizar revisão de acesso|
|Gerenciamento de Grupo|Atualizar configurações de notificação de email da revisão de acesso|
|Gerenciamento de Grupo|Atualizar a configuração de contagem de recorrências da revisão de acesso|
|Gerenciamento de Grupo|Atualizar a duração de recorrência da revisão de acesso na configuração de dias|
|Gerenciamento do Usuário|Atualizar a configuração do tipo final de recorrência da revisão de acesso|
|Gerenciamento do Usuário|Atualizar a configuração do tipo de recorrência da revisão de acesso|



## <a name="privileged-identity-management"></a>Privileged Identity Management

|Auditar categoria|Atividade|
|---|---|
|PIM|ActivationAborted|
|PIM|ActivationApproved|
|PIM|ActivationCanceled|
|PIM|ActivationDenied|
|PIM|ActivationRequested|
|PIM|Added|
|PIM|AddedOutsidePIM|
|PIM|Assign|
|PIM|DismissAlert|
|PIM|Elevar|
|PIM|ReactivateAlert|
|PIM|Removido|
|PIM|RemovedOutsidePIM|
|PIM|Solicitar interrupção da análise|
|PIM|Alterações na configuração de função|
|PIM|ScanAlertsNow|
|PIM|Inscrição|
|PIM|Unassign|
|PIM|Unelevate|
|PIM|UpdateAlertSettings|
|PIM|UpdateCurrentState|


## <a name="self-service-group-management"></a>Gerenciamento de grupo de autoatendimento

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Grupo|Redefinir a senha do usuário|
|Gerenciamento de Grupo|Restaurar usuário|
|Gerenciamento de Grupo|Definir alteração forçada de senha de usuário|
|Gerenciamento de Grupo|Definir gerenciador de usuários|
|Gerenciamento de Grupo|Definir metadados de token OAuth de usuários como habilitados|
|Gerenciamento de Grupo|Atualizar carimbo de data/hora StsRefreshTokenValidFrom|
|Gerenciamento de Grupo|Atualizar segredos externos|
|Gerenciamento de Grupo|Atualizar usuário|
|Gerenciamento de Grupo|O administrador gera uma senha temporária|


## <a name="self-service-password-management"></a>Gerenciamento de senhas de autoatendimento

|Auditar categoria|Atividade|
|---|---|
|Gerenciamento de Diretório|Os administradores exigem que o usuário redefina a senha|
|Gerenciamento de Diretório|Atribuir usuário externo ao aplicativo|
|Gerenciamento do Usuário|Email não enviado; o usuário cancelou a assinatura|
|Gerenciamento do Usuário|Convidar usuário externo|
|Gerenciamento do Usuário|Resgatar convite para usuário externo|
|Gerenciamento do Usuário|Criação de locatário viral|
|Gerenciamento do Usuário|Criação de usuário viral|
|Gerenciamento do Usuário|Registro de senha do usuário|
|Gerenciamento do Usuário|Redefinição de senha do usuário|
|Gerenciamento do Usuário|Impedido de executar a redefinição de senha de autoatendimento|


## <a name="terms-of-use"></a>Termos de uso

|Auditar categoria|Atividade|
|---|---|
|Termos de Uso|Aceitar os Termos de Uso|
|Termos de Uso|Criar Termos de Uso|
|Termos de Uso|Recusar Termos de Uso|
|Termos de Uso|Excluir Consentimento|
|Termos de Uso|Excluir Termos de Uso|
|Termos de Uso|Editar Termos de Uso|
|Termos de Uso|Expirar Termos de Uso|
|Termos de Uso|Exclusão irreversível dos Termos de Uso|
|Termos de Uso|Publicar Termos de Uso|
|Termos de Uso|Cancelar a publicação dos Termos de Uso|


## <a name="next-steps"></a>Próximas etapas

- [Visão geral dos relatórios do Azure AD](overview-reports.md).
- [Relatório de logs de auditoria](concept-audit-logs.md). 
- [Acesso programático aos relatórios do Azure AD](concept-reporting-api.md)

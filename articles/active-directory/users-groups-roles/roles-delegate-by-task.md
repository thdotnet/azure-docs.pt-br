---
title: Delegar funções com privilégios mínimos por tarefa de administrador - Azure Active Directory | Microsoft Docs
description: Funções para delegar para tarefas de identidade no Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 05/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3799496d13259c943847625a2cf6a39a8edb1d35
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/01/2019
ms.locfileid: "70207250"
---
# <a name="administrator-roles-by-admin-task-in-azure-active-directory"></a>Funções de administrador por tarefa de administrador no Azure Active Directory

Neste artigo, você poderá encontrar as informações necessárias para restringir as permissões de administrador de um usuário, atribuindo funções com privilégios mínimos no Azure AD (Azure Active Directory). Você encontrará tarefas de administrador organizadas por área de recurso e a função com privilégios mínimos necessária para executar cada tarefa, além de funções de Administrador não global adicionais que podem executar a tarefa.

## <a name="application-proxy"></a>Proxy do aplicativo

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar aplicativo proxy do aplicativo | Administrador de aplicativos | 
Configurar propriedades do grupo de conectores | Administrador de aplicativos | 
Criar registro de aplicativo quando a capacidade estiver desabilitada para todos os usuários | Desenvolvedor de aplicativos | Administrador de aplicativos de nuvem, Administrador de aplicativos
Criar grupo de conectores | Administrador de aplicativos | 
Excluir grupo de conectores | Administrador de aplicativos | 
Desabilitar proxy de aplicativo | Administrador de aplicativos | 
Baixar o serviço do conector | Administrador de aplicativos | 
Ler todas as configurações | Administrador de aplicativos | 

## <a name="b2c"></a>B2C

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Criar diretórios do Azure AD B2C | Todos os usuários não convidados ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Criar aplicativos B2C | Administrador Global | 
Criar aplicativos corporativos | Administrador de Aplicativos de Nuvem | Administrador de Aplicativos
Criar, ler, atualizar e excluir políticas de B2C | Administrador Global | 
Criar, ler, atualizar e excluir provedores de identidade | Administrador Global | 
Criar, ler, atualizar e excluir fluxos de usuários de redefinição de senha | Administrador Global | 
Criar, ler, atualizar e excluir fluxos de usuários de edição de perfil | Administrador Global | 
Criar, ler, atualizar e excluir fluxos de usuários de entrada | Administrador Global | 
Criar, ler, atualizar e excluir fluxo de usuários de entrada |Administrador Global | 
Criar, ler, atualizar e excluir atributos de usuário | Administrador Global | 
Criar, ler, atualizar e excluir usuários | Administrador global ([consulte a documentação](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs))
Ler todas as configurações | Administrador Global | 
Ler os logs de auditoria do B2C | Administrador global ([consulte a documentação](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs)) | 

> [!NOTE]
> Azure AD B2C administradores globais não têm as mesmas permissões que os administradores globais do Azure AD. Se você tiver Azure AD B2C privilégios de administrador global, verifique se você está em um diretório Azure AD B2C e não em um diretório do AD do Azure.

## <a name="company-branding"></a>Identidade visual da empresa

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar a identidade visual da empresa | Administrador Global | 
Ler todas as configurações | Leitores de diretórios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="company-properties"></a>Propriedades da empresa

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar propriedades da empresa | Administrador Global | 

## <a name="connect"></a>Conectar

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Autenticação de passagem | Administrador Global | 
Ler todas as configurações | Administrador Global | 
Logon único contínuo | Administrador Global | 

## <a name="connect-health"></a>Connect Health

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Adicionar ou excluir serviços | Proprietário ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Aplicar correções para erro de sincronização | Colaborador ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Proprietário
Configurar notificações | Colaborador ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Proprietário
Definir Configurações | Proprietário ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Configurar notificações de sincronização | Colaborador ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Proprietário
Ler os relatórios de segurança do ADFS | Leitor de Segurança | Colaborador, Proprietário
Ler todas as configurações | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Ler os erros de sincronização | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Ler os serviços de sincronização | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Exibir métricas e alertas | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Exibir métricas e alertas | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Exibir métricas e alertas do serviço de sincronização | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário


## <a name="custom-domain-names"></a>Nomes de domínio personalizados

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Gerenciar domínios | Administrador Global | 
Ler todas as configurações | Leitores de diretórios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="domain-services"></a>Serviços de Domínio

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Criar instância do Azure AD Domain Services | Administrador Global | 
Executar todas as tarefas do Azure Active Directory Domain Services | Grupo Administradores do Azure AD DC ([consulte a documentação](../../active-directory-domain-services/tutorial-create-management-vm.md#administrative-tasks-you-can-perform-on-an-azure-ad-ds-managed-domain)) | 
Ler todas as configurações | Leitor na assinatura do Azure que contém o serviço AD DS | 

## <a name="devices"></a>Dispositivos

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Desabilitar dispositivo | Administrador de dispositivo em nuvem | 
Habilitar dispositivo | Administrador de dispositivo em nuvem | 
Ler a configuração básica | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Ler as chaves do BitLocker | Leitor de Segurança | Administrador de senhas, Administrador da segurança

## <a name="enterprise-applications"></a>Aplicativos empresariais

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Consentimento para quaisquer permissões delegadas | Administrador de aplicativos de nuvem | Administrador de aplicativos
Autorização para permissões de aplicativos que não incluem Microsoft Graph ou Microsoft Azure AD Graph | Administrador de aplicativos de nuvem | Administrador de aplicativos
Consentimento de permissões de aplicativos para Microsoft Graph ou Microsoft Azure AD Graph | Administrador Global | 
Consentimento para aplicativos acessando dados próprios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Criar aplicativos empresariais | Administrador de aplicativos de nuvem | Administrador de aplicativos
Gerenciar proxy de aplicativo | Administrador de aplicativos | 
Gerenciar configurações de usuário | Administrador Global | 
Revisão de acesso de leitura de um grupo ou de um aplicativo | Leitor de Segurança | Administrador de segurança, Administrador de usuários
Ler todas as configurações | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Atualizar atribuições de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar proprietários de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar propriedades de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar provisionamento de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar autoatendimento de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar propriedades de logon único | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos

## <a name="entitlement-management"></a>Gerenciamento de direitos
Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Adicionar recursos a um catálogo | Administrador de usuários | Com o gerenciamento de direitos, você pode delegar essa tarefa para o proprietário do catálogo ([consulte a documentação](../governance/entitlement-management-delegate.md#add-a-catalog-owner-or-an-access-package-manager))
Adicionar sites do SharePoint Online ao catálogo | Administrador global


## <a name="groups"></a>Grupos

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Atribuir licença | Administrador de usuários | 
Criar grupo | Administrador de usuários | 
Criar, atualizar ou excluir a revisão de acesso de um grupo ou de um aplicativo | Administrador de usuários | 
Gerenciar expiração de grupo | Administrador de usuários | 
Gerenciar configurações de grupo | Administrador Global | 
Ler todas as configurações (exceto associação oculta) | Leitores de diretórios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Ler associação oculta | Membro do grupo | Proprietário do grupo, administrador de senhas, administrador do Exchange, administrador do SharePoint, administrador de equipes, administrador de usuários
Ler membros de grupos com membros ocultos | Administrador de Assistência Técnica | Administrador de usuários, administrador de equipes
Revogar licença | Administrador de licenças | Administrador de usuários
Atualizar associação de grupo | Proprietário do grupo ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador de usuários
Atualizar proprietários do grupo | Proprietário do grupo ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador de usuários
Atualizar propriedades do grupo | Proprietário do grupo ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador de usuários

## <a name="identity-protection"></a>Identity Protection

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar notificações de alerta| Administrador de Segurança | 
Configurar e habilitar ou desabilitar política de MFA| Administrador de Segurança | 
Configurar e habilitar ou desabilitar política de risco de entrada| Administrador de Segurança | 
Configurar e habilitar ou desabilitar política de risco do usuário | Administrador de Segurança | 
Configurar resumos semanais | Administrador de Segurança| 
Ignorar todas as detecções de risco | Administrador de Segurança | 
Corrigir ou ignorar vulnerabilidade | Administrador de Segurança | 
Ler todas as configurações | Leitor de Segurança | 
Ler todas as detecções de riscos | Leitor de Segurança | 
Ler vulnerabilidades | Leitor de Segurança | 

## <a name="licenses"></a>Licenças

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Atribuir licença | Administrador de licenças | Administrador de usuários
Ler todas as configurações | Leitores de diretórios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Revogar licença | Administrador de licenças | Administrador de usuários
Experimentar ou comprar uma assinatura | Administrador de cobrança | 


## <a name="monitoring---audit-logs"></a>Monitoramento - Log de auditoria

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Ler logs de auditoria | Leitor de relatórios | Leitor de segurança, Administrador da segurança

## <a name="monitoring---sign-ins"></a>Monitoramento - Entradas

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Ler logs de entrada | Leitor de relatórios | Leitor de segurança, Administrador da segurança

## <a name="multi-factor-authentication"></a>Autenticação multifator

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Excluir todas as senhas de aplicativos existentes geradas pelos usuários selecionados | Administrador Global | 
Desabilitar MFA | Administrador Global | 
Habilitar MFA | Administrador Global | 
Gerenciar configurações do serviço de MFA | Administrador Global | 
Exigir que os usuários selecionados forneçam métodos de contato novamente | Administrador de Autenticação | 
Restaurar a autenticação multifator em todos os dispositivos lembrados  | Administrador de Autenticação | 

## <a name="mfa-server"></a>Servidor MFA

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Bloquear/desbloquear usuários | Administrador Global | 
Configurar bloqueio de conta | Administrador Global | 
Configurar regras de cache | Administrador Global | 
Configurar alerta de fraude | Administrador Global
Configurar notificações | Administrador Global | 
Configurar bypass avulso | Administrador Global | 
Definir configurações de chamada telefônica | Administrador Global | 
Configurar provedores | Administrador Global | 
Definir configurações do servidor | Administrador Global | 
Ler relatório de atividades | Administrador Global | 
Ler todas as configurações | Administrador Global | 
Ler o status do servidor | Administrador Global |  

## <a name="organizational-relationships"></a>Relações organizacionais

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Gerenciar provedores de identidade | Administrador Global | 
Gerenciar configurações | Administrador Global | 
Gerenciar termos de uso | Administrador Global | 
Ler todas as configurações | Administrador Global | 

## <a name="password-reset"></a>Redefinição de senha

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar métodos de autenticação | Administrador Global |
Configurar personalização | Administrador Global |
Configurar notificação | Administrador Global |
Configurar integração local | Administrador Global |
Configurar propriedades de redefinição de senha | Administrador de Usuários | Administrador Global
Configurar registro | Administrador Global |
Ler todas as configurações | Administrador de Segurança | Administrador de Usuários |

## <a name="privileged-identity-management"></a>Privileged Identity Management

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Atribuir usuários a funções | Administrador de funções com privilégios | 
Definir configurações de função | Administrador de funções com privilégios | 
Exibir atividade de auditoria | Leitor de segurança | 
Exibir associações de função | Leitor de segurança | 

## <a name="roles-and-administrators"></a>Funções e administradores

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Gerenciar atribuições de função | Administrador de funções com privilégios | 
Revisão de acesso de leitura de uma função do Azure AD  | Leitor de Segurança | Administrador da segurança, Administrador de função com privilégios
Ler todas as configurações | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 

## <a name="security---authentication-methods"></a>Segurança - Métodos de Autenticação

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar métodos de autenticação | Administrador Global | 
Ler todas as configurações | Administrador Global | 

## <a name="security---conditional-access"></a>Segurança-acesso condicional

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar endereços IP confiáveis de MFA | Administrador de Acesso Condicional | 
Criar controles personalizados | Administrador de Acesso Condicional | Administrador de segurança
Criar locais nomeados | Administrador de Acesso Condicional | Administrador de segurança
Criar políticas | Administrador de Acesso Condicional | Administrador de segurança
Criar termos de uso | Administrador de Acesso Condicional | Administrador de segurança
Criar certificado de conectividade VPN | Administrador de Acesso Condicional | Administrador de segurança
Excluir política clássica | Administrador de Acesso Condicional | Administrador de segurança
Excluir termos de uso | Administrador de Acesso Condicional | Administrador de segurança
Excluir certificado de conectividade VPN | Administrador de Acesso Condicional | Administrador de segurança
Desabilitar política clássica | Administrador de Acesso Condicional | Administrador de segurança
Gerenciar controles personalizados | Administrador de Acesso Condicional | Administrador de segurança
Gerenciar locais nomeados | Administrador de Acesso Condicional | Administrador de segurança
Gerenciar termos de uso | Administrador de Acesso Condicional | Administrador de segurança
Ler todas as configurações | Leitor de segurança | Administrador de segurança
Ler localizações nomeadas | Leitor de segurança | Administrador de acesso condicional, administrador de segurança

## <a name="security---identity-security-score"></a>Segurança - Pontuação de segurança de identidade

Tarefa | Função com privilégios mínimos | Funções adicionais | 
---- | --------------------- | ----------------
Ler todas as configurações | Leitor de segurança | Administrador de segurança
Ler pontuação de segurança | Leitor de segurança | Administrador de segurança
Atualizar status do evento | Administrador de segurança | 

## <a name="security---risky-sign-ins"></a>Segurança - Entradas arriscadas

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Ler todas as configurações | Leitor de Segurança | 
Ler as entradas arriscadas | Leitor de Segurança | 

## <a name="security---users-flagged-for-risk"></a>Segurança - Usuários sinalizados para risco

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Ignorar todos os eventos | Administrador de Segurança | 
Ler todas as configurações | Leitor de Segurança | 
Ler usuários sinalizados para risco | Leitor de Segurança | 

## <a name="users"></a>Usuários

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Adicionar usuário à função do diretório | Administrador de funções com privilégios | 
Adicionar usuário a grupo | Administrador de usuários | 
Atribuir licença | Administrador de licenças | Administrador de usuários
Criar um usuário convidado | Emissor de convites independente | Administrador de usuários
Criar usuário | Administrador de usuários | 
Excluir usuários | Administrador de usuários | 
Invalidar tokens de atualização de administradores limitados (consulte a documentação) | Administrador de usuários | 
Invalidar tokens de atualização de não administradores (consulte a documentação) | Administrador de senhas | Administrador de usuários
Invalidar tokens de atualização de administradores com privilégios (consulte a documentação) | Administrador Global | 
Ler a configuração básica | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions) | 
Redefinir senha para administradores limitados (consulte a documentação) | Administrador de usuários | 
Redefinir senha de não administradores (consulte a documentação) | Administrador de senhas | Administrador de usuários
Redefinir senha de administradores com privilégios | Administrador Global | 
Revogar licença | Administrador de licenças | Administrador de usuários
Atualizar todas as propriedades, exceto Nome UPN | Administrador de usuários | 
Atualizar nome UPN para administradores limitados (consulte a documentação) | Administrador de usuários | 
Atualizar a propriedade do nome UPN em administradores com privilégios (consulte a documentação) | Administrador Global | 
Atualizar configurações do usuário | Administrador Global | 


## <a name="support"></a>Suporte

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Enviar tíquete de suporte | Administrador de Serviços | Administrador de aplicativos, administrador da proteção de informações do Azure, administrador de cobrança, administrador de aplicativos de nuvem, administrador de conformidade, administrador do Dynamics 365, administrador de análise de desktop, administrador do Exchange, senha Administrador, administrador do Intune, administrador do Skype for Business, administrador de Power BI, administrador de autenticação privilegiada, administrador do SharePoint, administrador de comunicações de equipes, administrador de equipes, administrador de usuários, Administrador do workplace Analytics

## <a name="next-steps"></a>Próximas etapas

* [Como atribuir ou remover funções de administrador do Azure AD](directory-manage-roles-portal.md)
* [Referência das funções de administrador do Azure AD](directory-assign-admin-roles.md)

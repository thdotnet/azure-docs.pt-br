---
title: Solucionar problemas de acesso ao Portal do EA do Azure
description: Este artigo descreve alguns problemas comuns que podem ocorrer com um EA (Contrato Enterprise do Azure) no Portal do EA do Azure.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/09/2019
ms.topic: troubleshooting
ms.service: billing
manager: boalcsva
ms.openlocfilehash: a2214ef0dd3b7c4f706ebbb529b6c44e956f9242
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70900872"
---
# <a name="troubleshoot-azure-ea-portal-access"></a>Solucionar problemas de acesso ao Portal do EA do Azure

Este artigo descreve alguns problemas comuns que podem ocorrer com um EA (Contrato Enterprise do Azure). O Portal do EA do Azure é usado para gerenciar os custos e usuários do Contrato Enterprise. Você pode ter esses problemas quando estiver configurando ou atualizando o acesso ao Portal do EA do Azure.

## <a name="issues-adding-an-admin-to-an-enrollment"></a>Problemas ao adicionar um administrador a um registro

Há diferentes tipos de níveis de autenticação para Registros Enterprise. Quando os níveis de autenticação são aplicados incorretamente, você pode ter problemas ao tentar entrar no Portal do EA do Azure.

Você usa o Portal do EA do Azure para permitir acesso a usuários com níveis de autenticação diferentes. Um administrador corporativo pode atualizar o nível de autenticação para atender aos requisitos de segurança da organização dele.

### <a name="authentication-level-types"></a>Tipos de nível de autenticação

- Somente conta Microsoft – para organizações que desejam usar, criar e gerenciar usuários por meio de contas Microsoft.
- Conta corporativa ou de estudante – para organizações que configuraram Active Directory com federação para a nuvem e todas as contas estão em um único locatário.
- Conta corporativa ou de estudante entre locatários – para organizações que configuraram Active Directory com federação para a nuvem e terão contas em vários locatários.
- Conta mista – permite que você adicione usuários com conta Microsoft e/ou com uma conta corporativa ou de estudante.

A primeira conta corporativa ou de estudante adicionada ao registro determina o domínio _padrão_ ou _mestre_. Para adicionar uma conta corporativa ou de estudante com outro locatário, você precisa alterar o nível de autenticação no registro para autenticação entre locatários.

Para atualizar o nível de autenticação:

1. Entrar no Portal do EA do Azure como um administrador corporativo.
2. Clique em **Gerenciar** no painel de navegação à esquerda.
3. Clique na guia **Registro**.
4. Em **Detalhes do Registro**, selecione **Nível de Autenticação**.
5. Clique no símbolo de lápis.
6. Clique em **Save** (Salvar).

![Exemplo mostrando níveis de autenticação ](./media/billing-ea-portal-troubleshoot/create-ea-authentication-level-types.png)

As contas Microsoft precisam ter uma ID associada criada em [https://signup.live.com](https://signup.live.com/).

Contas corporativas ou de estudante estão disponíveis para organizações que configuraram um Active Directory com federação e em que todas as contas estão em um único locatário. Os usuários poderão ser adicionados com a autenticação de usuário federado corporativa ou de estudante se o Active Directory interno da empresa for federado.

Se a sua organização não usar a federação do Active Directory, você não poderá usar seu endereço corporativo ou de estudante. Em vez disso, registre ou crie um novo endereço de email e registre-o como uma conta Microsoft.

## <a name="unable-to-access-the-azure-ea-portal"></a>Não é possível acessar o Portal do EA do Azure

Se você receber uma mensagem de erro ao tentar entrar no Portal do EA do Azure, use as seguintes etapas de solução de problemas:

- Verifique se você está usando a URL correta do Portal do EA do Azure, que é https://ea.azure.com.
- Determine se o acesso ao Portal do EA do Azure foi adicionado como uma conta corporativa ou de estudante ou como uma conta Microsoft.
  - Se você estiver usando sua conta corporativa, insira seu email de trabalho e senha. A senha de trabalho é fornecida pela organização. Se tiver problemas com a senha, você poderá verificar com o departamento de TI sobre como redefini-la.
  - Se você estiver usando uma conta Microsoft, insira seu endereço de email e senha da conta Microsoft. Se você tiver esquecido sua senha da conta Microsoft, poderá redefini-la em [https://account.live.com/password/reset](https://account.live.com/password/reset).
- Use uma sessão de navegador em modo incógnito ou particular para entrar, de modo que nenhum cookie ou informações em cache de sessões anteriores ou existentes sejam mantidos. Limpe o cache do navegador e use uma janela em modo incógnito ou particular para abrir https://ea.azure.com.
- Se você receber um erro de _Usuário Inválido_ ao usar uma conta Microsoft, isso talvez se deva a você ter várias contas Microsoft. Aquela que você está tentando usar para entrar não é o endereço de email principal.
Ou, se você receber um erro de _Usuário Inválido_, poderá ser porque o tipo de conta incorreto foi usado quando o usuário foi adicionado ao registro. Por exemplo, uma conta corporativa ou de estudante em vez de uma conta Microsoft. Neste exemplo, você solicita que outro administrador do EA adicione a conta correta ou há necessidade de entrar em contato com o [suporte](https://support.microsoft.com/supportforbusiness/productselection?sapId=cf791efa-485b-95a3-6fad-3daf9cd4027c).

## <a name="no-activation-email-received"></a>Nenhum email de ativação recebido

Um email de ativação do Portal do EA do Azure é enviado do *waep@microsoft.com* . Se você não tiver recebido um email de ativação, verifique sua pasta de spam ou lixo eletrônico em busca do email. Ele é enviado com o assunto _Convite para exibir/gerenciar o serviço do Microsoft Azure_. Ele é enviado para cada administrador de EA recém-adicionado.

Se tiver certeza de que foi configurado como o administrador do EA, você não precisará aguardar o recebimento do email de ativação para entrar no Portal do EA do Azure. Em vez disso, você pode acessar https://ea.azure.com e entrar com seu endereço de email (conta Microsoft, corporativa ou de estudante) e senha.

Se você precisar verificar o alias principal, vá para [https://account.live.com](https://account.live.com). Em seguida, clique em **Suas Informações** e, em seguida, clique em **Gerenciar o modo de entrar na Microsoft**. Siga os avisos para verificar um endereço de email alternativo e obter um código para acessar informações confidenciais. Digite o código de segurança. Selecione **Configurar mais tarde** se você não quiser configurar a autenticação de dois fatores.

Você verá a página **Gerenciar o modo de entrar na Microsoft**, na qual é possível exibir os aliases de conta que você tem. Verifique se o alias principal é aquele que você está usando para entrar no Portal do EA do Azure. Se não for, você poderá torná-lo seu alias principal. Ou então, em vez disso, você pode usar o alias principal para o Portal do EA do Azure.

## <a name="next-steps"></a>Próximas etapas

- Os administradores do Portal do EA do Azure devem ler [Administração do Portal do EA do Azure](billing-ea-portal-administration.md) para aprender sobre tarefas administrativas comuns.

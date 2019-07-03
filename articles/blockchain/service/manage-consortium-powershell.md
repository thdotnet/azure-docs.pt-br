---
title: Gerenciar membros em um consórcio do serviço de Blockchain do Azure usando o PowerShell do Azure
description: Saiba como gerenciar os membros do serviço de Blockchain do Azure consortium por meio do PowerShell do Azure.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/10/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 4bb72bc3fe8b85a8d2aed88e02f5f3150abb6899
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66493659"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-by-using-powershell"></a>Gerenciar membros consortium no serviço de Blockchain do Azure usando o PowerShell

Você pode usar o PowerShell para gerenciar membros do blockchain consortium para o serviço de Blockchain do Azure. Membros que têm privilégios de administrador podem convidar, adicionar, remover e alterar as funções para todos os participantes de consórcio blockchain. Membros que têm privilégios de usuário podem exibir todos os participantes de consórcio blockchain e alterar seu nome de exibição de membro.

## <a name="prerequisites"></a>Pré-requisitos

* Criar um membro de blockchain, usando o [portal do Azure](create-member.md).
* Para obter mais informações sobre consórcios, membros e nós, consulte [consortium de serviço do Azure Blockchain](consortium.md).

## <a name="open-azure-cloud-shell"></a>Abrir o Azure Cloud Shell

O Azure Cloud Shell é um shell interativo gratuito que pode ser usado para executar as etapas neste artigo. Ele tem ferramentas do Azure instaladas e configuradas para usar com sua conta.

Você também pode abrir o Cloud Shell em uma guia separada do navegador, vá para [shell.azure.com/powershell](https://shell.azure.com/powershell). Selecione **cópia** para copiar os blocos de código, cole-a no Cloud Shell e selecione **Enter** para executá-lo.

## <a name="install-the-powershell-module"></a>Instalar o módulo do PowerShell

Instale o pacote de Microsoft.AzureBlockchainService.ConsortiumManagement.PS da Galeria do PowerShell.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-the-information-preference"></a>Defina a preferência de informações

Você pode obter mais informações ao executar os cmdlets, definindo a variável de preferência de informações. Por padrão, *$InformationPreference* é definido como *SilentlyContinue*.

Para obter informações mais detalhadas dos cmdlets, defina a preferência no PowerShell, da seguinte maneira:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Estabelecer uma conexão Web3

Para gerenciar membros consortium, estabeleça uma conexão Web3 para seu ponto de extremidade de membro de Blockchain Service. Você pode usar esse script para definir variáveis globais para chamar os cmdlets de gerenciamento consortium.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

Substitua *\<senha da conta do membro\>* com a senha da conta de membro que você usou quando criou o membro.

Localize os outros valores no portal do Azure:

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Ir para o membro de serviço de Blockchain padrão **visão geral** página.

    ![Visão geral de membros](./media/manage-consortium-powershell/member-overview.png)

    Substitua *\<conta membro\>* e *\<RootContract endereço\>* com os valores do portal.

1. Para o endereço do ponto de extremidade, selecione **nós de transação**e, em seguida, selecione o **nó da transação padrão**. O nó padrão tem o mesmo nome que o membro de blockchain.
1. Selecione **Cadeias de conexão**.

    ![Cadeias de conexão](./media/manage-consortium-powershell/connection-strings.png)

    Substitua *\<endereço do ponto de extremidade\>* com o valor de **HTTPS (chave de acesso 1)** ou **HTTPS (chave de acesso 2)** .

## <a name="manage-the-network-and-smart-contracts"></a>Gerenciar a rede e contratos inteligentes

Use os cmdlets de contrato inteligente e de rede para estabelecer uma conexão para contratos inteligentes de um ponto de extremidade de blockchain responsáveis pelo gerenciamento consortium.

### <a name="import-consortiummanagementcontracts"></a>Import-ConsortiumManagementContracts

Use este cmdlet para se conectar a contratos inteligentes do gerenciamento de consortium. Esses contratos são usados para gerenciar e aplicar os membros dentro do consortium.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| RootContractAddress | Endereço do contrato raiz dos contratos inteligentes de gerenciamento consortium | Sim |
| Web3Client | Objeto Web3Client obtido de New-Web3Connection | Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>Importação Web3Account

Use este cmdlet para criar um objeto para manter as informações de conta de gerenciamento do nó remoto.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Endereço de conta de membro do Blockchain | Sim |
| ManagedAccountPassword | Senha da conta de endereço | Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>New-Web3Connection

Use este cmdlet para estabelecer uma conexão para o ponto de extremidade RPC de um nó de transação.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Endereço de ponto de extremidade de membro de Blockchain | Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="manage-the-consortium-members"></a>Gerenciar os membros consortium

Use cmdlets de gerenciamento membro consortium para gerenciar membros dentro do consortium. As ações disponíveis dependem da sua função consortium.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Use este cmdlet para obter detalhes de membro ou membros do consórcio da lista.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| Name | O nome do membro Blockchain Service que você deseja recuperar os detalhes sobre. Quando um nome for inserido, ele retorna detalhes do membro. Quando um nome é omitido, ele retorna uma lista de todos os membros de consortium. | Não |
| Members | Objeto de membros obtido de importação ConsortiumManagementContracts | Sim |
| Web3Client | Objeto Web3Client obtido de New-Web3Connection | Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

#### <a name="example-output"></a>Saída de exemplo

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Use este cmdlet para remover um membro de blockchain.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| Name | Nome do membro a ser removido | Sim |
| Members | Objeto de membros obtido de importação ConsortiumManagementContracts | Sim |
| Web3Account | Objeto Web3Account obtido de importação Web3Account | Sim |
| Web3Client | Objeto Web3Client obtido de New-Web3Connection | Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Use este cmdlet para definir atributos de membro, incluindo o nome de exibição e a função consortium de blockchain.

Os administradores do consórcio podem definir **DisplayName** e **função** para todos os membros. Um membro de consórcio com a função de usuário pode alterar apenas o nome de exibição do seu próprio membro.

`Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| Name | Nome do membro blockchain | Sim |
| DisplayName | Novo nome de exibição | Não |
| AccountAddress | Endereço da conta | Não |
| Members | Objeto de membros obtido de importação ConsortiumManagementContracts | Sim |
| Web3Account | Objeto Web3Account obtido de importação Web3Account | Sim |
| Web3Client |  Objeto Web3Client obtido de New-Web3Connection| Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="manage-the-consortium-members-invitations"></a>Gerenciar convites dos membros do consórcio

Use os cmdlets de gerenciamento de convite de membro consortium para gerenciar convites de membros consortium. As ações disponíveis dependem da sua função consortium.

### <a name="new-blockchainmemberinvitation"></a>New-BlockchainMemberInvitation

Use este cmdlet para convidar novos membros para o consortium.

`New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| SubscriptionId | ID de assinatura do Azure do membro para convidar | Sim |
| Role | A função consortium. Valores podem ser administrador ou usuário. ADMINISTRADOR é a função de administrador consortium. USUÁRIO é a função de membro consortium. | Sim |
| Members | Objeto de membros obtido de importação ConsortiumManagementContracts | Sim |
| Web3Account | Objeto Web3Account obtido de importação Web3Account | Sim |
| Web3Client | Objeto Web3Client obtido de New-Web3Connection | Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-BlockchainMemberInvitation

Use este cmdlet para recuperar ou listar o status do convite do membro consortium.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| SubscriptionId | A ID de assinatura do Azure do membro para convidar. Se a assinatura ID for fornecido, ele retorna a assinatura IDs detalhes do convite. Se a ID da assinatura for omitida, ele retorna uma lista de todos os convites de membro. | Não |
| Members | Objeto de membros obtido de importação ConsortiumManagementContracts | Sim |
| Web3Client | Objeto Web3Client obtido de New-Web3Connection | Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
$ContractConnection | Get-BlockchainMemberInvitation – SubscriptionId <Azure subscription ID>
```

#### <a name="example-output"></a>Saída de exemplo

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-BlockchainMemberInvitation

Use este cmdlet para revogar o convite de um membro consortium.

`Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| SubscriptionId | ID de assinatura do Azure do membro revogar | Sim |
| Members | Objeto de membros obtido de importação ConsortiumManagementContracts | Sim |
| Web3Account | Objeto Web3Account obtido de importação Web3Account | Sim |
| Web3Client | Objeto Web3Client obtido de New-Web3Connection | Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

Use este cmdlet para definir a **função** para um convite existente. Somente os administradores de consortium podem alterar os convites.

`Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parâmetro | DESCRIÇÃO | Obrigatório |
|-----------|-------------|:--------:|
| SubscriptionId | ID de assinatura do Azure do membro para convidar | Sim |
| Role | Nova função de consortium para o convite. Os valores podem ser **usuário** ou **ADMIN**. | Sim |
| Members |  Objeto de membros obtido de importação ConsortiumManagementContracts | Sim |
| Web3Account | Objeto Web3Account obtido de importação Web3Account | Sim |
| Web3Client | Objeto Web3Client obtido de New-Web3Connection | Sim |

#### <a name="example"></a>Exemplo

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre consórcios, membros e nós, consulte:

> [!div class="nextstepaction"]
> [Azure consortium de Blockchain Service](consortium.md)

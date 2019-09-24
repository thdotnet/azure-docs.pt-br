---
title: Gerenciar pontos de extremidade privados no Azure
description: Saiba como criar um ponto de extremidade privado usando o portal do Azure
services: private-link
author: KumudD
ms.service: private-link
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 2b7ad95dc652f2099af75308f05ddd0684e088aa
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104740"
---
# <a name="create-a-private-endpoint-using-azure-portal"></a>Criar um ponto de extremidade privado usando portal do Azure

Um ponto de extremidade privado é o bloco de construção fundamental para o link privado no Azure. Ele permite que os recursos do Azure, como VMs (máquinas virtuais), se comuniquem de forma privada com recursos de link privado. Neste guia de início rápido, você aprenderá a criar uma VM em uma rede virtual do Azure, um servidor do banco de dados SQL com um ponto de extremidade privado do Azure usando o Azure PowerShell. Em seguida, você pode acessar com segurança o servidor do banco de dados SQL da VM.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure"></a>Entrar no Azure

Entre no Portal do Azure em https://portal.azure.com.

## <a name="create-a-vm"></a>Criar uma máquina virtual
Nesta seção, você criará uma rede virtual e a sub-rede para hospedar a VM usada para acessar seu recurso de Link Privado (um SQL Server no Azure, neste exemplo).

### <a name="create-the-virtual-network"></a>Criar a rede virtual


Nesta seção, você criará uma rede virtual e a sub-rede para hospedar a VM usada para acessar o recurso de Link Privado.

1. No canto superior esquerdo da tela, selecione **Criar um recurso** > **Rede** > **Rede virtual**.
1. Em **Criar rede virtual**, insira ou selecione estas informações:

    | Configuração | Valor |
    | ------- | ----- |
    | NOME | Insira *MyVirtualNetwork*. |
    | Espaço de endereço | Insira *10.1.0.0/16*. |
    | Subscription | Selecione sua assinatura.|
    | Resource group | Selecione **Criar novo** e insira *myResourceGroup*, depois selecione **OK**. |
    | Location | Selecione **WestCentralUS**.|
    | Sub-rede – Nome | Insira *mySubnet*. |
    | Sub-rede – Intervalo de endereços | Insira *10.1.0.0/24*. |
    |||
1. Deixe o restante com os valores padrão e selecione **Criar**.


### <a name="create-virtual-machine"></a>Criar máquina virtual

1. No lado superior esquerdo da tela no portal do Azure, selecione **Criar um recurso** > **Computação** > **Máquina Virtual**.

1. Em **Criar uma máquina virtual – Noções básicas**, insira ou selecione estas informações:

    | Configuração | Valor |
    | ------- | ----- |
    | **DETALHES DO PROJETO** | |
    | Subscription | Selecione sua assinatura. |
    | Resource group | Selecione **myResourceGroup**. Você o criou na seção anterior.  |
    | **DETALHES DA INSTÂNCIA** |  |
    | Nome da máquina virtual | Insira *myVm*. |
    | Região | Selecione **WestCentralUS**. |
    | Opções de disponibilidade | Deixe o padrão **Nenhuma redundância de infraestrutura necessária**. |
    | Imagem | Selecione **Windows Server 2019 Datacenter**. |
    | Tamanho | Deixe o padrão **Standard DS1 v2**. |
    | **CONTA DE ADMINISTRADOR** |  |
    | Nome de Usuário | Insira um nome de usuário de sua escolha. |
    | Senha | Insira uma senha de sua escolha. A senha deve ter no mínimo 12 caracteres e atender a [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Confirmar Senha | Reinsira a senha. |
    | **REGRAS DE PORTA DE ENTRADA** |  |
    | Porta de entrada públicas | Deixar o padrão **Nenhum**. |
    | **ECONOMIZE DINHEIRO** |  |
    | Já tem uma licença do Windows? | Deixe o padrão **Não**. |
    |||

1. Selecione **Avançar: Discos**.

1. Em **Criar uma máquina virtual – Discos**, mantenha os padrões e selecione **Avançar: Rede**.

1. Em **Criar uma máquina virtual – Rede**, selecione estas informações:

    | Configuração | Valor |
    | ------- | ----- |
    | Rede virtual | Deixe o padrão **MyVirtualNetwork**.  |
    | Espaço de endereço | Deixar o padrão **10.1.0.0/24**.|
    | Sub-rede | Deixar o padrão **mySubnet (10.1.0.0/24)** .|
    | IP público | Deixe o padrão **(novo) myVm-ip**. |
    | Porta de entrada públicas | Selecione **Permitir portas selecionadas**. |
    | Selecione as portas de entrada | Selecione **HTTP** e **RDP**.|
    |||


1. Selecione **Examinar + criar**. Você é levado até a página **Examinar + criar**, na qual o Azure valida sua configuração.

1. Quando vir a mensagem **Validação aprovada**, selecione **Criar**.

## <a name="create-a-sql-database-server"></a>Criar um servidor do banco de dados SQL
Nesta seção, você criará um servidor do banco de dados SQL no Azure. 

1. No lado superior esquerdo da tela no portal do Azure, selecione **Criar um recurso** > **Bancos de Dados** > **Banco de Dados SQL**.

1. Em **Criar Banco de Dados SQL – Noções Básicas**, insira ou selecione estas informações:

    | Configuração | Valor |
    | ------- | ----- |
    | **Detalhes do banco de dados** | |
    | Subscription | Selecione sua assinatura. |
    | Resource group | Selecione **myResourceGroup**. Você o criou na seção anterior.|
    | **DETALHES DA INSTÂNCIA** |  |
    | Nome do banco de dados  | Insira *mydatabase*. Se esse nome já estiver sendo usado, crie um nome exclusivo. |
    |||
5. Em  **Servidor**, selecione **Criar novo**. 
6. Em  **Novo servidor**, insira ou selecione estas informações:

    | Configuração | Valor |
    | ------- | ----- |
    |Nome do servidor  | Insira *myserver*. Se esse nome já estiver sendo usado, crie um nome exclusivo.|
    | Logon de administrador do servidor| Insira um nome de administrador de sua escolha. |
    | Senha | Insira uma senha de sua escolha. A senha deve ter no mínimo 8 caracteres e atender a requisitos complexidade definidos. |
    | Location | Selecione uma região do Azure na qual deseja que o SQL Server resida. |
    
7. Selecione  **OK**. 
8. Selecione  **Analisar + criar**. Você é levado até a página  **Analisar + criar** , na qual o Azure valida sua configuração. 
9. Quando vir a mensagem Validação aprovada, selecione  **Criar**. 
10. Quando vir a mensagem Validação aprovada, selecione Criar. 

## <a name="create-a-private-endpoint"></a>Criar um ponto de extremidade privado

Nesta seção, você criará um SQL Server e adicionará um ponto de extremidade privado a ele. 

1. No lado superior esquerdo da tela no portal do Azure, selecione **Criar um recurso** > **Rede** > **Central de Link Privado (Versão Prévia)** .
2. Em **Central de Link Privado – Visão Geral**, na opção **Criar uma conexão privada com um serviço**, selecione **Iniciar**.
1. Em **Criar um ponto de extremidade privado (versão prévia) – noções básicas**, insira ou selecione estas informações:

    | Configuração | Valor |
    | ------- | ----- |
    | **Detalhes do projeto** | |
    | Subscription | Selecione sua assinatura. |
    | Resource group | Selecione **myResourceGroup**. Você o criou na seção anterior.|
    | **DETALHES DA INSTÂNCIA** |  |
    | NOME | Insira * myPrivateEndpoint*. Se esse nome já estiver sendo usado, crie um nome exclusivo. |
    |Região|Selecione **WestCentralUS**.|
    |||
5. Selecione  **Avançar: Recurso**.
6. Em  **Criar um ponto de extremidade privado – recurso**, insira ou selecione estas informações:

    | Configuração | Valor |
    | ------- | ----- |
    |Método de conexão  | Selecione conectar-se a um recurso do Azure em meu diretório.|
    | Subscription| Selecione sua assinatura. |
    | Tipo de recurso | Selecione  **Microsoft.Sql/servers**. |
    | Recurso |Selecione *myServer*|
    |Sub-recurso de destino |Selecione *sqlServer*|
    |||
7. Selecione  **Avançar: configuração**.
8. Em **Criar um ponto de extremidade privado (versão prévia) – configuração**, insira ou selecione estas informações:

    | Configuração | Valor |
    | ------- | ----- |
    |**REDE**| |
    | Rede virtual| Selecione *MyVirtualNetwork*. |
    | Sub-rede | Selecione  *mySubnet*. |
    |**INTEGRAÇÃO DE DNS PRIVADO**||
    |Integrar com a zona DNS privado |Selecione  **Sim**. |
    |Zona DNS privado |Selecione *(New)privatelink.database.windows.net* |
    |||

1. Selecione  **Analisar + criar**. Você é levado até a página  **Analisar + criar** , na qual o Azure valida sua configuração. 
2. Quando você vir a mensagem  **validação aprovada** , selecione  **Criar**. 
 
## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Conectar-se a uma VM usando a RDP (Área de Trabalho Remota)


Depois de criar **myVm*, conecte-se a ele pela Internet da seguinte maneira: 

1. Na barra de pesquisa do portal, insira *myVm*.

1. Selecione o botão **Conectar**. Depois de selecionar o botão **Conectar**, **Conectar-se à máquina virtual** abre.

1. Selecione **Baixar Arquivo RDP**. O Azure cria um arquivo *.rdp* (protocolo RDP) e ele é baixado no computador.

1. Abra o arquivo downloaded.rdp*.

    1. Se solicitado, selecione **Conectar**.

    1. Insira o nome de usuário e a senha que você especificou ao criar a VM.

        > [!NOTE]
        > Talvez seja necessário selecionar **Mais escolhas** > **Usar uma conta diferente** para especificar as credenciais inseridas durante a criação da VM.

1. Selecione **OK**.

1. Você pode receber um aviso do certificado durante o processo de logon. Se você receber um aviso de certificado, selecione **Sim** ou **Continuar**.

1. Depois que a área de trabalho da VM for exibida, minimize-a para voltar para sua área de trabalho local.  

## <a name="access-the-sql-database-server-privately-from-the-vm"></a>Acessar o servidor do banco de dados SQL de forma privada da VM

1. Na Área de Trabalho Remota de  *myVM*, abra o PowerShell.
2. Insira  `nslookup myserver.database.windows.net`. 

    Você receberá uma mensagem semelhante a esta:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
3. Install [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

4. In **Connect to server**, enter or select this information:

    | Setting | Value |
    | ------- | ----- |
    | Server type| Select **Database Engine**.|
    | Server name| Select *myserver.database.windows.net* |
    | User name | Enter a password provided during the SQL server creation. |
    |Password |Enter a password provided during the SQL server creation. |
    |Remember password|Select **Yes**.|
    |||
1. Select **Connect**.
2. Browse databases from left menu.
3. (Optionally) Create or query information from mydatabase.
4. Close the remote desktop connection to *myVm*. 

## Clean up resources 
When you're done using the private endpoint, SQL server, and the VM, delete the resource group and all of the resources it contains: 
1. Enter *myResourceGroup* in the **Search** box at the top of the portal and select *myResourceGroup* from the search results. 
2. Select **Delete resource group**. 
3. Enter myResourceGroup for **TYPE THE RESOURCE GROUP NAME** and select **Delete**.

## Next steps

In this quickstart, you created a VM on a virtual network, a SQL database server, and a private endpoint for private access. You connected to one VM from the internet and securely communicated to the SQL database server using Private Link. To learn more about private endpoints, see [What is Azure private endpoint?](private-endpoint-overview.md).


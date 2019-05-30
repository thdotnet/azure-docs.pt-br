---
title: Início Rápido - Configurar o Azure NetApp Files e criar um volume NFS | Microsoft Docs
description: Início Rápido - Descreve como configurar rapidamente o Azure NetApp Files e criar um volume.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 4/16/2019
ms.author: b-juche
ms.openlocfilehash: 2bcd8163cb3c6071812d4d247b5b333edcfc89e5
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523018"
---
# <a name="quickstart-set-up-azure-netapp-files-and-create-an-nfs-volume"></a>Início Rápido: Configurar o Azure NetApp Files e criar um volume do NFS 

Este artigo mostra como configurar rapidamente o Azure NetApp Files e criar um volume. 

Neste início rápido, você configurará os seguintes itens:

- Registro no Azure NetApp Files e no provedor de recursos do NetApp
- Uma conta do NetApp
- Um pool de capacidade
- Um volume NFS para Azure NetApp Files

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="before-you-begin"></a>Antes de começar 

> [!IMPORTANT] 
> Você precisa ter acesso ao serviço Azure NetApp Files.  Para solicitar acesso ao serviço, consulte a [página de envio de lista de espera do Azure NetApp Files](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u).  Você deve esperar um email de confirmação oficial da equipe do Azure NetApp Files antes de continuar. 

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Registrar-se no Azure NetApp Files e no provedor de recursos do NetApp

1. No portal do Azure, clique no ícone do Azure Cloud Shell no canto superior direito.

    ![ícone do Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell-window.png)

2. Especifique a assinatura que foi incluída na lista de permissões para o Azure NetApp Files:
    
        az account set --subscription <subscriptionId>

3. Registre o provedor de recursos do Azure: 
    
        az provider register --namespace Microsoft.NetApp --wait  

    O processo de registro pode levar algum tempo para ser concluído.

## <a name="create-a-netapp-account"></a>Criar uma conta do NetApp

1. Na caixa de pesquisa do portal do Azure, insira **Azure NetApp Files** e, em seguida, selecione **Azure NetApp Files (versão prévia)** na lista exibida.

      ![Selecionar o Azure NetApp Files](../media/azure-netapp-files/azure-netapp-files-select-azure-netapp-files.png)

2. Clique em **+ Adicionar** para criar uma nova conta do NetApp.

     ![Criar uma conta do NetApp](../media/azure-netapp-files/azure-netapp-files-create-new-netapp-account.png)

3. Na janela Nova Conta do NetApp, forneça as seguintes informações: 
   1. Insira **myaccount1** para o nome da conta. 
   2. Selecione sua assinatura.
   3. Selecione **Criar novo** para criar o grupo de recursos. Insira **myRG1** para o nome do grupo de recursos. Clique em **OK**. 
   4. Selecione a localização da conta.  

      ![Janela Nova Conta do NetApp](../media/azure-netapp-files/azure-netapp-files-new-account-window.png)  

      ![Janela Grupo de recursos](../media/azure-netapp-files/azure-netapp-files-resource-group-window.png)

4. Clique em **Criar** para criar sua conta do NetApp.

## <a name="set-up-a-capacity-pool"></a>Configurar um pool de capacidade

1. Na folha de gerenciamento do Azure NetApp Files, selecione sua conta do NetApp (**myaccount1**).

    ![Selecionar a conta do NetApp](../media/azure-netapp-files/azure-netapp-files-select-netapp-account.png)  

2. Na folha de gerenciamento do Azure NetApp Files de sua conta do NetApp, clique em **Pools de capacidade**.

    ![Clicar em Pools de capacidade](../media/azure-netapp-files/azure-netapp-files-click-capacity-pools.png)  

3. Clique em **+ Adicionar pools**. 

    ![Clicar em Adicionar pools](../media/azure-netapp-files/azure-netapp-files-click-add-pools.png)  

4. Forneça informações para o pool de capacidade: 
    1. Insira **mypool1** como o nome do pool.
    2. Selecione **Premium** para o nível de serviço. 
    3. Especifique **4 (TiB)** como o tamanho do pool. 

5. Clique em **OK**.

## <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Criar um volume NFS para o Azure NetApp Files

1. Na folha de gerenciamento do Azure NetApp Files de sua conta do NetApp, clique em **Volumes**.

    ![Clicar em Volumes](../media/azure-netapp-files/azure-netapp-files-click-volumes.png)  

2. Clique em **+ Adicionar volume**.

    ![Clicar em Adicionar volumes](../media/azure-netapp-files/azure-netapp-files-click-add-volumes.png)  

3. Na janela Criar um Volume, forneça informações para o volume: 
   1. Insira **myvol1** como o nome do volume. 
   3. Selecione o pool de capacidade (**mypool1**).
   4. Use o valor padrão para a cota. 
   5. Na rede virtual, clique em **Criar nova** para criar uma VNET (rede virtual) do Azure.  Em seguida, preencha as seguintes informações:
       * Insira **myvnet1** como o nome da VNET.
       * Especifique um espaço de endereço para a configuração, por exemplo, 10.7.0.0/16
       * Insira **myANFsubnet** como o nome da sub-rede.
       * Especifique o intervalo de endereços da sub-rede, por exemplo, 10.7.0.0/24. Observe que não é possível compartilhar a sub-rede dedicada com outros recursos.
       * Selecione **Microsoft.NetApp/volumes** para a delegação da sub-rede.
       * Clique em **OK** para criar a VNET.
   6. Na sub-rede, selecione a VNET recém-criada (**myvnet1**) como a sub-rede delegada.

      ![Janela Criar um volume](../media/azure-netapp-files/azure-netapp-files-create-volume-window.png)  

      ![Janela Criar rede virtual](../media/azure-netapp-files/azure-netapp-files-create-virtual-network-window.png)  

4. Clique em **Protocolo** e selecione **NFS** como o tipo de protocolo para o volume.   

    Insira **myfilepath1** como o caminho do arquivo que será usado para criar o caminho de exportação para o volume. 

    ![Especifique o protocolo NFS para o início rápido](../media/azure-netapp-files/azure-netapp-files-quickstart-protocol-nfs.png)

5. Clique em **Revisar + Criar**.

    ![Janela Examinar e criar](../media/azure-netapp-files/azure-netapp-files-review-and-create-window.png)  

5. Examine as informações do volume e, em seguida, clique em **Criar**.  
    O volume criado será exibido na folha Volumes.

    ![Volume criado](../media/azure-netapp-files/azure-netapp-files-create-volume-created.png)  

## <a name="clean-up-resources"></a>Limpar recursos

Quando você terminar, e se quiser, poderá excluir o grupo de recursos. A ação de exclusão de um grupo de recursos é irreversível.  

> [!IMPORTANT]
> Todos os recursos dentro dos grupos de recursos serão excluídos permanentemente e não será possível desfazer essa ação. 

1. Na caixa de pesquisa do portal do Azure, insira **Azure NetApp Files** e, em seguida, selecione **Azure NetApp Files** na lista exibida.

2. Na lista de assinaturas, clique no grupo de recursos (myRG1) que você quer excluir. 

    ![Navegar até grupo de recursos](../media/azure-netapp-files/azure-netapp-files-azure-navigate-to-resource-groups.png)


3. Na página de grupo de recursos, clique em **Excluir grupo de recursos**.

    ![Excluir grupo de recursos](../media/azure-netapp-files/azure-netapp-files-azure-delete-resource-group.png) 

    Uma janela é aberta e exibe um aviso sobre os recursos que serão excluídos com o grupo de recursos.

4. Insira o nome do grupo de recursos (myRG1) para confirmar que você deseja excluir permanentemente o grupo de recursos e todos os recursos contidos nele e, em seguida, clique em **Excluir**.

    ![Excluir grupo de recursos](../media/azure-netapp-files/azure-netapp-files-azure-confirm-resource-group-deletion.png ) 

## <a name="next-steps"></a>Próximas etapas  

> [!div class="nextstepaction"]
> [Gerenciar volumes usando o Azure NetApp Files](azure-netapp-files-manage-volumes.md)  

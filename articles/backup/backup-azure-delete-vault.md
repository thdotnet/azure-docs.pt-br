---
title: Excluir um cofre de serviços de recuperação no Azure
description: Descreve como excluir um cofre dos serviços de recuperação.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: raynew
ms.openlocfilehash: 01c20ce84f5c97b3a0ac437fe602861085b5052c
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827901"
---
# <a name="delete-a-recovery-services-vault"></a>Excluir um cofre dos Serviços de Recuperação

Este artigo descreve como excluir uma [Backup do Azure](backup-overview.md) o cofre dos serviços de recuperação. Ele contém instruções para remoção de dependências e, em seguida, excluir um cofre.


## <a name="before-you-start"></a>Antes de começar

Você não pode excluir um cofre de serviços de recuperação que tenha dependências, como servidores protegidos ou servidores de gerenciamento de backup associados ao cofre.<br/>
Cofre que contém dados de backup não pode ser excluído (ou seja, mesmo se você tiver interrompido a proteção, mas mantidos os dados de backup).

Se você excluir um cofre que contém as dependências, em seguida, ele exibirá o seguinte erro:

![Excluir erro de cofre](./media/backup-azure-delete-vault/error.png)

Para excluir normalmente um cofre de seguir as etapas mencionadas na sequência abaixo:
- Interrompa a proteção e excluir dados de backup
- Exclua os servidores protegidos ou servidores de gerenciamento de backup
- Excluir o cofre


## <a name="delete-backup-data-and-backup-items"></a>Excluir dados de backup e itens de backup

Antes de continuar a leitura adicional **[isso ](#before-you-start)** seção para entender as dependências e o processo de exclusão do cofre.

### <a name="for-protected-items-in-cloud"></a>Para itens protegidos na nuvem

Para interromper a proteção e excluir os dados de backup, execute a abaixo:

1. No portal > Serviços de recuperação do cofre > itens de Backup escolher itens protegidos na nuvem.

    ![selecione o tipo de backup](./media/backup-azure-delete-vault/azure-storage-selected.jpg)

2. Para cada item, você precisa para o botão direito do mouse e escolha **parar backup**.

    ![selecione o tipo de backup](./media/backup-azure-delete-vault/stop-backup-item.png)

3. Na **parar Backup** > **escolha uma opção**, selecione **excluir dados de Backup**.
4. Digite o nome do item e, em seguida, clique em **parar backup**.
   - Isso confirma que você deseja excluir o item.
   - O **parar Backup** botão é ativado depois de verificar.
   - Se você mantém e não exclua os dados, você não poderá excluir o cofre.

     ![Excluir dados de backup](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Verifique as **notificação** ![excluir dados de backup](./media/backup-azure-delete-vault/messages.png). Após a conclusão, o serviço exibe a mensagem: **Interrompendo o backup e a exclusão de fazer backup de dados para "*Item de Backup*"** . **A operação concluída com êxito**.
6. Clique em **Refresh** na **itens de Backup** menu, para verificar se o item de Backup é removido.

      ![Excluir dados de backup](./media/backup-azure-delete-vault/empty-items-list.png)

### <a name="for-mars-agent"></a>Para o agente MARS

Para interromper a proteção e excluir dados de backup, execute as etapas na ordem listada abaixo:

- [Etapa 1: Excluir itens de backup do console de gerenciamento de MARS](#step-1-delete-backup-items-from-mars-management-console)
- [Etapa 2: No portal de remover o agente de Backup do Azure](#step-1-delete-backup-items-from-mars-management-console)


#### <a name="step-1-delete-backup-items-from-mars-management-console"></a>Etapa 1: Excluir itens de backup do console de gerenciamento de MARS

Se não for possível executar esta etapa devido à indisponibilidade do servidor, em seguida, entre em contato com o suporte da Microsoft.

- Inicie o console de gerenciamento de MARS, vá para o **ações** painel e escolha **agendar Backup**.
- Partir **modificar ou interromper um Backup agendado** assistente, escolha a opção **interromper o uso desta agenda de backup e excluir todos os backups armazenados** e clique em **próxima**.

    ![Modificar ou Parar um Backup Agendado](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- Partir **interromper um Backup agendado** assistente, clique em **concluir**.

    ![Interromper um Backup agendado](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- Você precisará inserir um Pin de segurança. Para gerar o PIN, execute as etapas a seguir:
  - Entre no Portal do Azure.
  - Navegue até **Cofre dos Serviços de Recuperação** > **Configurações** > **Propriedades**.
  - Em **PIN de Segurança**, clique em **Gerar**. Copie esse PIN. (Esse PIN é válido somente por cinco minutos)
- No Console de gerenciamento (aplicativo de cliente), cole o PIN e clique em **Okey**.

  ![Pin de segurança](./media/backup-azure-delete-vault/security-pin.png)

- No **modificar progresso do Backup** assistente, você verá *dados de backup excluídos serão retidos por 14 dias. Após esse tempo, os dados de backup serão excluídos permanentemente.*  

    ![Excluir a infraestrutura de Backup](./media/backup-azure-delete-vault/deleted-backup-data.png)

Agora que você excluiu os itens de backup no local, conclua as seguintes etapas no portal.

#### <a name="step-2-from-portal-remove-azure-backup-agent"></a>Etapa 2: No portal de remover o agente de Backup do Azure

Certifique-se [etapa 1](#step-1-delete-backup-items-from-mars-management-console) é concluída antes de continuar:

1. No menu do painel do cofre, clique em **infraestrutura de Backup**.
2. Clique em **servidores protegidos** para exibir os servidores de infraestrutura.

    ![selecione seu cofre para abrir o painel dele](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. Na lista **Servidores Protegidos**, clique em Agente de Backup do Azure.

    ![selecione o tipo de backup](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Clique no servidor na lista de servidores protegidos usando o agente de Backup do Azure.

    ![selecione o servidor protegido específico](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. No painel do servidor selecionado, clique em **excluir**.

    ![exclua o servidor selecionado](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Sobre o **exclua** menu, digite o nome do servidor e clique em **excluir**.

     ![Excluir dados de backup](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Se você estiver vendo o erro abaixo, primeiro execute as etapas listadas em [excluindo itens de backup do console de gerenciamento](#step-1-delete-backup-items-from-mars-management-console).
>
>![Falha na exclusão](./media/backup-azure-delete-vault/deletion-failed.png)
>
>Se não for possível executar as etapas para excluir backups no console de gerenciamento, por exemplo, devido à indisponibilidade do servidor com o console de gerenciamento, entre em contato com o suporte da Microsoft.

7. Verifique as **notificação** ![excluir dados de backup](./media/backup-azure-delete-vault/messages.png). Após a conclusão, o serviço exibe a mensagem: **Interrompendo o backup e a exclusão de fazer backup de dados para "*Item de Backup*"** . **A operação concluída com êxito**.
8. Clique em **Refresh** na **itens de Backup** menu, para verificar se o item de Backup é removido.


### <a name="for-mabs-agent"></a>Para o agente do MABS

Para interromper a proteção e excluir dados de backup, execute as etapas na ordem listada abaixo:

- [Etapa 1: Excluir itens de backup do console de gerenciamento do MABS](#step-1-delete-backup-items-from-mabs-management-console)
- [Etapa 2: No portal, remova os servidores de gerenciamento de Backup do Azure](#step-2-from-portal-remove-azure-backup-agent)

#### <a name="step-1-delete-backup-items-from-mabs-management-console"></a>Etapa 1: Excluir itens de backup do console de gerenciamento do MABS

Se não for possível executar esta etapa devido à indisponibilidade do servidor, em seguida, entre em contato com o suporte da Microsoft.

**Método 1** para interromper a proteção e excluir dados de backup, execute as etapas a seguir:

1.  No DPM Administrator Console, clique em **proteção** na barra de navegação.
2.  No painel de exibição, selecione o membro do grupo de proteção que você deseja remover. Botão direito do mouse para escolher **parar proteção de membros do grupo** opção.
3.  Dos **parar proteção** caixa de diálogo, selecione **excluir dados protegidos** > **excluir armazenamento online** caixa de seleção e, em seguida, clique em **parar Proteção**.

    ![Excluir armazenamento online](./media/backup-azure-delete-vault/delete-storage-online.png)

Agora, o status de membro protegido é alterado para **réplica inativa disponível**.

5. O grupo de proteção inativa com o botão direito e selecione **remover proteção inativa**.

    ![Remover proteção inativa](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. Dos **excluir proteção inativa** janela, selecione **excluir armazenamento on-line** e clique em **Okey**.

    ![Remover réplicas em disco e online](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**Método 2** iniciar o **MABS gerenciamento** console. No **Selecionar método de proteção de dados** seção, desmarque **desejo proteção online**.

  ![Selecionar método de proteção de dados](./media/backup-azure-delete-vault/data-protection-method.png)

Agora que você excluiu os itens de backup no local, conclua as seguintes etapas no portal.

#### <a name="step-2-from-portal-remove-azure-backup-management-servers"></a>Etapa 2: No portal, remova os servidores de gerenciamento de Backup do Azure

Certifique-se [etapa 1](#step-1-delete-backup-items-from-mabs-management-console) é concluída antes de continuar:

1. No menu do painel do cofre, clique em **infraestrutura de Backup**.
2. Clique em **servidores de gerenciamento de Backup** para exibir os servidores.

    ![Selecione o cofre para abrir seu painel](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Clique com o botão direito > **excluir**.
4. Sobre o **exclua** menu, digite o nome do servidor e clique em **excluir**.

     ![Excluir dados de backup](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Se você estiver vendo o erro abaixo, primeiro execute as etapas listadas em [excluindo itens de backup do console de gerenciamento](#step-2-from-portal-remove-azure-backup-management-servers).
>
>![Falha na exclusão](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Se não for possível executar as etapas para excluir backups no console de gerenciamento, por exemplo, devido à indisponibilidade do servidor com o console de gerenciamento, entre em contato com o suporte da Microsoft.

5. Verifique as **notificação** ![excluir dados de backup](./media/backup-azure-delete-vault/messages.png). Após a conclusão, o serviço exibe a mensagem: **Interrompendo o backup e a exclusão de fazer backup de dados para "*Item de Backup*"** . **A operação concluída com êxito**.
6. Clique em **Refresh** na **itens de Backup** menu, para verificar se o item de Backup é removido.


## <a name="delete-the-recovery-services-vault"></a>Excluir o Cofre de Serviços de Recuperação

1. Quando todas as dependências foram removidas, role até a **Essentials** painel no menu do cofre.
2. Verifique se que não haja nenhum **itens de Backup**, **fazer Backup de servidores de gerenciamento**, ou **itens replicados** listados. Se itens ainda aparecerem no cofre, [removê-los](#delete-backup-data-and-backup-items).

3. Quando não houver nenhum outro item no cofre, clique em **Excluir** no painel do cofre.

    ![Excluir dados de backup](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Para verificar se você deseja excluir o cofre, clique em **Sim**. O cofre é excluído e o portal retorna para o menu de serviço **Novo** .

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Excluir o Cofre de serviços de recuperação usando o cliente do Azure Resource Manager

Esta opção para excluir o cofre dos serviços de recuperação é recomendada somente quando todas as dependências são removidas e ainda estiver havendo a *erro de exclusão do cofre*.



- Dos **Essentials** painel no menu do cofre, verifique se que não haja nenhum **fazer Backup de itens**, **fazer Backup de servidores de gerenciamento**, ou **deitensreplicados** listados. Se houver itens de backup, em seguida, execute as etapas no [excluir dados de backup e itens de backup](#delete-backup-data-and-backup-items).
- Repita [excluir o cofre do portal](#delete-the-recovery-services-vault).
- Se todas as dependências são removidas e ainda estiver havendo a *erro de exclusão do cofre* , em seguida, use a ferramenta ARMClient para executar as etapas abaixo;

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Instalar chocolatey partir [aqui](https://chocolatey.org/) e instalar o ARMClient para executar o comando abaixo:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Entre sua conta do Azure e execute este comando:

    `ARMClient.exe login [environment name]`

3. No portal do Azure, colete o ID e o recurso grupo nome da assinatura para o cofre que você deseja excluir.

Para obter mais informações sobre o comando ARMClient, consulte [documento](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Usar o cliente do Azure Resource Manager para excluir o cofre dos serviços de recuperação

1. Execute o seguinte comando usando sua ID da assinatura, o nome do grupo de recursos e o nome do cofre. Quando você executar o comando, ele exclui o cofre se você não tiver todas as dependências.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Se o cofre não estiver vazio, você receberá o erro "Cofre não pode ser excluído pois há recursos existentes dentro do cofre". Para remover os itens protegidos / contêiner dentro de um cofre, faça o seguinte:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. No portal do Azure, verifique se que o cofre é excluído.


## <a name="next-steps"></a>Próximas etapas

[Saiba mais sobre](backup-azure-recovery-services-vault-overview.md) cofres dos serviços de recuperação.<br/>
[Saiba mais sobre](backup-azure-manage-windows-server.md) monitorar e gerenciar cofres de serviços de recuperação.

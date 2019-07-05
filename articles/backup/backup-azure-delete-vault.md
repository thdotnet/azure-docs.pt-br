---
title: Excluir um cofre de serviços de recuperação no Azure
description: Descreve como excluir um cofre dos serviços de recuperação.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: raynew
ms.openlocfilehash: e195d9a4b9d2bbe21848e083dbccf864188e0790
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508518"
---
# <a name="delete-a-recovery-services-vault"></a>Excluir um cofre dos Serviços de Recuperação

Este artigo descreve como excluir uma [Backup do Azure](backup-overview.md) o cofre dos serviços de recuperação. Ele contém instruções para remover dependências e, em seguida, excluir um cofre e excluir um cofre por força.


## <a name="before-you-start"></a>Antes de começar

Antes de começar, é importante entender que você não pode excluir um cofre dos serviços de recuperação com servidores registrados nele, ou que contém dados de backup.

- Para excluir normalmente um cofre, cancelar o registro de servidores que ele contém, remover os dados do cofre e, em seguida, exclua o cofre.
- Se você tentar excluir um cofre que ainda tem dependências, uma mensagem de erro é emitida e você precisará remover manualmente as dependências de cofre, incluindo:
    - Itens submetidos a backup
    - Servidores protegidos
    - Fazer backup de servidores de gerenciamento (servidor de Backup do Azure, o DPM) ![selecione seu cofre para abrir seu painel](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)
- Se você não quiser reter dados no cofre de serviços de recuperação e deseja excluir o cofre, você pode excluir o cofre por força.
- Se você tentar excluir um cofre, mas não for possível, o cofre ainda estará configurado para receber dados de backup.


## <a name="delete-a-vault-from-the-azure-portal"></a>Excluir um cofre do portal do Azure

1. Abra o painel do cofre.  
2. No painel, clique em **excluir**. Verifique se que você deseja excluir.

    ![selecione seu cofre para abrir o painel dele](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

Se você receber um erro, remova [itens de backup](#remove-backup-items), [servidores de infraestrutura](#remove-azure-backup-management-servers), e [pontos de recuperação](#remove-azure-backup-agent-recovery-points)e, em seguida, exclua o cofre.

![Excluir erro de cofre](./media/backup-azure-delete-vault/error.png)


## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Excluir o Cofre de serviços de recuperação usando o cliente do Azure Resource Manager

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Instalar chocolatey partir [aqui](https://chocolatey.org/) e instalar o ARMClient para executar o comando abaixo:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Entre sua conta do Azure e execute este comando:

    `ARMClient.exe login [environment name]`

3. No portal do Azure, colete o ID e o recurso grupo nome da assinatura para o cofre que você deseja excluir.

Para obter mais informações sobre o comando ARMClient, consulte [documento](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Usar o cliente do Azure Resource Manager para excluir o cofre dos serviços de recuperação

1. Execute o seguinte comando usando sua ID da assinatura, o nome do grupo de recursos e o nome do cofre. Quando você executa o comando exclui o cofre se você não tiver todas as dependências.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Se o cofre não vazio, você receber o erro "Cofre não pode ser excluído pois há recursos existentes dentro do cofre". Para remover os itens protegidos / contêiner dentro de um cofre, faça o seguinte:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. No portal do Azure, verifique se que o cofre é excluído.


## <a name="remove-vault-items-and-delete-the-vault"></a>Remova itens do cofre e excluir o cofre

Remova todas as dependências antes do Cofre de serviços de recuperação é excluído.

### <a name="remove-backup-items"></a>Remover itens de backup

Esse procedimento fornece um exemplo que mostra como remover dados de backup de arquivos do Azure.

1. Clique em **itens de Backup** > **Azure Storage (arquivos do Azure)**

    ![selecione o tipo de backup](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

2. Cada item de arquivos do Azure para remover e, em seguida, clique com o botão direito **parar backup**.

    ![selecione o tipo de backup](./media/backup-azure-delete-vault/stop-backup-item.png)


3. Na **parar Backup** > **escolha uma opção**, selecione **excluir dados de Backup**.
4. Digite o nome do item e, em seguida, clique em **parar backup**.
   - Isso confirma que você deseja excluir o item.
   - O **parar Backup** botão é ativado depois de verificar.
   - Se você mantém e não exclua os dados, você não poderá excluir o cofre.

     ![Excluir dados de backup](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Opcionalmente, forneça um motivo por que você está excluindo os dados e adicionar comentários.
6. Para verificar se o trabalho de exclusão é concluída, verifique as mensagens do Azure ![Excluir dados de backup](./media/backup-azure-delete-vault/messages.png).
7. Depois que o trabalho for concluído, o serviço envia uma mensagem: **o processo de backup foi interrompido e os dados de backup foram excluídos**.
8. Depois de excluir um item na lista, no menu **Itens de Backup**, clique em **Atualizar** para ver os itens no cofre.

      ![Excluir dados de backup](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="deleting-backup-items-from-management-console"></a>Excluindo itens de backup do console de gerenciamento

Para excluir os itens de backup de infraestrutura de Backup, navegue até o Console de gerenciamento (MARS, servidor de Backup do Azure ou SC DPM dependendo de onde seus itens de back-são protegidas) do servidor de local.

### <a name="for-mars-agent"></a>Para o agente MARS

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

Agora que você excluiu os itens de backup no local, conclua as etapas do portal a seguir:
- Para MARS siga as etapas em [pontos de recuperação de agente de remover o Backup do Azure](#remove-azure-backup-agent-recovery-points)

### <a name="for-mabs-agent"></a>Para o agente do MABS

Há métodos diferentes para parar/excluir proteção online, executar qualquer uma dos métodos abaixo:

**Método 1**

Inicie o **MABS gerenciamento** console. No **Selecionar método de proteção de dados** seção, desmarque **desejo proteção online**.

  ![Selecionar método de proteção de dados](./media/backup-azure-delete-vault/data-protection-method.png)

**Método 2**

Para excluir um grupo de proteção, primeiro você deve interromper a proteção do grupo. Use o procedimento a seguir para interromper a proteção e habilitar a exclusão de um grupo de proteção.

1.  No DPM Administrator Console, clique em **proteção** na barra de navegação.
2.  No painel de exibição, selecione o membro do grupo de proteção que você deseja remover. Botão direito do mouse para escolher **parar proteção de membros do grupo** opção.
3.  Dos **parar proteção** caixa de diálogo, selecione **excluir dados protegidos** > **excluir armazenamento online** caixa de seleção e, em seguida, clique em **parar Proteção**.

    ![Excluir armazenamento online](./media/backup-azure-delete-vault/delete-storage-online.png)

Agora, o status de membro protegido é alterado para **réplica inativa disponível**.

5. O grupo de proteção inativa com o botão direito e selecione **remover proteção inativa**.

    ![Remover proteção inativa](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. Dos **excluir proteção inativa** janela, selecione **excluir armazenamento on-line** e clique em **Okey**.

    ![Remover réplicas em disco e online](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

Agora que você excluiu os itens de backup no local, conclua as etapas do portal a seguir:
- Para o MABS e o DPM siga as etapas em [servidores de gerenciamento de Backup do Azure remova](#remove-azure-backup-management-servers).


### <a name="remove-azure-backup-management-servers"></a>Remova os servidores de gerenciamento de Backup do Azure

Antes de remover o servidor de gerenciamento de backup do Azure, certifique-se de executar as etapas listadas em [excluindo itens de backup do console de gerenciamento](#deleting-backup-items-from-management-console).

1. No menu do painel do cofre, clique em **infraestrutura de Backup**.
2. Clique em **servidores de gerenciamento de Backup** para exibir os servidores.

    ![Selecione o cofre para abrir seu painel](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Clique com o botão direito > **excluir**.
4. Sobre o **exclua** menu, digite o nome do servidor e clique em **excluir**.

     ![Excluir dados de backup](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)
5.  Opcionalmente, forneça um motivo por que você está excluindo os dados e adicionar comentários.

> [!NOTE]
> Se você estiver vendo o erro abaixo, primeiro execute as etapas listadas em [excluindo itens de backup do console de gerenciamento](#deleting-backup-items-from-management-console).
>
>![Falha na exclusão](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Se não for possível executar as etapas para excluir backups no console de gerenciamento, por exemplo, devido à indisponibilidade do servidor com o console de gerenciamento, entre em contato com o suporte da Microsoft.

6. Para verificar se o trabalho de exclusão é concluída, verifique as mensagens do Azure ![Excluir dados de backup](./media/backup-azure-delete-vault/messages.png).
7. Depois que o trabalho for concluído, o serviço envia uma mensagem: **o processo de backup foi interrompido e os dados de backup foram excluídos**.
8. Depois de excluir um item na lista, na **infraestrutura de Backup** menu, clique em **atualizar** para ver os itens no cofre.


### <a name="remove-azure-backup-agent-recovery-points"></a>Remover pontos de recuperação de agente de Backup do Azure

Antes de remover o ponto de recuperação de backup do Azure, certifique-se de executar as etapas listadas em [excluindo itens de backup do console de gerenciamento](#deleting-backup-items-from-management-console).

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

7. Opcionalmente, forneça um motivo por que você está excluindo os dados e adicionar comentários.

> [!NOTE]
> Se você estiver vendo o erro abaixo, primeiro execute as etapas listadas em [excluindo itens de backup do console de gerenciamento](#deleting-backup-items-from-management-console).
>
>![Falha na exclusão](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Se não for possível executar as etapas para excluir backups no console de gerenciamento, por exemplo, devido à indisponibilidade do servidor com o console de gerenciamento, entre em contato com o suporte da Microsoft. 

8. Para verificar se o trabalho de exclusão é concluída, verifique as mensagens do Azure ![Excluir dados de backup](./media/backup-azure-delete-vault/messages.png).
9. Depois de excluir um item na lista, na **infraestrutura de Backup** menu, clique em **atualizar** para ver os itens no cofre.


### <a name="delete-the-vault-after-removing-dependencies"></a>Excluir o cofre após a remoção de dependências

1. Quando todas as dependências foram removidas, role até a **Essentials** painel no menu do cofre.
2. Verifique se que não haja nenhum **itens de Backup**, **fazer Backup de servidores de gerenciamento**, ou **itens replicados** listados. Se itens ainda aparecerem no cofre, removê-los.

3. Quando não houver nenhum outro item no cofre, clique em **Excluir** no painel do cofre.

    ![Excluir dados de backup](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Para verificar se você deseja excluir o cofre, clique em **Sim**. O cofre é excluído e o portal retorna para o menu de serviço **Novo** .

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>E se eu parar o processo de backup, mas mantiver os dados?

Se você parar o processo de backup, mas manter acidentalmente os dados, você deve excluir os dados de backup conforme descrito nas seções anteriores.

## <a name="next-steps"></a>Próximas etapas

[Saiba mais sobre](backup-azure-recovery-services-vault-overview.md) cofres dos serviços de recuperação.

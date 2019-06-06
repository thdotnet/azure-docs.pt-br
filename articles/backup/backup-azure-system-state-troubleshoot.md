---
title: Solucionar problemas de Backup do estado do sistema com o Backup do Azure
description: Solucione problemas de Backup do estado do sistema.
services: backup
author: srinathvasireddy
manager: sivan
keywords: como fazer backup; estado do sistema de backup
ms.service: backup
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: srinathvasireddy
ms.openlocfilehash: 53b9f8fb58a6e70a4bd2cd02adb9ce824466d7de
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481588"
---
# <a name="troubleshoot-system-state-backup"></a>Solucionar problemas de Backup do estado do sistema

Este artigo descreve soluções para problemas que você pode encontrar ao usar o Backup do estado do sistema.

## <a name="pre-requisite"></a>Pré-requisito

Antes, solucionar problemas de Backup do estado do sistema com o Backup do Azure, verifique se você realizar o abaixo de pré-requisitos verificar.  

### <a name="verify-windows-server-backup-is-installed"></a>Verifique se o que backup do Windows Server está instalado

Verifique se o Backup do Windows Server está instalado e habilitado no servidor. Para verificar o status da instalação, execute o comando do PowerShell abaixo:

 ```
 PS C:\> Get-WindowsFeature Windows-Server-Backup
 ```
Se a saída exibe a **estado de instalação** como **disponíveis**, isso significa que o recurso de backup do Windows Server está disponível para a instalação, mas não está instalada no servidor. No entanto se o Backup do Windows Server não estiver instalado, em seguida, use um dos métodos para instalá-lo abaixo.

**Método 1: Instalar o Backup do Windows Server usando o PowerShell**

Para instalar o Backup do Windows Server usando o PowerShell, execute o comando abaixo:

  ```
  PS C:\> Install-WindowsFeature -Name Windows-Server-Backup
  ```

**Método 2: Instalar usando o Gerenciador do servidor de Backup do Windows Server**

Para instalar o Backup do Windows Server usando o Gerenciador do servidor, execute a abaixo:

1. No **Manager Sever** e clique em **adicionar funções e recursos**. O **Assistente para adição de funções e recursos** é exibida.

    ![painel](./media/backup-azure-system-state-troubleshoot/server_management.jpg)

2. Selecione **tipo de instalação** e clique em **próxima**.

    ![Tipo de instalação](./media/backup-azure-system-state-troubleshoot/install_type.jpg)

3. Selecione um servidor no pool de servidores e clique em **próxima**. Na função de servidor, deixe a seleção padrão e clique em **próxima**.
4. Selecione **Backup do Windows Server** na **recursos** guia e clique em **próxima**.

    ![recursos](./media/backup-azure-system-state-troubleshoot/features.png)

5. No **confirmação** , clique em **instalar** para iniciar o processo de instalação.
6. No **resultados** guia, ele exibirá o recurso é instalado com êxito no Windows Server de Backup do Windows Server.

    ![result](./media/backup-azure-system-state-troubleshoot/results.jpg)


### <a name="system-volume-information-permission"></a>Permissão de informações de Volume do sistema

Certifique-se de que o sistema Local tem controle total **System Volume Information** pasta localizada no volume em que o windows está instalado. Normalmente, isso é **C:\System Volume informações**. Backup do Windows Server pode falhar se as permissões acima não estão definidas corretamente

### <a name="dependent-services"></a>Serviços dependentes

Verifique se o abaixo de serviços estão em estado de execução:

**Nome do serviço** | **Tipo de inicialização**
--- | ---
Call(RPC) de procedimento remoto | Automático
System(EventSystem) de eventos do COM+ | Automático
Service(SENS) de notificação de eventos do sistema | Automático
Copy(VSS) de sombra de volume | Manual
Provider(SWPRV) de cópia de sombra de Software da Microsoft | Manual

### <a name="validate-windows-server-backup-status"></a>Validar o status do Backup do Windows Server

Para validar o status do Backup do Windows Server, execute a abaixo:

  * Verifique se que o WSB PowerShell está em execução

    -   Executar `Get-WBJob` de uma com privilégios elevados do PowerShell e verifique se ele não retorna o seguinte erro:

    > [!WARNING]
    > Get-WBJob: O termo 'Get-WBJob' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome ou se um caminho foi incluído, verifique se ele está correto e tente novamente.

    -   Se ele falhar com este erro, em seguida, reinstale o recurso de Backup do Windows Server na máquina do servidor, conforme mencionado nos pré-requisitos etapa 1.

  * Verifique se o backup do WSB está funcionando corretamente, executando o comando do prompt de comando abaixo:

      ` wbadmin start systemstatebackup -backuptarget:X: -quiet `

      > [!NOTE]
      >Substitua o X pela letra da unidade do volume em que você deseja armazenar o estado do sistema de backup de imagem.

    - Verifique periodicamente o status do trabalho executando `Get-WBJob` comando do PowerShell com privilégios elevados        
    - Após a conclusão do trabalho de backup Verifique o status final do trabalho executando `Get-WBJob -Previous 1` comando

Se o trabalho falhar, ele indica um problema WSB que resulta em falha de Backups de estado do sistema de agente de MARS.

## <a name="common-errors"></a>Erros comuns

### <a name="vss-writer-timeout-error"></a>Erro de tempo limite do gravador VSS

| Sintoma | Causa | Resolução
| -- | -- | --
| -Agente de MARS falha com mensagem de erro: "O trabalho WSB falha com erros VSS. Verifique os logs de eventos do VSS para resolver a falha"<br/><br/> -Erro a seguir está presente nos logs de eventos do aplicativo VSS: "Um gravador VSS rejeitou um evento com erro 0x800423f2, tempo limite do gravador expirou entre os eventos de congelar e descongelar."| Gravador VSS não conseguir concluir no tempo devido à falta de recursos de CPU e memória na máquina <br/><br/> Outro software de backup já está usando o gravador VSS, assim não foi possível concluir a operação de instantâneo para esse backup | Aguarde a CPU/memória ser liberados no sistema ou anular os processos usando muita memória/CPU e tente a operação novamente <br/><br/>  Aguarde até que o backup em andamento seja concluída e tente a operação em um momento posterior, quando não há backups estão em execução no computador


### <a name="insufficient-disk-space-to-grow-shadow-copies"></a>Espaço em disco insuficiente para aumentar as cópias de sombra

| Sintoma | Resolução
| -- | --
| -Agente de MARS falha com mensagem de erro: O backup falhou porque o volume de cópia de sombra não pôde ser aumentado devido a espaço em disco suficiente nos volumes que contêm arquivos do sistema <br/><br/> – Após o log de erro/aviso está presente nos logs de eventos do sistema de volsnap: "Havia espaço em disco insuficiente no volume c: para aumentar o armazenamento de cópia de sombra cópias de sombra de c: devido a essa falha de todas as cópias de sombra de volume c: estão em risco de que está sendo excluído" | -Liberar espaço no volume realçado no log de eventos para que haja espaço suficiente para cópias de sombra a crescer enquanto o backup está em andamento <br/><br/> -Ao configurar o espaço de cópia de sombra, podemos pode restringir a quantidade de espaço usado para cópia de sombra, para obter mais informações consulte este [artigo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788050(v=ws.11)#syntax)


### <a name="efi-partition-locked"></a>Partição EFI bloqueada

| Sintoma | Resolução
| -- | --
| Agente de MARS falha com mensagem de erro: "Estado do sistema novamente até falhou porque a partição do sistema EFI está bloqueada. Isso pode ser devido ao acesso de partição do sistema por uma segurança de terceiros ou software de backup" | -Se o problema for devido a um software de segurança de terceiros, em seguida, você precisa entrar em contato com o fornecedor do antivírus para que elas podem permitir que o agente MARS <br/><br/> -Se estiver executando um software de backup de terceiros, aguarde que ele termine e, em seguida, repita back para cima


## <a name="next-steps"></a>Próximas etapas

- Para obter mais informações sobre o estado do sistema Windows na implantação do Resource Manager, consulte [fazer backup de estado do sistema do Windows Server](backup-azure-system-state.md)

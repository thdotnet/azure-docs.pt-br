---
title: Solucionar problemas o Azure Backup agent
description: Solucionar problemas de instalação e registro do agente de Backup do Azure
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: saurse
ms.openlocfilehash: 1c4c2ed6265bdb3c29986fb0b90c3d85d32aadca
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434006"
---
# <a name="troubleshoot-the-microsoft-azure-recovery-services-mars-agent"></a>Solucionar problemas do agente do Microsoft Azure Recovery Services (MARS)

Este artigo descreve como resolver erros, talvez você veja durante a configuração, o registro, o backup, e a restauração.

## <a name="basic-troubleshooting"></a>Solução básica de problemas

É recomendável que você verifique o seguinte antes de iniciar a solução de problemas Microsoft o agente de serviços de recuperação do Azure (MARS):

- [Verifique se o agente de MARS está atualizado](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409).
- [Verifique se você tem conectividade de rede entre o agente MARS e o Azure](https://aka.ms/AB-A4dp50).
- Certifique-se de que MARS está em execução (no console de serviço). Se necessário, reinicie e tente a operação novamente.
- [Certifique-se de 5 a 10% espaço no volume livre está disponível no local de pasta de rascunho](https://aka.ms/AB-AA4dwtt).
- [Verifique se outro processo ou software antivírus está interferindo com o Azure Backup](https://aka.ms/AB-AA4dwtk).
- Se o agendamento de backup falha, mas funciona o backup manual, consulte [Backups não são executados de acordo com a agenda](https://aka.ms/ScheduledBackupFailManualWorks).
- Verifique se que seu sistema operacional tem as atualizações mais recentes.
- [Verifique se não há suporte para unidades e arquivos com atributos sem suporte são excluídos do backup](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup).
- Verifique se que o relógio do sistema protegido é configurado para o fuso horário correto.
- [Verifique se o .NET Framework 4.5.2 ou posterior está instalado no servidor](https://www.microsoft.com/download/details.aspx?id=30653).
- Se você estiver tentando registrar seu servidor em um cofre:
  - Verifique se o agente seja desinstalado no servidor e que ele seja excluído do portal.
  - Use a mesma senha que foi usada inicialmente para registrar o servidor.
- Para backups offline, verifique se que o Azure PowerShell 3.7.0 é instalado na origem e o computador de cópia antes de iniciar o backup.
- Se o agente de Backup está em execução em uma máquina virtual do Azure, consulte [deste artigo](https://aka.ms/AB-AA4dwtr).

## <a name="invalid-vault-credentials-provided"></a>Credenciais do cofre fornecidas inválidas

**Mensagem de erro**: Credenciais do cofre fornecidas inválidas. O arquivo está corrompido ou não possui as credenciais mais recentes associadas ao serviço de recuperação. (ID: 34513)

| Causa | Ações recomendadas |
| ---     | ---    |
| **As credenciais do Cofre não são válidas** <br/> <br/> Arquivos de credencial do cofre podem estar corrompidos ou podem ter expirado. (Por exemplo, eles podem ter sido baixados mais de 48 horas antes da hora de registro.)| Baixe as novas credenciais do Cofre de serviços de recuperação no portal do Azure. (Consulte a etapa 6 na [baixar o agente de MARS](https://docs.microsoft.com/azure/backup/backup-configure-vault#download-the-mars-agent) seção.) Em seguida, siga estas etapas, conforme apropriado: <ul><li> Se você já tiver instalado e registrado MARS, abra o console do MMC do agente de Backup do Microsoft Azure e, em seguida, selecione **registrar servidor** na **ações** painel para concluir o registro com o novo credenciais. <br/> <li> Se a nova instalação falhar, tente reinstalar com as novas credenciais.</ul> **Observação**: Se vários arquivos de credencial de cofre foram baixados, apenas o último arquivo é válido para as próximas 48 horas. É recomendável que você baixe um novo arquivo de credencial de cofre.
| **Servidor proxy/firewall está bloqueando o registro** <br/>ou o <br/>**Não há conectividade com a internet** <br/><br/> Se seu servidor proxy ou máquina possui conectividade de internet limitada e não garantem o acesso para as URLs necessárias, o registro falhará.| Siga estas etapas:<br/> <ul><li> Trabalhar com sua equipe de TI para garantir que o sistema tem conectividade com a internet.<li> Se você não tiver um servidor proxy, verifique se que a opção de proxy não estiver selecionada quando você registrar o agente. [Verifique suas configurações de proxy](#verifying-proxy-settings-for-windows).<li> Se você tiver um servidor de firewall/proxy, trabalhar com sua equipe de rede para garantir que essas URLs e endereços IP têm acesso:<br/> <br> **URLs**<br> www.msftncsi.com <br> .Microsoft.com <br> .WindowsAzure.com <br> .microsoftonline.com <br> .windows.net <br>**Endereços IP**<br>  20.190.128.0/18 <br>  40.126.0.0/18 <br/></ul></ul>Tente registrar novamente depois de concluir as etapas de solução de problemas anteriores.
| **Software antivírus está bloqueando o registro** | Se você tiver um software antivírus instalado no servidor, adicione regras de exclusão necessários para a varredura antivírus para esses arquivos e pastas: <br/><ui> <li> CBengine.exe <li> CSC.exe<li> A pasta de rascunho. Local padrão é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch. <li> A pasta bin em C:\Program Files\Microsoft Azure Recovery Services Agent\Bin.

### <a name="additional-recommendations"></a>Recomendações adicionais
- Vá para C:/Windows/Temp e verifique se há mais de 60.000 ou 65.000 arquivos com a extensão .tmp. Se houver, exclua esses arquivos.
- Certifique-se de data do computador e correspondência de tempo o fuso horário local.
- Certifique-se [esses sites](backup-configure-vault.md#verify-internet-access) são adicionados aos seus sites confiáveis no Internet Explorer.

### <a name="verifying-proxy-settings-for-windows"></a>Verificando configurações de proxy para Windows

1. Baixar o PsExec do [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) página.
1. Executar `psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"` em um prompt de comando elevado.

   Esse comando abrirá o Internet Explorer.
1. Vá para **ferramentas** > **opções da Internet** > **conexões** > **configurações da LAN**.
1. Verifique as configurações de proxy para a conta do sistema.
1. Se nenhum proxy estiver configurado e os detalhes de proxy são fornecidos, remova os detalhes.
1. Se um proxy estiver configurado e os detalhes do proxy estão incorretos, certifique-se a **IP de Proxy** e **porta** detalhes estão corretos.
1. Feche o Internet Explorer.

## <a name="unable-to-download-vault-credential-file"></a>Não é possível baixar o arquivo de credenciais do cofre

| Erro   | Ações recomendadas |
| ---     | ---    |
|Falha ao baixar o arquivo de credenciais do cofre. (ID: 403) | <ul><li> Tente baixar as credenciais do cofre usando um navegador diferente, ou siga estas etapas: <ul><li> Inicie o Internet Explorer. Selecione F12. </li><li> Vá para o **rede** guia e limpar o cache e cookies. </li> <li> Atualize a página.<br></li></ul> <li> Verifique se a assinatura está desabilitada/expirado.<br></li> <li> Verifique se qualquer regra de firewall está bloqueando o download. <br></li> <li> Verifique se que você ainda não tiver esgotado o limite do cofre (50 computadores por cofre).<br></li>  <li> Certifique-se de que o usuário tem as permissões de Backup do Azure que são necessárias para baixar credenciais do cofre e registrar um servidor com o cofre. Ver [controle de acesso baseado em função para gerenciar pontos de recuperação de Backup do Azure](backup-rbac-rs-vault.md).</li></ul> |

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>O Agente de Serviços de Recuperação do Microsoft Azure não pôde se conectar ao Backup do Microsoft Azure

| Erro  | Possível causa | Ações recomendadas |
| ---     | ---     | ---    |
| <br /><ul><li>O agente de serviço de recuperação do Microsoft Azure não pôde se conectar ao Backup do Microsoft Azure. (ID: 100050) Verifique suas configurações de rede e certifique-se de que você seja capaz de se conectar à internet.<li>Autenticação de proxy (407) necessária. |Um proxy está bloqueando a conexão. |  <ul><li>No Internet Explorer, vá para **ferramentas** > **opções da Internet** > **segurança** > **Internet**. Selecione **nível personalizado** e role para baixo até a **download de arquivo** seção. Selecione **Habilitar**.<p>Você também precisa adicionar [URLs e endereços IP](backup-configure-vault.md#verify-internet-access) aos seus sites confiáveis no Internet Explorer.<li>Altere as configurações para usar um servidor proxy. Em seguida, forneça os detalhes do servidor proxy.<li> Se seu computador tem acesso limitado à internet, certifique-se de que as configurações de firewall no computador ou proxy permite que esses [URLs e endereços IP](backup-configure-vault.md#verify-internet-access). <li>Se você tiver um software antivírus instalado no servidor, exclua esses arquivos da verificação antivírus: <ul><li>CBEngine.exe (em vez de dpmra.exe).<li>CSC.exe (relacionado ao .NET Framework). Há um CSC.exe para cada versão do .NET Framework instalado no servidor. Exclua arquivos de CSC.exe para todas as versões do .NET Framework no servidor afetado. <li>O local de pasta ou cache temporário. <br>O local padrão para a pasta de rascunho ou o caminho do cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch.<li>A pasta bin em C:\Program Files\Microsoft Azure Recovery Services Agent\Bin.


## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Falha ao configurar a chave de criptografia para backups seguros

| Erro | Possíveis causas | Ações recomendadas |
| ---     | ---     | ---    |
| <br />Falha ao definir a chave de criptografia para backups seguros. Ativação não foi totalmente bem-sucedida, mas a senha de criptografia foi salvo no seguinte arquivo. |<li>O servidor já está registrado com outro cofre.<li>Durante a configuração, a frase secreta foi corrompida.| Cancelar o registro do servidor do cofre e registre-o novamente com uma nova senha.

## <a name="the-activation-did-not-complete-successfully"></a>A ativação não foi concluída com êxito

| Erro  | Possíveis causas | Ações recomendadas |
|---------|---------|---------|
|<br />A ativação não foi concluída com êxito. A operação atual falhou devido a um erro de serviço interno [0x1FC07]. Aguarde um pouco e repita a operação. Se o problema persistir, contate o suporte da Microsoft.     | <li> A pasta de rascunho está localizada em um volume que não tem espaço suficiente. <li> A pasta de rascunho foi movida incorretamente. <li> O arquivo OnlineBackup.KEK está ausente.         | <li>Atualizar para o [mais recente versão](https://aka.ms/azurebackup_agent) do agente do MARS.<li>Mova o local de pasta ou cache temporário para um volume com espaço livre entre 5% e 10% do tamanho total dos dados de backup. Para mover corretamente o local do cache, consulte as etapas em [perguntas comuns sobre como fazer backup de arquivos e pastas](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Certifique-se de que o arquivo OnlineBackup.KEK está presente. <br>*O local padrão para a pasta de rascunho ou o caminho do cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.        |

## <a name="encryption-passphrase-not-correctly-configured"></a>A frase secreta de criptografia não está configurada corretamente

| Erro  | Possíveis causas | Ações recomendadas |
|---------|---------|---------|
| <br />Error 34506. A senha de criptografia armazenada neste computador não está configurada corretamente.    | <li> A pasta de rascunho está localizada em um volume que não tem espaço suficiente. <li> A pasta de rascunho foi movida incorretamente. <li> O arquivo OnlineBackup.KEK está ausente.        | <li>Atualize para a [versão mais recente](https://aka.ms/azurebackup_agent) do MARS Agent.<li>Mova o local de pasta ou cache temporário para um volume com espaço livre entre 5% e 10% do tamanho total dos dados de backup. Para mover corretamente o local do cache, consulte as etapas em [perguntas comuns sobre como fazer backup de arquivos e pastas](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Certifique-se de que o arquivo OnlineBackup.KEK está presente. <br>*O local padrão para a pasta de rascunho ou o caminho do cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.         |


## <a name="backups-dont-run-according-to-schedule"></a>Backups não são executados de acordo com a agenda
Se os backups agendados não serão disparados automaticamente, mas backups manuais funcionem corretamente, tente as seguintes ações:

- Certifique-se de que a agenda de backup do Windows Server não entra em conflito com o agendamento de backup do Azure arquivos e pastas.

- Verifique se o status do backup online é definido como **habilitar**. Para verificar o status, siga estas etapas:

  1. No Agendador de tarefas, expanda **Microsoft** e selecione **Backup Online**.
  1. Clique duas vezes em **Microsoft OnlineBackup** e vá para o **disparadores** guia.
  1. Verifique se o status é definido como **Enabled**. Se não estiver, selecione **edite**, selecione **Enabled**e, em seguida, selecione **Okey**.

- Certifique-se a conta de usuário selecionada para executar a tarefa seja **SYSTEM** ou **grupo de administradores locais** no servidor. Para verificar a conta de usuário, vá para o **gerais** guia e verifique o **segurança** opções.

- Verifique se o que PowerShell 3.0 ou posterior está instalado no servidor. Para verificar a versão do PowerShell, execute este comando e verificar se o `Major` é de número de versão 3 ou posterior:

  `$PSVersionTable.PSVersion`

- Verifique se esse caminho é parte do `PSMODULEPATH` variável de ambiente:

  `<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`

- Se a política de execução do PowerShell para `LocalMachine` é definida como restrita, o cmdlet do PowerShell que dispara a tarefa de backup pode falhar. Execute estes comandos no modo elevado para verificar e definir a política de execução para um `Unrestricted` ou `RemoteSigned`:

  `PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

  `PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`

- Certifique-se de que nenhum arquivo de MSOnlineBackup de módulo de PowerShell ausente ou corrompido. Se houver arquivos ausentes ou corrompidos, siga estas etapas:

  1. Em qualquer computador que tenha um agente de MARS está funcionando corretamente, copie a pasta MSOnlineBackup C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules.
  1. No computador problemático, cole os arquivos copiados no mesmo local da pasta (C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules).

     Se já houver uma pasta MSOnlineBackup na máquina, cole os arquivos nele ou substituir arquivos existentes.


> [!TIP]
> Para garantir que as alterações são aplicadas de forma consistente, reinicie o servidor depois de executar as etapas anteriores.


## <a name="troubleshoot-restore-problems"></a>Solucionar problemas de restauração

O Backup do Azure pode não montar com êxito o volume de recuperação, mesmo depois de vários minutos. E você poderá receber mensagens de erro durante o processo. Para iniciar a recuperação normalmente, siga estas etapas:

1.  Cancele o processo de montagem, se ele estiver sendo executado por vários minutos.

2.  Verifique se você tem a versão mais recente do agente de Backup. Para verificar a versão na **ações** painel do console do MARS, selecione **sobre o Microsoft Azure Recovery Services Agent**. Confirme se o número de **versão** é igual ou maior do que a versão mencionada [neste artigo](https://go.microsoft.com/fwlink/?linkid=229525). Selecione este link para [baixar a versão mais recente](https://go.microsoft.com/fwLink/?LinkID=288905).

3.  Vá para **Gerenciador de dispositivos** > **controladores de armazenamento** e localize **iniciador iSCSI da Microsoft**. Se você localizá-lo, vá diretamente para a etapa 7.

4.  Se você não conseguir localizar o serviço iniciador Microsoft iSCSI, tentar encontrar uma entrada em **Gerenciador de dispositivos** > **controladores de armazenamento** chamado **dispositivo desconhecido** com a ID de Hardware **ROOT\ISCSIPRT**.

5.  Clique com botão direito **dispositivo desconhecido** e selecione **Atualizar Software de Driver**.

6.  Atualize o driver ao selecionar a opção para **Pesquisar automaticamente software de driver atualizado**. Essa atualização deve alterar **dispositivo desconhecido** à **iniciador iSCSI da Microsoft**:

    ![Captura de tela do Gerenciador de Dispositivos do Backup do Azure, com controladores de Armazenamento realçados](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Vá para **Gerenciador de tarefas** > **serviços (Local)**  > **serviço iniciador Microsoft iSCSI**:

    ![Captura de tela do Gerenciador de Tarefas do Backup do Azure, com Serviços (Local) realçado](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)

8.  Reinicie o serviço Iniciador do Microsoft iSCSI. Para fazer isso, o serviço com o botão direito e selecione **parar**. Em seguida, clique com botão direito-lo novamente e selecione **iniciar**.

9.  Repita a recuperação usando a [Restauração Instantânea](backup-instant-restore-capability.md).

Se a recuperação ainda falhar, reinicie seu servidor ou cliente. Se você não quiser reiniciar, ou se a recuperação ainda falhar mesmo após a reinicialização do servidor, tente [recuperando de outra máquina](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contate o suporte
Caso ainda precise de ajuda, [contate o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver seu problema rapidamente.

## <a name="next-steps"></a>Próximas etapas
* Obter mais detalhes sobre [como fazer backup do Windows Server com o Azure Backup agent](tutorial-backup-windows-server-to-azure.md).
* Se você precisar restaurar um backup, consulte [restaurar arquivos para uma máquina Windows](backup-azure-restore-windows-server.md).

---
title: Solucionar problemas do Agente de Backup do Azure
description: Solucionar problemas de instalação e registro do Azure Backup Agent
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: saurse
ms.openlocfilehash: 2c2ed46ed6e4a5d6663387777d3425d18b50500e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67060209"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent"></a>Solucionar problemas do agente do MARS (Serviços de Recuperação do Microsoft Azure)

Veja como resolver possíveis erros durante a configuração, registro, backup e restauração.

## <a name="basic-troubleshooting"></a>Solução básica de problemas

Recomendamos que você execute o abaixo de validação, antes de começar a solução de problemas de agente do Microsoft Azure Recovery Services (MARS):

- [Verifique se o que agente do Microsoft Azure Recovery Services (MARS) é atualizado](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [Certifique-se de que há conectividade de rede entre o agente MARS e o Azure](https://aka.ms/AB-A4dp50)
- Certifique-se de que os Serviços de Recuperação do Microsoft Azure estão em execução (no console de Serviço). Reinicie e tente novamente a operação, se necessário
- [Certifique-se de que há de 5 a 10% de espaço de volume livre disponível no local da pasta temporária](https://aka.ms/AB-AA4dwtt)
- [Verifique se outro processo ou software antivírus está interferindo com o Backup do Azure](https://aka.ms/AB-AA4dwtk)
- [Falha no backup agendado, mas o backup manual funciona](https://aka.ms/ScheduledBackupFailManualWorks)
- Certifique-se de que o SO possui as atualizações mais recentes
- [Verifique se não há suporte para unidades e arquivos com atributos sem suporte são excluídos do backup](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)
- Certifique-se de que o **Relógio do sistema** no sistema protegido está configurado com o fuso horário correto <br>
- [Certifique-se de que o servidor tenha pelo menos o .Net Framework versão 4.5.2 e superior](https://www.microsoft.com/download/details.aspx?id=30653)<br>
- Se você estiver tentando **registrar novamente seu servidor** em um cofre, então: <br>
  - Certifique-se de que o agente foi desinstalado do servidor e de que ele foi excluído do portal <br>
  - Use a mesma senha que foi inicialmente usada para registrar o servidor <br>
- No caso de backup offline, verifique se que o Azure PowerShell versão 3.7.0 está instalado no computador de origem e de cópia antes de começar a operação de backup offline
- [Consideração quando o agente de Backup está em execução em uma máquina virtual do Azure](https://aka.ms/AB-AA4dwtr)

## <a name="invalid-vault-credentials-provided"></a>Credenciais do cofre fornecidas inválidas

**Mensagem de erro**: Credenciais do cofre fornecidas inválidas. O arquivo está corrompido ou não possui as credenciais mais recentes associadas ao serviço de recuperação. (ID: 34513)

| Causa | Ação recomendada |
| ---     | ---    |
| **As credenciais do cofre são inválidas** <br/> <br/> Arquivos de credencial do cofre podem estar corrompidos ou podem ter expirado (ou seja, baixado mais de 48 horas antes da hora de registro)| Baixe uma nova credencial do cofre dos serviços de recuperação no portal do Azure (consulte *etapa 6* sob [ **baixar o agente de MARS** ](https://docs.microsoft.com/azure/backup/backup-configure-vault#download-the-mars-agent) seção) e executar a abaixo: <ul><li> Se você já tiver instalado e registrado o agente de Backup do Microsoft Azure, em seguida, abra o console do MMC do agente de Backup do Microsoft Azure e escolha **registrar servidor** no painel de ação para concluir o registro com o baixado recentemente credenciais <br/> <li> Se a nova instalação do falha, em seguida, tente instalar novamente usando as novas credenciais</ul> **Observação**: Se vários arquivos de credencial de cofre são baixados anteriormente, somente o último arquivo baixado é válido dentro de 48 horas. Portanto, é recomendável baixar a nova novo arquivo de credenciais do cofre.
| **Servidor proxy/firewall está bloqueando <br/>ou <br/>conectividade com a Internet não** <br/><br/> Se seu computador ou servidor Proxy tem acesso limitado à Internet, em seguida, listando as URLs necessárias o registro falhará.| Para resolver esse problema, execute a abaixo:<br/> <ul><li> Trabalhar com sua equipe de TI para garantir que o sistema tem conectividade com a Internet<li> Se você não tem o servidor Proxy, verifique se a opção de proxy é desmarcada ao registrar o agente, verifique se as etapas de configurações de proxy listadas [aqui](#verifying-proxy-settings-for-windows)<li> Se você tiver um servidor de firewall/proxy, em seguida, trabalhe com a equipe de rede para garantir que abaixo IP e URLs do endereço têm acesso<br/> <br> **URLs**<br> - *www.msftncsi.com* <br>-  *.Microsoft.com* <br> -  *.WindowsAzure.com* <br>-  *.microsoftonline.com* <br>-  *.windows.net* <br>**Endereço IP**<br> - *20.190.128.0/18* <br> - *40.126.0.0/18* <br/></ul></ul>Tente registrar novamente depois de concluir as etapas de solução de problemas acima
| **Um software antivírus está bloqueando** | Se você tiver um software antivírus instalado no servidor, em seguida, adicione regras de exclusão necessários para os seguintes arquivos da verificação antivírus: <br/><ui> <li> *CBengine.exe* <li> *CSC.exe*<li> Pasta de rascunho, o local padrão é *C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch* <li> A pasta bin em *C:\Program Files\Microsoft Azure Recovery Services Agent\Bin*

### <a name="additional-recommendations"></a>Recomendações adicionais
- Vá para *c: / Windows/Temp* e verifique se há mais de 60.000 ou 65.000 arquivos com a extensão. tmp. Se houver, exclua esses arquivos
- Certifique-se de que corresponda a data e a hora do computador com o fuso horário local
- Verifique se o [seguir](backup-configure-vault.md#verify-internet-access) sites serão adicionados ao IE sites confiáveis

### <a name="verifying-proxy-settings-for-windows"></a>Verificando configurações de proxy para Windows

- Baixe **psexec** de [aqui](https://docs.microsoft.com/sysinternals/downloads/psexec)
- Execute este `psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"` comando do prompt com privilégios elevados:
- Isso inicializará *Internet Explorer* janela
- Vá para *ferramentas* -> *opções da Internet* -> *conexões* -> *configurações da LAN*
- Verifique as configurações de proxy para *sistema* conta
- Se nenhum proxy estiver configurado e os detalhes de proxy são fornecidos, em seguida, remova os detalhes
-   Se o proxy estiver configurado e detalhes do proxy estão incorretas, em seguida, certifique-se *IP de Proxy* e *porta* detalhes são precisos
- Fechar *do Internet Explorer*

## <a name="unable-to-download-vault-credential-file"></a>Não é possível baixar o arquivo de credenciais do cofre

| Detalhes do erro | Ações recomendadas |
| ---     | ---    |
|Falha ao baixar o arquivo de credenciais do cofre. (ID: 403) | <ul><li> Tente baixar as credenciais do cofre usando um navegador diferente ou execute as etapas a seguir: <ul><li> Inicie o Internet Explorer, pressione F12. </li><li> Vá até a guia **Rede** para limpar o cache e os cookies do IE </li> <li> Atualize a página<br>(OU)</li></ul> <li> Verifique se a assinatura está desabilitada/expirada<br>(OU)</li> <li> Verifique se alguma regra de firewall está bloqueando o download do arquivo de credenciais do cofre <br>(OU)</li> <li> Verifique se você não esgotou o limite do cofre (50 computadores por cofre)<br>(OU)</li>  <li> Verifique se o usuário obteve permissão de Backup do Azure para baixar credencial do cofre e registrar o servidor com o cofre, consulte [artigo](backup-rbac-rs-vault.md)</li></ul> |

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>O Agente de Serviços de Recuperação do Microsoft Azure não pôde se conectar ao Backup do Microsoft Azure

| Detalhes do erro | Possíveis causas | Ações recomendadas |
| ---     | ---     | ---    |
| **Erro** <br /><ol><li>*O Agente de Serviços de Recuperação do Microsoft Azure não pôde se conectar ao Backup do Microsoft Azure. (ID: 100050) Verifique as configurações de rede e veja se é possível conectar a Internet*<li>*(407) Autenticação de Proxy Obrigatória* |Proxy bloqueando a conexão. |  <ul><li>Inicie **IE** > **Configuração** > **Opções da Internet** > **Segurança** > **Internet**. Em seguida, selecione **Nível personalizado** e role até encontrar a seção de download de arquivo. Selecione **Habilitar**.<li>Você também precisará adicionar esses sites no IE [sites confiáveis](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins).<li>Altere as configurações para usar um servidor proxy. Em seguida, forneça os detalhes do servidor proxy.<li> Se seu computador tem acesso limitado à internet, certifique-se de que as configurações de firewall no computador ou proxy permite que esses [URLs](backup-configure-vault.md#verify-internet-access) e [endereço IP](backup-configure-vault.md#verify-internet-access). <li>Se você tiver um antivírus instalado no servidor, exclua os seguintes arquivos da verificação de antivírus. <ul><li>CBEngine.exe (em vez de dpmra.exe).<li>CSC.exe (relacionado ao .NET Framework). Há um CSC.exe para cada versão .NET instalada no servidor. Exclua os arquivos CSC.exe vinculados a todas as versões do .NET Framework no servidor afetado. <li>Pasta de Rascunho ou local do cache. <br>*O local padrão para a pasta de rascunho ou o caminho do local do cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.<li>A pasta bin C: \ Arquivos de Programas \ Microsoft Agente dos Serviços de Recuperação do Azure \ Bin



## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Falha ao configurar a chave de criptografia para backups seguros

| Detalhes do erro | Possíveis causas | Ações recomendadas |
| ---     | ---     | ---    |
| **Erro** <br />*Falha ao configurar a chave de criptografia para backups seguros. A ativação não foi bem-sucedida, mas a frase secreta de criptografia foi salva no seguinte arquivo* . |<li>O servidor já está registrado com outro cofre.<li>Durante a configuração, a frase secreta foi corrompida.| Cancele o registro do servidor do cofre e registre-se novamente com uma nova frase secreta.

## <a name="the-activation-did-not-complete-successfully"></a>A ativação não foi concluída com êxito

| Detalhes do erro | Possíveis causas | Ações recomendadas |
|---------|---------|---------|
|**Erro** <br />*A ativação não foi concluída com êxito. A operação atual falhou devido a um erro de serviço interno [0x1FC07]. Aguarde um pouco e repita a operação. Se o problema persistir, contate o suporte da Microsoft*     | <li> A pasta de Rascunho está localizada em um volume que não possui espaço suficiente. <li> A pasta de Rascunho é movida incorretamente para outro local. <li> O arquivo OnlineBackup.KEK está ausente.         | <li>Atualize para a [versão mais recente](https://aka.ms/azurebackup_agent) do MARS Agent.<li>Mova a pasta temporária ou o local do cache para um volume com espaço livre igual a 5-10% do tamanho total dos dados de backup. Para mover corretamente o local do cache, consulte as etapas em [Perguntas sobre o Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Certifique-se de que o arquivo OnlineBackup.KEK está presente. <br>*O local padrão para a pasta de rascunho ou o caminho do local do cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.        |

## <a name="encryption-passphrase-not-correctly-configured"></a>A frase secreta de criptografia não está configurada corretamente

| Detalhes do erro | Possíveis causas | Ações recomendadas |
|---------|---------|---------|
|**Erro** <br />*Erro 34506. A senha de criptografia armazenada neste computador não está configurada corretamente*.    | <li> A pasta de Rascunho está localizada em um volume que não possui espaço suficiente. <li> A pasta de Rascunho é movida incorretamente para outro local. <li> O arquivo OnlineBackup.KEK está ausente.        | <li>Atualize para a [versão mais recente](https://aka.ms/azurebackup_agent) do MARS Agent.<li>Mova a pasta de rascunho ou o local do cache para um volume com espaço livre equivalente a 5-10% do tamanho total dos dados de backup. Para mover corretamente o local do cache, consulte as etapas em [Perguntas sobre o Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Certifique-se de que o arquivo OnlineBackup.KEK está presente. <br>*O local padrão para a pasta de rascunho ou o caminho do local do cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.         |


## <a name="backups-dont-run-according-to-the-schedule"></a>Os backups não são executados de acordo com o agendamento
Se os backups agendados não forem acionados automaticamente, mas os backups manuais funcionarem sem problemas, tente estas ações:

- Certifique-se de agendamento de backup do Windows Server não está em conflito com o Azure agenda de backup de arquivos e pastas.

- Verifique se o status de Backup Online é definido como **habilitar**. Para verificar o status de executar a abaixo:

  - Abra **Agendador de tarefas** e expanda **Microsoft**e selecione **Backup Online**.
  - Clique duas vezes em **Microsoft-OnlineBackup** e acesse a guia **Gatilhos**.
  - Verificar se o status está definido como **Enabled**. Se não estiver, selecione **edite** > **Enabled** caixa de seleção e clique em **Okey**.

- Certifique-se a conta de usuário selecionada para executar a tarefa seja **SYSTEM** ou **grupo de administradores locais** no servidor. Para verificar a conta de usuário, vá para o **gerais** guia e verifique o **segurança** opções.

- Verifique se o que PowerShell 3.0 ou posterior está instalado no servidor. Para verificar a versão do PowerShell, execute o seguinte comando e verifique se o número da versão *Principal* é igual ou maior do que 3.

  `$PSVersionTable.PSVersion`

- Verifique se o caminho a seguir faz parte do *PSMODULEPATH* variável de ambiente

  `<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`

- Se a política de execução do powershell para *LocalMachine* estiver definida como restrita, o cmdlet do PowerShell que aciona a tarefa de backup poderá falhar. Execute os seguintes comandos no modo elevado, verificar e definir a política de execução para um *Unrestricted* ou *RemoteSigned*

  `PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

  `PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`

- Verifique se há ausentes ou corrompidos **PowerShell** módulo **MSonlineBackup**. Caso haja qualquer arquivo ausente ou corrompido, para resolver esse problema, execute a abaixo:

  - De qualquer computador com o agente de MARS está funcionando corretamente, copie a pasta de MSOnlineBackup da *(C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules)* caminho.
  - Colar isto no computador problemático no mesmo caminho *(C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules)* .
  - Se **MSOnlineBackup** pasta já existia na máquina, colar ou substituir os arquivos de conteúdo dentro dele.


> [!TIP]
> Para garantir a aplicação consistente das alterações, reinicie o servidor após executar as etapas acima.


## <a name="troubleshoot-restore-issues"></a>Solucionar problemas de restauração

O Backup do Azure pode não montar com êxito o volume de recuperação, mesmo depois de vários minutos. Talvez você também receba mensagens de erro durante o processo. Para iniciar a recuperação normalmente, execute estas etapas:

1.  Cancele o processo de montagem em andamento, caso ele esteja em execução há vários minutos.

2.  Verifique se você está usando a versão mais recente do Agente de Backup do Azure. Para descobrir a versão, no painel **Ações** do console do MARS, selecione **Sobre o Agente dos Serviços de Recuperação do Microsoft Azure**. Confirme se o número de **versão** é igual ou maior do que a versão mencionada [neste artigo](https://go.microsoft.com/fwlink/?linkid=229525). Baixe a versão mais recente [aqui](https://go.microsoft.com/fwLink/?LinkID=288905).

3.  Acesse **Gerenciador de Dispositivos** > **Controladores de Armazenamento** e localize o **Iniciador do Microsoft iSCSI**. Se conseguir localizá-lo, vá diretamente para a etapa 7.

4.  Se você não conseguir localizar o serviço Iniciador do Microsoft iSCSI, tente encontrar uma entrada em **Gerenciador de Dispositivos** > **Controladores de Armazenamento** chamada **Dispositivo Desconhecido** com a ID de Hardware **ROOT\ISCSIPRT**.

5.  Clique com o botão direito em **Dispositivo Desconhecido** e selecione **Atualizar Software de Driver**.

6.  Atualize o driver ao selecionar a opção para **Pesquisar automaticamente software de driver atualizado**. A conclusão da atualização deve alterar o **Dispositivo Desconhecido** para o **Iniciador do Microsoft iSCSI** conforme mostrado abaixo.

    ![Captura de tela do Gerenciador de Dispositivos do Backup do Azure, com controladores de Armazenamento realçados](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Vá para **Gerenciador de Tarefas** > **Serviços (Local)**  > **Serviço Iniciador do Microsoft iSCSI**.

    ![Captura de tela do Gerenciador de Tarefas do Backup do Azure, com Serviços (Local) realçado](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)

8.  Reinicie o serviço Iniciador do Microsoft iSCSI. Para fazer isso, clique com botão direito no serviço, selecione **Parar**, clique novamente com botão direito e selecione **Iniciar**.

9.  Repita a recuperação usando a [**Restauração Instantânea**](backup-instant-restore-capability.md).

Se a recuperação ainda falhar, reinicie o servidor ou o cliente. Se você não quiser reinicializar, ou a recuperação ainda falhar mesmo após a reinicialização do servidor, tente recuperar de uma máquina alternativa. Execute as etapas [deste artigo](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contate o suporte
Se ainda tiver dúvidas, [entre em contato com o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver seu problema rapidamente.

## <a name="next-steps"></a>Próximas etapas
* Obtenha mais detalhes sobre [como fazer backup do Windows Server com o Azure Backup Agent](tutorial-backup-windows-server-to-azure.md).
* Se você precisar restaurar um backup, use este artigo para [restaurar os arquivos para um computador que usa o Windows](backup-azure-restore-windows-server.md).

---
title: Habilitar área de trabalho remota para Linux no Azure Lab Services | Microsoft Docs
description: Saiba como habilitar a área de trabalho remota para máquinas virtuais do Linux em um laboratório no Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2019
ms.author: spelluru
ms.openlocfilehash: 389d467bd9672743d4a086e8a1c505fb0366dba7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237096"
---
# <a name="enable-and-use-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Habilitar e usar a área de trabalho remota para máquinas virtuais do Linux em um laboratório no Azure Lab Services
Este artigo mostra como realizar as seguintes tarefas:

- Habilitar área de trabalho remota para VM do Linux
- Como professor pode se conectar ao modelo de VM por meio de Conexão de área de trabalho remota (RDP).
- Como os alunos se conectam a VM via RDP aluno

## <a name="enable-remote-desktop-for-linux-vm"></a>Habilitar área de trabalho remota para VM do Linux
Durante a criação do laboratório, podem habilitar a professores **conexão de área de trabalho remota** para **Linux** imagens. O **habilitar a Conexão de área de trabalho remota** opção seja mostrada quando uma imagem do Linux é selecionada para o modelo. Quando essa opção é habilitada, professores podem se conectar ao modelo de VM e aluno VMs via RDP (área de trabalho remota). 

![Habilitar a conexão de área de trabalho remota para uma imagem do Linux](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

Sobre o **habilitando a Conexão de área de trabalho remota** caixa de mensagem, selecione **continuar com a área de trabalho remota**. 

![Habilitar a conexão de área de trabalho remota para uma imagem do Linux](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> Habilitando **conexão de área de trabalho remota** abre somente os **RDP** porta em máquinas Linux. Você, como um professor, conecte-se à máquina Linux usando SSH pela primeira vez e instala pacotes RDP e a interface gráfica do usuário para que você pode se conectar à máquina Linux usando o RDP mais tarde. Em seguida, você **publicar** a imagem para que os alunos possam RDP no aluno VMs do Linux. 

## <a name="supported-operating-systems"></a>Sistemas operacionais com suporte
Atualmente, a conexão de área de trabalho remota tem suporte para os seguintes sistemas operacionais:

- openSUSE Leap 42.3
- 7.5 baseado em centOS
- Debian 9 "Stretch"
- Ubuntu Server 16.04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>Professores conectar-se ao modelo de VM usando o RDP
Professores devem se conectar ao modelo de VM usando o SSH e instalar pacotes RDP e GUI nele. Em seguida, os professores podem usar as etapas a seguir para se conectar às VMs do Linux usando o RDP: 

Você vê o **área de trabalho remota** opção para se conectar ao modelo de VM no momento da criação do laboratório. 

![Conectar-se ao modelo via RDP no momento da criação](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

Você vê o **área de trabalho remota** opção na página de home do laboratório depois que o laboratório é criado e o modelo de VM é iniciada. Se ele não for iniciado, inicie a VM do modelo. 

![Conectar-se ao modelo via RDP após o laboratório é criado](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

Para obter mais informações sobre como se conectar à VM usando SSH ou RDP, consulte [conectar-se usando o SSH ou RDP]((#connect-using-ssh-or-rdp). 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Professores se conectar a um aluno VM usando o RDP
Professor/professor pode se conectar a um VM do aluno, alternando para o **máquinas virtuais** exibir e selecionar a **conectar** ícone. Antes disso, devem professores **publicar** a imagem de modelo com os pacotes RDP e GUI instalado nele. 

![Conectar-se para o VM de aluno de professores](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

Para obter mais informações sobre como se conectar à VM usando SSH ou RDP, consulte [conectar-se usando o SSH ou RDP]((#connect-using-ssh-or-rdp). 

## <a name="students-connecting-to-the-student-vm"></a>Conectar-se para o VM de aluno de alunos
Aluno pode RDP para suas VMs do Linux após o proprietário de laboratório (professor/professor) **publica** o modelo de VM com RDP e GUI os pacotes instalados no computador. Siga estas etapas: 

1. Quando um aluno entra portal Labs do diretamente (`https://labs.azure.com`) ou usando um link de registro (`https://labs.azure.com/register/<registrationCode>`), um bloco para cada aluno de laboratório tem acesso ao é exibido. 
2. No bloco, selecione **iniciar** se a VM é interrompida. 
3. Selecione **Conectar**. Você verá duas opções para conectar-se à VM: **SSH** e **área de trabalho remota**.

    ![VM – opções de conexão do aluno](../media/how-to-enable-remote-desktop-linux/student-vm-connect-options.png)

## <a name="connect-using-ssh-or-rdp"></a>Conecte-se usando SSH ou RDP
Se você selecionar o **SSH** opção, consulte o seguinte **conectar-se a sua máquina virtual** caixa de diálogo:  

![Cadeia de caracteres de conexão SSH](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Selecione o **cópia** botão ao lado da caixa de texto para copiá-lo na área de transferência. Salve a cadeia de caracteres de conexão SSH. Use essa cadeia de conexão de um terminal SSH (como [Putty](https://www.putty.org/)) para conectar a máquina virtual.

Se você selecionar o **RDP** opção, um arquivo RDP é baixada em seu computador. Salvá-lo e abri-lo para se conectar à máquina. 

## <a name="next-steps"></a>Próximas etapas
Confira os seguintes artigos:

- [Como administrador, crie e gerencie contas de laboratório](how-to-manage-lab-accounts.md)
- [Como um proprietário de laboratório, crie e gerencie laboratórios](how-to-manage-classroom-labs.md)
- [Como um proprietário de laboratório, configure e publique modelos](how-to-create-manage-template.md)
- [Como um usuário de laboratório, acesse os laboratórios de sala de aula](how-to-use-classroom-lab.md)


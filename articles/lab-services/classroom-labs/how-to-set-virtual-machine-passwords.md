---
title: Definir senhas para as VMs no Azure Lab Services | Microsoft Docs
description: Saiba como definir e redefinir senhas para máquinas virtuais (VMs) em laboratórios de sala de aula do Azure Lab Services.
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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 3701c69ff97d840f6cba175df8f02bc55ece498b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67123195"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs"></a>Defina ou redefina a senha para máquinas virtuais nos laboratórios de sala de aula
Este artigo fornece maneiras diferentes de senhas de configuração e resettings para acessar as VMs nos laboratórios de sala de aula. 

## <a name="lab-owners-teachers"></a>Proprietários de laboratório (professores)
Um proprietário de laboratório (professor) pode definir uma senha para VMs no laboratório na **definir credenciais** página do Assistente de criação de laboratório.

![Definir as credenciais](../media/tutorial-setup-classroom-lab/set-credentials.png)

Em seguida, o proprietário de laboratório pode redefinir a senha (se necessário) sobre o **configurar modelo** página do Assistente de criação de laboratório. 

![Página de configuração do modelo após a conclusão](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)

O proprietário de laboratório também pode redefinir a senha depois que o laboratório for criado, no painel. 

![Redefinição de senha menu na home page](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)

Em seguida, insira a nova senha na **definir senha** da página e selecione **definir senha**. 

![Redefinição de senha menu na home page](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="lab-users-students"></a>Usuários do laboratório (estudantes)
No momento da criação do laboratório. Ao mesmo tempo, o proprietário de laboratório pode habilitar ou desabilitar, permitindo que os alunos para definir suas próprias senhas para as VMs. Se o proprietário de laboratório (professor) habilita essa opção, o usuário de laboratório (aluno) terá uma opção para definir a senha para a VM quando o aluno faz logon na VM pela primeira vez. Em um **VM Windows**, pressione **CTRL + ALT + END**e selecione **alterar senha**. 

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre outras opções de uso de alunos você (como um proprietário de laboratório) podem configurar, consulte o seguinte artigo: [Configurar o uso do aluno](how-to-configure-student-usage.md).

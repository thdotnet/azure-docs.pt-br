---
title: Conceitos dos laboratórios de sala de aula - Azure Lab Services | Microsoft Docs
description: Aprenda os conceitos básicos de serviços de laboratório e como ele pode tornar fácil criar e gerenciar os laboratórios.
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
ms.date: 02/26/2019
ms.author: spelluru
ms.openlocfilehash: 8bbb486b0dbf1a5e25f5ee4d1f8e5e01b999a8ba
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067386"
---
# <a name="classroom-labs-concepts"></a>Conceitos dos laboratórios de sala de aula
A lista a seguir contém definições e principais conceitos de serviços de laboratório:

## <a name="quota"></a>Quota
Cota é o limite de tempo (em horas) que um professor pode definir para um aluno usar um laboratório de VM. Ele pode ser definido como 0 ou um número específico de horas. Se a cota é definida como 0, um aluno pode usar apenas a máquina virtual quando uma agenda está em execução ou quando um professor ativa manualmente a máquina virtual para o aluno.
 
## <a name="schedules"></a>Agendas
As agendas são os slots de tempo (uma vez ou recorrente) que um professor pode criar para a classe. Todas as máquinas virtuais no laboratório são iniciadas automaticamente no início para a agenda e eles são interrompidos no final da agenda. Horas de cota não são usadas quando uma agenda está em execução.

## <a name="template-virtual-machine"></a>Máquina virtual de modelo
Uma máquina virtual de modelo em um laboratório é uma imagem de máquina virtual base da qual as máquinas virtuais de todos os usuários são criadas. Criadores de treinadores/laboratório configurar a máquina virtual de modelo e configurá-lo com o software que eles desejam fornecer aos participantes de treinamento para fazer os laboratórios. Quando você publica um modelo de VM, o Azure Lab Services cria ou atualiza as VMs de laboratório com base no modelo de VM. 


## <a name="user-profiles"></a>Perfis do usuário
Este artigo descreve os diferentes perfis de usuário no Azure Lab Services. 

### <a name="lab-account-owner"></a>Proprietário da conta do laboratório
Normalmente, e o administrador de TI dos recursos de nuvem da organização, que tem a assinatura do Azure atua como um proprietário de conta de laboratório e realiza as seguintes tarefas:   

- Define uma conta de laboratório para sua organização.
- Gerencia e configura as políticas em todos os laboratórios.
- Concede permissões para as pessoas na organização para criarem um laboratório usando a conta do laboratório.

### <a name="professor"></a>Professor
Normalmente, os usuários como um professor ou um instrutor online criam laboratórios de sala de aula em uma conta de laboratório. Um educador executa as seguintes tarefas: 

- Cria um laboratório de sala de aula.
- Cria máquinas virtuais no laboratório. 
- Instala o software apropriado em máquinas virtuais.
- Especifica quem pode acessar o laboratório.
- Fornece um link de registro para o laboratório para alunos.

### <a name="student"></a>Aluno
Um estudante executa as seguintes tarefas:

- Usa o link de registro que o usuário do laboratório recebe de um criador do laboratório para registrar com o laboratório. 
- Conecta-se a uma máquina virtual no laboratório e usa-a para fazer trabalhos, tarefas e projetos em sala de aula. 

## <a name="next-steps"></a>Próximas etapas
Introdução à configuração de uma conta de laboratório necessária para criar um laboratório de sala de aula usando o Azure Lab Services:

- [Configurar uma conta de laboratório](tutorial-setup-lab-account.md)

---
title: Use as máquinas virtuais de ciência de dados do Azure
description: Conecte-se a um Azure ciência de dados Máquina Virtual (DSVM) para estender o poder de computação disponível para Notebooks do Azure.
services: app-service
documentationcenter: ''
author: getroyer
manager: andneil
ms.assetid: 0ccc2529-e17f-4221-b7c7-9496d6a731cc
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2019
ms.author: getroyer
ms.openlocfilehash: d4321fe60d47bf942e2c413ba81c2c9f54317fd1
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65597860"
---
# <a name="use-azure-data-science-virtual-machines"></a>Use as máquinas virtuais de ciência de dados do Azure

Por padrão, os projetos executados **computação gratuita** camada, o que é limitada a 4 GB de memória e 1 GB de dados para evitar abusos. Você pode ignorar essas limitações usando uma máquina virtual diferente da provisionada em uma assinatura do Azure. Para essa finalidade, a melhor opção é uma máquina Virtual de ciência de dados (DSVM) do Azure usando o **máquina de Virtual de ciência de dados para Linux (Ubuntu)** imagem. Tal uma DSVM vem pré-configurada com tudo o que você precisa para blocos de anotações do Azure e é exibido automaticamente na **executar** lista suspensa em blocos de anotações do Azure.

> [!Note]
> Blocos de anotações do Azure tem suporte apenas em DSVMs criados com a imagem do Linux Ubuntu em. Não há suporte para blocos de anotações nas imagens CentOS Linux, Windows 2016 ou Windows 2012.

## <a name="create-a-dsvm-instance"></a>Criar uma instância DSVM

Para criar uma nova instância de DSVM, siga as instruções para [Criar uma VM de Ciência de Dados do Ubuntu](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Para mais informações, inclusive os detalhes de preços, consulte [as máquinas virtuais de ciência de dados](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="connect-to-the-dsvm"></a>Conectar-se à DSVM

Após o DSVM estiver criado, selecione o **executar** lista suspensa no Azure Notebooks painel do projeto e selecione a instância apropriada do DSVM. A lista suspensa mostra as instâncias do DSVM se as seguintes condições forem verdadeiras:

- Você está conectado ao Azure Notebooks com uma conta que usa o AAD (Azure Active Directory), por exemplo, uma conta corporativa.
- Sua conta está conectada a uma assinatura do Azure.
- Você tem uma ou mais máquinas virtuais na assinatura, com pelo menos acesso de leitor, que usa a máquina Virtual de ciência de dados para a imagem do Linux (Ubuntu).)

![Instâncias de Máquina Virtual de Ciência de Dados na lista suspensa no painel do projeto](media/project-compute-tier-dsvm.png)

Quando você seleciona uma instância de DSVM, o Azure Notebooks pode solicitar as credenciais de máquina específicas que usou quando criou a VM.

Se qualquer uma das condições não forem atendidos, você poderá se conectar com a DSVM. Na lista suspensa, selecione o **direta de computação** opção, que solicitará um nome (para mostrar na lista), endereço IP da VM e porta (normalmente 8000, a porta padrão para que monitora JupyterHub) e as credenciais VM:

![Prompt que coleta informações do servidor para a opção de Computação Direta](media/project-compute-tier-direct.png)

Você pode obter esses valores da página de DSVM no portal do Azure.

## <a name="accessing-azure-notebooks-files-from-the-dsvm"></a>Acessar arquivos do Azure Notebooks do DSVM

Para preservar a paridade de caminhos de arquivo com o **computação gratuita** camada, é possível apenas abrir um projeto ao mesmo tempo em uma DSVM. Para abrir um novo projeto, você deve desligar o projeto aberto pela primeira vez.

![Botão de desligamento em blocos de anotações do Azure](media/shutdown.png)

Quando um projeto é executado em uma máquina virtual, os arquivos são montados no diretório raiz do servidor do Jupyter (o diretório mostrado no JupyterHub), substituindo os arquivos de blocos de anotações do Azure padrão. Quando você desligar a VM usando o **desligamento** botão no bloco de anotações da interface do usuário, os blocos de anotações do Azure restaura os arquivos padrão.

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre DSVMs na [Introdução ao Azure dados máquinas virtuais de ciência](/machine-learning/data-science-virtual-machine/overview).

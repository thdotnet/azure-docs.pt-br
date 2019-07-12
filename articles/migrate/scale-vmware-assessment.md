---
title: Avaliar grandes números de VMs VMware para a migração para o Azure com migrações para Azure | Microsoft Docs
description: Descreve como avaliar grandes números de VMs VMware para migração para o Azure usando o serviço migrações para Azure.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: 6102a1c59be3627b95dc1e0cb1d1d712d5832d2f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811330"
---
# <a name="assess-large-numbers-of-vmware-vms-for-migration-to-azure"></a>Avaliar grandes números de VMs VMware para migração para o Azure


Este artigo descreve como avaliar grandes números (1000 35.000) de VMs do VMware locais para migração para o Azure, usando a ferramenta de avaliação de servidor de migrar do Azure

[As migrações para Azure](migrate-services-overview.md) fornece um hub de ferramentas que ajudam você a descobrir, avaliar e migrar cargas de trabalho, infraestrutura e aplicativos para o Microsoft Azure. O hub inclui ferramentas de migrações para Azure e as ofertas do ISV (fornecedor) independentes de software de terceiros. 

Neste artigo, você aprenderá a:
> [!div class="checklist"]
> * Plano de avaliação em escala.
> * Configurar permissões do Azure e preparar o VMware para avaliação.
> * Criar um projeto de migrações para Azure e criar uma avaliação.
> * Examine a avaliação durante o planejamento para a migração.


> [!NOTE]
> Se você quiser experimentar uma prova de conceito para avaliar duas VMs antes de avaliar em grande escala, siga nosso [série de tutoriais](tutorial-prepare-vmware.md)

## <a name="plan-for-assessment"></a>Plano para avaliação

Ao planejar a avaliação de grande número de VMs VMware, há algumas coisas a considerar:

- **Planejar projetos de migrações para Azure**: Descubra como implantar projetos do migrações para Azure. Por exemplo, se seus data centers estão em regiões diferentes, ou você precisa armazenar a descoberta, avaliação ou metadados relacionados à migração em uma geografia diferente, talvez seja necessário vários projetos. 
- **Planejar soluções**: As migrações para Azure usam um dispositivo migrações para Azure de local, implantado como uma VM do VMware, para descobrir continuamente as VMs. O dispositivo monitora as alterações de ambiente como a adição de VMs, discos ou adaptadores de rede. Ele também envia dados de desempenho e metadados sobre elas para o Azure. Você precisará descobrir quantos dispositivos você precisa implantar.
- **Plano de contas para a descoberta de**: O dispositivo de migrações para Azure usa uma conta com acesso ao servidor do vCenter para descobrir as VMs para avaliação e migração. Se você estiver descobrindo VMs mais de 10.000, configure várias contas.


## <a name="planning-limits"></a>Planejamento de limites
 
Use os limites resumidos nesta tabela para o planejamento.

**Planejamento** | **Limites**
--- | --- 
**Projetos de migração do Azure** | Avalie até 35.000 VMs em um projeto.
**Dispositivo de migrações para Azure** | Só pode se conectar a um dispositivo de um único servidor do vCenter.<br/><br/> Um dispositivo só pode ser associado um único projeto de migrações para Azure.<br/> Um dispositivo pode descobrir até 10.000 VMs em um servidor vCenter.
**Avaliação de migração do Azure** | Você pode avaliar até 35.000 VMs em uma única avaliação.

Com esses limites em mente, aqui estão alguns exemplos de implantação:


**vCenter Server** | **VMs no servidor** | **Recomendações** | **Ação**
---|---|---
Um | < 10,000 | Um projeto de migrações para Azure.<br/> Um dos dispositivos.<br/> Uma conta do vCenter para descoberta. | Configurar dispositivo, conecte-se ao servidor do vCenter com uma conta.
Um | > 10,000 | Um projeto de migrações para Azure.<br/> Vários dispositivos.<br/> Várias contas do vCenter. | Configure o dispositivo para cada 10.000 VMs.<br/><br/> Configurar contas do vCenter e divida o estoque para limitar o acesso para uma conta para VMs de menos de 10.000.<br/> Conecte cada dispositivo ao servidor do vCenter com uma conta.<br/> Você pode analisar as dependências entre computadores que são descobertos com diferentes dispositivos.
Vários | < 10,000 |  Um projeto de migrações para Azure.<br/> Vários dispositivos.<br/> Uma conta do vCenter para descoberta. | Configurar dispositivos, conecte-se ao servidor do vCenter com uma conta.<br/> Você pode analisar as dependências entre computadores que são descobertos com diferentes dispositivos.
Vários | > 10,000 | Um projeto de migrações para Azure.<br/> Vários dispositivos.<br/> Várias contas do vCenter. | Se a descoberta do vCenter Server VMs < 10.000, configurar um dispositivo para cada servidor do vCenter.<br/><br/> Se descoberta do vCenter Server > 10.000 VMs, configurar um dispositivo para cada 10.000 VMs.<br/> Configurar contas do vCenter e divida o estoque para limitar o acesso para uma conta para VMs de menos de 10.000.<br/> Conecte cada dispositivo ao servidor do vCenter com uma conta.<br/> Você pode analisar as dependências entre computadores que são descobertos com diferentes dispositivos.


## <a name="plan-discovery-in-a-multi-tenant-environment"></a>Descoberta de plano em um ambiente de multilocatário

Se você estiver planejando um ambiente multilocatário, você pode definir o escopo de descoberta no vCenter Server.

- Você pode definir o escopo de descoberta do dispositivo para um vCenter Server datacenter, o cluster ou a pasta de clusters, o host ou a pasta de hosts ou máquinas virtuais individuais.
- Se seu ambiente é compartilhado entre locatários e você não quiser descobrir cada locatário separadamente, você pode definir o escopo de acesso à conta do vCenter que o dispositivo usa para a descoberta. 
    - Se os locatários compartilham os hosts, crie as credenciais com acesso somente leitura para as VMs que pertencem ao locatário específico. 
    - Use essas credenciais para a descoberta de dispositivo de migrações para Azure.
    - Avaliação de migração do Azure não pode descobrir VMs se a conta do vCenter tem acesso concedido no nível de pasta da VM do vCenter. Há suporte para pastas de hosts e clusters. 

## <a name="prepare-for-assessment"></a>Preparar para a avaliação

Prepare o Azure e do VMware para avaliação de servidor. 

1. Verifique se [VMware dar suporte a requisitos e limitações](migrate-support-matrix-vmware.md).
2. Configure permissões para sua conta do Azure interagir com as migrações para Azure.
3. Prepare o VMware para avaliação.


Siga as instruções em [este tutorial](tutorial-prepare-vmware.md) para definir essas configurações.


## <a name="create-a-project"></a>Criar um projeto

Acordo com suas necessidades de planejamento, faça o seguinte:

1. Crie projetos de migrações para Azure.
2. Adicione a ferramenta de avaliação de servidor de migrar do Azure para os projetos.

[Saiba mais](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Criar e revisar uma avaliação

1. Crie avaliações para VMs do VMware.
1. Examine as avaliações em preparação para o planejamento da migração.


Siga as instruções em [este tutorial](tutorial-assess-vmware.md) para definir essas configurações.
    

## <a name="next-steps"></a>Próximas etapas

Neste artigo você:
 
> [!div class="checklist"] 
> * Planejado para dimensionar as avaliações de migrações para Azure para VMs do VMware
> * VMware para avaliação e Azure preparado
> * Criou um projeto de migrações para Azure e executou avaliações
> * Revisadas as avaliações em preparação para migração.

Agora, [Saiba como](concepts-assessment-calculation.md) as avaliações são calculadas e como [modificar avaliações](how-to-modify-assessment.md).

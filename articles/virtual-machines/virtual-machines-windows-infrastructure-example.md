---
title: Passo a passo da infraestrutura do Azure de exemplo | Microsoft Docs
description: "Saiba mais sobre as principais diretrizes de design e implementação referentes à implantação de uma infraestrutura de exemplo no Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 1e52ae69951b6b1feee6207033a85a583d13bcc2
ms.openlocfilehash: 851dd4a2a76eff0e2f95c9a0a7280986cfb3e26a


---
# <a name="example-azure-infrastructure-walkthrough"></a>Passo a passo de infraestrutura do Azure de exemplo
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Este artigo explica como criar uma infraestrutura de aplicativo de exemplo. Fornecemos detalhes de como projetar uma infraestrutura para um repositório online simples que reúne todas as diretrizes e decisões sobre convenções de nomenclatura, conjuntos de disponibilidade, redes virtuais e balanceadores de carga, bem como a implantação de fato das VMs (máquinas virtuais).

## <a name="example-workload"></a>Carga de trabalho de exemplo
A Adventure Works Cycles deseja criar um aplicativo do repositório online no Azure que consiste em:

* Dois servidores IIS que executam o front-end do cliente em uma camada da Web
* Dois servidores IIS que processam dados e pedidos em uma camada de aplicativo
* Duas instâncias do Microsoft SQL Server com grupos de disponibilidade AlwaysOn (dois SQL Servers e uma testemunha do nó principal) para armazenar dados de produtos e pedidos em uma camada de banco de dados
* Dois controladores de domínio do Active Directory para contas de clientes e fornecedores em uma camada de autenticação
* Todos os servidores estão localizados em duas sub-redes:
  * uma sub-rede de front-end para os servidores Web 
  * uma sub-rede de back-end para os servidores de aplicativos, cluster do SQL e controladores de domínio

![Diagrama de níveis diferentes de infraestrutura de aplicativos](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

O tráfego seguro da Web de entrada deve ter balanceamento de carga entre os servidores Web enquanto os clientes navegam pelo repositório online. O tráfego de processamento de pedidos na forma de solicitações HTTP dos servidores Web deve ser balanceado entre os servidores de aplicativos. Além disso, a infraestrutura deve ser projetada para alta disponibilidade.

O projeto resultante deve incorporar:

* Uma assinatura e uma conta do Azure
* Um único grupo de recursos
* Contas de armazenamento
* Uma rede virtual com duas sub-redes
* Conjuntos de disponibilidade para as VMs com funções semelhantes
* Máquinas virtuais

Todos os itens acima seguem estas convenções de nomenclatura:

* A Adventure Works Cycles usa **[carga de trabalho de TI]-[localização]-[recurso do Azure]** como prefixo
  * Neste exemplo, “**azos**” (Repositório Online do Azure) é o nome de carga de trabalho de TI e “**use**” (Leste dos EUA 2) é a localização
* As contas de armazenamento usam adventureazosusesa**[descrição]**
  * 'adventure' foi adicionado ao prefixo para garantir a exclusividade, e os nomes de conta de armazenamento não dão suporte ao uso de hifens.
* As redes virtuais usam AZOS-USE-VN**[número]**
* Os conjuntos de disponibilidade usam azos-use-as-**[função]**
* Os nomes de máquina virtual usam azos-use-vm-**[nomevm]**

## <a name="azure-subscriptions-and-accounts"></a>Assinaturas e contas do Azure
A Adventure Works Cycles usa sua assinatura Enterprise, chamada Adventure Works Enterprise Subscription, para fornecer cobrança para essa carga de trabalho de TI.

## <a name="storage-accounts"></a>Contas de armazenamento
A Adventure Works Cycles determinou que precisava de duas contas de armazenamento:

* **adventureazosusesawebapp** para o armazenamento padrão dos servidores Web, servidores de aplicativos, controladores de domínio e seus discos de dados.
* **adventureazosusesasql** para o Armazenamento Premium das VMs SQL Server e seus discos de dados.

## <a name="virtual-network-and-subnets"></a>Rede virtual e sub-redes
Como a rede virtual não precisa de conectividade contínua com a rede local da Adventure Work Cycles, ela decidiu usar uma rede virtual somente em nuvem.

Criaram uma rede virtual somente em nuvem com as seguintes configurações usando o portal do Azure:

* Name: AZOS-USE-VN01
* Local: Leste dos EUA 2
* Espaço de endereço da rede virtual: 10.0.0.0/8
* Primeira sub-rede:
  * Nome: FrontEnd
  * Espaço de endereço: 10.0.1.0/24
* Segunda sub-rede:
  * Nome: BackEnd
  * Espaço de endereço: 10.0.2.0/24

## <a name="availability-sets"></a>Conjuntos de disponibilidade
Para manter a alta disponibilidade de todas as quatro camadas de seu repositório online, a Adventure Works Cycles decidiu usar quatro conjuntos de disponibilidade:

* **azos-use-as-web** para os servidores Web
* **azos-use-as-app** para os servidores de aplicativos
* **azos-use-as-sql** para os SQL Servers
* **azos-use-as-dc** para os controladores de domínio

## <a name="virtual-machines"></a>Máquinas virtuais
A Adventure Works Cycles decidiu usar os seguintes nomes para suas VMs do Azure:

* **azos-use-vm-web01** para o primeiro servidor Web
* **azos-use-vm-web02** para o segundo servidor Web
* **azos-use-vm-app01** para o primeiro servidor de aplicativos
* **azos-use-vm-app02** para o segundo servidor de aplicativos
* **azos-use-vm-sql01** para o primeiro SQL Server no cluster
* **azos-use-vm-sql02** para o segundo SQL Server no cluster
* **azos-use-vm-dc01** para o primeiro controlador de domínio
* **azos-use-vm-dc02** para o segundo controlador de domínio

Veja abaixo a configuração resultante.

![Infraestrutura final do aplicativo implantada no Azure](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Essa configuração inclui:

* Uma rede virtual somente em nuvem com duas sub-redes (front-end e back-end)
* Duas contas de armazenamento
* Quatro conjuntos de disponibilidade, um para cada camada do repositório online
* As máquinas virtuais das quatro camadas
* Um conjunto de balanceamento de carga externo para tráfego da Web baseado em HTTPS da Internet para os servidores Web
* Um conjunto de balanceamento de carga interno para tráfego da Web dos servidores Web para os servidores de aplicativos
* Um único grupo de recursos

## <a name="next-steps"></a>Próximas etapas
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]




<!--HONumber=Dec16_HO3-->



---
title: Como usar o Gerenciamento de API do Azure com redes virtuais internas | Microsoft Docs
description: Saiba como instalar e configurar o Gerenciamento de API do Azure na rede virtual interna
services: api-management
documentationcenter: ''
author: vladvino
manager: kjoshi
editor: ''
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: apimpm
ms.openlocfilehash: a5d8a724a0b4dd6899a71187176b9d444e5fe19c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051686"
---
# <a name="using-azure-api-management-service-with-an-internal-virtual-network"></a>Usar o serviço de Gerenciamento de API do Azure com rede virtual interna
Com as VNETs (Redes Virtuais) do Azure, o Gerenciamento de API pode gerenciar as APIs que não estão acessíveis pela Internet. Várias tecnologias de VPN estão disponíveis para fazer a conexão. O Gerenciamento de API pode ser implantado em dois modos principais dentro de uma rede virtual:
* Externo
* Interna

Quando o gerenciamento de API é implantado no modo de rede virtual interna, todos os serviço pontos de extremidade (o gateway de proxy, o desenvolvedor portal, o gerenciamento direto e Git) somente são visíveis em uma rede virtual que você controle o acesso ao. Nenhum dos pontos de extremidade de serviço é registrado no servidor DNS público.

> [!NOTE]
> Porque existem entradas DNS para os pontos de extremidade de serviço, esses pontos de extremidade não estará acessíveis até [DNS está configurado](#apim-dns-configuration) para a rede virtual.

Usando o Gerenciamento de API no modo interno você pode chegar aos seguintes cenários:

* Torne as APIs hospedadas em seu datacenter privado seguras e acessíveis por terceiros externamente usando as conexões VPN Site a Site ou ExpressRoute do Azure.
* Habilite cenários de nuvem híbrida expondo as APIs baseadas em nuvem e as APIs locais por meio de um gateway comum.
* Gerencie suas APIs hospedadas em várias localizações geográficas usando um único ponto de extremidade de gateway.

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para executar as etapas descritas neste artigo, você precisa ter:

+ **Uma assinatura ativa do Azure**.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ **Uma instância de Gerenciamento de API do Azure**. Para obter mais informações, consulte [Criar uma instância do Gerenciamento de API do Azure](get-started-create-service-instance.md).
+ Quando um serviço de gerenciamento de API é implantado em uma rede virtual, uma [lista de portas](./api-management-using-with-vnet.md#required-ports) são usados e precisam ser abertas. 

## <a name="enable-vpn"> </a>Criar um Gerenciamento de API em uma rede virtual interna
O serviço de gerenciamento de API em uma rede virtual interna é hospedado por trás de um [balanceador de carga interno (clássico)](https://docs.microsoft.com/azure/load-balancer/load-balancer-get-started-ilb-classic-cloud). Isso é a única opção disponível e não pode ser alterado.

### <a name="enable-a-virtual-network-connection-using-the-azure-portal"></a>Habilite uma conexão de rede virtual usando o portal do Azure

1. Navegue até sua instância de Gerenciamento de API do Azure no [portal do Azure](https://portal.azure.com/).
2. Selecione **Rede virtual**.
3. Configure a instância de Gerenciamento de API a ser implantada dentro da rede virtual.

    ![Menu para configurar um Gerenciamento de API do Azure em uma rede virtual interna][api-management-using-internal-vnet-menu]

4. Clique em **Salvar**.

Após a implantação bem-sucedida, você deve ver **privados** endereço IP virtual e **público** endereço IP virtual do seu serviço de gerenciamento de API na folha de visão geral. O **privados** endereço IP virtual é uma carga equilibrado endereço IP dentro do gerenciamento de API delegada sub-rede em que `gateway`, `portal`, `management` e `scm` pontos de extremidade podem ser acessados. O **pública** endereço IP virtual é usado **somente** para o tráfego do plano de controle `management` ponto de extremidade em porta 3443 e possam ser bloqueadas para baixo até a [ApiManagement] [ ServiceTags] servicetag.

![Painel de Gerenciamento de API com uma rede virtual interna configurada][api-management-internal-vnet-dashboard]

> [!NOTE]
> O console de Teste disponível no portal do Azure não funcionará para o serviço implantado do VNET **Interno**, pois a URL do Gateway não está registrada no DNS público. É necessário usar o Console de Teste fornecido no **portal do Desenvolvedor**.

### <a name="enable-a-virtual-network-connection-by-using-powershell-cmdlets"></a>Habilitar uma conexão de rede virtual usando cmdlets do PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Você também pode habilitar a conectividade de rede virtual usando cmdlets do PowerShell.

* Crie um serviço de gerenciamento de API dentro de uma rede virtual: Use o cmdlet [New-AzApiManagement](/powershell/module/az.apimanagement/new-azapimanagement) para criar um serviço de gerenciamento de API dentro de uma rede virtual e configurá-lo para usar o tipo de rede virtual interna.

* Atualize uma implantação existente de um serviço de gerenciamento de API dentro de uma rede virtual: Use o cmdlet [AzApiManagementRegion atualização](/powershell/module/az.apimanagement/update-azapimanagementregion) mover um serviço de gerenciamento de API existente dentro de uma rede virtual e configurá-lo para usar o tipo de rede virtual interna.

## <a name="apim-dns-configuration"></a>Configuração de DNS
Quando o Gerenciamento de API está no modo de rede virtual externa, o DNS é gerenciado pelo Azure. Para o modo de rede virtual interna, você precisa gerenciar o seu próprio roteamento.

> [!NOTE]
> O serviço de Gerenciamento de API não escuta as solicitações que vêm de endereços IP. Ele só responde às solicitações para o nome de host configurado em seus pontos de extremidade de serviço. Esses pontos de extremidade incluem o gateway, o portal do Azure, o portal do Desenvolvedor, o ponto de extremidade de gerenciamento direto e o Git.

### <a name="access-on-default-host-names"></a>Acesso em nomes de host padrão
Quando você cria um serviço de gerenciamento de API, chamado "contosointernalvnet" por exemplo, os seguintes pontos de extremidade de serviço são configurados por padrão:

   * Gateway ou proxy: contosointernalvnet.azure Azure-API.NET

   * O portal do Azure e o portal do desenvolvedor: contosointernalvnet.portal.azure Azure-API.NET

   * Ponto de extremidade de gerenciamento direto: contosointernalvnet.management.azure Azure-API.NET

   * Git: contosointernalvnet.scm.azure-api.net

Para acessar esses pontos de extremidade do serviço de Gerenciamento de API, você pode criar uma máquina virtual em uma sub-rede conectada à rede virtual na qual o Gerenciamento de API está implantado. Supondo que o endereço IP virtual interno para seu serviço seja 10.1.0.5, você pode mapear o arquivo de hosts % SystemDrive%\drivers\etc\hosts, da seguinte maneira:

   * 10.1.0.5     contosointernalvnet.azure-api.net

   * 10.1.0.5     contosointernalvnet.portal.azure-api.net

   * 10.1.0.5     contosointernalvnet.management.azure-api.net

   * 10.1.0.5     contosointernalvnet.scm.azure-api.net

Então você pode acessar todos os pontos de extremidade do serviço da máquina virtual criada.
Se você usar um servidor DNS personalizado em uma rede virtual, também poderá criar registros DNS A e acessar esses pontos de extremidade de qualquer lugar em sua rede virtual.

### <a name="access-on-custom-domain-names"></a>Acessar em nomes de domínio personalizados

1. Se você não quiser acessar o serviço de Gerenciamento de API com os nomes de host padrão, poderá configurar nomes de domínio personalizados para todos os seus pontos de extremidade de serviço, conforme mostra a imagem abaixo:

   ![Configurar o domínio personalizado para Gerenciamento de API][api-management-custom-domain-name]

2. Em seguida, você pode criar registros no seu servidor DNS para acessar os pontos de extremidade que só estão acessíveis pela rede virtual.

## Roteamento do <a name="routing"> </a>

* Uma carga balanceada *privada* endereço IP virtual do intervalo de sub-rede será reservado e usado para acessar os pontos de extremidade de serviço Gerenciamento de API de dentro da rede virtual. Isso *privada* endereço IP pode ser encontrado na folha de visão geral para o serviço no portal do Azure. Esse endereço deve ser registrado com os servidores DNS usados pela rede virtual.
* Uma carga balanceada *pública* endereço IP (VIP) também será reservado para fornecer acesso ao ponto de extremidade de serviço de gerenciamento pela porta 3443. Isso *pública* endereço IP pode ser encontrado na folha de visão geral para o serviço no portal do Azure. O *pública* endereço IP é usado apenas para controlar o tráfego de plano para o `management` ponto de extremidade em porta 3443 e possam ser bloqueadas para baixo até a [ApiManagement] [ ServiceTags] servicetag .
* Endereços IP do intervalo de sub-rede IP (DIP) serão atribuídos a cada VM no serviço e serão usado para acessar recursos na rede virtual. Um endereço IP público (VIP) será ser usado para acessar recursos fora da rede virtual. Se as listas de restrição de IP são usadas para proteger os recursos dentro da rede virtual, todo o intervalo para a sub-rede em que o serviço é implantado o gerenciamento de API deve especificado para conceder ou restringir o acesso do serviço.
* A com balanceamento de carga público e endereços IP privados podem ser encontrados na folha de visão geral no portal do Azure.
* Os endereços IP atribuídos para acesso público e privado podem mudar se o serviço é removido do e, em seguida, adicionado novamente à rede virtual. Se isso acontecer, ele pode ser necessário atualizar registros de DNS, regras de roteamento e listas de restrição de IP na rede virtual.

## <a name="related-content"> </a>Conteúdo relacionado
Para saber mais, consulte os seguintes artigos:
* [Problemas comuns de configuração de rede ao configurar o Gerenciamento de API do Azure em uma rede virtual][Common network configuration problems]
* [Perguntas frequentes sobre rede virtual](../virtual-network/virtual-networks-faq.md)
* [Criando um registro no DNS](/previous-versions/windows/it-pro/windows-2000-server/bb727018(v=technet.10))

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-using-with-internal-vnet.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: get-started-create-service-instance.md
[Common network configuration problems]: api-management-using-with-vnet.md#network-configuration-issues

[ServiceTags]: ../virtual-network/security-overview.md#service-tags


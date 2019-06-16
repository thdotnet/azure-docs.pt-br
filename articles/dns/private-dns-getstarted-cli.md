---
title: Criar uma zona privada do DNS do Azure usando a CLI do Azure
description: Neste procedimento, você pode cria e testar uma zona DNS e registro no DNS do Azure. Este é uma guia passo a passo para criar e gerenciar sua primeira zona e registro de DNS privado usando a CLI do Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 6/13/2019
ms.author: victorh
ms.openlocfilehash: d882a9c40efc5e9bcb1a5e1c02f1ac73970d57db
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076423"
---
# <a name="create-an-azure-dns-private-zone-using-the-azure-cli"></a>Criar uma zona privada do DNS do Azure usando a CLI do Azure

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Esse procedimento explicará as etapas para criar sua primeira zona DNS privada e o registro usando a CLI do Azure.

Uma zona DNS é usada para hospedar os registros DNS para um domínio específico. Para iniciar a hospedagem do seu domínio no DNS do Azure, você precisará criar uma zona DNS para esse nome de domínio. Cada registro DNS para seu domínio é criado dentro dessa zona DNS. Para publicar uma zona de DNS privado em sua rede virtual, você deve especificar a lista de redes virtuais que podem resolver registros na zona.  Eles são chamados *vinculado* redes virtuais. Quando o registro automático estiver habilitado, o DNS do Azure também atualiza os registros da zona sempre que uma máquina virtual é criada, alterações de seu ' endereço IP, ou é excluída.

Neste procedimento, você aprenderá como:

> [!div class="checklist"]
> * Criar uma zona de DNS privado
> * Criar máquinas virtuais de teste
> * Criar um registro DNS adicional
> * Testar a zona privada

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Se preferir, você poderá concluir este procedimento usando o [Azure PowerShell](private-dns-getstarted-powershell.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>Criar o grupo de recursos

Primeiro, crie um grupo de recursos para conter a zona DNS: 

```azurecli
az group create --name MyAzureResourceGroup --location "East US"
```

## <a name="create-a-dns-private-zone"></a>Criar uma zona de DNS privado

O exemplo a seguir cria uma rede virtual denominada **myAzureVNet**. Em seguida, ele cria uma zona DNS chamada **private.contoso.com** na **MyAzureResourceGroup** grupo de recursos, vincula-se a zona DNS para o **MyAzureVnet** rede virtual, e Habilita o registro automático.

```azurecli
az network vnet create \
  --name myAzureVNet \
  --resource-group MyAzureResourceGroup \
  --location eastus \
  --address-prefix 10.2.0.0/16 \
  --subnet-name backendSubnet \
  --subnet-prefixes 10.2.0.0/24

az network private-dns zone create -g MyAzureResourceGroup \
   -n private.contoso.com

az network private-dns link vnet create -g MyAzureResourceGroup -n MyDNSLink \
   -z private.contoso.com -v myAzureVNet -e true
```

Se você quiser criar uma zona apenas para a resolução de nome (nenhum registro automático de nome de host), você pode usar o `-e false` parâmetro.

### <a name="list-dns-private-zones"></a>Listar zonas de DNS privado

Para enumerar zonas DNS, use `az network private-dns zone list`. Para obter ajuda, consulte `az network dns zone list --help`.

Especificar o grupo de recursos lista apenas as zonas dentro do grupo de recursos:

```azurecli
az network private-dns zone list \
  -g MyAzureResourceGroup
```

Omitir o grupo de recursos lista todas as zonas na assinatura:

```azurecli
az network private-dns zone list 
```

## <a name="create-the-test-virtual-machines"></a>Criar as máquinas virtuais de teste

Agora, crie duas máquinas virtuais para que você possa testar a zona DNS privada:

```azurecli
az vm create \
 -n myVM01 \
 --admin-username AzureAdmin \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --nsg NSG01 \
 --nsg-rule RDP \
 --image win2016datacenter

az vm create \
 -n myVM02 \
 --admin-username AzureAdmin \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --nsg NSG01 \
 --nsg-rule RDP \
 --image win2016datacenter
```

Isso levará alguns minutos para ser concluído.

## <a name="create-an-additional-dns-record"></a>Criar um registro DNS adicional

Para criar um registro DNS, use o comando `az network private-dns record-set [record type] add-record`. Para obter ajuda para adicionar, por exemplo, os registros A, veja `az network private-dns record-set A add-record --help`.

 O exemplo a seguir cria um registro com o nome relativo **db** na Zona DNS **private.contoso.com** no grupo de recursos **MyAzureResourceGroup**. O nome totalmente qualificado do conjunto de registros é **db.private.contoso.com**. O tipo de registro é “A”, com o endereço IP “10.2.0.4”.

```azurecli
az network private-dns record-set a add-record \
  -g MyAzureResourceGroup \
  -z private.contoso.com \
  -n db \
  -a 10.2.0.4
```

### <a name="view-dns-records"></a>Exibir registros DNS

Para listar os registros DNS em sua zona, execute:

```azurecli
az network private-dns record-set list \
  -g MyAzureResourceGroup \
  -z private.contoso.com
```

## <a name="test-the-private-zone"></a>Testar a zona privada

Agora você pode testar a resolução de nome para a zona privada **private.contoso.com**.

### <a name="configure-vms-to-allow-inbound-icmp"></a>Configurar VMs para permitir ICMP de entrada

Você pode usar o comando ping para testar a resolução de nome. Portanto, configure o firewall em ambas as máquinas virtuais para permitir pacotes ICMP de entrada.

1. Conecte-se a myVM01 e abra uma janela do Windows PowerShell com privilégios de administrador.
2. Execute o comando a seguir:

   ```powershell
   New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
   ```

Repita para myVM02.

### <a name="ping-the-vms-by-name"></a>Executar ping de VMs por nome

1. No prompt de comando do Windows PowerShell myVM02, execute ping em myVM01 usando o nome de host registrado automaticamente:

   ```
   ping myVM01.private.contoso.com
   ```

   Você deve ver uma saída semelhante a esta:

   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```

2. Execute ping no nome **db** que você criou anteriormente:

   ```
   ping db.private.contoso.com
   ```

   Você deve ver uma saída semelhante a esta:

   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Excluir todos os recursos

Quando não for mais necessário, exclua o **MyAzureResourceGroup** grupo de recursos para excluir os recursos criados neste procedimento.

```azurecli
az group delete --name MyAzureResourceGroup
```

## <a name="next-steps"></a>Próximas etapas

Neste procedimento, você implantado uma zona DNS privada, criada um registro DNS e testou a zona.
Agora você pode aprender mais sobre zonas DNS privadas.

> [!div class="nextstepaction"]
> [Usando o DNS do Azure para domínios privados](private-dns-overview.md)

---
title: Habilitar a criptografia de disco para clusters do Windows do Azure Service Fabric | Microsoft Docs
description: Este artigo descreve como habilitar a criptografia de disco para nós de cluster do Service Fabric do Azure usando o Azure Key Vault no Azure Resource Manager.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: navya
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/22/2019
ms.author: aljo
ms.openlocfilehash: c31fc43729bcb58c755959db0c8bc5185b8197f4
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66471404"
---
# <a name="enable-disk-encryption-for-azure-service-fabric-cluster-nodes-in-windows"></a>Habilitar a criptografia de disco para nós de cluster do Azure Service Fabric no Windows 
> [!div class="op_single_selector"]
> * [Criptografia de Disco para Windows](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Criptografia de Disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Neste tutorial, você aprenderá a habilitar a criptografia de disco em nós de cluster do Service Fabric no Windows. Você precisará seguir estas etapas para cada um dos tipos de nó e conjuntos de dimensionamento de máquina virtual. Para criptografar os nós, vamos usar a funcionalidade de criptografia de disco do Azure em conjuntos de dimensionamento de máquina virtual.

O guia abrange os seguintes tópicos:

* Principais conceitos a serem consideradas quando habilitar criptografia de disco em escala de máquina virtual de cluster do Service Fabric define no Windows.
* Etapas a serem seguidas antes de habilitar a criptografia de disco no Service Fabric nós de cluster no Windows.
* Etapas a serem seguidas para habilitar a criptografia de disco em nós de cluster do Service Fabric no Windows.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Pré-requisitos

**Self-registration** 

A visualização da criptografia de disco para o conjunto de dimensionamento de máquina virtual exige o registro de autoatendimento. Use as seguintes etapas: 

1. Primeiro, execute o seguinte comando:
    ```powershell
    Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
    ```
2. Aguarde 10 minutos até que lê o status *registrado*. Você pode verificar o status executando o seguinte comando: 
    ```powershell
    Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
    Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
    ```
**Cofre da Chave do Azure** 

1. Criar um cofre de chaves na mesma assinatura e região que o conjunto de dimensionamento e, em seguida, selecione a **EnabledForDiskEncryption** política no cofre de chaves de acesso usando o cmdlet do PowerShell. Você também pode definir a política usando a interface do usuário do Key Vault no portal do Azure com o seguinte comando:
    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
    ```
2. Instale a versão mais recente do [CLI do Azure](/cli/azure/install-azure-cli), que tem os novos comandos de criptografia.
3. Instale a versão mais recente do [SDK do Azure com o Azure PowerShell](https://github.com/Azure/azure-powershell/releases) de versão. A seguir está os cmdlets de Azure Disk Encryption para habilitar de conjunto de dimensionamento de máquinas virtuais ([definir](/powershell/module/az.compute/set-azvmssdiskencryptionextension)) a criptografia, recuperar ([obter](/powershell/module/az.compute/get-azvmssvmdiskencryption)) o status de criptografia e remover ([desabilitar](/powershell/module/az.compute/disable-azvmssdiskencryption)) instância do conjunto de criptografia na escala.

| Comando | Version |  `Source`  |
| ------------- |-------------| ------------|
| Get-AzVmssDiskEncryptionStatus   | 1.0.0 ou posterior | Az.Compute |
| Get-AzVmssVMDiskEncryptionStatus   | 1.0.0 ou posterior | Az.Compute |
| Disable-AzVmssDiskEncryption   | 1.0.0 ou posterior | Az.Compute |
| Get-AzVmssDiskEncryption   | 1.0.0 ou posterior | Az.Compute |
| Get-AzVmssVMDiskEncryption   | 1.0.0 ou posterior | Az.Compute |
| Set-AzVmssDiskEncryptionExtension   | 1.0.0 ou posterior | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Cenários com suporte para criptografia de disco
* Criptografia para conjuntos de dimensionamento de máquina virtual tem suporte apenas para conjuntos de dimensionamento criados com discos gerenciados. Não é suportado para conjuntos de escala de discos nativos (ou não gerenciados).
* Criptografia tem suporte para o sistema operacional e conjuntos de volumes de dados em escala de máquina virtual no Windows. Desabilitar a criptografia também tem suporte para o sistema operacional e conjuntos de volumes de dados para o dimensionamento de máquinas virtuais no Windows.
* Operações de recriação de imagem e atualização de máquina virtual para conjuntos de dimensionamento de máquinas virtuais não têm suporte na visualização atual.


## <a name="create-a-new-cluster-and-enable-disk-encryption"></a>Criar um novo cluster e habilitar a criptografia de disco

Use os comandos a seguir para criar um cluster e habilitar a criptografia de disco usando um modelo do Azure Resource Manager e um certificado autoassinado.

### <a name="sign-in-to-azure"></a>Entrar no Azure 
Entrar com os seguintes comandos:
```powershell
Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```azurecli

azure login
az account set --subscription $subscriptionId

```

### <a name="use-the-custom-template-that-you-already-have"></a>Usar um modelo personalizado que você já tem 

Se você precisar criar um modelo personalizado para atender às suas necessidades, é altamente recomendável que você inicie com um dos modelos que estão disponíveis na [exemplos de modelo de criação de cluster do Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) página. Para [personalizar o modelo de cluster] [ customize-your-cluster-template] seção, consulte as diretrizes a seguir.

Se você já tiver um modelo personalizado, verifique se todos os parâmetros relacionados ao certificado três no modelo e o arquivo de parâmetro são nomeados da seguinte maneira e que valores são nulos da seguinte maneira:

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```


```azurecli

declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```

### <a name="deploy-an-application-to-a-service-fabric-cluster-in-windows"></a>Implantar um aplicativo em um cluster do Service Fabric no Windows
Para implantar um aplicativo em seu cluster, siga as etapas e as diretrizes em [implantar e remover aplicativos usando o PowerShell](service-fabric-deploy-remove-applications.md).


### <a name="enable-disk-encryption-for-the-virtual-machine-scale-sets-created-previously"></a>Habilitar a criptografia de disco para os conjuntos de dimensionamento de máquina virtual criada anteriormente

Para habilitar a criptografia de disco para a escala de máquina virtual define você criou as etapas anteriores, execute os seguintes comandos:
 
```powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```azurecli

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


### <a name="validate-if-disk-encryption-is-enabled-for-a-virtual-machine-scale-set-in-windows"></a>Validar se a criptografia de disco está habilitada para uma escala de máquina virtual definido no Windows
Obter o status de um conjunto de dimensionamento de máquina virtual inteira ou em qualquer instância em um conjunto de dimensionamento, executando os seguintes comandos.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


Além disso, você pode entrar para o conjunto de dimensionamento de máquina virtual e verifique se que as unidades são criptografadas.

### <a name="disable-disk-encryption-for-a-virtual-machine-scale-set-in-a-service-fabric-cluster"></a>Desabilitar a criptografia de disco para um conjunto em um cluster do Service Fabric de dimensionamento de máquina virtual 
Desabilite a criptografia de disco para um conjunto, executando os seguintes comandos de dimensionamento de máquina virtual. Observe que desabilitar a criptografia de disco se aplica para o conjunto de dimensionamento de máquina virtual inteira e não uma instância individual.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Próximas etapas
Neste ponto, você deve ter um cluster seguro e saber como habilitar e desabilitar a criptografia de disco para nós de cluster do Service Fabric e conjuntos de dimensionamento de máquina virtual. Para obter diretrizes semelhantes em nós de cluster do Service Fabric no Linux, confira [Disk Encryption para Linux](service-fabric-enable-azure-disk-encryption-linux.md).

[customize-your-cluster-template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure#creating-a-custom-arm-template

---
title: Habilitar a criptografia de disco para clusters do Linux do Azure Service Fabric | Microsoft Docs
description: Este artigo descreve como habilitar a criptografia de disco para nós de cluster do Azure Service Fabric no Linux usando o Azure Resource Manager e o Azure Key Vault.
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
ms.openlocfilehash: 47b07188d1757708fb494c6a66e93379657e806a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66258773"
---
# <a name="enable-disk-encryption-for-azure-service-fabric-cluster-nodes-in-linux"></a>Habilitar a criptografia de disco para nós de cluster do Azure Service Fabric no Linux 
> [!div class="op_single_selector"]
> * [Criptografia de Disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Criptografia de Disco para Windows](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

Neste tutorial, você aprenderá a habilitar a criptografia de disco em nós de cluster do Azure Service Fabric no Linux. Você precisará seguir estas etapas para cada um dos tipos de nó e conjuntos de dimensionamento de máquina virtual. Para criptografar os nós, vamos usar a funcionalidade de criptografia de disco do Azure em conjuntos de dimensionamento de máquina virtual.

O guia abrange os seguintes tópicos:

* Principais conceitos a serem consideradas quando os conjuntos de habilitação de criptografia de disco em escala de máquina virtual de cluster do Service Fabric no Linux.
* As etapas a serem seguidas antes de habilitar a criptografia de disco no Service Fabric cluster nós do Linux.
* Etapas a serem seguidas para habilitar a criptografia de disco em nós de cluster do Service Fabric no Linux.



[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Pré-requisitos

 **Self-registration**

A visualização da criptografia de disco para o conjunto de dimensionamento de máquina virtual exige o registro de autoatendimento. Use as seguintes etapas:

1. Execute o comando a seguir: 
    ```powershell
    Register-AzProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
    ```
2. Aguarde 10 minutos até que lê o status *registrado*. Você pode verificar o status executando o seguinte comando:
    ```powershell
    Get-AzProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
    Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
    ```
**Cofre da Chave do Azure**

1. Crie um cofre de chaves na mesma assinatura e região como conjunto de dimensionamento. Em seguida, selecione a **EnabledForDiskEncryption** política no cofre de chaves de acesso usando o cmdlet do PowerShell. Você também pode definir a política usando a interface do usuário do Key Vault no portal do Azure com o seguinte comando:
    ```powershell
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
    ```
2. Instale a versão mais recente do [CLI do Azure](/cli/azure/install-azure-cli), que tem os novos comandos de criptografia.

3. Instale a versão mais recente do [SDK do Azure com o Azure PowerShell](https://github.com/Azure/azure-powershell/releases) de versão. A seguir está os cmdlets de Azure Disk Encryption para habilitar de conjunto de dimensionamento de máquinas virtuais ([definir](/powershell/module/az.compute/set-azvmssdiskencryptionextension)) a criptografia, recuperar ([obter](/powershell/module/az.compute/get-azvmssvmdiskencryption)) o status de criptografia e remover ([desabilitar](/powershell/module/az.compute/disable-azvmssdiskencryption)) instância do conjunto de criptografia na escala.


| Comando | Version |  source  |
| ------------- |-------------| ------------|
| Get-AzVmssDiskEncryptionStatus   | 1.0.0 ou posterior | Az.Compute |
| Get-AzVmssVMDiskEncryptionStatus   | 1.0.0 ou posterior | Az.Compute |
| Disable-AzVmssDiskEncryption   | 1.0.0 ou posterior | Az.Compute |
| Get-AzVmssDiskEncryption   | 1.0.0 ou posterior | Az.Compute |
| Get-AzVmssVMDiskEncryption   | 1.0.0 ou posterior | Az.Compute |
| Set-AzVmssDiskEncryptionExtension   | 1.0.0 ou posterior | Az.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Cenários com suporte para criptografia de disco
* Criptografia para conjuntos de dimensionamento de máquina virtual tem suporte apenas para conjuntos de dimensionamento criados com discos gerenciados. Não é suportado para conjuntos de escala de discos nativos (ou não gerenciados).
* Criptografia e a desabilitação da criptografia com suporte para volumes de sistema operacional e dados em conjuntos de dimensionamento de máquina virtual no Linux. 
* Operações de recriação de imagem e atualização de máquina virtual (VM) para conjuntos de dimensionamento de máquinas virtuais não têm suporte na visualização atual.


## <a name="create-a-new-cluster-and-enable-disk-encryption"></a>Criar um novo cluster e habilitar a criptografia de disco

Use os comandos a seguir para criar um cluster e habilitar a criptografia de disco usando um modelo do Azure Resource Manager e um certificado autoassinado.

### <a name="sign-in-to-azure"></a>Entrar no Azure  

Entrar com os seguintes comandos:

```powershell

Login-AzAccount
Set-AzContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

### <a name="use-the-custom-template-that-you-already-have"></a>Usar um modelo personalizado que você já tem 

Se você precisar criar um modelo personalizado, é altamente recomendável que você use um dos modelos na [exemplos de modelo de criação de cluster do Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) página. 

Se você já tiver um modelo personalizado, verifique novamente se todos os parâmetros relacionados ao certificado três no modelo e o arquivo de parâmetro são nomeados da seguinte maneira. Certifique-se também de que os valores são nulos da seguinte maneira:

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

Como criptografia de disco de dados apenas é suportada para conjuntos de dimensionamento de máquina virtual no Linux, você deve adicionar um disco de dados usando um modelo do Resource Manager. Atualize seu modelo para provisão de disco de dados da seguinte maneira:

```Json
   
   "storageProfile": { 
            "imageReference": { 
              "publisher": "[parameters('vmImagePublisher')]", 
              "offer": "[parameters('vmImageOffer')]", 
              "sku": "[parameters('vmImageSku')]", 
              "version": "[parameters('vmImageVersion')]" 
            }, 
            "osDisk": { 
              "caching": "ReadOnly", 
              "createOption": "FromImage", 
              "managedDisk": { 
                "storageAccountType": "[parameters('storageAccountType')]" 
              } 
           }, 
                "dataDisks": [ 
                { 
                    "diskSizeGB": 1023, 
                    "lun": 0, 
                    "createOption": "Empty" 
   
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

Aqui está o comando CLI equivalente. Altere os valores nas instruções declaradas para os valores apropriados. A CLI dá suporte a todos os outros parâmetros compatíveis com o comando anterior do PowerShell.

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

### <a name="mount-a-data-disk-to-a-linux-instance"></a>Montar um disco de dados para uma instância do Linux
Antes de continuar com a criptografia em um conjunto de dimensionamento de máquina virtual, verifique se que o disco de dados adicionados está montado corretamente. Entre VM de cluster do Linux e execute o **LSBLK** comando. A saída deve mostrar esse disco de dados adicionados na **ponto de montagem** coluna.


### <a name="deploy-application-to-a-service-fabric-cluster-in-linux"></a>Implantar o aplicativo em um cluster do Service Fabric no Linux
Para implantar um aplicativo em seu cluster, siga as etapas e as diretrizes em [guia de início rápido: Implantar contêineres do Linux no Service Fabric](service-fabric-quickstart-containers-linux.md).


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

### <a name="validate-if-disk-encryption-is-enabled-for-a-virtual-machine-scale-set-in-linux"></a>Validar se a criptografia de disco está habilitada para uma escala de máquina virtual definido no Linux
Para obter o status de um conjunto de dimensionamento de máquina virtual inteira ou em qualquer instância em um conjunto de dimensionamento, execute os comandos a seguir.
Além disso, você pode entrar para a VM de cluster do Linux e executar o **LSBLK** comando. A saída deve mostrar o disco de dados adicionados na **ponto de montagem** coluna e o **tipo** coluna deve ler *Crypt*.

```powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```azurecli
az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```

### <a name="disable-disk-encryption-for-a-virtual-machine-scale-set-in-a-service-fabric-cluster"></a>Desabilitar a criptografia de disco para um conjunto em um cluster do Service Fabric de dimensionamento de máquina virtual
Desabilite a criptografia de disco para um conjunto, executando os seguintes comandos de dimensionamento de máquina virtual. Observe que desabilitar a criptografia de disco se aplica para o conjunto de dimensionamento de máquina virtual inteira e não uma instância individual.

```powershell
$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```azurecli
az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Próximas etapas
Neste ponto, você deve ter um cluster seguro e saber como habilitar e desabilitar a criptografia de disco para nós de cluster do Service Fabric e conjuntos de dimensionamento de máquina virtual. Para obter diretrizes semelhantes em nós de cluster do Service Fabric no Linux, confira [criptografia de disco para o Windows](service-fabric-enable-azure-disk-encryption-windows.md). 

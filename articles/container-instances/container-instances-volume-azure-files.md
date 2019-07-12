---
title: Monte um volume de Arquivos do Azure em Instâncias de Contêiner do Azure
description: Saiba como montar um volume de Arquivos do Azure para persistir o estado com Instâncias de Contêiner do Azure
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 07/08/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: bc09aa500743d608c0a3a7a379fe9584c9c55e9b
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657636"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Montar um compartilhamento de arquivos do Azure em Instâncias de Contêiner do Azure

Por padrão, as Instâncias de Contêiner do Azure são sem monitoração de estado. Se o contêiner parar ou falhar, todo o seu estado será perdido. Para persistir o estado além do tempo de vida do contêiner, você deve montar um volume de um repositório externo. Este artigo mostra como montar um compartilhamento de arquivos do Azure com [Arquivos do Azure](../storage/files/storage-files-introduction.md) para ser usado com Instâncias de Contêiner do Azure. Os arquivos do Azure oferecem compartilhamentos de arquivos totalmente gerenciados na nuvem que são acessíveis por meio do protocolo SMB padrão do setor. Usar um compartilhamento de arquivos do Azure com Instâncias de Contêiner do Azure fornece recursos de compartilhamento de arquivos semelhantes ao uso de um compartilhamento de arquivos do Azure com máquinas virtuais do Azure.

> [!NOTE]
> Montar um Compartilhamento de Arquivos do Azure está atualmente restrito a contêineres do Linux. Enquanto estamos trabalhando para trazer todos os recursos para contêineres do Windows, você pode encontrar diferenças da plataforma atual na [visão geral](container-instances-overview.md#linux-and-windows-containers).

## <a name="create-an-azure-file-share"></a>Criar um compartilhamento de arquivos do Azure

Antes de usar um compartilhamento de arquivos do Azure com Instâncias de Contêiner do Azure, você deve criá-lo. Execute o seguinte script para criar uma conta de armazenamento para hospedar o compartilhamento de arquivos e o próprio compartilhamento. O nome da conta de armazenamento deve ser globalmente exclusivo para que o script adicione um valor aleatório à cadeia de caracteres de base.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Create the file share
az storage share create --name $ACI_PERS_SHARE_NAME --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME
```

## <a name="get-storage-credentials"></a>Obter credenciais de armazenamento

Para montar um compartilhamento de arquivos do Azure como um volume nas Instâncias de Contêiner do Azure, você precisa de três valores: o nome da conta de armazenamento, o nome do compartilhamento e a chave de acesso de armazenamento.

Se você tiver usado o script acima, o nome da conta de armazenamento foi armazenado na variável $ACI_PERS_STORAGE_ACCOUNT_NAME. Para ver o nome da conta, digite:

```console
echo $ACI_PERS_STORAGE_ACCOUNT_NAME
```

O nome do compartilhamento já é conhecido (definido como *acishare* no script acima), portanto, falta apenas a chave da conta de armazenamento, que pode ser encontrada usando o seguinte comando:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume---cli"></a>Implantar o contêiner e montar o volume - CLI

Para montar um compartilhamento de arquivos do Azure como um volume em um contêiner usando a CLI do Azure, especifique o ponto de montagem de volume e compartilhamento quando você cria o contêiner com [criar contêiner az][az-container-create]. Se você seguiu as etapas anteriores, será possível montar o compartilhamento já criado usando o comando a seguir para criar um contêiner:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image mcr.microsoft.com/azuredocs/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

O `--dns-name-label` valor deve ser exclusivo dentro da região do Azure em que você cria a instância de contêiner. Atualize o valor no comando anterior se você receber uma mensagem de erro do **rótulo do nome DNS** ao executar o comando.

## <a name="manage-files-in-mounted-volume"></a>Gerenciar arquivos no volume montado

Depois que o contêiner for iniciado, você pode usar o aplicativo web simples implantado por meio da Microsoft [aci-hellofiles][aci-hellofiles] image to create small text files in the Azure file share at the mount path you specified. Obtain the web app's fully qualified domain name (FQDN) with the [az container show][az-container-show] comando:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --query ipAddress.fqdn --output tsv
```

Depois de salvar o texto usando o aplicativo, você pode usar o [portal do Azure][portal] or a tool like the [Microsoft Azure Storage Explorer][storage-explorer] para recuperar e inspecionar o arquivo gravado no compartilhamento de arquivo.

## <a name="deploy-container-and-mount-volume---yaml"></a>Implantar o contêiner e montar o volume - YAML

Você também pode implantar um grupo de contêiner e montar um volume em um contêiner com a CLI do Azure e um [modelo YAML](container-instances-multi-container-yaml.md). A implantação pelo modelo YAML é o método preferencial ao implantar grupos de contêineres que consistem em vários contêineres.

O modelo YAML a seguir define um grupo de contêineres com um contêiner criado com o `aci-hellofiles` imagem. O contêiner monta o compartilhamento de arquivos do Azure *acishare* criado anteriormente como um volume. No local indicado, insira a chave de armazenamento e o nome da conta de armazenamento que hospeda o compartilhamento de arquivos. 

Como no exemplo da CLI, o `dnsNameLabel` valor deve ser exclusivo dentro da região do Azure em que você cria a instância de contêiner. Atualize o valor no arquivo YAML, se necessário.

```yaml
apiVersion: '2018-10-01'
location: eastus
name: file-share-demo
properties:
  containers:
  - name: hellofiles
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-hellofiles
      ports:
      - port: 80
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /aci/logs/
        name: filesharevolume
  osType: Linux
  restartPolicy: Always
  ipAddress:
    type: Public
    ports:
      - port: 80
    dnsNameLabel: aci-demo
  volumes:
  - name: filesharevolume
    azureFile:
      sharename: acishare
      storageAccountName: <Storage account name>
      storageAccountKey: <Storage account key>
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

Para implantar com o modelo YAML, salve o YAML anterior em um arquivo chamado `deploy-aci.yaml`, em seguida, execute o [criar contêiner az][az-container-create] com o `--file` parâmetro:

```azurecli
# Deploy with YAML template
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```
## <a name="deploy-container-and-mount-volume---resource-manager"></a>Implantar o contêiner e montar o volume - Gerenciador de recursos

Além da implantação da CLI e YAML, você pode implantar um grupo de contêiner e montar um volume em um contêiner usando o Azure [modelo do Resource Manager](/azure/templates/microsoft.containerinstance/containergroups).

Primeiro, popule a matriz `volumes` na seção `properties` do grupo de contêineres do modelo. 

Em seguida, para cada contêiner no qual você deseja montar o volume, preencher a `volumeMounts` Pole o `properties` seção da definição de contêiner.

O modelo do Resource Manager a seguir define um grupo de contêineres com um contêiner criado com o `aci-hellofiles` imagem. O contêiner monta o compartilhamento de arquivos do Azure *acishare* criado anteriormente como um volume. No local indicado, insira a chave de armazenamento e o nome da conta de armazenamento que hospeda o compartilhamento de arquivos. 

Nos exemplos anteriores, o `dnsNameLabel` valor deve ser exclusivo dentro da região do Azure em que você cria a instância de contêiner. Atualize o valor no modelo, se necessário.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "variables": {
    "container1name": "hellofiles",
    "container1image": "mcr.microsoft.com/azuredocs/aci-hellofiles"
  },
  "resources": [
    {
      "name": "file-share-demo",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                }
              ],
              "volumeMounts": [
                {
                  "name": "filesharevolume",
                  "mountPath": "/aci/logs"
                }
              ]
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            }
          ],
          "dnsNameLabel": "aci-demo"
        },
        "volumes": [
          {
            "name": "filesharevolume",
            "azureFile": {
                "shareName": "acishare",
                "storageAccountName": "<Storage account name>",
                "storageAccountKey": "<Storage account key>"
            }
          }
        ]
      }
    }
  ]
}
```

Para implantar o modelo do Resource Manager, salve o JSON anterior em um arquivo chamado `deploy-aci.json`, em seguida, execute o [criar implantação de grupo az][az-group-deployment-create] com o `--template-file` parâmetro:

```azurecli
# Deploy with Resource Manager template
az group deployment create --resource-group myResourceGroup --template-file deploy-aci.json
```


## <a name="mount-multiple-volumes"></a>Montar vários volumes

Para montar vários volumes em uma instância de contêiner, você deve implantar usando um [modelo do Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups) ou um arquivo YAML. Para usar um modelo ou um arquivo YAML, forneça os detalhes de compartilhamento e defina os volumes populando a `volumes` Pole o `properties` seção do modelo. 

Por exemplo, se você criou dois compartilhamentos de arquivos do Azure denominados *share1* e *share2* na conta de armazenamento *myStorageAccount*, o `volumes` matriz em um Gerenciador de recursos modelo deve aparecer semelhante ao seguinte:

```JSON
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Em seguida, para cada contêiner do grupo de contêineres no qual você deseja montar os volumes, popule a matriz `volumeMounts` na seção `properties` da definição de contêiner. Por exemplo, isso monta os dois volumes, *myvolume1* e *myvolume2*, definidos anteriormente:

```JSON
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

## <a name="next-steps"></a>Próximas etapas

Saiba como montar outros tipos de volume em Instâncias de Contêiner do Azure:

* [Montar um volume emptyDir em Instâncias de Contêiner do Azure](container-instances-volume-emptydir.md)
* [Montar um volume gitRepo em Instâncias de Contêiner do Azure](container-instances-volume-gitrepo.md)
* [Montar um volume secreto em Instâncias de Contêiner do Azure](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/_/microsoft-azuredocs-aci-hellofiles 
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
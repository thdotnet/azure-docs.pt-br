---
title: Usar uma identidade gerenciada com as tarefas de registro de contêiner do Azure
description: Fornece um acesso de tarefa de registro de contêiner do Azure para recursos do Azure, incluindo outros registros de contêiner privado, atribuindo uma identidade gerenciada para recursos do Azure.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 5b60727472a06aaac8ccd3dce8609461e8972311
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148038"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>Usar Azure managed identity no ACR tarefas 

Use uma [identidade de recursos do Azure gerenciado](../active-directory/managed-identities-azure-resources/overview.md) autenticar de tarefas de ACR a um registro de contêiner do Azure ou outros recursos do Azure, sem necessidade de fornecer ou gerenciar as credenciais no código. Por exemplo, use uma identidade gerenciada para efetuar pull ou push de imagens de contêiner para outro registro como uma etapa em uma tarefa.

Neste artigo, você aprenderá mais sobre identidades gerenciadas e como:

> [!div class="checklist"]
> * Habilitar uma identidade atribuída pelo sistema ou uma identidade atribuída pelo usuário em uma tarefa ACR
> * Conceder o acesso de identidade para recursos do Azure, como outros registros de contêiner do Azure
> * Use a identidade gerenciada para acessar os recursos de uma tarefa 

Para criar os recursos do Azure, este artigo exigirá que você execute a CLI do Azure versão 2.0.66 ou posterior. Execute `az --version` para encontrar a versão. Se precisar instalar ou atualizar, consulte [Instalar a CLI do Azure][azure-cli].

## <a name="why-use-a-managed-identity"></a>Por que usar uma identidade gerenciada?

Uma identidade gerenciada dos recursos do Azure fornece serviços do Azure com uma identidade gerenciada automaticamente no Azure Active Directory (Azure AD). Você pode configurar [alguns recursos do Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), incluindo tarefas de ACR, com uma identidade gerenciada. Em seguida, use a identidade para acessar outros recursos do Azure, sem passar credenciais no código ou scripts.

Identidades gerenciadas são de dois tipos:

* *Identidades atribuídas pelo usuário*, que você pode atribuir a vários recursos e persistir durante o tempo que desejar. As identidades atribuídas pelo usuário estão atualmente em pré-visualização.

* Um *gerenciados pelo sistema de identidade*, que é exclusivo a um recurso específico, como uma tarefa ACR e dura pelo tempo de vida do recurso.

Depois de configurar um recurso do Azure com uma identidade gerenciada, conceda o acesso de identidade a outro recurso, assim como qualquer entidade de segurança. Por exemplo, atribua uma identidade gerenciada uma função com pull, push e pull ou outras permissões para um registro de contêiner privado no Azure. (Para obter uma lista completa de funções de registro, confira [Funções e permissões do Registro de Contêiner do Azure](container-registry-roles.md).) Você pode conceder à identidade acesso a um ou mais recursos.

## <a name="create-container-registries"></a>Criar registros de contêiner

Para este tutorial, você precisa três registros de contêiner:

* Você pode usar o registro primeiro para criar e executar tarefas ACR. Neste artigo, esse registro de fonte é denominado *myregistry*. 
* Os segundo e terceiro registros são registros de destino para a primeira tarefa de exemplo enviar por push uma imagem que ele se baseia. Neste artigo, os registros de destino são nomeados *customregistry1* e *customregistry2*.

Substitua seus próprios nomes de registro em etapas posteriores.

Se você ainda não tiver os registros de contêiner do Azure necessários, consulte [guia de início rápido: Criar um registro de contêiner privado usando a CLI do Azure](container-registry-get-started-azure-cli.md). Você não precisa enviar imagens por push ao registro ainda.

## <a name="example-task-with-a-system-assigned-identity"></a>Exemplo: Tarefa com uma identidade atribuída pelo sistema

Este exemplo mostra como criar uma [tarefa de várias etapas](container-registry-tasks-multi-step.md) com uma identidade atribuída pelo sistema. A tarefa cria uma imagem e, em seguida, usa a identidade para autenticar com dois registros de destino para enviar a imagem.

As etapas para essa tarefa de exemplo são definidas em uma [arquivo YAML](container-registry-tasks-reference-yaml.md) denominado `testtask.yaml`. O arquivo está localizado no diretório do multipleRegistries a [acr tarefas](https://github.com/Azure-Samples/acr-tasks) amostras de repositório. O arquivo está reproduzido aqui:

```yml
version: v1.0.0
steps:
  - build: -t {{.Values.REGISTRY1}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY1}}/hello-world:{{.Run.ID}}"]
  - build: -t {{.Values.REGISTRY2}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY2}}/hello-world:{{.Run.ID}}"]
```

### <a name="create-task-with-system-assigned-identity"></a>Criar a tarefa com identidade atribuída pelo sistema

Criar a tarefa *vários reg* executando o seguinte [criar tarefa de acr az] [ az-acr-task-create] comando. O contexto de tarefa é a pasta multipleRegistries do repositório de exemplos, e o comando faz referência ao arquivo `testtask.yaml` no repositório. O `--assign-identity` parâmetro sem valor adicional cria uma identidade atribuída pelo sistema para a tarefa. Essa tarefa é configurada para que você precise dispará-lo manualmente, mas você pode configurá-lo para executar quando confirmações são enviadas por push para o repositório ou é feita uma solicitação de pull. 

```azurecli
az acr task create \
  --registry myregistry \
  --name multiple-reg \
  --context https://github.com/Azure-Samples/acr-tasks.git#:multipleRegistries \
  --file testtask.yaml \
  --commit-trigger-enabled false \
  --pull-request-trigger-enabled false \
  --assign-identity
```

Na saída do comando, o `identity` seção mostra uma identidade de tipo `SystemAssigned` é definido na tarefa. O `principalId` é a ID da entidade da identidade do serviço:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 

Use o [show do az acr tarefa] [ az-acr-task-show] comando para armazenar o `principalId` em uma variável, deve ser usada em comandos posteriores:

```azurecli
principalID=$(az acr task show --name multiple-reg --registry myregistry --query identity.principalId --output tsv)
```

### <a name="give-identity-push-permissions-to-two-target-container-registries"></a>Conceder permissões de envio por push de identidade para dois registros de contêiner de destino

Nesta seção, conceda as permissões de identidade atribuída pelo sistema para enviar por push para os registros de destino de duas, denominados *customregistry1* e *customregistry2*.

Primeiro use o [show do az acr] [ az-acr-show] comando para obter a ID do recurso de cada registro e armazenar as IDs em variáveis:

```azurecli
reg1_id=$(az acr show --name customregistry1 --query id --output tsv)
reg2_id=$(az acr show --name customregistry2 --query id --output tsv)
```

Use o [criar atribuição de função az] [ az-role-assignment-create] comando para atribuir a identidade a `acrpush` função para cada registro. Essa função tem permissões para as imagens de pull e push para um registro de contêiner.

```azurecli
az role assignment create --assignee $principalID --scope $reg1_id --role acrpush
az role assignment create --assignee $principalID --scope $reg2_id --role acrpush
```

### <a name="add-target-registry-credentials-to-task"></a>Adicionar credenciais de registro de destino para a tarefa

Agora use o [adicionar credencial de tarefa de acr az] [ az-acr-task-credential-add] comando para adicionar credenciais da identidade para a tarefa para que ele possa se autenticar com os dois registros de destino.

```azurecli
az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry1.azurecr.io \
  --use-identity [system]

az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry2.azurecr.io \
  --use-identity [system]
```

### <a name="manually-run-the-task"></a>Executar a tarefa manualmente

Use o [tarefa de acr az] [ az-acr-task-run] comando para disparar manualmente a tarefa. O `--set` parâmetro é usado para passar os nomes de servidor de logon dos registros de duas destino como valores para as variáveis de tarefa `REGISTRY1` e `REGISTRY2`.

```azurecli
az acr task run \
  --name multiple-reg \
  --registry myregistry \
  --set REGISTRY1=customregistry1.azurecr.io \
  --set REGISTRY2=customregistry2.azurecr.io
```

A saída é semelhante a:

```console
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:02 Successfully executed container: acb_step_0
2019/05/31 22:16:02 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:02 Pushing image: customregistry2.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry2.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:08 Successfully pushed image: customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:08 Executing step ID: acb_step_2. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:08 Scanning for dependencies...
2019/05/31 22:16:08 Successfully scanned dependencies
2019/05/31 22:16:08 Launching container with name: acb_step_2
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:09 Successfully executed container: acb_step_2
2019/05/31 22:16:09 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:09 Pushing image: customregistry1.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry1.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:21 Successfully pushed image: customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:21 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.985291)
2019/05/31 22:16:21 Populating digests for step ID: acb_step_0...
2019/05/31 22:16:22 Successfully populated digests for step ID: acb_step_0
2019/05/31 22:16:22 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 5.743225)
2019/05/31 22:16:22 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 1.925959)
2019/05/31 22:16:22 Populating digests for step ID: acb_step_2...
2019/05/31 22:16:24 Successfully populated digests for step ID: acb_step_2
2019/05/31 22:16:24 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 11.061057)
2019/05/31 22:16:24 The following dependencies were found:
2019/05/31 22:16:24
- image:
    registry: customregistry2.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b
- image:
    registry: customregistry1.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b

Run ID: cf31 was successful after 35s
```

## <a name="example-task-with-a-user-assigned-identity"></a>Exemplo: Tarefa com uma identidade atribuída pelo usuário

Neste exemplo, você cria uma identidade atribuída pelo usuário com permissões para ler segredos do Azure key vault. Você pode atribuir essa identidade para uma tarefa de várias etapas que lê o segredo, cria uma imagem e entra no CLI do Azure para ler a marca de imagem.

### <a name="create-a-key-vault-and-store-a-secret"></a>Criar um cofre de chaves e armazenar um segredo

Primeiro, se necessário, crie um grupo de recursos denominado *myResourceGroup* na *eastus* local com o seguinte [criar grupo de az] [ az-group-create]comando:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Use o [az creata] [ az-keyvault-create] comando para criar um cofre de chaves. Certifique-se de especificar um nome exclusivo de Cofre de chaves. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Store um segredo de exemplo no cofre de chaves usando o [conjunto secreto do az keyvault] [ az-keyvault-secret-set] comando:

```azurecli
az keyvault secret set \
  --name SampleSecret \
  --value "Hello ACR Tasks!" \
  --description ACRTasksecret  \
  --vault-name mykeyvault
```

Por exemplo, você talvez queira armazenar credenciais para autenticar com um registro privado do Docker, portanto, é possível efetuar pull de uma imagem privada.

### <a name="create-an-identity"></a>Criar uma identidade

Criar uma identidade chamada *myACRTasksId* em sua assinatura usando o [criar identidade az] [ az-identity-create] comando. Você pode usar o mesmo grupo de recursos que você usou anteriormente para criar um registro de contêiner ou Cofre de chaves ou um diferente.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Para configurar a identidade nas etapas a seguir, use o comando [az identity show][az-identity-show] para armazenar a ID do recurso da entidade e a ID da entidade de serviço nas variáveis.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)
```

### <a name="grant-identity-access-to-keyvault-to-read-secret"></a>Conceder acesso de identidade para ler um segredo no cofre de chaves

Execute o seguinte [az keyvault set-policy] [ az-keyvault-set-policy] comando para definir uma política de acesso no cofre de chaves. O exemplo a seguir permite que a identidade atribuída pelo usuário obter os segredos do Cofre de chaves. Esse acesso é necessário posteriormente para executar uma tarefa de várias etapa com êxito.

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $principalID --secret-permissions get
```

### <a name="grant-identity-reader-access-to-the-resource-group-for-registry"></a>Conceder acesso de leitor de identidade para o grupo de recursos para registro

Execute o seguinte [criar atribuição de função az] [ az-role-assignment-create] comando para atribuir a identidade de uma função de leitor, nesse caso, para o grupo de recursos que contém o registro de origem. Essa função é necessário posteriormente para executar uma tarefa de várias etapa com êxito.

```azurecli
az role assignment create --role reader --resource-group myResourceGroup --assignee $principalID
```

### <a name="create-task-with-user-assigned-identity"></a>Criar tarefa com identidade atribuída pelo usuário

Agora, crie uma [tarefa de várias etapas](container-registry-tasks-multi-step.md) e atribuí-lo a identidade atribuída pelo usuário. Para esta tarefa de exemplo, criar uma [arquivo YAML](container-registry-tasks-reference-yaml.md) denominado `managed-identities.yaml` em um diretório de trabalho local e cole o seguinte conteúdo. Certifique-se de substituir o nome do Cofre de chaves no arquivo com o nome do seu Cofre de chaves

```yml
version: v1.0.0
# Replace mykeyvault with the name of your key vault
secrets:
  - id: name
    keyvault: https://mykeyvault.vault.azure.net/secrets/SampleSecret
steps:
  # Verify that the task can access the secret in the key vault
  - cmd: bash -c 'if [ -z "$MY_SECRET" ]; then echo "Secret not resolved"; else echo "Secret resolved"; fi'
    env: 
      - MY_SECRET='{{.Secrets.name}}' 

  # Build/push the website image to source registry, using dockerfile in the Azure-Samples repo
  - cmd: docker build -t {{.Run.Registry}}/my-website:{{.Run.ID}} https://github.com/Azure-Samples/aci-helloworld.git
  - push: 
    - "{{.Run.Registry}}/my-website:{{.Run.ID}}"
  
  # Login to Azure CLI with identity and list the tags to verify that the image is in the registry
  - cmd: microsoft/azure-cli az login --identity
  - cmd: microsoft/azure-cli az acr repository show-tags -n {{.Values.registryName}} --repository my-website
```

Essa tarefa faz o seguinte:

* Verifica se ele pode acessar o segredo no cofre de chaves. Esta etapa é para fins de demonstração. Em um cenário do mundo real, talvez seja necessário uma etapa de tarefas para obter credenciais para acessar um repositório de Hub do Docker privado.
* Compila e envia por push o `mywebsite` imagem no registro de origem.
* Os logs para a CLI do Azure à lista de `my-website` marcas no registro de fonte de imagem.

Criar uma tarefa chamada *msitask* e passá-lo a ID de recurso da identidade atribuída pelo usuário criado anteriormente. Essa tarefa de exemplo é criada a partir de `managed-identities.yaml` arquivo salvo no diretório de trabalho local, para que você precise dispará-lo manualmente.

```azurecli
az acr task create \
  --name msitask \
  --registry myregistry \
  --context /dev/null \
  --file managed-identities.yaml \
  --pull-request-trigger-enabled false \
  --commit-trigger-enabled false \
  --assign-identity $resourceID
```

Na saída do comando, o `identity` seção mostra uma identidade de tipo `UserAssigned` é definido na tarefa. O `principalId` é a ID da entidade da identidade do serviço:

```console
[...]
"identity": {
    "principalId": null,
    "tenantId": null,
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRTasksId": {
        "clientId": "xxxxxxxx-f17e-4768-bb4e-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-1335-433d-bb6c-xxxxxxxxxxxx"
      }
[...]
```

### <a name="manually-run-the-task"></a>Executar a tarefa manualmente

Use o [tarefa de acr az] [ az-acr-task-run] comando para disparar manualmente a tarefa. O `--set` parâmetro é usado para transmitir o nome do registro de origem para a tarefa:

```azurecli
az acr task run --name msitask --registry myregistry --set registryName=myregistry  
```

A saída mostra que o segredo for resolvido, a imagem com êxito é criada e enviada por push e a tarefa registra em log para a CLI do Azure com a identidade para ler a marca de imagem do registro de origem:

```console
Queued a run with ID: cf32
Waiting for an agent...
2019/06/10 23:25:37 Downloading source code...
2019/06/10 23:25:38 Finished downloading source code
2019/06/10 23:25:39 Using acb_vol_4e4a0a7c-b6ef-4ec5-b40f-3436fc5eb0f5 as the home volume
2019/06/10 23:25:41 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/10 23:25:41 Successfully set up Docker network: acb_default_network
2019/06/10 23:25:41 Setting up Docker configuration...
2019/06/10 23:25:42 Successfully set up Docker configuration
2019/06/10 23:25:42 Logging in to registry: myregistry.azurecr.io
2019/06/10 23:25:43 Successfully logged into myregistry.azurecr.io
2019/06/10 23:25:43 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:43 Launching container with name: acb_step_0
Secret resolved
2019/06/10 23:25:44 Successfully executed container: acb_step_0
2019/06/10 23:25:44 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:44 Launching container with name: acb_step_1
Sending build context to Docker daemon  74.75kB

[...]

cf32: digest: sha256:cbb4aa83b33f6959d83e84bfd43ca901084966a9f91c42f111766473dc977f36 size: 1577
2019/06/10 23:26:05 Successfully pushed image: myregistry.azurecr.io/my-website:cf32
2019/06/10 23:26:05 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:05 Launching container with name: acb_step_3
[
  {
    "environmentName": "AzureCloud",
    "id": "xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx",
    "isDefault": true,
    "name": "DanLep Internal Consumption",
    "state": "Enabled",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
      "user": {
      "assignedIdentityInfo": "MSI",
      "name": "systemAssignedIdentity",
      "type": "servicePrincipal"
    }
  }
]
2019/06/10 23:26:09 Successfully executed container: acb_step_3
2019/06/10 23:26:09 Executing step ID: acb_step_4. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:09 Launching container with name: acb_step_4
[
  "cf32"
]
2019/06/10 23:26:14 Successfully executed container: acb_step_4
2019/06/10 23:26:14 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.025312)
2019/06/10 23:26:14 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 13.703823)
2019/06/10 23:26:14 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 6.791506)
2019/06/10 23:26:14 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 3.852972)
2019/06/10 23:26:14 Step ID: acb_step_4 marked as successful (elapsed time in seconds: 5.079079)
```


## <a name="next-steps"></a>Próximas etapas

Neste artigo, você aprendeu sobre como usar identidades gerenciadas com tarefas de registro de contêiner do Azure e como:

> [!div class="checklist"]
> * Habilitar uma identidade atribuída pelo sistema ou usuário atribuído em uma tarefa ACR
> * Conceder o acesso de identidade para recursos do Azure, como outros registros de contêiner do Azure
> * Use a identidade gerenciada para acessar os recursos de uma tarefa  

* Aprenda sobre [identidades gerenciadas dos recursos do Azure](/azure/active-directory/managed-identities-azure-resources/).

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-create]: /cli/azure/identity?view=azure-cli-latest#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[az-group-create]: /cli/azure/group?#az-group-create
[az-keyvault-create]: /cli/azure/keyvault?#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy

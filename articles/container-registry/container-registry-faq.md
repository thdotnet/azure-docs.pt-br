---
title: Registro de contêiner do Azure - perguntas frequentes
description: Respostas para perguntas frequentes relacionadas ao serviço de registro de contêiner do Azure
services: container-registry
author: sajayantony
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 5/13/2019
ms.author: sajaya
ms.openlocfilehash: beeb4986750e398071e3afb6c1f04663f858cec1
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303567"
---
# <a name="frequently-asked-questions-about-azure-container-registry"></a>Perguntas frequentes sobre o registro de contêiner do Azure

Este artigo aborda perguntas frequentes e problemas conhecidos sobre o registro de contêiner do Azure.

## <a name="resource-management"></a>Gerenciamento de recursos

- [Posso criar um registro de contêiner do Azure usando um modelo do Resource Manager?](#can-i-create-an-azure-container-registry-using-a-resource-manager-template)
- [Há verificação para as imagens no ACR de vulnerabilidades de segurança?](#is-there-security-vulnerability-scanning-for-images-in-acr)
- [Como configurar o Kubernetes com o registro de contêiner do Azure?](#how-do-i-configure-kubernetes-with-azure-container-registry)
- [Como posso obter credenciais de administrador para um registro de contêiner?](#how-do-i-get-admin-credentials-for-a-container-registry)
- [Como obter credenciais de administrador em um modelo do Resource Manager?](#how-do-i-get-admin-credentials-in-a-resource-manager-template)
- [Exclusão de replicação falhará com o status de proibido, embora a replicação é excluída usando a CLI do Azure ou Azure PowerShell](#delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell)

### <a name="can-i-create-an-azure-container-registry-using-a-resource-manager-template"></a>Posso criar um registro de contêiner do Azure usando um modelo do Resource Manager?

Sim. Eis [um modelo](https://github.com/Azure/azure-cli/blob/master/src/command_modules/azure-cli-acr/azure/cli/command_modules/acr/template.json) que você pode usar para criar um registro.

### <a name="is-there-security-vulnerability-scanning-for-images-in-acr"></a>Há verificação para as imagens no ACR de vulnerabilidades de segurança?

Sim. Consulte a documentação do [Twistlock](https://www.twistlock.com/2016/11/07/twistlock-supports-azure-container-registry/) e [azul-piscina](http://blog.aquasec.com/image-vulnerability-scanning-in-azure-container-registry).

### <a name="how-do-i-configure-kubernetes-with-azure-container-registry"></a>Como configurar o Kubernetes com o registro de contêiner do Azure?

Consulte a documentação para [Kubernetes](http://kubernetes.io/docs/user-guide/images/#using-azure-container-registry-acr) e as etapas para [serviço Kubernetes do Azure](container-registry-auth-aks.md).

### <a name="how-do-i-get-admin-credentials-for-a-container-registry"></a>Como posso obter credenciais de administrador para um registro de contêiner?

> [!IMPORTANT]
> A conta de usuário do administrador destina-se um único usuário acessar o registro, principalmente para fins de teste. Não é recomendável compartilhar as credenciais da conta do administrador com vários usuários. Para cenários remotos, recomenda-se identidade individual para usuários e entidades de serviço. Ver [visão geral da autenticação](container-registry-authentication.md).

Antes de obter credenciais de administrador, verifique se o que usuário do administrador do registro está habilitado.

Para obter as credenciais usando a CLI do Azure:

```azurecli
az acr credential show -n myRegistry
```

Usando o Powershell do Azure:

```powershell
Invoke-AzureRmResourceAction -Action listCredentials -ResourceType Microsoft.ContainerRegistry/registries -ResourceGroupName myResourceGroup -ResourceName myRegistry
```

### <a name="how-do-i-get-admin-credentials-in-a-resource-manager-template"></a>Como obter credenciais de administrador em um modelo do Resource Manager?

> [!IMPORTANT]
> A conta de usuário do administrador destina-se um único usuário acessar o registro, principalmente para fins de teste. Não é recomendável compartilhar as credenciais da conta do administrador com vários usuários. Para cenários remotos, recomenda-se identidade individual para usuários e entidades de serviço. Ver [visão geral da autenticação](container-registry-authentication.md).

Antes de obter credenciais de administrador, verifique se o que usuário do administrador do registro está habilitado.

Para obter a senha primeiro:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[0].value]"
}
```

Para obter a segunda senha:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[1].value]"
}
```

### <a name="delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell"></a>Exclusão de replicação falhará com o status de proibido, embora a replicação é excluída usando a CLI do Azure ou Azure PowerShell

O erro é visto quando o usuário tem permissões em um registro, mas não tem permissões no nível de leitor na assinatura. Para resolver esse problema, atribua permissões de leitor na assinatura para o usuário:


```azurecli  
az role assignment create --role "Reader" --assignee user@contoso.com --scope /subscriptions/<subscription_id> 
```

## <a name="registry-operations"></a>Operações de registro

- [Como acessar o Docker Registry V2 de API HTTP?](#how-do-i-access-docker-registry-http-api-v2)
- [Como faço para excluir todos os manifestos não são referenciados por qualquer marca em um repositório?](#how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository)
- [Por que não o uso da cota de registro reduz após a exclusão de imagens?](#why-does-the-registry-quota-usage-not-reduce-after-deleting-images)
- [Como validar alterações de cota de armazenamento?](#how-do-i-validate-storage-quota-changes)
- [Como autenticar com meu registro ao executar a CLI em um contêiner?](#how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container)
- [O registro de contêiner do Azure oferece configuração somente do TLS v 1.2 e como habilitar o TLS v1.2?](#does-azure-container-registry-offer-tls-v12-only-configuration-and-how-to-enable-tls-v12)
- [Registro de contêiner do Azure dá suporte a confiar em conteúdo?](#does-azure-container-registry-support-content-trust)
- [Como faço para dar acesso a imagens de pull ou push sem permissão para gerenciar o recurso de registro?](#how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource)
- [Como habilitar a quarentena automáticas de imagem para um registro](#how-do-i-enable-automatic-image-quarantine-for-a-registry)

### <a name="how-do-i-access-docker-registry-http-api-v2"></a>Como acessar o Docker Registry V2 de API HTTP?

ACR dá suporte a Docker Registry V2 de API HTTP. As APIs podem ser acessadas em `https://<your registry login server>/v2/`. Exemplo: `https://mycontainerregistry.azurecr.io/v2/`

### <a name="how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository"></a>Como faço para excluir todos os manifestos não são referenciados por qualquer marca em um repositório?

Se você estiver usando bash:

```bash
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv  | xargs -I% az acr repository delete -n myRegistry -t myRepository@%
```

Para o Powershell:

```powershell
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv | %{ az acr repository delete -n myRegistry -t myRepository@$_ }
```

Observação: Você pode adicionar `-y` no comando delete para ignorar a confirmação.

Para obter mais informações, consulte [excluir imagens de contêiner no registro de contêiner do Azure](container-registry-delete.md).

### <a name="why-does-the-registry-quota-usage-not-reduce-after-deleting-images"></a>Por que não o uso da cota de registro reduz após a exclusão de imagens?

Essa situação pode ocorrer se as camadas subjacentes ainda estão sendo referenciadas por outras imagens de contêiner. Se você excluir uma imagem sem referências, o uso do registro de atualizações em poucos minutos.

### <a name="how-do-i-validate-storage-quota-changes"></a>Como validar alterações de cota de armazenamento?

Crie uma imagem com uma camada de 1GB usando o seguinte arquivo docker. Isso garante que a imagem tem uma camada que não é compartilhada por qualquer outra imagem no registro.

```dockerfile
FROM alpine
RUN dd if=/dev/urandom of=1GB.bin  bs=32M  count=32
RUN ls -lh 1GB.bin
```

Criar e enviar a imagem para seu registro usando a CLI do docker.

```bash
docker build -t myregistry.azurecr.io/1gb:latest .
docker push myregistry.azurecr.io/1gb:latest
```

Você deve ser capaz de ver que aumentou a utilização de armazenamento no portal do Azure, ou você pode consultar usando a CLI de uso.

```bash
az acr show-usage -n myregistry
```

Excluir a imagem usando o portal ou a CLI do Azure e verificar o uso de atualizada em poucos minutos.

```bash
az acr repository delete -n myregistry --image 1gb
```

### <a name="how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container"></a>Como autenticar com meu registro ao executar a CLI em um contêiner?

Você precisa executar o contêiner de CLI do Azure montando o soquete Docker:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock azuresdk/azure-cli-python:dev
```

O contêiner, instala `docker`:

```bash
apk --update add docker
```

Em seguida, autenticar com o registro:

```azurecli
az acr login -n MyRegistry
```

### <a name="does-azure-container-registry-offer-tls-v12-only-configuration-and-how-to-enable-tls-v12"></a>O registro de contêiner do Azure oferece configuração somente do TLS v 1.2 e como habilitar o TLS v1.2?

Sim. Habilitar o TLS usando qualquer cliente docker recente (versão 18.03.0 e acima). 

### <a name="does-azure-container-registry-support-content-trust"></a>Registro de contêiner do Azure dá suporte a confiar em conteúdo?

Sim, você pode usar imagens confiáveis no registro de contêiner do Azure, desde o [Docker cartório](https://docs.docker.com/notary/getting_started/) foi integrado e pode ser habilitado. Para obter detalhes, consulte [confiar em conteúdo no registro de contêiner do Azure](container-registry-content-trust.md).


####  <a name="where-is-the-file-for-the-thumbprint-located"></a>Onde está o arquivo para a impressão digital localizada?

Em `~/.docker/trust/tuf/myregistry.azurecr.io/myrepository/metadata`:

* Chaves públicas e certificados de todas as funções (exceto funções de delegação) são armazenados do `root.json`.
* Chaves públicas e certificados da função de delegação são armazenados no arquivo JSON de sua função do pai (por exemplo `targets.json` para o `targets/releases` função).

É aconselhável para verificar essas chaves públicas e certificados depois da verificação TUF geral feita pelo cliente do Docker e cartório.

### <a name="how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource"></a>Como faço para dar acesso a imagens de pull ou push sem permissão para gerenciar o recurso de registro?

Dá suporte ao ACR [funções personalizadas](container-registry-roles.md) que fornecem níveis diferentes de permissões. Especificamente, `AcrPull` e `AcrPush` as funções permitem que os usuários às imagens de pull e/ou envio por push sem a permissão para gerenciar o recurso de registro no Azure.

* Portal do Azure: O registro de controle de acesso (IAM) -> -> Adicionar (selecione `AcrPull` ou `AcrPush` para a função).
* CLI do Azure: Localize a ID de recurso do registro, executando o seguinte comando:

  ```azurecli
  az acr show -n myRegistry
  ```
  
  Em seguida, você pode atribuir a `AcrPull` ou `AcrPush` função para um usuário (o exemplo a seguir usa `AcrPull`):

  ```azurecli
    az role assignment create --scope resource_id --role AcrPull --assignee user@example.com
    ```

  Ou, atribua a função a uma entidade de serviço identificada pela sua ID do aplicativo:

  ```
  az role assignment create --scope resource_id --role AcrPull --assignee 00000000-0000-0000-0000-000000000000
  ```

O destinatário, em seguida, é capaz de autenticar e acessar imagens no registro.

* Para autenticar para um registro:
    
  ```azurecli
  az acr login -n myRegistry 
  ```

* Para repositórios de lista:

  ```azurecli
  az acr repository list -n myRegistry
  ```

 Para efetuar pull de uma imagem:
    
  ```azurecli
  docker pull myregistry.azurecr.io/hello-world
  ```

Com o uso de apenas o `AcrPull` ou `AcrPush` função, o destinatário não tem permissão para gerenciar o recurso de registro no Azure. Por exemplo, `az acr list` ou `az acr show -n myRegistry` não mostrará o registro.

### <a name="how-do-i-enable-automatic-image-quarantine-for-a-registry"></a>Como habilitar a quarentena automáticas de imagem para um registro?

Quarentena de imagem é um recurso de visualização do ACR. Você pode habilitar o modo de quarentena de um registro para que somente as imagens que passaram com êxito a verificação de segurança são visíveis aos usuários normais. Para obter detalhes, consulte o [repositório GitHub de ACR](https://github.com/Azure/acr/tree/master/docs/preview/quarantine).

## <a name="diagnostics"></a>Diagnósticos

- [pull do docker falha com erro: net/http: solicitação cancelada enquanto aguarda a conexão (Client.Timeout excedido enquanto aguarda os cabeçalhos)](#docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers)
- [push do docker for bem-sucedida, mas pull do docker falha com erro: não autorizado: autenticação necessária](#docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required)
- [Habilitar e obter os logs de depuração do daemon de docker](#enable-and-get-the-debug-logs-of-the-docker-daemon) 
- [Novas permissões de usuário podem não ser eficaz imediatamente após a atualização](#new-user-permissions-may-not-be-effective-immediately-after-updating)
- [Informações de autenticação não são fornecidas no formato correto em chamadas de API REST diretas](#authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls)
- [Por que não o portal do Azure lista todos os Meus repositórios ou marcas?](#why-does-the-azure-portal-not-list-all-my-repositories-or-tags)
- [Como coletar rastreamentos de http no Windows?](#how-do-i-collect-http-traces-on-windows)

### <a name="docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers"></a>pull do docker falha com erro: net/http: solicitação cancelada enquanto aguarda a conexão (Client.Timeout excedido enquanto aguarda os cabeçalhos)

 - Se esse erro é um problema temporário, a tentativa será bem-sucedida.
 - Se `docker pull` falha contínua, em seguida, pode haver um problema com o daemon do docker. O problema geralmente pode ser atenuado reiniciando o daemon do docker. 
 - Se você continuar vendo esse problema depois de reiniciar o daemon do docker, o problema poderia ser alguns problemas de conectividade de rede com a máquina. Para verificar se gerais de rede na máquina está íntegro, testar, como um comando `ping www.bing.com`.
 - Você deve sempre ter um mecanismo de repetição em todas as operações de cliente do docker.

### <a name="docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required"></a>push do docker for bem-sucedida, mas pull do docker falha com erro: não autorizado: autenticação necessária

Esse erro pode ocorrer com a versão do Red Hat do daemon do docker, onde `--signature-verification` é habilitado por padrão. Você pode verificar as opções de daemon do docker para o Red Hat Enterprise Linux (RHEL) ou Fedora executando o seguinte comando:

```bash
grep OPTIONS /etc/sysconfig/docker
```

Por exemplo, Fedora 28 Server tem as seguintes opções de daemon do docker:

```
OPTIONS='--selinux-enabled --log-driver=journald --live-restore'
```

Com o `--signature-verification=false` ausentes, `docker pull` falhará com um erro semelhante a:

```bash
Trying to pull repository myregistry.azurecr.io/myimage ...
unauthorized: authentication required
```

Para resolver o erro:
1. Adicione a opção `--signature-verification=false` para o arquivo de configuração do daemon de docker `/etc/sysconfig/docker`. Por exemplo:

  ```
  OPTIONS='--selinux-enabled --log-driver=journald --live-restore --signature-verification=false'
  ```
2. Reinicie o serviço daemon do docker executando o seguinte comando:

  ```bash
  sudo systemctl restart docker.service
  ```

Detalhes da `--signature-verification` pode ser encontrada executando `man dockerd`.

### <a name="enable-and-get-the-debug-logs-of-the-docker-daemon"></a>Habilitar e obter os logs de depuração do daemon de docker  

Inicie `dockerd` com o `debug` opção. Primeiro, crie o arquivo de configuração do daemon de docker (`/etc/docker/daemon.json`) se ele não existe e adicionar o `debug` opção:

```json
{   
    "debug": true   
}
```

Em seguida, reinicie o daemon. Por exemplo, com Ubuntu 14.04:

```bash
sudo service docker restart
```

Detalhes podem ser encontrados na [documentação do Docker](https://docs.docker.com/engine/admin/#enable-debugging). 

 * Os logs podem ser gerados em locais diferentes, dependendo do seu sistema. Por exemplo, para o Ubuntu 14.04, ele tem `/var/log/upstart/docker.log`.   
Ver [documentação do Docker](https://docs.docker.com/engine/admin/#read-the-logs) para obter detalhes.    

 * Para o Docker para Windows, os logs são gerados sob % LOCALAPPDATA%/docker/. No entanto não pode conter todas as informações de depuração ainda.   

   Para acessar o log completo de daemon, talvez seja necessário algumas etapas adicionais:

    ```console
    docker run --privileged -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /usr/local/bin/docker:/usr/local/bin/docker alpine sh

    docker run --net=host --ipc=host --uts=host --pid=host -it --security-opt=seccomp=unconfined --privileged --rm -v /:/host alpine /bin/sh
    chroot /host
    ```
    Agora você tem acesso a todos os arquivos da VM executando `dockerd`. O log está em `/var/log/docker.log`.

### <a name="new-user-permissions-may-not-be-effective-immediately-after-updating"></a>Novas permissões de usuário podem não ser eficaz imediatamente após a atualização

Quando você conceder novas permissões (novas funções) para uma entidade de serviço, a alteração não podem entram em vigor imediatamente. Há dois motivos possíveis:

* Atraso da atribuição de função do Active Directory do Azure. Normalmente é rápida, mas pode levar minutos devido a atraso de propagação.
* Atraso de permissão no servidor de token do ACR. Isso pode levar até 10 minutos. Para atenuar, você pode `docker logout` e, em seguida, autentique novamente com o mesmo usuário após um minuto:

  ```bash
  docker logout myregistry.azurecr.io
  docker login myregistry.azurecr.io
  ```

Atualmente, ACR não dá suporte a exclusão de replicação inicial pelos usuários. A solução alternativa é incluir a replicação inicial criados no modelo, mas ignorar a sua criação, adicionando `"condition": false` conforme mostrado abaixo:

```json
{
    "name": "[concat(parameters('acrName'), '/', parameters('location'))]",
    "condition": false,
    "type": "Microsoft.ContainerRegistry/registries/replications",
    "apiVersion": "2017-10-01",
    "location": "[parameters('location')]",
    "properties": {},
    "dependsOn": [
        "[concat('Microsoft.ContainerRegistry/registries/', parameters('acrName'))]"
     ]
},
```

### <a name="authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls"></a>Informações de autenticação não são fornecidas no formato correto em chamadas de API REST diretas

Você pode encontrar uma `InvalidAuthenticationInfo` erro, especialmente ao usar o `curl` ferramenta com a opção `-L`, `--location` (para seguir redirecionamentos).
Por exemplo, ao buscar o blob usando `curl` com `-L` opção e a autenticação básica:

```bash
curl -L -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest
```

pode resultar em resposta a seguir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Error><Code>InvalidAuthenticationInfo</Code><Message>Authentication information is not given in the correct format. Check the value of Authorization header.
RequestId:00000000-0000-0000-0000-000000000000
Time:2019-01-01T00:00:00.0000000Z</Message></Error>
```

A causa raiz é que alguns `curl` implementações seguem redirecionamentos com cabeçalhos da solicitação original.

Para resolver o problema, você precisará seguir redirecionamentos manualmente, sem os cabeçalhos. Imprimir os cabeçalhos de resposta com o `-D -` opção de `curl` e, em seguida, extraia: o `Location` cabeçalho:

```bash
redirect_url=$(curl -s -D - -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest | grep "^Location: " | cut -d " " -f2 | tr -d '\r')
curl $redirect_url
```

### <a name="why-does-the-azure-portal-not-list-all-my-repositories-or-tags"></a>Por que não o portal do Azure lista todos os Meus repositórios ou marcas? 

Se você estiver usando o navegador Microsoft Edge, você pode ver no máximo 100 repositórios ou as marcas listadas. Se o registro tiver mais de 100 repositórios ou marcas, é recomendável que você use o navegador Firefox ou Chrome listá-los.

### <a name="how-do-i-collect-http-traces-on-windows"></a>Como coletar rastreamentos de http no Windows?

#### <a name="prerequisites"></a>Pré-requisitos

- Habilite a descriptografia https no fiddler:  <https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS>
- Habilite o Docker para usar um proxy por meio da interface do usuário do Docker: <https://docs.docker.com/docker-for-windows/#proxies>
- Certifique-se de reverter ao concluir.  Docker não funcionará com esse recurso habilitado e o fiddler não está em execução.

#### <a name="windows-containers"></a>Contêineres do Windows

Configurar o proxy do Docker para 127.0.0.1: 8888

#### <a name="linux-containers"></a>Contêineres do Linux

Localize o ip do Docker de comutador virtual da vm:

```powershell
(Get-NetIPAddress -InterfaceAlias "*Docker*" -AddressFamily IPv4).IPAddress
```

Configurar o proxy de Docker para a saída do comando anterior e a porta 8888 (por exemplo, 10.0.75.1:8888)

## <a name="tasks"></a>Tarefas

- [Como em lotes de execuções de cancelar?](#how-do-i-batch-cancel-runs)
- [Como posso incluir a pasta. git no comando de compilação de acr az?](#how-do-i-include-the-git-folder-in-az-acr-build-command)

### <a name="how-do-i-batch-cancel-runs"></a>Como em lotes de execuções de cancelar?

Os comandos a seguir cancela todas as tarefas em execução no registro especificado.

```azurecli
az acr task list-runs -r $myregistry --run-status Running --query '[].runId' -o tsv \
| xargs -I% az acr task cancel-run -r $myregistry --run-id %
```

### <a name="how-do-i-include-the-git-folder-in-az-acr-build-command"></a>Como posso incluir a pasta. git no comando de compilação de acr az?

Se você passar uma pasta de origem local para o `az acr build` comando, o `.git` pasta é excluída do pacote carregado por padrão. Você pode criar um `.dockerignore` arquivo com a configuração a seguir. Ele informa ao comando para restaurar todos os arquivos em `.git` no pacote carregado. 

```
!.git/**
```

Essa configuração também se aplica a `az acr run` comando.

## <a name="cicd-integration"></a>Integração de CI/CD

- [CircleCI](https://github.com/Azure/acr/blob/master/docs/integration/CircleCI.md)
- [Ações do GitHub](https://github.com/Azure/acr/blob/master/docs/integration/github-actions/github-actions.md)

## <a name="error-references-for-az-acr-check-health"></a>Referências de erro para `az acr check-health`

### <a name="dockercommanderror"></a>DOCKER_COMMAND_ERROR

Esse erro significa que o cliente docker para a CLI não pôde ser encontrado, que impede o localizar a versão do docker, avaliar o status do daemon de docker e garantir que o comando docker pull pode ser executado.

*Possíveis soluções*: Instalando o cliente de docker; Adicionar caminho de docker para as variáveis do sistema.

### <a name="dockerdaemonerror"></a>DOCKER_DAEMON_ERROR

Esse erro significa que o status do daemon de docker não está disponível ou que ele não pôde ser acessado usando a CLI. Isso significa que as operações de docker (por exemplo, "logon", "pull") não estará disponíveis por meio da CLI.

*Possíveis soluções*: Reinicie o daemon do docker ou validar que ele está instalado corretamente.

### <a name="dockerversionerror"></a>DOCKER_VERSION_ERROR

Esse erro significa que a CLI não foi capaz de executar o comando `docker --version`.

*Possíveis soluções*: tente executar o comando manualmente, verifique se você tiver a versão mais recente da CLI e investigar a mensagem de erro.

### <a name="dockerpullerror"></a>DOCKER_PULL_ERROR

Esse erro significa que a CLI não pôde efetuar pull em uma imagem de exemplo para seu ambiente.

*Possíveis soluções*: validar se todos os componentes necessários para efetuar pull de uma imagem estiverem sendo executado corretamente.

### <a name="helmcommanderror"></a>HELM_COMMAND_ERROR

Esse erro significa que o cliente helm não foi possível encontrar pela CLI, que impede outras operações do helm.

*Possíveis soluções*: Verifique se que o helm é instalado e seu caminho é adicionado às variáveis de ambiente do sistema.

### <a name="helmversionerror"></a>HELM_VERSION_ERROR

Esse erro significa que a CLI não pôde determinar a versão do Helm instalada. Isso pode acontecer se a versão da CLI do Azure (ou, se a versão do helm) que está sendo usada está obsoleta.

*Possíveis soluções*: atualizar para a versão mais recente da CLI do Azure ou para a versão recomendada do helm; execute o comando manualmente e investigar a mensagem de erro.

### <a name="connectivitydnserror"></a>CONNECTIVITY_DNS_ERROR

Esse erro significa que o DNS para o servidor de logon do registro específica foi um ping, mas não respondeu a ele, que significa que ele não estiver disponível. Isso pode indicar alguns problemas de conectividade. Isso também pode significar que o registro não existir, o usuário não tem as permissões do registro (para recuperar seu servidor de logon corretamente), ou que o registro de destino é em uma nuvem diferente daquele que está sendo usado na CLI do Azure.

*Possíveis soluções*: validar a conectividade; Verifique a ortografia do registro, e que o registro existe; Verifique se que o usuário tem as permissões corretas nele e que a nuvem do registro é o mesmo que está sendo usado na CLI do Azure.

### <a name="connectivityforbiddenerror"></a>CONNECTIVITY_FORBIDDEN_ERROR

Isso significa que o ponto de extremidade de desafio para o registro de determinado respondeu com o status 403 Proibido HTTP. Isso significa que os usuários não têm acesso ao registro, provavelmente devido a uma configuração de rede virtual.

*Possíveis soluções*: remover as regras de rede virtual ou adicionar o IP do cliente atual à lista de permissões.

### <a name="connectivitychallengeerror"></a>CONNECTIVITY_CHALLENGE_ERROR

Esse erro significa que o ponto de extremidade do desafio de registro de destino não emitiu um desafio.

*Possíveis soluções*: tente novamente após algum tempo. Se o erro persistir, abra o problema de am no https://aka.ms/acr/issues.

### <a name="connectivityaadloginerror"></a>CONNECTIVITY_AAD_LOGIN_ERROR

Esse erro significa que o ponto de extremidade do desafio de registro de destino emitiu um desafio, mas o registro não oferece suporte a logon do AAD.

*Possíveis soluções*: tente outra maneira de log em, por exemplo, as credenciais de administrador. Caso o usuário deseja fazer logon com suporte do AAD, abra o problema de am no https://aka.ms/acr/issues.

### <a name="connectivityrefreshtokenerror"></a>CONNECTIVITY_REFRESH_TOKEN_ERROR

Isso significa que o servidor de logon do registro não respondeu com um token de atualização, o que significa que o acesso ao registro de destino foi negado. Isso pode acontecer se o usuário não tem as permissões corretas no registro ou se as credenciais do usuário para a CLI do Azure estão obsoletas.

*Soluções potenciais*: Verifique se o usuário tem as permissões corretas no registro; executar `az login` para atualizar as permissões, tokens e credenciais.

### <a name="connectivityaccesstokenerror"></a>CONNECTIVITY_ACCESS_TOKEN_ERROR

Isso significa que o servidor de logon do registro não respondeu com um token de acesso, o que significa que o acesso ao registro de destino foi negado. Isso pode acontecer se o usuário não tem as permissões corretas no registro ou se as credenciais do usuário para a CLI do Azure estão obsoletas.

*Soluções potenciais*: Verifique se o usuário tem as permissões corretas no registro; executar `az login` para atualizar as permissões, tokens e credenciais.

### <a name="loginservererror"></a>LOGIN_SERVER_ERROR

Isso significa que a CLI não foi possível localizar o servidor de logon do registro especificado e nenhum sufixo padrão foi encontrado para a nuvem atual. Isso pode acontecer se o registro não existir, se o usuário não tem as permissões corretas no registro, se a nuvem do registro e a nuvem atual da CLI do Azure não correspondem, ou se a versão da CLI do Azure está obsoleta.

*Possíveis soluções*: verificar se a ortografia está correta e que o registro existe; Verifique se o usuário tem as permissões corretas no registro e que as nuvens do registro e do ambiente de CLI corresponderem, atualize a CLI do Azure para a versão mais recente.

## <a name="next-steps"></a>Próximas etapas

* [Saiba mais](container-registry-intro.md) sobre o registro de contêiner do Azure.
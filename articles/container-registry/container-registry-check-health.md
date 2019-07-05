---
title: Verifique a integridade do registro no registro de contêiner do Azure
description: Saiba como executar um comando rápido de diagnóstico para identificar problemas comuns ao usar um registro de contêiner do Azure, incluindo a configuração do Docker local e a conectividade com o registro
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: 3e5b5467f9fa25e23f6661c6630d346aa85e2205
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555092"
---
# <a name="check-the-health-of-an-azure-container-registry"></a>Verificar a integridade de um registro de contêiner do Azure

Ao usar um registro de contêiner do Azure, você pode ocasionalmente encontrar problemas. Por exemplo, você não poderá efetuar pull de uma imagem de contêiner devido a um problema com o Docker em seu ambiente local. Ou, um problema de rede pode impedir a conexão com o registro. 

Como uma primeira etapa de diagnóstico, execute as [az acr verificação de integridade][az-acr-check-health] command to get information about the health of the environment and optionally access to a target registry. This command is available in Azure CLI version 2.0.67 or later. If you need to install or upgrade, see [Install Azure CLI][azure-cli].

## <a name="run-az-acr-check-health"></a>Run az acr check-health

Os exemplos a seguir mostram diferentes maneiras de executar o `az acr check-health` comando.

> [!NOTE]
> Se você executar o comando no Azure Cloud Shell, o ambiente local não é verificado. No entanto, você pode verificar o acesso a um registro de destino.

### <a name="check-the-environment-only"></a>Verificar apenas o ambiente

Para verificar o local do Docker daemon, a versão da CLI e a configuração de cliente do Helm, execute o comando sem parâmetros adicionais:

```azurecli
az acr check-health
```

### <a name="check-the-environment-and-a-target-registry"></a>Verifique o ambiente e um registro de destino

Para verificar o acesso a um registro, bem como executar verificações do ambiente local, passe o nome de um registro de destino. Por exemplo:

```azurecli
az acr check-health --name myregistry
```

## <a name="error-reporting"></a>Relatório de erros

O comando registra informações na saída padrão. Se um problema for detectado, ele fornece um código de erro e uma descrição. Para obter mais informações sobre os códigos e as soluções possíveis, consulte o [referência de erro](container-registry-health-error-reference.md).

Por padrão, o comando interrompe sempre que encontra um erro. Você também pode executar o comando para que ele forneça a saída para todas as verificações de integridade, mesmo se forem encontrados erros. Adicionar o `--ignore-errors` parâmetro, conforme mostrado nos exemplos a seguir:

```azurecli
# Check environment only
az acr check-health --ignore-errors

# Check environment and target registry
az acr check-health --name myregistry --ignore-errors
```      

Exemplo de saída:

```console
$ az acr check-health --name myregistry --ignore-errors --yes

Docker daemon status: available
Docker version: Docker version 18.09.2, build 6247962
Docker pull of 'mcr.microsoft.com/mcr/hello-world:latest' : OK
ACR CLI version: 2.2.9
Helm version:
Client: &version.Version{SemVer:"v2.14.1", GitCommit:"5270352a09c7e8b6e8c9593002a73535276507c0", GitTreeState:"clean"}
DNS lookup to myregistry.azurecr.io at IP 40.xxx.xxx.162 : OK
Challenge endpoint https://myregistry.azurecr.io/v2/ : OK
Fetch refresh token for registry 'myregistry.azurecr.io' : OK
Fetch access token for registry 'myregistry.azurecr.io' : OK
```  



## <a name="next-steps"></a>Próximas etapas

Para obter detalhes sobre os códigos de erro retornados pelo [az acr verificação de integridade][az-acr-check-health] de comando, consulte o [referência de erro de verificação de integridade](container-registry-health-error-reference.md).

Consulte a [perguntas frequentes sobre](container-registry-faq.md) para perguntas frequentes e outros problemas conhecidos sobre o registro de contêiner do Azure.





<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health

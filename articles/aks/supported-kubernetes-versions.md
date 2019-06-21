---
title: Versões do Kubernetes com suporte no Serviço de Kubernetes do Azure
description: Entender a política de suporte de versão do Kubernetes e o ciclo de vida dos clusters no AKS (Serviço de Kubernetes do Azure)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 05/20/2019
ms.author: saudas
ms.openlocfilehash: b7910ee6f58c582b824cec834d92a24c0e184bfb
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205285"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Versões do Kubernetes com suporte no AKS (Serviço de Kubernetes do Azure)

A comunidade Kubernetes libera versões secundárias aproximadamente a cada três meses. Essas versões incluem novos recursos e melhorias. As versões de patch são mais frequentes (às vezes, semanais) e são destinadas apenas a correções de bugs críticas em uma versão secundária. Essas versões de patch incluem correções de vulnerabilidades de segurança ou de bugs importantes que afetam um grande número de clientes e produtos em execução na produção com base no Kubernetes.

AKS pretende verificar e lançar as novas versões do Kubernetes dentro de 30 dias de um lançamento de upstream, sujeito à estabilidade da versão.

## <a name="kubernetes-versions"></a>Versões do Kubernetes

Kubernetes usa o padrão [controle de versão semântico](https://semver.org/) esquema de controle de versão. Isso significa que cada versão do Kubernetes segue o esquema de numeração:

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
  1.13.7
```

Cada número na versão indica compatibilidade geral com a versão anterior:

* Alterações de API com a alteração de versões principais quando incompatível ou compatibilidade com versões anteriores pode ser corrompida.
* As versões secundárias alterar quando forem feitas alterações de funcionalidade que são compatíveis com versões anteriores para as outras versões secundárias.
* São feitas alterações de versões de patch quando correções de bugs compatíveis com versões anteriores.

Em geral, os usuários devem se esforçar para executar a versão de patch mais recente da versão secundária são executados, por exemplo, se o cluster de produção estiver em *1.13.6* e *1.13.7* é o patch mais recente disponível versão disponível para o *1.13* série, você deve atualizar para o *1.13.7* assim que você é capaz de garantir que o cluster é totalmente com patch e suporte.

## <a name="kubernetes-version-support-policy"></a>Política de suporte de versão do Kubernetes

O AKS dá suporte a quatro versões secundárias do Kubernetes:

* A versão secundária atual que é liberada no AKS (N)
* Três versões secundárias anteriores. Cada versão secundária com suporte também dá suporte a dois patches estáveis.

Isso é conhecido como "N-3"-(N (versão mais recente) - 3 (as versões secundárias)).

Por exemplo, se apresenta AKS *1.13.x* hoje em dia, o suporte é fornecido para as seguintes versões:

Nova versão secundária    |    Lista de versões com suporte
-----------------    |    ----------------------
1.13.x               |    1.12.a, 1.12.b, 1.11.a, 1.11.b, 1.10.a, 1.10.b

Em que "x" e "1).a" e "1).b" são versões de patch representativo.

Para obter detalhes sobre comunicações sobre alterações de versão e as expectativas, consulte "Comunicações" abaixo.

Quando uma nova versão secundária é introduzida, os mais antigo versão e patch versões secundárias com suporte são preteridas e removidas. Por exemplo, se a lista de versões com suporte do atual está:

<a name="supported-version-list"></a>Lista de versões com suporte
----------------------
1.12.a, 1.12.b, 1.11.a, 1.11.b, 1.10.a, 1.10.b, 1.9.a, 1.9.b

E AKS libera 1.13.x, isso significa que as versões de 1.9.x (todas as versões de 1,9) serão removidas e sem suporte.

> [!NOTE]
> Observe que se os clientes estiverem executando uma versão sem suporte do Kubernetes, eles serão solicitados a atualização ao solicitar suporte para o cluster. Clusters que executam versões do Kubernetes sem suporte não são cobertas pela [AKS dão suporte às políticas](https://docs.microsoft.com/azure/aks/support-policies).


Além do citado acima em versões secundárias, AKS oferece suporte a dois mais recentes *patch** versões de uma determinada versão secundária. Por exemplo, considerando as seguintes versões com suporte:

<a name="supported-version-list"></a>Lista de versões com suporte
----------------------
1.12.1, 1.12.2, 1.11.4, 1.11.5

Se Kubernetes upstream lançado 1.12.3 e 1.11.6 e AKS libera as versões de patch, as versões de patch mais antigas são preteridas e removidas e torna-se a lista de versões com suporte:

<a name="supported-version-list"></a>Lista de versões com suporte
----------------------
1.12.*2*, 1.12.*3*, 1.11.*5*, 1.11.*6*

> [!NOTE]
> Os clientes não devem fixar a criação do cluster, CI ou outras tarefas automatizadas para as versões de patch específicos. 

### <a name="communications"></a>Comunicações

* Para o novo **secundárias** versões do Kubernetes
  * Todos os usuários são notificados publicamente sobre a nova versão e qual versão será removida.
  * Quando uma nova versão de patch é liberada, a versão de patch mais antiga é removida ao mesmo tempo.
  * Os clientes têm **60 dias** partir da data de notificação pública para atualizar para uma versão com suporte de versão secundária.
* Para o novo **patch** versões do Kubernetes
  * Todos os usuários são notificados da nova versão do patch que está sendo lançado e a atualização para a versão de patch mais recente.
  * Os usuários têm **30 dias** para atualizar para uma versão de patch mais recente, com suporte. Os usuários têm **30 dias** para atualizar para uma versão de patch com suporte antes do mais antigo é removido.

AKS define "liberado" como a disponibilidade geral, habilitada em todos os SLO / qualidade de medições de serviço e está disponível em todas as regiões.

> [!NOTE]
> Os clientes são notificados de Kubernetes lançamentos de versões e reprovações, quando uma versão secundária é preterido/removidos usuários recebem 60 dias para atualizar para uma versão com suporte. No caso de versões de patch, os clientes recebem a 30 dias para atualizar para uma versão com suporte.

As notificações são enviadas por meio de:

* [Notas de versão do AKS](https://aka.ms/aks/releasenotes)
* Notificações do portal do Azure
* [Canal de atualização do Azure][azure-update-channel]

### <a name="policy-exceptions"></a>Exceções à política

O AKS se reserva o direito de adicionar ou remover versões novos/existentes que foram identificadas para ter um ou mais críticos de produção que afetam a bugs ou problemas de segurança sem aviso prévio.

Versões de patch específicos podem ser ignoradas ou distribuição acelerada dependendo da gravidade do problema bug ou de segurança.

### <a name="azure-portal-and-cli-default-versions"></a>Portal do Azure e versões de padrão da CLI

Quando você implanta um cluster AKS no portal ou com a CLI do Azure, o cluster é sempre definido como a versão secundária do n-1 e o patch mais recente. Por exemplo, se o AKS oferece suporte à *1.13.x*, *1.12.a* + *1.12.b*, *1.11.a*  +   *1.11.b*, *1.10.a* + *1.10b*, é a versão padrão para novos clusters *1.12.b*.

AKS padrão é n-1 (por exemplo, 1.12.b minor.latestPatch) para fornecer aos clientes um conhecido, estável e a versão corrigida por padrão.

## <a name="list-currently-supported-versions"></a>Lista de versões compatíveis no momento

Para descobrir quais versões estão disponíveis atualmente para sua assinatura e região, use o comando [az aks get-versions][az-aks-get-versions]. O exemplo a seguir lista as versões disponíveis do Kubernetes para a região *EastUS*:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

A saída é semelhante ao exemplo a seguir, que mostra essa versão do Kubernetes *1.13.5* é a versão mais recente disponível:

```
KubernetesVersion    Upgrades
-------------------  ------------------------
1.13.5               None available
1.12.7               1.13.5
1.12.6               1.12.7, 1.13.5
1.11.9               1.12.6, 1.12.7
1.11.8               1.11.9, 1.12.6, 1.12.7
1.10.13              1.11.8, 1.11.9
1.10.12              1.10.13, 1.11.8, 1.11.9
```

## <a name="faq"></a>Perguntas frequentes

**O que acontece quando um cliente atualiza um cluster do Kubernetes com uma versão secundária sem suporte?**

Se você estiver usando o *n-4* versão, estão fora do suporte e será solicitado a atualizar. Se a atualização de n-4 de versão para n-3 for bem-sucedida, agora você está dentro de nossas políticas de suporte. Por exemplo:

- Se as versões com suporte do AKS *1.13.x*, *1.12.a* + *1.12.b*, *1.11.c*  +  *1!d 1.11*, e *1.10.e* + *1.10F* e você estiver usando *1.9.g* ou *1.9.h*, estão fora do suporte.
- Se a atualização da *1.9.g* ou *1.9.h* para *1.10.e* ou *1.10.f* for bem-sucedida, retornar a dentro de nossas políticas de suporte.

Não há suporte para atualizações para versões anteriores à *n-4*. Nesses casos, é recomendado que os clientes criem clusters do AKS e reimplantem suas cargas de trabalho.

**O que significa 'fora de suporte'**

'Fora do suporte para' significa que a versão que você está executando está fora da lista de versões com suporte, e você será solicitado a atualizar o cluster para uma versão com suporte ao solicitar suporte. Além disso, o AKS não faz qualquer tempo de execução ou outras garantias para clusters de fora da lista de versões com suporte.

**O que acontece quando um cliente dimensiona um cluster do Kubernetes com uma versão secundária sem suporte?**

Para as versões secundárias sem suporte no AKS, a escala ou a redução horizontal continua a funcionar sem problemas.

**Um cliente pode permanecer em uma versão do Kubernetes para sempre?**

Sim. No entanto, se o cluster não estiver em uma das versões com suporte pelo AKS, o cluster está fora das políticas de suporte do AKS. O Azure não atualiza nem exclui o cluster automaticamente.

**A qual versão a mestre dá suporte quando o cluster do agente não está em uma das versões com suporte do AKS?**

O mestre é atualizado automaticamente para a versão mais recente com suporte.

## <a name="next-steps"></a>Próximas etapas

Para obter informações de como atualizar seu cluster, confira [Atualizar um cluster do AKS (Serviço de Kubernetes do Azure)][aks-upgrade].

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions

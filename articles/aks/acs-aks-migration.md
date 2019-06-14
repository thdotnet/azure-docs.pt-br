---
title: Migrar do serviço de contêiner do Azure (ACS) para o serviço de Kubernetes do Azure (AKS)
description: Migre do serviço de contêiner do Azure (ACS) para o serviço de Kubernetes do Azure (AKS).
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: dcee8da943603fb0978caf9992be76347ca197d6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65977708"
---
# <a name="migrate-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Migrar do serviço de contêiner do Azure (ACS) para o serviço de Kubernetes do Azure (AKS)

Este artigo ajuda você a planejar e executar uma migração bem-sucedida entre o serviço de contêiner do Azure (ACS) com o Kubernetes e o serviço de Kubernetes do Azure (AKS). Para ajudá-lo a tomar decisões importantes, este guia fornece detalhes sobre as diferenças entre o ACS e o AKS e fornece uma visão geral do processo de migração.

## <a name="differences-between-acs-and-aks"></a>Diferenças entre o ACS e o AKS

O ACS e AKS diferem em algumas áreas importantes que afetam a migração. Antes de qualquer migração, você deve examinar e planejar resolver as diferenças a seguir:

* Usarem nós do AKS [discos gerenciados](../virtual-machines/windows/managed-disks-overview.md).
    * Discos não gerenciados devem ser convertidos antes de anexá-los a nós do AKS.
    * Custom `StorageClass` objetos para os discos do Azure devem ser alterados de `unmanaged` para `managed`.
    * Qualquer `PersistentVolumes` devem usar `kind: Managed`.
* AKS oferece suporte à [vários pools de nó](https://docs.microsoft.com/azure/aks/use-multiple-node-pools) (atualmente em versão prévia).
* Nós com base no Windows Server estão atualmente em [versão prévia no AKS](https://azure.microsoft.com/blog/kubernetes-on-azure/).
* AKS oferece suporte a um conjunto limitado de [regiões](https://docs.microsoft.com/azure/aks/quotas-skus-regions).
* O AKS é um serviço gerenciado com um plano de controle de Kubernetes hospedado. Você talvez precise modificar seus aplicativos se anteriormente você tiver modificado a configuração do ACS mestres.

## <a name="differences-between-kubernetes-versions"></a>Diferenças entre as versões do Kubernetes

Se você estiver migrando para uma versão mais recente do Kubernetes (por exemplo, de 1.7.x para 1.9.x), examine os recursos a seguir para entender algumas alterações para a API do Kubernetes:

* [Migrando um ThirdPartyResource para CustomResourceDefinition](https://kubernetes.io/docs/tasks/access-kubernetes-api/migrate-third-party-resource/)
* [Alterações de API de cargas de trabalho nas versões 1.8 e 1.9](https://kubernetes.io/docs/reference/workloads-18-19/)

## <a name="migration-considerations"></a>Considerações sobre a migração

### <a name="agent-pools"></a>Pools de agente

Embora o AKS gerencia o plano de controle Kubernetes, você ainda define o tamanho e o número de nós incluir em seu novo cluster. Supondo que você deseja ter um mapeamento de 1:1 de ACS para o AKS, convém capturar as informações de nó de ACS existentes. Use esses dados quando você cria o novo cluster do AKS.

Exemplo:

| NOME | Count | Tamanho da VM | Sistema operacional |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 | Windows |

Como as máquinas virtuais serão implantadas na sua assinatura durante a migração, você precisa verificar se suas cotas e limites são suficientes para esses recursos. 

Para obter mais informações, consulte [assinatura do Azure e limites de serviço](https://docs.microsoft.com/azure/azure-subscription-service-limits). Para verificar suas cotas atuais, no portal do Azure, vá para o [folha de assinaturas](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), selecione sua assinatura e, em seguida, selecione **uso + cotas**.

### <a name="networking"></a>Rede

Para aplicativos complexos, geralmente a migração ocorre ao longo do tempo em vez de uma só vez. Isso significa que os ambientes novos e antigos talvez precise se comunicar pela rede. Aplicativos que usava `ClusterIP` serviços se comuniquem talvez precise ser exposto como tipo `LoadBalancer` e ser protegido adequadamente.

Para concluir a migração, você vai querer apontar os clientes para os novos serviços que estão em execução no AKS. É recomendável que você redirecionar tráfego atualizando DNS para apontar para o balanceador de carga que se encontra na frente do cluster do AKS.

### <a name="stateless-applications"></a>Aplicativos sem monitoração de estado

A migração de aplicativos sem monitoração de estado é o caso mais simples. Você irá aplicar suas definições de YAML para o novo cluster, verifique se que tudo está funcionando conforme o esperado e redirecionar o tráfego para ativar o novo cluster.

### <a name="stateful-applications"></a>Aplicativos com monitoração de estado

Planeje cuidadosamente sua migração de aplicativos com monitoração de estado para evitar a perda de dados ou tempo de inatividade inesperado.

#### <a name="highly-available-applications"></a>Aplicativos altamente disponíveis

Você pode implantar alguns aplicativos com monitoração de estado em uma configuração de alta disponibilidade. Esses aplicativos podem copiar dados entre réplicas. Se você estiver usando esse tipo de implantação, você poderá criar um novo membro no novo cluster AKS e, em seguida, migre com impacto mínimo sobre os chamadores de downstream. Em geral, as etapas de migração para esse cenário são:

1. Crie uma nova réplica secundária no AKS.
2. Aguarde a replicação de dados.
3. Não mais de tornar o novo primário de uma réplica secundária.
4. Apontar o tráfego para o cluster do AKS.

#### <a name="migrating-persistent-volumes"></a>Migrando volumes persistentes

Se você estiver migrando volumes persistentes existentes para o AKS, geralmente você seguirá estas etapas:

1. Grava de fechamento para novas sessões para o aplicativo. (Essa etapa é opcional e requer tempo de inatividade.)
2. Tire instantâneos dos discos.
3. Crie novos discos gerenciados a partir de instantâneos.
4. Crie volumes persistentes no AKS.
5. Atualizar as especificações de pod para [usar volumes existentes](https://docs.microsoft.com/azure/aks/azure-disk-volume) em vez de PersistentVolumeClaims (provisionamento estático).
6. Implante o aplicativo no AKS.
7. Valide.
8. Apontar o tráfego para o cluster do AKS.

> [!IMPORTANT]
> Se você optar por não gravações de fechamento para novas sessões, você precisará replicar os dados para a nova implantação. Caso contrário, você perderá os dados que foram gravados depois que você tirou instantâneos de disco.

Algumas ferramentas de código-fonte aberto podem ajudá-lo a criar discos gerenciados e migrar volumes entre clusters de Kubernetes:

* [Extensão de cópia do disco da CLI do Azure](https://github.com/noelbundick/azure-cli-disk-copy-extension) copia e converte os discos em grupos de recursos e regiões do Azure.
* [Extensão do Azure da CLI Kube](https://github.com/yaron2/azure-kube-cli) enumera volumes Kubernetes do ACS e os migra para um cluster do AKS.

#### <a name="azure-files"></a>Arquivos do Azure

Ao contrário dos discos, os Arquivos do Azure podem ser montados em vários hosts simultaneamente. Em seu cluster AKS, Azure e o Kubernetes não impedem que você crie um pod que ainda usa seu cluster do ACS. Para evitar a perda de dados e um comportamento inesperado, certifique-se de que os clusters não são gravadas os mesmos arquivos ao mesmo tempo.

Se seu aplicativo pode hospedar várias réplicas que apontam para o mesmo compartilhamento de arquivos, siga as etapas de migração sem monitoração de estado e implantar suas definições YAML no novo cluster. Caso contrário, uma abordagem de migração possível envolve as seguintes etapas:

1. Implante seu aplicativo no AKS com uma contagem de réplica de 0.
2. Dimensione o aplicativo sobre o ACS como 0. (Essa etapa requer tempo de inatividade.)
3. Dimensione o aplicativo no AKS até 1.
4. Valide.
5. Apontar o tráfego para o cluster do AKS.

Se você quiser iniciar com um compartilhamento vazio e faça uma cópia dos dados de origem, você pode usar o [ `az storage file copy` ](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) comandos para migrar seus dados.

### <a name="deployment-strategy"></a>Estratégia de implantação

É recomendável que você use seu pipeline de CI/CD existente para implantar uma configuração válida para o AKS. Clonar suas tarefas de implantação existente e certifique-se de que `kubeconfig` aponta para o novo cluster do AKS.

Se isso não é possível exportar definições de recursos do ACS e, em seguida, aplicá-las no AKS. É possível usar `kubectl` para exportar objetos.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

Várias ferramentas de código-fonte aberto podem ajudar, dependendo das suas necessidades de implantação:

* [Velero](https://github.com/heptio/ark) (essa ferramenta exige 1.7 Kubernetes).
* [Extensão do Kube CLI do Azure](https://github.com/yaron2/azure-kube-cli)
* [ReShifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Etapas da migração

1. [Criar um cluster AKS](https://docs.microsoft.com/azure/aks/create-cluster) por meio do portal do Azure, a CLI do Azure ou o modelo do Resource Manager.

   > [!NOTE]
   > Localizar modelos do Azure Resource Manager de exemplo para AKS na [do Azure para o AKS](https://github.com/Azure/AKS/tree/master/examples/vnet) repositório no GitHub.

2. Faça as alterações necessárias para suas definições YAML. Por exemplo, substitua `apps/v1beta1` com `apps/v1` para `Deployments`.

3. [Migrar volumes](#migrating-persistent-volumes) (opcional) do seu cluster do ACS para seu cluster do AKS.

4. Use o sistema de CI/CD para implantar aplicativos no AKS. Ou use kubectl para aplicar as definições de YAML.

5. Valide. Certifique-se de que seus aplicativos funcionem conforme o esperado e todos os dados migrados sido copiados.

6. Redirecione o tráfego. Atualize o DNS para apontar os clientes para sua implantação do AKS.

7. Conclua tarefas pós-migração. Se você migrou os volumes e optou por não gravações de fechamento para novas sessões, copie os dados para o novo cluster.

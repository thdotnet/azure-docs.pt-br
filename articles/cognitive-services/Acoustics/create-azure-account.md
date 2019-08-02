---
title: Configuração da conta do lote do Azure acústica do projeto acústicos
titlesuffix: Azure Cognitive Services
description: Este "como" descreve a configuração de uma conta do lote do Azure para uso com o Unity do projeto acústica e integrações do mecanismo inreal.
services: cognitive-services
author: ashtat
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ROBOTS: NOINDEX
ms.openlocfilehash: f11dfda62ebb53aba6254f2db4eace7c524141d4
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704892"
---
# <a name="project-acoustics-azure-batch-account-setup"></a>Configuração da conta do lote do Azure acústica do projeto acústicos
Este "como" descreve a configuração de uma conta do lote do Azure para uso com o Unity do projeto acústica e integrações do mecanismo inreal.

## <a name="get-an-azure-subscription"></a>Obtenha uma assinatura do Azure
Uma [assinatura do Azure](https://azure.microsoft.com/free/) é necessária antes de configurar contas do lote e armazenamento. Se você estiver se inscrevendo pela primeira vez, o Azure fornece alguns recursos gratuitos de tempo limitado e o crédito de US $200.

## <a name="create-azure-batch-and-storage-accounts"></a>Criar contas de lote e armazenamento do Azure
Em seguida, siga [estas instruções](https://docs.microsoft.com/azure/batch/batch-account-create-portal) para configurar o seu Azure Batch e as contas do Armazenamento do Azure associadas.

Escolha as opções padrão para as contas de lote e armazenamento:
  
  ![Captura de tela de opções de novas contas do lote do Azure mostrando as configurações padrão](media/new-batch-account-create.png)

  ![Captura de tela das opções de novas contas do armazenamento do Azure mostrando as configurações padrão](media/batch-storage-account-create.png)

Demora alguns minutos para o Azure implantar as contas. Procure uma notificação de conclusão no canto superior direito do portal.
  
  ![Captura de tela de notificação implantada de contas do Azure](media/batch-accounts-deploy-notification.png)

Suas contas agora devem estar visíveis no seu painel.
  
  ![Captura de tela do painel de portal do Azure mostrando uma conta de armazenamento e lote](media/azure-portal-dashboard.png)

## <a name="set-up-acoustics-bake-ui-with-azure-credentials"></a>Configurar a acústica da interface do usuário para assar com credenciais do Azure
Clique no link da conta do Lote no painel e, em seguida, clique no link **Chaves** na página da conta do Lote para acessar suas credenciais.
  
  ![Captura de tela da conta do lote do Azure com a página link para chaves realçada](media/batch-access-keys.png)

  ![Captura de tela da página chaves de conta do lote do Azure com chaves de acesso](media/batch-keys-info.png)

Clique no link **Conta de armazenamento** na página para acessar suas credenciais da conta do Armazenamento do Azure.
  
  ![Captura de tela da página chaves de conta de armazenamento do Azure com chaves de acesso](media/storage-keys-info.png)

Insira essas credenciais no [plug-](unity-baking.md) in de torta de Unity ou no [plug-in](unreal-baking.md)de inreal.

## <a name="node-types-and-region-support"></a>Tipos de nó e o suporte de região
A acústica do projeto requer nós de VM do Azure otimizados para computação da série Fsv2 e H que podem não ter suporte em todas as regiões do Azure. Por favor, verifique [esta tabela](https://azure.microsoft.com/global-infrastructure/services) para garantir que você está escolhendo o local certo para sua conta do Lote.
![Captura de tela mostrando máquinas virtuais do Azure por região](media/azure-regions.png) 

## <a name="upgrading-your-quota"></a>Atualizando sua cota
As contas do Azure Batch são aprovisionadas na criação de contas com um limite de 20 núcleos de computação. Talvez queiramos aumentar esse limite para intervalos mais rápidos, pois você pode paralelizar sua carga de trabalho acústica em vários nós, até o número de pontos de investigação em sua cena. Você pode solicitar um aumento de cota, clicando na **cota** link na sua página de portal de lote do Azure e, em seguida, clicando em **solicitação de aumento de cota**:

![Captura de tela da página de cota do Azure](media/azure-quotas.png)

## <a name="next-steps"></a>Próximas etapas
* Integrar o plug-in acústica do projeto ao seu [Unity](unity-integration.md) ou projeto [inreal](unreal-integration.md)


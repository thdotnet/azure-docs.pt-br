---
title: Carregamento e armazenamento na nuvem com Serviços de Mídia do Microsoft Azure | Microsoft Docs
description: Este artigo aborda conceitos de carregamento e armazenamento na nuvem.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/11/2019
ms.author: juliako
ms.openlocfilehash: 9cbb995eb3310a2263185d6fd6dba20efce37f38
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550160"
---
# <a name="cloud-upload-and-storage"></a>Upload e armazenamento na nuvem

Para iniciar o gerenciamento, a criptografia, a codificação, a análise e a transmissão de conteúdo de mídia no Azure, você precisa criar uma conta nos Serviços de Mídia. Ao criar uma conta dos Serviços de Mídia, você precisa fornecer o nome de um recurso de conta de Armazenamento do Azure. A conta de armazenamento especificada está conectada à sua conta de Serviços de Mídia. 

A conta dos Serviços de Mídia e todas as contas de armazenamento associadas precisam estar na mesma assinatura do Azure. Recomenda-se usar as contas de armazenamento no mesmo local da conta de Serviços de Mídia para evitar os custos adicionais da saída de dados e latência.

Você deve ter uma conta de armazenamento **Primária**, e pode ter quantas contas de armazenamento **Secundárias** você quiser associadas à sua conta dos Serviços de Mídia. Os Serviços de Mídia oferecem suporte a contas **v2 para fins gerais** (GPv2) ou contas **v1 para fins gerais** (GPv1). 

>[!NOTE]
> As contas somente blob não são permitidas como **Primárias**. 

É recomendável que você use GPv2, para que você possa aproveitar a vantagem de escolher entre camadas de armazenamento frequente e esporádico. Para saber mais sobre as contas de armazenamento, consulte [Visão geral da conta de Armazenamento do Microsoft Azure](../../storage/common/storage-account-overview.md). 

Há diferentes SKUs que você pode escolher para sua conta de armazenamento. Para obter mais informações, confira [Contas de armazenamento](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest). Caso deseje fazer experimentos com contas de armazenamento, use `--sku Standard_LRS`. No entanto, ao escolher um SKU para produção, você deverá considerar `--sku Standard_RAGRS`, que fornece replicação geográfica para a continuidade dos negócios. 

## <a name="assets-in-a-storage-account"></a>Ativos em uma conta de armazenamento

Em serviços de mídia v3, as APIs de armazenamento são usadas para carregar arquivos em ativos. Para obter mais informações, consulte [conceito de ativos](assets-concept.md).

> [!Note]
> Você não deve tentar alterar o conteúdo de contêineres de blob que foram gerados pelo SDK dos Serviços de Mídia sem o uso de APIs de Serviços de Mídia.
 
## <a name="storage-side-encryption"></a>Criptografia do armazenamento

Para proteger os Ativos em repouso, os ativos devem ser criptografados pela criptografia do armazenamento. A tabela a seguir mostra como a criptografia do armazenamento funciona nos Serviços de Mídia v3:

|Opção de criptografia|Descrição|Serviços de Mídia v3|
|---|---|---|
|Criptografia do Armazenamento dos Serviços de Mídia| Criptografia AES-256, chave gerenciada pelos Serviços de Mídia|Não é compatível<sup>(1)</sup>|
|[Criptografia do Serviço de Armazenamento para dados em repouso](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Criptografia do servidor oferecida pelo Armazenamento do Microsoft Azure, chave gerenciada pelo Azure ou pelo cliente|Com suporte|
|[Criptografia do cliente de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Criptografia do cliente oferecida pelo armazenamento do Azure, chave gerenciada pelo cliente no Key Vault|Sem suporte|

<sup>1</sup> Nos Serviços de Mídia v3, a criptografia de armazenamento (criptografia AES-256) somente terá suporte para compatibilidade com versões anteriores quando os Ativos tiverem sido criados com os Serviços de Mídia v2. Isso significa que o v3 funciona com recursos criptografados de armazenamento existentes, mas não permite a criação de novos recursos.

## <a name="storage-account-errors"></a>Erros de conta de armazenamento

O estado de "Desconectado" para uma conta de serviços de mídia indica que a conta não tem mais acesso a um ou mais contas de armazenamento anexado devido a uma alteração nas chaves de acesso de armazenamento. Chaves de acesso de armazenamento atualizado são necessárias pelos serviços de mídia para executar muitas tarefas na conta.

A seguir estão os principais cenários que resultariam em uma conta de serviços de mídia não ter acesso às contas de armazenamento anexado. 

|Problema|Solução|
|---|---|
|A conta de serviços de mídia ou a conta de armazenamento anexado (s) foram migrados para separar as assinaturas. |Migre a conta de serviços de mídia ou contas de armazenamento para que eles estejam todos na mesma assinatura. |
|A conta de serviços de mídia está usando uma conta de armazenamento anexado em uma assinatura diferente, como ele era uma conta de serviços de mídia antecipada em que isso tinha suporte. Todas as contas de serviços de mídia iniciais foram convertidas em contas de Gerenciador de recursos da Azure (ARM) com base em modernos e terão um estado desconectado. |Migre a conta de armazenamento ou a conta de serviços de mídia para que eles estejam todos na mesma assinatura.|

## <a name="next-steps"></a>Próximas etapas

Para saber como anexar uma conta de armazenamento para sua conta de Serviços de Mídia, consulte [Criar uma conta](create-account-cli-quickstart.md).

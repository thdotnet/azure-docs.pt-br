---
title: Terminologia de visualização de compartilhamento de dados do Azure
description: Terminologia de visualização de compartilhamento de dados do Azure
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 4e7db84666b9d3786c3fc25e3653d24d0b95f2e4
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789222"
---
# <a name="azure-data-share-preview-concepts"></a>Conceitos de visualização de compartilhamento de dados do Azure 

Visualização de compartilhamento de dados do Azure apresenta algumas novas terminologias relacionadas ao compartilhamento de dados. Este artigo explica que alguns usados com frequência termos que você pode ver, usados em todo o serviço. 

## <a name="data-provider"></a>Provedor de dados

Um provedor de dados é a organização que está compartilhando dados com seus consumidores. Normalmente, o provedor de dados pode ser um proprietário ou um curador dos dados. Provedores de dados deseja compartilhar dados de vários tipos. Alguns exemplos de dados que um provedor de dados pode desejar compartilhar dados brutos, como o ponto de vendas ou dados de série temporal. Um provedor de dados talvez queira compartilhar dados pré-processada e estruturados que já contém análises e informações. 

## <a name="data-consumer"></a>Consumidor de dados 

Um consumidor de dados é a organização que está recebendo dados de um provedor de dados. O consumidor de dados pode querer unir os dados compartilhados com seus próprios dados para derivar insights. Em alguns casos, o consumidor de dados pode estar recebendo dados que já foi processados. 

## <a name="data-share"></a>Compartilhamento de dados

Um compartilhamento de dados é um grupo de conjuntos de dados que são compartilhados como uma única entidade. Conjuntos de dados podem ser de um número de fontes de dados do Azure que são compatíveis com o compartilhamento de dados do Azure. Atualmente, o compartilhamento de dados do Azure dá suporte ao armazenamento de BLOBs do Azure e o Azure Data Lake Store. 

## <a name="share-subscription"></a>Assinatura de compartilhamento 

Uma assinatura de compartilhamento é criada quando um consumidor de dados aceita um convite de compartilhamento de dados de um provedor de dados. Provedores de dados podem exibir as assinaturas de compartilhamento ativos, navegando até **enviados compartilhamentos** em seus dados do Azure compartilham a conta e selecionando **compartilhar assinaturas**.

Um consumidor de dados pode verificar se eles têm uma assinatura de compartilhamento ativos, navegando até **compartilhamentos recebida** e exibindo o status de seus compartilhamentos recebidos. 

## <a name="snapshot"></a>Instantâneo

Um instantâneo pode ser criado por um consumidor de dados quando eles aceitarem um convite de compartilhamento de dados. Quando eles aceitarem um convite, elas podem disparar um instantâneo completo dos dados compartilhados com eles. O instantâneo é uma cópia dos dados no ponto no tempo que o consumidor de dados gerado o instantâneo. 

Há dois tipos de instantâneos - completos e incrementais. Um instantâneo completo contém todos os dados dentro do compartilhamento de dados. Um instantâneo incremental contém todos os dados que foram atualizadas/adicionadas desde o último instantâneo foi disparado. 

## <a name="snapshot-settings-in-azure-data-share"></a>Configurações de instantâneo no compartilhamento de dados do Azure
 
Um provedor de dados pode habilitar uma configuração de instantâneo para um compartilhamento de dados. Essa configuração permite que os consumidores de dados receber atualizações incrementais conforme elas ocorrem. Essa configuração deve ser habilitada se o provedor de dados deseja ter seus consumidores de dados para receber atualizações aos dados que foi compartilhados. 

Se um provedor de dados habilita essa configuração, um intervalo de recorrência pode ser selecionado. O intervalo de recorrência pode ser por hora ou diária. 

Um consumidor de dados tem a opção de participar dessa agenda de instantâneo para receber atualizações incrementais, que inclui todos os dados que foi alterado desde que eles geraram pela primeira vez um novo instantâneo. 

## <a name="invitation"></a>Convite

Um provedor de dados pode convidar vários destinatários para seu compartilhamento de dados. Eles podem fazer isso adicionando destinatários para o compartilhamento de dados. Convites também podem ser adicionados depois que um compartilhamento de dados foi criado. 

Um provedor de dados pode excluir um convite após ele ter sido enviado. Observe que, se um provedor de dados exclui um convite após ele ser aceito, o consumidor de dados ainda pode ter uma assinatura de compartilhamento ativos. Se o provedor de dados exclui um convite e ainda não foram aceitas, o consumidor de dados não poderá aceitá-la. 

## <a name="recipient"></a>Destinatário

Um destinatário é alguém que recebe um convite para um compartilhamento de dados. Normalmente, um provedor de dados irá adicionar destinatários para compartilhamento de dados criados por eles. Depois que o destinatário de um convite aceita o convite, eles se tornam um consumidor de dados.  

## <a name="next-steps"></a>Próximas etapas

Para saber como iniciar o compartilhamento de dados, continue com o [compartilhar seus dados](share-your-data.md) tutorial.


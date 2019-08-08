---
title: Introdução às Filas do Azure - Armazenamento do Azure
description: Introdução às Filas do Azure
author: mhopkins-msft
ms.author: mhopkins
ms.date: 06/07/2019
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: cbrooks
ms.openlocfilehash: 67e4874fcca93633140b7630ceadd273d1646f86
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721165"
---
# <a name="what-are-azure-queues"></a>O que são as Filas do Azure?

O Armazenamento de Filas do Azure é um serviço usado para armazenar grandes quantidades de mensagens. Você acessa as mensagens em qualquer lugar do mundo por meio de chamadas autenticadas usando HTTP ou HTTPS. Uma mensagem da fila pode ter até 64 KB. Uma fila pode conter milhões de mensagens, até o limite da capacidade total de uma conta de armazenamento.

## <a name="common-uses"></a>Usos comuns

Usos comuns de Armazenamento de filas incluem:

* Criar uma lista de pendências de trabalho para processar de maneira assíncrona
* Transmitir mensagens de uma função Web do Azure para uma função de Trabalho do Azure

## <a name="queue-service-concepts"></a>Conceitos do serviço Fila

O serviço Fila contém os seguintes componentes:

![Conceitos de fila](./media/storage-queues-introduction/queue1.png)

* **Formato de URL:** As consultas são endereçáveis usando o seguinte formato de URL:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    A URL a seguir endereça um fila no diagrama:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Conta de armazenamento:** Todo o acesso ao Armazenamento do Azure ocorre por meio de uma conta de armazenamento. Consulte [Escalabilidade e Metas de Desempenho do Armazenamento do Azure](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) para obter detalhes sobre a capacidade da conta de armazenamento.

* **Fila:** Uma fila contém um conjunto de mensagens. O nome da fila **precisa** estar em letras minúsculas. Para saber mais sobre filas de nomenclatura, confira [Nomenclatura de filas e metadados](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **Mensagem:** Uma mensagem, em qualquer formato, de até 64 KB. Antes da versão 2017-07-29, a vida útil máxima permitida era de sete dias. Para a versão 2017-07-29 ou posterior, a vida útil máxima pode ser qualquer número positivo ou -1, indicando que a mensagem não expira. Se esse parâmetro for omitido, a vida útil padrão será de sete dias.

## <a name="next-steps"></a>Próximas etapas

* [Criar uma conta de armazenamento](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Introdução às filas usando .NET](storage-dotnet-how-to-use-queues.md)

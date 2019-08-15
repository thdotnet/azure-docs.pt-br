---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/09/2019
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: 03674a51566ab89791725d1a51c7af12ed6949e5
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952915"
---
Para ler dados no armazenamento de arquivo morto, primeiro você deve alterar a camada do blob para quente ou fria. Esse processo é conhecido como reidratação e pode levar horas para ser concluído. Recomendamos tamanhos de blob grandes para o desempenho ideal de reidratação. Reidratar vários blobs pequenos simultaneamente pode inserir um tempo adicional. Atualmente, há duas prioridades de reidratar, alta (visualização) e padrão, que podem ser definidas por meio da propriedade *x-MS-reidratar-Priority* opcional em uma operação de [definição](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) de BLOB ou de [cópia](https://docs.microsoft.com/rest/api/storageservices/copy-blob) de BLOB.

* **Prioridade padrão**: A solicitação reidratação será processada na ordem em que foi recebida e pode levar até 15 horas.
* **Prioridade alta (visualização)** : A solicitação reidratação será priorizada em relação às solicitações padrão e poderá ser concluída em menos de uma hora. A alta prioridade pode levar mais de 1 hora, dependendo do tamanho do blob e da demanda atual. As solicitações de alta prioridade têm garantia de serem priorizadas em relação às solicitações de prioridade padrão.

> [!NOTE]
> A prioridade padrão é a opção reidratação padrão para arquivamento. Alta prioridade é uma opção mais rápida que custará mais do que a reidratação de prioridade padrão e geralmente é reservada para uso em situações de restauração de dados de emergência.

Durante a reidratação, você pode marcar a propriedade de blob *Status do arquivo* para confirmar se a camada foi alterada. O status exibe "reidratação pendentes para camada quente" ou "reidratação pendente para camada fria" dependendo da camada de destino. Após a conclusão, a propriedade status do arquivo é removida, e a propriedade de blob de *Nível de Acesso* reflete a nova camada frequente ou esporádica.

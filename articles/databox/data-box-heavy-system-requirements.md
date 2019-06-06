---
title: Requisitos de sistema pesadas de caixa de dados do Microsoft Azure | Microsoft Docs
description: Saiba mais sobre o software e requisitos de rede para seu pesada de caixa de dados do Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: article
ms.date: 05/22/2019
ms.author: alkohli
ms.openlocfilehash: b9e249885bd0e930773d4b374f85d72e60abdbdc
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427748"
---
# <a name="azure-data-box-heavy-system-requirements-preview"></a>Requisitos de sistema pesadas de caixa de dados do Azure (visualização)

Este artigo descreve os requisitos de sistema importantes para seu dispositivo pesada de caixa de dados do Azure e para clientes que se conectam ao dispositivo. Recomendamos que você examine as informações com atenção antes de implantar seu pesada de caixa de dados e, em seguida, voltar a ela conforme necessário durante a implantação e operação subsequente.

Os requisitos do sistema incluem:

* **Requisitos de software para hosts que se conectam a dados caixa pesada** -descreve as plataformas com suporte, navegadores da Web local da interface do usuário, os clientes SMB e quaisquer requisitos adicionais para hosts que podem se conectar à caixa de dados.
* **Requisitos de rede para os dados de caixa pesada** -fornece informações sobre os requisitos de rede para a operação ideal do dispositivo pesada de caixa de dados.

## <a name="software-requirements"></a>Requisitos de software

Os requisitos de software incluem as informações sobre os sistemas operacionais com suporte, os navegadores com suporte para a interface do usuário Web local e os clientes SMB.

### <a name="supported-operating-systems-for-clients"></a>Sistemas operacionais compatíveis para clientes

[!INCLUDE [data-box-supported-os-clients](../../includes/data-box-supported-os-clients.md)]

### <a name="supported-file-systems-for-linux-clients"></a>Sistemas de arquivos com suporte para clientes Linux

[!INCLUDE [data-box-supported-file-systems-clients](../../includes/data-box-supported-file-systems-clients.md)]

### <a name="supported-storage-accounts"></a>Contas de armazenamento com suporte

[!INCLUDE [data-box-supported-storage-accounts](../../includes/data-box-supported-storage-accounts.md)]

### <a name="supported-storage-types"></a>Tipos de armazenamento com suporte

[!INCLUDE [data-box-supported-storage-types](../../includes/data-box-supported-storage-types.md)]

### <a name="supported-web-browsers"></a>Navegadores da Web com suporte

[!INCLUDE [data-box-supported-web-browsers](../../includes/data-box-supported-web-browsers.md)]

## <a name="networking-requirements"></a>Requisitos de rede

Seu datacenter deve ter uma rede de alta velocidade. Para velocidades de cópia mais rápidas, duas conexões de 40 GbE podem ser utilizadas em paralelo (uma por nó). Se você não tenha 40-GbE disponível, recomendamos que você tenha pelo menos duas conexões de 10 GbE (uma por nó).

## <a name="next-steps"></a>Próximas etapas

* [Implante seu Azure Data Box](data-box-deploy-ordered.md)

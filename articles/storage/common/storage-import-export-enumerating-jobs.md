---
title: Listar todos os trabalhos de Importação/Exportação do Azure | Microsoft Docs
description: Saiba como listar todos os trabalhos do serviço de Importação/Exportação do Azure em uma assinatura.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 0ae9e7fa76c8ecbb724cf0f494e648df989dff30
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478684"
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a>Enumerando trabalhos no serviço de Importação/Exportação do Azure
Para enumerar todos os trabalhos em uma assinatura, chame a operação [List Jobs](/rest/api/storageimportexport/jobs). `List Jobs` retorna uma lista de trabalhos, bem como os seguintes atributos:

-   O tipo de trabalho (Importação ou Exportação)

-   O estado atual do trabalho

-   A conta de armazenamento associada ao trabalho

## <a name="next-steps"></a>Próximas etapas

* [Usando a API REST do serviço de Importação/Exportação](storage-import-export-using-the-rest-api.md)

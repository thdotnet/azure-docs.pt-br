---
title: O que aconteceu ao meu projeto do WebJob (serviço conectado do Armazenamento do Azure do Visual Studio)? | Microsoft Docs
description: Descreve o que aconteceu em um projeto do Azure WebJob depois de se conectar a uma conta de armazenamento usando os serviços conectados do Visual Studio
services: storage
author: ghogen
manager: jillfra
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: c54214e0b919ddaa60403a1c986a144100143577
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510464"
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>O que aconteceu ao meu projeto do WebJob (serviço conectado do Armazenamento do Azure do Visual Studio)?
## <a name="references-added"></a>Referências adicionadas
O pacote do NuGet do Armazenamento do Azure foi adicionado ao seu projeto do Visual Studio.  
Esse pacote adiciona as referências de .NET a seguir:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Cadeia de conexão do Armazenamento do Azure adicionada
No arquivo App.config do seu projeto, as entradas **AzureWebJobsStorage** e **AzureWebJobsDashboard** foram atualizadas com a cadeia de conexão e a chave da conta de armazenamento selecionada.

Para sabe r mais, consulte [Recursos de documentação de WebJobs do Azure](https://go.microsoft.com/fwlink/?linkid=390226).


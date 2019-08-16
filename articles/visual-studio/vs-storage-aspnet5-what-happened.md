---
title: O que aconteceu com meu projeto do ASP.NET 5 (serviços conectados do Visual Studio) | Microsoft Docs
description: Descreve o que acontece após a conexão a uma conta de armazenamento do Azure em um projeto do ASP.NET 5 do Visual Studio usando os serviços conectados do Visual Studio
services: storage
author: ghogen
manager: jillfra
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: e29c0302ecd703cb02199df95892e24917baf8e8
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510764"
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>O que aconteceu com meu projeto do ASP.NET 5 (serviços conectados do Armazenamento do Azure do Visual Studio)?
## <a name="references-added"></a>Referências adicionadas
O pacote do NuGet do armazenamento do Azure foi adicionado ao seu projeto do Visual Studio.  
Esse pacote adiciona as referências de .NET a seguir:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

Além disso, o pacote NuGet **Microsoft.Framework.Configuration.Json** foi adicionado.

## <a name="connection-string-for-azure-storage-added"></a>Cadeia de conexão do Armazenamento do Azure adicionada
No arquivo config.json do seu projeto, foi criado um elemento com a cadeia de conexão e chave da conta de armazenamento selecionada.

Para obter mais informações, consulte [ASP.NET 5](https://www.asp.net/vnext).


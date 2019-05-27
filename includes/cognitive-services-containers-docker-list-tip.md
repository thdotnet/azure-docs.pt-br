---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 02/11/2019
ms.openlocfilehash: 480cec39e42e075ad83ad3e32b88c638094db64f
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124279"
---
> [!TIP]
> Você pode usar o comando [imagens do estivador](https://docs.docker.com/engine/reference/commandline/images/) para listar as imagens do contêiner transferidas por download. Por exemplo, o comando a seguir lista o ID, o repositório e a tag de cada imagem do contêiner transferida por download, formatada como uma tabela:
>
>  ```
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>
>  IMAGE ID            REPOSITORY              TAG
>  ebbee78a6baa       <container-name>         latest
>  ``` 
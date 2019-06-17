---
title: Acessar e personalizar o novo portal do desenvolvedor - gerenciamento de API do Azure | Microsoft Docs
description: Saiba como usar o novo portal do desenvolvedor no gerenciamento de API.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2019
ms.author: apimpm
ms.openlocfilehash: deb3fdf3069aaad4982d71806c209fe516e3c3d6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66743569"
---
# <a name="access-and-customize-the-new-developer-portal-in-azure-api-management"></a>Acessar e personalizar o novo portal do desenvolvedor no gerenciamento de API do Azure

Este artigo mostra como acessar o novo portal do desenvolvedor do gerenciamento de API. Ele orienta você por meio da experiência do editor visual - adicionando e editando conteúdo - bem como personalizar a aparência do site.

![Novo portal do desenvolvedor de gerenciamento de API](media/api-management-howto-developer-portal/cover.png)

## <a name="prerequisites"></a>Pré-requisitos

- Conclua o início rápido a seguir: [Criar uma instância do Gerenciamento de API do Azure](get-started-create-service-instance.md).
- Importar e publicar uma instância de gerenciamento de API do Azure. Para obter mais informações, consulte [Importar e publicar](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

> [!NOTE]
> O novo portal do desenvolvedor está atualmente em visualização.

## <a name="managed-and-self-hosted-versions"></a>Versões de auto-hospedadas e gerenciadas

Você pode compilar seu portal do desenvolvedor de duas maneiras:

- **Versão gerenciada** - por edição e personalizando o portal construído em sua instância de gerenciamento de API e acessível por meio da URL `<your-api-management-instance-name>.developer.azure-api.net`.
- **Versão auto-hospedado** – com a implantação e hospedagem interna de seu portal fora de uma instância de gerenciamento de API. Essa abordagem permite que você edite o portal codebase e estender a funcionalidade básica fornecida. Para obter detalhes e instruções, consulte a [repositório GitHub com o código-fonte do portal][1].

## <a name="access-the-managed-version-of-the-portal"></a>Acessar a versão gerenciada do portal

Siga as etapas abaixo para acessar a versão gerenciada do portal.

1. Vá para sua instância de serviço de gerenciamento de API no portal do Azure.
1. Clique no **novo portal do desenvolvedor (visualização)** botão na barra de navegação superior. Um nova guia do navegador com uma versão administrativa do portal será aberto. Se você estiver acessando o portal pela primeira vez, o conteúdo padrão será provisionado automaticamente.

## <a name="edit-and-customize-the-managed-version-of-the-portal"></a>Editar e personalizar a versão gerenciada do portal

O vídeo abaixo, demonstramos como editar o conteúdo do portal, personalize a aparência do site da Web e publicar as alterações.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre o novo portal do desenvolvedor:

- [Repositório GitHub com o código-fonte][1]
- [Instruções sobre a hospedagem interna do portal][2]
- [Roadmap público do projeto][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects
---
title: Configurar um nome de domínio personalizado para a sua instância de Gerenciamento de API do Azure | Microsoft Docs
description: Este tópico descreve como configurar um nome de domínio personalizado para sua instância de Gerenciamento de API do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 07/01/2019
ms.author: apimpm
ms.openlocfilehash: 59b44dcc9ec3a1f7c274f426a19aa8ed2258db3e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509303"
---
# <a name="configure-a-custom-domain-name"></a>Configurar um nome de domínio personalizado

Quando você cria uma instância do serviço de gerenciamento de API do Azure, o Azure atribui a ele um subdomínio do Azure-API.NET (por exemplo, `apim-service-name.azure-api.net`). No entanto, você pode expor seus pontos de extremidade de gerenciamento de API usando seu próprio nome de domínio personalizado, como **contoso.com**. Este tutorial mostra como mapear um nome DNS personalizado existente para pontos de extremidade expostos por uma instância de gerenciamento de API.

> [!WARNING]
> Clientes que desejam usar a anexação de certificado para aumentar a segurança de seus aplicativos devem usar um nome de domínio personalizado > e o certificado que gerenciam, não o padrão. Clientes que fixam o certificado padrão > assumirão uma dependência forte das propriedades do certificado que não controlam, o que não é uma prática recomendada.

## <a name="prerequisites"></a>Pré-requisitos

Para executar as etapas descritas neste artigo, você precisa ter:

-   Uma assinatura ativa do Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

-   Uma instância de gerenciamento de API. Para obter mais informações, consulte [Criar uma instância do Gerenciamento de API do Azure](get-started-create-service-instance.md).
-   Um nome de domínio personalizado que pertence a você. O nome de domínio personalizado que você deseja usar deve ser adquirido separadamente e hospedado em um servidor DNS. Este tópico não dá instruções sobre como hospedar um nome de domínio personalizado.
-   Você deve ter um certificado válido com chaves pública e privada (.PFX). Assunto ou nome alternativo da entidade (SAN) deve corresponder ao nome de domínio (Isso habilita a instância de gerenciamento de API para expor com segurança URLs sobre SSL).

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Usar o portal do Azure para definir um nome de domínio personalizado

1. Navegue até sua instância de gerenciamento de API na [portal do Azure](https://portal.azure.com/).
1. Clique em **Domínios personalizados e SSL**.

    Há vários pontos de extremidade ao qual você pode atribuir um nome de domínio personalizado. No momento, os seguintes pontos de extremidade estão disponíveis:

    - **Proxy** (o padrão é: `<apim-service-name>.azure-api.net`),
    - **Portal** (o padrão é: `<apim-service-name>.portal.azure-api.net`),
    - **Gerenciamento** (o padrão é: `<apim-service-name>.management.azure-api.net`),
    - **SCM** (o padrão é: `<apim-service-name>.scm.azure-api.net`).

    > [!NOTE]
    > Você pode atualizar todos os pontos de extremidade ou alguns deles. Normalmente, os clientes atualizam **Proxy** (essa URL é usada para chamar a API exposta por meio do Gerenciamento de API) e **Portal** (URL do portal do desenvolvedor). **Gerenciamento** e **SCM** pontos de extremidade são usados internamente pelo somente os proprietários de instância de gerenciamento de API e, portanto, com menos frequência recebem um nome de domínio personalizado. Na maioria dos casos, somente um nome único de domínio personalizado pode ser definido para um determinado ponto de extremidade. No entanto, o **Premium** camada dá suporte à configuração de vários nomes de host para o **Proxy** ponto de extremidade.

1. Selecione o ponto de extremidade que você deseja atualizar.
1. Na janela à direita, clique em **Personalizado**.

    - Em **Nome de domínio personalizado**, especifique o nome que você deseja usar. Por exemplo: `api.contoso.com`. Nomes de domínio curinga (por exemplo, \*. domain.com) também têm suporte.
    - No **certificado**, selecione um certificado do Cofre de chaves. Você também pode carregar um válido. PFX do arquivo e forneça sua **senha**, se o certificado é protegido com uma senha.

    > [!TIP]
    > É recomendável usar o Azure Key Vault para gerenciar certificados e defini-los como autorotate.
    > Se você usar o Azure Key Vault para gerenciar o certificado SSL de domínio personalizado, verifique se o certificado é inserido no cofre de chaves [como um _certificado_](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate), e não um _segredo_.
    >
    > Para buscar um certificado SSL, o gerenciamento de API deve ter a lista de permissões de segredos um get no cofre de chaves do Azure que contém o certificado. Ao usar o portal do Azure, que todas as etapas de configuração necessárias serão concluídas automaticamente. Ao usar as ferramentas de linha de comando ou a API de gerenciamento, essas permissões devem ser concedidas manualmente. Isso é feito em duas etapas. Primeiro, use a página de identidades gerenciado em sua instância de gerenciamento de API Certifique-se de que a identidade gerenciada está habilitada e anote a id da entidade mostrada nessa página. Em segundo lugar, dar a lista de permissão e obter permissões de segredos para essa id de entidade de segurança no Azure Key Vault que contém o certificado.
    >
    > Se o certificado for definido como autorotate, gerenciamento de API selecionará a versão mais recente automaticamente sem qualquer tempo de inatividade para o serviço (se sua camada de gerenciamento de API tem SLA - e. de i. em todas as camadas, exceto a camada de desenvolvedor).

1. Clique em Aplicar.

    > [!NOTE]
    > O processo de atribuição do certificado pode levar aproximadamente 15 minutos dependendo do tamanho da implantação. O SKU de desenvolvedor tem tempo de inatividade, SKU Básico e posteriores não têm tempo de inatividade.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Próximas etapas

[Atualizar e colocar em escala o serviço](upgrade-and-scale.md)

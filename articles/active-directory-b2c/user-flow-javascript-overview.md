---
title: JavaScript e a página Contrato versões - Azure Active Directory B2C | Microsoft Docs
description: Saiba como habilitar o JavaScript e usar as versões de contrato de página no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: ef474bec71a9015209b5748b6947816002bd4a5d
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511970"
---
# <a name="javascript-and-page-contract-versions-in-azure-active-directory-b2c"></a>Versões de contrato de página e JavaScript no Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

B2C do AD do Azure fornece um conjunto de conteúdo do pacote que contém o HTML, CSS e JavaScript para os elementos de interface do usuário em seus fluxos de usuário e políticas personalizadas. Para habilitar o JavaScript para seus aplicativos, você deve adicionar um elemento a sua [política personalizada](active-directory-b2c-overview-custom.md) ou habilitá-lo no portal para fluxos de usuário, selecione um contrato de página e use [b2clogin.com](b2clogin.md) nas suas solicitações.

Se você pretende habilitar [JavaScript](javascript-samples.md) código do lado do cliente, você deve ter certeza de que os elementos que você estiver baseando o JavaScript em são imutáveis. Caso contrário, todas as alterações podem provocar comportamento inesperado em suas páginas de usuário. Para evitar esses problemas, você pode impor o uso de um contrato de página e especificar uma versão de contrato de página. Isso garante que todas as definições de conteúdo que você já baseia seu JavaScript são imutáveis. Mesmo se você não pretende habilitar o JavaScript, você pode especificar uma versão de contrato de página para todas as páginas.

## <a name="user-flows"></a>Fluxos de usuário

Nas propriedades de fluxo de usuário, você pode habilitar o JavaScript, o que também impõe o uso de um contrato de página. Em seguida, você pode definir a versão do contrato de página conforme descrito na próxima seção.

![Habilitar configuração de JavaScript](media/user-flow-javascript-overview/javascript-settings.png)

Independentemente de pretender ou não habilitar o JavaScript nas propriedades do um fluxo de usuário, você poderá especificar uma versão de contrato de página para as páginas desse fluxo. Abra o fluxo de usuário e selecione **Layouts de página**. Em **Nome do Layout**, selecione uma página de fluxo de usuário e escolha o **Página de Versão do Contrato**.

![Habilitar configuração de JavaScript](media/user-flow-javascript-overview/page-contract-version.png)

## <a name="custom-policies"></a>Políticas personalizadas

Para habilitar o JavaScript em políticas personalizadas, você deve adicionar o **ScriptExecution** elemento para o **RelyingParty** elemento no arquivo de política personalizada. Para obter mais informações, consulte [exemplos de JavaScript para uso no Azure Active Directory B2C](javascript-samples.md).

Se você habilitar o JavaScript em suas políticas personalizadas, você pode especificar uma versão de contrato de página para todas as páginas. Para obter mais informações sobre como especificar um contrato de página, consulte [selecione um contrato de página no Azure Active Directory B2C usando políticas personalizadas](page-contract.md).

## <a name="next-steps"></a>Próximas etapas

Confira os [Exemplos de JavaScript para uso no Azure Active Directory B2C](javascript-samples.md).

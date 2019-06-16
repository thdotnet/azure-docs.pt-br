---
title: Perguntas Frequentes de configuração de aplicativo do Azure | Microsoft Docs
description: Perguntas frequentes sobre a configuração de aplicativo do Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: e321c0b473b110597b5b87a6e67666737116daa2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393342"
---
# <a name="azure-app-configuration-faq"></a>Perguntas Frequentes de configuração de aplicativo do Azure

Este artigo aborda perguntas frequentes sobre a configuração de aplicativo do Azure.

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>Como a Configuração de Aplicativos é diferente do Azure Key Vault?

Configuração do aplicativo foi projetada para um conjunto distinto de casos de uso: ele ajuda os desenvolvedores a gerenciar as configurações do aplicativo e controlar a disponibilidade de recursos. Ele tem como objetivo simplificar muitas das tarefas de trabalhar com dados de configuração complexas.

Dá suporte à configuração de aplicativo:

- Namespaces hierárquicos
- Rotulação
- Consultas amplas
- Recuperação de lote
- Operações de gerenciamento especializado
- Uma interface de usuário de gerenciamento de recurso

Configuração de aplicativo é um complementa para o Cofre de chaves e os dois devem ser usados lado a lado na maioria das implantações de aplicativo.

## <a name="should-i-store-secrets-in-app-configuration"></a>Deve armazenar segredos na configuração de aplicativo?

Embora a configuração de aplicativo fornece segurança robusta, o Cofre de chaves ainda é o melhor lugar para armazenar segredos do aplicativo. Key Vault fornece criptografia em nível de hardware, políticas de acesso granular e operações de gerenciamento, como rotação de certificado.

## <a name="does-app-configuration-encrypt-my-data"></a>Configuração do aplicativo criptografar Meus dados?

Sim. Configuração de aplicativo criptografa todos os valores de chave, que ele contém, e ela criptografa a comunicação de rede. Nomes de chave são usados como índices para recuperar dados de configuração e não são criptografados.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>Como devo armazenar configurações para vários ambientes (teste, preparo, produção e assim por diante)?

No momento, você pode controlar quem tem acesso à configuração de aplicativo em um nível por repositório. Use um armazenamento separado para cada ambiente que exige permissões diferentes. Essa abordagem lhe dá o melhor isolamento de segurança.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>Quais são as maneiras recomendadas para usar a configuração de aplicativo?

Ver [práticas recomendadas](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>Quanto custa a configuração de aplicativo?

O serviço está livre para usar durante a visualização pública.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>Como relatar um problema ou fornecer uma sugestão?

Você pode entrar em contato conosco diretamente na [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Próximas etapas

* [Sobre a configuração de aplicativo do Azure](./overview.md)

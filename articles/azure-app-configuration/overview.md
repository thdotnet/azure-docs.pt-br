---
title: O que é Configuração de Aplicativo do Azure? | Microsoft Docs
description: Uma visão geral do serviço Configuração de Aplicativo do Azure.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: a76eab5d51dd73fb6b38ebebaa8421e789274f84
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59994785"
---
# <a name="what-is-azure-app-configuration"></a>O que é Azure App Configuration?

O Azure App Configuration fornece um serviço para gerenciar centralmente as configurações de aplicativo. Programas modernos, especialmente programas executando em uma nuvem, geralmente possuem muitos componentes que são distribuídos por natureza. A distribuição das definições de configuração entre esses componentes pode levar a erros difíceis de serem resolvidos durante uma implantação de aplicativo. Use a Configuração de Aplicativo para armazenar todas as configurações do aplicativo e proteger os acessos em um só lugar.

O App Configuration é gratuito durante o período de avaliação. Se você quiser experimentar, [registre-se](https://aka.ms/azconfig/register) na versão de avaliação.

## <a name="why-use-app-configuration"></a>Por que usar o App Configuration?

Os aplicativos baseados em nuvem geralmente são executados em várias máquinas virtuais ou contêineres em várias regiões e usam diversos serviços externos. Criar um aplicativo distribuído desse tipo, robusto e escalonável, é um desafio. 

Várias metodologias de programação ajudam os desenvolvedores a lidar com a crescente complexidade da criação de aplicativos. Por exemplo, o aplicativo de 12 fatores descreve muitos padrões de arquitetura bem testados e melhores práticas para uso com aplicativos em nuvem. Uma recomendação básica deste guia é separar a configuração do código. Nesse caso, as configurações de um aplicativo devem ser mantidas externas a seu executável e lidas a partir de seu ambiente de tempo de execução ou de uma fonte externa.

Embora qualquer aplicativo possa fazer uso do App Configuration, os exemplos a seguir são os tipos de aplicativo que se beneficiam desse uso:

* Microsserviços baseados no Azure Kubernetes Service, Azure Service Fabric ou em outros aplicativos em contêiner implantados em uma ou mais geografias
* Aplicativos serverless, que incluem o Azure Functions ou outros aplicativos de computação sem estado baseados em evento
* Pipeline de implantação contínua

O App Configuration oferece os seguintes benefícios:

* Um serviço totalmente gerenciado que pode ser configurado em minutos
* Representações e mapeamentos de chave flexíveis
* Marcação com rótulos
* Reprodução pontual de configurações
* Comparação de dois conjuntos de configurações em dimensões com definições personalizadas
* Segurança aprimorada por meio de identidades gerenciadas do Azure
* Criptografias de dados completas, em repouso ou em trânsito
* Integração nativa com estruturas populares

O App Configuration complementa o [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), que é usado para armazenar segredos do aplicativo. O App Configuration facilita a implementação dos seguintes cenários:

* Gerenciamento centralizado e distribuição de dados de configuração hierárquica para diferentes ambientes e geografias
* Alterações na configuração dinâmica sem a necessidade de reimplementar ou reiniciar um aplicativo
* Gerenciamento de recursos

## <a name="use-app-configuration"></a>Usar Configuração de Aplicativo

A maneira mais fácil de adicionar um repositório de configurações de aplicativo ao seu aplicativo é por meio de uma biblioteca de clientes fornecida pela Microsoft. Com base na linguagem e estrutura de programação, os melhores métodos a seguir estão disponíveis para você.

| Estrutura e linguagem de programação | Como conectar-se |
|---|---|
| .NET Core e ASP.NET Core | Provedor de Configuração de Aplicativo para .NET Core |
| .NET e ASP.NET | Construtor de Configuração de Aplicativo para .NET |
| Java Spring | Cliente de Configuração de Aplicativo para Spring Cloud |
| Outros | API REST de Configuração de Aplicativo |

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um aplicativo Web ASP.NET Core](./quickstart-aspnet-core-app.md)  

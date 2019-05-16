---
title: Gerenciamento de recursos da Configuração de Aplicativos do Azure | Microsoft Docs
description: Uma visão geral de como a Configuração de Aplicativos do Azure pode ser usada para ativar e desligar recursos de aplicativo sob demanda
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 304ca08275ccf4bf32d38e3fc89dd5cf07460fa4
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413776"
---
# <a name="feature-management"></a>Gerenciamento de recursos

Tradicionalmente, o envio de um novo recurso de aplicativo exige uma reimplantação completa do aplicativo em si. Para testar um recurso, provavelmente, você precisará implantar seu aplicativo muitas vezes no controle quando ele se tornar visível ou quando alguém puder vê-lo. O gerenciamento de recursos é uma prática moderna de desenvolvimento de software que separa a liberação do recurso da implantação do código e permite alterações rápidas na disponibilidade de recursos sob demanda. Ele usa uma técnica chamada *sinalizador de recurso* (também conhecido como *alternância de funcionalidades*, *opção de recurso* e assim por diante) para administrar o ciclo de vida de um recurso dinamicamente. Ele ajuda os desenvolvedores a lidar com os seguintes problemas:

* **Gerenciamento de branch de código**: Os sinalizadores de recurso são frequentemente usados para encapsular uma nova funcionalidade do aplicativo que está em desenvolvimento. Essa funcionalidade é "oculta" por padrão. Você pode enviá-la com segurança, embora não esteja concluída, que ela permanecerá inativa em produção. Usando essa abordagem, chamada *implantação escura*, você pode liberar todo o código no final de cada ciclo de desenvolvimento. Você não precisa manter nenhum branch de código em vários ciclos devido a um recurso levar mais de um período para ser concluído.
* **Teste em produção**: Você pode usar os sinalizadores de recurso para permitir acesso antecipado a uma nova funcionalidade em produção. Por exemplo, limite esse acesso apenas aos membros da equipe ou aos testadores beta internos. Esses usuários obterão a experiência de produção com fidelidade total, em vez de uma experiência simulada ou parcial em um ambiente de teste.
* **Liberação de versões de pré-lançamento**: Você também pode usar os sinalizadores de recurso para distribuir uma nova funcionalidade aos usuários finais de forma incremental. Direcione um pequeno percentual da população de usuários primeiro e aumente esse percentual gradualmente com o tempo, depois de ganhar mais confiança na implementação.
* **Comutador de encerramento instantâneo**: Os sinalizadores de recurso fornecem uma rede de segurança inerente para liberação de uma nova funcionalidade. Com eles, você pode não só ativar, mas também desligar recursos de aplicativo imediatamente. Você obtém a capacidade de desabilitar rapidamente um recurso, se necessário, sem a necessidade de recompilar e reimplantar seu aplicativo.
* **Ativação seletiva**: Embora a maioria dos sinalizadores de recurso exista apenas até suas funcionalidades associadas serem liberadas com êxito, alguns podem durar por um longo tempo. Você pode usá-los como uma maneira de segmentar os usuários e fornecer um conjunto específico de recursos para cada grupo. Por exemplo, você pode ter um recurso que funciona em determinado navegador da Web. Defina um sinalizador de recurso, de modo que somente os usuários desse navegador possam ver e usar o recurso. A vantagem dessa abordagem é que você pode expandir com facilidade a lista de navegadores compatíveis posteriormente sem precisar alterar nenhum código.

## <a name="basic-concepts"></a>Conceitos básicos

Veja a seguir vários novos termos relacionados ao gerenciamento de recursos.

* **Sinalizador de recurso**: Um sinalizador de recurso é uma variável com um estado binário igual a *ativado* ou *desativado* e que tem um bloco de código associado. Seu estado decide se o bloco de código é executado ou não.
* **Gerenciador de recursos**: Um gerenciador de recursos é um pacote de aplicativos que cuida do ciclo de vida de todos os sinalizadores de recursos em um aplicativo. Normalmente, ele fornece funcionalidades adicionais, como cache de sinalizadores e atualização de seus estados.
* **Filtro**: Um filtro é uma regra para avaliação do estado de um sinalizador de recurso. Grupo de usuários, tipo de dispositivo ou navegador, geolocalização, janela de tempo são todos exemplos do que pode representar um filtro.

Uma implementação eficaz do gerenciamento de recursos consiste em, pelo menos, dois componentes que funcionam em conjunto: um aplicativo que faz uso de sinalizadores de recursos e um repositório separado que armazena os sinalizadores de recursos e seus estados atuais. Suas interações são ilustradas abaixo.

## <a name="feature-flag-usage-in-code"></a>Uso de sinalizador de recurso no código

O padrão básico para implementar sinalizadores de recurso em um aplicativo é simples. Você pode conceitualizá-lo como uma variável de estado booliana usada com uma instrução condicional `if` no código.

```csharp
if (featureFlag) {
    // Run the following code
}
```

Nesse caso, se `featureFlag` é definido como `True`, o bloco de código embutido é executado; caso contrário, ele é ignorado. O valor de `featureFlag` pode ser definido estaticamente:

```csharp
bool featureFlag = true;
```

Ele também pode ser avaliado com base em determinadas regras:

```csharp
bool featureFlag = isBetaUser();
```

Um padrão de sinalizador de recurso um pouco mais complicado inclui uma instrução `else` também.

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

No entanto, isso pode ser reescrito no padrão básico. O artigo [Usar sinalizadores de recurso em um aplicativo ASP.NET Core](./use-feature-flags-dotnet-core.md) mostra a vantagem de padronização em um padrão de código simples.

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
}

if (!featureFlag) {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Repositório de sinalizadores de recurso

Para usá-los com eficiência, você precisará externalizar todos os sinalizadores de recurso usados em um aplicativo. Isso permitirá que você altere os estados de sinalizador de recurso sem modificar e reimplantar o aplicativo em si. A Configuração de Aplicativos do Azure foi projetada para ser um repositório centralizado de sinalizadores de recurso. Você pode utilizá-la para definir diferentes tipos de sinalizadores de recurso e manipular seus estados de maneira rápida e segura. Em seguida, você pode acessar esses sinalizadores de recurso com facilidade em seu aplicativo usando as bibliotecas da Configuração de Aplicativos para várias estruturas de linguagem de programação. O artigo [Usar sinalizadores de recursos em um aplicativo ASP.NET Core](./use-feature-flags-dotnet-core.md) mostra como o provedor da Configuração de Aplicativos do .NET Core e as bibliotecas do Gerenciamento de Recursos são usados juntos como uma solução completa para implementação de sinalizadores de recursos para seu aplicativo Web ASP.NET.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Adicionar sinalizadores de recurso a um aplicativo Web ASP.NET Core](./quickstart-feature-flag-aspnet-core.md)  

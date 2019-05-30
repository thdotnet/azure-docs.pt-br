---
title: Práticas recomendadas de configuração de aplicativo do Azure | Microsoft Docs
description: Saiba como usar melhor configuração de aplicativo do Azure
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
ms.openlocfilehash: 3d9a597e7ced631627a121f3f0757e472f9a4bae
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393585"
---
# <a name="azure-app-configuration-best-practices"></a>Práticas recomendadas de configuração de aplicativo do Azure

Este artigo discute práticas recomendadas e padrões comuns quando você estiver usando a configuração de aplicativo do Azure.

## <a name="key-groupings"></a>Agrupamentos de chave

Configuração de aplicativo fornece duas opções para organizar as chaves:

* Prefixos de chave
* Rótulos

Você pode usar uma ou ambas as opções para agrupar suas chaves.

*Prefixos de chave* são as partes de início de chaves. Logicamente, você pode agrupar um conjunto de chaves usando o mesmo prefixo em seus nomes. Prefixos podem conter vários componentes conectados por um delimitador, como `/`, semelhante a um caminho de URL, para formar um namespace. Essas hierarquias são úteis quando você estiver armazenando chaves para muitos aplicativos, serviços de componentes e ambientes em um repositório de configuração do aplicativo.

Uma coisa importante para ter em mente é que as chaves são o que referencia o código do aplicativo para recuperar os valores das configurações correspondentes. As chaves não devem alterar, caso contrário, você precisará modificar seu código sempre que ocorre.

*Rótulos* são um atributo em chaves. Eles são usados para criar variações de uma chave. Por exemplo, você pode atribuir rótulos para várias versões de uma chave. Uma versão pode ser uma iteração, um ambiente ou outras informações contextuais. O aplicativo pode solicitar um conjunto completamente diferente de valores de chave, especificando outro rótulo. Como resultado, todas as referências de chave permanecem inalteradas no seu código.

## <a name="key-value-compositions"></a>Composições de chave-valor

Configuração de aplicativo trata todas as chaves armazenadas com ele como entidades independentes. Configuração de aplicativo não tenta inferir qualquer relação entre as chaves ou para herdar valores de chave com base em sua hierarquia. Você pode agregar vários conjuntos de chaves, no entanto, usando rótulos juntamente com a configuração apropriada de empilhamento no código do aplicativo.

Vamos examinar um exemplo. Suponha que você tenha uma configuração denominada **Asset1**, cujo valor pode variar com base no ambiente de desenvolvimento. Você criar uma chave chamada "Asset1" com um rótulo vazio e um rótulo denominado "Desenvolvimento". No primeiro rótulo, você coloca o valor padrão para **Asset1**, e colocar um valor específico para o "Desenvolvimento" no segundo.

Em seu código, você recuperar os valores de chave sem nenhum rótulo e, em seguida, recuperar o mesmo conjunto de valores de chave de uma segunda vez com o rótulo de "Desenvolvimento". Quando você recupera os valores na segunda vez, os valores anteriores das chaves são substituídos. O sistema de configuração do .NET Core permite "empilhar" vários conjuntos de dados de configuração sobre cada um do outro. Se existir uma chave em mais de um conjunto, o último conjunto que o contém é usado. Com uma estrutura moderna de programação, como .NET Core, você obtém essa funcionalidade de empilhamento gratuitamente se você usar um provedor de configuração nativo para acessar a configuração do aplicativo. O trecho de código a seguir mostra como você pode implementar o empilhamento em um aplicativo .NET Core:

```csharp
// Augment the ConfigurationBuilder with Azure App Configuration
// Pull the connection string from an environment variable
configBuilder.AddAzureAppConfiguration(options => {
    options.Connect(configuration["connection_string"])
           .Use(KeyFilter.Any, LabelFilter.Null)
           .Use(KeyFilter.Any, "Development");
});
```

## <a name="app-configuration-bootstrap"></a>Inicialização de configuração de aplicativo

Para acessar o repositório de configuração de aplicativo, você pode usar sua cadeia de caracteres de conexão, que está disponível no portal do Azure. Como cadeias de caracteres de conexão contêm informações de credenciais, elas são consideradas segredos. Esses segredos precisam ser armazenados no cofre de chaves do Azure e seu código deve ser autenticado para o Key Vault para recuperá-los.

Uma opção melhor é usar o recurso de identidades gerenciadas no Azure Active Directory. Com identidades gerenciadas, você precisa apenas a URL de ponto de extremidade de configuração de aplicativo para acesso de inicialização em seu repositório de configuração do aplicativo. Você pode inserir a URL no código do aplicativo (por exemplo, o *appSettings. JSON* arquivo). Ver [integrar com o Azure identidades gerenciadas](howto-integrate-azure-managed-service-identity.md) para obter detalhes.

## <a name="app-or-function-access-to-app-configuration"></a>Aplicativo ou função de acesso à configuração de aplicativo

Você pode fornecer acesso à configuração de aplicativo para aplicativos web ou funções, usando qualquer um dos seguintes métodos:

* Por meio do portal do Azure, insira a cadeia de conexão em seu repositório de configuração do aplicativo nas configurações do aplicativo do serviço de aplicativo.
* Store a cadeia de caracteres de conexão para o repositório de configuração de aplicativo no cofre de chaves e [referenciá-lo no serviço de aplicativo](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references).
* Usar o Azure managed identidades para acessar o repositório de configuração do aplicativo. Para obter mais informações, consulte [integrar com o Azure identidades gerenciadas](howto-integrate-azure-managed-service-identity.md).
* Configuração de envio por push da configuração de aplicativo no serviço de aplicativo. Configuração de aplicativo fornece uma função de exportação (no portal do Azure e a CLI do Azure) que envia dados diretamente no serviço de aplicativo. Com esse método, você não precisa alterar o código do aplicativo em todos os.

## <a name="next-steps"></a>Próximas etapas

* [Chaves e valores](./concept-key-value.md)

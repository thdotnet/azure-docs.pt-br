---
title: Visão geral da definição de exibição em aplicativos gerenciados do Azure | Microsoft Docs
description: Descreve o conceito de criação de definição de exibição de aplicativos gerenciados do Azure.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 06/12/2019
ms.openlocfilehash: 6735787f9b43f98ab611584f3c7191c9f927dbc2
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478739"
---
# <a name="view-definition-artifact-in-azure-managed-applications"></a>Artefato de definição de exibição em aplicativos gerenciados do Azure

Definição da exibição é um artefato opcional em aplicativos gerenciados do Azure. Ele permite personalizar a página de visão geral e adicionar mais modos de exibição, como recursos personalizados e métricas.

Este artigo fornece uma visão geral de artefato de definição de exibição e seus recursos.

## <a name="view-definition-artifact"></a>Exibir artefato de definição

O artefato de definição de exibição deve ser nomeado **viewDefinition.json** e colocado no mesmo nível **Createuidefinition** e **Maintemplate** no. zip pacote que cria uma definição de aplicativo gerenciado. Para saber como criar o pacote. zip e publicar uma definição de aplicativo gerenciado, consulte [publica uma definição de aplicativo gerenciado do Azure](publish-managed-app-definition-quickstart.md)

## <a name="view-definition-schema"></a>Esquema de definição de exibição

O **viewDefinition.json** arquivo tem apenas um nível superior `views` propriedade, que é uma matriz de modos de exibição. Cada modo de exibição é mostrado na interface do usuário de aplicativo gerenciado como um item de menu separado na tabela de conteúdo. Cada exibição tem um `kind` propriedade que define o tipo da exibição. Ele deve ser definido como um dos seguintes valores: [Visão geral](#overview), [métricas](#metrics), [CustomResources](#custom-resources). Para obter mais informações, consulte o atual [esquema JSON para viewDefinition.json](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).

Exemplo de JSON para definição de exibição:

```json
{
    "views": [
        {
            "kind": "Overview",
            "properties": {
                "header": "Welcome to your Azure Managed Application",
                "description": "This managed application is for demo purposes only.",
                "commands": [
                    {
                        "displayName": "Test Action",
                        "path": "testAction"
                    }
                ]
            }
        },
        {
            "kind": "Metrics",
            "properties": {
                "displayName": "This is my metrics view",
                "version": "1.0.0",
                "charts": [
                    {
                        "displayName": "Sample chart",
                        "chartType": "Bar",
                        "metrics": [
                            {
                                "name": "Availability",
                                "aggregationType": "avg",
                                "resourceTagFilter": [ "tag1" ],
                                "resourceType": "Microsoft.Storage/storageAccounts",
                                "namespace": "Microsoft.Storage/storageAccounts"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "kind": "CustomResources",
            "properties": {
                "displayName": "Test custom resource type",
                "version": "1.0.0",
                "resourceType": "testCustomResource",
                "createUIDefinition": { },
                "commands": [
                    {
                        "displayName": "Custom Test Action",
                        "path": "testAction"
                    },
                    {
                        "displayName": "Custom Context Action",
                        "path": "testCustomResource/testContextAction",
                        "icon": "Stop",
                        "createUIDefinition": { },
                    }
                ],
                "columns": [
                    {"key": "name", "displayName": "Name"},
                    {"key": "properties.myProperty1", "displayName": "Property 1"},
                    {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
                ]
            }
        }
    ]
}

```

## <a name="overview"></a>Visão geral

`"kind": "Overview"`

Quando você fornece este modo de exibição **viewDefinition.json**, ela substitui a página de visão geral de padrão em seu aplicativo gerenciado.

```json
{
    "kind": "Overview",
    "properties": {
        "header": "Welcome to your Azure Managed Application",
        "description": "This managed application is for demo purposes only.",
        "commands": [
            {
                "displayName": "Test Action",
                "path": "testAction"
            }
        ]
    }
}
```

|Propriedade|Obrigatório|DESCRIÇÃO|
|---------|---------|---------|
|cabeçalho|Não|O cabeçalho da página de visão geral.|
|description|Não|A descrição do seu aplicativo gerenciado.|
|Comandos|Não|A matriz de botões de barra de ferramentas adicionais de página de visão geral, consulte [comandos](#commands).|

## <a name="metrics"></a>metrics

`"kind": "Metrics"`

A exibição de métricas permite que você coletar e agregar dados de seus recursos de aplicativo gerenciado no [métricas do Azure Monitor](../azure-monitor/platform/data-platform-metrics.md).

```json
{
    "kind": "Metrics",
    "properties": {
        "displayName": "This is my metrics view",
        "version": "1.0.0",
        "charts": [
            {
                "displayName": "Sample chart",
                "chartType": "Bar",
                "metrics": [
                    {
                        "name": "Availability",
                        "aggregationType": "avg",
                        "resourceTagFilter": [ "tag1" ],
                        "resourceType": "Microsoft.Storage/storageAccounts",
                        "namespace": "Microsoft.Storage/storageAccounts"
                    }
                ]
            }
        ]
    }
}
```

|Propriedade|Obrigatório|DESCRIÇÃO|
|---------|---------|---------|
|displayName|Não|O título exibido do modo de exibição.|
|version|Não|A versão da plataforma usada para renderizar a exibição.|
|Gráficos|Sim|A matriz de gráficos da página métricas.|

### <a name="chart"></a>Gráfico

|Propriedade|Obrigatório|DESCRIÇÃO|
|---------|---------|---------|
|displayName|Sim|O título exibido do gráfico.|
|chartType|Não|A visualização a ser usado para este gráfico. Por padrão, ele usa um gráfico de linhas. Suporte para tipos de gráfico: `Bar, Line, Area, Scatter`.|
|metrics|Sim|A matriz de métricas para plotar nesse gráfico. Para saber mais sobre métricas com suporte no portal do Azure, consulte [métricas com suporte no Azure Monitor](../azure-monitor/platform/metrics-supported.md)|

### <a name="metric"></a>Métrica

|Propriedade|Obrigatório|DESCRIÇÃO|
|---------|---------|---------|
|name|Sim|O nome da métrica.|
|aggregationType|Sim|O tipo de agregação a ser usado para essa métrica. Tipos de agregação com suporte: `none, sum, min, max, avg, unique, percentile, count`|
|namespace|Não|Informações adicionais para determinar o provedor de métricas correto.|
|resourceTagFilter|Não|Matriz de marcas de recurso (serão separados com `or` word) para as métricas que seria exibido. Aplica-se na parte superior do filtro de tipo de recurso.|
|resourceType|Sim|O tipo de recurso para o qual as métricas seriam exibidas.|

## <a name="custom-resources"></a>Recursos personalizados

`"kind": "CustomResources"`

Você pode definir vários modos de exibição desse tipo. Cada modo de exibição representa uma **exclusivos** tipo de recurso personalizado do provedor personalizado que você definiu na **Maintemplate**. Para obter uma introdução aos provedores personalizados, confira [Visão geral da Visualização de Provedores Personalizados do Azure](custom-providers-overview.md).

Nessa exibição, que você pode executar GET, PUT, excluir e postar operações para o tipo de recurso personalizado. Operações POST pode ser globais ações personalizadas ou ações personalizadas em um contexto de tipo de recurso personalizado.

```json
{
    "kind": "CustomResources",
    "properties": {
        "displayName": "Test custom resource type",
        "version": "1.0.0",
        "resourceType": "testCustomResource",
        "createUIDefinition": { },
        "commands": [
            {
                "displayName": "Custom Test Action",
                "path": "testAction"
            },
            {
                "displayName": "Custom Context Action",
                "path": "testCustomResource/testContextAction",
                "icon": "Stop",
                "createUIDefinition": { },
            }
        ],
        "columns": [
            {"key": "name", "displayName": "Name"},
            {"key": "properties.myProperty1", "displayName": "Property 1"},
            {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
        ]
    }
}
```

|Propriedade|Obrigatório|DESCRIÇÃO|
|---------|---------|---------|
|displayName|Sim|O título exibido do modo de exibição. O título deve ser **exclusivos** para cada exibição CustomResources no seu **viewDefinition.json**.|
|version|Não|A versão da plataforma usada para renderizar a exibição.|
|resourceType|Sim|O tipo de recurso personalizado. Deve ser um **exclusivo** tipo de recurso personalizado do provedor personalizado.|
|createUIDefinition|Não|Criar definição de interface do usuário do esquema para criar o comando de recurso personalizado. Para obter uma introdução à criação de definições de interface do usuário, consulte [Introdução ao CreateUiDefinition](create-uidefinition-overview.md)|
|Comandos|Não|A matriz de botões de barra de ferramentas adicionais do modo de exibição CustomResources, consulte [comandos](#commands).|
|colunas|Não|A matriz de colunas de recurso personalizado. Se não estiver definido o `name` coluna será mostrada por padrão. A coluna deve ter `"key"` e `"displayName"`. Para a chave, forneça a chave da propriedade a ser exibido em uma exibição. Se aninhado, use o ponto como delimitador, por exemplo, `"key": "name"` ou `"key": "properties.property1"`. Para o nome de exibição, forneça o nome de exibição da propriedade a ser exibido em uma exibição. Você também pode fornecer um `"optional"` propriedade. Quando definido como true, a coluna está oculta em uma exibição, por padrão.|

## <a name="commands"></a>Comandos

Comandos é uma matriz de botões de barra de ferramentas adicionais que são exibidos na página. Cada comando representa uma ação de POSTAGEM do seu provedor de personalizado do Azure definido em **Maintemplate**. Para obter uma introdução aos provedores personalizados, consulte [visão geral dos provedores personalizados do Azure](custom-providers-overview.md).

```json
{
    "commands": [
        {
            "displayName": "Start Test Action",
            "path": "testAction",
            "icon": "Start",
            "createUIDefinition": { }
        },
    ]
}
```

|Propriedade|Obrigatório|DESCRIÇÃO|
|---------|---------|---------|
|displayName|Sim|O nome exibido do botão de comando.|
|caminho|Sim|O nome de ação do provedor personalizado. A ação deve ser definida em **Maintemplate**.|
|ícone|Não|O ícone do botão de comando. Lista de ícones com suporte é definida em [esquema JSON](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).|
|createUIDefinition|Não|Crie esquema de definição de interface do usuário para o comando. Para obter uma introdução à criação de definições de interface do usuário, consulte [Introdução ao CreateUiDefinition](create-uidefinition-overview.md).|

## <a name="next-steps"></a>Próximas etapas

- Para obter uma introdução aos aplicativos gerenciados, consulte [Visão geral de aplicativos gerenciados do Azure](overview.md).
- Para obter uma introdução aos provedores personalizados, consulte [visão geral dos provedores personalizados do Azure](custom-providers-overview.md).

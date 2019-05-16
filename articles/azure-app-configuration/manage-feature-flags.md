---
title: Tutorial para uso da Configuração de Aplicativos do Azure para gerenciar sinalizadores de recurso | Microsoft Docs
description: Neste tutorial, você aprenderá a gerenciar sinalizadores de recurso separadamente de seu aplicativo usando a Configuração de Aplicativos do Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: d995a2e9f0d05dc3b0853036e8fb3c04ccdfab96
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412285"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Tutorial: Gerenciar sinalizadores de recurso na Configuração de Aplicativos do Azure

Você pode armazenar todos os sinalizadores de recurso na Configuração de Aplicativos do Azure e administrá-los em um único local. Ela tem uma interface do usuário de portal, chamada **Gerenciador de Recursos**, projetada especificamente para sinalizadores de recursos. Além disso, a Configuração de Aplicativos dá suporte ao esquema de dados de sinalizador de recurso do .NET Core nativamente.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Definir e gerenciar sinalizadores de recurso na Configuração de Aplicativos.
> * Acessar sinalizadores de recurso em seu aplicativo.

## <a name="create-feature-flags"></a>Criar sinalizadores de recurso

O **Gerenciador de Recursos** no portal do Azure para a Configuração de Aplicativos fornece uma interface do usuário para criar e gerenciar os sinalizadores de recursos usados em seu aplicativo. Siga as etapas abaixo para adicionar um novo sinalizador de recurso.

1. Selecione **Gerenciador de Recursos** > **+ Criar** para adicionar um sinalizador de recurso.

    ![Lista de sinalizadores de recurso](./media/azure-app-configuration-feature-flags.png)

2. Insira um nome de chave exclusivo para o sinalizador de recurso. Você precisa desse nome para referenciar o sinalizador no código.

3. Opcionalmente, forneça ao sinalizador de recurso uma descrição mais amigável para humanos.

4. Defina o estado inicial do sinalizador de recurso. Geralmente, ele é apenas *Ativado* ou *Desativado*.

    ![Criação do sinalizador de recurso](./media/azure-app-configuration-feature-flag-create.png)

5. Quando o estado for *Ativado*, opcionalmente, especifique qualquer condição adicional para qualificá-la com **Adicionar filtro**. Insira uma chave de filtro interna ou personalizada e associe parâmetros. Os filtros internos incluem:

    | Chave | Parâmetros JSON |
    |---|---|
    | Microsoft.Percentage | {"Value": 0-100%} |
    | Microsoft.TimeWindow | {"Start": horário UTC, "End": horário UTC} |

    ![Filtro de sinalizador de recurso](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Atualizar os estados do sinalizador de recurso

Siga as etapas a seguir para alterar o valor do estado de um sinalizador de recurso.

1. Selecione **Gerenciador de Recursos**.

2. Clique em **...** > **Editar** à direita de um sinalizador de recurso que deseja modificar.

3. Defina um novo estado para o sinalizador de recurso.

## <a name="access-feature-flags"></a>Acessar sinalizadores de recurso

Os sinalizadores de recursos criados pelo **Gerenciador de Recursos** são armazenados e recuperados como valores de chave normais. Eles são mantidos em um prefixo de namespace especial *.appconfig.featureflag*. Exiba os valores de chave subjacentes usando o **Gerenciador de Configurações**. Seu aplicativo pode recuperá-los usando provedores de configuração da Configuração de Aplicativos, SDKs, extensões de linha de comando e APIs REST.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a gerenciar sinalizadores de recurso e seus estados usando a Configuração de Aplicativos. Confira os recursos a seguir para obter mais informações sobre o suporte de gerenciamento de recursos na Configuração de Aplicativos e no ASP.NET Core.

* [Usar sinalizadores de recurso em um aplicativo ASP.NET Core](./use-feature-flags-dotnet-core.md)

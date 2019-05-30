---
title: Instalar o Visual Studio e o SSDT para o SQL Data Warehouse | Microsoft Docs
description: Instalar o Visual Studio e o SSDT (Ferramentas de Desenvolvimento do SQL Server) para o SQL Data Warehouse do Azure
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/05/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: b2e34f1f72b1b0aa76d4a3031102d052118dae5f
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304123"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Instalar o Visual Studio e o SSDT para o SQL Data Warehouse
Use o Visual Studio de 2019 para desenvolver aplicativos para o SQL Data Warehouse. SSDT de 2019 do Visual Studio não tiver suporte para o SQL Data Warehouse. 

Usando o Visual Studio com SSDT permite que você use o Pesquisador de objetos do SQL Server para explorar as tabelas, exibições, procedimentos armazenados e muitos outros objetos visualmente no SQL Data Warehouse. Ele também permite executar consultas.

> [!NOTE]
> O SQL Data Warehouse ainda não dá suporte aos Projetos do Banco de Dados do Visual Studio. Para receber atualizações periódicas sobre esse recurso, registre-se no [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>Etapa 1: Instalar Visual Studio
Siga estes links para baixar e instalar o Visual Studio. Se você já tiver o Visual Studio 2013 ou posterior instalado, pule para a Etapa 2 e instale o SSDT.

1. [Baixar o Visual Studio][].
2. Siga o guia [Instalação do Visual Studio][Installing Visual Studio] no MSDN e escolha as configurações padrão.

## <a name="step-2-install-ssdt"></a>Etapa 2: instalar o SSDT
Para instalar o SSDT para o Visual Studio, verifique primeiro se há atualização do SSDT a partir do Visual Studio seguindo as etapas a seguir.

1. No Visual Studio, clique em **Ferramentas** / **Extensões e Atualizações…** / **Atualizações**
2. Selecione **Atualizações do Produto**, em seguida, procure **Atualização do Microsoft SQL Server para as ferramentas do banco de dados**

Você deve ter a versão mais recente instalada se uma atualização não for encontrada. Para confirmar se o SSDT está instalado, clique em **Ajuda** / **Sobre o Microsoft Visual Studio** e procure o SQL Server Data Tools na lista. Se a opção de instalação não estiver disponível no Visual Studio, você pode visitar o [Download do SSDT] [ SSDT Download] página para baixar e instalar o SSDT manualmente.

## <a name="next-steps"></a>Próximas etapas
Agora que você tem a versão mais recente do SSDT, você está pronto para [conectar-se] [ connect] ao seu SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Baixar o Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu

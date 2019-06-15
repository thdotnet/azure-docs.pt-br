---
title: Armazenamento usando o portal do Azure no banco de dados do Azure para PostgreSQL – servidor único de crescimento automático
description: Este artigo descreve como você pode habilitar automaticamente aumentar o armazenamento usando o portal do Azure no banco de dados do Azure para PostegreSQL - servidor único
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: 9f88fe3e8a30dd80c5331bd6c8ea7aec401c6fb9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66676752"
---
# <a name="auto-grow-storage-using-the-azure-portal-in-azure-database-for-postgresql---single-server"></a>Armazenamento usando o portal do Azure no banco de dados do Azure para PostgreSQL – servidor único de crescimento automático
Este artigo descreve como você pode configurar um banco de dados do Azure para PostgreSQL armazenamento de servidor cresça sem afetar a carga de trabalho.

Quando um servidor atinge o limite de armazenamento alocado, o servidor é marcado como somente leitura. No entanto, se você habilitar o crescimento automático de armazenamento, o armazenamento do servidor aumenta para acomodar os dados de cada vez maiores. Para servidores com menos de 100 GB provisionado de armazenamento, o tamanho de armazenamento provisionado é aumentado em 5 GB, assim o armazenamento livre estiver abaixo de 1 GB ou 10% do armazenamento provisionado maior. Para servidores com mais de 100 GB de armazenamento provisionado, o tamanho de armazenamento provisionado é aumentado em 5% quando o espaço de armazenamento livre estiver abaixo de 5% do tamanho do armazenamento provisionado. Armazenamento máximo limita conforme especificado [aqui](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#storage) se aplicam.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de instruções, você precisa:
- Um [servidor de Banco de Dados do Azure para PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="enable-storage-auto-grow"></a>Habilitar armazenamento auto crescer 

Siga estas etapas para definir o crescimento automático de armazenamento do servidor do PostgreSQL:

1. No [portal do Azure](https://portal.azure.com/), selecione seu banco de dados do Azure para servidor PostgreSQL.

2. Na página do servidor PostgreSQL, sob **as configurações**, clique em **tipo de preço** para abrir a página camada de preços.

3. No **aumento automático** seção, selecione **Sim** para habilitar o crescimento automático de armazenamento.

    ![Banco de dados do Azure para PostgreSQL - Settings_Pricing_tier – o aumento automático](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Clique em **OK** para salvar as alterações.

5. Uma notificação confirmará auto crescer foi habilitada com êxito.

    ![Banco de dados do Azure para PostgreSQL – êxito de aumento automático](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre [como criar alertas em métricas](howto-alert-on-metric.md).

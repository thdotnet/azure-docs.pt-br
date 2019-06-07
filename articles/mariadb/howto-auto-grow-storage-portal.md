---
title: Armazenamento no banco de dados do Azure de crescimento automático para MariaDB usando o portal do Azure
description: Este artigo descreve como você pode habilitar automaticamente o crescimento do armazenamento de banco de dados do Azure para MariaDB usando o portal do Azure
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: bb3291b66776a5f0f6be16069b2d6a999b2d1f32
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676872"
---
# <a name="auto-grow-storage-in-azure-database-for-mariadb-using-the-azure-portal"></a>Armazenamento no banco de dados do Azure de crescimento automático para MariaDB usando o portal do Azure
Este artigo descreve como você pode configurar um banco de dados do Azure para o armazenamento de servidor MariaDB cresça sem afetar a carga de trabalho.

Quando um servidor atinge o limite de armazenamento alocado, o servidor é marcado como somente leitura. No entanto, se você habilitar o crescimento automático de armazenamento, o armazenamento do servidor aumenta para acomodar os dados de cada vez maiores. Para servidores com menos de 100 GB provisionado de armazenamento, o tamanho de armazenamento provisionado é aumentado em 5 GB, assim o armazenamento livre estiver abaixo de 1 GB ou 10% do armazenamento provisionado maior. Para servidores com mais de 100 GB de armazenamento provisionado, o tamanho de armazenamento provisionado é aumentado em 5% quando o espaço de armazenamento livre estiver abaixo de 5% do tamanho do armazenamento provisionado. Armazenamento máximo limita conforme especificado [aqui](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#storage) se aplicam.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de instruções, você precisa:
- Um [banco de dados do Azure para MariaDB](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="enable-storage-auto-grow"></a>Habilitar armazenamento auto crescer 

Siga estas etapas para definir o crescimento automático de armazenamento do servidor do MariaDB:

1. No [portal do Azure](https://portal.azure.com/), selecione seu banco de dados do Azure para MariaDB.

2. Na página do servidor MariaDB, sob **as configurações** título, clique em **tipo de preço** para abrir a página camada de preços.

3. Na seção de aumento automático, selecione **Sim** para habilitar o crescimento automático de armazenamento.

    ![Banco de dados do Azure para MariaDB - Settings_Pricing_tier – o aumento automático](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Clique em **OK** para salvar as alterações.

5. Uma notificação confirmará auto crescer foi habilitada com êxito.

    ![Banco de dados do Azure para MariaDB - êxito de aumento automático](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre [como criar alertas em métricas](howto-alert-metric.md).

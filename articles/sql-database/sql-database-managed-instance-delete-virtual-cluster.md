---
title: Excluir uma sub-rede depois que a exclusão de um banco de dados do SQL Azure a instância gerenciada | Microsoft Docs
description: Saiba como excluir uma rede virtual do Azure após a exclusão de um banco de dados do SQL Azure a instância gerenciada.
services: sql-database
ms.service: sql-database
ms.subservice: management
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
manager: craigg
ms.date: 06/26/2019
ms.openlocfilehash: 4679ecda210fa78aad4315bc6602b67dd1795ce9
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67427981"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-database-managed-instance"></a>Excluir uma sub-rede depois que a exclusão de um banco de dados do SQL Azure a instância gerenciada

Este artigo fornece diretrizes sobre como excluir manualmente uma sub-rede depois de excluir o último banco de dados SQL a instância gerenciada do que residem nele.

Banco de dados SQL usa um [cluster virtual](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) para conter a instância gerenciada excluída. O cluster virtual persiste por 12 horas após a exclusão de instância, para que você possa criar rapidamente as instâncias gerenciadas na mesma sub-rede. Não há nenhum custo para manter um cluster virtual vazio. Durante esse período, a sub-rede associada ao cluster virtual não pode ser excluída.

Se você não quiser aguardar 12 horas e preferir excluir o cluster virtual e sua sub-rede imediatamente, você pode fazer isso manualmente. Exclua o cluster virtual manualmente usando o portal do Azure ou os API de clusters virtuais.

> [!NOTE]
> O cluster virtual não deve conter nenhuma instância gerenciada para a exclusão seja bem-sucedida.

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Excluir o cluster virtual do portal do Azure

Para excluir um cluster virtual, usando o portal do Azure, pesquise os recursos de cluster virtual.

![Captura de tela do portal do Azure, com a caixa de pesquisa realçado](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Depois de localizar o cluster virtual que deseja excluir, selecione esse recurso e selecione **excluir**. Você será solicitado a confirmar a exclusão de cluster virtual.

![Captura de tela do portal do Azure Virtual clusters do painel, com a opção de exclusão realçada](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

A área de notificações do portal do Azure mostra a confirmação de que o cluster virtual foi excluído. Exclusão bem-sucedida de cluster virtual libera imediatamente a sub-rede para reutilização.

> [!TIP]
> Se não há nenhuma instância gerenciada, mostrada no cluster virtual, e não é possível excluir o cluster virtual, certifique-se de que você não tiver uma implantação de instância contínuo em andamento. Isso inclui a implantações iniciadas e canceladas que ainda estão em andamento. Revisar as implantações do guia do grupo de recursos, a instância foi implantado para indicará que todas as implantações em andamento. Nesse caso, o await para a implantação concluir, exclua a instância gerenciada e, em seguida, o cluster virtual.

## <a name="delete-virtual-cluster-by-using-the-api"></a>Excluir o cluster virtual por meio da API

Para excluir um cluster virtual por meio da API, use os parâmetros do URI especificados na [método de exclusão de clusters virtuais](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Próximas etapas

- Para obter uma visão geral, confira [O que é uma Instância Gerenciada?](sql-database-managed-instance.md).
- Saiba mais sobre [arquitetura de conectividade na Instância Gerenciada](sql-database-managed-instance-connectivity-architecture.md).
- Saiba como [modificar uma rede virtual existente para a Instância Gerenciada](sql-database-managed-instance-configure-vnet-subnet.md).
- Para um tutorial que mostra como criar uma rede virtual, criar uma Instância Gerenciada e restaurar um banco de dados com base em um backup de banco de dados, confira [Criar uma Instância Gerenciada do Banco de Dados SQL do Azure](sql-database-managed-instance-get-started.md).
- Para problemas de DNS, confira [Configurar um DNS personalizado](sql-database-managed-instance-custom-dns.md).

---
title: Versões com suporte no banco de dados do Azure para MariaDB
description: Descreve as versões com suporte no Banco de Dados do Azure para o MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: 950294094584958e83f6a16630a6e1f897785e46
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69897280"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Banco de Dados do Azure Suportado para as versões do servidor MariaDB

O banco de dados do Azure para MariaDB foi desenvolvido a partir do [servidor MariaDB](https://downloads.mariadb.org/)de código-fonte aberto, usando o mecanismo de InnoDB. 

MariaDB usa o esquema de nomenclatura X. Y. Z. X é a versão principal, Y é a versão secundária e Z é a versão do patch.

> [!NOTE]
> No serviço, um gateway é usado para redirecionar as conexões para as instâncias de servidor. Depois que a conexão é estabelecida, o cliente MySQL exibe a versão do MariaDB definida no gateway, não a versão real em execução na sua instância do servidor MariaDB. Para determinar a versão da sua instância do servidor MariaDB, use `SELECT VERSION();` o comando.

O Banco de Dados do Azure para MariaDB atualmente suporta a seguinte versão:

## <a name="mariadb-version-102"></a>MariaDB versão 10,2

Versão do patch: 10.2.23

Consulte a [documentação do MariaDB](https://mariadb.com/kb/en/library/mariadb-10223-release-notes/) para saber mais sobre melhorias e correções no MariaDB 10.2.23.

## <a name="mariadb-version-103"></a>MariaDB versão 10,3

Versão do patch: 10.3.14

Consulte a [documentação do MariaDB](https://mariadb.com/kb/en/library/mariadb-10314-release-notes/) para saber mais sobre melhorias e correções no MariaDB 10.3.14.

## <a name="managing-updates-and-upgrades"></a>Gerenciar atualizações e upgrades
O serviço gerencia automaticamente atualizações para atualizações de patch. Por exemplo, 10.2.21 para 10.2.23.  

Atualmente, não há suporte para atualizações de versão principal e secundária. Por exemplo, não há suporte para a atualização de MariaDB 10,2 para MariaDB 10,3. Se você quiser atualizar de 10,2 para 10,3, faça um [despejo e restaure](./howto-migrate-dump-restore.md) -o para um servidor que foi criado com a nova versão do mecanismo.

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre cotas e limitações específicas de recursos com base em sua **camada de serviço**, confira [Camadas de serviço](./concepts-pricing-tiers.md).

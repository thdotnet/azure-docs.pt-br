---
title: Versões com suporte no banco de dados do Azure para MariaDB
description: Descreve as versões com suporte no Banco de Dados do Azure para o MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 760cb39ea1f3980faba348c7aa1de68a66b20a8d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065717"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Banco de Dados do Azure Suportado para as versões do servidor MariaDB

O Banco de Dados do Azure para o MariaDB foi desenvolvido a partir do [servidor MariaDB de código aberto](https://downloads.mariadb.org/), usando o mecanismo InnoDB. O Banco de Dados do Azure para MariaDB atualmente suporta a seguinte versão:

## <a name="mariadb-version-102"></a>MariaDB versão 10.2

Consulte a [MariaDB documentação](https://mariadb.com/kb/en/library/mariadb-10223-release-notes/) para saber mais sobre aperfeiçoamentos e correções no MariaDB 10.2.23.

## <a name="mariadb-version-103"></a>Versão do MariaDB 10.3

Consulte a [MariaDB documentação](https://mariadb.com/kb/en/library/mariadb-10314-release-notes/) para saber mais sobre aperfeiçoamentos e correções no MariaDB 10.3.14.

> [!NOTE]
> No serviço, um gateway é usado para redirecionar as conexões para as instâncias de servidor. Depois que a conexão é estabelecida, o cliente MySQL exibe a versão do MariaDB definida no gateway, não a versão real em execução na sua instância do servidor MariaDB. Para determinar a versão da sua instância do servidor MariaDB, use o comando `SELECT VERSION();` no prompt do MySQL.

## <a name="managing-updates-and-upgrades"></a>Gerenciar atualizações e upgrades

O serviço gerencia automaticamente a aplicação de patch para atualizações secundárias de versão.

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre cotas e limitações específicas de recursos com base em sua **camada de serviço**, confira [Camadas de serviço](./concepts-pricing-tiers.md).

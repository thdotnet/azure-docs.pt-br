---
title: Topologias de rede para migrações de banco de dados instância gerenciada do SQL usando o serviço de migração de banco de dados do Azure | Microsoft Docs
description: Saiba as configurações de origem e destino para o serviço de migração de banco de dados do Azure.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/07/2019
ms.openlocfilehash: 74613599903f7cde606295a1e2d9eaaa0924cf50
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808411"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-azure-database-migration-service"></a>Topologias de rede para migrações da instância gerenciada do Azure SQL DB usando o serviço de migração de banco de dados do Azure

Este artigo aborda várias topologias de rede que o serviço de migração de banco de dados do Azure pode trabalhar para fornecer uma experiência de migração abrangente de servidores do SQL local para o banco de dados de instância gerenciada do SQL.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Instância Gerenciada do Banco de Dados SQL do Azure configurada para cargas de trabalho Híbridas 

Use essa topologia se a Instância Gerenciada do Banco de Dados SQL do Azure está conectada à rede local. Essa abordagem fornece o roteamento de rede mais simplificado e gera a taxa máxima de transferência de dados durante a migração.

![Topologia de rede para cargas de trabalho híbridas](media/resource-network-topologies/hybrid-workloads.png)

**Requisitos**

- Nesse cenário, a instância de banco de dados do SQL Azure gerenciado e a instância do serviço de migração de banco de dados do Azure são criados na mesma rede virtual do Azure, mas usam sub-redes diferentes.  
- A VNet usada neste cenário também é conectada à rede local usando o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Instância Gerenciada do Banco de Dados SQL do Azure isolada da rede local

Use essa topologia de rede se o ambiente exigir um ou mais dos seguintes cenários:

- A instância gerenciada do banco de dados SQL é isolada de conectividade local, mas a instância do serviço de migração de banco de dados do Azure está conectada à rede local.
- Se as políticas de controle de acesso com base da função (RBAC) estão em vigor e você precisar limitar os acesso dos usuários à mesma assinatura que está hospedando a instância gerenciada do banco de dados SQL.
- As VNets usadas para o banco de dados de instância gerenciada do SQL e o serviço de migração de banco de dados do Azure estão em assinaturas diferentes.

![Topologia de rede da Instância Gerenciada isolada da rede local](media/resource-network-topologies/mi-isolated-workload.png)

**Requisitos**

- A rede virtual que usa o serviço de migração de banco de dados do Azure para este cenário também deve estar conectada à rede local usando o (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Configure [rede virtual emparelhamento de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre a VNet usada para o banco de dados SQL gerenciados instância e serviço de migração de banco de dados do Azure.

## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Migrações de nuvem para nuvem: VNet compartilhada

Use essa topologia se o SQL Server de origem estiver hospedado em uma VM do Azure e compartilha a mesma rede virtual com a instância de banco de dados do SQL Azure gerenciado e o serviço de migração de banco de dados do Azure.

![Topologia de rede para migrações de nuvem para a nuvem com uma VNet compartilhada](media/resource-network-topologies/cloud-to-cloud.png)

**Requisitos**

- Nenhum requisito adicional.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Nuvem para migrações de nuvem: VNet isolada

Use essa topologia de rede se o ambiente exigir um ou mais dos seguintes cenários:

- A instância gerenciada do banco de dados SQL é provisionada em uma VNet isolada.
- Se as políticas de controle de acesso com base da função (RBAC) estão em vigor e você precisar limitar os acesso dos usuários à mesma assinatura que está hospedando a instância gerenciada do banco de dados SQL.
- As VNets usadas para o banco de dados de instância gerenciada do SQL e o serviço de migração de banco de dados do Azure estão em assinaturas diferentes.

![Topologia de rede para migrações de nuvem para a nuvem com uma VNet isolada](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Requisitos**

- Configure [rede virtual emparelhamento de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre a VNet usada para o banco de dados SQL gerenciados instância e serviço de migração de banco de dados do Azure.

## <a name="inbound-security-rules"></a>Regras de segurança de entrada

| **NOME**   | **PORTA** | **PROTOCOLO** | **FONTE** | **DESTINO** | **AÇÃO** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Qualquer      | Qualquer          | DMS SUBNET | Qualquer             | PERMITIR      |

## <a name="outbound-security-rules"></a>Regras de segurança de saída

| **NOME**                  | **PORTA**                                              | **PROTOCOLO** | **FONTE** | **DESTINO**           | **AÇÃO** | **Motivo para a regra**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gerenciamento                | 443,9354                                              | TCP          | Qualquer        | Qualquer                       | PERMITIR      | Comunicação do plano de gerenciamento por meio do barramento de serviço e o armazenamento de BLOBs do Azure. <br/>(Se o emparelhamento da Microsoft é habilitado, você talvez não precise essa regra.)                                                             |
| Diagnósticos               | 12000                                                 | TCP          | Qualquer        | Qualquer                       | PERMITIR      | DMS usa a regra para coletar informações de diagnóstico para fins de solução de problemas.                                                                                                                      |
| Servidor de origem SQL         | 1433 (ou porta TCP IP que o SQL Server está escutando) | TCP          | Qualquer        | Espaço de endereço local | PERMITIR      | Conectividade de origem do SQL Server do DMS <br/>(Se você tiver conectividade site a site, talvez não seja necessário usar essa regra.)                                                                                       |
| Instância nomeada do SQL Server | 1434                                                  | UDP          | Qualquer        | Espaço de endereço local | PERMITIR      | Conectividade de origem de instância nomeada do SQL Server do DMS <br/>(Se você tiver conectividade site a site, talvez não seja necessário usar essa regra.)                                                                        |
| Compartilhamento SMB                 | 445                                                   | TCP          | Qualquer        | Espaço de endereço local | PERMITIR      | Compartilhamento de rede SMB para que o DMS armazene arquivos de backup de banco de dados para migrações para o MI do Banco de Dados SQL do Azure e os SQL Servers na VM do Azure <br/>(Se você tiver conectividade site a site, talvez não seja necessária essa regra). |
| DMS_subnet                | Qualquer                                                   | Qualquer          | Qualquer        | DMS_Subnet                | PERMITIR      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Consulte também

- [Migrar o SQL Server para a Instância Gerenciada do Banco de Dados SQL do Azure](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Visão geral de pré-requisitos para usar o serviço de migração de banco de dados do Azure](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Criar uma rede virtual usando o portal do Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Próximas etapas

- Para obter uma visão geral do serviço de migração de banco de dados do Azure, consulte o artigo [o que é o serviço de migração de banco de dados do Azure?](dms-overview.md).
- Para obter informações atuais sobre disponibilidade regional do serviço de migração de banco de dados do Azure, consulte a [produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration) página.

---
title: "API REST: regras de firewall no nível de servidor do Banco de Dados SQL do Azure | Microsoft Docs"
description: "Saiba como configurar as regras de firewall de nível de servidor dos endereços IP que acessam os bancos de dados SQL do Azure usando o API REST."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: fc874f3c-c623-4924-8cb7-32a8c31e666c
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: ae230c012a17eb73c8993a32197c844c6abaa2a4
ms.openlocfilehash: e164e1da7ec5f2da157900c86b3313fff1affed9
ms.lasthandoff: 02/17/2017


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Configurar regras de firewall no nível de servidor do Banco de Dados SQL do Azure usando a API REST

O Banco de Dados SQL do Microsoft Azure usa regras de firewall para permitir conexões com servidores e bancos de dados. Você pode definir configurações de firewall nos níveis de banco de dados e de servidor para o banco de dados de usuário ou mestre no seu servidor do Banco de Dados SQL do Azure a fim de permitir seletivamente o acesso ao banco de dados.

> [!IMPORTANT]
> Para permitir que os aplicativos do Azure se conectem ao seu servidor de banco de dados, as conexões do Azure devem estar habilitadas. Para obter mais informações sobre regras de firewall e como habilitar conexões do Azure, confira [Firewall do Banco de Dados SQL do Azure](sql-database-firewall-configure.md). Se você estiver fazendo conexões dentro do limite de nuvem do Azure, terá que abrir algumas portas TCP adicionais. Para obter mais informações, veja a seção **V12 do Banco de Dados SQL: fora versus dentro** de [Portas além da 1433 para ADO.NET 4.5 e Banco de Dados SQL V12](sql-database-develop-direct-route-ports-adonet-v12.md)
> 
> 

## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Gerenciar regras de firewall no nível de servidor pela API REST
1. O gerenciamento das regras de firewall por meio da API REST deve ser autenticado. Para obter informações, consulte [Guia do desenvolvedor para a autorização com a API do Azure Resource Manager](../azure-resource-manager/resource-manager-api-authentication.md).
2. As regras no nível de servidor podem ser criadas, atualizadas ou excluídas usando a API REST
   
    Para criar ou atualizar uma regra de firewall no nível de servidor, execute o método PUT usando o seguinte:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
   
    Corpo da solicitação
   
        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 

    Para remover uma regra de firewall existente no nível de servidor, execute o método DELETE usando o seguinte:

        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Gerenciar regras de firewall usando a API REST
* [Criar ou Atualizar uma Regra de Firewall](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Excluir regra de firewall](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Obter regra de firewall](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Listar Todas as Regras de Firewall](https://msdn.microsoft.com/library/azure/mt604478.aspx)

## <a name="next-steps"></a>Próximas etapas
Para ver um artigo sobre como usar o Transact-SQL para criar regras de firewall no nível do banco de dados e no nível do servidor, consulte [Configurar regras de firewall nos níveis do banco de dados e do servidor de Banco de Dados SQL do Azure usando o T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Para ver artigos sobre como criar regras de firewall no nível de servidor usando outros métodos, confira: 

* [Configurar regras de firewall no nível do servidor do Banco de Dados SQL do Azure usando o portal do Azure](sql-database-configure-firewall-settings.md)
* [Como configurar um firewall do Banco de Dados SQL do Azure usando o PowerShell](sql-database-configure-firewall-settings-powershell.md)

Para ver um tutorial sobre como criar um banco de dados, consulte [Seu primeiro Banco de Dados SQL do Azure](sql-database-get-started.md).
Para obter ajuda com a conexão de um Banco de Dados SQL do Azure a partir de aplicativos de fonte aberta ou de terceiros, consulte [Exemplos de código do cliente de início rápido para o Banco de Dados SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Para entender como navegar para os bancos de dados, consulte [Gerenciar o acesso ao banco de dados e a segurança de logon](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Recursos adicionais
* [Protegendo o banco de dados](sql-database-security-overview.md)
* [Central de segurança do mecanismo de banco de dados do SQL Server e banco de dados SQL do Azure](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->




---
title: Configurar regras de aplicativo do Firewall do Azure com o SQL FQDNs
description: Neste artigo, você aprenderá como configurar o SQL FQDNs nas regras de aplicativo do Firewall do Azure.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 7/11/2019
ms.author: victorh
ms.openlocfilehash: e188a5dda8f936ad369aa2b9222bc726bb0d6a5e
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786569"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>Configurar regras de aplicativo do Firewall do Azure com o SQL FQDNs

> [!IMPORTANT]
> Regras de aplicativo do Azure do Firewall com FQDNs SQL está atualmente em visualização pública.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Agora você pode configurar regras de aplicativo do Firewall do Azure com SQL FQDNs. Isso permite que você limite o acesso de suas redes virtuais para apenas especificados instâncias do SQL server.

Com o SQL FQDNs, você pode filtrar o tráfego:

- De suas redes virtuais para um banco de dados SQL do Azure ou um Azure SQL Data Warehouse. Por exemplo:  Permitir o acesso a apenas *server1.database.windows.net sql*.
- Do local para instâncias gerenciadas do SQL do Azure ou SQL IaaS em execução em suas redes virtuais.
- Do spoke-to-spoke para instâncias gerenciadas do SQL do Azure ou SQL IaaS em execução em suas redes virtuais.

Durante a visualização pública, o FQDN do SQL filtragem tem suporte no [modo de proxy](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#connection-policy) somente (a porta 1433). Se você usar o SQL no modo de redirecionamento padrão, você pode filtrar o acesso usando a marca de serviço do SQL como parte da [regras de rede](overview.md#network-traffic-filtering-rules).
Se você usar portas não padrão para tráfego SQL IaaS, você pode configurar as portas nas regras de firewall de aplicativo.

> [!NOTE]
> Regras de aplicativo com FQDNs SQL está disponível atualmente em todas as regiões por meio da CLI do Azure, REST e modelos. A interface do usuário do portal está sendo adicionada a regiões de forma incremental e estarão disponível em todas as regiões quando a distribuição é concluída.

## <a name="configure-using-azure-cli"></a>Configurar usando a CLI do Azure

1. Implantar um [Firewall do Azure usando a CLI do Azure](deploy-cli.md).
2. Se você filtrar o tráfego para a instância gerenciada do SQL, SQL Data Warehouse ou banco de dados SQL, verifique se o modo de conectividade do SQL é definido como **Proxy**. Para saber como mudar o modo de conectividade do SQL, consulte [arquitetura de conectividade do SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#change-azure-sql-database-connection-policy). 

   > [!NOTE]
   > SQL *proxy* modo pode resultar em mais de latência em comparação comparada *redirecionar*. Se você quiser continuar usando o modo de redirecionamento, que é o padrão para clientes conectando-se no Azure, você pode filtrar o acesso usando o SQL [marca de serviço](service-tags.md) no firewall [regras de rede](tutorial-firewall-deploy-portal.md#configure-a-network-rule).

3. Configure uma regra de aplicativo com o FQDN do SQL para permitir o acesso a um SQL server:

   ```azurecli
   az network firewall application-rule create \
   -g FWRG \
   -f azfirewall \
   -c FWAppRules \
   -n srule \
   --protocols mssql=1433 \
   --source-addresses 10.0.0.0/24 \
   --target-fqdns sql-serv1.database.windows.net
   ```

## <a name="configure-using-the-azure-portal"></a>Configurar usando o portal do Azure
1. Implantar um [Firewall do Azure usando a CLI do Azure](deploy-cli.md).
2. Se você filtrar o tráfego para a instância gerenciada do SQL, SQL Data Warehouse ou banco de dados SQL, verifique se o modo de conectividade do SQL é definido como **Proxy**. Para saber como mudar o modo de conectividade do SQL, consulte [arquitetura de conectividade do SQL Azure](../sql-database/sql-database-connectivity-architecture.md#change-azure-sql-database-connection-policy). 

   > [!NOTE]
   > SQL *proxy* modo pode resultar em mais de latência em comparação comparada *redirecionar*. Se você quiser continuar usando o modo de redirecionamento, que é o padrão para clientes conectando-se no Azure, você pode filtrar o acesso usando o SQL [marca de serviço](service-tags.md) no firewall [regras de rede](tutorial-firewall-deploy-portal.md#configure-a-network-rule).
3. Adicionar a regra de aplicativo com o protocolo apropriado, a porta e o FQDN do SQL e, em seguida, selecione **salvar**.
   ![regra de aplicativo com o FQDN do SQL](media/sql-fqdn-filtering/application-rule-sql.png)
4. SQL o acesso de uma máquina virtual em uma rede virtual que filtra o tráfego através do firewall. 
5. Validar que [logs de Firewall do Azure](log-analytics-samples.md) mostrar o tráfego é permitido.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o proxy do SQL e modos de redirecionamento, consulte [arquitetura de conectividade de banco de dados SQL do Azure](../sql-database/sql-database-connectivity-architecture.md).
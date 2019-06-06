---
title: Configurar a restrição do tráfego de rede de saída para clusters de HDInsight do Azure
description: Saiba como configurar a restrição do tráfego de rede de saída para clusters de HDInsight do Azure.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: howto
ms.date: 05/30/2019
ms.openlocfilehash: 0e3a35c2ceed5f3bb08b2d332f05bbaf416c94b2
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743241"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall-preview"></a>Configurar o tráfego de rede de saída para clusters de HDInsight do Azure usando o Firewall (visualização)

Este artigo fornece as etapas para proteger o tráfego de saída do seu cluster HDInsight usando o Firewall do Azure. As etapas a seguir pressupõem que você está configurando um Firewall do Azure para um cluster existente. Se você estiver implantando um novo cluster e atrás de um firewall, crie seu cluster HDInsight e a sub-rede primeiro e, em seguida, siga as etapas neste guia.

## <a name="background"></a>Segundo plano

Clusters de HDInsight do Azure normalmente são implantados em sua própria rede virtual. O cluster tem dependências de serviços fora da rede virtual que exigem acesso à rede para funcionar corretamente.

Há várias dependências que exigem o tráfego de entrada. O tráfego de gerenciamento de entrada não pode ser enviado por meio de um dispositivo de firewall. Os endereços de origem para esse tráfego são conhecidos e publicados [aqui](hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip). Você também pode criar regras de grupo de segurança de rede (NSG) com essas informações para proteger o tráfego de entrada para os clusters.

As dependências de tráfego de saída do HDInsight são definidas quase que totalmente com FQDNs, que não têm endereços IP estáticos por trás delas. A falta de endereços estáticos significa que os grupos de segurança de rede (NSGs) não pode ser usados para bloquear o tráfego de saída de um cluster. Os endereços mudam com frequência suficiente, um não é possível configurar regras com base na resolução de nome atual e usá-la para configurar as regras NSG.

A solução para proteger os endereços de saída é usar um dispositivo de firewall que controlam o tráfego de saída com base em nomes de domínio. Firewall do Azure pode restringir o tráfego de HTTP e HTTPS de saída com base no FQDN de destino ou [FQDN marcas](https://docs.microsoft.com/azure/firewall/fqdn-tags).

## <a name="configuring-azure-firewall-with-hdinsight"></a>Configurando o Firewall do Azure com HDInsight

Um resumo das etapas para bloquear a saída de seu HDInsight existente com o Firewall do Azure são:
1. Crie um firewall.
1. Adicionar regras de aplicativo do firewall
1. Adicione regras de rede do firewall.
1. Crie uma tabela de roteamento.

### <a name="create-a-new-firewall-for-your-cluster"></a>Criar um novo firewall para seu cluster

1. Crie uma sub-rede denominada **AzureFirewallSubnet** na rede virtual em que o cluster existe. 
1. Criar um novo firewall **teste FW01** usando as etapas no [Tutorial: Implantar e configurar o Firewall do Azure usando o portal do Azure](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall).

### <a name="configure-the-firewall-with-application-rules"></a>Configurar o firewall com as regras de aplicativo

Crie uma coleção de regras de aplicativo que permite que o cluster enviar e receber comunicações importantes.

Selecione o novo firewall **FW01 teste** do portal do Azure. Clique em **regras** sob **configurações** > **coleção de regras de aplicativo** > **adicionar coleção de regras de aplicativo**.

![Título: Adicionar a coleção de regras de aplicativo](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

Sobre o **adicionar a coleção de regras de aplicativo** tela, conclua as seguintes etapas:

1. Insira um **nome**, **prioridade**e clique em **permitir** do **ação** menu suspenso.
1. Adicione as seguintes regras:
    1. Uma regra para permitir o tráfego do HDInsight e o Windows Update:
        1. No **marcas FQDN** seção, forneça uma **nome**e defina **endereços de origem** para `*`.
        1. Selecione **HDInsight** e o **WindowsUpdate** do **FQDN marcas** menu suspenso.
    1. Uma regra para permitir que a atividade de logon do Windows:
        1. No **destino FQDNs** seção, forneça uma **nome**e defina **endereços de origem** para `*`.
        1. Insira `https:443` sob **protocolo: Port** e `login.windows.net` sob **FQDNS de destino**.
    1. Se o cluster é apoiado por WASB, em seguida, adicione uma regra para o WASB:
        1. No **destino FQDNs** seção, forneça uma **nome**e defina **endereços de origem** para `*`.
        1. Insira `http:80,https:443` sob **protocolo: porta** e a url da conta de armazenamento sob **destino FQDNS**. O formato será semelhante a < storage_account_name.blob.core.windows.net >. Para usar somente https garantir conexões ["transferência segura obrigatória"](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) está habilitada na conta de armazenamento.
1. Clique em **Adicionar**.

![Título: Insira os detalhes de coleção de regra de aplicativo](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

### <a name="configure-the-firewall-with-network-rules"></a>Configurar o firewall com as regras de rede

Crie as regras de rede para configurar corretamente o seu cluster HDInsight.

1. Selecione o novo firewall **FW01 teste** do portal do Azure.
1. Clique em **regras** sob **configurações** > **coleção de regras de rede** > **adicionar coleção de regras de rede**.
1. No **adicionar a coleção de regras de rede** tela, insira um **nome**, **prioridade**e clique em **permitir** do **ação** menu suspenso.
1. Crie as seguintes regras:
    1. Uma regra de rede na seção de endereços IP que permite que o cluster executar a sincronização de relógio usando NTP.
        1. No **regras** seção, forneça uma **nome** e selecione **UDP** do **protocolo** lista suspensa.
        1. Definir **endereços de origem** e **endereços de destino** para `*`.
        1. Definir **portas de destino** para 123.
    1. Se você estiver usando o pacote de segurança Enterprise (ESP), em seguida, adicione uma regra de rede na seção de endereços IP que permite a comunicação com o AAD-DS para clusters do ESP.
        1. Determine os dois endereços IP dos controladores de domínio.
        1. Na próxima linha a **regras** seção, forneça uma **nome** e selecione **qualquer** do **protocolo** lista suspensa.
        1. Definir **endereços de origem** `*`.
        1. Insira todos os endereços IP dos controladores de domínio em **endereços de destino** separados por vírgulas.
        1. Definir **portas de destino** para `*`.
    1. Se você estiver usando o armazenamento do Azure Data Lake, você pode adicionar uma regra de rede na seção de endereços IP para resolver um problema SNI com ADLS Gen1 e Gen2. Essa opção roteará o tráfego para o firewall, que pode resultar em custos mais altos para grandes cargas de dados, mas o tráfego será registrado e auditável nos logs de firewall.
        1. Determine o endereço IP para sua conta de armazenamento do Data Lake. Você pode usar um comando do powershell, como `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` para resolver o FQDN para um endereço IP.
        1. Na próxima linha a **regras** seção, forneça uma **nome** e selecione **TCP** do **protocolo** lista suspensa.
        1. Definir **endereços de origem** `*`.
        1. Insira o endereço IP para sua conta de armazenamento **endereços de destino**.
        1. Definir **portas de destino** para `*`.
    1. (Opcional) Se você estiver usando o Log Analytics, em seguida, crie uma regra de rede na seção de endereços IP para permitir a comunicação com seu espaço de trabalho do Log Analytics.
        1. Na próxima linha a **regras** seção, forneça uma **nome** e selecione **TCP** do **protocolo** lista suspensa.
        1. Definir **endereços de origem** `*`.
        1. Definir **endereços de destino** para `*`.
        1. Definir **portas de destino** para `12000`.
    1. Configure uma regra de rede na seção marcas de serviço do SQL que permitirá que você faça logon e auditar o tráfego SQL, a menos que configurado os pontos de extremidade de serviço para o SQL Server na sub-rede HDInsight que irá ignorar o firewall.
        1. Na próxima linha a **regras** seção, forneça uma **nome** e selecione **TCP** do **protocolo** lista suspensa.
        1. Definir **endereços de origem** `*`.
        1. Definir **endereços de destino** para `*`.
        1. Selecione **Sql** da **marcas de serviço** lista suspensa.
        1. Definir **portas de destino** para `1433,11000-11999,14000-14999`.
1. Clique em **adicionar** para concluir a criação de sua coleção de regras de rede.

![Título: Insira os detalhes de coleção de regra de aplicativo](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

### <a name="create-and-configure-a-route-table"></a>Criar e configurar uma tabela de rotas

Crie uma tabela de rotas com as seguintes entradas:

1. Reflete os seis de [esta lista de endereços IP de gerenciamento de HDInsight necessários](../hdinsight/hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) com um próximo salto da **Internet**:
    1. Quatro endereços IP para todos os clusters em todas as regiões
    1. Dois endereços IP que são específicos para a região em que o cluster é criado
1. Uma rota de solução de virtualização para 0.0.0.0/0 de endereço IP com o próximo nó que está sendo seu endereço IP privado do Firewall do Azure.

Por exemplo, para configurar a tabela de rotas para um cluster criado na região dos EUA de "EUA", use as seguintes etapas:

1. Entre no Portal do Azure.
1. Selecione o seu firewall do Azure **FW01 teste**. Cópia de **endereço IP privado** listado na **visão geral** página. Para este exemplo, usaremos um **endereço do 10.1.1.4 de exemplo**
1. Crie uma nova tabela de rota.
1. Clique em **rotas** sob **configurações**.
1. Clique em **adicionar** criar rotas para os endereços IP na tabela a seguir.

| Nome da rota | Prefixo de endereço | Tipo do próximo salto | Endereço do próximo salto |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | ND |
| 23.99.5.239 | 23.99.5.239/32 | Internet | ND |
| 168.61.48.131 | 168.61.48.131/32 | Internet | ND |
| 138.91.141.162 | 138.91.141.162/32 | Internet | ND |
| 13.67.223.215 | 13.67.223.215/32 | Internet | ND |
| 40.86.83.253 | 40.86.83.253/32 | Internet | ND |
| 0.0.0.0 | 0.0.0.0/0 | Solução de virtualização | 10.1.1.4 |

Conclua a configuração da tabela de rota:

1. Atribua a tabela de rotas criada à sua sub-rede HDInsight clicando **subredes** sob **configurações** e, em seguida, **associar**.
1. No **associar sub-rede** tela, selecione a rede virtual que o cluster foi criado em e o **sub-rede HDInsight** usado para o cluster HDInsight.
1. Clique em **OK**.

## <a name="edge-node-or-custom-application-traffic"></a>Nó de borda ou o tráfego de aplicativo personalizado

As etapas acima permitirá que o cluster operar sem problemas. Você ainda precisará configurar dependências para acomodar seus aplicativos personalizados em execução em nós de borda, se aplicável.

Dependências do aplicativo devem ser identificadas e adicionadas ao Firewall do Azure ou a tabela de rotas.

As rotas devem ser criadas para o tráfego de aplicativo evitar problemas de roteamento assimétrico.

Se seus aplicativos tenham outras dependências, eles precisarão ser adicionados ao Firewall do Azure. Crie regras de aplicativo para permitir o tráfego HTTP/HTTPS e regras de Rede para todo o resto.

## <a name="logging"></a>Registrando em log

Firewall do Azure pode enviar logs para alguns sistemas de armazenamento diferente. Para obter instruções sobre como configurar o registro em log para o seu firewall, siga as etapas em [Tutorial: Monitorar logs de Firewall do Azure e métricas](../firewall/tutorial-diagnostics.md).

Depois de concluir a configuração de registro em log, se você estiver registrando dados para o Log Analytics, você pode exibir o tráfego bloqueado com uma consulta como o seguinte:

```
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

A integração do Firewall do Azure com logs do Azure Monitor é útil ao primeiro obter um aplicativo funcionando quando você não estiver ciente de todas as dependências do aplicativo. Saiba mais sobre os logs do Azure Monitor em [Analisar dados de log no Azure Monitor](../azure-monitor/log-query/log-query-overview.md)

## <a name="access-to-the-cluster"></a>Acesso ao cluster
Depois de ter a configuração de firewall com êxito, você pode usar o ponto de extremidade interno (https://<clustername>-int.azurehdinsight.net) para acessar o Ambari de dentro da VNET. Para usar o ponto de extremidade público (https://<clustername>. azurehdinsight.net) ou ssh ponto de extremidade (<clustername>-ssh.azurehdinsight.net), verifique se você tem as rotas à direita na tabela de rotas e a configuração de regras de NSG para evitar o problema de roteamento assimétrico explicado [aqui](https://docs.microsoft.com/azure/firewall/integrate-lb).

## <a name="configure-another-network-virtual-appliance"></a>Configurar outro dispositivo de rede virtual

>[!Important]
> As seguintes informações estão **apenas** necessária se você quiser configurar um dispositivo de rede virtual (NVA) diferente do Firewall do Azure.

As instruções anteriores ajudarão-lo a configurar o Firewall do Azure para restringir o tráfego de saída do seu cluster HDInsight. Firewall do Azure é configurado automaticamente para permitir o tráfego para muitos dos cenários comuns de importantes. Se você quiser usar outro dispositivo de rede virtual, você precisará configurar manualmente um número de recursos adicionais. Tenha em mente como o seguinte a configurar seu dispositivo de rede virtual:

* Os serviços compatíveis com o Ponto de Extremidade de Serviço devem ser configurados com pontos de extremidade de serviço.
* Dependências de endereço IP são para o tráfego não HTTP/S (tráfego TCP e UDP).
* Pontos de extremidade HTTP/HTTPS FQDN podem ser colocados em seu dispositivo NVA.
* Pontos de extremidade HTTP/HTTPS do curinga são dependências que podem variar com base em um número de qualificadores.
* Atribua a tabela de rotas que você criar a sua sub-rede do HDInsight.

### <a name="service-endpoint-capable-dependencies"></a>Dependências com capacidade de Ponto de Extremidade de Serviço

| **Ponto de extremidade** |
|---|
| SQL do Azure |
| Armazenamento do Azure |
| Azure Active Directory |

#### <a name="ip-address-dependencies"></a>Dependências de endereço IP

| **Ponto de extremidade** | **Detalhes** |
|---|---|
| \*:123 | Verificação do relógio do NTP. O tráfego é verificado em vários pontos de extremidade na porta 123 |
| IPs publicado [aqui](hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) | Esses são o serviço HDInsight |
| Clusters de IPs privados do AAD-DS para ESP |
| \*: 16800 Windows para ativação do KMS |
| \*12000 para o Log Analytics |

#### <a name="fqdn-httphttps-dependencies"></a>Dependências de HTTP/HTTPS do FQDN

>[!Important]
> A lista a seguir concede apenas alguns dos mais importantes FQDNs. Você pode obter a lista completa de FQDNs para configurar seu NVA [nesse arquivo](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json).

| **Ponto de extremidade**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                |
| ocsp.digicert.com:80                                                |

## <a name="next-steps"></a>Próximas etapas

* [Arquitetura de rede virtual do Azure HDInsight](hdinsight-virtual-network-architecture.md)

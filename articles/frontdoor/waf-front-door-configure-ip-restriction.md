---
title: Configurar uma regra de restrição de IP com uma regra de firewall de aplicativo web da frente do serviço do Azure
description: Saiba como configurar uma regra de firewall de aplicativo web para restringir os endereços IP para um ponto de extremidade de serviço de porta da frente do Azure existente.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud;tyao
ms.openlocfilehash: 88c5c284f26203ff3d6c39810a7b2810c1ebbc5a
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743158"
---
# <a name="configure-an-ip-restriction-rule-with-a-web-application-firewall-for-azure-front-door-service"></a>Configurar uma regra de restrição de IP com o firewall do aplicativo web da frente do serviço do Azure
Este artigo mostra como configurar regras de restrição de IP no firewall do aplicativo web (WAF) da frente do serviço do Azure usando a CLI do Azure, Azure PowerShell ou um modelo do Azure Resource Manager.

Uma regra de controle de acesso com base no endereço IP é uma regra WAF personalizada que lhe permite controlar o acesso aos seus aplicativos web. Isso é feito especificando uma lista de endereços IP ou intervalos de endereços IP no formato de Classless inter-Domain Routing (CIDR).

Por padrão, o aplicativo web é acessível pela internet. Se você quiser limitar o acesso aos clientes em uma lista de endereços IP conhecidos ou intervalos de endereços IP, você deve criar duas regras de correspondência de IP. A primeira regra correspondente de IP contém a lista de endereços IP como valores correspondentes e define a ação para **permitir**. O outro, com prioridade mais baixa, bloqueia todos os outros endereços IP usando o **todos os** operador e definindo a ação como **bloco**. Depois que uma regra de restrição de IP é aplicada, as solicitações originadas de endereços fora dessa lista de permissão recebem uma resposta 403 Proibido.  

## <a name="configure-a-waf-policy-with-the-azure-cli"></a>Configurar uma política de WAF com a CLI do Azure

### <a name="prerequisites"></a>Pré-requisitos
Antes de começar a configurar uma diretiva de restrição de IP, configure seu ambiente da CLI e criar um perfil de porta da frente o serviço do Azure.

#### <a name="set-up-the-azure-cli-environment"></a>Configurar o ambiente de CLI do Azure
1. Instalar o [CLI do Azure](/cli/azure/install-azure-cli), ou usar o Azure Cloud Shell. O Azure Cloud Shell é um shell Bash gratuito que pode ser executado diretamente no portal do Azure. Ele tem a CLI do Azure instalada e configurada para usar com sua conta. Selecione o **Experimente** botão nos comandos da CLI que siga e, em seguida, entrar sua conta do Azure na sessão do Cloud Shell que é aberta. Depois que a sessão é iniciada, digite `az extension add --name front-door` para adicionar a extensão de serviço de porta da frente do Azure.
 2. Se você estiver usando a CLI localmente no Bash, entrar no Azure usando `az login`.

#### <a name="create-an-azure-front-door-service-profile"></a>Criar um perfil de porta da frente o serviço do Azure
Criar um perfil de porta da frente o serviço do Azure seguindo as instruções descritas em [guia de início rápido: Criar uma porta de entrada para um aplicativo web global altamente disponível](quickstart-create-front-door.md).

### <a name="create-a-waf-policy"></a>Criar uma política de WAF

Criar uma política de WAF, usando o [waf-política de rede az criar](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-create) comando. No exemplo que segue, substitua o nome da política *IPAllowPolicyExampleCLI* com um nome de política exclusivo.

```azurecli-interactive 
az network waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-a-custom-ip-access-control-rule"></a>Adicionar uma regra personalizada de controle de acesso IP

Use o [personalizado-regra de política de waf de rede az criar](/cli/azure/ext/front-door/network/waf-policy/custom-rule?view=azure-cli-latest#ext-front-door-az-network-waf-policy-custom-rule-create) comando para adicionar uma IP acesso controle regra personalizada para a política de WAF você acabou de criar.

Nos exemplos a seguir:
-  Substitua *IPAllowPolicyExampleCLI* com sua política exclusiva criada anteriormente.
-  Substitua *ip-endereço de-intervalo de-1*, *ip-endereço de-intervalo de-2* com seu próprio intervalo.

Primeiro, crie o IP permitem que a regra para os endereços especificados.

```azurecli
az network waf-policy custom-rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --match-condition RemoteAddr IPMatch "<ip-address-range-1>","<ip-address-range-2>" \
  --action Allow \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
Em seguida, crie uma **bloqueie todo** regra com prioridade mais baixa que o anterior **permitir** regra. Novamente, substitua *IPAllowPolicyExampleCLI* no exemplo a seguir com a sua diretiva exclusiva que você criou anteriormente.

```azurecli
az network waf-policy custom-rule create \
  --name IPDenyAllRule\
  --priority 2 \
  --rule-type MatchRule \
  --match-condition RemoteAddr Any
  --action Block \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
    
### <a name="find-the-id-of-a-waf-policy"></a>Localizar a ID de uma política de WAF 
Localizar a ID de uma política WAF, usando o [show do az rede waf política](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-show) comando. Substitua *IPAllowPolicyExampleCLI* no exemplo a seguir com a sua diretiva exclusiva que você criou anteriormente.

   ```azurecli
   az network waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Vincular uma política de WAF a um host de front-end do serviço de porta da frente do Azure

Defina o serviço do Azure da frente *WebApplicationFirewallPolicyLink* ID para a ID de política usando o [atualização do az rede front-door](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update) comando. Substitua *IPAllowPolicyExampleCLI* com sua política exclusiva que você criou anteriormente.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/<resource- name>/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
Neste exemplo, a política de WAF é aplicada a **FrontendEndpoints [0]** . Você pode vincular a política de WAF para qualquer um dos seus front-ends.
> [!Note]
> Você precisa definir a **WebApplicationFirewallPolicyLink** propriedade apenas uma vez para vincular uma política de WAF a um front-end do serviço de porta da frente do Azure. Atualizações de política subsequentes são aplicadas automaticamente para o front-end.

## <a name="configure-a-waf-policy-with-azure-powershell"></a>Configurar uma política de WAF com o Azure PowerShell

### <a name="prerequisites"></a>Pré-requisitos
Antes de começar a configurar uma diretiva de restrição de IP, configure o ambiente do PowerShell e criar um perfil de porta da frente o serviço do Azure.

#### <a name="set-up-your-powershell-environment"></a>Configurar o ambiente do PowerShell
O Azure PowerShell fornece um conjunto de cmdlets que usam o [do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelo para gerenciar recursos do Azure.

Você pode instalar o [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) no computador local e usá-lo em qualquer sessão do PowerShell. Siga as instruções na página para entrar no PowerShell usando suas credenciais do Azure e, em seguida, instale o módulo de Az.

1. Conectar-se ao Azure usando o comando a seguir e, em seguida, usar uma caixa de diálogo interativa para entrar.
    ```
    Connect-AzAccount
    ```
 2. Antes de instalar um módulo de serviço de porta da frente do Azure, verifique se que você tem a versão atual do módulo PowerShellGet instalado. Execute o seguinte comando e, em seguida, reabra o PowerShell.

    ```
    Install-Module PowerShellGet -Force -AllowClobber
    ``` 

3. Instale o módulo de Az.FrontDoor usando o comando a seguir. 
    
    ```
    Install-Module -Name Az.FrontDoor
    ```
### <a name="create-an-azure-front-door-service-profile"></a>Criar um perfil de porta da frente o serviço do Azure
Criar um perfil de porta da frente o serviço do Azure seguindo as instruções descritas em [guia de início rápido: Criar uma porta de entrada para um aplicativo web global altamente disponível](quickstart-create-front-door.md).

### <a name="define-an-ip-match-condition"></a>Definir uma condição de correspondência IP
Use o [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) comando para definir uma condição de correspondência IP.
No exemplo a seguir, substitua *ip-endereço de-intervalo de-1*, *ip-endereço de-intervalo de-2* com seu próprio intervalo.    
```powershell
$IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty IPMatch `
-MatchValue ["ip-address-range-1", "ip-address-range-2"]
```
Criar um IP *correspondem à condição de todos os* regra usando o seguinte comando:
```powershell
$IPMatchALlCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty Any        
  ```
    
### <a name="create-a-custom-ip-allow-rule"></a>Criar um personalizado a regra de permissão de IP

Use o [New-AzFrontDoorCustomRuleObject](/powershell/module/Az.FrontDoor/New-azfrontdoorwafcustomruleobject) comando para definir uma ação e definir uma prioridade. No exemplo a seguir, solicitações de cliente IPs que correspondem à lista serão permitidas.

```powershell
$IPAllowRule = New-AzFrontDoorCustomRuleObject `
-Name "IPAllowRule" `
-RuleType MatchRule `
-MatchCondition $IPMatchCondition `
-Action Allow -Priority 1
```
Criar uma **bloqueie todo** regra com prioridade menor do que o IP anterior **permitir** regra.
```powershell
$IPBlockAll = New-AzFrontDoorCustomRuleObject `
-Name "IPDenyAll" `
-RuleType MatchRule `
-MatchCondition $IPMatchALlCondition `
-Action Block `
-Priority 2
```

### <a name="configure-a-waf-policy"></a>Configurar uma política de WAF
Localizar o nome do grupo de recursos que contém o perfil de porta da frente o serviço do Azure usando `Get-AzResourceGroup`. Em seguida, configure uma política de WAF com o IP **bloqueie todo** regra usando [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule $IPBlockAll `
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Vincular uma política de WAF a um host de front-end do serviço de porta da frente do Azure

Vincule um objeto de diretiva de WAF a um existente front-end host e a atualização do Azure da frente propriedades do serviço. Primeiro, recupere o objeto de serviço de porta da frente do Azure por meio [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor). Em seguida, defina a **WebApplicationFirewallPolicyLink** propriedade para a ID de recurso *$IPAllowPolicyExamplePS*, criado na etapa anterior, usando o [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor)comando.

```powershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> Neste exemplo, a política de WAF é aplicada a **FrontendEndpoints [0]** . Você pode vincular uma política de WAF para qualquer um dos seus front-ends. Você precisa definir a **WebApplicationFirewallPolicyLink** propriedade apenas uma vez para vincular uma política de WAF a um front-end do serviço de porta da frente do Azure. Atualizações de política subsequentes são aplicadas automaticamente para o front-end.


## <a name="configure-a-waf-policy-with-a-resource-manager-template"></a>Configurar uma política de WAF com um modelo do Resource Manager
Para exibir o modelo que cria uma política de serviço de porta da frente do Azure e uma política de WAF com regras de restrição de IP personalizadas, acesse [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip).


## <a name="next-steps"></a>Próximas etapas

- Saiba como [criar um perfil de serviço do Azure da frente](quickstart-create-front-door.md).

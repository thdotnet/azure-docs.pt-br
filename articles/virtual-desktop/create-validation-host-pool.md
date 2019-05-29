---
title: Criar um pool de host de Visualização da Área de Trabalho Virtual do Windows para validar atualizações de serviço – Azure
description: Como criar um pool de host de validação para monitorar atualizações de serviço antes de distribuir atualizações para a produção.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 05/08/2019
ms.author: v-chjenk
ms.openlocfilehash: 91dd716a595dd56c14b3be0bf11a7b69eeb470d6
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834115"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Tutorial: Criar um pool de hosts para validar as atualizações de serviço

Pools de hosts são uma coleção de uma ou mais máquinas virtuais idênticas dentro dos ambientes de locatário de Versão Prévia da Área de Trabalho Virtual do Windows. Antes de implantar pools de host para seu ambiente de produção, é altamente recomendável criar um pool de host de validação. As atualizações são aplicadas primeiro para pools de host de validação, permitindo que você monitore atualizações de serviço antes de implantá-las em seu ambiente de produção. Sem um pool de host de validação, você pode não detectar alterações que apresentam erros, o que pode resultar em tempo de inatividade para os usuários em seu ambiente de produção.

Para garantir que seus aplicativos funcionem com as atualizações mais recentes, o pool de host de validação deve ser o mais semelhante possível aos pools de host em seu ambiente de produção. Os usuários devem se conectar com frequência ao pool de host de validação como fazem com o pool de host de produção. Se você tiver testes automatizados em seu pool de host, deverá incluí-los no pool de host de validação.

Você pode depurar problemas no pool de host de validação com o [recurso de diagnóstico](diagnostics-role-service.md) ou com os [artigos de para solucionar problemas da Área de Trabalho Virtual do Windows](https://docs.microsoft.com/Azure/virtual-desktop/troubleshoot-set-up-overview).

>[!NOTE]
> É recomendável deixar o pool de host de validação em vigor para testar todas as atualizações futuras.

Antes de começar, [baixe e importe o módulo do PowerShell da Área de Trabalho Virtual do Windows](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), caso ainda não tenha feito isso.

## <a name="create-your-host-pool"></a>Criar seu pool de host

Você pode criar um pool de host seguindo as instruções em qualquer um dos seguintes artigos:
- [Tutorial: Criar um pool de host com o Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Criar um pool de host com um modelo do Azure Resource Manager](create-host-pools-arm-template.md)
- [Criar um pool de host com o PowerShell](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Definir seu pool de host como um pool de host de validação

Execute os seguintes cmdlets do PowerShell para definir o novo pool de host como um pool de host de validação. Substitua os valores entre aspas pelos valores relevantes à sua sessão:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Execute o seguinte cmdlet do PowerShell para confirmar que a propriedade de validação foi definida. Substitua os valores entre aspas pelos valores relevantes para sua sessão.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Os resultados do cmdlet devem ser semelhantes a esta saída:

```
    TenantName          : contoso 
    TenantGroupName     : Default Tenant Group
    HostPoolName        : contosoHostPool
    FriendlyName        :
    Description         :
    Persistent          : False 
    CustomRdpProperty   : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnv       : True
    Ring                :
```

## <a name="update-schedule"></a>Agenda de atualização

Na versão prévia, as atualizações de serviço ocorrem aproximadamente em frequência mensal. Se houver grandes problemas, atualizações críticas serão fornecidas a um ritmo mais frequente.

## <a name="next-steps"></a>Próximas etapas

Agora que você criou um pool de host de validação, pode, opcionalmente, preencher seu pool de host de validação com RemoteApps. Para saber mais sobre como gerenciar aplicativos na Área de Trabalho Virtual do Windows, confira o tutorial Gerenciar grupos de aplicativos.

> [!div class="nextstepaction"]
> [Tutorial Gerenciar grupos de aplicativos](./manage-app-groups.md)

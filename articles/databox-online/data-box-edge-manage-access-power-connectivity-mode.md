---
title: Modo de conectividade, energia e acesso de dispositivo de borda da caixa de dados do Azure | Microsoft Docs
description: Descreve como gerenciar o acesso, a potência e o modo de conectividade para o dispositivo de borda de caixa de dados do Azure que ajuda a transferir dados para o Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: 2426ecd9c480b0cc236295384ae04c60051f37ce
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442962"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge"></a>Gerenciar o acesso, a potência e o modo de conectividade para a borda da caixa de dados do Azure

Este artigo descreve como gerenciar o modo de acesso, energia e conectividade para a borda da caixa de dados do Azure. Essas operações são executadas por meio da interface do usuário da web local ou o portal do Azure.

Neste artigo, você aprenderá a:

> [!div class="checklist"]
> * Gerenciar o acesso de dispositivo
> * Gerenciar o modo de conectividade
> * Gerenciar potência


## <a name="manage-device-access"></a>Gerenciar o acesso de dispositivo

O acesso ao seu dispositivo de borda da caixa de dados é controlado pelo uso de uma senha do dispositivo. Você pode alterar a senha por meio da interface do usuário da web local. Você também pode redefinir a senha do dispositivo no portal do Azure.

### <a name="change-device-password"></a>Alterar a senha de dispositivo

Siga estas etapas na interface do usuário local para alterar a senha do dispositivo.

1. Na interface do usuário de web local, vá para **manutenção > alteração de senha**.
2. Digite a senha atual e, em seguida, a nova senha. A senha fornecida deve ter entre 8 e 16 caracteres. A senha deve ter 3 dos seguintes caracteres: maiúscula, minúscula, numérica e caracteres especiais. Confirme a nova senha.

    ![Alterar senha](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. Selecione **Alterar senha**.
 
### <a name="reset-device-password"></a>Redefinir a senha do dispositivo

O fluxo de trabalho de redefinição não exige que o usuário recupere a senha antiga e é útil quando a senha é perdida. Esse fluxo de trabalho é executado no portal do Azure.

1. No portal do Azure, acesse **visão geral > Redefinir senha de administrador**.

    ![Redefinir senha](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Digite a nova senha e confirme-a. A senha fornecida deve ter entre 8 e 16 caracteres. A senha deve ter 3 dos seguintes caracteres: maiúscula, minúscula, numérica e caracteres especiais. Selecione **redefinir**.

    ![Redefinir senha](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-resource-access"></a>Gerenciar o acesso aos recursos

Para criar o dados de caixa de borda/dados de caixa de Gateway, o IoT Hub e o recurso de armazenamento do Azure, você precisa de permissões como um colaborador ou superior em um nível de grupo de recursos. Você também precisa os provedores de recursos correspondentes a serem registrados. Para todas as operações que envolvem as credenciais e a chave de ativação, também são necessárias permissões para a API do Graph do Azure Active Directory. Eles são descritos nas seções a seguir.

### <a name="manage-microsoft-azure-active-directory-graph-api-permissions"></a>Gerenciar permissões do Azure Active Directory API do Microsoft Graph

Ao gerar a chave de ativação para o dispositivo de borda da caixa de dados ou a realização de operações que exigem credenciais, você precisa de permissões para a API do Graph do Azure Active Directory. As operações que precisam de credenciais pode ser:

-  Criando um compartilhamento com uma conta de armazenamento associada.
-  Criando um usuário de quem pode acessar os compartilhamentos no dispositivo.

Você deve ter uma `User` acessar no locatário do Active Directory, pois você precisa ser capaz de `Read all directory objects`. Você não pode ser um usuário convidado já que não têm permissões para `Read all directory objects`. Se você for um convidado, em seguida, as operações como geração de uma ativação chave, a criação de um compartilhamento em seu dispositivo de borda da caixa de dados, a criação de um usuário, a configuração da borda da função de computação, a redefinição de senha do dispositivo todos falhará.

Para obter mais informações sobre como fornecer acesso aos usuários para a API do Graph do Azure Active Directory, consulte [padrão de acesso para os administradores, usuários e usuários convidados](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).

### <a name="register-resource-providers"></a>Registrar provedores de recursos

Para provisionar um recurso no Azure (no modelo do Azure Resource Manager), você precisa de um provedor de recursos que dá suporte à criação do recurso. Por exemplo, para provisionar uma máquina virtual, você deve ter um provedor de recursos 'Microsoft. Compute' disponível na assinatura.
 
Provedores de recursos são registrados no nível da assinatura. Por padrão, qualquer nova assinatura do Azure é previamente registrada com uma lista de provedores de recursos comumente usados. O provedor de recursos 'Microsoft.DataBoxEdge' não está incluído nesta lista.

Você não precisa conceder permissões de acesso para o nível de assinatura para que os usuários possam criar recursos, como 'Microsoft.DataBoxEdge' em seus grupos de recursos que eles têm direitos de proprietário, desde que os provedores de recursos para esses recursos já está registrado.

Antes de tentar criar qualquer recurso, certifique-se de que o provedor de recursos está registrado na assinatura. Se o provedor de recursos não estiver registrado, você precisará certificar-se de que o usuário que está criando o novo recurso tem direitos suficientes para registrar o provedor de recurso necessário no nível da assinatura. Se você ainda não fez isso, bem, você verá o seguinte erro:

*A assinatura <Subscription name> não tem permissões para registrar os provedores de recursos: Microsoft.DataBoxEdge.*


Para obter uma lista de provedores de recursos registrado na assinatura atual, execute o seguinte comando:

```PowerShell
Get-AzResourceProvider -ListAvailable |where {$_.Registrationstate -eq "Registered"}
```

Para o dispositivo de borda da caixa de dados, `Microsoft.DataBoxEdge` deve ser registrado. Para registrar `Microsoft.DataBoxEdge`, administrador de assinatura deve ser executado o comando a seguir:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.DataBoxEdge
```

Para obter mais informações sobre como registrar um provedor de recursos, consulte [resolva erros de registro do provedor de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="manage-connectivity-mode"></a>Gerenciar o modo de conectividade

Além de modo padrão totalmente conectada, o dispositivo também pode executar no modo parcialmente conectado ou desconectado totalmente. Cada um desses modos é descrita como abaixo:

- **Totalmente conectada** -este é o modo padrão normal em que o dispositivo funciona. O upload de nuvem e o download de dados está habilitada neste modo. Você pode usar o portal do Azure ou da interface do usuário da web local para gerenciar o dispositivo.

- **Parcialmente desconectado** – nesse modo, o dispositivo não é possível carregar ou baixar qualquer compartilhamento no entanto os dados podem ser gerenciados por meio do portal do Azure.

    Esse modo é normalmente usado quando em uma rede de satélite medida e o objetivo é minimizar o consumo de largura de banda da rede. Consumo de rede mínima ainda poderão ocorrer para monitoramento de operações de dispositivo.

- **Disconnected** - Neste modo, o dispositivo é totalmente desconectado da nuvem e os uploads e downloads na nuvem são desativados. O dispositivo só pode ser gerenciado pela interface da web local.

    Esse modo é normalmente usado quando você deseja colocar o dispositivo offline.

Para alterar o modo de dispositivo, siga estas etapas:

1. Na interface do usuário da web local do seu dispositivo, acesse **Configuração> Configurações da nuvem**.
2. Na lista suspensa, selecione o modo que você deseja que operam no dispositivo. Você pode selecionar **totalmente conectada**, **parcialmente conectada**, e **totalmente desconectado**. Para executar o dispositivo no modo desconectado parcialmente, habilite **gerenciamento do portal do Azure**.

    ![Modo de conectividade](media/data-box-edge-manage-access-power-connectivity-mode/connectivity-mode.png)
 
## <a name="manage-power"></a>Gerenciar potência

Você pode desligar ou reiniciar seu dispositivo físico, usando o interface do usuário da web local. Nós recomendamos que antes de reiniciar, colocar os compartilhamentos offline no servidor de data e, em seguida, no dispositivo. Essa ação minimiza a possibilidade de corrupção de dados.

1. Na interface do usuário de web local, vá para **manutenção > configurações de energia**.
2. Selecione **desligamento** ou **reiniciar** dependendo do que você pretende fazer.

    ![Configurações de energia](media/data-box-edge-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Quando solicitado a confirmar, selecione **Sim** para continuar.

> [!NOTE]
> Se você desligar o dispositivo físico, será preciso pressionar o botão de energia no dispositivo para ativá-lo.

## <a name="next-steps"></a>Próximas etapas

- Saiba como [gerenciar compartilhamentos](data-box-edge-manage-shares.md).

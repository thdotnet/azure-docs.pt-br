---
title: Configurar um laboratório para usar o Gateway de área de trabalho remota no Azure DevTest Labs | Microsoft Docs
description: Saiba como configurar um laboratório no Azure DevTest Labs com um gateway de área de trabalho remota para garantir o acesso seguro para as VMs do laboratório sem precisar expor a porta do RDP.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 430734878c01d10a4e7dd385dc75d8d502a2d82c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078996"
---
# <a name="configure-your-lab-in-azure-devtest-labs-to-use-a-remote-desktop-gateway"></a>Configurar seu laboratório no Azure DevTest Labs para usar um gateway de área de trabalho remota
No Azure DevTest Labs, você pode configurar um gateway de área de trabalho remota para o laboratório garantir o acesso seguro para as máquinas virtuais (VMs) do laboratório sem precisar expor a porta do RDP. O laboratório fornece um local central para os usuários do laboratório exibir e se conectar a todas as máquinas virtuais, que eles têm acesso. O **Connect** botão a **Máquina Virtual** página cria um arquivo RDP específicos do computador que você pode abrir para se conectar à máquina. Você pode personalizar ainda mais e proteger a conexão de RDP conectando-se em seu laboratório para um gateway de área de trabalho remota. 

Essa abordagem é mais segura porque o usuário de laboratório autentica diretamente ao computador do gateway ou pode usar as credenciais da empresa em um computador ingressado no domínio de gateway para se conectar às suas máquinas. O laboratório também suporta usando a autenticação de token para o computador do gateway que permite que os usuários se conectem suas máquinas virtuais do laboratório sem ter a porta do RDP exposta à internet. Este artigo apresenta um exemplo sobre como configurar um laboratório que usa a autenticação de token para se conectar a máquinas de laboratório.

## <a name="architecture-of-the-solution"></a>Arquitetura da solução

![Arquitetura da solução](./media/configure-lab-remote-desktop-gateway/architecture.png)

1. O [conteúdo do arquivo RDP Obtenha](/rest/api/dtl/virtualmachines/getrdpfilecontents) ação é chamada quando você seleciona o **Connect** button.1. 
1. Invoca a ação de conteúdo do arquivo RDP obter `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` para solicitar um token de autenticação.
    1. `{gateway-hostname}` é o nome do host do gateway especificado na **configurações de laboratório** página para seu laboratório no portal do Azure. 
    1. `{lab-machine-name}` é o nome da máquina que você está tentando se conectar.
    1. `{port-number}` é a porta na qual a conexão precisa ser feita. Normalmente, essa porta é 3389. Se o laboratório de VM estiver usando o [IP compartilhado](devtest-lab-shared-ip.md) recurso no DevTest Labs, a porta será diferente.
1. O gateway de área de trabalho remota adia a chamada de `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` para uma função do Azure para gerar o token de autenticação. O serviço DevTest Labs inclui automaticamente a chave de função no cabeçalho da solicitação. A tecla de função deve ser salva no cofre de chaves do laboratório. O nome do segredo a ser mostrado como **segredo do token de Gateway** sobre o **configurações do laboratório** página para o laboratório.
1. A função do Azure deve retornar um token para autenticação de token com base em certificado no computador do gateway.  
1. O arquivo RDP obter conteúdo de ação e retorna o arquivo RDP completo, incluindo as informações de autenticação.
1. Abra o arquivo RDP usando o programa de conexão de RDP preferencial. Lembre-se de que nem todos os programas de conexão de RDP dão suporte a autenticação de token. O token de autenticação têm uma data de expiração, definido pelo aplicativo de funções. Verifique a conexão para a VM de laboratório antes do token expira.
1. Depois que o computador do gateway de área de trabalho remota autentica o token no arquivo RDP, a conexão será encaminhado para seu computador de laboratório.

### <a name="solution-requirements"></a>Requisitos da solução
Para trabalhar com o recurso de autenticação de token do DevTest Labs, há alguns requisitos de configuração para as máquinas de gateway, serviços de nome de domínio (DNS) e funções.

### <a name="requirements-for-remote-desktop-gateway-machines"></a>Requisitos para máquinas de gateway de área de trabalho remota
- Certificado SSL deve ser instalado no computador do gateway para lidar com o tráfego HTTPS. O certificado deve corresponder o nome de domínio totalmente qualificado (FQDN) do balanceador de carga para o farm de servidores de gateway ou o FQDN do computador em si, se houver apenas um computador. Certificados SSL de curinga não funcionam.  
- Um certificado de autenticação instalado no computador (es) de gateway. Criar um certificado de autenticação usando [SigningCertificate.ps1 criar](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Create-SigningCertificate.ps1) script.
- Instalar o [autenticação conectável](https://code.msdn.microsoft.com/windowsdesktop/Remote-Desktop-Gateway-517d6273) módulo que dá suporte à autenticação de token para o gateway de área de trabalho remota. Um exemplo de um módulo desse tipo é `RDGatewayFedAuth.msi` que acompanha [imagens do System Center Virtual Machine Manager (VMM)](/system-center/vmm/install-console?view=sc-vmm-1807). Para obter mais informações sobre o System Center, consulte [documentação do System Center](https://docs.microsoft.com/system-center/) e [os detalhes de preços](https://www.microsoft.com/cloud-platform/system-center-pricing).  
- O servidor de gateway pode manipular as solicitações feitas a `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}`.

    O nome de host do gateway é o FQDN do balanceador de carga do farm de servidores de gateway ou o FQDN do computador em si, se houver apenas um computador. O `{lab-machine-name}` é o nome do computador de laboratório que você está tentando se conectar, e o `{port-number}` é a porta na qual a conexão será feita.  Por padrão, essa porta é 3389.  No entanto, se a máquina virtual está usando o [IP compartilhado](devtest-lab-shared-ip.md) recurso no DevTest Labs, a porta será diferente.
- O [roteamento de solicitação do aplicativo](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) módulo para o servidor de informações da Internet (IIS) pode ser usado para redirecionar `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` solicitações para a função do azure, que manipula a solicitação para obter um token para autenticação.


## <a name="requirements-for-azure-function"></a>Requisitos para a função do Azure
Solicitação de identificadores de função do Azure com o formato de `https://{function-app-uri}/app/host/{lab-machine-name}/port/{port-number}` e retorna o token de autenticação com base na mesma assinatura de certificado instalado nos computadores de gateway. O `{function-app-uri}` é o uri usado para acessar a função. A tecla de função é automaticamente transmitido no cabeçalho da solicitação. Para uma função de exemplo, consulte [ https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/src/RDGatewayAPI/Functions/CreateToken.cs ](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/src/RDGatewayAPI/Functions/CreateToken.cs). 


## <a name="requirements-for-network"></a>Requisitos de rede

- O DNS para o FQDN associado ao certificado SSL instalado nos computadores de gateway deve direcionar o tráfego para o computador do gateway ou o balanceador de carga do farm de computador do gateway.
- Se o computador de laboratório usa IPs privados, deve haver um caminho de rede do computador do gateway para a máquina de laboratório, por meio do compartilhamento da mesma rede virtual ou usando redes virtuais emparelhadas.

## <a name="configure-the-lab-to-use-token-authentication"></a>Configure o laboratório para usar autenticação de token 
Esta seção mostra como configurar um laboratório para usar uma máquina de gateway de área de trabalho remota que dá suporte à autenticação de token. Esta seção não aborda como configurar um farm de gateway de área de trabalho remota em si. Para obter essas informações, consulte o [amostra para criar um gateway de área de trabalho remota](#sample-to-create-a-remote-desktop-gateway) seção no final deste artigo. 

Antes de atualizar as configurações de laboratório, armazene a chave necessária para executar com êxito a função para retornar um token de autenticação no cofre de chaves do laboratório. Você pode obter o valor de chave de função **gerenciar** página para a função no portal do Azure. Para obter mais informações sobre como salvar um segredo em um cofre de chaves, consulte [adicionar um segredo ao Cofre de chaves](../key-vault/quick-create-portal.md#add-a-secret-to-key-vault). Salve o nome do segredo para uso posterior.

Para localizar a ID do Cofre de chaves do laboratório, execute o seguinte comando da CLI do Azure: 

```azurecli
az resource show --name {lab-name} --resource-type 'Microsoft.DevTestLab/labs' --resource-group {lab-resource-group-name} --query properties.vaultName
```

Configure o laboratório para usar a autenticação de token usando estas etapas:

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Selecione **Todos os Serviços** e selecione **Laboratórios de Desenvolvimento/Teste** na lista.
1. Na lista de laboratórios, selecione suas **laboratório**.
1. Na página do laboratório, selecione **configuração e políticas**.
1. No menu à esquerda, na **as configurações** seção, selecione **configurações do laboratório**.
1. No **área de trabalho remota** seção, insira o nome de domínio totalmente qualificado (FQDN) ou o endereço IP do computador do gateway dos serviços de área de trabalho remota ou de farm para o **nome do host do Gateway** campo. Esse valor deve corresponder ao FQDN do certificado SSL usado em máquinas de gateway.

    ![Opções de área de trabalho remota nas configurações de laboratório](./media/configure-lab-remote-desktop-gateway/remote-desktop-options-in-lab-settings.png)
1. No **área de trabalho remota** seção, para **o token do Gateway** segredo, digite o nome do segredo criado anteriormente. Esse valor não é a chave de função em si, mas o nome do segredo no cofre de chaves do laboratório que contém a chave de função.

    ![Segredo do token de gateway nas configurações de laboratório](./media/configure-lab-remote-desktop-gateway/gateway-token-secret.png)
1. **Salvar** alterações.

    > [!NOTE] 
    > Ao clicar em **salve**, você concorda com [termos de licença do Gateway de área de trabalho remota](https://www.microsoft.com/licensing/product-licensing/products). Para obter mais informações sobre o gateway remoto, consulte [bem-vindo aos serviços de área de trabalho remota](https://aka.ms/rds) e [implantar seu ambiente de área de trabalho remota](/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure).


Se configurar o laboratório por meio da automação é preferencial, consulte [DevTestLabGateway.ps1 conjunto](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Set-DevTestLabGateway.ps1) para um exemplo de script do PowerShell definir **nome do host de gateway** e **segredo do token de gateway**as configurações. O [repositório GitHub do Azure DevTest Labs](https://github.com/Azure/azure-devtestlab) também fornece um modelo do Azure Resource Manager que cria ou atualiza um laboratório com as **nome do host de gateway** e **segredo do token de gateway**as configurações.

## <a name="configure-network-security-group"></a>Configurar o grupo de segurança de rede
Para proteger ainda mais o laboratório, um grupo de segurança de rede (NSG) pode ser adicionado à rede virtual usada por máquinas virtuais do laboratório. Para obter instruções sobre como configurar um NSG, consulte [criar, alterar ou excluir um grupo de segurança de rede](../virtual-network/manage-network-security-group.md).

Aqui está um exemplo NSG que permita apenas o tráfego que passa pela primeira vez por meio do gateway para acessar as máquinas de laboratório. A origem nesta regra é o endereço IP do computador do gateway único ou o endereço IP do balanceador de carga na frente de máquinas de gateway.

![Grupo de segurança de rede - regras](./media/configure-lab-remote-desktop-gateway/network-security-group-rules.png)

## <a name="sample-to-create-a-remote-desktop-gateway"></a>Exemplo para criar um gateway de área de trabalho remota

> [!NOTE] 
> Ao usar os modelos de exemplo, você concorda com [termos de licença do Gateway de área de trabalho remota](https://www.microsoft.com/licensing/product-licensing/products). Para obter mais informações sobre o gateway remoto, consulte [bem-vindo aos serviços de área de trabalho remota](https://aka.ms/rds) e [implantar seu ambiente de área de trabalho remota](/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure).

O [repositório GitHub do Azure DevTest Labs](https://github.com/Azure/azure-devtestlab) fornece alguns exemplos para ajudar a instalação os recursos necessários para usar a autenticação de token e o gateway de área de trabalho remota com o DevTest Labs. Esses exemplos incluem modelos do Azure Resource Manager para o aplicativo de funções, configurações de laboratório e máquinas de gateway.

Siga estas etapas para configurar uma solução de exemplo para o farm de gateway de área de trabalho remota.

1. Crie um certificado de assinatura.  Run [Create-SigningCertificate.ps1](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Create-SigningCertificate.ps1). Salve a impressão digital, a senha e a codificação de Base64 do certificado criado.
2. Obter um certificado SSL. FQDN associado com o certificado SSL deve ser para o domínio que você controla. Salve a impressão digital, a senha e a codificação de Base64 para esse certificado. Para obter a impressão digital usando o PowerShell, use os comandos a seguir.

    ```powershell
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate;
    $cer.Import(‘path-to-certificate’);
    $hash = $cer.GetCertHashString()
    ```

    Para obter a codificação de Base64 usando o PowerShell, use o comando a seguir.

    ```powershell
    [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(‘path-to-certificate’))
    ```
3. Baixar arquivos do [ https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway ](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway).

    O modelo requer acesso a alguns outros modelos do Resource Manager e os recursos relacionados no mesmo URI base. Copie todos os arquivos de [ https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/arm/gateway ](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/arm/gateway) e RDGatewayFedAuth.msi em um contêiner de blob em uma conta de armazenamento.  
4. Implante **azuredeploy. JSON** partir [ https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway ](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway). O modelo usa os seguintes parâmetros:
    - adminUsername – Obrigado.  Nome de usuário administrador para as máquinas de gateway.
    - adminPassword – Obrigado. Senha da conta de administrador para as máquinas de gateway.
    - instanceCount – número de máquinas de gateway para criar.  
    - alwaysOn – indica se deve manter o aplicativo de funções do Azure criado em um estado quente ou não. Manter o aplicativo Azure Functions evitará atrasos quando os usuários tentarem se conectar ao seu laboratório de VM pela primeira vez, mas tem implicações de custo.  
    - tokenLifetime – o período de tempo que o token criado será válido. O formato é hh.
    - sslCertificate – o Base64 codificação do certificado SSL para o computador do gateway.
    - sslCertificatePassword – a senha do certificado SSL para o computador do gateway.
    - sslCertificateThumbprint - impressão digital do certificado para a identificação no repositório de certificados local do certificado SSL.
    - signCertificate – o Base64 codificação para o certificado de assinatura para o computador do gateway.
    - signCertificatePassword – a senha do certificado de assinatura para o computador do gateway.
    - signCertificateThumbprint - impressão digital do certificado para a identificação no repositório de certificados local do certificado de autenticação.
    - artifactslocation – localização do URI em que todos os recursos de suporte podem ser encontrados. Esse valor deve ser um UIR totalmente qualificado, e não um caminho relativo.
    - artifactslocationsastoken – token a assinatura de acesso compartilhado (SAS) usado para acessar recursos de apoio, se o local é uma conta de armazenamento do Azure.

    O modelo pode ser implantado usando a CLI do Azure usando o comando a seguir:

    ```azurecli
    az group deployment create --resource-group {resource-group} --template-file azuredeploy.json --parameters @azuredeploy.parameters.json -–parameters _artifactsLocation=”{storage-account-endpoint}/{container-name}” -–parameters _artifactsLocationSasToken = “?{sas-token}”
    ```

    Aqui estão as descrições dos parâmetros:

    - O {storage-conta-endpoint} pode ser obtido executando `az storage account show --name {storage-acct-name} --query primaryEndpoints.blob`.  O {storage-acct-name} é o nome da conta de armazenamento que contém os arquivos que você carregou.  
    - O {container-name} é o nome do contêiner em que o {storage-acct-name} que contém os arquivos que você carregou.  
    - O {sas-token} pode ser obtido executando `az storage container generate-sas --name {container-name} --account-name {storage-acct-name} --https-only –permissions drlw –expiry {utc-expiration-date}`. 
        - O {storage-acct-name} é o nome da conta de armazenamento que contém os arquivos que você carregou.  
        - O {container-name} é o nome do contêiner em que o {storage-acct-name} que contém os arquivos que você carregou.  
        - O {utc-data de expiração} é a data, no UTC, em que o token SAS irá expirar e o token SAS não pode ser usado para acessar a conta de armazenamento.

    Registre os valores para gatewayFQDN e gatewayIP da saída do modelo de implantação. Você também precisará salvar o valor da chave para a função recém-criada, que pode ser encontrada na função de [configurações do aplicativo de funções](../azure-functions/functions-how-to-use-azure-function-app-settings.md) guia.
5. Configure o DNS para que o certificado, FQDN de SSL, direciona para endereço IP do gatewayIP da etapa anterior.

    Depois que o farm de Gateway de área de trabalho remota é criado e atualizações DNS apropriadas são feitas, está pronto para ser usado por um laboratório no DevTest Labs. O **nome do host do gateway** e **segredo do token de gateway** configurações devem ser configuradas para usar os computadores de gateway que você implantou. 

    > [!NOTE]
    > Se o computador de laboratório usa IPs privados, deve haver um caminho de rede do computador do gateway para a máquina de laboratório, por meio do compartilhamento da mesma rede virtual ou usando uma rede virtual emparelhada.

    Depois que o gateway e laboratório estiverem configurados, o arquivo de conexão criado quando o usuário de laboratório clica na **Connect** incluirá automaticamente as informações necessárias para conectar-se usando a autenticação de token.     

## <a name="next-steps"></a>Próximas etapas
Consulte o seguinte artigo para saber mais sobre os serviços de área de trabalho remota: [Documentação de serviços de área de trabalho remota](/windows-server/remote/remote-desktop-services/Welcome-to-rds)



---
title: Publicar um aplicativo Web para uma VM do Azure a partir do Visual Studio
description: Publicar um aplicativo Web ASP.NET para uma Máquina Virtual do Azure a partir do Visual Studio
services: virtual-machines-windows
author: ghogen
manager: douge
tags: azure-service-management
ms.assetid: 70267837-3629-41e0-bb58-2167ac4932b3
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/03/2017
ms.author: ghogen
ms.openlocfilehash: 4b8e3ddf1cf5d61f730ce01a35ee0813b47ad2d2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66305926"
---
# <a name="publish-an-aspnet-web-app-to-an-azure-vm-from-visual-studio"></a>Publicar um aplicativo Web ASP.NET para uma VM do Azure a partir do Visual Studio

Este documento descreve como publicar um aplicativo de web do ASP.NET para uma máquina virtual do Azure (VM) usando o **máquinas virtuais do Microsoft Azure** recurso de publicação no Visual Studio de 2019.  

## <a name="prerequisites"></a>Pré-requisitos
Para usar o Visual Studio para publicar um projeto ASP.NET em uma VM do Azure, a VM deve estar configurada corretamente.

- O computador deve estar configurado para executar um aplicativo Web ASP.NET e ter o WebDeploy instalado.

- A VM deve ter um nome DNS configurado. Para obter mais informações, confira [Criar um nome de domínio totalmente qualificado no portal do Azure para uma VM do Windows](portal-create-fqdn.md).

## <a name="publish-your-aspnet-web-app-to-the-azure-vm-using-visual-studio"></a>Publicar seu aplicativo Web ASP.NET na VM do Azure usando o Visual Studio
A seção a seguir descreve como publicar um aplicativo Web ASP.NET existente em uma máquina virtual do Azure.

1. Abra sua solução de aplicativo web no Visual Studio de 2019.
2. Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e escolha **Publicar...**
3. Use a seta à direita da página para rolar pelas opções de publicação até encontrar **Máquinas Virtuais do Microsoft Azure**.  

   ![Página Publicar – seta para a direita]

4. Selecione o ícone **Máquinas Virtuais do Microsoft Azure** e depois **Publicar**.

   ![Publicar Página – Ícone da Máquina Virtual do Microsoft Azure]

5. Escolha a conta adequada (com a assinatura do Azure conectada à sua máquina virtual).  
   - Se estiver conectado ao Visual Studio, a lista de contas é preenchida com todas as suas contas autenticadas.  
   - Se não estiver conectado ou se a conta de que precisa não estiver listada, escolha "Adicionar uma conta..." e siga os prompts para fazer logon.  
   ![Seletor de Conta do Azure]  

6. Selecione a VM apropriada na lista de Máquinas Virtuais Existentes.

   > [!Note]
   > O preenchimento dessa lista pode demorar um pouco.

   ![Seletor de VM do Azure]

7. Clique em OK para começar a publicar.

8. Quando solicitado a fornecer credenciais, forneça o nome de usuário e a senha de uma conta de usuário na VM que está configurado com direitos de publicação de destino. Essas credenciais são normalmente o nome de usuário administrador e a senha usada ao criar a VM.  

   ![Logon no WebDeploy]

9. Aceite o certificado de segurança.

   ![Erro no Certificado]

10. Observe a janela de Saída para verificar o progresso da operação de publicação.

    ![Janela de saída]

11. Se a publicação for bem-sucedida, um navegador é inicializado para abrir a URL do site recém-publicado.

**Sucesso!**

Você publicou com êxito seu aplicativo Web para uma máquina virtual do Azure.

## <a name="publish-page-options"></a>Opções da página Publicação

Depois de concluir o assistente de publicação, a página Publicação é aberta no documento junto com o novo perfil de publicação selecionado.

### <a name="re-publish"></a>Republicar

Para publicar atualizações em seu aplicativo Web, selecione o botão **Publicar** na página Publicar.  
- Se solicitado, insira nome de usuário e senha.  
- A publicação começa imediatamente.

![Página Publicar – botão Publicar]

### <a name="modify-publish-profile-settings"></a>Modificar as configurações de perfil da publicação

Para exibir e modificar as configurações de perfil da publicação, selecione **Configurações...** .  

![Página Publicar – botão Configurações]

Suas configurações devem se parecer com isto:  

![Configurações da publicação – página Conexão]

#### <a name="save-user-name-and-password"></a>Salvar nome de usuário e senha
- Evite fornecer informações de autenticação toda vez que você publica. Para fazer isso, preencher a **nome de usuário** e **senha** campos e selecione o **salvar senha** caixa.
- Use o botão **Validar Conexão** para confirmar que foram inseridas as informações corretas.

#### <a name="deploy-to-clean-web-server"></a>Implantar para limpar o servidor Web

- Se você quiser garantir que o servidor web tenha uma cópia limpa do aplicativo web após cada carregamento e que nenhum outro arquivo é deixado de uma implantação anterior, você pode verificar a **remover arquivos adicionais no destino** caixa de seleção de  **Configurações** guia.

- Aviso: Publicar com essa configuração exclui todos os arquivos que existem no servidor web (diretório wwwroot). Verifique se você sabe o estado do computador antes de publicar com essa opção habilitada. 

![Configurações de publicação – página Configurações]

## <a name="next-steps"></a>Próximas etapas

### <a name="set-up-cicd-for-automated-deployment-to-azure-vm"></a>Configurar CI/CD para implantação automática à VM do Azure

Para configurar um pipeline de entrega contínua com o Azure Pipelines, confira [Implantar a uma máquina virtual do Windows](https://docs.microsoft.com/vsts/build-release/apps/cd/deploy-webdeploy-iis-deploygroups).

[VM Overview - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSName.png
[IP Address Config - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/IPAddressConfigDNSName.png
[VM Overview - DNS Configured]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSConfigured.png
[Página Publicar – seta para a direita]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageRightArrow.png
[Publicar Página – Ícone da Máquina Virtual do Microsoft Azure]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageMicrosoftAzureVirtualMachineIcon.png
[Seletor de Conta do Azure]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectAccount.png
[Seletor de VM do Azure]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectVM.png
[Logon no WebDeploy]: ../../../includes/media/publish-web-app-from-visual-studio/WebDeployLogin.png
[Erro no Certificado]: ../../../includes/media/publish-web-app-from-visual-studio/CertificateError.png
[Janela de Saída]: ../../../includes/media/publish-web-app-from-visual-studio/OutputWindow.png
[Página Publicar – botão Publicar]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPagePublishButton.png
[Página Publicar – botão Configurações]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageSettingsButton.png
[Configurações da publicação – página Conexão]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsConnectionPage.png
[Configurações de publicação – página Configurações]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsSettingsPage.png

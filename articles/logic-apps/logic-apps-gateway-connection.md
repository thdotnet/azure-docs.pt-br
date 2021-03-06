---
title: "Conexão do gateway de dados local com Aplicativos Lógicos | Microsoft Docs"
description: "Saiba como criar uma conexão ao gateway de dados no local de um aplicativo lógico."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/05/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: 0a16f22b6e3bb60091409c64b631afcba3d6db10


---
# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Conectar-se ao gateway de dados local para Aplicativos Lógicos
Conectores de aplicativos lógicos compatíveis permitem que você configure a conexão para acessar os dados no local por meio do gateway de dados local.  As etapas a seguir explicam como instalar e configurar o gateway de dados local para trabalhar com um aplicativo lógico.

## <a name="prerequisites"></a>Pré-requisitos
  * É necessário usar um endereço de email corporativo ou de estudante no Azure para associar o gateway de dados local com sua conta (conta do Azure Active Directory)
  * Se estiver usando uma Conta da Microsoft (por exemplo, @outlook.com, @live.com)), você poderá usar sua conta do Azure para criar um endereço de email corporativo ou de estudante [seguindo estas etapas](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal) 
  * É necessário ter o gateway de dados local [instalado em um computador local](logic-apps-gateway-install.md).
  * O gateway não deve ter sido declarado por outro gateway de dados local do Azure ([a declaração ocorre com a criação da etapa 2 abaixo](#2-create-an-azure-on-premises-data-gateway-resource)) - uma instalação só pode ser associada a um recurso de gateway.

## <a name="installing-and-configuring-the-connection"></a>Instalando e configurando a conexão
### <a name="1-install-the-on-premises-data-gateway"></a>1. Instale o gateway de dados local
Informações sobre como instalar o gateway de dados local podem ser encontradas [neste artigo](logic-apps-gateway-install.md).  O gateway deve ser instalado em um computador local antes de continuar com o restante das etapas.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. Crie um recurso de gateway de dados local do Azure
Uma vez instalado, você deve associar sua assinatura do Azure com o gateway de dados local.

1. Faça logon no Azure usando o mesmo endereço de email corporativo ou escolar usado durante a instalação do gateway
2. Clique no botão de recurso **Novo**
3. Procure e selecione o **gateway de dados local**
4. Preencha as informações para associar o gateway à sua conta e selecione o **Nome de Instalação**
   
    ![Conexão ao Gateway de Dados Local][1]
5. Clique no botão **Criar** para criar o recurso

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. Crie uma conexão com o aplicativo lógico no designer
Agora que sua assinatura do Azure está associada a uma instância do gateway de dados local, você pode criar uma conexão a ele de dentro de um aplicativo lógico.

1. Abra um aplicativo lógico e escolha um conector que ofereça suporte à conectividade local (na data de publicação deste artigo, o SQL Server)
2. Marque a caixa de seleção **Conectar por meio do gateway de dados local**
   
    ![Criação de Gateway do Designer de Aplicativo Lógico][2]
3. Selecione o **Gateway** ao qual se conectar e preencha outras informações de conexão necessárias
4. Clique em **Criar** para criar a conexão

A conexão deve ter sido configurada com êxito para uso em seu aplicativo lógico.  

## <a name="next-steps"></a>Próximas etapas
* [Exemplos comuns e cenários de aplicativos lógicos](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Recursos de integração corporativa](../logic-apps/logic-apps-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/logic-apps-gateway-connection/createblade.png
[2]: ./media/logic-apps-gateway-connection/blankconnection.png
[3]: ./media/logic-apps-logic-gateway-connection/checkbox.png



<!--HONumber=Jan17_HO3-->



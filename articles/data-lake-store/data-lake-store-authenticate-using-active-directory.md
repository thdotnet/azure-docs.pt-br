---
title: "Autenticação de serviço a serviço: Data Lake Store com o Azure Active Directory | Microsoft Docs"
description: "Saiba como obter a autenticação de serviço a serviço com o Data Lake Store usando o Azure Active Directory"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 820b7c5d-4863-4225-9bd1-df4d8f515537
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/02/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 1e6ae31b3ef2d9baf578b199233e61936aa3528e
ms.openlocfilehash: ae2280f7bd7945f723d88dc6ce3f9a117074e93f
ms.lasthandoff: 03/03/2017


---
# <a name="service-to-service-authentication-with-data-lake-store-using-azure-active-directory"></a>Autenticação de serviço a serviço com o Data Lake Store usando o Azure Active Directory
> [!div class="op_single_selector"]
> * [Autenticação serviço a serviço](data-lake-store-authenticate-using-active-directory.md)
> * [Autenticação do usuário final](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

O Azure Data Lake Store usa o Azure Active Directory para autenticação. Antes de criar um aplicativo que funciona com o Azure Data Lake Store ou com o Azure Data Lake Analytics, primeiro você deve decidir como deseja autenticar seu aplicativo no Azure Active Directory (Azure AD). As duas principais opções disponíveis são:

* Autenticação do usuário final 
* Autenticação serviço a serviço (este artigo) 

As duas opções resultam no fornecimento de um token OAuth 2.0 a seu aplicativo, que é anexado a cada solicitação feita ao Azure Data Lake Store ou ao Azure Data Lake Analytics.

Este artigo explica como criar um **aplicativo Web do Azure AD para autenticação serviço a serviço**. Para obter instruções sobre a configuração de aplicativo do Azure AD para autenticação de usuário final, consulte [Autenticação de usuário final com o Data Lake Store usando o Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Pré-requisitos
* Uma assinatura do Azure. Consulte [Obter avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>Etapa 1: Criar um aplicativo Web do Active Directory

Crie e configure um aplicativo Web do Azure AD para autenticação serviço a serviço com o Azure Data Lake Store usando o Azure Active Directory. Para obter instruções, consulte [Criar um aplicativo do Azure AD](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Ao seguir as instruções do link acima, verifique se você selecionou **Aplicativo Web/API** para tipo de aplicativo, conforme mostrado na captura de tela abaixo.

![Criar aplicativo Web](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "Criar aplicativo Web")

## <a name="step-2-get-client-id-client-secret-and-tenant-id"></a>Etapa 2: Obter ID do cliente, segredo do cliente e ID do locatário
Ao fazer logon por meio de programação, você precisa da id para seu aplicativo. Se o aplicativo for executado com suas próprias credenciais, você também precisará de uma chave de autenticação.

* Para obter instruções sobre como recuperar a ID e o segredo do cliente do aplicativo, consulte [Obter ID do aplicativo e chave de autenticação](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Para obter instruções sobre como recuperar a ID do locatário, consulte [Obter ID do locatário](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>Etapa 3: Atribuir o aplicativo do Azure AD ao arquivo ou pasta da conta do Azure Data Lake Store (apenas para autenticação de serviços)
1. Faça logon no novo [portal do Azure](https://portal.azure.com) e abra a conta do Azure Data Lake Store que você deseja associar ao aplicativo do Azure Active Directory criado anteriormente.
2. Na folha de sua conta do Repositório Data Lake, clique em **Gerenciador de Dados**.
   
    ![Crie diretórios na conta do Data Lake Store](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "criar diretórios na conta Data Lake")
3. Na folha **Data Explorer**, clique no arquivo ou pasta para o qual você deseja fornecer acesso ao aplicativo do Azure AD e, em seguida, clique em **Acessar**. Para configurar o acesso a um arquivo, você deverá clicar em **Acessar** da folha **Visualização do Arquivo**.
   
    ![Configurar ACLs no sistema de arquivos do Data Lake](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "definir ACLs no sistema de arquivos do Data Lake")
4. A folha **Acesso** lista o acesso padrão e o acesso personalizado já atribuídos à raiz. Clique no ícone **Adicionar** para adicionar as ACLs de nível personalizado.
   
    ![Lista de acesso padrão e personalizado](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "lista de acesso padrão e personalizado")
5. Clique no ícone **Adicionar** para abrir a folha **Adicionar Acesso Personalizado**. Nessa folha, clique em **Selecionar Usuário ou Grupo** e, na folha **Selecionar Usuário ou Grupo**, procure o aplicativo do Azure Active Directory que você criou anteriormente. Se houver muitos grupos para sua pesquisa, use a caixa de texto na parte superior para filtrar pelo nome do grupo. Clique no grupo que você deseja adicionar e clique em **Selecionar**.
   
    ![Adicionar um grupo](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "Adicionar um grupo")
6. Clique em **Selecionar Permissões**, selecione as permissões e se você desejar atribuir as permissões como uma ACL padrão, acessar a ACL ou ambos. Clique em **OK**.
   
    ![Atribuir permissões ao grupo](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "Atribuir permissões ao grupo")
   
    Para obter mais informações sobre permissões no Data Lake Store e ACLs de acesso/padrão, consulte [Controle de acesso no Data Lake Store](data-lake-store-access-control.md).
7. Na folha **Adicionar Acesso Personalizado**, clique em **OK**. O grupo recém-adicionado, com as permissões associadas, estará listado na folha **Acesso** .
   
    ![Atribuir permissões ao grupo](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "Atribuir permissões ao grupo")    

## <a name="next-steps"></a>Próximas etapas
Neste artigo, você criou um aplicativo Web do Azure AD e reuniu as informações necessárias em seus aplicativos cliente, que você cria usando o SDK do .NET, SDK do Java, etc. Agora você pode prosseguir para os artigos seguintes, que falam sobre como usar o aplicativo Web do Azure AD para primeiro se autenticar no Data Lake Store e, em seguida, executar outras operações no repositório.

* [Introdução ao Repositório Azure Data Lake usando o SDK do .NET](data-lake-store-get-started-net-sdk.md)
* [Introdução ao Azure Data Lake Store usando o SDK do Java](data-lake-store-get-started-java-sdk.md)

Este artigo ensinou em detalhes as etapas básicas necessárias para obter um usuário principal para cima e em execução para o seu aplicativo. Você pode ver os seguintes artigos para obter mais informações:
* [Usar o PowerShell para criar entidade de serviço](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Usar autenticação de certificado para autenticação de entidade de serviço](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal#create-service-principal-with-certificate)
* [Outros métodos para autenticar no AD do Azure](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios)




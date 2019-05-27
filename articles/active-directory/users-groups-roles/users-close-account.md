---
title: Fechar sua conta corporativa ou de estudante em um diretório não gerenciado - Azure Active Directory | Microsoft Docs
description: Como fechar sua conta corporativa ou de estudante em um Active Directory do Azure não gerenciado.
services: active-directory
author: rolyon
manager: mtillman
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 05/20/2019
ms.author: rolyon
ms.reviewer: ''
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39b359ef7feeaec541ba17e98a5d1e9b74c6403a
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957994"
---
# <a name="close-your-work-or-school-account-in-an-unmanaged-directory"></a>Fechar sua conta corporativa ou de estudante em um diretório não gerenciado

Se você for um usuário em uma organização não gerenciada do Azure Active Directory (Azure AD) e você não precisa usar os aplicativos da organização ou manter qualquer associação com o for, você pode fechar sua conta a qualquer momento. Um diretório não gerenciado não tem um Administrador Global. Os usuários em um diretório não gerenciado podem fechar suas contas por conta própria, sem a necessidade de entrar em contato com o administrador.

Os usuários em um diretório não gerenciado geralmente são criados durante o autoatendimento de inscrição. Um exemplo pode ser um operador de informações em uma organização que se inscreve para um serviço gratuito. Para obter mais informações sobre a inscrição de autoatendimento, consulte [What ' s Self-service Inscreva-se para o Azure Active Directory?](directory-self-service-signup.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="before-you-begin"></a>Antes de começar

Antes de fechar sua conta, você deve confirmar os seguintes itens:

* Verifique se você for um usuário do Azure não gerenciado diretório do AD. Se você pertencer a um diretório gerenciado, você não pode fechar sua conta. Se você pertence a um diretório gerenciado e deseja fechar sua conta, contate o administrador. Para obter informações sobre como determinar se você pertencer a um diretório não gerenciado, consulte [excluir o usuário do locatário não gerenciado](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant).

* Salve quaisquer dados que você deseja manter. Para obter informações sobre como enviar uma solicitação de exportação, consulte [acessando e exportar logs gerados pelo sistema para locatários não gerenciados](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants).

> [!WARNING]
> Fechando sua conta é irreversível. Quando você fechar sua conta, todos os dados pessoais serão removidos. Você não terá mais acesso à sua conta e dados associados com sua conta.

## <a name="close-your-account"></a>Encerrar sua conta

Para fechar uma conta de escola ou não gerenciado, siga estas etapas:

1. Entrar no [fechar sua conta](https://go.microsoft.com/fwlink/?linkid=873123), usando a conta que você deseja fechar.

1. Na **Minhas solicitações de dados**, selecione **fechar conta**.

    ![Minhas solicitações de dados - fechar conta](./media/users-close-account/close-account.png)

1. Examine a mensagem de confirmação e, em seguida, selecione **Sim**.

    ![Minhas solicitações de dados - confirmar fechar](./media/users-close-account/confirm-close.png)

## <a name="next-steps"></a>Próximas etapas

- [O que é o autoatendimento Inscreva-se para o Azure Active Directory?](directory-self-service-signup.md)
- [Excluir o usuário do locatário não gerenciado](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant)
- [Acessando e exportar logs gerados pelo sistema para locatários não gerenciados](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants)

---
title: Habilitar a solução de VMware do Azure pelo serviço CloudSimple
description: Descreve como habilitar o serviço de CloudSimple em uma assinatura do Azure e, em seguida, registrar o provedor de recursos CLoudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: cacd5510147ce997efec922f4b4656956a098d88
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66676932"
---
# <a name="register-the-microsoftvmwarecloudsimple-resource-provider-on-your-azure-subscription"></a>Registrar o provedor de recursos Microsoft.VMwareCloudSimple em sua assinatura do Azure

O serviço CloudSimple permite consumir solução CloudSimple do VMware do Azure. Para usar o serviço CloudSimple, ele primeiro deve ser habilitado em sua assinatura do Azure. Em seguida, você pode registrar o serviço de Microsoft.VMwareCloudSimple como seu provedor de recursos.

## <a name="enable-the-cloudsimple-service"></a>Habilitar o serviço CloudSimple

Para habilitar o serviço de CloudSimple em sua assinatura do Azure, abra uma solicitação de suporte com [o suporte da Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Selecione as seguintes opções ao enviar a solicitação.

* Tipo de problema: **Técnico**
* Assinatura: **Sua ID de assinatura**
* Tipo de serviço: **Solução do VMware CloudSimple**
* Tipo de problema: **Cota de nós dedicados**
* Subtipo de problema: **Aumentar a cota de nós dedicados**
* Assunto: **Habilitar o serviço CloudSimple**

Você também pode contatar seu representante de conta da Microsoft em [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com). Forneça sua ID de assinatura do Azure no email.  

Depois que o serviço CloudSimple está habilitado para sua assinatura, você pode habilitar o provedor de recursos na assinatura.

## <a name="register-the-resource-provider"></a>Registre o provedor de recursos

1. Entre no [Portal do Azure](https://portal.azure.com).

2. Selecione **Todos os serviços**.

3. Pesquise e selecione **assinaturas**.

    ![Selecione assinaturas](media/cloudsimple-service-select-subscriptions.png)

4. Selecione a assinatura na qual você deseja habilitar o serviço CloudSimple.

5. Clique em **provedores de recursos** para a assinatura.

6. Use **Microsoft.VMwareCloudSimple** para filtrar o provedor de recursos.

7. Selecione o provedor de recursos e clique em **registrar**.

    ![Registrar provedor de recursos](media/cloudsimple-service-enable-resource-provider.png)

## <a name="next-steps"></a>Próximas etapas

* Saiba como [criar um serviço CloudSimple](create-cloudsimple-service.md)
* Saiba como [configurar um ambiente de nuvem privada](quickstart-create-private-cloud.md)
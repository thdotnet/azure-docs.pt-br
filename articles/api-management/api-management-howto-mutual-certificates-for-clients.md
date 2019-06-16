---
title: Proteger APIs usando a autenticação de certificado do cliente no Gerenciamento de API — Gerenciamento de API do Azure | Microsoft Docs
description: Saiba como proteger o acesso às APIs usando certificados do cliente
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: apimpm
ms.openlocfilehash: 5427c4050b6b70c18da7a1899d16e448c41e81c6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427319"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Como proteger APIs usando a autenticação de certificado do cliente no Gerenciamento de API

O Gerenciamento de API fornece a capacidade de proteger o acesso às APIs (isto é, cliente para Gerenciamento de API) usando certificados do cliente. Você pode validar o certificado de entrada e verificar as propriedades do certificado em relação a valores desejados usando expressões de política.

Para obter informações sobre como proteger o acesso ao serviço de back-end de uma API usando certificados de cliente (ou seja, o gerenciamento de API para back-end), consulte [como proteger serviços de back-end usando o cliente de autenticação de certificado](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

> [!IMPORTANT]
> Para receber e verificar os certificados de cliente na camada de consumo primeiro você deve ativar o "Solicitação de certificado do cliente" configuração na folha "Domínios personalizados", conforme mostrado abaixo.

![Solicitar certificado de cliente](./media/api-management-howto-mutual-certificates-for-clients/request-client-certificate.png)

## <a name="checking-the-issuer-and-subject"></a>Verificando o emissor e a entidade

As políticas abaixo podem ser configuradas para verificar o emissor e a entidade de um certificado do cliente:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName.Name != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Para desabilitar o uso de lista de revogação de certificado verificando `context.Request.Certificate.VerifyNoRevocation()` em vez de `context.Request.Certificate.Verify()`.
> Se o certificado do cliente for autoassinado, raiz (ou intermediária) certificado (s) de AC deve ser [carregado](api-management-howto-ca-certificates.md) ao gerenciamento de API para `context.Request.Certificate.Verify()` e `context.Request.Certificate.VerifyNoRevocation()` funcione.

## <a name="checking-the-thumbprint"></a>Verificando a impressão digital

Veja abaixo que as políticas podem ser configuradas para verificar a impressão digital de um certificado do cliente:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Para desabilitar o uso de lista de revogação de certificado verificando `context.Request.Certificate.VerifyNoRevocation()` em vez de `context.Request.Certificate.Verify()`.
> Se o certificado do cliente for autoassinado, raiz (ou intermediária) certificado (s) de AC deve ser [carregado](api-management-howto-ca-certificates.md) ao gerenciamento de API para `context.Request.Certificate.Verify()` e `context.Request.Certificate.VerifyNoRevocation()` funcione.

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>Verificação de uma impressão digital em relação a certificados carregados no Gerenciamento de API

O exemplo a seguir mostra como verificar a impressão digital de um certificado do cliente em relação a certificados carregados no Gerenciamento de API:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify()  || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

> [!NOTE]
> Para desabilitar o uso de lista de revogação de certificado verificando `context.Request.Certificate.VerifyNoRevocation()` em vez de `context.Request.Certificate.Verify()`.
> Se o certificado do cliente for autoassinado, raiz (ou intermediária) certificado (s) de AC deve ser [carregado](api-management-howto-ca-certificates.md) ao gerenciamento de API para `context.Request.Certificate.Verify()` e `context.Request.Certificate.VerifyNoRevocation()` funcione.

> [!TIP]
> Problema de deadlock do certificado do cliente descrito neste [artigo](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) pode se manifestar de várias maneiras, por exemplo, solicitações de congelar, o resultado de solicitações `403 Forbidden` código de status após atingir o tempo limite, `context.Request.Certificate` é `null`. Esse problema geralmente afeta `POST` e `PUT` solicitações com comprimento de conteúdo de aproximadamente 60 KB ou maior.
> Para evitar esse problema ocorra ativar configuração de "Certificado de cliente Negotiate" para nomes de host desejado na folha de "Domínios personalizados", conforme mostrado abaixo. Esse recurso não está disponível na camada de consumo.

![Negociação de certificado do cliente](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

## <a name="next-steps"></a>Próximas etapas

-   [Como garantir serviços de back-end usando autenticação de certificado do cliente](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
-   [Como carregar certificados](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)

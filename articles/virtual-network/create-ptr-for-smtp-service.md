---
title: Configurar zonas de pesquisa inversa para uma verificação de banner de SMTP no Azure
titlesuffix: Azure Virtual Network
description: Descreve como configurar zonas de pesquisa inversa para uma verificação de banner de SMTP no Azure
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: dcscontentpm
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 084fdb7f850f3819738a982127fa98efab114197
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059031"
---
# <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Configurar zonas de pesquisa inversa para uma verificação de banner de SMTP

Este artigo descreve como usar uma zona inversa no DNS do Azure e criar um registro DNS (PTR) reverso para verificação de faixa de SMTP.

## <a name="symptom"></a>Sintoma

Se você hospedar um servidor SMTP no Microsoft Azure, poderá receber a seguinte mensagem de erro ao enviar ou receber uma mensagem de servidores de email remotos:

**554: Nenhum registro PTR**

## <a name="solution"></a>Solução

Para um endereço IP virtual no Azure, os registros inversos são criados em zonas de domínio pertencentes à Microsoft, não em zonas de domínio personalizado.

Para configurar os registros PTR em zonas pertencentes à Microsoft, use a propriedade -ReverseFqdn no recurso PublicIpAddress. Para obter mais informações, veja [Configurar DNS reverso para serviços hospedados no Azure](../dns/dns-reverse-dns-for-azure-services.md).

Quando você configurar os registros PTR, verifique se o endereço IP e o FQDN reverso pertencem à assinatura. Se você tentar definir um FQDN reverso que não pertença à assinatura, receberá a seguinte mensagem de erro:

    Set-AzPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership:
                        
    1) O ReverseFqdn realiza a correspondência do FQDN de qualquer recurso IP público na assinatura;
    2) ReverseFqdn resolve para o fqdn (através da cadeia de registros CName) de qualquer recurso ip público sob a assinatura;
    3) Ele resolve para o endereço IP (por meio do CName e da cadeia de registros A) de qualquer recurso de IP público estático na assinatura.

Se você alterar manualmente seu banner de SMTP para corresponder ao nosso FQDN padrão inverso, o servidor de email remoto ainda poderá falhar porque ele poderá esperar que o host do banner de SMTP corresponda ao registro MX para o domínio.

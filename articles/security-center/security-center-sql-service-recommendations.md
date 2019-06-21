---
title: Proteger os serviços de dados e armazenamento do Azure na Central de segurança do Azure | Microsoft Docs
description: Este documento aborda as recomendações na Central de Segurança do Azure que ajudam a proteger seus dados e o serviço SQL do Azure, bem como a cumprir as políticas de segurança.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2019
ms.author: monhaber
ms.openlocfilehash: 2ac0e4ebaafb8b0c9c79e885cecbefc5a65c1823
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275249"
---
# <a name="protect-azure-data-and-storage-services-in-azure-security-center"></a>Proteger os serviços de dados e armazenamento do Azure na Central de segurança do Azure
Este tópico mostra como exibir e implementar recomendações de segurança para recursos de dados e armazenamento. A Central de segurança do Azure encontrada essas recomendações ao analisar o estado de segurança de seus recursos do Azure.

## <a name="view-your-data-security-information"></a>Exibir suas informações de segurança de dados

1. No **higiene de troca de segurança de recursos** seção, clique em **recursos de armazenamento e dados**.

   ![Recursos de armazenamento de & dados](./media/security-center-monitoring/click-data.png)

    O **segurança de dados** página é aberta com recomendações para os recursos de dados.

     ![Recursos de dados](./media/security-center-monitoring/sql-overview.png)

Nessa página, você pode:

* Clique o **visão geral** guia lista todas as recomendações de recursos de dados a ser corrigida. 
* Clique em cada guia e exibir as recomendações do tipo de recurso.

    > [!NOTE]
    > Para saber mais sobre criptografia de armazenamento, leia [Habilitar a criptografia para a conta de armazenamento do Azure na Central de Segurança do Azure](security-center-enable-encryption-for-storage-account.md).


## <a name="remediate-a-recommendation-on-a-data-resource"></a>Corrigir uma recomendação em um recurso de dados

1. De qualquer uma das guias de recursos, clique em um recurso. A página de informações será aberta, listando as recomendações para ser corrigidas.

    ![Informações sobre o recurso](./media/security-center-monitoring/sql-recommendations.png)

2. Clique em uma recomendação. A página de recomendação é aberta e exibe a **etapas de correção** para implementar a recomendação.

   ![Etapas de correção](./media/security-center-monitoring/remediate1.png)

3. Clique em **agir**. É exibida a página de configurações de recursos.

    ![Habilitar a recomendação](./media/security-center-monitoring/remediate2.png)

4. Siga as **etapas de correção** e clique em **salvar**.

## <a name="data-and-storage-recommendations"></a>Recomendações de disco e de armazenamento

|Tipo de recurso|Classificação de segurança|Recomendações|DESCRIÇÃO|
|----|----|----|----|
|Conta de armazenamento|20|Transferência segura para contas de armazenamento deve ser habilitada|A transferência segura é uma opção que força a sua conta de armazenamento a aceitar somente solicitações de conexões seguras (HTTPS). O HTTPS garante a autenticação entre o servidor e o serviço e protege os dados em trânsito contra ataques de camada de rede, como o man-in-the-middle, interceptação e sequestro de sessão.|
|Redis|20|Somente conexões seguras para seu Cache Redis devem ser habilitadas|Habilite apenas conexões via SSL para o Cache Redis do Azure. O uso de conexões seguras garante a autenticação entre o servidor e o serviço e protege dados em trânsito de ataques de camada de rede, como ataques intermediários, interceptação e sequestro de sessão.|
|SQL|15|Transparent Data Encryption em bancos de dados SQL deve ser habilitado|Habilite a criptografia transparente de dados para proteger dados em repouso e atender aos requisitos de conformidade.|
|SQL|15|Auditoria do SQL server deve ser habilitado|Habilite auditoria para servidores SQL do Azure. (Apenas o serviço do SQL Azure. Não inclui o SQL em execução nas suas máquinas virtuais.)|
|Data Lake Analytics|5|Logs de diagnóstico do Data Lake Analytics devem ser habilitados|Ativar os logs e mantenha-os por até um ano. Isso permitirá que você recrie trilhas de atividade para fins de investigação quando ocorrer um incidente de segurança ou sua rede estiver comprometida. |
|Data Lake Store|5|Logs de diagnóstico do Azure Data Lake Store devem ser habilitados|Ativar os logs e mantenha-os por até um ano. Isso permite recriar trilhas de atividades para fins de investigação quando ocorre um incidente de segurança ou quando sua rede é comprometida. |
|SQL|30|As vulnerabilidades de seus bancos de dados SQL devem ser corrigidas.|Avaliação de vulnerabilidade de SQL examina vulnerabilidades de segurança do banco de dados e expõe quaisquer desvios das práticas recomendadas, como configurações incorretas, permissões excessivas e dados confidenciais sem proteção. Resolver as vulnerabilidades encontradas pode melhorar muito o desenvolvimento da segurança de seu banco de dados.|
|SQL|20|Provisionar um administrador do Microsoft Azure Active Directory para o servidor SQL|Provisione um administrador do Microsoft Azure Active Directory para o servidor SQL para habilitar a autenticação do Microsoft Azure Active Directory. A autenticação do Microsoft Azure Active Directory permite o gerenciamento simplificado de permissões e o gerenciamento centralizado de identidades dos usuários de banco de dados e de outros serviços da Microsoft.|
|Conta de armazenamento|15|Acesso às contas de armazenamento com firewall e configurações de rede virtual deve ser restrito|Audite o acesso irrestrito à rede nas configurações de firewall da conta de armazenamento. Em vez disso, configure as regras de rede de forma que somente aplicativos das redes permitidas podem acessar a conta de armazenamento. Para permitir conexões da Internet específica ou em clientes locais, você pode conceder acesso ao tráfego de redes virtuais do Azure de específicas ou a intervalos de endereços IP de Internet públicos.|
|Conta de armazenamento|1|Contas de armazenamento devem ser migradas para novos recursos do Azure Resource Manager|Use o novo v2 do Azure Resource Manager para suas contas de armazenamento para fornecer melhorias de segurança, como: mais forte controle de acesso (RBAC), auditoria melhor, com base no Gerenciador de recursos de implantação e governança, acesso a identidades gerenciadas, o acesso ao Cofre de chaves para segredos e a autenticação baseada no AD do Azure e suporte para marcas e os grupos de recursos para facilitar o gerenciamento de segurança.|

## <a name="see-also"></a>Consulte também
Para saber mais sobre as recomendações que se aplicam a outros tipos de recursos do Azure, consulte os tópicos a seguir:

* [Protegendo suas máquinas virtuais na Central de Segurança do Azure](security-center-virtual-machine-recommendations.md)
* [Protegendo seus aplicativos na Central de segurança do Azure](security-center-application-recommendations.md)
* [Protegendo sua rede na Central de Segurança do Azure](security-center-network-recommendations.md)

Para saber mais sobre a Central de segurança, consulte os tópicos a seguir:

* [Configurando políticas de segurança na Central de Segurança do Azure](tutorial-security-policy.md) : saiba como configurar políticas de segurança para suas assinaturas e grupos de recursos do Azure.
* [Gerenciando e respondendo a alertas de segurança na Central de Segurança do Azure](security-center-managing-and-responding-alerts.md) : aprenda a gerenciar e a responder a alertas de segurança.
* [Perguntas frequentes da Central de Segurança do Azure](security-center-faq.md) : encontre as perguntas frequentes sobre como usar o serviço.

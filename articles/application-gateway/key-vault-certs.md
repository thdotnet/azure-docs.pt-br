---
title: Terminação SSL com certificados de Cofre de chaves do Azure
description: Saiba como você pode integrar o Gateway de aplicativo do Azure com o Key Vault para certificados de servidor que estão anexados para ouvintes habilitado para HTTPS.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: victorh
ms.openlocfilehash: 18af315c58c838a7237acfbcc32f622a0edbd3b3
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827637"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>Terminação SSL com certificados do Key Vault

[O Azure Key Vault](../key-vault/key-vault-whatis.md) é um segredo de plataforma gerenciada armazena o que você pode usar para proteger segredos, chaves e certificados SSL. O Gateway de aplicativo do Azure dá suporte à integração com o Key Vault (em visualização pública) para certificados de servidor que estão anexados para ouvintes habilitado para HTTPS. Esse suporte é limitado ao v2 SKU de Gateway de aplicativo.

> [!IMPORTANT]
> Integração do Gateway de aplicativo com o Key Vault está atualmente em visualização pública. Essa visualização é fornecida sem um contrato de nível de serviço (SLA) e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos. Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Essa visualização pública oferece dois modelos para a terminação SSL:

- Você pode fornecer explicitamente os certificados SSL anexados ao ouvinte. Esse modelo é a maneira tradicional de passar os certificados SSL para o Gateway de aplicativo para a terminação SSL.
- Opcionalmente, você pode fornecer uma referência a um certificado existente do cofre da chave ou segredo quando você cria um ouvinte HTTPS habilitado.

Integração do Gateway de aplicativo com o Key Vault oferece muitos benefícios, incluindo:

- Aumentar a segurança, como certificados SSL não são tratados diretamente pela equipe de desenvolvimento do aplicativo. Integração permite que uma equipe de segurança separados para:
  * Configure gateways de aplicativo.
  * Controlar os ciclos de vida de gateway de aplicativo.
  * Conceder permissões para gateways de aplicativo selecionados para acessar certificados que são armazenados no cofre de chaves.
- Suporte para a importação de certificados existentes no seu Cofre de chaves. Ou use APIs Key Vault para criar e gerenciar novos certificados com qualquer um dos parceiros de Cofre de chaves confiáveis.
- Suporte para a renovação automática de certificados que são armazenadas no cofre de chaves.

O Gateway de aplicativo atualmente suporta apenas certificados software validado. Módulo de segurança de hardware (HSM)-não há suporte para certificados validados. Depois que o Gateway de aplicativo é configurado para usar certificados do Key Vault, suas instâncias de recuperem o certificado do Key Vault e instalação-los localmente para a terminação SSL. As instâncias também sondam o Cofre de chaves em intervalos de 24 horas para recuperar uma versão de renovação do certificado, se ele existir. Se um certificado atualizado for encontrado, o certificado SSL associado atualmente com o ouvinte HTTPS é girado automaticamente.

## <a name="how-integration-works"></a>Como funciona a integração

Integração do Gateway de aplicativo com o Key Vault requer um processo de configuração de três etapas:

1. **Criar uma identidade gerenciada atribuída pelo usuário**

   Criar ou reutilizar uma existente identidade atribuída pelo usuário gerenciada, que usa o Gateway de aplicativo para recuperar certificados de Cofre de chaves em seu nome. Para saber mais, confira [O que são identidades gerenciadas para recursos do Azure?](../active-directory/managed-identities-azure-resources/overview.md). Esta etapa cria uma nova identidade no locatário do Azure Active Directory. A identidade é confiável para a assinatura que é usada para criar a identidade.

1. **Configurar o Cofre de chaves**

   Você, em seguida, importe um certificado existente ou cria um novo em seu Cofre de chaves. O certificado será usado por aplicativos executados por meio do gateway de aplicativo. Nesta etapa, você também pode usar um segredo do Cofre de chaves que é armazenado como um sem senha, em base 64 codificados arquivo PFX. É recomendável usar um tipo de certificado por causa do recurso de renovação automática está disponível com objetos de tipo de certificado no cofre de chaves. Depois de criar um certificado ou um segredo, definir políticas de acesso no cofre de chaves para permitir que a identidade a serem concedidos *obter* acesso ao segredo.

1. **Configurar o gateway de aplicativo**

   Depois de concluir as duas etapas anteriores, você pode configurar ou modificar um gateway de aplicativo existente para usar a identidade atribuída pelo usuário gerenciada. Você também pode configurar o certificado SSL do ouvinte HTTP para apontar para o URI completo do certificado de Cofre de chaves ou ID secreta.

   ![Certificados de Cofre de chaves](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Próximas etapas

[Configura a terminação SSL com certificados do Key Vault usando o PowerShell do Azure](configure-keyvault-ps.md)

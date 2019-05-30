---
title: Publicar oferta de aplicativo de SaaS do Azure | O Azure Marketplace
description: O processo e as etapas de publicação de uma oferta de aplicativo SaaS no Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: pbutlerm
ms.openlocfilehash: 5574a7ba216eed42896d6fcb1890585f76561834
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258078"
---
# <a name="publish-a-saas-application-offer"></a>Publicar uma oferta de aplicativo do SaaS

Depois de criar uma nova oferta, fornecendo as informações sobre o **nova oferta** página, você pode publicar a oferta. Selecione **publicar** para iniciar o processo de publicação.

> [!IMPORTANT] 
> SaaS oferecem funcionalidade está sendo migrada para o [Microsoft Partner Center](https://partner.microsoft.com/dashboard/directory).  Todos os novos editores devem usar o Partner Center para criar novas ofertas de SaaS e gerenciando ofertas existentes.  Editores atuais com ofertas de SaaS estão sendo migrados batchwise do Portal de parceiro do Cloud Partner Center.  Portal do Cloud Partner exibirá mensagens de status para indicar quando as ofertas específicas do existentes foram migradas.
> Para obter mais informações, consulte [criar uma nova oferta de SaaS](../../partner-center-portal/create-new-saas-offer.md).


O diagrama a seguir mostra as etapas de alto nível para a publicação de uma oferta de aplicativo SaaS.

![Etapas de publicação da oferta](./media/offer-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Descrição detalhada de etapas de publicação

A tabela a seguir descreve cada etapa de publicação, com um tempo estimado (máximo) para concluir cada etapa.

|     **Step**       |     **Hora**      |  **Descrição**  |
|  ---------------   |  ---------------  |  ---------------  |
|         Certificação           |       2 semanas            |          A oferta é analisada pela equipe de certificação do Azure. Esta etapa realizará varreduras por vírus, malware, conformidade de segurança e problemas de segurança. Ele verificará se essa oferta atende a todos os critérios de qualificação (confira os [pré-requisitos](./cpp-prerequisites.md)). Comentários serão fornecidos se um problema for encontrado.         |
|           Empacotamento         |       1 hora            |       Os recursos técnicos da oferta são empacotados para uso do cliente e os sistemas de leads são configurados e configurados.            |
|        O editor termina            |         -          |        Revisão final do editor e confirmação antes que a oferta seja publicada. Você pode implantar sua oferta nas assinaturas selecionadas (nas etapas a informações de oferta) para verificar se ele atende a todos os seus requisitos. Selecione **Go Live** para que sua oferta possa passar para a próxima etapa.           |
|        Empacotamento            |        1 hora           |        A oferta finalizada é replicada nos sistemas e regiões de produção do mercado.           |
|        Live            |       4 dias            |         A oferta é lançada, replicada para as regiões exigidas e disponibilizada ao público.          |

Aguarde até 10 dias úteis para que o processo de publicação seja concluído e a oferta seja liberada. Depois de concluir o processo de publicação, a oferta de SaaS será listada no [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules).

## <a name="next-steps"></a>Próximas etapas

[Atualizar uma oferta existente](./cpp-update-existing-offer.md)

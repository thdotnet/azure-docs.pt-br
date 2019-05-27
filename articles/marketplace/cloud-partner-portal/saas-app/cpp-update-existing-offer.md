---
title: Atualizar oferta existente de aplicativo de SaaS do Azure | O Azure Marketplace
description: Como atualizar uma oferta existente de aplicativo SaaS no Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: pbutlerm
ms.openlocfilehash: 2195c9a5e1f0d3683ea8cf6564d97cbabd072f81
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834209"
---
# <a name="update-an-existing-saas-application-offer"></a>Atualizar uma oferta existente de aplicativo SaaS

Há vários tipos de atualizações que você talvez queira fazer na oferta depois que ela for publicada e estiver ativa. Qualquer alteração que você fizer na nova versão da sua oferta deve ser salva e republicada para ser refletida no Marketplace. Este artigo percorre os diferentes aspectos da atualização de sua oferta de SaaS no [Portal do Cloud Partner](https://cloudpartner.azure.com/).

> [!IMPORTANT] 
> SaaS oferecem funcionalidade está sendo migrada para o [Microsoft Partner Center](https://partner.microsoft.com/dashboard/directory).  Todos os novos editores devem usar o Partner Center para criar novas ofertas de SaaS e gerenciando ofertas existentes.  Editores atuais com ofertas de SaaS estão sendo migrados batchwise do Portal de parceiro do Cloud Partner Center.  Portal do Cloud Partner exibirá mensagens de status para indicar quando as ofertas específicas do existentes foram migradas.

Há várias razões por que você talvez queira atualizar sua oferta, tais como:

- Adicionar uma nova versão a um aplicativo existente.
- Atualizar um aplicativo.
- Adicionar novos recursos a um aplicativo.
- Atualizar os metadados da oferta no Marketplace.

Para ajudá-lo nessas modificações, o portal fornece os recursos **Comparar** e **Histórico**.

## <a name="unpermitted-changes-to-a-saas-offer"></a>Alterações não permitidas a uma oferta de SaaS

Há atributos de uma oferta de SaaS que não podem ser alterados depois que a oferta estiver ativa no Azure Marketplace. Você não pode alterar as configurações a seguir:

- ID da oferta e ID do Editor da oferta
- Marcas de versão, por exemplo: `1.0.1`
- Alterações no modelo de cobrança/licença para ofertas existentes.

## <a name="common-update-operations"></a>Operações comuns de atualização
 
As operações de inserção e atualização são comuns.

### <a name="update-offer-contacts"></a>Atualizar os contatos da oferta

Use as etapas a seguir para atualizar os contatos de suporte de sua oferta.

1. Entrar no [Portal do Cloud Partner](https://cloudpartner.azure.com/).
2. Em **Todas as ofertas**, localize a oferta que você gostaria de atualizar.
3. Vá para a guia **Contatos**. Atualize seus contatos.
4. Selecione **Publicar** para iniciar o fluxo de trabalho para publicar suas alterações.


### <a name="update-offer-marketplace-metadata"></a>Atualizar metadados do marketplace da oferta

Utilize os seguintes passos para atualizar os metadados do mercado associados à sua oferta. (por exemplo: nome da empresa, logotipos, etc.)

1. Entrar no [Portal do Cloud Partner](https://cloudpartner.azure.com/).
2. Em **Todas as ofertas**, localize a oferta que você gostaria de atualizar.
3. Vá até a guia **Detalhes da vitrine eletrônica**. Use as instruções no artigo [Publicar oferta de SaaS](./cpp-publish-offer.md) para fazer alterações de metadados.
4. Selecione **Publicar** para iniciar o fluxo de trabalho para publicar suas alterações.

## <a name="compare-feature"></a>Recurso Comparar

Ao fazer alterações em uma oferta publicada, é possível usar o recurso Comparar para auditar as alterações feitas. A próxima captura de tela mostra a opção de comparação para uma oferta publicada.

![Use Comparar para ver as alterações da oferta no Portal do Cloud Partner](./media/saas-offer-compare.png)

### <a name="to-use-the-compare-feature"></a>Para usar o recurso Comparar:

1. A qualquer momento do processo de edição, clique no botão Comparar para sua oferta.
2. Veja as versões lado a lado de metadados e ativos de marketing.

## <a name="publishing-history"></a>Histórico de publicação

Para ver a atividade de histórico de publicação, selecione a guia **Histórico** na barra de menu de navegação esquerda do Portal do Cloud Partner. Você pode ver as ações de data/hora executadas durante o tempo de vida das ofertas do Marketplace do Azure.

![Consulte o histórico de ofertas no Portal do Cloud Partner](./media/saas-offer-history.png)

Você pode usar a página de histórico de auditoria para pesquisar uma oferta específica e aplicar filtros, como o Editor, oferta e o tipo de evento (por exemplo, OfferGoLiveRequested.) Você também pode baixar o histórico de auditoria como um arquivo csv.


## <a name="next-steps"></a>Próximas etapas

[Oferta de aplicativo SaaS](./cpp-saas-offer.md)
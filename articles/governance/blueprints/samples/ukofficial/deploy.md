---
title: As etapas de implantação de exemplo - OFICIAL do Reino Unido e NHS do Reino Unido blueprint-
description: As etapas do OFICIAL do Reino Unido e exemplos de especificações técnicas do Reino Unido NHS de implantação.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/26/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 43aae882f27031d3e51ac8a4f5a68d243a973d6d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67453197"
---
# <a name="deploy-the-uk-official-and-uk-nhs-blueprint-samples"></a>Implantar o OFICIAL do Reino Unido e exemplos de especificações técnicas do NHS do Reino Unido

Para implantar o OFICIAL do Reino Unido e exemplos de especificações técnicas do NHS do Reino Unido, as etapas a seguir devem ser executadas:

> [!div class="checklist"]
> - Criar um blueprint com base na amostra
> - Marcar sua cópia do exemplo como **Publicado**
> - Atribuir a sua cópia do blueprint a uma assinatura existente

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free) antes de começar.

## <a name="create-blueprint-from-sample"></a>Criar um blueprint com base na amostra

Primeiro, implemente a amostra de blueprint criando um blueprint no ambiente usando a amostra como ponto de partida.

1. Selecione **Todos os serviços** e pesquise e selecione **Política** no painel esquerdo. Na página **Política**, selecione **Blueprints**.

1. Na página **Introdução** à esquerda, selecione o botão **Criar** em _Criar um blueprint_.

1. Localizar o **OFICIAL do Reino Unido** ou **NHS do Reino Unido** exemplo de plano gráfico em _outros exemplos_ e selecione **usar este exemplo**.

1. Insira as informações _Básicas_ do exemplo de blueprint:

   - **Nome do blueprint**: forneça um nome para a sua cópia do exemplo de blueprint.
   - **Localização da definição**: Use as reticências e selecione o grupo de gerenciamento em que deseja salvar a cópia da amostra.

1. Selecione a guia _Artefatos_ na parte superior da página ou clique em **Avançar: Artefatos** na parte inferior da página.

1. Examine a lista de artefatos que compõem o exemplo de blueprint. Muitos dos artefatos têm parâmetros que definiremos mais tarde. Selecione **Salvar Rascunho** quando terminar de examinar o exemplo de blueprint.

## <a name="publish-the-sample-copy"></a>Publicar a cópia do exemplo

Agora a cópia do exemplo de blueprint foi criada em seu ambiente. Ela é criada no modo **Rascunho** e deve ser **Publicada** antes de ser atribuída e implantada. A cópia do exemplo de plano gráfico pode ser personalizada ao seu ambiente e as necessidades, mas essa modificação pode movê-lo para fora do padrão.

1. Selecione **Todos os serviços** e pesquise e selecione **Política** no painel esquerdo. Na página **Política**, selecione **Blueprints**.

1. Selecione a página **Definições de Blueprint** à esquerda. Use os filtros para localizar a cópia da amostra de blueprint e, em seguida, selecione-a.

1. Selecione **Publicar blueprint** na parte superior da página. Na nova página à direita, informe a **Versão** da sua cópia da amostra de blueprint. Essa propriedade será útil se você fizer uma modificação mais tarde. Fornecer **alterar notas** , como "primeira versão publicada do exemplo de especificação técnica OFICIAL do Reino Unido ou NHS do Reino Unido." Em seguida, selecione **Publicar** na parte inferior da página.

## <a name="assign-the-sample-copy"></a>Atribuir a cópia de exemplo

Quando a cópia do exemplo de blueprint for **Publicada** com êxito, ele poderá ser atribuído a uma assinatura do grupo de gerenciamento em que ele foi salvo. Esta é a etapa em que os parâmetros são fornecidos para tornar exclusiva cada implantação da cópia do exemplo de blueprint.

1. Selecione **Todos os serviços** e pesquise e selecione **Política** no painel esquerdo. Na página **Política**, selecione **Blueprints**.

1. Selecione a página **Definições de Blueprint** à esquerda. Use os filtros para localizar a cópia da amostra de blueprint e, em seguida, selecione-a.

1. Selecione **Atribuir blueprint** na parte superior da página de definição de blueprint.

1. Forneça os valores de parâmetro para a atribuição de blueprint:

   - Noções básicas

     - **Assinaturas**: Selecione uma ou mais das assinaturas que estão no grupo de gerenciamento em que você salvou a cópia do exemplo de blueprint. Se você selecionar mais de uma assinatura, será criada uma atribuição para cada uma, usando os parâmetros inseridos.
     - **Nome da atribuição**: O nome é pré-preenchido para você com base no nome do blueprint.
       Altere-o conforme necessário ou mantenha-o como está.
     - **Localização**: Selecione uma região para a identidade gerenciada a ser criada. O Blueprint do Azure usa essa identidade gerenciada para implantar todos os artefatos no blueprint atribuído. Para saber mais, veja [identidades gerenciadas para recursos do Azure](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Versão de definição de blueprint**: Escolha uma versão **Publicada** da cópia da amostra de blueprint.

   - Bloquear atribuição

     Selecione a configuração de bloqueio de blueprint para o seu ambiente. Para obter mais informações, consulte [bloqueio de recursos de projetos](../../concepts/resource-locking.md).

   - Identidade Gerenciada

     Deixe a opção de identidade gerenciada _atribuída ao sistema_ padrão.

   - Parâmetros do artefato

     Os parâmetros definidos nesta seção se aplicam ao artefato sob o qual ele está definido. Esses são [parâmetros dinâmicos](../../concepts/parameters.md#dynamic-parameters), pois são definidos durante a atribuição do blueprint. Para obter uma lista completa ou parâmetros de artefato e suas descrições, confira a [Tabela de parâmetros de artefato](#artifact-parameters-table).

1. Depois que todos os parâmetros forem inseridos, selecione **Atribuir** na parte inferior da página. A atribuição de blueprint é criada, e a implantação de artefato é iniciada. A implantação leva aproximadamente uma hora. Para verificar o status da implantação, abra a atribuição de blueprint.

> [!WARNING]
> As amostras internas de blueprint e o serviço Azure Blueprints são **gratuitos**. Os recursos do Azure são [precificados por produto](https://azure.microsoft.com/pricing/). Use a [Calculadora de Preços](https://azure.microsoft.com/pricing/calculator/) para estimar o custo da execução de recursos implantados por essa amostra de blueprint.

## <a name="artifact-parameters-table"></a>Tabela de parâmetros de artefato

A seguinte tabela fornece uma lista dos parâmetros de artefato de blueprint:

Nome do artefato|Tipo de artefato|Nome do parâmetro|DESCRIÇÃO|
|-|-|-|-|
|Especificações técnicas de iniciativa para OFICIAL do Reino Unido ou NHS do Reino Unido|Atribuição de política |Tipos de recursos para auditar os logs de diagnóstico (política: Especificações técnicas de iniciativa para OFICIAL do Reino Unido ou NHS do Reino Unido) |Lista de tipos de recurso de auditoria se a configuração de log de diagnóstico é habilitada de observação.  Para os valores aceitáveis, consulte [com suporte a serviços, esquemas e categorias para os Logs de diagnóstico do Azure](../../../../azure-monitor/platform/diagnostic-logs-schema.md). |
|[Versão prévia]: Implantar o Agente do Log Analytics para VMs do Linux |Atribuição de política |Opcional: Lista de imagens VM que tem suporte para o sistema operacional Linux para adicionar ao escopo (política: [Preview]: Implantar o agente do Log Analytics para VMs do Linux) |(Opcional) Valor padrão é _none_. Para obter mais informações, consulte [criar um espaço de trabalho do Log Analytics no portal do Azure](../../../../azure-monitor/learn/quick-create-workspace.md). |
|[Versão prévia]: Implantar o Agente do Log Analytics para VMs do Windows |Atribuição de política |Opcional: Lista de imagens VM que têm suporte para o sistema operacional Windows para adicionar ao escopo (política: [Preview]: Implantar o agente do Log Analytics para VMs do Windows) |(Opcional) Valor padrão é _none_. Para obter mais informações, consulte [criar um espaço de trabalho do Log Analytics no portal do Azure](../../../../azure-monitor/learn/quick-create-workspace.md). |

## <a name="next-steps"></a>Próximas etapas

Agora que você tiver revisado as etapas para implantar o OFICIAL do Reino Unido e os exemplos de especificações técnicas do Reino Unido NHS, visite os seguintes artigos para saber mais sobre a visão geral e o mapeamento de controle:

> [!div class="nextstepaction"]
> [Plantas OFICIAL do Reino Unido e do Reino Unido NHS - visão geral](./index.md)
> [OFICIAL do Reino Unido e planos gráficos NHS do Reino Unido – mapeamento de controle](./control-mapping.md)

Outros artigos sobre blueprints e como usá-los:

- Saiba mais sobre o [ciclo de vida do blueprint](../../concepts/lifecycle.md).
- Saiba como usar [parâmetros estáticos e dinâmicos](../../concepts/parameters.md).
- Saiba como personalizar a [ordem de sequenciamento de blueprint](../../concepts/sequencing-order.md).
- Saiba como usar o [bloqueio de recurso de blueprint](../../concepts/resource-locking.md).
- Saiba como [atualizar atribuições existentes](../../how-to/update-existing-assignments.md).

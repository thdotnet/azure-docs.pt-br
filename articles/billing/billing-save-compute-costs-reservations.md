---
title: O que são Reservas do Azure?
description: Saiba mais sobre as Reservas do Azure e os preços para economizar nos custos de máquinas virtuais, de bancos de dados SQL, do Azure Cosmos DB e de outros recursos.
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: banders
ms.openlocfilehash: cd0a70aa0fb5096c5b0157ae078c961da03109bc
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565303"
---
# <a name="what-are-azure-reservations"></a>O que são Reservas do Azure?

As Reservas do Azure ajudam a economizar pagando previamente por um ano ou três anos de máquina virtual, de capacidade de computação do Banco de Dados SQL, de produtividade do Azure Cosmos DB ou de outros recursos do Azure. Pagar previamente permite que você obtenha um desconto nos recursos que você usar. As reservas podem reduzir significativamente os custos com máquinas virtuais, computação do Banco de Dados SQL, Azure Cosmos DB ou outros recursos em até 72% nos preços pagos conforme o uso. As reservas fornecem um desconto de cobrança e não afetam o estado de tempo de execução dos recursos.

Você pode comprar uma instância reservada no [portal do Azure](https://ms.portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

## <a name="why-buy-a-reservation"></a>Por que comprar uma reserva?

Se você tiver máquinas virtuais, Azure Cosmos DB ou bancos de dados SQL executadas por longos períodos de tempo, comprar uma reserva dá a opção mais econômica. Por exemplo, quando você executa continuamente quatro instâncias de um serviço sem uma reserva, você será cobrado com taxas pré-pagas. Se você comprar uma reserva para esses recursos, você obtém imediatamente o desconto de reserva. Os recursos não serão mais cobrados com base nas taxas pagas conforme o uso.

## <a name="charges-covered-by-reservation"></a>Encargos cobertos por reserva

Planos de serviço:

- **Instância de máquina Virtual reservada** -uma reserva apenas abrange os custos de computação de máquina virtual. Não cobre encargos adicionais de software, rede e armazenamento.
- **Capacidade reservada do Azure Cosmos DB** -uma reserva abrange a taxa de transferência provisionada para os seus recursos. Ela não cobre encargos de armazenamento e rede.
- **Banco de dados SQL reservadas vCore** : somente os custos de computação estão incluídos com uma reserva. A licença é cobrada separadamente.

Para as máquinas virtuais do Windows e banco de Dados SQL, você pode cobrir os custos de licenciamento com o [Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Quem é elegível para comprar uma reserva?

Para comprar um plano, você deve ter uma função de proprietário de assinatura em um Enterprise (MS-AZR - 0017p ou MS-AZR - 0148p) ou uma assinatura pré-paga (MS-AZR - 003p ou MS-AZR - 0023P). Provedores de soluções de nuvem podem usar o portal do Azure ou [Partner Center](/partner-center/azure-reservations) para comprar reservas do Azure.

Os clientes do EA podem limitar as compras para administradores EA desabilitando o **adicionar instâncias reservadas** opção no Portal do EA. Os administradores EA devem ser um proprietário da assinatura pelo menos uma assinatura de EA para comprar uma reserva. A opção é útil para empresas que querem uma equipe centralizada para comprar reservas para centros de custo diferentes. Após a compra, equipes centralizadas podem adicionar os proprietários do Centro de custo para as reservas. Proprietários, em seguida, podem definir o escopo de reserva para suas assinaturas. A equipe central não precisa ter acesso de proprietário de assinatura em que a reserva é adquirida.

Um desconto de reserva se aplica somente aos recursos associados a assinaturas adquiridas por meio de planos individuais com taxas pré-pagas, CSP e Enterprise.

## <a name="scope-reservations"></a>Reservas de escopo

Você pode definir o escopo de uma reserva para uma assinatura ou grupos de recursos. Definindo o escopo de uma reserva seleciona em que a economia de reserva se aplica. Quando você definir o escopo de reserva para um grupo de recursos, os descontos de reserva se aplicam somente ao grupo de recursos — não toda a assinatura.

### <a name="reservation-scoping-options"></a>Opções de escopo de reserva

Com o recurso de grupo de escopo que você tem três opções para definir o escopo de uma reserva, dependendo das suas necessidades:

- **Um único escopo de grupo de recursos** — se aplica o desconto de reserva para os recursos correspondentes no somente o grupo de recursos selecionado.
- **Um único escopo de assinatura** — se aplica o desconto de reserva para os recursos correspondentes na assinatura selecionada.
- **Escopo compartilhado** — se aplica o desconto de reserva a correspondência de recursos em assinaturas qualificadas que estão no contexto de cobrança. Para clientes do Enterprise Agreement, o contexto de cobrança é o registro. Para assinaturas individuais com taxas pagas conforme o uso, o escopo de cobrança é todas as assinaturas qualificadas criadas pelo administrador da conta.

Durante a aplicação de descontos de reserva no seu uso, o Azure processa a reserva na seguinte ordem:

1. Reservas às quais têm o escopo para um grupo de recursos
2. Reservas de escopo único
3. Reservas de escopo compartilhado

Um único grupo de recursos pode obter descontos de reserva de várias reservas, dependendo de como você definir o escopo de suas reservas.

### <a name="scope-a-reservation-to-a-resource-group"></a>Definir o escopo de uma reserva de um grupo de recursos

Você pode definir o escopo de reserva para um grupo de recursos quando você compra a reserva, ou definir o escopo após a compra. Você deve ser um proprietário de assinatura para definir o escopo de reserva para um grupo de recursos.

Para definir o escopo, vá para o [comprar a reserva](https://ms.portal.azure.com/#blade/Microsoft\_Azure\_Reservations/CreateBlade/referrer/Browse\_AddCommand) página no portal do Azure. Em seguida, selecione o tipo de reserva que você deseja comprar. No **selecione o produto que você deseja adquirir** formulário de seleção, altere o **escopo** valor para **único grupo de recursos** e selecione um grupo de recursos.

![Exemplo mostrando a seleção de compra de reserva de VM](./media/billing-save-compute-costs-reservations/select-product-to-purchase.png)

Recomendações de compra para o grupo de recursos na máquina virtual reserva são mostradas. As recomendações são calculadas pela análise do seu uso nos últimos 30 dias. Uma recomendação de compra será feita se o custo da execução de recursos com as instâncias reservadas é mais barato do que o custo da execução de recursos com as taxas pagas conforme o uso. Para obter mais informações sobre as recomendações de compra de reserva, consulte a [recomendações de compra de instância reservada obter com base no padrão de uso](https://azure.microsoft.com/blog/get-usage-based-reserved-instance-recommendations) postagem de blog.

Você sempre pode atualizar o escopo depois de comprar uma reserva. Para fazer isso, vá para a reserva, clique em **configuração** e rescope a reserva. Compor escopo de uma reserva não é uma transação comercial. O termo de reserva não é alterado. Para obter mais informações sobre como atualizar o escopo, consulte [atualizar o escopo depois que você comprar uma reserva](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

![Exemplo mostrando uma alteração de escopo de reserva](./media/billing-save-compute-costs-reservations/rescope-reservation-resource-group.png)

### <a name="monitor-and-optimize-reservation-usage"></a>Monitorar e otimizar o uso de reserva

Você pode monitorar o uso de reserva de várias maneiras – por meio do portal do Azure, por meio de APIs ou por meio de dados de uso. Para ver todas as reservas de que você tem acessem, acesse **reservas** no portal do Azure. A grade de reservas mostra o último percentual de utilização gravado para a reserva. Clique em reserva para ver a utilização de longo prazo da reserva.

Você também pode obter a utilização de reserva usando [APIs](billing-reservation-apis.md#see-reservation-usage) e de seus [dados de uso](billing-understand-reserved-instance-usage-ea.md#common-cost-and-usage-tasks) se você for um cliente do enterprise agreement.

Se você perceber que a utilização de seu grupo de recursos com escopo de reserva é baixa, em seguida, você pode atualizar o escopo de reserva para a assinatura única ou compartilhá-lo entre o contexto de cobrança. Também pode dividir a reserva e aplicar as reservas resultantes para diferentes grupos de recursos.

### <a name="other-considerations"></a>Outras considerações

Se você não tiver recursos em um grupo de recursos correspondentes, a reserva será subutilizada. A reserva automaticamente não se aplica a um grupo de recursos diferente ou a assinatura em que há pouca utilização.

Um escopo de reserva não é atualizado automaticamente se você mover o grupo de recursos de uma assinatura para outra. Você precisará rescope a reserva. Caso contrário, a reserva será subutilizada.

## <a name="discounted-subscription-and-offer-types"></a>Tipos de assinatura e a oferta com desconto

Descontos de reserva se aplica às seguintes assinaturas qualificadas e tipos de oferta.

- Contrato Enterprise (números de oferta: MS-AZR-0017P ou MS-AZR - 0148p)
- Planos individuais com tarifas pré-pagas (números de oferta: MS-AZR-0003P ou MS-AZR - 0023 P)
- Assinaturas de CSP

Os recursos executados em uma assinatura com outros tipos de oferta não recebem o desconto de reserva.

## <a name="how-is-a-reservation-billed"></a>Como uma reserva é cobrada?

A reserva é cobrada no método de pagamento associado à assinatura. Se você tiver uma assinatura do Enterprise, o custo de reserva será deduzido do seu saldo de investimento. Se o saldo de investimento não cobrir o custo da reserva, você será cobrado pelo excedente. Se você tiver uma assinatura de um plano individual com taxas pagas conforme o uso, o cartão de crédito em sua conta é cobrado imediatamente. Se você for cobrado por fatura, verá os encargos na sua próxima fatura.

## <a name="how-reservation-discount-is-applied"></a>Como o desconto de reserva é aplicado

O desconto de reserva se aplica a correspondência de atributos que você selecionar quando você compra a reserva o uso de recursos. Os atributos incluem o escopo em que as VMs, os banco de dados SQL, o Azure Cosmos DB ou outros recursos correspondentes são executados. Por exemplo, se você quiser um desconto de reserva para quatro máquinas virtuais de D2 padrão na região Oeste dos EUA, em seguida, selecione a assinatura onde as VMs estão em execução.

Um desconto de reserva é "*uso-it-ou-perder-it*". Se você não tiver recursos correspondentes para qualquer hora, em seguida, você perderá uma quantidade de reserva para essa hora. Você não pode transportar encaminhar horas reservadas não utilizadas.

Quando você desligar um recurso, o desconto de reserva se aplica automaticamente a outro recurso correspondente no escopo especificado. Se nenhum recurso de correspondência é encontrado no escopo especificado, são as horas reservadas *perdido*.

Por exemplo, mais tarde você pode criar um recurso e tem uma reserva de correspondência é subutilizada. Neste exemplo, o desconto de reserva se aplica automaticamente para o novo recurso correspondente.

Se as máquinas virtuais são executadas em diferentes assinaturas em sua conta ou registro, então, selecione o escopo como compartilhado. O escopo compartilhado permite que o desconto de reserva seja aplicado entre assinaturas. Você pode alterar o escopo depois de comprar uma reserva. Para obter mais informações, consulte [Gerenciar Reservas do Azure](billing-manage-reserved-vm-instance.md).

Um desconto de reserva se aplica somente aos recursos associados à Enterprise, CSP, ou assinaturas com pagamento medida que vá taxas. Os recursos executados em uma assinatura com outros tipos de oferta não recebem o desconto de reserva.

## <a name="when-the-reservation-term-expires"></a>Quando o prazo de reserva expira

No final do prazo de reserva, o desconto de cobrança expira e a máquina virtual, o Banco de Dados SQL, o Azure Cosmos DB ou outro recurso é cobrado pelo preço pago conforme o uso. As Reservas do Azure não são renovadas automaticamente. Para continuar recebendo o desconto de cobrança, você deve comprar uma nova reserva para serviços e softwares elegíveis.

## <a name="discount-applies-to-different-sizes"></a>Desconto se aplica às tamanhos diferentes

Quando você compra uma reserva, o desconto pode ser aplicado a outras instâncias com atributos que estão dentro do mesmo grupo de tamanho. Esse recurso é conhecido como a flexibilidade de tamanho de instância. A flexibilidade da cobertura de desconto depende do tipo de reserva e dos atributos que você escolhe quando compra a reserva.

Planos de serviço:

- Instâncias de VM reservadas: Quando você comprar a reserva e seleciona **otimizado para**: **flexibilidade de tamanho de instância**, a cobertura de desconto depende do tamanho da VM que você selecionar. A reserva pode ser aplicada aos tamanhos de VMs (máquinas virtuais) no mesmo grupo de série de tamanho. Para obter mais informações, confira [Flexibilidade de tamanho de máquina virtual com Instâncias de VM Reservadas](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Capacidade reservada do Banco de Dados SQL: A cobertura de desconto depende do nível de desempenho escolhido. Para obter mais informações, confira [Entender como um desconto de reserva do Azure é aplicado](billing-understand-reservation-charges.md).
- Capacidade reservada do Azure Cosmos DB: A cobertura de desconto depende da taxa de transferência provisionada. Para obter mais informações, confira [Entender como um desconto de reserva do Azure Cosmos DB é aplicado](billing-understand-cosmosdb-reservation-charges.md).

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Se você tiver dúvidas ou precisar de Ajuda, [criar uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre as Reservas do Azure com os seguintes artigos:
    - [Gerenciar Reservas do Azure](billing-manage-reserved-vm-instance.md)
    - [Entender o uso de reserva para a sua assinatura com taxas pagas conforme o uso](billing-understand-reserved-instance-usage.md)
    - [Entender o uso de reserva para seu registro de empresa](billing-understand-reserved-instance-usage-ea.md)
    - [Custos de software do Windows não estão incluídos nas reservas](billing-reserved-instance-windows-software-costs.md)
    - [Reservas do Azure no programa de CSP (Provedor de Soluções na Nuvem) do Partner Center](/partner-center/azure-reservations)

- Saiba mais sobre as reservas de planos do serviço:
    - [Máquinas virtuais com instâncias de VM reservadas do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Capacidade reservada de recursos do BD Cosmos do Azure com o Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Capacidade reservada de recursos de computação de banco de dados SQL com o banco de dados SQL](../sql-database/sql-database-reserved-capacity.md) Saiba mais sobre reservas para planos de software:
    - [Planos de software Red Hat das reservas do Azure](../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Planos de software do SUSE das reservas do Azure](../virtual-machines/linux/prepay-suse-software-charges.md)

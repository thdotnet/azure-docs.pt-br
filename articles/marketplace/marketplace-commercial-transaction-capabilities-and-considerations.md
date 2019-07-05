---
title: Recursos e Considerações sobre Transações Comerciais do Marketplace | Azure
description: Este artigo descreve as considerações de preço, faturamento, faturamento e pagamento da transação para um tipo de oferta.
services: Azure, Marketplace, Compute, Storage, Networking, Transact Offer Type
author: yijenj
manager: nuno costa
ms.service: marketplace
ms.topic: article
ms.date: 10/29/2018
ms.author: yijenj
ms.openlocfilehash: d266b314f19979578b7e7b8de4e7a7090200c9d2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445447"
---
# <a name="commercial-marketplace-transaction-capabilities-and-considerations"></a>Considerações e recursos de transações comerciais do marketplace

Este artigo aborda os seguintes tópicos relacionados ao comércio para o marketplace comercial

* Opções de publicação no marketplace
* Visão geral de transações
* Transact modelos de cobrança
* Transact requisitos

## <a name="marketplace-publishing-options"></a>Opções de publicação no marketplace

As seguintes opções de publicação estão disponíveis para editores de marketplace comercial.

### <a name="list--trial-publishing-options"></a>Opções de publicação de lista e avaliação

Os editores podem aproveitar a lista, avaliação e BYOL opções de publicação do promocionais e fins de aquisição de usuário. Com essas opções, Microsoft não participa diretamente as transações de licença de software do Editor e não há nenhuma taxa de transação associado. Os editores são responsáveis por dar suporte a todos os aspectos da transação de licença de software, incluindo, entre outros: pedido, atendimento, medição, faturamento, faturamento, pagamento e cobrança. Com as opções de publicação de lista e avaliação, os editores mantêm 100% dos valores de licenciamento de software do editor coletadas do cliente. 

### <a name="transact-publishing-option"></a>Opção de publicação de transação

Além da lista e avaliação opções de publicação, a opção de publicação transact está disponível para os editores. Isso tira proveito dos recursos de comércio disponível globalmente da Microsoft e permite que a Microsoft para transações de marketplace de nuvem do host em nome do Editor.

## <a name="transact-general-overview"></a>Visão geral de transações

Ao usar a opção de publicação de transação, a Microsoft permite que a venda de software de terceiros e a implantação de alguns tipos de oferta para a assinatura do cliente do Azure. O publicador deve considerar a cobrança das taxas de infraestrutura, e do publicador software taxas de licenciamento, ao selecionar uma cobrança de modelo e tipo de oferta.

A opção de publicação Transact atualmente tem suporte para os seguintes tipos de oferta: Máquinas virtuais, aplicativos do Azure e aplicativos SaaS.


![[Transacionar Enterprise lida no Marketplace do Azure]](./media/marketplace-publishers-guide/Transact-enterprise-deals.png)

### <a name="billing-infrastructure-costs"></a>Custos de infra-estrutura de faturamento

**Para máquinas virtuais e aplicativos do Azure**

Para máquinas virtuais e aplicativos do Azure, os valores de uso da infraestrutura do Azure são cobradas da assinatura do Azure do cliente.  Os valores de uso da infraestrutura são precificadas e apresentadas separadamente dos valores de licenciamento do provedor de software na fatura do cliente.

**Para aplicativos SaaS**

Para aplicativos SaaS, o editor deve contabilizar os valores de uso da infraestrutura do Azure e os valores de licenciamento de software como um item de custo único.  Ela é representada como uma taxa fixa para o cliente. O uso da infraestrutura do Azure é gerenciado e faturado diretamente para o parceiro.  Os valores reais de uso da infraestrutura não são vistos pelo cliente.  Em geral, os editores optam por incluir os valores de uso da infraestrutura do Azure em seus preços de licença de software.  Os valores de licenciamento de software não são medidos ou baseados no consumo.

## <a name="transact-billing-models"></a>Transact modelos de cobrança

Dependendo da opção de transação usada, os valores de licença de software do editor podem ser apresentados da seguinte forma:  

* Grátis: Sem custo para licenças de software. 

* Traga sua própria licença (BYOL): Todas as cobranças aplicáveis por licenças de software são gerenciadas diretamente entre o editor e o cliente. A Microsoft só passa pelos valores de uso da infraestrutura do Azure. (Máquinas Virtuais e Aplicativos do Azure somente).

* Pago conforme o uso: Os valores da licença de software são apresentadas como uma taxa de preços por hora, por núcleo (vCPU) com base na infraestrutura do Azure usada. Isso se aplica apenas a máquinas virtuais e aplicativos do Azure.

* • Preço da assinatura: Taxas de licença de software são apresentadas como uma mensal ou anual, recorrente a tarifa cobrada como uma taxa fixa ou por estação. Isso se aplica somente a aplicativos SaaS e aplicativos gerenciados do Azure.

* Avaliação de software gratuita: Sem cobrança de licenças de software por 30 dias ou 90 dias.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Preços gratuitos e traga sua própria licença (BYOL)

Ao publicar uma oferta de transação gratuita ou com licença própria, a Microsoft não desempenha nenhum papel na facilitação da transação de vendas para os valores de licença de software. Como a lista e as opções de publicação de avaliação, o editor mantém 100% dos valores de licença de software. 

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Preço pré-pago e assinatura (site-based)

WPay-como-você-go e quando publicar uma oferta de transação pagamento conforme o uso ou assinatura, a Microsoft fornece a tecnologia e serviços para processar a licença de software de compras, retorna o preço de assinatura e estornos. Nesse cenário, o editor autoriza a Microsoft a agir como um agente para essas finalidades. A editora permite que a Microsoft facilite a transação de licenciamento de software, mantendo sua designação como vendedor, provedor, distribuidor e licenciador.

Microsoft permite que os clientes ordem, licença e usa um software publisher, sujeitos aos termos e condições do mercado de comercial da Microsoft e o contrato de licenciamento de usuário final do Editor. Os editores devem fornecer seu contrato de licenciamento do usuário final ou selecione o [contrato padrão](https://docs.microsoft.com/azure/marketplace/standard-contract) ao criar a oferta.


### <a name="free-software-trials"></a>Avaliações de software livre

Para cenários de publicação em transações, o editor pode disponibilizar uma licença de software gratuitamente por 30 dias ou 90 dias. Esse recurso de desconto não inclui o custo do uso da infraestrutura do Azure que é impulsionado pelo uso da solução de parceiro.

### <a name="private-offers"></a>Ofertas privadas

Além de usar tipos de oferta e cobrança modelos para lucrar com uma oferta, os editores podem transact uma oferta privada, completa com preços negociado e específicos do negócio ou configurações personalizadas. As ofertas privadas são suportadas por todas as três opções de publicação de transações.

Essa opção permite maior ou menor preço que a oferta disponível publicamente. Ofertas privadas podem ser usadas para desconto ou adicionar um prêmio para uma oferta. As ofertas privadas podem ser disponibilizadas para um ou mais clientes por meio de white listando sua assinatura do Azure no nível da oferta.


### <a name="examples"></a>Exemplos

**Pré-paga** 

* Se você ativar a opção Pague conforme o uso, terá a seguinte estrutura de custos.

|Custo de sua licença  | US $ 1,00 por hora  |
|---------|---------|
|Custo de uso do Azure (D1/1-Núcleo)    |   US $ 0,14 por hora     |
|*O cliente é cobrado pela Microsoft*    |  *US $ 1,14 por hora*       |

* Nesse cenário, a Microsoft fatura US$ 1,14 por hora pelo uso da sua imagem de VM publicada.

|Faturas da Microsoft  | US$ 1,14 por hora  |
|---------|---------|
|A Microsoft paga para você 80% do seu custo de licença|   US $ 0,80 por hora     |
|A Microsoft mantém 20% do seu custo de licença  |  US $ 0,20 por hora       |
|A Microsoft mantém a 100% do custo de uso do Azure | US $ 0,14 por hora |

**Traga sua própria licença (BYOL)**

* Se você habilitar a opção de BYOL, você tem a seguinte estrutura de custo.

|Custo de sua licença  | Valor de licença negociada e faturada por você  |
|---------|---------|
|Custo de uso do Azure (D1/1-Núcleo)    |   US $ 0,14 por hora     |
|*O cliente é cobrado pela Microsoft*    |  *US $ 0,14 por hora*       |

* Nesse cenário, a Microsoft fatura US$ 0,14 por hora pelo uso da sua imagem de VM publicada.

|Faturas da Microsoft  | US $ 0,14 por hora  |
|---------|---------|
|A Microsoft mantém o custo de uso do Azure    |   US $ 0,14 por hora     |
|A Microsoft mantém 0% do seu custo de licença   |  US $ 0.00 por hora       |

**Assinatura de SaaS App**

Essa opção deve ser configurada para venda por meio da Microsoft e pode ser calculada em uma taxa fixa ou por usuário em uma base mensal ou anual.
• Se você habilitar a origem de venda por meio da opção da Microsoft para uma oferta de SaaS, você terá a seguinte estrutura de custo.

|Custo de sua licença       | US $100,00 por mês  |
|--------------|---------|
|Custo de uso do Azure (D1/1-Núcleo)    | Cobrados diretamente para o publicador, não o cliente |
|*O cliente é cobrado pela Microsoft*    |  *US $ 100,00 por mês (observação: o editor deve contabilizar quaisquer custos incorridos ou de infraestrutura de passagem no valor da licença)*  |

* Nesse cenário, a Microsoft cobra US $ 100,00 pela sua licença de software e paga US $ 80,00 ao editor.
* Parceiros que qualificaram para a taxa de serviço do Marketplace reduzido verá que uma taxa de transação reduzido o SaaS oferece desde maio de 2019 até de 2020 de junho. Nesse cenário, a Microsoft cobra $100,00 pelos sua licença de software e paga out US $90,00 ao publicador.

|Faturas da Microsoft  | US $100,00 por mês  |
|---------|---------|
|A Microsoft paga para você 80% do seu custo de licença <br> \* A Microsoft paga 90% do seu custo de licença para todos os aplicativos SaaS qualificados   |   US $80,00 por mês <br> \* US $90,00 por mês    |
|A Microsoft mantém 20% do seu custo de licença <br> \* A Microsoft mantém a 10% do seu custo de licença para todos os aplicativos SaaS qualificados.  |  US $20,00 por mês <br> \* $10.00     |

* **Tarifa de serviço do Marketplace reduzido:** Para determinados produtos de SaaS que você publicar em nosso Marketplace comerciais, Microsoft reduzirá sua taxa de serviço do Marketplace de 20% (conforme descrito no contrato de Editor do Microsoft) a 10%.  Em ordem para o seu produto qualificar, pelo menos um dos seus produtos deve ser designado pela Microsoft como IP co-sell ready ou priorizada de venda conjunta de IP. Para receber essa taxa de serviço do Marketplace reduzida para o mês, qualificação deve ser atendida pelo menos cinco (5) dias úteis antes do final do mês do calendário. Reduzido o serviço Marketplace taxa não se aplicam a VMs, aplicativos gerenciados ou quaisquer outros produtos disponibilizados por meio de nosso Marketplace comercial.  Essa taxa de serviço do Marketplace reduzido estará disponível para ofertas qualificadas, com encargos de licença, coletados pela Microsoft entre 1 de maio de 2019 e 30 de junho de 2020.  Após esse tempo, a taxa de serviço do Marketplace retornará ao seu valor normal.

### <a name="customer-invoicing-payment-billing-and-collections"></a>Faturamento, pagamento, faturamento e cobrança do cliente

**Faturamento e pagamento**

O editor pode usar o método de faturamento preferido do cliente para fornecer valores de licença de software de assinatura ou PAYGO.

**Contrato Enterprise** 

Se o método de faturamento preferido do cliente for o Microsoft Enterprise Agreement, os valores de licença de software serão cobrados usando esse método de faturamento como um custo discriminado, separado dos custos de uso específicos do Azure.

**Cartões de crédito e a fatura mensal** 

Os clientes também podem pagar usando um cartão de crédito e uma fatura mensal. Nesse caso, os valores de licença de software serão cobrados da mesma forma que o cenário do Enterprise Agreement, como um custo discriminado, separado dos custos de uso específicos do Azure.

Por exemplo, se o cliente compra usando um cartão de crédito:

|DESCRIÇÃO    |    Data  |
|----------|----------|
|Período de ordem   | 15 de agosto de 2018 – 30 de agosto de 2018 |
|Prazo final (mês)   | 30 de agosto de 2018 |
|Data de cobrança | 1 de setembro de 2018 |
|Data de pagamento do cliente | 1 de setembro de 2018 |
|Efetue a caução de período (cartões de crédito, 30 dias) | 1 de setembro de 2018 – 30 de setembro de 2018 |
|Início do período de coleta | 1 de setembro de 2018 |
|Término do período de coleta (máximo, 30 dias) | 30 de setembro de 2018 |
|Data de cálculo de pagamento (mensalmente no dia 15) | 1 de outubro de 2018 |
|Data de pagamento | 15 de outubro de 2018 |

Se o cliente comprar usando um Enterprise Agreement:

| DESCRIÇÃO |    Data  |
|----------|----------|
|Período de ordem | 15 de agosto de 2018 – 30 de agosto de 2018 |
|Prazo final (trimestre) | 30 de setembro de 2018 |
|Data de cobrança | 15 de outubro de 2018 |
|Efetue a caução de período (cartões de crédito, 30 dias) | n/d |
|Início do período de coleta | 15 de outubro de 2018 |
|Término do período de coleta (máximo, 90 dias) | 15 de janeiro de 2019 |
|Data de pagamento do cliente | 30 de dezembro de 2018 |
|Data de cálculo de pagamento (mensalmente no dia 15) | 15 de janeiro de 2019 |
|Data de pagamento | 15 de fevereiro de 2019 |

**Créditos gratuitos e compromisso monetário** 

Alguns clientes optam por pagar antecipadamente o Azure com um compromisso monetário no Enterprise Agreement ou receberam créditos gratuitos para uso com o Azure. Embora esses créditos possam ser usados para pagar pelo uso do Azure, eles não podem ser usados para pagar valores de licença de software do editor.

**Cobrança e coleções** 

O faturamento da licença do software do editor é apresentado usando o método de faturamento selecionado pelo cliente e segue o cronograma do faturamento. Clientes sem um Contrato Enterprise em vigor são cobrados mensalmente pelas licenças de software do mercado. Os clientes com um Contrato Enterprise são faturados mensalmente por meio de uma fatura apresentada trimestralmente.

Quando modelos de preços de assinatura ou de pagamento conforme o uso são selecionados, a Microsoft atua como agente do editor e é responsável por todos os aspectos de faturamento, pagamento e cobrança.

### <a name="publisher-payout-and-reporting"></a>Pagamento e relatórios do editor

* Quaisquer valores de licenciamento de software coletados pela Microsoft como um agente estão sujeitas a um valor de transação de 20%, a menos que seja especificado de outra forma e sejam deduzidas no momento do pagamento do editor.

* Os clientes normalmente compram usando o Enterprise Agreement ou um contrato de pagamento por utilização habilitado para cartão de crédito. O tipo de contrato determina o tempo de cobrança, faturamento, cobrança e pagamento.

>[!NOTE] 
>Todos os relatórios e insights para a opção de publicação de transação estão disponíveis por meio da seção de informações da seção do Portal do Cloud Partner ou análise do Partner Center.

#### <a name="billing-questions-and-support"></a>Perguntas sobre cobrança e suporte

Para obter mais informações e políticas legais, consulte o [Contrato do editor](https://cloudpartner.azure.com/Content/Unversioned/PublisherAgreement2.pdf) (disponível no Portal do Cloud Partner).

Para obter ajuda sobre questões sobre cobrança, entre em contato com [suporte do publicador marketplace comercial](https://aka.ms/marketplacepublishersupport).

## <a name="transact-requirements"></a>Transact requisitos

Os requisitos de transação para diferentes tipos de oferta são abordados nesta seção.

### <a name="requirements-for-all-offer-types"></a>Requisitos para todos os tipos de oferta

- Uma conta da Microsoft e as informações financeiras são necessárias para a opção, independentemente da oferta de publicação de transação do modelo de preços.
- Obrigatórias informações financeiras incluem o perfil de conta e imposto de pagamento.

Para obter mais informações sobre como configurar essas contas, consulte [gerenciar sua conta do Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/manage-account#financial-details).


### <a name="requirements-for-specific-offer-types"></a>Requisitos para tipos de oferta específica

A opção de publicação de transações está disponível apenas para uso com os seguintes tipos de oferta de mercado: 

**Máquina Virtual** 

Selecione modelos gratuitos, traga sua licença própria ou pague conforme o preço e apresente-os como SKUs definidos no nível da oferta. Na fatura do cliente do Azure, a Microsoft apresenta os valores de licença de software do editor separadamente dos valores de infraestrutura subjacentes do Azure. Os valores de infraestrutura do Azure são impulsionadas pelo uso do software do editor.

**Aplicativos do Azure: Modelo de solução ou aplicativo gerenciado** 

Deve provisionar uma ou mais máquinas virtuais e pulls por meio da soma dos preços de máquina virtual. Para aplicativos gerenciados em um único plano, uma assinatura mensal de taxa fixa pode ser selecionada como o modelo de preço, em vez do preço da máquina virtual. Em alguns casos, as taxas de uso de infraestrutura do Azure são passadas para o cliente separadamente de taxas de licença de software, mas na mesma instrução de cobrança. No entanto, se você configurar um aplicativo gerenciado oferta para encargos de infraestrutura de ISV, os recursos do Azure são cobrados com o publicador e o cliente recebe uma taxa fixa que inclui o custo de infraestrutura, licenças de software e serviços de gerenciamento.

## <a name="next-steps"></a>Próximas etapas

* Examine os requisitos de qualificação nas opções de publicação pela seção de tipo de oferta para finalizar a seleção e a configuração de sua oferta.
* Examine os padrões de publicação por vitrine eletrônica para obter exemplos de como sua solução é mapeada para uma configuração e tipo de oferta.

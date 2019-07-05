---
title: Criar uma nova oferta de SaaS no mercado comercial
description: Como criar um novo Software como uma oferta de serviço (SaaS) para listar ou vender no Azure Marketplace, AppSource, ou por meio do programa provedor de solução de nuvem (CSP) usando o portal do Marketplace comerciais na Microsoft Partner Center.
author: mattwojo
manager: evansma
ms.author: mattwoj
ms.service: marketplace
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: dc086bc1252c084b717807213b5ba4c7f9d7bb97
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514058"
---
# <a name="create-a-new-saas-offer"></a>Criar uma nova oferta de SaaS

Para começar a criação de Software como serviço (SaaS) oferece, certifique-se que você primeiro [criar uma conta no Partner Center](./create-account.md) e abra o [painel Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/offers), com o **visão geral** guia selecionada.

![Painel de Marketplace comercial no Partner Center](./media/new-offer-overview.png)

Selecione o + **criar um novo...** botão e, em seguida, selecione a **Software como serviço** item de menu. 

Se você selecionar um dos tipos de oferta, você será redirecionado para o mais antigo [Portal do Cloud Partner](https://cloudpartner.azure.com/).  Ofertas de SaaS somente estão disponíveis no portal do Marketplace comercial no Partner Center neste momento. 

![Criar janela de oferta no Partner Center](./media/new-offer-click.png)

O **nova oferta** caixa de diálogo é exibida. 

![Caixa de diálogo nova oferta](./media/new-offer-popup.png)


## <a name="offer-id-and-name"></a>ID da oferta e nome

- **ID da Oferta**: Crie um identificador exclusivo para cada oferta em sua conta. Essa ID será visível para os clientes no endereço da URL para a oferta do marketplace e modelos do Azure Resource Manager (se aplicável). ID da oferta deve ser em letras minúsculas, alfanuméricos (incluindo hifens e sublinhados, mas não há espaço em branco). Isso é limitado a 50 caracteres e não pode ser atualizado depois que você selecione Criar.  
Exemplo: teste oferta 1
<br>Resultando na URL: `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`

- **Nome da oferta**: O nome oficial da sua oferta de aplicativo SaaS, consistente em toda a publicações, anúncios e sites da web.  Esse nome pode ser com a marca registrado.  Oferecer o nome não deve conter espaço em branco, emojis (a menos que sejam o símbolo de direitos autorais ou de marca comercial) e deve ser limitado a 50 caracteres.
<br>Exemplo: Oferta de teste 1&#8482;

Selecione **Criar**.  Uma **visão geral da oferta** página é criada para esta oferta.  

<!---
![Offer overview on Partner Center](./media/commercial-marketplace-offer-overview.png)
-->

## <a name="offer-overview"></a>Visão geral da oferta

O **visão geral da oferta** página inclui: 

- O **status da publicação** exibe uma representação visual das etapas necessárias para publicar essa oferta e quanto tempo levará para completar a cada etapa. Ícones de etapa de publicação incompleta ficará esmaecidas. 

- O **visão geral da oferta** menu contém uma lista de links para executar operações nessa oferta. Esta lista de operações será alterada com base na seleção feita para sua oferta.  
    - Se a oferta é um rascunho – excluir rascunho 
    - Se a oferta estiver ativa – Stop vender a oferta 
    - Se a oferta está em visualização – Go-live 
    - Se você não tiver concluído o logon do publicador – Cancelar publicação

## <a name="offer-setup"></a>Instalação de oferta

O **oferecer instalação** guia solicita as informações a seguir. Selecione **salvar** após concluir esses campos.

- **Você gostaria de venda por meio da Microsoft?** (Sim/não)
    - **Sim**, você gostaria de vender a sua oferta por meio da Microsoft, com a Microsoft hospeda as transações do marketplace em seu nome; ou 
    - **Não**, você preferir simplesmente listar sua oferta por meio dos marketplaces, processamento de todas as transações monetárias independentemente do Microsoft.    

### <a name="sell-through-microsoft"></a>Vender por meio da Microsoft

Venda por meio da Microsoft fornece melhor descoberta de cliente e aquisição, permite que a Microsoft para transações de marketplace de host em seu nome e tira proveito dos recursos de comércio disponível globalmente da Microsoft.

#### <a name="saas-offer-requirements"></a>Requisitos de oferta de SaaS

Para listar o Software como serviço (SaaS) oferece com o Marketplace comercial no Partner Center, os seguintes critérios devem ser atendidos:

- Sua oferta deve usar [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) para gerenciamento de identidade e autenticação.
- Sua oferta deve usar [APIs de preenchimento SaaS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) para integrar com o Azure Marketplace.
- Para obter requisitos mais abrangentes, consulte a [guia de publicação da oferta SaaS](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

#### <a name="saas-on-azure-billing-infrastructure-costs"></a>SaaS nos custos de infraestrutura de cobrança do Azure
Se a oferta de SaaS é hospedada no Azure, você, como o Editor, deve considerar as taxas de uso de infraestrutura do Azure e taxas de licenciamento de software como um item único custo. Esse custo é representado como uma taxa mensal fixa para o cliente. Uso de infraestrutura do Azure é gerenciado e cobrado diretamente a você, o parceiro. Os valores reais de uso da infraestrutura não são vistos pelo cliente. Em geral, os editores optam por incluir os valores de uso da infraestrutura do Azure em seus preços de licença de software. 

Taxas de licenciamento são apresentadas como uma taxa de taxa fixa de assinatura mensal, recorrente baseada em site e não são medidas de software ou baseado em consumo.

|**A custo da licença**|**US $100 por mês**|
|:---|:---|
|Custo de uso do Azure (D1/1-Núcleo)|Cobrados diretamente para o publicador, não o cliente|
|Cliente será cobrado pela Microsoft|US $100,00 por mês (Publisher deve considerar os custos de infraestrutura incorridos ou passagem na taxa de licença)|

- Nesse cenário, a Microsoft cobra US $ 100,00 pela sua licença de software e paga US $ 80,00 ao editor.
- Parceiros que qualificaram para o **reduzido tarifa de serviço do Marketplace** verá uma taxa de transação reduzido o SaaS oferece desde maio de 2019 até de 2020 de junho. Nesse cenário, a Microsoft cobra $100,00 pelos sua licença de software e paga out US $90,00 ao publicador.

> [!NOTE]
> **Reduzido a tarifa de serviço do Marketplace**: Para determinados SaaS oferece o que você publicou em nosso Marketplace comerciais, Microsoft reduzirá sua taxa de serviço do Marketplace de 20% (conforme descrito no contrato de Editor do Microsoft) a 10%. Para sua oferta qualificar que, pelo menos uma das suas ofertas deverão ter sido designada pela Microsoft como estando pronto para venda conjunta de IP ou priorizada de venda conjunta do IP.  Qualificação deve ser atendida pelo menos cinco (5) dias úteis antes do final de cada mês do calendário para receber essa taxa de serviço do Marketplace reduzida para o mês.  A taxa de serviço do Marketplace reduzido não se aplica a VMs, aplicativos gerenciados ou quaisquer outros produtos disponibilizados por meio de nosso Marketplace comercial.  A taxa de serviço do Marketplace reduzido só estará disponível para ofertas qualificadas para encargos de licença, coletados pela Microsoft entre 1 de maio de 2019 e 30 de junho de 2020.  Após esse período, a taxa de serviço do Marketplace retornará ao seu valor normal. 

|**Faturas da Microsoft**|**US $100 por mês**|
|:---|:---|
|A Microsoft paga para você 80% do seu custo de licença <br>**Para aplicativos de SaaS qualificados, a Microsoft paga para 90% do seu custo de licença*|US $80,00 por mês <br>*$* 90,00 por mês *|


#### <a name="csp-program-opt-in"></a>CSP aceitação do programa
O [provedor de solução de nuvem (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) programa permite que as ofertas de software para alcançar milhões de clientes da Microsoft qualificados com pouca investimento em marketing e venda.

- **Canais: Ativar minha oferta disponível no programa CSP** (caixa de seleção)

Decidindo disponibilizar sua oferta no programa CSP permite que provedores de soluções de nuvem vender seu produto como parte de uma solução integrada aos clientes. 

### <a name="list-through-microsoft"></a>A lista por meio da Microsoft

Promova seu negócio com a Microsoft com a criação de uma listagem do marketplace. Selecionando para listar sua oferta somente e não transact por meio do Microsoft significa que a Microsoft não participa diretamente em transações de licença de software. Não há nenhuma taxa de transação associado e o publicador mantém 100% de qualquer software coletadas do cliente de taxas de licenciamento. No entanto, o publicador é responsável por dar suporte a todos os aspectos da transação de licença de software, incluindo mas não limitado a: ordem de preenchimento, monitoração, cobrança, faturamento, pagamento e coleção. 

- **Como você deseja que os clientes potenciais para interagir com essa listagem de oferta?**

##### <a name="get-it-now-free"></a>Obtenha agora (gratuito)
Liste sua oferta para clientes gratuitamente, fornecendo uma URL válida (começando com http ou https) em que eles podem acessar seu aplicativo.  Por exemplo: `https://contoso.com/saas-app`

##### <a name="free-trial"></a>Avaliação gratuita
Liste sua oferta aos clientes em uma base de avaliação gratuita, fornecendo uma URL válida (começando com http ou https) em que eles podem acessar seu aplicativo.  Por exemplo: `https://contoso.com/trial/saas-app`

##### <a name="contact-me"></a>Entrar em contato comigo
Colete informações de contato do cliente conectando-se seu sistema de gerenciamento de relacionamento do cliente (CRM). O cliente será solicitado permissão para compartilhar suas informações. Esses detalhes do cliente, junto com o nome da oferta, a ID e a origem do marketplace onde eles encontrados sua oferta, serão enviados para o sistema CRM que você configurou. Para obter mais informações sobre como configurar o CRM, consulte [gerenciamento de clientes potenciais do Connect](#connect-lead-management). 

## <a name="example-marketplace-offer-listing"></a>Listagem de oferta de marketplace de exemplo

![Listagem com anotações de oferta de marketplace de exemplo](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Habilitar um test drive

Uma unidade de teste é uma ótima maneira de demonstrar sua oferta para clientes potenciais, oferecendo a opção ' experimentar antes de comprar ', resultando em aumento de conversão e a geração de clientes potenciais altamente qualificados. [Saiba mais sobre unidades de teste.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

- **Habilitar um test drive** (caixa de seleção) 

Habilitando o teste de unidade, você precisará configurar um ambiente de demonstração para os clientes testar sua oferta por um período fixo de tempo. 

### <a name="type-of-test-drive"></a>Tipo de teste de unidade

- **[Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)** : Um modelo de implantação que contém todos os recursos do Azure que compõem sua solução. Os produtos que se encaixam nesse cenário usar somente os recursos do Azure.
- **[Dynamics 365 para o Central de negócios](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)** : A Microsoft hospeda e mantém o serviço de unidade de teste (incluindo o provisionamento e implantação) para um sistema de planejamento de recursos de enterprise Central de negócios (Finanças, operações, forneça a cadeia, CRM, etc.).  
- **[Dynamics 365 for Customer Engagement](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)** : A Microsoft hospeda e mantém o serviço de unidade de teste (incluindo o provisionamento e implantação) para um sistema de engajamento do cliente (vendas, serviço, serviço de projeto, o serviço de campo, etc.).  
- **[Dynamics 365 para operações](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)** : A Microsoft hospeda e mantém o serviço de unidade de teste (incluindo o provisionamento e implantação) para um recurso da empresa finanças e operações do sistema (Finanças, operações, fabricação, cadeia de suprimentos, etc.) de planejamento. 
- **[Aplicativo lógico](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)** : Um modelo de implantação que abrange todas as arquiteturas de solução complexa. Todos os produtos personalizados devem usar esse tipo de teste de unidade.
- **[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)** : Um link incorporado a um painel personalizado. Produtos que desejam demonstrar que um interactive visual do Power BI deve usar esse tipo de teste de unidade. Aqui, basta fazer upload da URL do Power BI Embedded.

#### <a name="additional-test-drive-resources"></a>Recursos de unidade de teste adicionais
- [Práticas recomendadas da técnica de unidade para teste](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Práticas recomendadas de Marketing do Test Drive](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Unidade de visão geral em uma página de teste](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Conectar-se o gerenciamento de leads

Conectar-se com clientes diretamente ao listar sua oferta dos marketplaces e vinculando seu sistema de gerenciamento de relacionamento do cliente (CRM) para que você possa receber informações de contato de cliente imediatamente depois que um cliente demonstra interesse ou implanta seu produto.

- **Escolha um destino de cliente potencial** (menu suspenso): Fornece detalhes de conexão para o sistema CRM, onde você gostaria de enviar a clientes potenciais. 

Partner Center suporta os seguintes sistemas CRM para o gerenciamento de clientes potenciais. Selecione o link para obter instruções de instalação.

- Email de contato Blob – fornecer do Azure, nome do contêiner e cadeia de conexão da conta de armazenamento. 
- [Tabela do Azure](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table) – forneça o email de contato e de cadeia de conexão da conta de armazenamento. 
- [Dynamics CRM Online](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) – forneça o email de contato, a URL e o modo de autenticação (Office 365 ou Azure Active Directory).
- [Ponto de extremidade HTTPS](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-https) – forneça o email de contato e URL de ponto de extremidade HTTPS. 
- [Marketo](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-marketo) – forneça o email de contato, ID de formulário, ID de conta do Munchkin e ID do servidor.
- [SalesForce](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-salesforce) -fornecer email de contato e ID da organização. 

#### <a name="additional-lead-management-resources"></a>Recursos de gerenciamento de cliente potencial adicionais
- [Perguntas frequentes do gerenciamento de clientes potenciais](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Erros comuns de configuração de cliente potencial](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Levar a uma página de visão geral de gerenciamento](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf)

Lembre-se **salvar** antes de passar para a próxima seção!

## <a name="properties"></a>propriedades
O **propriedades** guia pede para você definir as categorias e setores usados para agrupar sua oferta no Marketplace, os contratos legais que dão suporte a sua oferta e sua versão do aplicativo. 

Selecione **salvar** após concluir esses campos. 

### <a name="category"></a>Category
Selecione um mínimo de um (1) e um máximo de três (3) categorias usadas para agrupar sua oferta em áreas de pesquisa marketplace apropriado. Entre em contato-out como sua oferta oferece suporte a essas categorias na descrição da oferta. 

### <a name="industry"></a>Setor
Selecione até dois (2) usados para agrupar sua oferta em áreas de pesquisa marketplace apropriado de setores. Se sua oferta não é específica a um setor, não selecione nenhum. Entre em contato-out como a sua oferta oferece suporte os setores selecionados na descrição da oferta. 

### <a name="app-version"></a>Versão do aplicativo
Este é um campo opcional usado no AppSource marketplace para identificar o número de versão de sua oferta. 

### <a name="standard-contract"></a>Contrato Standard

- **Contrato padrão de uso?**

Para simplificar o processo de aquisição para clientes e reduzir a complexidade legal para fornecedores de software, a Microsoft oferece um modelo de contrato padrão para ajudar a facilitar a uma transação no marketplace. 

Em vez de criar termos e condições personalizados, do Azure Marketplace pode optar por oferecer seus softwares sob o contrato padrão, o que os clientes só precisam verificar e aceitar uma vez. 

O contrato padrão podem ser encontrado aqui: https://go.microsoft.com/fwlink/?linkid=2041178.

#### <a name="terms-of-use"></a>Termos de uso

Se os termos da licença forem diferentes do contrato padrão, você pode optar por inserir seus próprios termos legais de uso aqui. Você também pode inserir até 10.000 caracteres de texto neste campo. Se os termos de uso exigem uma descrição mais detalhada, insira um único link de URL neste campo onde os termos de licença adicionais podem ser encontrados. Ele será exibido para os clientes como um link ativo.

Os clientes precisam aceitar esses termos antes de poderem testar o aplicativo. 

Lembre-se **salvar** antes de passar para a próxima seção!

## <a name="offer-listing"></a>Listagem de oferta

A oferta de listagem guia exibe os idiomas (e mercados) onde sua oferta está disponível, no momento, o inglês (Estados Unidos) é o único local disponível. Além disso, esta página exibe o status da listagem de idioma específico e a data/hora em que ele foi adicionado. Você precisará definir os detalhes do marketplace (oferecer nome, descrição, termos de pesquisa, etc.) para cada idioma / mercado.

### <a name="offer-listings"></a>Listagens de oferta

Forneça os detalhes a serem exibidos no marketplace, incluindo descrições de sua oferta e ativos de marketing.

- **Nome** (obrigatório): O nome definido aqui será exibido como o título de sua listagem de oferta no marketplace(s) escolhida. O nome é preenchida previamente com base em seu anterior **nova oferta** entrada.  Isso pode ser com a marca registrado.  Isso não deve conter espaço em branco, emojis (a menos que sejam os símbolos de marca comercial e os direitos autorais) e deve ser limitado a 50 caracteres.
- **Resumo** (obrigatório): Forneça uma breve descrição de sua oferta a ser usado nos resultados de pesquisa de listagens do marketplace. Até 100 caracteres de texto podem ser inseridos neste campo.
- **Descrição** (obrigatório): Forneça uma descrição de sua oferta a ser exibido na visão geral de listagens do marketplace. Considere a inclusão de uma proposta de valor, os principais benefícios, todas as associações a categoria ou setor, oportunidades de compra no aplicativo, qualquer necessária divulgações e um link para saber mais.
Até 3.000 caracteres de texto podem ser inseridos neste campo. Para obter dicas adicionais, consulte o artigo [redigir uma descrição excelente aplicativo](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description).
- **Pesquisar palavras-chave**: Insira até três palavras-chave que os clientes podem usar para localizar sua oferta no marketplace(s).
- **Instruções de Introdução** (obrigatório): Explique como configurar e começar a usar seu aplicativo para clientes em potencial.  Neste início rápido pode conter links para documentação online mais detalhada. Até 3.000 caracteres de texto podem ser inseridos neste campo. 

#### <a name="links"></a>Links

- **Política de privacidade** (obrigatório): Vincular a política de privacidade da sua organização. Você é responsável por garantir que seu aplicativo está em conformidade com normas e leis de privacidade e para fornecer uma política de privacidade válido
- **Materiais de Marketing de programa do CSP** (opcional): Você deve fornecer um link para material de marketing, se você optar por estender sua oferta para o [provedor de solução de nuvem (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) programa. CSP estende sua oferta para uma variedade maior de clientes qualificados, permitindo que os parceiros CSP agrupar, mercado e revender sua oferta. Esses revendedores precisará ter acesso aos materiais de marketing de sua oferta. Para obter mais informações, consulte [Go-To-Market Services](https://partner.microsoft.com/reach-customers/gtm).
- **Links úteis** (opcional): Opcionais documentos online complementares sobre seu aplicativo ou serviços relacionados listados, fornecendo uma **Title** e **URL**. Adicionar links úteis adicionais clicando **+ adicionar uma URL**.

#### <a name="contact-information"></a>Informações de contato

- **Contatos**: Para cada contato de cliente, forneça um funcionário **nome** , **número de telefone**, e **Email** endereço.  (Esses *não* ser exibidos publicamente). Um **URL de suporte** também é necessária para o **contato de suporte** grupo.  (Essas informações *serão* ser exibidos publicamente).

**Contatar suporte** (obrigatório): Para perguntas de suporte geral.

**Contato de engenharia** (obrigatório): Para questões técnicas.

**Contato do Gerenciador de canal** (obrigatório): Para perguntas de revendedor relacionadas ao programa CSP.

#### <a name="files-and-images"></a>Arquivos e imagens

- **Documentos** (obrigatório): Adicione documentos de marketing relacionados para sua oferta, no formato PDF, fornecendo um mínimo de um (1) e máximo de três (3) documentos por oferta.
- **Imagens** (opcional): Existem vários lugares onde as imagens de logotipo da sua oferta podem aparecer em todo o marketplace(s), que exigem os seguintes tamanhos: pequeno: 48x48 pixels _(obrigatório),_ médio: 90 x 90 pixels, grande: 216x216 pixels _(obrigatório),_ grande: 255 x 115 pixels e Hero: 815 x 290 pixels. Todas as imagens devem estar no. Formato PNG.
- **Capturas de tela** (obrigatório): Adicione capturas de tela que demonstra sua oferta. Um máximo de cinco (5) capturas de tela pode ser adicionado e deve ser dimensionado com 1280 x 720 pixels. Todas as imagens devem estar no. Formato PNG.
- **Vídeos** (opcional): Adicione links para vídeos demonstrando sua oferta. É possível usar links para vídeos do YouTube e/ou do Vimeo, que são mostrados juntamente com sua oferta aos clientes. Você também precisará inserir uma imagem em miniatura do vídeo, dimensionada para 1280 x 720 pixels no formato PNG. Você pode exibir um máximo de quatro vídeos por oferta.

Lembre-se **salvar** antes de passar para a próxima seção!

#### <a name="additional-marketplace-listing-resources"></a>Listagem de recursos adicionais do marketplace

- [Práticas recomendadas para o marketplace listagens da oferta](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)


## <a name="preview"></a>Visualização

O **versão prévia** guia permite que você defina limitada **Preview público** para liberar sua oferta antes de publicar sua oferta ao vivo para o público mais amplo do marketplace.

> [!IMPORTANT]
> Você deve selecionar **entrar no ar** antes de sua oferta será publicada em tempo real para o público-alvo marketplace público depois de verificar sua oferta em visualização.

- **Defina um público de visualização: Adicione um único email de conta do AAD/MSA por linha, juntamente com uma descrição opcional.**

Adicionar endereços de email em até dez (10) manualmente ou vinte (20) se carregando um arquivo CSV, para conta existente de Microsoft (MSA) ou as contas do Azure Active Directory (AAD) para ajudar a validar a sua oferta antes da publicação em tempo real. Ao adicionar essas contas, você está definindo um público que poderão ser acesso de visualização para a sua oferta antes de publicá-lo a marketplace(s). Se sua oferta já estiver funcionando, você ainda pode definir um público de visualização para testar as alterações ou atualizações para sua oferta.

> [!NOTE]
> O público-alvo de visualização é diferente de um público particular. Um público de visualização é permitido o acesso à sua oferta _anteriores_ ao que está sendo transmitido ao vivo em um dos marketplaces. Você também pode optar por criar um plano e torná-lo disponível apenas para um público particular. No **plano de listagem** guia, você pode definir um público-alvo privado com o **trata de um plano privado** caixa de seleção. Em seguida, você pode definir um público-alvo privado até 20.000 clientes usando IDs de locatário do Azure.

## <a name="technical-configuration"></a>Configurações técnicas

O **configurações técnicas** guia define os detalhes técnicos (caminho da URL, webhook, ID de locatário e ID do aplicativo) usados para se conectar à sua oferta. Essa conexão nos permite provisionar sua oferta para o cliente final se eles optarem por adquiri-lo. Diagramas que descreve o uso dos campos coletados estão disponíveis na documentação para [preenchimento SaaS APIs](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2).

- **URL da página de aterrissagem** (obrigatório): Defina a URL do site que os clientes serão levado depois de adquirir sua oferta do marketplace. Essa URL será o ponto de extremidade que recebe um token quando um cliente é roteado para a página. Esse token pode ser trocado por detalhes usando o resolve o cumprimento de APIs de provisionamento. Esses detalhes e quaisquer outras que você coleta podem ser usadas como parte de uma página da web interativos de cliente criado em sua experiência para concluir o registro e ativar a compra.

- **Webhook de Conexão** (obrigatório): Para todos os eventos assíncronos que a Microsoft precisa para enviar para você em nome do cliente (exemplo: Assinatura de SaaS ficou inválida), exigimos que você forneça um webhook de conexão. Se você ainda não tiver um sistema de webhook em vigor, a configuração mais simples é ter um aplicativo de lógica de ponto de extremidade HTTP que atenderá quaisquer eventos que está sendo postados nele e, em seguida, tratá-las adequadamente (por exemplo, https:\//prod-1westus.logic.azure.com:443/work). Para saber mais, confira [Chamar, disparar ou aninhar fluxos de trabalho com pontos de extremidade HTTP em aplicativos lógicos](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint).

- **A ID de locatário do Azure AD** (obrigatório): Por dentro do portal do Azure, exigimos que você [criar um aplicativo do Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) para que possamos validar a conexão entre nossos dois serviços estiver atrás de uma comunicação autenticada. Para localizar o [ID do locatário](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-id), vá para o Azure Active Directory e selecione **propriedades**, em seguida, procure o **ID de diretório** número listado (por exemplo, 50c464d3-4930-494c-963c-1e951d15360e).

- **A ID de aplicativo do Azure AD** (obrigatório): Você também precisa de sua [ID do aplicativo](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key) e uma chave de autenticação. Para obter esses valores, vá para o Azure Active Directory e selecione **registros de aplicativo**, em seguida, procure o **ID do aplicativo** número listado (por exemplo, 50c464d3-4930-494c-963c-1e951d15360e). Para localizar a chave de autenticação, acesse **as configurações** e selecione **chaves**. Você precisará fornecer uma descrição e duração e, em seguida, irá ser fornecido um valor numérico.

 Observe que a ID do aplicativo do Azure está associada à sua ID do publicador, portanto certifique-se de que a mesma ID de aplicativo é usada em todas as suas ofertas.

## <a name="plan-overview"></a>Visão geral de plano

O **visão geral do plano** guia permite que você forneça uma variedade de opções do plano de dentro da mesma oferta. Esses planos (também conhecidos como SKUs) podem diferir em termos de versão, monetização ou camadas de serviço. Você deve configurar pelo menos um plano para vender a sua oferta no marketplace.

Depois de criado, você verá seus nomes de plano, IDs, modelos, disponibilidade (pública ou privada) de preços atual publicando o status e as ações disponíveis.

**Ações** disponíveis na **visão geral do plano** variam, dependendo do status atual do seu plano e pode incluir:

- Se o status do plano estiver **rascunho** – exclua o rascunho
- Se o status do plano estiver **Live** – Stop vender o plano ou do público-alvo privada de sincronização

**Criar novo plano** (mínimo de um plano para aqueles que selecionar para venda por meio da Microsoft)

- **ID do plano:** Crie uma ID de plano exclusivos para cada plano nesta oferta. Essa ID será visível aos clientes em que a URL e o Azure Resource Manager modelos de produto (se aplicável). Use apenas caracteres de letras minúsculas, alfanuméricos, traços ou sublinhados. Um máximo de 50 caracteres são permitidos para essa ID do plano. Observe que a ID não pode ser modificada depois que a escolha criar.
- **Nome do plano:** Os clientes verão esse nome ao decidir qual planeja selecione dentro de sua oferta. Crie um nome de oferta exclusiva para cada plano nesta oferta. O nome do plano é usado para diferenciar os planos de software que podem ser uma parte da mesma oferta (ex.: Nome da oferta: Windows Server; plans: Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Listagem de plano

O **planejar listagem** guia exibe os idiomas (e mercados) em que seu plano está disponível, no momento, o inglês (Estados Unidos) é o único local disponível. Além disso, esta página exibe o status da listagem de idioma específico e a data/hora em que ele foi adicionado. Você precisará definir os detalhes do marketplace (oferecer nome, descrição, termos de pesquisa, etc.) para cada idioma / mercado.

#### <a name="plan-listing-details"></a>Lista detalhes do plano

Selecionando uma das linguagens de plano serão exibidas as **plano de listagem** informações, incluindo **nome** e **descrição.**

- **Nome**: Previamente preenchido com base em sua visualização **novo plano** entrada e será exibido como o título do seu plano da oferta"Software" exibido no marketplace.
- **Descrição:** Essa descrição é uma oportunidade para explicar o que torna esse plano de software exclusivo e quaisquer diferenças de outros planos de software dentro de sua oferta. Pode conter até 500 caracteres.

Selecione **salvar** após concluir esses campos.

#### <a name="plan-pricing-and-availability"></a>Plano de preços e disponibilidade

O **preços e disponibilidade** guia permite que você configure os mercados que este plano estarão disponível no, o modelo de monetização desejado, preço e cobrança do termo. Além disso, você pode indicar se deve torná-lo visível para todos ou apenas para clientes específicos (um público particular).

#### <a name="markets"></a>Mercados

- **Editar mercados** (opcional)

Cada plano deve estar disponível no mercado de pelo menos um. Marque a caixa de seleção para qualquer local de mercado no qual você gostaria de disponibilizar esse plano. Uma caixa de pesquisa e um botão para selecionar os países de "Remessa de imposto sobre", em que a Microsoft remits vendas e imposto sobre o uso em seu nome, estão incluídos para ajudar. 


Se você já definiu os preços para seu plano nos Estados Unidos dólares (USD) e adicionar outro local de colocação no mercado, o preço para o mercado de novo será calculado de acordo com as taxas de câmbio atuais. Você sempre deve examinar o preço para cada mercado antes da publicação. Preços podem ser examinados usando o link "Exportar preços (xlsx)" depois de salvar suas alterações.

#### <a name="pricing"></a>Preços

- **Modelo de preços**: Taxa fixa ou estação com base em

**Taxa fixa:** Habilite o acesso a sua oferta com um preço de taxa fixa de único preço mensal ou anual. Isso às vezes é chamado como preço com base no site.

**Estações com base em:** Habilitar o acesso à sua oferta com o preço com base no número de usuários acessando a oferta ou ocupando *estações*. Esse modelo baseado em estação permite que você defina o mínimo e o número máximo de estações permitidas com base no preço. Dessa forma, diferentes preços podem ser configurados com base no número de usuários por meio da configuração de vários planos.  Esses campos são opcionais. Se deixado em branco, o número de estações será interpretado como não tendo um limite (mínimo de 1) e máximo de tantos quanto o sistema pode dar suporte. Esses campos podem ser editados como parte de uma atualização para o seu plano.

Depois de publicado, a opção de modelo de preço cobrança não pode ser alterada. Além disso, todos os planos da oferta mesmo devem compartilhar o mesmo modelo de preços.

- **Termo de cobrança**: Mensal ou anual

Selecione a frequência com que os clientes devem pagar o preço listado. Preço mensal ou anual a pelo menos um deve ser fornecido, ou ambas as opções podem ser disponibilizadas para os clientes.

- **Preço**: USD por mês ou USD por ano

Conjunto de preços na moeda local (USD = dólar americano) são convertidos em moeda local selecionados todos os mercados usando as taxas de câmbio atuais disponíveis durante a instalação. Valide esses preços antes de publicar por meio da exportação de planilha de preços e examinar o preço em cada mercado. Se você quiser definir preços personalizados em um mercado individual, modificar e importar a planilha de preços. São responsáveis por validar esse preço e essas configurações.
**Primeiro, você deve salvar as alterações de preços para habilitar a exportação de dados de preço.*

Analise seus preços cuidadosamente antes de publicar, pois há algumas restrições sobre o que pode alterar depois que um plano é publicado:

- Depois que um plano é publicado, o modelo de preços não pode ser alterado.
- Depois que um termo de cobrança é publicado para um plano, ele não pode ser removido posteriormente.
- Depois que um preço para um mercado em seu plano for publicado, ele não pode ser alterado posteriormente.

### <a name="plan-audience"></a>Público-alvo do plano

Você tem a opção de configurar cada plano seja visível para todos ou apenas um público específico de sua escolha. Você pode atribuir a associação nesse público-alvo restrito usando IDs de locatário do AD do Azure.

#### <a name="privacy"></a>Privacidade

- **Isso é um plano privado** (caixa de seleção opcional)

Marque esta caixa para tornar o seu plano privado e visíveis somente para o público-alvo restrito de sua escolha. Depois de publicado como um plano privado, você pode atualizar o público-alvo ou optar por tornar o plano disponível para todos. Depois que um plano é publicado como visível para todos, ele deve permanecer visível para todos. (O plano não pode ser configurado como um plano privado novamente).

- **Público restrito (IDs de locatário)**

Atribua o público-alvo que terão acesso a este plano privado. O acesso é atribuído usando IDs de locatário com a opção para incluir uma descrição de cada ID de locatário atribuído. Um máximo de IDs de locatário 10 podem ser adicionados ou IDs de locatário aos 20.000 clientes se estiver importando um arquivo de planilha. csv.

Um locatário é uma representação de uma organização, com uma ID, representada como um GUID (identificador global exclusivo, um número inteiro de 128 bits usado para identificar recursos). É uma instância dedicada do Azure AD que uma organização ou um desenvolvedor de aplicativos recebe ao criar uma relação com Microsoft, como inscrever-se no Azure, no Microsoft Intune ou no Microsoft 365. Cada locatário do AD do Azure é distinto e separado de outros diretórios do AD do Azure. Para verificar se o locatário, entrar no portal do Azure com a conta que você deseja usar para gerenciar seu aplicativo. Se houver um locatário, você será conectado a ele automaticamente e verá o nome do locatário diretamente abaixo do nome da conta. Passe o mouse sobre o nome da conta no canto superior direito do portal do Azure para ver seu nome, email, diretório e ID do locatário (um GUID) e seu domínio. Se sua conta estiver associada a vários locatários, você pode selecionar o nome da sua conta para abrir um menu no qual você pode alternar entre locatários. Cada locatário tem sua própria ID exclusiva. Você também pode procurar ID do locatário da sua organização usando uma URL de nome de domínio em: [ https://www.whatismytenantid.com ](https://www.whatismytenantid.com).

Embora as ofertas de SaaS usam IDs de locatário para definir um público particular, outros tipos de oferta podem usar IDs de assinatura do Azure (que também são representados como GUIDs).

> [!NOTE]
> O público-alvo privado (ou público restrito) difere de um público de visualização. No **[visualização](#preview)** guia, você pode definir um público de visualização. Um público de visualização é permitido o acesso à sua oferta *anteriores* à oferta que está sendo transmitida ao vivo no marketplace. Enquanto a designação de particular de público-alvo se aplica somente a um plano específico, o público-alvo de visualização pode exibir todos os planos (privada ou não), mas somente durante o período de visualização limitada, enquanto o plano é testado e validado.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Lista de exemplos de planos dentro de uma oferta do marketplace

![Planejamento de exemplo do marketplace listando com anotações](./media/marketplace-plan.svg)

## <a name="test-drive"></a>Test drive

O **Test drive** guia permite que você configure uma demonstração (ou "unidade de teste") que permitirá que os clientes testar sua oferta antes de confirmar para adquiri-lo. Saiba mais no artigo [o que é Test Drive?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive). Se você não deseja mais fornecem um test drive para sua oferta, retornar para o **[oferecem instalação](#offer-setup)** página e desmarque **Ativar unidade de teste**.

### <a name="technical-configuration"></a>Configurações técnicas
Os seguintes tipos de unidades de teste estão disponíveis, cada um com seus próprios requisitos técnicos de configuração.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Aplicativo lógico](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (configuração técnica não obrigada)

#### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Configurações técnicas do Azure Resource Manager o test drive

Um modelo de implantação que contém todos os recursos do Azure que compõem sua solução. Os produtos que se encaixam nesse cenário usar somente os recursos do Azure. Saiba mais sobre como configurar uma [test-drive do Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Regiões** (obrigatório): Atualmente, há 26 regiões com suporte do Azure onde seu test drive pode ser disponibilizado. Normalmente, você vai querer disponibilizar seu test drive nas regiões onde você antecipar o maior número de clientes, para que eles podem selecionar a região mais próxima para o melhor desempenho. Você precisará certificar-se de que sua assinatura tem permissão para implantar todos os recursos necessários em cada uma das regiões em que você está selecionando.

- **Instâncias**: Selecione o tipo (quente ou frio) e do número de instâncias disponíveis, que serão multiplicadas pelo número de regiões em que sua oferta está disponível.

**Hot**: Esse tipo de instância é implantado e está aguardando acesso por região selecionada. Os clientes podem acessar instantaneamente *quente* instâncias de um test drive, em vez de ter que esperar para uma implantação. A desvantagem é que tais instâncias estão sempre em execução na sua assinatura do Azure; portanto, incorrerão em um maior custo de tempo de atividade. É altamente recomendável ter pelo menos um *quente* da instância, como a maioria dos clientes não querem esperar para implantações completas, resultando em um horário no uso do cliente, se nenhum *quente* instância está disponível.

**Frio**: Esse tipo de instância representa o número total de instâncias que podem ser implantados, possivelmente, por região. Instâncias frias exigem todo o modelo do Gerenciador de recursos de unidade de teste para implantar quando um cliente solicita o test drive, portanto, *frio* instâncias são muito mais lentas carregar que *quente* instâncias. A desvantagem é que você só precisa pagar para a duração do teste de unidade, mas *não* sempre em execução em sua assinatura do Azure, como com um *quente* instância.

- **Modelo do Azure Resource Manager de teste de unidade**: Carregue o arquivo. zip que contém o modelo do Azure Resource Manager.  Saiba mais sobre como criar um modelo do Azure Resource Manager no artigo de início rápido [criar e implantar modelos do Azure Resource Manager usando o portal do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

- **Duração do test drive** (obrigatório): Insira o período de tempo que o Test Drive permanecerá ativo, no número de horas. O Test Drive é encerrado automaticamente após o término desse período de tempo. Essa duração poderá apenas apostar conjunto por um número inteiro de horas (por exemplo horas de "2", "1.5" não é válido).

#### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Configuração de técnica para Dynamics 365 um test drive

A Microsoft pode remover a complexidade da configuração de um test drive de hospedagem e mantendo o serviço de provisionamento e a implantação usando esse tipo de teste de unidade. A configuração para esse tipo de unidade de teste hospedado é o mesmo, independentemente se o test drive é como alvo um público-alvo Business Central, o envolvimento do cliente ou operações.

- **Máximo de unidades de teste simultâneos** (obrigatório): Defina o número máximo de clientes que pode usar seu test drive de uma só vez. Cada usuário simultâneo consumirá uma licença do Dynamics 365, enquanto o test drive está ativo, portanto, você precisará garantir que você possui licenças suficientes disponíveis para dar suporte o limite máximo definido. O valor recomendado é de 3 a 5.

- **Duração do test drive** (obrigatório): Insira o período de tempo que o Test Drive permanecerá ativo, definindo o número de horas. Após este número de horas, a sessão terminar e não consumirá uma das suas licenças. Recomendamos um valor de 2 a 24 horas, dependendo da complexidade de sua oferta. Essa duração poderá apenas apostar conjunto por um número inteiro de horas (por exemplo horas de "2", "1.5" não é válido).  O usuário pode solicitar uma nova sessão se são executadas fora do tempo e desejam acessar a unidade de teste novamente.

- **URL da instância** (obrigatório): A URL onde o cliente começará o test drive. Normalmente, a URL da sua instância do Dynamics 365 que executam seu aplicativo com dados de exemplo instalados (por exemplo, https://testdrive.crm.dynamics.com).

- **URL da API Web da instância** (obrigatório): Recuperar a URL da API da Web para sua instância do Dynamics 365 logon em sua conta do Microsoft 365 e navegando até **as configurações** \&gt; **Personalização** \&gt; **Recursos para desenvolvedores** \&gt; **API de Web (URL raiz do serviço) da instância**, copie a URL encontrada aqui (por exemplo, https://testdrive.crm.dynamics.com/api/data/v9.0).

- **Nome da função** (obrigatório): Forneça o nome de função de segurança que você definiu na sua unidade de teste personalizada do Dynamics 365. Isso será atribuído ao usuário durante o test drive (por exemplo, teste-drive-função).

#### <a name="technical-configuration-for-logic-app-test-drive"></a>Configurações técnicas para a unidade de teste de aplicativo lógico

Todos os produtos personalizados devem usar esse tipo de modelo de implantação do teste de unidade que abrange uma variedade de arquiteturas de solução complexa. Para obter mais informações sobre como configurar o aplicativo lógico de unidades de teste, visite [operações](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) e [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) no GitHub.

- **Região** (obrigatório, a única seleção suspensa lista): Atualmente, há 26 regiões com suporte do Azure onde seu test drive pode ser disponibilizado. Os recursos para seu aplicativo lógico serão implantados na região selecionada. Se seu aplicativo lógico tem todos os recursos personalizados armazenados em uma região específica, verifique se que essa região está selecionada aqui. A melhor maneira de fazer isso é totalmente implantar seu aplicativo lógico localmente em sua assinatura do Azure no portal e verificar se ele funciona corretamente antes de fazer essa seleção.

- **Máximo de unidades de teste simultâneos** (obrigatório): Defina o número máximo de clientes que pode usar seu test drive de uma só vez. Essas unidades já foram implantadas, permitindo que os clientes instantaneamente acessá-los sem esperar por uma implantação de teste.

- **Duração do test drive** (obrigatório): Insira o período de tempo que o Test Drive permanecerá ativo, no número de horas. O test drive é encerrado automaticamente após o término desse período de tempo.

- **Nome do grupo de recursos do Azure** (obrigatório): Insira o [grupo de recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) nome onde seu test drive do aplicativo lógico é salvo.

- **Nome do aplicativo lógico do Azure** (obrigatório): Insira o nome do aplicativo lógico que atribui a unidade de teste para o usuário. Esse aplicativo lógico deve ser salvo no grupo de recursos do Azure acima.

- **Nome do aplicativo lógico desprovisionamento** (obrigatório): Insira o nome do aplicativo lógico que desprovisiona o test drive quando o cliente for concluído. Esse aplicativo lógico deve ser salvo no grupo de recursos do Azure acima.

#### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Configuração técnica não é necessária para unidades de teste do Power BI

Produtos que desejam demonstrar que um interactive visual do Power BI pode usar um link incorporado para compartilhar um painel personalizado como o test drive, nenhuma outra técnica configuração necessária. Saiba mais sobre como configurar[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview) aplicativos de modelo.

### <a name="deployment-subscription-details"></a>Detalhes da assinatura de implantação

Para implantar o Test Drive em seu nome, criar e fornecer uma assinatura do Azure separados, únicos. (Não é necessária para unidades de teste do Power BI).

- **ID da assinatura do Azure** (necessário para o Azure Resource Manager e aplicativos lógicos): Insira a ID de assinatura para conceder acesso a seus serviços de conta do Azure para cobrança e emissão de relatórios de uso de recursos. Recomendamos que você considere [criando uma assinatura do Azure separada](https://docs.microsoft.com/azure/billing/billing-create-subscription) a ser usado para as unidades de teste, se você ainda não tiver um. Você pode localizar sua ID de assinatura do Azure fazendo logon na [portal do Azure](https://portal.azure.com/) e navegando até a **assinaturas** guia do menu do lado esquerdo. Selecionando a guia exibirá a ID da assinatura (por exemplo, "a83645ac-1234-5ab6-6789-1h234g764ghty").

- **A ID de locatário do Azure AD** (obrigatório): Insira seu Azure Active Directory (AD) [ID do locatário](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-id). Para localizar essa ID, entrar na [portal do Azure](https://portal.azure.com/), selecione a guia Active Directory no menu à esquerda, selecione **propriedades** , em seguida, procure o **ID de diretório** número listado (por exemplo, 50c464d3-4930-494c-963c-1e951d15360e). Você também pode procurar ID do locatário da sua organização usando sua URL de nome de domínio em: [ https://www.whatismytenantid.com ](https://www.whatismytenantid.com).

- **Nome do locatário do Azure AD** (necessário para o Dynamics 365): Insira o nome do Azure Active Directory (AD). Para localizar esse nome, entrar para o [portal do Azure](https://portal.azure.com/), no canto superior direito, o nome do locatário será listado sob o nome da conta.

- **A ID de aplicativo do Azure AD** (obrigatório): Insira seu Azure Active Directory (AD) [ID do aplicativo](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key). Para localizar essa ID, entrar na [portal do Azure](https://portal.azure.com/), selecione a guia Active Directory no menu à esquerda, selecione **registros de aplicativo**, em seguida, procure o **ID do aplicativo** número listado (por exemplo, 50c464d3-4930-494c-963c-1e951d15360e).

- **Segredo do cliente de aplicativo do Azure AD** (obrigatório): Insira o seu aplicativo do Azure AD [segredo do cliente](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key). Para localizar esse valor, entrar para o [portal do Azure](https://portal.azure.com/). Selecione o **Azure Active Directory** guia no menu à esquerda, selecione **registros de aplicativo**, em seguida, selecione seu aplicativo de teste de unidade. Em seguida, selecione **certificados e segredos**, selecione **novo segredo do cliente**, insira uma descrição, selecione **nunca** sob **Expires**, em seguida, escolher **adicionar**. Certifique-se de copiar o valor. (Não navegue para fora da página antes de você fazer isso, caso contrário, você não terá acesso ao valor.)

Lembre-se **salvar** antes de passar para a próxima seção!

### <a name="test-drive-listings-optional"></a>Listagens de unidade de teste (opcional)

O **listagens de Test Drive** opção encontrado sob o **Test drive** guia exibe os idiomas (e mercados) onde a unidade de teste está disponível, no momento, o inglês (Estados Unidos) é o único local disponível . Além disso, esta página exibe o status da listagem de idioma específico e a data/hora em que ele foi adicionado. Você precisará definir os detalhes de unidade de teste (Descrição, manual do usuário, vídeos, etc.) para cada idioma/mercado.

- **Descrição** (obrigatório): Descreva seu test drive, o que será demonstrado, os objetivos para o usuário para fazer experiências com recursos para explorar e qualquer informação relevante para ajudar o usuário a determinar se é necessário adquirir sua oferta. Até 3.000 caracteres de texto podem ser inseridos neste campo. 

- **Acessar informações** (necessário para unidades de teste do Azure Resource Manager e lógica): Explique o que um cliente precisa saber para acessar e usar essa unidade de teste. Percorra um cenário de uso de sua oferta e o que o cliente deve saber para acessar recursos em toda a unidade de teste. Até 10.000 caracteres de texto podem ser inseridos neste campo.

- **Manual do usuário** (obrigatório): Uma detalhado passo a passo de sua experiência de teste de unidade. O Manual do usuário deve abranger exatamente o que você deseja o cliente para obter experimentem o test drive e servir como uma referência para as perguntas que eles podem ter. O arquivo deve estar no formato PDF e ser nomeado (máximo de 255 caracteres) depois de carregar.

- **Vídeos: Adicionar vídeos** (opcional): Vídeos podem ser carregados para o YouTube ou Vimeo e referenciados aqui com uma imagem do link e miniatura (533, 324 pixels), para que um cliente pode exibir um passo a passo de informações para ajudá-los a entender melhor o test drive, incluindo como usar com êxito os recursos do seu oferecem e compreender os cenários que destacam suas vantagens.
  - **Nome** (obrigatório)
  - **URL (YouTube ou Vimeo somente)** (obrigatório)
  - **Miniatura (533 x 324px)** : Arquivo de imagem deve estar no formato PNG.

Selecione **salvar** após concluir esses campos.

## <a name="publish"></a>Publicar

#### <a name="submit-offer-to-preview"></a>Enviar para visualização da oferta

Depois de concluir todas as seções necessárias da oferta, selecione **publicar** no canto superior direito do portal. Você será redirecionado para o **revisão e publicar** página. 

Se essa for a primeira vez em que publicar essa oferta, você pode:

- Ver o status de conclusão para cada seção da oferta.
    - *Não foi iniciado* – significa que a seção não tocou e precisa ser concluída.
    - *Incompleto* – significa que a seção tem erros que precisam ser corrigidas ou requer mais informações a ser fornecido. Por favor, volte para a seção (ões) e atualizá-lo.
    - *Completa* – significa que a seção for concluída, todos os dados necessários foram fornecidos e não existem erros. Todas as seções da oferta devem ser em um estado completo antes de enviar a oferta.
- Fornece instruções de testes para a equipe de certificação para garantir que seu aplicativo seja testado corretamente, além de quaisquer notas suplementares útil para entender seu aplicativo.
- Enviar a oferta para publicação, selecionando **enviar**. Nós lhe enviaremos um email para que você saiba quando uma versão de visualização da oferta está disponível para que você possa revisar e aprovar. Você deve retornar ao Partner Center e selecione **Go-live** para a oferta para publicar sua oferta para o público (ou se uma privada oferece, para o público-alvo privado).

## <a name="next-steps"></a>Próximas etapas

- [Atualizar uma oferta existente no mercado comercial](./update-existing-offer.md)

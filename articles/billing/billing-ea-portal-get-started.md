---
title: Introdução ao Portal do EA do Azure
description: Este artigo explica como os clientes de EA do Azure usam o Portal do EA do Azure.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/09/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 1882b283f376a1bb8706132263c83e1a24ec0705
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70900923"
---
# <a name="get-started-with-the-azure-ea-portal"></a>Introdução ao Portal do EA do Azure

Este artigo ajuda os clientes diretos e indiretos do EA do Azure a começar a usar o [Portal do EA do Azure](https://ea.azure.com) com informações básicas sobre:

- Como o Portal do EA do Azure é estruturado.
- Funções usadas no Portal do EA do Azure.
- Como começar a criar assinaturas.
- Análise dos custos no Portal do EA do Azure e no portal do Azure.

Veja um vídeo que mostra uma sessão de integração completa do Portal do EA do Azure:

[Vídeo de integração do Portal do EA do Azure](https://www.youtube.com/watch?v=OiZ1GdBpo-I)

>[!VIDEO https://www.youtube.com/embed/OiZ1GdBpo-I]

## <a name="azure-ea-portal-hierarchy"></a>Hierarquia do Portal do EA do Azure

A hierarquia do Portal do EA do Azure consiste em:

**Portal do EA do Microsoft Azure** – o Portal do EA do Azure é um portal de gerenciamento online que ajuda você a gerenciar custos para seus serviços de EA do Azure. Você o usa para criar uma hierarquia de EA do Azure, incluindo departamentos, contas e assinaturas. Você também pode usá-lo para reconciliar os custos de seus serviços consumidos, baixar relatórios de uso e exibir listas de preços. E você cria chaves de API para seu registro.

**Departamentos** – você cria departamentos para ajudar a segmentar os custos em agrupamentos lógicos e, em seguida, definir um orçamento ou uma cota no nível do departamento.

**Contas** – são unidades organizacionais no Portal do EA do Azure e são usadas para gerenciar assinaturas. Contas também usadas para relatórios.

**Assinaturas** – são a menor unidade no Portal do EA do Azure. Elas são contêineres para os serviços do Azure gerenciados pelo administrador de serviços.

O diagrama a seguir ilustra as hierarquias simples do EA do Azure.

![Diagrama de hierarquias simples do EA do Azure](./media/billing-ea-portal-get-started/ea-hierarchies.png)

## <a name="enterprise-user-roles"></a>Funções de usuário corporativo

Para administrar os Serviços do Azure em seu registro, há quatro funções administrativas de usuário corporativo distintas:

- Administrador corporativo
- Administrador de departamento
- Proprietário da conta
- Administrador de serviço

As funções são usadas para concluir tarefas em dois portais do Microsoft Azure diferentes. O Portal do EA do Azure (https://ea.azure.com) é usado para ajudá-lo a gerenciar a cobrança e os custos. O portal do Azure (https://portal.azure.com) é usado para gerenciar os Serviços do Azure.

As funções de usuário são associadas a uma conta de usuário. Para validar a autenticidade do usuário, cada usuário deve ter uma conta Microsoft, corporativa ou de estudante válida. Verifique se cada conta está associada a um endereço de email monitorado ativamente. As notificações de conta são enviadas para o endereço de email.

Ao configurar usuários, você pode atribuir várias contas corporativas, de estudante ou da Microsoft à função de administrador corporativo. No entanto, você pode atribuir apenas uma conta Microsoft, corporativa ou de estudante à função de proprietário da conta. Além disso, uma única conta Microsoft, corporativa ou de estudante pode ter as funções de administrador corporativo e proprietário da conta aplicadas a ela.

### <a name="enterprise-administrator"></a>Administrador corporativo

A função de administrador corporativo tem o nível de acesso mais alto. Os usuários com essa função podem:

- Gerenciar contas e proprietários de conta
- Gerenciar outros administradores corporativos
- Gerenciar administradores de departamento
- Gerenciar contatos de notificação
- Exibir o uso em todas as contas
- Exibir encargos não cobrados em todas as contas

Você pode ter vários administradores corporativos em um Registro Enterprise. Você pode permitir acesso somente leitura a administradores corporativos. Todos eles herdam a função de administrador do departamento.

### <a name="department-administrator"></a>Administrador de departamento

Os usuários com essa função podem:

- Criar e gerenciar departamentos
- Criar novos proprietários de conta
- Exibir detalhes de uso dos departamentos que eles gerenciam
- Exibir custos, se as permissões necessárias forem concedidas

Você pode ter vários administradores de departamento para cada Registro Enterprise.

Você pode permitir acesso somente leitura aos administradores de departamento. Para permitir acesso somente leitura, edite ou crie um novo administrador de departamento e defina a opção somente leitura como **Sim**.

### <a name="account-owner"></a>Proprietário da conta

Os usuários com essa função podem:

- Criar e gerenciar assinaturas
- Gerenciar administradores de serviço
- Exibir o uso de assinaturas

Cada conta requer uma conta Microsoft, corporativa ou de estudante exclusiva. Para obter mais informações sobre as funções administrativas do Portal do EA do Azure, confira [Entendendo as funções administrativas do Contrato Enterprise no Azure](billing-understand-ea-roles.md).

### <a name="service-administrator"></a>Administrador de serviço

O administrador de serviços tem permissões para gerenciar serviços no portal do Azure e atribuir usuários à função de coadministrador.

## <a name="activate-your-enrollment"></a>Ative seu registro

Para ativar o serviço, o administrador corporativo inicial abre o Portal do EA do Azure em [https://ea.azure.com](https://ea.azure.com) e entra usando o endereço de email do email de convite.

Se você tiver mais de um registro, escolha um para ativar. Por padrão, somente os registros ativos são mostrados. Para exibir o histórico de registros, desmarque a opção **Ativo** no canto superior direito do Portal do EA do Azure.

Em Registro, o status mostra **Ativo**.

![Exemplo mostrando um registro ativo](./media/billing-ea-portal-get-started/ea-enrollment-status.png)

Somente administradores corporativos do Azure existentes podem criar outros admins corporativos.

### <a name="create-another-enterprise-admin"></a>Criar outro admin corporativo

- Entre no [Portal do EA do Azure](https://ea.azure.com) e navegue até **Gerenciar** > **Detalhes de Registro** e clique em **+ Adicionar Administrador** no canto superior direito da página.

Verifique se você tem os endereços de email do usuário e o método de autenticação preferencial, por exemplo, com conta Microsoft, corporativa ou de estudante. Você precisa das informações para adicionar um usuário.

Se você não for o administrador do EA, entre em contato com um administrador do EA para solicitar que ele o adicione a um registro. Depois que for adicionado a um registro, você receberá um email de ativação.

Se o administrador do EA não puder ajudá-lo, crie uma [solicitação de suporte do Portal do EA do Azure](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c). Forneça as seguintes informações:

- Número de registro
- Endereço de email para adicionar e tipo de autenticação (conta Microsoft, corporativa ou de estudante)
- Aprovação de email de um administrador de EA existente
  - Se o administrador de EA existente não estiver disponível, entre em contato com seu parceiro ou consultor de software para solicitar que ele altere os detalhes de contato por meio da ferramenta VLSC.

Para obter mais informações sobre as funções administrativas empresariais, confira [Entendendo as funções administrativas do Contrato Enterprise no Azure](billing-understand-ea-roles.md).

## <a name="create-an-azure-ea-department"></a>Criar um departamento de EA do Azure

Administradores corporativos e administradores de departamento usam departamentos para organizar e relatar os serviços e o uso do Azure Enterprise por departamento e centro de custo. O administrador corporativo é capaz de:

- Adicionar ou remover departamentos
- Associar uma conta a um departamento
- Criar administradores de departamento
- Permitir que administradores de departamento exibam preços e custos

Um administrador de departamento pode adicionar novas contas aos próprios departamentos. Pode também remover contas desses departamentos, mas não do registro.

Para adicionar um departamento:

1. Na área de navegação à esquerda, clique em **Gerenciar**.
2. Clique na guia **Departamento**, clique em **+ Adicionar Departamento** e, em seguida, insira as informações necessárias.
3. O nome do departamento é o único campo obrigatório. Ele precisa ter, pelo menos, três caracteres.
4. Ao concluir, clique em **Adicionar**.

## <a name="add-a-department-admin"></a>Adicionar um administrador de departamento

Depois que um departamento é criado, o administrador corporativo do Azure pode adicionar administradores de departamento e associar cada um deles a um departamento. O administrador do departamento é capaz de:

- Criar outros administradores de departamento
- Exibir e editar propriedades do departamento, tais como o nome ou o centro de custo
- Adicionar uma conta aos próprios departamentos
- Remover uma conta dos próprios departamentos
- Baixar detalhes de uso dos próprios departamentos
- Exibir o uso mensal e os encargos do próprio departamento se um administrador corporativo tiver concedido a ele permissão para isso <sup>1</sup>

### <a name="to-add-a-department-admin"></a>Para adicionar um administrador de departamento

Como um administrador corporativo:

1. Na área de navegação à esquerda, clique em **Gerenciar**.
2. Clique na guia **Departamento** e, em seguida, clique no departamento.
3. Clique em **+ Adicionar Administrador** e adicione as informações necessárias.
4. Para acesso somente leitura, defina a opção **Somente Leitura** como **Sim** e clique em **Adicionar**.

![Exemplo mostrando a caixa de diálogo Adicionar Administrador do Departamento](./media/billing-ea-portal-get-started/ea-create-add-department-admin.png)

### <a name="to-set-read-only-access"></a>Para definir o acesso somente leitura

Você pode permitir acesso somente leitura aos administradores de departamento. Ao criar um administrador do departamento:

- Defina a opção somente leitura como **Sim**.

Para editar um administrador do departamento existente:

1. Selecione um departamento e, em seguida, clique no símbolo de lápis ao lado do **Administrador do Departamento** que você deseja editar.
2. Defina a abertura somente leitura como **Sim**. Em seguida, clique em **Salvar**.

Os usuários com a função de administrador corporativo obtêm permissões de administrador do departamento automaticamente.

<sup>1</sup> Se você tiver recebido permissão para exibir o uso mensal e os encargos do departamento mas não puder vê-los, entre em contato com seu parceiro.

## <a name="add-an-account"></a>Adicionar uma conta

A estrutura da conta e da assinatura afeta o modo como elas são administradas e como elas aparecem em suas faturas e relatórios. Exemplos de organização típica incluem estruturar por divisões empresariais, equipes funcionais e locais geográficos.

Para adicionar uma conta:

1. No Portal do EA do Azure, clique em **Gerenciar** na área de navegação à esquerda.
2. Clique na guia **Conta** e depois, na página **Conta**, clique em **+Adicionar Conta**.
3. Selecione um departamento ou deixe-o como não atribuído e selecione o tipo de autenticação desejado.
4. Digite um nome amigável a ser usado para identificar a conta no relatório.
5. Digite o endereço de **Email do Proprietário da Conta** para associar à nova conta.
6. Confirme o endereço de email e clique em **Adicionar**.

![Exemplo mostrando a lista de contas e a opção + Adicionar Conta](./media/billing-ea-portal-get-started/create-ea-add-an-account.png)

Você pode adicionar outra conta clicando em **Adicionar Outra Conta** ou pode clicar em **Adicionar** no canto inferior direito da barra de ferramentas à esquerda.

Para confirmar a propriedade da conta:

1. Entre no Portal do EA do Azure.
2. Confirme a propriedade da conta exibindo o status. O status deve mudar de **Pendente** para **Data de Início/Término**. A Data de Início/Término é a data em que o usuário se conectou pela primeira vez e a data de término do contrato.


## <a name="change-account-owner"></a>Alterar proprietário da conta

Os administradores corporativos podem usar o Portal do EA do Azure para transferir a propriedade da conta de assinatura em um registro. A ação move todas as assinaturas de uma conta de usuário de origem para uma conta de usuário de destino.

Pontos importantes sobre a transferência de informações de conta de usuário:

- A transferência de uma conta corporativa ou de estudante para outra é uma ação compatível.
- A transferência de uma conta Microsoft para uma conta corporativa ou de estudante é uma ação compatível.
- A transferência de uma conta corporativa ou de estudante para uma conta Microsoft não é uma ação compatível.
- A transferência de uma conta Microsoft para outra é uma ação compatível. A conta de destino deve ser uma conta válida do Azure Commerce para ser um destino válido para transferências. Para novas contas, você será solicitado a criar uma conta do Azure Commerce ao entrar no Portal do EA do Azure. Para contas existentes, você deve primeiro criar uma nova assinatura do Azure antes que a conta seja considerada qualificada.
- Quando você conclui uma transferência de assinatura, a Microsoft atualiza o proprietário da conta.

Políticas de RBAC:

- Somente as transferências de assinatura do Azure entre duas IDs organizacionais no mesmo locatário preservam as políticas de RBAC (controle de acesso baseado em função) do Azure, atribuições de função de administrador de serviços e atribuições de função de coadministrador. Outras transferências de assinatura resultam na perda de suas políticas de RBAC e das atribuições de função de coadministrador de administrador de serviços. Políticas e funções de administrador não são transferidas entre diretórios diferentes. Os administradores de serviços são atualizados para o proprietário da conta de destino.
- Quando você executa transferências de assinatura entre duas IDs organizacionais no mesmo locatário, as políticas de RBAC e as funções de administrador de serviços e de coadministrador existentes são preservadas.

Antes de alterar um proprietário da conta:

1. Exiba a guia **Conta** e identifique a conta de origem. A conta de origem precisa estar ativa.
2. Identifique a conta de destino. Ela precisa estar ativa.

Para transferir a propriedade da conta para todas as assinaturas:

1. Na área de navegação à esquerda, clique em **Gerenciar**.
2. Clique na guia **Conta** e passe o mouse sobre uma conta.
3. Clique no símbolo de alterar proprietário da conta à direita. O símbolo se assemelha a uma pessoa.
4. Selecione uma conta qualificada e clique em **Avançar**.
5. Confirme a transferência e clique em **Enviar**.

![Imagem mostrando o símbolo Alterar Proprietário da Conta](./media/billing-ea-portal-get-started/create-ea-create-sub-transfer-account-ownership-of-sub.png)

Para transferir a propriedade da conta para uma assinatura única:

1. Na área de navegação à esquerda, clique em **Gerenciar**.
2. Clique na guia **Conta** e passe o mouse sobre uma conta.
3. Clique no símbolo transferir assinaturas à direita. O símbolo se assemelha a uma página.
4. Selecione uma assinatura qualificada e clique em **Avançar**.
5. Confirme a transferência e clique em **Enviar**.

![Imagem mostrando o símbolo Transferir Assinaturas](./media/billing-ea-portal-get-started/ea-transfer-subscriptions.png)

Veja um vídeo que mostra o gerenciamento de usuários do Portal do EA do Azure:

[Vídeo de gerenciamento de usuários do Portal do EA do Azure](https://www.youtube.com/watch?v=621jVkvmwm8)

>[!VIDEO https://www.youtube.com/embed/621jVkvmwm8]

## <a name="create-a-subscription"></a>Criar uma assinatura

Os proprietários da conta podem exibir e gerenciar assinaturas. Você pode usar assinaturas para dar acesso a ambientes e projetos de desenvolvimento às equipes de sua organização. Por exemplo, teste, produção, desenvolvimento e preparo. Ao criar assinaturas diferentes para cada ambiente de aplicativo, você ajuda a proteger cada ambiente. Você também pode atribuir uma conta de administrador de serviços diferente para cada assinatura. Você pode associar assinaturas a qualquer número de serviços. O proprietário da conta cria assinaturas e atribui uma conta de administrador de serviços a cada assinatura na conta dele.

### <a name="add-a-subscription"></a>Adicionar uma assinatura

Use as informações a seguir para adicionar uma assinatura.

Na primeira vez que você adicionar uma assinatura à conta, será solicitado que você aceite o contrato MOSA e um plano de taxas. Embora eles não sejam aplicáveis a clientes de Contrato Enterprise, eles são necessários para criar sua assinatura. O aditamento de registro do Contrato Enterprise do Microsoft Azure substitui os itens acima e a sua relação contratual não é alterada. Quando solicitado, selecione a caixa indicando que você aceita os termos.

Todas as novas assinaturas são criadas com o nome de assinatura padrão de _Microsoft Azure Enterprise_. Você pode atualizar o nome da assinatura para diferenciá-la das outras assinaturas no registro. Isso é feito também para garantir que ela seja reconhecida em relatórios no nível empresarial.

Para adicionar uma assinatura:

1. No Portal do EA do Azure, entre na conta.
2. Clique na guia **Admin** e, em seguida, clique em **Assinatura** na parte superior da página.
2. Verifique se você está conectado como o proprietário da conta em questão.
3. Clique em **+ Adicionar Assinatura** e, em seguida, clique em **Comprar**.
  Na primeira vez que você adicionar uma assinatura a uma conta, deverá fornecer suas informações de contato. Ao adicionar assinaturas adicionais, suas informações de contato serão adicionadas para você.
4. Clique em **Assinaturas**, selecione a assinatura que você criou e clique em **Editar Detalhes da Assinatura**.
5. Atualize o **Nome da Assinatura** e o **Administrador de Serviços** e selecione a marca de seleção.
  O nome da assinatura aparece em relatórios e é o nome do projeto associado à assinatura no portal de desenvolvimento.

Novas assinaturas podem levar até 24 horas para aparecer na lista de assinaturas. Depois de ter criado uma assinatura, você pode:

- [Editar detalhes da assinatura](https://account.azure.com/Subscriptions)
- [Gerenciar chaves de assinatura](https://portal.azure.com/#home)

## <a name="transfer-pay-as-you-go-subscription-to-ea-subscription"></a>Transferir uma assinatura paga conforme o uso para assinatura de EA

Para transferir uma assinatura individual com as tarifas pagas conforme o uso para uma assinatura de EA, você precisa criar uma solicitação de suporte no portal do Azure. Para criar uma solicitação de suporte, clique em **+ Nova solicitação de suporte** na área Ajuda e Suporte.


## <a name="view-usage-summary-and-download-reports"></a>Exibir resumo de uso e baixar relatórios

Os administradores corporativos podem exibir um resumo dos dados de uso, do compromisso monetário consumido e das cobranças associadas com o uso adicional no portal do Azure EA. As cobranças são apresentadas no nível do resumo em todas as contas e assinaturas.

Para exibir o uso detalhado de contas específicas

Baixe o relatório de detalhes de uso. Clique em **Relatórios** e, em seguida, clique na guia **Uso de Download**. Na lista de relatórios, clique em **Baixar** para o relatório mensal que deseja obter.

O relatório não inclui as taxas aplicáveis. Talvez haja uma latência de até oito horas do momento em que o uso foi incorrido até ser refletido no relatório.

Para exibir os relatórios e os grafos de Resumo de Uso:

1. No Portal do EA do Azure, na área de navegação à esquerda, clique em **Relatórios** e exiba a guia **Resumo de Uso**.  
  ![](./media/billing-ea-portal-get-started/create-ea-view usage-summary-and-download-reports.png)
2. Selecione um período de compromisso.
3. Alterne entre **M** (Mensal) e **P** (Personalizado) no canto superior direito da página para exibir o **Resumo de Uso** com datas de início e de término personalizadas.  
  ![](./media/billing-ea-portal-get-started/create-ea-view-usage-summary-and-download-reports-custom-view.png)
4. Selecione um período ou mês no grafo para exibir detalhes adicionais.
5. O grafo mostra o uso mês a mês com um detalhamento do uso utilizado, os encargos adicionais de serviço, os encargos cobrados separadamente e os encargos do Marketplace.
6. Para o mês selecionado, filtre por departamentos, contas e assinaturas abaixo do grafo.
7. Alterne entre **Cobrar por Serviços** e **Cobrar por Hierarquia**.
8. Expanda e recolha entre o **Serviço do Azure**, os **Encargos Cobrados Separadamente** e o **Azure Marketplace** para exibir detalhes.

Veja um vídeo que mostra como exibir o uso:

[Vídeo de uso do Portal do EA do Azure](https://www.youtube.com/watch?v=Cv2IZ9QCn9E)

>[!VIDEO https://www.youtube.com/embed/Cv2IZ9QCn9E]

### <a name="download-csv-reports"></a>Baixar relatórios CSV

Os administradores corporativos usam a página de Download de Relatório Mensal para baixar vários relatórios como arquivos CSV. Elas incluem:

- Saldo e encargos
- Detalhes de uso
- Cobranças do Marketplace
- Tabela de preços

Para baixar relatórios:


1. No Portal do EA do Azure, clique em **Relatórios**.
2. Clique em **Baixar Uso** na parte superior da página.
3. Selecione **Baixar** ao lado do relatório do mês.

Pode haver uma latência de até cinco dias entre a data em que o uso ocorreu e o momento em que ele é mostrado nos relatórios.

Os usuários que baixarem arquivos CSV com o Safari para Excel poderão encontrar erros de formatação. Para evitar erros, abra o arquivo usando um editor de texto.

![Exemplo mostrando a página Baixar Uso](./media/billing-ea-portal-get-started/create-ea-download-csv-reports.png)

Aqui está um vídeo que mostra como baixar informações de uso:

[Vídeo de uso do Portal do EA do Azure](https://www.youtube.com/watch?v=eY797htT1qg)

>[!VIDEO https://www.youtube.com/embed/eY797htT1qg]

## <a name="schedule-an-onboarding-call"></a>Agendar uma chamada de integração

Se você quiser agendar uma sessão presencial de integração de cliente, crie uma solicitação de suporte no [Suporte ao Portal do EA do Azure](https://support.microsoft.com/supportrequestform/e114582c-4e51-af46-10b1-1f0cc141e133). Selecione **Integração** como a **Categoria do Problema**.

## <a name="next-steps"></a>Próximas etapas
- Os administradores do Portal do EA do Azure devem ler [Administração do Portal do EA do Azure](billing-ea-portal-administration.md) para aprender sobre tarefas administrativas comuns.
- Se você precisar de ajuda para solucionar problemas do Portal do EA do Azure, confira [Solucionar problemas de acesso ao Portal do EA do Azure](billing-ea-portal-troubleshoot.md).

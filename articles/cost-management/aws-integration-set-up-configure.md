---
title: Instalar e configurar a integração de relatório de uso e custo do AWS no gerenciamento de custos do Azure
description: Este artigo orienta você durante a instalação e configuração de integração de relatório de uso e custo do AWS no gerenciamento de custos do Azure.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: 951178a82e0975f5f2af71bd48cf0f931246ae37
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002117"
---
# <a name="set-up-and-configure-aws-cost-and-usage-report-integration"></a>Instalar e configurar a integração de relatório de uso e custo do AWS

Com o Amazon Web Services (AWS) integração de relatório (CUR) de uso e custo, você pode monitorar e controlar seus gastos AWS no gerenciamento de custos do Azure. A integração permite que um único local no portal do Azure onde você pode monitorar e controle de gastos para o Azure e AWS. Este artigo explica como configurar a integração e configurá-lo para que você usar os recursos de gerenciamento de custos do Azure para analisar os custos e revisar os orçamentos.

O relatório de uso e custo do AWS armazenado em um bucket S3 usando suas credenciais de acesso do AWS para obter definições de relatório e baixar arquivos GZIP CSV do relatório de custo processos de gerenciamento.

## <a name="create-a-cost-and-usage-report-in-aws"></a>Criar um relatório de uso e custo no AWS

Usar um relatório de uso e custo é a maneira recomendada de AWS para coletar e processar os custos do AWS. Para obter mais informações, consulte o [relatório de uso e custo do AWS](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) documentação.

Use o **relatórios de uso e custo** página do console do gerenciamento de custo e cobrança no AWS para criar um relatório de uso e custo com as seguintes etapas:

1. Entre no console de gerenciamento do AWS e abra o [console de gerenciamento de custo e cobrança](https://console.aws.amazon.com/billing).
2. No painel de navegação, selecione **relatórios de uso e custo**.
3. Selecione **criar relatório**.
4. Para **nome do relatório**, insira um nome para seu relatório.
5. Para **detalhes adicionais de relatório** , para incluir as IDs de cada recurso no relatório e selecione **incluem IDs de recurso**.
6. Para **configurações de atualização de dados**, selecione se deseja que o relatório de uso e custo do AWS para atualizar se AWS aplica reembolsos, créditos, ou oferecer suporte a taxas de sua conta após finalizar sua fatura. Quando um relatório é atualizado, um novo relatório é carregado para o Amazon S3. É recomendável deixar essa configuração em.
7. Selecione **Avançar**.
8. Para **bucket S3**, escolha **configurar**.
9. Na caixa de diálogo Configurar o Bucket de S3, siga um destes procedimentos:
    1. Selecione um bucket existente na lista suspensa e escolha **próxima**.
    2. Digite um nome de bucket e a região onde você deseja criar uma nova partição de memória e escolha **próxima**.
10. Selecione, depois de confirmar que essa política é corrigir e escolha Salvar.
11. (Opcional) Para o prefixo de caminho do relatório, insira o prefixo de caminho de relatório que você deseja antecedendo para o nome do seu relatório.
Se você não especificar um prefixo, o prefixo padrão é o nome que você especificou para o relatório na etapa 4 e o intervalo de datas para o relatório, no seguinte formato: `/report-name/date-range/`
12. Para **unidade de tempo**, escolha **por hora**.
13. Para **controle de versão do relatório**, escolha se deseja que cada versão do relatório para substituir a versão anterior do relatório ou para ser entregue além das versões anteriores.
14. Para **habilitar a integração de dados para**, nenhuma seleção é necessária.
15. Para **compactação**, selecione **GZIP**.
16. Selecione **Avançar**.
17. Depois de examinar as configurações para o seu relatório, selecione **revisão e concluir**.

    Observe o nome do relatório. Você o usará em etapas posteriores.

Pode levar até 24 horas para AWS para começar a entrega de relatórios para o bucket do Amazon S3. Após a entrega é iniciado, o AWS atualiza os arquivos de relatório de uso e custo do AWS pelo menos uma vez por dia. Você pode continuar a configurar o ambiente do AWS sem aguardar a entrega iniciar.

## <a name="create-a-role-and-policy-in-aws"></a>Criar uma função e a política no AWS

O gerenciamento de custos do Azure acessa o bucket S3 em que o relatório de uso e custo está localizado várias vezes ao dia. O serviço precisa acessar as credenciais para verificar se há novos dados. Criar uma função e a política no AWS para permitir o gerenciamento de custo para acessá-lo.

Para habilitar o acesso baseado em função para uma conta do AWS no gerenciamento de custos, a função é criada no console do AWS. Você precisa ter o _arn da função_ e _ID externa_ do console do AWS. Posteriormente, use-os na **criar um conector do AWS** página no gerenciamento de custos.

Use o Assistente de criação de nova função:

1. Entre no seu console do AWS e selecione **Services**.
2. Na lista de serviços, selecione **IAM**.
3. Selecione **funções** e, em seguida, selecione **Create Role**.
4. Na próxima página, selecione **outra conta do AWS**.
5. Na **ID da conta**, insira **432263259397**.
6. Na **opções**, selecione **exigir ID externa (melhor prática quando um terceiro assumirá que essa função)** .
7. Na **ID externa**, insira a ID externa. A ID externa é uma senha compartilhada entre a função do AWS e o gerenciamento de custos do Azure. A mesma ID externa também é usada na **novo conector** página no gerenciamento de custos. Por exemplo, se parece com uma ID externa _Companyname1234567890123_.

    > [!NOTE]
    > Não altere a seleção para **exigir MFA**. Ele deve permanecer desmarcado.
8. Selecione **Avançar: Permissões.**
9. Selecione **Criar política**. Uma nova guia do navegador é aberta. Isso é onde você pode criar uma política.
10. Selecione **escolher um serviço**.

Configure a permissão para o relatório de uso e custo:

1. Insira **relatório de uso e custo**.
2. Selecione **nível de acesso** > **leitura** > **DescribeReportDefinitions**. Esta etapa permite o gerenciamento de custos ler a quais relatórios CUR são definidos e determinar se eles correspondem o pré-requisito de definição de relatório.
3. Selecione **adicionar permissões adicionais**.

Configure a permissão para seus objetos e o bucket S3:

1. Selecione **escolher um serviço**.
2. Enter **S3**.
3. Selecione **nível de acesso** > **lista** > **ListBucket**. Esta ação obtém a lista de objetos no Bucket de S3.
4. Selecione **nível de acesso** > **leitura** > **GetObject**. Essa ação permite que o download de arquivos de cobrança.
5. Selecione **recursos**.
6. Selecione **bucket – adicionar ARN**.
7. Na **nome do Bucket**, insira o número de buckets usado para armazenar os arquivos CUR.
8. Selecione **objeto – adicionar ARN**.
9. Na **nome do Bucket**, insira o número de buckets usado para armazenar os arquivos CUR.
10. Na **nome do objeto**, selecione **qualquer**.
11. Selecione **adicionar permissões adicionais**.

Configure a permissão para o Gerenciador de custo:

1. Selecione **escolher um serviço**.
2. Insira **serviço do Gerenciador de custo**.
3. Selecione **ações de todos os serviço de Gerenciador de custo (ce:\*)** . Esta ação valida se a coleção está correta.
4. Selecione **adicionar permissões adicionais**.

Adicione a permissão para organizações de AWS:

1. Insira **organizações**.
2. Selecione **nível de acesso** > **lista** > **ListAccounts**. Esta ação obtém os nomes das contas.
3. Na **examinar política**, insira um nome para a nova política. Verifique que você inseriu as informações corretas e, em seguida, selecione **Criar política**.
4. Volte à guia anterior e atualize a página da Web do seu navegador. Na barra de pesquisa, procure a nova política.
5. Selecione **próxima: revisão**.
6. Insira um nome para a nova função. Verifique que você inseriu as informações corretas e, em seguida, selecione **Create Role**.

    Observe a função ARN e a ID externa usado nas etapas anteriores, quando você criou a função. Você vai usá-los posteriormente quando você configura o conector de gerenciamento de custos do Azure.

A política JSON deve se parecer com o exemplo a seguir. Substitua _bucketname_ com o nome do seu bucket de S3.

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
"organizations:ListAccounts",
            "ce:*",
            "cur:DescribeReportDefinitions"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucketname",
                "arn:aws:s3:::bucketname/*"
            ]
        }
    ]
}
```

## <a name="set-up-a-new-aws-connector-in-azure"></a>Configurar um novo conector do AWS no Azure

Use as informações a seguir para criar um conector do AWS e começar a monitorar os custos do AWS:

1. Entre no [Portal do Azure](https://portal.azure.com).
2. Vá para **gerenciamento + cobrança de custos** > **gerenciamento de custos**.
3. Sob **as configurações**, selecione **conectores (visualização) de nuvem**.  
    ![Exemplo mostrando os conectores de nuvem (visualização) configuração)](./media/aws-integration-setup-configure/cloud-connectors-preview01.png).
4. Selecione **+ adicionar** na parte superior da página para criar um conector.
5. Sobre o **criar um conector do AWS** página, na **nome de exibição**, insira um nome para seu conector.  
    ![Exemplo da página para criar um conector do AWS](./media/aws-integration-setup-configure/create-aws-connector01.png)
6. Opcionalmente, selecione o grupo de gerenciamento padrão. Ele armazenará descobertas todas as contas vinculadas. Você pode configurá-lo mais tarde.
7. No **cobrança** seção, selecione **cobrar automaticamente o % 1 na disponibilidade geral** se você quiser garantir a operação contínua quando o preview expira. Se você selecionar a opção automática, você deve selecionar uma assinatura de cobrança.
8. Para **arn da função**, insira o valor que você usou quando definiu a função no AWS.
9. Para **ID externa**, insira o valor que você usou quando definiu a função no AWS.
10. Para **nome do relatório**, insira o nome que você criou no AWS.
11. Selecione **próxima** e, em seguida, selecione **criar**.

Pode levar algumas horas para que os novos escopos do AWS, o AWS consolidados conta e contas do AWS vinculado e exibir seus dados de custo.

Depois de criar o conector, é recomendável que você atribua o controle de acesso a ele. Os usuários recebem permissões para os escopos de descoberta recentemente: AWS consolidados conta e contas do AWS vinculado. O usuário que cria o conector é o proprietário do conector, a conta consolidada e todas as contas vinculadas.

Atribuindo permissões de conector aos usuários após a descoberta não atribuir permissões para os escopos do AWS existentes. Em vez disso, somente novas contas vinculadas são atribuídas permissões.

## <a name="take-additional-steps"></a>Etapas adicionais

- [Configurar grupos de gerenciamento](../governance/management-groups/index.md#initial-setup-of-management-groups), se você ainda não fez isso.
- Verifique se novos escopos são adicionados ao seu seletor de escopo. Selecione **Refresh** para exibir os dados mais recentes.
- Sobre o **conectores de nuvem** , selecione o conector e selecione **vá para a conta de cobrança** para atribuir a conta vinculada a grupos de gerenciamento.

## <a name="manage-cloud-connectors"></a>Gerenciar conectores de nuvem

Quando você seleciona um conector na **conectores de nuvem** página, você pode:

- Selecione **ir para a conta de cobrança** para exibir informações para o AWS consolidado conta.
- Selecione **controle de acesso** para gerenciar a atribuição de função para o conector.
- Selecione **editar** para atualizar o conector. Você não pode alterar o número da conta AWS, porque ele aparece na arn da função. Mas você pode criar um novo conector.
- Selecione **Verify** para executar novamente o teste de verificação para certificar-se de que o gerenciamento de custos pode coletar dados usando as configurações do conector.

![Lista de exemplos de conectores criados do AWS](./media/aws-integration-setup-configure/list-aws-connectors.png)

## <a name="set-up-azure-management-groups"></a>Configurar grupos de gerenciamento do Azure

Para criar um único lugar para exibir informações do provedor de nuvem, você precisa colocar suas assinaturas do Azure e contas do AWS vinculados no mesmo grupo de gerenciamento. Se você ainda não tiver configurado seu ambiente do Azure com grupos de gerenciamento, consulte [instalação de grupos de gerenciamento inicial](../governance/management-groups/index.md#initial-setup-of-management-groups).

Se você desejar separar os custos, você pode criar um grupo de gerenciamento que contém apenas as contas do AWS vinculado.

## <a name="set-up-an-aws-consolidated-account"></a>Configurar um AWS consolidados conta

A conta do AWS consolidados combina a cobrança e pagamento para várias contas do AWS. Ele também atua como uma conta do AWS vinculado.

![Detalhes de exemplo para um AWS consolidados conta](./media/aws-integration-setup-configure/aws-consolidated-account01.png)

Na página, você pode:

- Selecione **atualizar** para atualização em massa a associação de AWS vinculado contas com um grupo de gerenciamento.
- Selecione **controle de acesso** para definir a atribuição de função para o escopo.

### <a name="permissions-for-an-aws-consolidated-account"></a>Permissões para um AWS consolidados conta

Por padrão, as permissões para uma conta do AWS consolidados são definidas no momento da criação da conta, com base nas permissões de conector do AWS. O criador do conector é o proprietário.

Gerenciar o nível de acesso usando o **nível de acesso** página do AWS consolidados conta. No entanto, o AWS vinculado contas não herdam permissões para a conta do AWS consolidados.

## <a name="set-up-an-aws-linked-account"></a>Configurar uma conta do AWS vinculado

A conta do AWS vinculado é onde os recursos do AWS são criados e gerenciados. Uma conta vinculada também atua como um limite de segurança.

Nessa página, você pode:

- Selecione **atualizar** para atualizar a associação de um AWS vinculado conta com um grupo de gerenciamento.
- Selecione **controle de acesso** para definir uma atribuição de função para o escopo.

![Exemplo de página da conta vinculada do AWS](./media/aws-integration-setup-configure/aws-linked-account01.png)

### <a name="permissions-for-an-aws-linked-account"></a>As permissões para um AWS vinculados conta

Por padrão, as permissões para uma conta do AWS vinculado são definidas no momento da criação, com base nas permissões de conector do AWS. O criador do conector é o proprietário. Gerenciar o nível de acesso usando o **nível de acesso** conta vinculada de página do AWS. AWS vinculado contas não herdam as permissões da conta do AWS consolidados.

Contas do AWS vinculadas sempre herdam permissões do grupo de gerenciamento ao qual eles pertencem.

## <a name="next-steps"></a>Próximas etapas

- Agora que você ter instalado e configurado a integração de relatório de uso e custo do AWS, vá para [uso e os custos do AWS gerenciar](aws-integration-manage.md).
- Se você estiver familiarizado com a análise de custo, consulte [explorar e analisar os custos com análise de custo](quick-acm-cost-analysis.md) guia de início rápido.
- Se você estiver familiarizado com orçamentos no Azure, consulte [criar e gerenciar orçamentos Azure](tutorial-acm-create-budgets.md).

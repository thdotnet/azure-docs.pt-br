---
title: "Conectar-se ao Azure DocumentDB usando ferramentas de análise de BI | Microsoft Docs"
description: "Aprenda a usar o driver ODBC do Azure DocumentDB para criar tabelas e exibições para que os dados normalizados possam ser exibidos no software de análise de dados e BI."
keywords: odbc, driver odbc
services: documentdb
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 9967f4e5-4b71-4cd7-8324-221a8c789e6b
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 01/26/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: f1b0fde1e6e31a8179ed61508348d850c5dd784f
ms.openlocfilehash: 9e2c0cff442f7c66a4b1c76ab612175410f49497


---

# <a name="connect-to-documentdb-using-bi-analytics-tools-with-the-odbc-driver"></a>Conectar-se ao DocumentDB usando ferramentas de análise de BI com o driver ODBC

O driver ODBC do DocumentDB permite que você se conecte ao DocumentDB usando ferramentas de análise de BI como SQL Server Integration Services, Power BI Desktop e Tableau para que você possa analisar e criar visualizações dos dados do DocumentDB nessas soluções.

O driver ODBC do DocumentDB está em conformidade com o ODBC 3.8 e dá suporte à sintaxe ANSI SQL-92. O driver oferece recursos avançados para ajudá-lo a renormalizar dados no DocumentDB. Usando o driver, você pode representar dados no DocumentDB como tabelas e exibições. O driver permite que você execute operações de SQL em tabelas e exibições incluindo o agrupamento por consultas, inserções, atualizações e exclusões.

## <a name="why-do-i-need-to-normalize-my-data"></a>Por que preciso normalizar meus dados?
O DocumentDB é um banco de dados NoSQL, portanto, ele permite o desenvolvimento rápido de aplicativos permitindo que aplicativos iterem seu modelo de dados em tempo real e não os limite a um esquema rígido. Um único banco de dados DocumentDB pode conter documentos JSON de várias estruturas. Isso é ótimo para o método RAD, mas quando você deseja analisar e criar relatórios de dados usando ferramentas de BI e análise de dados, os dados geralmente precisam ser nivelados e aderir a um esquema específico.

Esse é o ponto em que o driver ODBC entra. Usando o driver ODBC, você agora pode renormalizar dados no DocumentDB em tabelas e exibições atendendo às suas necessidades de relatórios e análise de dados. Os esquemas renormalizados não têm impacto sobre os dados subjacentes e não confinam os desenvolvedores para aderir a eles, eles simplesmente permitem utilizar ferramentas em conformidade com ODBC para acessar os dados. Agora seu banco de dados DocumentDB não será apenas um favorito para sua equipe de desenvolvimento, mas seus analistas de dados o amarão também.

Vamos começar com o driver ODBC.

## <a name="a-idinstallastep-1-install-the-documentdb-odbc-driver"></a><a id="install"></a>Etapa 1: Instalar o driver ODBC do DocumentDB
1. Baixe o [Microsoft Azure DocumentDB ODBC 64-bit.msi](https://aka.ms/documentdb-odbc-64x64) para o sistema operacional Windows de 64 bits ou o [Microsoft Azure DocumentDB ODBC 32-bit.msi](https://aka.ms/documentdb-odbc-32x32) para o sistema operacional Windows de 32 bits.2. Execute o arquivo msi localmente, que inicia o **Assistente de Instalação do Driver ODBC do Microsoft Azure DocumentDB**. 
2. Conclua o assistente de instalação usando a entrada padrão para instalar o driver ODBC.
3. Abra o aplicativo **Administrador de Fonte de Dados ODBC** no computador, você pode fazer isso digitando **Fontes de dados ODBC** na caixa de pesquisa do Windows. 
    Você pode confirmar se driver foi instalado clicando na guia **Drivers** e assegurando que **Driver ODBC do Microsoft DocumentDB** está listado.

    ![Administrador de fonte de dados ODBC do DocumentDB](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver.png)

## <a name="a-idconnectastep-2-connect-to-your-documentdb-database"></a><a id="connect"></a>Etapa 2: Conectar-se ao seu banco de dados DocumentDB

1. Depois de [Instalar o driver ODBC do DocumentDB](#install), na janela **Administrador de Fonte de Dados do ODBC**, clique em **Adicionar**. Você pode criar um DSN de usuário ou de sistema. Neste exemplo, estamos criando um DSN de usuário.
2. Na janela **Criar Nova Fonte de Dados**, selecione **Driver ODBC do Microsoft DocumentDB** e clique em **Concluir**.
3. Na janela **Instalação de SDN do Driver ODBC do DocumentDB**, preencha o seguinte: 

    ![Janela Instalação de DSN do Driver ODBC do DocumentDB](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-dsn-setup.png)
    - **Nome da Fonte de Dados**: seu próprio nome amigável para o DSN do ODBC. Esse nome é exclusivo para sua conta do DocumentDB, portanto, nomeie-a corretamente se você tiver várias contas.
    - **Descrição**: uma breve descrição da fonte de dados.
    - **Host**: URI da sua conta do DocumentDB. Você pode recuperá-lo da folha de Chaves do DocumentDB no Portal do Azure, conforme mostra a captura de tela a seguir. 
    - **Tecla de Acesso**: a chave primária ou secundária de leitura e gravação ou somente leitura da folha de Chaves do DocumentDB no Portal do Azure, conforme mostra a captura de tela a seguir. É recomendável que você use a chave somente leitura se o DSN for usado para relatórios e processamento de dados somente leitura.
    ![Folha de Chaves do DocumentDB](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-keys.png)
    - **Criptografar a Tecla de Acesso para**: selecione a melhor opção com base nos usuários deste computador. 
4. Clique no botão **Testar** para ter certeza de que você pode se conectar à sua conta do DocumentDB. 
5. Clique em **Opções Avançadas de** e defina os seguintes valores:
    - **Consistência de Consulta**: selecione o [nível de consistência](documentdb-consistency-levels.md) para as operações. O padrão é Sessão.
    - **Número de tentativas**: insira o número de vezes para tentar novamente uma operação se a solicitação inicial não for concluída devido à limitação.
    - **Arquivo de Esquema**: você tem várias opções aqui.
        - Por padrão, deixando a essa entrada como está (vazia), o driver examina os dados da primeira página de todas as coleções para determinar o esquema de cada coleção. Isso é conhecido como Mapeamento de Coleção. Sem um arquivo de esquema definido, o driver deve executar o exame para cada sessão de driver e pode resultar em um tempo de inicialização mais alto de um aplicativo usando o DSN. É recomendável sempre associar um arquivo de esquema para um DSN.
        - Se você já tiver um arquivo de esquema (possivelmente um criado usando o [Editor de Esquema](#schema-editor)), poderá clicar em **Procurar**, navegar até o arquivo, clicar em **Salvar** e clicar em **OK**.
        - Se você desejar criar um novo esquema, clique em **OK** e clique em **Editor de Esquema** na janela principal. Em seguida, vá para as informações do [Editor de Esquema](#schema-editor). Ao criar o novo arquivo de esquema, não se esqueça de voltar para a janela **Opções Avançadas** para incluir o arquivo de esquema recém-criado.

6. Depois de concluir e fechar a janela **Instalação de DSN do Driver ODBC do DocumentDB**, o novo DSN de Usuário é adicionado à guia DSN de Usuário.

    ![Novo DSN do ODBC do DocumentDB na guia DSN de Usuário](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-user-dsn.png)

## <a name="a-idcollection-mappingastep-3-create-a-schema-definition-using-the-collection-mapping-method"></a><a id="#collection-mapping"></a>Etapa 3: Criar uma definição de esquema usando o método de mapeamento de coleção

Há dois tipos de métodos de amostragem que você pode usar: **mapeamento de coleção** ou **delimitadores de tabela**. Uma sessão de amostragem pode utilizar ambos os métodos de amostragem, mas cada coleta pode usar apenas um método de amostragem específico. As etapas a seguir criam um esquema para os dados em uma ou mais coleções usando o método de mapeamento de coleção. Esse método de amostragem recupera os dados na página de uma coleção para determinar a estrutura dos dados. Ele transpõe uma coleção para uma tabela no lado do ODBC. Esse método de amostragem é eficiente e rápido quando os dados em uma coleção são homogêneos. Se uma coleção contiver o tipo heterogêneo de dados, será recomendável usar o [método de mapeamento de delimitadores de tabela](#table-mapping) uma vez que ele fornece um método de amostragem mais robusto para determinar as estruturas de dados na coleção. 

1. Depois de concluir as etapas 1 a 4, em [Conectar-se ao seu banco de dados DocumentDB](#connect), clique em **Editor de Esquema** na janela **Instalação de DSN do Driver ODBC do DocumentDB**.

    ![Botão do editor de esquema na janela Instalação de DSN do Driver ODBC do DocumentDB](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-schema-editor.png)
2. Na janela **Editor de Esquema**, clique em **Criar Novo**.
    A janela **Gerar Esquema** mostra todas as coleções na conta do DocumentDB. 
3. Selecione uma ou mais coleções para obter o exemplo e clique em **Exemplo**. 
4. Na guia **Modo de Exibição de Design**, o banco de dados, o esquema e a tabela são representados. Na exibição de tabela, o exame mostra o conjunto de propriedades associado aos nomes de coluna (Nome SQL, Nome da Fonte etc.).
    Para cada coluna, você pode modificar o nome SQL, o tipo SQL, o comprimento SQL (se aplicável), a escala (se aplicável), a precisão (se aplicável) e o que permite valor nulo da coluna.
    - Você pode definir **Ocultar Coluna** como **true** se deseja excluir a coluna dos resultados da consulta. As colunas marcadas com Ocultar Coluna = true não são retornadas para seleção e projeção, embora ainda façam parte do esquema. Por exemplo, você pode ocultar todas as propriedades obrigatórias do sistema do DocumentDB começando com “_”.
    - A coluna **id** é o único campo que não pode ser oculto porque ele é usado como a chave primária no esquema normalizado. 
5. Quando você terminar de definir o esquema, clique em **Arquivo** | **Salvar**, navegue até o diretório para salvar o esquema e clique em **Salvar**.

    Se no futuro você quiser usar este esquema com um DSN, abra a janela Instalação de DSN do Driver ODBC do DocumentDB (por meio do Administrador da Fonte de Dados ODBC), clique em Opções Avançadas e, na caixa Arquivo de Esquema, navegue até o esquema salvo. Salvar um arquivo de esquema em um DSN existente modifica a conexão DSN de escopo para os dados e a estrutura definidos pelo esquema.

## <a name="a-idtable-mappingastep-4-create-a-schema-definition-using-the-table-delimiters-mapping-method"></a><a id="table-mapping"></a>Etapa 4: Criar uma definição de esquema usando o método de mapeamento de delimitadores de tabela

Há dois tipos de métodos de amostragem que você pode usar: **mapeamento de coleção** ou **delimitadores de tabela**. Uma sessão de amostragem pode utilizar ambos os métodos de amostragem, mas cada coleta pode usar apenas um método de amostragem específico. 

As etapas a seguir criam um esquema para os dados em uma ou mais coleções usando o método de mapeamento de **delimitadores de tabela**. É recomendável que você use esse método de amostragem quando suas coleções contiverem tipos heterogêneos de dados. Você pode usar esse método para definir o escopo de amostragem para um conjunto de atributos e seus valores correspondentes. Por exemplo, se um documento contém uma propriedade "Type", você pode definir o escopo de amostragem para os valores dessa propriedade. O resultado final da amostragem seria um conjunto de tabelas para cada um dos valores de Type especificados. Por exemplo, Type = Carro gerará uma tabela Carro, enquanto Type = Avião gerará uma tabela Avião.

1. Depois de concluir as etapas 1 a 4, em [Conectar-se ao seu banco de dados DocumentDB](#connect), clique em **Editor de Esquema** na janela Instalação de DSN do Driver ODBC do DocumentDB.
2. Na janela **Editor de Esquema**, clique em **Criar Novo**.
    A janela **Gerar Esquema** mostra todas as coleções na conta do DocumentDB. 
3. Selecione uma coleção na guia **Exibição de Exemplo**, na coluna **Definição do Mapeamento** da coleção, clique em **Editar**. Em seguida, na janela **Definição do Mapeamento**, selecione o método **Delimitadores de Tabela**. Faremos o seguinte:

    a. Na caixa **Atributos**, digite o nome de uma propriedade de delimitador. Esta é uma propriedade do documento para a qual você deseja definir o escopo de amostragem, por exemplo, Cidade, e pressione Enter. 

    b. Se você quiser definir o escopo de amostragem para determinados valores para o atributo que acabou de inserir, selecione o atributo na caixa de seleção e insira um valor na caixa **Valor**, por exemplo, Seattle, e pressione Enter. Você pode continuar a adicionar vários valores para atributos. Apenas certifique-se de que o atributo correto esteja selecionado quando estiver inserindo valores.

    Por exemplo, se você incluir um valor de **Atributos** de Cidade e desejar limitar sua tabela para incluir apenas linhas com um valor de cidade de Nova York e Dubai, insira Cidade na caixa Atributos e Nova York e Dubai na caixa **Valores**.
4. Clique em **OK**. 
5. Depois de concluir as definições de mapeamento das coleções para as quais deseja realizar a amostragem, na janela **Editor de Esquema**, clique em **Exemplo**.
     Para cada coluna, você pode modificar o nome SQL, o tipo SQL, o comprimento SQL (se aplicável), a escala (se aplicável), a precisão (se aplicável) e o que permite valor nulo da coluna.
    - Você pode definir **Ocultar Coluna** como **true** se deseja excluir a coluna dos resultados da consulta. As colunas marcadas com Ocultar Coluna = true não são retornadas para seleção e projeção, embora ainda façam parte do esquema. Por exemplo, você pode ocultar todas as propriedades obrigatórias do sistema do DocumentDB começando com “_”.
    - A coluna **id** é o único campo que não pode ser oculto porque ele é usado como a chave primária no esquema normalizado. 
6. Quando você terminar de definir o esquema, clique em **Arquivo** | **Salvar**, navegue até o diretório para salvar o esquema e clique em **Salvar**.
7. Na janela **Instalação de DSN do Driver ODBC do DocumentDB**, clique em **Opções Avançadas**. Em seguida, na caixa **Arquivo de Esquema**, navegue até o arquivo de esquema salvo e clique em **OK**. Clique em **OK** novamente para salvar o DSN. Isso salva o esquema que você criou no DSN. 

## <a name="optional-creating-views"></a>(Opcional) Criando exibições
Você pode definir e criar exibições como parte do processo de amostragem. Essas exibições são equivalentes às exibições do SQL. Eles são somente leitura e são o escopo que as seleções e projeções do SQL do DocumentDB definiram. 

Para criar uma exibição para seus dados, na janela **Editor de Esquema**, na coluna **Definições de Exibição**, clique em **Adicionar** na linha da coleção para a qual realizar a amostragem. Em seguida, na janela **Definições de Exibição**, faça o seguinte:
1. Clique em **Novo**, insira um nome para exibição, por exemplo, EmployeesfromSeattleView e, em seguida, clique em **OK**.
2. Na janela **Editar Exibição**, insira uma consulta do DocumentDB. Ela deve ser uma consulta SQL do DocumentDB, por exemplo, `SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Gender, c.Manager FROM c WHERE c.City = “Seattle”` e clique em **OK**.

Você pode criar quantas exibições desejar. Depois de definir as exibições, você pode realizar a amostragem dos dados. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>Etapa 5: Exibir os dados em ferramentas de BI como Power BI Desktop

Você pode usar o novo DSN para conectar o DocumentADB com qualquer ferramenta em conformidade com ODBC, essa etapa simplesmente mostra como se conectar ao Power BI Desktop e criar uma visualização do Power BI.

1. Abra o Power BI Desktop.
2. Clique em **Obter Dados**.
3. Na janela **Obter Dados**, clique em **Outro** | **ODBC** | **Conectar**.
4. Na janela **Do ODBC**, selecione o nome da fonte de dados que você criou e clique em **OK**. Você pode deixar as entradas de **Opções Avançadas** em branco.
5. Na janela **Acesse uma fonte de dados usando um driver ODBC**, selecione **Padrão ou Personalizada** e clique em **Conectar**. Você não precisa incluir as **Propriedades de cadeia de conexão de credencial**.
6. Na janela **Navegador**, no painel esquerdo, expanda o banco de dados, o esquema e selecione a tabela. O painel de resultados inclui os dados usando o esquema que você criou.
7. Para visualizar os dados no Power BI Desktop, marque a caixa na frente do nome da tabela e clique em **Carregar**.
8. No Power BI Desktop, na extrema esquerda, selecione a guia Dados ![Guia Dados no Power BI Desktop](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-data-tab.png) para confirmar se os dados foram importados.
9. Agora você pode criar elementos visuais usando o Power BI clicando na guia Relatório ![guia Relatório no Power BI Desktop](./media/documentdb-nosql-odbc-driver/documentdb-nosql-odbc-driver-report-tab.png), em **Visual Novo** e personalizando o bloco. Para obter mais informações sobre como criar visualizações no Power BI Desktop, consulte [Tipos de visualização no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Solucionar problemas

Se você receber o seguinte erro, verifique se os valores de **Host** e **Tecla de Acesso** copiados do Portal do Azure na [Etapa 2](#connect) estão corretos e tente novamente. Use os botões de cópia à direita dos valores de **Host** e **Tecla de Acesso** no Portal do Azure para copiar os valores sem erros.

    [HY000]: [Microsoft][DocumentDB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"The input authorization token can't serve the request. Please check that the expected payload is built as per the protocol, and check the key being used. Server used the following payload to sign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o DocumentDB, consulte [O que é o DocumentDB?](documentdb-introduction.md).


<!--HONumber=Jan17_HO4-->



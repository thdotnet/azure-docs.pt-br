---
title: Reimplantar os pacotes do SQL Server Integration Services para o banco de dados SQL do Azure | Microsoft Docs
description: Saiba como migrar pacotes do Integration Services do SQL Server para o banco de dados SQL.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/08/2019
ms.openlocfilehash: 603a9df8e3f499c832bbfdcbef966de86003d6b7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080646"
---
# <a name="redeploy-sql-server-integration-services-packages-to-azure-sql-database"></a>Reimplantar os pacotes do SQL Server Integration Services para o banco de dados SQL

Se você usar o SQL Server Integration Services (SSIS) e deseja migrar seus projetos/pacotes do SSIS da origem SSISDB hospedado pelo SQL Server para o SSISDB hospedado pelo banco de dados SQL de destino, você poderá reimplantá-los usando a implantação do Integration Services Assistente. Você pode iniciar o Assistente de dentro do SSMS (SQL Server Management Studio).

Se a versão do SSIS que você usa for anterior a 2012, antes de reimplantar os projetos/pacotes do SSIS no modelo de implantação de projeto, primeiro será necessário convertê-los usando o 	Assistente de Conversão de Projetos do Integration Services, que também pode ser iniciado pelo SSMS. Para obter mais informações, consulte o artigo [Converter projetos para o modelo de implantação de projeto](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> O serviço de migração de banco de dados do Azure (DMS) atualmente não dá suporte a migração de uma fonte de SSISDB a um servidor de banco de dados SQL, mas você pode reimplantar seus projetos/pacotes do SSIS usando o processo a seguir.

Neste artigo, você aprenderá a:
> [!div class="checklist"]
>
> * Avaliar projetos/pacotes do SSIS de origem.
> * Migrar projetos/pacotes do SSIS para o Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir essas etapas, você precisa:

* SSMS versão 17.2 ou posterior.
* Uma instância de servidor de banco de dados de destino para hospedar o SSISDB. Se você não tiver uma, crie um servidor de banco de dados SQL (sem um banco de dados) usando o portal do Azure navegando para o SQL Server (somente servidor lógico) [formulário](https://ms.portal.azure.com/#create/Microsoft.SQLServer).
* SSIS devem ser provisionado no ADF Azure Data Factory () que contém o IR Azure-SSIS Integration Runtime () com o SSISDB hospedado pela instância do servidor de banco de dados SQL de destino (conforme descrito no artigo [provisionar a integração do Azure-SSIS Tempo de execução no Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)).

## <a name="assess-source-ssis-projectspackages"></a>Avaliar projetos/pacotes do SSIS de origem

Enquanto a avaliação do código-fonte SSISDB ainda não foi integrada para o Assistente de migração do banco de dados (DMA) ou o serviço de migração de banco de dados do Azure (DMS), os projetos/pacotes do SSIS será avaliado/validado conforme eles são implantados novamente para o destino no SSISDB hospedado em um Servidor de banco de dados SQL do Azure.

## <a name="migrate-ssis-projectspackages"></a>Migrar projetos/pacotes do SSIS

Para migrar projetos/pacotes do SSIS para o servidor de banco de dados SQL, execute as seguintes etapas.

1. Abra o SSMS e, em seguida, selecione **Opções** para exibir a caixa de diálogo **Conectar ao Servidor**.

2. Sobre o **Login** guia, especifique as informações necessárias para se conectar ao servidor de banco de dados SQL que irá hospedar o SSISDB de destino.

    ![Guia Logon do SSIS](media/how-to-migrate-ssis-packages/dms-ssis-login-tab.png)

3. Na guia **Propriedades da Conexão**, na caixa de texto **Conectar ao banco de dados**, selecione ou insira **SSISDB** e, em seguida, selecione **Conectar**.

    ![Guia Propriedades da Conexão do SSIS](media/how-to-migrate-ssis-packages/dms-ssis-conncetion-properties-tab.png)

4. No Pesquisador de Objetos do SSMS, expanda o nó **Catálogo do Integration Services**, expanda **SSISDB** e, se não houver nenhuma pasta existente, clique com botão direito em **SSISDB** e crie uma nova pasta.

5. Em **SSISDB**, expanda qualquer pasta, clique com botão direito em **Projetos** e, em seguida, selecione **Implantar Projeto**.

    ![Nó SSISDB do SSIS expandido](media/how-to-migrate-ssis-packages/dms-ssis-ssisdb-node-expanded.png)

6. No Assistente de Implantação do Integration Services, na página **Introdução**, examine as informações e, em seguida, selecione **Avançar**.

    ![Página Introdução do Assistente de Implantação](media/how-to-migrate-ssis-packages/dms-deployment-wizard-introduction-page.png)

7. Na página **Selecionar Origem**, especifique o projeto do SSIS existente que você deseja implantar.

    Se o SSMS também está conectado ao SQL Server que hospeda o SSISDB de origem, selecione **Catálogo do Integration Services** e, em seguida, insira o nome do servidor e o caminho do projeto no catálogo para implantar o projeto diretamente.

    Como alternativa, selecione **Arquivo de implantação do projeto** e, em seguida, especifique o caminho para um arquivo de implantação do projeto existente (.ispac) para implantar o projeto.

    ![Página Selecionar Origem do Assistente de Implantação](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-source-page.png)
 
8. Selecione **Avançar**.
9. Na página **Selecionar Destino**, especifique o destino para o projeto.

    a. Na caixa de texto do nome do servidor, insira o nome totalmente qualificado do servidor de banco de dados SQL (< nome_do_servidor >. database.windows.net).

    b. Forneça as informações de autenticação e, em seguida, selecione **Conectar**.

    ![Página Selecionar Destino do Assistente de Implantação](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-destination-page.png)

    c. Selecione **navegue** para especificar a pasta de destino no SSISDB e, em seguida, selecione **próxima**.

    > [!NOTE]
    > O botão **Avançar** será habilitado somente após você ter selecionado **Conectar**.

10. Na página **Validar**, exiba quaisquer erros/avisos e, se necessário, modifique os pacotes adequadamente.

    ![Página Validar do Assistente de Implantação](media/how-to-migrate-ssis-packages/dms-deployment-wizard-validate-page.png)

11. Selecione **Avançar**.

12. Na página **Examinar**, examine as configurações de implantação.

    > [!NOTE]
    > Você pode alterar suas configurações, selecionando **anterior** ou selecionando qualquer um dos links etapa no painel esquerdo.

13. Selecione **Implantar** para iniciar o processo de implantação.

14. Após a conclusão do processo de implantação, você pode exibir a página de resultados, que exibe o êxito ou a falha de cada ação de implantação.
    a. Se qualquer ação falhou, na coluna **Resultado**, selecione **Falhou** para exibir uma explicação do erro.
    b. Opcionalmente, selecione **Salvar Relatório** para salvar os resultados em um arquivo XML.

15. Selecione **Fechar** para sair do Assistente de Implantação do Integration Services.

Se a implantação do seu projeto teve êxito sem falhas, é possível selecionar todos os pacotes contidos nele para executar no Azure-SSIS IR.

## <a name="next-steps"></a>Próximas etapas

* Verificar as diretrizes de migração no [Guia de Migração de Banco de Dados da Microsoft](https://datamigration.microsoft.com/).

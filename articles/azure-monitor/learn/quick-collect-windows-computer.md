---
title: Configurar o Agente do Azure Log Analytics para computadores Windows híbridos | Microsoft Docs
description: Neste guia de início rápido, veja como implantar o Agente do Log Analytics para computadores Windows em execução fora do Microsoft Azure e como habilitar a coleta de dados com o Log Analytics.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 04/09/2019
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 15885137b9559bf34fb2b985398401af09caa629
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602989"
---
# <a name="configure-the-log-analytics-agent-for-windows-computers-in-a-hybrid-environment"></a>Configurar o Agente do Log Analytics para computadores Windows em um ambiente híbrido
O [Azure Log Analytics](../../azure-monitor/platform/agent-windows.md) pode coletar dados diretamente dos computadores Windows físicos ou virtuais em um único repositório para análise e correlação detalhadas. O Log Analytics pode coletar dados de um datacenter ou de outro ambiente de nuvem. Este guia de início rápido mostra como configurar e coletar dados do computador Windows com algumas etapas simples.  Para saber mais sobre VMs do Windows no Microsoft Azure, confira o tópico [Coletar dados sobre máquinas virtuais do Azure](../../azure-monitor/learn/quick-collect-azurevm.md).  

Para entender a configuração com suporte, confira os tópicos [Suporte para sistemas operacionais Windows](../../azure-monitor/platform/log-analytics-agent.md#supported-windows-operating-systems) e [Configuração de firewall de rede](../../azure-monitor/platform/log-analytics-agent.md#network-firewall-requirements).
 
Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-the-azure-portal"></a>Entre no Portal do Azure
Entre no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-workspace"></a>Criar um workspace
1. No portal do Azure, clique em **Todos os serviços**. Na caixa de pesquisa, insira **Log Analytics**. A lista filtra com base em sua entrada, à medida que você digita. Selecione **Log Analytics**:

    ![Portal do Azure](media/quick-collect-windows-computer/azure-portal-01.png)
  
2. Selecione **Criar** e forneça os seguintes detalhes:

   * Insira um nome para o novo **espaço de trabalho do Log Analytics**. Algo parecido com **DefaultLAWorkspace**.
   * Selecione uma **assinatura** para a qual vincular. Se o padrão não for a opção desejada, selecione outra na lista.
   * Para o **Grupo de Recursos**, selecione um grupo de recursos existente que contém uma ou mais máquinas virtuais do Microsoft Azure.  
   * Selecione o **Local** no qual as VMs serão implantadas. Aqui está uma lista de [regiões em que o Log Analytics está disponível](https://azure.microsoft.com/regions/services/).  
   * Se estiver criando um workspace para uma assinatura feita após 2 de abril de 2018, ele usará automaticamente o plano de preços **por GB**. Não será possível selecionar um tipo de preço. Se estiver criando um workspace para uma assinatura feita antes de 2 de abril de 2018 ou uma assinatura associada a um registro de EA existente, escolha o tipo de preço desejado. Para saber mais sobre os tipos de preço, confira as [informações de preço do Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Criar recursos de Log Analytics](media/quick-collect-windows-computer/create-loganalytics-workspace-02.png)<br>  

3. Após fornecer as informações necessárias no painel do **espaço de trabalho do Log Analytics**, selecione **OK**.  

Você pode acompanhar o progresso no menu **Notificações**, enquanto as informações são verificadas e o workspace é criado.

## <a name="get-the-workspace-id-and-key"></a>Obter a ID e a chave do workspace
Para instalar o Microsoft Monitoring Agent para Windows, é necessário obter a ID e a chave do espaço de trabalho do Log Analytics. O assistente de instalação exige essas informações para configurar adequadamente o agente e garantir que ele possa se comunicar com o Log Analytics.  

1. No canto superior esquerdo do portal do Azure, selecione **Todos os serviços**. Na caixa de pesquisa, insira **Log Analytics**. A lista filtra com base em sua entrada, à medida que você digita. Selecione **Log Analytics**.
2. Na lista de espaços de trabalho do Log Analytics, selecione o espaço de trabalho criado anteriormente. Você deve ter atribuído a ele o nome de **DefaultLAWorkspace**.
3. Selecione **Configurações avançadas**:

    ![Configurações avançadas do Log Analytics](media/quick-collect-windows-computer/log-analytics-advanced-settings-01.png)
  
4. Selecione **Fontes Conectadas** e depois **Servidores Windows**.
5. Copie os valores à direita de **ID do workspace** e **Chave primária**. Cole-os em seu editor de preferência.

## <a name="install-the-agent-for-windows"></a>Instalar o agente para Windows
As etapas a seguir servem para instalar e configurar o agente do Log Analytics no Microsoft Azure e no Azure Governamental. Você usará o programa de instalação do Microsoft Monitoring Agent para instalar o agente no computador.

1. Dando continuidade do conjunto anterior de etapas, vá até a página **Servidores Windows** e selecione a versão de **Download do Agente do Windows** que você deseja baixar. Selecione a versão adequada da arquitetura do processador do sistema operacional Windows.
2. Execute a Instalação para instalar o agente no seu computador.
2. Sobre o **boas-vindas** página, selecione **próxima**.
3. Na página **Termos de Licença**, leia a licença e selecione **Aceito**.
4. Na página **Pasta de Destino**, altere ou mantenha a pasta de instalação padrão e selecione **Avançar**.
5. Na página **Opções de configuração do agente**, conecte o agente ao Azure Log Analytics e selecione **Avançar**.
6. Na página do **Azure Log Analytics**, faça o seguinte:
   1. Cole a **ID do workspace** e a **Chave do workspace (chave primária)** que você copiou anteriormente. Caso o computador esteja subordinado a um espaço de trabalho do Log Analytics no Azure Governamental, selecione **Azure US Government** na lista do **Azure Cloud**.  
   2. Caso o computador precise se comunicar por meio de um servidor proxy ao serviço Log Analytics, selecione **Avançado** e forneça a URL e o número da porta do servidor proxy. Caso o servidor proxy exija autenticação, digite o nome de usuário e a senha para se autenticar com o servidor proxy, e selecione **Avançar**.  
7. Depois de adicionar as definições de configuração, selecione **Avançar**:

    ![Configuração do Microsoft Monitoring Agent](media/quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)

8. Na página **Pronto para Instalar**, examine suas escolhas e selecione **Instalar**.
9. Na página **Configuração concluída com êxito**, selecione **Concluir**.

Após concluir o processo de instalação e configuração, o Microsoft Monitoring Agent é exibido no Painel de Controle. Você pode revisar sua configuração e verificar se o agente está conectado ao Log Analytics. Quando está conectado, o agente exibe a seguinte mensagem na guia do **Azure Log Analytics**: **O Microsoft Monitoring Agent se conectou com êxito ao serviço de Log Analytics da Microsoft.**<br><br> ![Status de conexão do MMA](media/quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

## <a name="collect-event-and-performance-data"></a>Coletar dados de desempenho e eventos
O Log Analytics pode coletar os eventos especificados dos logs de eventos do Windows e de contadores de desempenho para análise e geração de relatórios de longo prazo. Além disso, o recurso pode realizar uma ação quando detectar uma condição específica. Siga estas etapas para configurar a coleta de eventos do log de eventos do Windows e de vários contadores de desempenho comuns para começar.  

1. No canto superior esquerdo do portal do Azure, selecione **Mais serviços**. Na caixa de pesquisa, insira **Log Analytics**. A lista filtra com base em sua entrada, à medida que você digita. Selecione **Log Analytics**.
2. Selecione **Configurações avançadas**:

    ![Configurações avançadas do Log Analytics](media/quick-collect-windows-computer/log-analytics-advanced-settings-01.png)
 
3. Selecione **Dados** e, em seguida, **Logs de Eventos do Windows**.  
4. Para adicionar um novo log de eventos, digite o nome do log. Digite **Sistema** e, em seguida, selecione o sinal de adição ( **+** ).  
5. Selecione as severidades **Erro** e **Aviso** na tabela.
6. No início da página, selecione **Salvar**.
7. Selecione **Contadores de Desempenho do Windows** para habilitar a coleta de contadores de desempenho em um computador Windows.
8. Quando configura os contadores de desempenho do Windows para um novo espaço de trabalho do Log Analytics pela primeira vez, você tem a opção de criar rapidamente vários contadores comuns. Todas as opções são listadas com uma caixa de seleção ao lado de cada uma delas:

    ![Contadores de desempenho do Windows](media/quick-collect-windows-computer/windows-perfcounters-default.png).
    
    Selecione **Adicionar os contadores de desempenho selecionados**. Os contadores são adicionados e predefinidos com um intervalo de amostragem de coleta de dez segundos.

9. No início da página, selecione **Salvar**.

## <a name="view-collected-data"></a>Exibir dados coletados
Agora que você habilitou a coleta de dados, vamos executar uma pesquisa de logs simples para visualizar alguns dados do computador de destino.  

1. No portal do Azure, vá até o workspace selecionado e escolha o bloco **Logs**.  
2. No painel **Pesquisa de Logs**, digite **Desempenho** e clique em **Executar**, na parte superior da caixa de pesquisa:
 
    ![Pesquisa de logs do Log Analytics](media/quick-collect-windows-computer/log-analytics-portal-queryexample.png)

    Por exemplo, a consulta na imagem a seguir retornou 735 registros de desempenho:

    ![Resultado da pesquisa de logs do Log Analytics](media/quick-collect-windows-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Limpar recursos
Quando não for mais necessário, você poderá remover o agente do computador e excluir o espaço de trabalho do Log Analytics.  

Para remover o agente, faça o seguinte:

1. Abra o Painel de Controle.
2. Abra **Programas e Recursos**.
3. Em **Programas e Recursos**, selecione **Microsoft Monitoring Agent** e selecione **Desinstalar**.

Para excluir o espaço de trabalho do Log Analytics criado anteriormente, selecione-o; em seguida, vá até a página de recursos e selecione **Excluir**:

![Excluir espaço de trabalho do Log Analytics](media/quick-collect-windows-computer/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Próximas etapas
Agora que já está coletando dados operacionais e de desempenho do computador Windows, você pode facilmente começar a explorar, analisar e atuar em relação aos dados coletados *gratuitamente*.  

Para saber como exibir e analisar os dados, prossiga para o tutorial:

> [!div class="nextstepaction"]
> [Exibir ou analisar dados no Log Analytics](tutorial-viewdata.md)

---
title: Solução de problemas do Azure Data Factory | Microsoft Docs
description: Solução de problemas do Azure Data Factory. Documento comum para todos os externo, as atividades de controle.
services: data-factory
author: abnarain
manager: craigg
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 6/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: d220730bb2e93e32d00e56ed98f4962ad89eda5a
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626318"
---
# <a name="troubleshooting-azure-data-factory"></a>Solução de problemas do Azure Data Factory
Este artigo lista as perguntas comuns de solução de problemas.

- [O Azure Databricks (bloco de anotações, jars, python)](#azure-databricks)
- [Azure Data Lake Analytics (U-SQL)](#azure-data-lake-analytics-u-sql)
- [Funções do Azure](#azure-functions)
- [Personalizado (o lote do Azure)](#custom-azure-batch)
- [HDInsight (Spark, Hive, MapReduce, Pig, Streaming do Hadoop)](#hdinsight-spark-hive-mapreduce-pig-hadoop-streaming)
- [Atividade da Web](#web-activity)



## <a name="azure-databricks"></a>Azure Databricks
| Código do Erro | Mensagem de erro                                          | Descrição do problema                             | Correção possível / ação recomendada                            |
| -------------- | ----------------------------------------------------- | --------------------------------------------------------------| :----------------------------------------------------------- |
| 3200           | Erro 403                                                    | Token de acesso do Databricks expirou.                         | Por padrão, o token de acesso do Databricks é válido por 90 dias.  Por favor, criar um novo token e atualizar serviço vinculado. |
| 3201           | Campo obrigatório ausente: settings.task.notebook_task.notebook_path | Ruim de criação: Caminho de notebook não está especificado corretamente. | Especifique o caminho do notebook na atividade do Databricks. |
| 3201           | Cluster... não existe                                 | Erro de criação: Cluster do Databricks não existe ou foi excluído. | Verifique se o cluster Databricks existe. |
| 3201           | URI de arquivo do python inválido:... Visite o guia do usuário do Databricks para esquemas URI com suporte. | Ruim de criação                                                | Especifique caminhos absolutos para esquemas de endereçamento de espaço de trabalho, ou "dbfs:/folder/subfolder/foo.py" para arquivos armazenados em DBFS. |
| 3201           | {0}   LinkedService deve ter o domínio e accessToken como propriedades necessárias | Ruim de criação                                                | Verifique se [definição de serviço vinculado](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | {0}   LinkedService deve especificar a Id de cluster existente ou novas informações de cluster para criação | Ruim de criação                                                | Verifique se [definição de serviço vinculado](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | Não há suporte para o tipo de nó Standard_D16S_v3. Suporte para tipos de nó:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3, Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2, Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3, Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2, Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2, Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3, Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s, Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2, Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2, Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3, Standard_ NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2, Standard_L64s_v2, Standard_L80s_v2 | Ruim de criação                                                | Consulte a mensagem de erro                                          |
| 3201           | Notebook_path inválido:... Atualmente, há suporte para apenas os caminhos absolutos. Caminhos devem começar com '/'. | Ruim de criação                                                | Consulte a mensagem de erro                                          |
| 3202           | Já foram criados nos últimos 3600 segundos, excedendo o limite de taxa de 1000 trabalhos:   Criações de trabalho 1000 por 3600 segundos. | Número excessivo de Databricks é executado em uma hora.                         | Verifique se todos os pipelines que usam este espaço de trabalho do Databricks para taxa de criação de trabalho.  Se pipelines iniciados muitas execuções de Databricks na agregação, migre alguns pipelines para um novo espaço de trabalho. |
| 3202           | Não foi possível analisar o objeto de solicitação: Esperado 'key' e o valor a ser definido para base_parameters de campo de mapa JSON, obteve ' chave: ""... ' em vez disso. | Erro de criação: Nenhum valor fornecido para o parâmetro         | Inspecione o json do pipeline e verifique se que todos os parâmetros no bloco de anotações baseParameters tem um valor vazio especificado. |
| 3202           | Usuário: SimpleUserContext {userId =..., nome =user@company.com, orgId =...} não está autorizado a acessar o cluster | Usuário que gerou o token de acesso não tem permissão para acessar o cluster Databricks especificado no serviço vinculado. | Verifique se o usuário tem as permissões necessárias no espaço de trabalho.   |
| 3203           | O cluster está em estado de encerrado, não está disponível para receber trabalhos. Tente corrigir o cluster ou tente novamente mais tarde. | Cluster foi encerrado.    Para o cluster interativo, isso pode ser uma condição de corrida. | Melhor maneira de evitar isso é usar clusters de trabalho.             |
| 3204           | Falha na execução do trabalho. Pode haver qualquer número de mensagens de erro, de estado do cluster inesperado à mensagem específicos da atividade.  Mais comum não é nenhuma mensagem de erro. | N/D                                                          | N/D                                                          |



## <a name="azure-data-lake-analytics-u-sql"></a>Azure Data Lake Analytics (U-SQL)

| Código de erro         | Mensagem de erro                                                | Descrição do problema                                          | Correção possível / ação recomendada                             |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2709                 | O token de acesso é de locatário errado                    | Locatário do AAD incorreto                                         | A entidade de serviço usada para acessar o ADLA pertence a outro locatário do AAD. Crie a nova entidade de serviço no mesmo locatário que a conta do ADLA. |
| 2711,   2705,   2704 | Negado. Falha na verificação de ACL. O recurso não existe ou o usuário não está autorizado a executar a operação solicitada<br/><br/>Usuário não é capaz de acessar o data Lake store  <br/><br/>Usuário não está autorizado a análise data lake | A entidade de serviço ou o certificado fornecido não tem acesso ao arquivo no armazenamento | Verifique se a entidade de serviço ou certificados que eles fornecem para trabalhos do ADLA tem acesso à conta do ADLA e o armazenamento do ADLS padrão para ele da pasta raiz. |
| 2711                 | Não é possível localizar o arquivo 'Do Azure Data Lake Store' ou pasta       | O caminho para o arquivo de u-SQL é incorreto ou o serviço vinculado não tem acesso a credenciais | Verifique se o caminho e as credenciais fornecidas no serviço vinculado |
| 2707                 | Não é possível resolver a conta de AzureDataLakeAnalytics. Verifique se 'AccountName' e 'DataLakeAnalyticsUri'. | A conta do ADLA no serviço vinculado está errada                  | Verifique se que a conta certa é fornecida             |
| 2703                 | Id do erro: E_CQO_SYSTEM_INTERNAL_ERROR ou qualquer erro começa com "Id do erro:" | Erro é proveniente de ADLA                                    | Qualquer erro que se parece com o exemplo significa que o trabalho foi enviado para ADLA e o script lá com falha. A investigação deve ser feita no ADLA. Se você abre o portal e navegue até a conta do ADLA, procure o trabalho usando a atividade ADF (não execução do pipeline Id) da Id de execução. O trabalho existe terão mais informações sobre o erro e ajudará a solucionar problemas. Se a resolução não está clara, entre em contato com a equipe de suporte do ADLA e forneça a URL do trabalho, que inclui o nome da conta e ID do trabalho. |
| 2709                 | Não é possível aceitamos seu trabalho no momento. O número máximo de trabalhos em fila para sua conta é 200. | Limitação no ADLA                                           | Reduza o número de trabalhos enviados para ADLA alterando configurações de simultaneidade em atividades e gatilhos do ADF, ou aumentar os limites em ADLA |
| 2709                 | Este trabalho foi rejeitado porque ele requer 24 AUs e essa conta tem uma política definida pelo administrador que impede que um trabalho usando mais de 5 AUs. | Limitação no ADLA                                           | Reduza o número de trabalhos enviados para ADLA alterando configurações de simultaneidade em atividades e gatilhos do ADF, ou aumentar os limites em ADLA |



## <a name="azure-functions"></a>Verificação de

| Código de erro | Mensagem de erro                           | DESCRIÇÃO                                                  | Correção possível / ação recomendada                           |
| ------------ | --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 3600         | Conteúdo da resposta não é um JObject válido | Isso significa que a função do Azure que foi chamada não retornou um conteúdo JSON na resposta. Atividade de função do Azure do ADF suporta apenas o conteúdo da resposta JSON. | Atualizar a função do Azure para retornar uma carga JSON válido, por exemplo, um C# função de retorno (ActionResult) novo < OkObjectResult ("{`\"Id\":\"123\"`}"); |
| 3600         | Invalid HttpMethod: ‘..’.               | Isso significa que o método Http especificado na carga de atividade não é suportado pela atividade de função do Azure. | Os métodos Http com suporte são:  <br/>PUT, POST, OBTER, EXCLUIR, OPÇÕES, HEAD, RASTREAMENTO |



## <a name="custom-azure-batch"></a>Personalizado (o lote do Azure)
| Código de erro | Mensagem de erro                                                | DESCRIÇÃO                                                  | Correção possível / ação recomendada                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2500         | Exceção inesperada e Falha na execução de ocorrências.             | Não é possível iniciar o comando ou o programa retornou um código de erro. | Verifique se o executável existe. Se o programa é iniciado, verifique o stdout e stderr carregado para a conta de armazenamento. É uma boa prática para emitir grandes logs em seu código para depuração. |
| 2501         | Não é possível acessar a conta do lote de usuário, verifique se as configurações de conta do lote. | Lote acesso chave ou pool nome incorreto fornecido.            | Preciso verificar o nome do pool e a chave de acesso de lote no serviço vinculado. |
| 2502         | Pode não armazenamento de usuário de acesso de conta, por favor, verifique as configurações de conta de armazenamento. | Armazenamento incorreto acesso ou o nome da chave da conta fornecido.       | É necessário verificar se a chave de acesso e o nome de conta de armazenamento no serviço vinculado. |
| 2504         | Operação retornou um código de status inválido 'BadRequest'     | Muitos arquivos no caminho da pasta se a atividade personalizada.  (O tamanho total de resourceFiles não pode ser mais de 32768 caracteres.) | Remova os arquivos desnecessários ou zip-los e adicionar um comando unzip para extrair, por exemplo: powershell.exe - nologo - noprofile-comando "& {Add-Type - A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory ($zipFile, $folder); }" ;  $folder\yourProgram.exe |
| 2505         | Não é possível criar a assinatura de acesso compartilhado, a menos que as credenciais de chave de conta são usadas. | Atividades personalizadas oferecem suporte apenas a contas de armazenamento que usarem uma chave de acesso. | Consulte a descrição                                            |
| 2507         | O caminho da pasta não existe ou está vazio:...            | Não há arquivos na conta de armazenamento no caminho especificado.       | FolderPath deve conter os executáveis que você deseja executar. |
| 2508         | Pasta de recurso está arquivos duplicados.               | Há vários arquivos de mesmo nome em subpastas diferentes do folderPath. | Atividades personalizadas nivelar a estrutura de pastas na folderPath.  Se a estrutura de pastas deve ser preservado, os arquivos zip e extraia-os no lote do Azure com um comando descompactar, por exemplo: powershell.exe - nologo - noprofile-comando "& {Add-Type - A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory ($zipFile, $folder); }" ;   $folder\yourProgram.exe |
| 2509         | Url de lote... é inválido, ele deve estar no formato de Uri.         | URLs de lote devem ser semelhantes a https:\//mybatchaccount.eastus.batch.azure.com | Consulte a descrição                                            |
| 2510         | Ocorreu um erro ao enviar a solicitação.               | URL de lote é inválido                                         | Verifique se a URL do lote.                                            |

## <a name="hdinsight-spark-hive-mapreduce-pig-hadoop-streaming"></a>HDInsight (Spark, Hive, MapReduce, Pig, Streaming do Hadoop)

| Código de erro | Mensagem de erro                                                | DESCRIÇÃO                                                  | Correção possível / ação recomendada                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2300,   2310 | Falha no envio de trabalho do Hadoop. Erro: O nome remoto não pôde ser resolvido. <br/><br/>O cluster não foi encontrado. | URI do cluster é inválida                              | Verifique se o cluster não foi excluído e o URI fornecido está correto. Você pode abrir o URI em qualquer navegador, e você deverá ver a UI do Ambari. Se o cluster estiver em uma rede virtual, em seguida, o URI deve ser o URI privado e você deve tentar abri-lo de uma VM que faz parte da mesma rede virtual. Obter mais informações para [rede Virtual no HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services). |
| 2\.300         | Falha no envio de trabalho do Hadoop. Trabalho:..., Cluster:... /. Erro: Uma tarefa foi cancelada. | O envio do trabalho atingiu o tempo limite.                         | Isso pode ser um problema de conectividade geral do HDInsight ou problema de conectividade de rede. Primeiro confirme que a interface do usuário do HDInsight Ambari está disponível por meio de qualquer navegador e suas credenciais ainda são válidas. Certifique-se de fazer isso na VM/computador em que o IR auto-hospedado está instalado se usando o ir auto-hospedado. Em seguida, tente enviar o trabalho do ADF novamente. Se ele ainda falhar, entre em contato com a equipe do ADF para obter suporte. |
| 2\.300         | Não autorizado:   Nome de usuário do Ambari ou a senha está incorreta  <br/><br/>Não autorizado:   Administrador de usuário está bloqueada no Ambari   <br/><br/>403 - Proibido: Acesso negado | Credenciais fornecidas para o HDInsight estão incorretas ou expiradas | Corrija-os e reimplante o serviço vinculado. Verifique se que as credenciais de trabalho no HDInsight primeiro abrindo o URI do cluster em qualquer navegador e tentar fazer logon. Se eles não funcionarem, você pode redefini-las no Portal do Azure. |
| 2300,   2310 | 502 - o servidor web recebeu uma resposta inválida ao atuar como gateway ou proxy de servidor       <br/>Gateway incorreto | Erro é proveniente de HDInsight                               | Esse erro é proveniente de cluster do HDInsight. Consultar [solução de problemas do HDInsight](https://hdinsight.github.io/ambari/ambari-ui-502-error.html) com erros comuns.    <br/>Para clusters do Spark causa pode ser também devido à [isso](https://hdinsight.github.io/spark/spark-thriftserver-errors.html). <br/><br/>[Link adicionais](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502) |
| 2\.300         | Falha no envio de trabalho do Hadoop. Trabalho:..., Cluster:... Erro: {\"erro\":\"não é possível atender à solicitação de trabalho de envio como serviço do templeton está ocupado com muitos enviam solicitações de trabalho. Aguarde algum tempo antes de tentar novamente a operação. Consulte o templeton.parallellism.job.submit config para configurar as solicitações simultâneas. \  <br/><br/>Falha no envio de trabalho do Hadoop. Trabalho: 161da5d4-6fa8-4ef4-a240-6b6428c5ae2f, Cluster: https:\//abc-analytics-prod-hdi-hd-trax-prod01.azurehdinsight.net/.   Error: {\"error\":\"java.io.IOException:   org.apache.hadoop.yarn.exceptions.YarnException: Failed to submit   application_1561147195099_3730 to YARN :   org.apache.hadoop.security.AccessControlException: Fila root.joblauncher já tem 500 aplicativos, não é possível aceitar o envio de aplicativo: application_1561147195099_3730\ | Muitos trabalhos estão sendo enviados para o HDInsight ao mesmo tempo | Considere limitar o número de trabalhos simultâneos que estão sendo enviadas para o HDI. Consulte a simultaneidade de atividade do ADF se eles estão sendo enviados pela mesma atividade. Altere os gatilhos para que as execuções de pipeline simultâneas são distribuídas ao longo do tempo. Consulte também docs do HDInsight para ajustar o "templeton.parallellism.job.submit" como erro sugere. |
| 2303,   2347 | Trabalho do Hadoop falhou com código de saída '5'. Consulte 'wasbs://adfjobs@adftrialrun.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr' para obter mais detalhes.  <br/><br/>Falha na execução de hive com o código de erro 'UserErrorHiveOdbcCommandExecutionFailure'.   Consulte 'wasbs://adfjobs@eclsupplychainblobd.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out' para obter mais detalhes | O trabalho foi enviado para o HDInsight, e houve falha no HDInsight | O trabalho foi enviado com êxito para o HDInsight. Houve falha no cluster. Por favor, abra o trabalho na interface do usuário do Ambari do HDInsight e abrir os logs de lá ou abra o arquivo do armazenamento, como os pontos de mensagem de erro-out. Os detalhes do erro será nesse arquivo. |
| 2328         | Erro de servidor interno ao processar a solicitação. Tente novamente a solicitação ou contate o suporte | Ocorre em HDInsight sob demanda.                              | Esse erro é proveniente de serviço do HDInsight quando não é o provisionamento do HDInsight. Entre em contato com a equipe do HDInsight e forneça o nome do cluster por demanda no. |
| 2310         | java.lang.NullPointerException                               | Ocorreu um erro ao enviar o trabalho para o cluster Spark      | Essa exceção é proveniente de HDInsight e está ocultando o problema real.   Contate a equipe do HDInsight para suporte e fornecerá o nome do cluster e o intervalo de tempo de execução da atividade. |
|              | Todos os outros erros                                             |                                                              | Consulte a [solução de problemas do HDInsight](../hdinsight/hdinsight-troubleshoot-guide.md) e [perguntas frequentes sobre o HDInsight](https://hdinsight.github.io/) |



## <a name="web-activity"></a>Atividade da Web

| Código de erro | Mensagem de erro                                                | DESCRIÇÃO                                                  | Correção possível / ação recomendada                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2108         | Invalid HttpMethod: ‘..’.                                    | Isso significa que o método Http especificado na carga de atividade não é suportado pela atividade da Web. | Os métodos Http com suporte são: <br/>PUT, POST, OBTER, EXCLUIR |
| 2108         | Erro de servidor inválido 500                                     | Erro interno no ponto de extremidade                               | Verifique a funcionalidade na URL (com o Fiddler/Postman): [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 401 não autorizado                                             | Falta de autenticação válido na solicitação                      | Fornecer um método de autenticação válido (o token pode ter expirado).   <br/><br/>Verifique a funcionalidade na URL (com o Fiddler/Postman): [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 403 Proibido                                                | Permissões necessárias ausentes                                 | Verifique as permissões de usuário no recurso acessado.   <br/><br/>Verifique a funcionalidade na URL (com o Fiddler/Postman): [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 400-Solicitação incorreta                                              | Solicitação de Http inválida                                         | Verifique a URL, o verbo e o corpo da solicitação.   <br/><br/>Use o Fiddler/Postman para validar a solicitação: [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 404 não encontrado                                                | Recurso não foi encontrado                                       | Use o Fiddler/Postman para validar a solicitação: [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Serviço indisponível                                          | O serviço está indisponível                                       | Use o Fiddler/Postman para validar a solicitação: [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Tipo de mídia sem suporte                                       | Tipo de conteúdo incompatível com o corpo de atividade da Web           | Especifique o Content-Type correto que corresponde ao formato de carga usar o Fiddler/Postman para validar a solicitação: [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | O recurso que você estiver procurando para tiver sido removido, teve seu nome alterado ou está temporariamente indisponível. | O recurso não está disponível                                | Use o Fiddler/Postman para verificar o ponto de extremidade: [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | A página que você está procurando não pode ser exibida porque está sendo usado um método inválido (verbo HTTP). | Método de atividade da Web incorreto foi especificado na solicitação   | Use o Fiddler/Postman para verificar o ponto de extremidade: [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | invalid_payload                                              | O corpo para a atividade da Web está incorreto                       | Use o Fiddler/Postman para verificar o ponto de extremidade: [Como usar o Fiddler para criar uma sessão HTTP](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |

#### <a name="how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application"></a>Como usar o Fiddler para criar uma sessão HTTP do aplicativo da web monitorados

1. Baixe e instale o [Fiddler](https://www.telerik.com/download/fiddler)

2. Se seu aplicativo web usa HTTPS:

   1. Abra o Fiddler

   2. Vá para **Ferramentas > Opções do Fiddler** e selecione como na captura de tela abaixo. 

      ![Opções do Fiddler](media/data-factory-troubleshoot-guide/fiddler-options.png)

3. Se seu aplicativo usa certificados SSL, você também deve adicionar o certificado do Fiddler para seu dispositivo.

4. Para adicionar o certificado do Fiddler para seu dispositivo, vá para **ferramentas** > **opções do Fiddler** > **HTTPS**  >   **Ações** > **exportar o certificado de raiz para a área de trabalho** para obter o certificado do Fiddler.

5. Desative a captura, de modo que pode ser limpa o cache do navegador para iniciar um novo rastreamento.

6. 1. Vá para **arquivo** > **capturar tráfego** ou pressione **F12**.
   2. Limpe o cache do navegador para que todos os itens armazenados em cache são removidos e devem ser baixados novamente.

7. Crie solicitação: 

8. 1. Clique na guia Composer
   2. Definir o método Http e a URL
   3. Adicionar cabeçalhos e corpo da solicitação, se necessário
   4. Clique em executar

9. Iniciar a captura de tráfego novamente e concluir a transação problemática em sua página.

10. Depois de concluído, vá para **arquivo** > **salvar** > **todas as sessões**.

Saiba mais sobre o Fiddler [aqui](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)

## <a name="next-steps"></a>Próximas etapas

Para obter mais ajuda na busca de soluções para o problema, a seguir são apresentados alguns outros recursos que você pode experimentar.

*  [Blogs](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitações de recursos](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Fórum do MSDN](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter](https://twitter.com/hashtag/DataFactory)




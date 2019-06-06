---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 5/30/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: ac0747dcff4e74363a58dc9aaf6da4dbd4c6a1c7
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427590"
---
O Azure Data Factory é um serviço multilocatário que tem os seguintes limites padrão em vigor para certificar-se de assinaturas de clientes são protegidas contra umas das outras cargas de trabalho. Para aumentar os limites até o máximo de sua assinatura, contate o suporte.

### <a name="version-2"></a>Versão 2

| Resource | Limite padrão | Limite máximo |
| -------- | ------------- | ------------- |
| Data factories em uma assinatura do Azure | 50 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Número total de entidades, como pipelines, conjuntos de dados, gatilhos, serviços vinculados e tempos de execução de integração, em um data factory | 5.000 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Total de núcleos de CPU para tempos de execução de integração do Azure-SSIS em uma assinatura | 256 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Execuções de pipeline simultâneas por data factory que é compartilhado entre todos os pipelines na fábrica | 10.000  | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| As execuções de atividade de externa simultânea por assinatura por [região de tempo de execução de integração do Azure](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)<br><small>Atividades externas são gerenciadas no tempo de execução de integração, mas executados em serviços vinculados, incluindo Databricks, procedimento armazenado, HDInsights e outras pessoas.</small> | 3000 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| As execuções de atividade de Pipeline simultânea por assinatura por [região de tempo de execução de integração do Azure](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) <br><small>Atividades do pipeline execute no tempo de execução de integração, incluindo Lookup, GetMetadata e excluam. </small>| 1000 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Simultâneas de criação de operações por assinatura por [região de tempo de execução de integração do Azure](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)<br><small>Incluindo a conexão de teste, lista de pastas de navegação e lista de tabelas, visualize dados. | 200 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Unidades de integração de dados simultâneos<sup>1</sup> consumo por assinatura por [região de tempo de execução de integração do Azure](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)| Grupo de região 1<sup>2</sup>: 6000<br>Grupo de região 2<sup>2</sup>: 3000<br>Grupo de região 3<sup>2</sup>: 1500 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Máximo atividades por pipeline, que inclui atividades internas para contêineres | 40 | 40 |
| Número máximo de tempos de execução de integração vinculada que podem ser criados em um único integration runtime auto-hospedado | 100 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Máximo de parâmetros por pipeline | 50 | 50 |
| Itens ForEach | 100.000 | 100.000 |
| Para cada paralelismo | 20 | 50 |
| Caracteres por expressão | 8.192 | 8.192 |
| Intervalo do gatilho de janela em cascata mínimo | 15 min | 15 min |
| Tempo limite máximo para a atividade de pipeline é executado | 7 dias | 7 dias |
| Bytes por objeto para objetos do pipeline<sup>3</sup> | 200 KB | 200 KB |
| Bytes por objeto de conjunto de dados e objetos de serviço vinculado<sup>3</sup> | 100 KB | 2.000 KB |
| Unidades de dados de integração<sup>1</sup> por execução de atividade de cópia | 256 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Gravar chamadas à API | 2.500/h<br/><br/> Esse limite é imposto pelo Azure Resource Manager, não pelo Azure Data Factory. | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Ler chamadas à API | 12,500/h<br/><br/> Esse limite é imposto pelo Azure Resource Manager, não pelo Azure Data Factory. | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Consultas de monitoramento por minuto | 1.000 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Operações CRUD de entidade por minuto | 50 | [Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |

<sup>1</sup> a unidade de integração de dados (DIU) é usada em uma operação de cópia de nuvem para a nuvem, saiba mais sobre [unidades de dados de integração (versão 2)](../articles/data-factory/copy-activity-performance.md#data-integration-units). Para obter informações sobre a cobrança, consulte [preços do Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/).

<sup>2</sup> [Integration Runtime do azure](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) é [disponível globalmente](https://azure.microsoft.com/global-infrastructure/services/) para garantir a conformidade de dados, eficiência e redução de rede os custos de egresso. 

| Grupo de região | Regiões | 
| -------- | ------ |
| Grupo de região 1 | Centro dos EUA, Leste dos EUA, Leste dos EUA 2, Europa Setentrional, Europa Ocidental, oeste dos EUA, oeste dos EUA 2 |
| Grupo de região 2 | Leste da Austrália, Sudeste da Austrália, Sul do Brasil, Índia Central, Leste do Japão, Northcentral dos EUA, Centro-Sul dos EUA, Sudeste Asiático, Centro-Oeste dos EUA |
| Grupo de regiões de 3 | Centro do Canadá, Leste da Ásia, França Central, Coreia Central, Sul do Reino Unido |

<sup>3</sup> pipeline, conjunto de dados e objetos de serviço vinculado representam um agrupamento lógico de sua carga de trabalho. Os limites desses objetos não estão relacionadas à quantidade de dados que você pode mover e processar com o Azure Data Factory. Data Factory é projetado para dimensionar para lidar com petabytes de dados.

### <a name="version-1"></a>Versão 1

| **Recurso** | **Limite padrão** | **Limite máximo** |
| --- | --- | --- |
| Data factories em uma assinatura do Azure |50 |[Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Pipelines em um data factory |2.500 |[Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Conjuntos de dados em um data factory |5.000 |[Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Fatias simultâneas por conjunto de dados |10 |10 |
| Bytes por objeto para objetos do pipeline<sup>1</sup> |200 KB |200 KB |
| Bytes por objeto para dados de definir e objetos de serviço vinculados<sup>1</sup> |100 KB |2.000 KB |
| Núcleos de cluster sob demanda do HDInsight do Azure dentro de uma assinatura<sup>2</sup> |60 |[Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Unidades de movimentação de dados por execução de atividade de cópia de nuvem<sup>3</sup> |32 |[Contate o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Contagem novas tentativas de execução de atividade do pipeline |1.000 |MaxInt (32 bits) |

<sup>1</sup> pipeline, conjunto de dados e objetos de serviço vinculado representam um agrupamento lógico de sua carga de trabalho. Os limites desses objetos não estão relacionadas à quantidade de dados que você pode mover e processar com o Azure Data Factory. Data Factory é projetado para dimensionar para lidar com petabytes de dados.

<sup>2</sup> Os núcleos sob demanda HDInsight são alocados da assinatura que contém o data factory. Como resultado, o limite anterior é o limite de núcleo imposto pelo Data Factory para núcleos do HDInsight sob demanda. Ele é diferente do limite de núcleo que está associada com sua assinatura do Azure.

<sup>3</sup> a unidade de movimentação de dados de nuvem (DMU) para a versão 1 é usado em uma operação de cópia de nuvem para nuvem, saiba mais sobre [unidades de movimentação de dados (versão 1) na nuvem](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units). Para obter informações sobre a cobrança, consulte [preços do Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/).

| **Recurso** | **Limite inferior padrão** | **Limite mínimo** |
| --- | --- | --- |
| Intervalo de agendamento |15 minutos |15 minutos |
| Intervalo entre novas tentativas |1 segundo |1 segundo |
| Valor de tempo limite de nova tentativa |1 segundo |1 segundo |

#### <a name="web-service-call-limits"></a>Limites de chamada de serviço Web
O Azure Resource Manager tem limites para chamadas à API. Você pode fazer chamadas de API em um intervalo dentro dos [limites de API do Gerenciador de Recursos do Azure](../articles/azure-subscription-service-limits.md#resource-group-limits).

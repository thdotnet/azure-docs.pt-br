---
title: Para serviços de dados na Central de segurança do Azure a detecção de ameaças | Microsoft Docs
description: Este tópico apresenta os alertas de serviços de dados disponíveis na Central de segurança do Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 87cfd2769e473d26c2dcae1b7b418f6fb1739915
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626281"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Detecção de ameaças para serviços de dados na Central de segurança do Azure

 A Central de segurança analisa os logs dos serviços de armazenamento de dados e dispara alertas quando detecta uma ameaça para os recursos de dados. Este tópico lista os alertas que a Central de segurança gera para os seguintes serviços:

* [Banco de dados SQL do Azure e SQL Data Warehouse](#data-sql)
* [Armazenamento do Azure](#azure-storage)

## Banco de dados SQL do Azure e SQL Data Warehouse <a name="data-sql"></a>

Ameaças SQL detecção detecta atividades anormais indicando incomuns e potencialmente prejudiciais tenta acessar ou explorar bancos de dados. A Central de segurança analisa os logs de auditoria do SQL e é executado nativamente no mecanismo do SQL.

|Alerta|DESCRIÇÃO|
|---|---|
|**Vulnerabilidade à injeção de SQL**|Um aplicativo gerou uma instrução SQL com erro no banco de dados. Isso pode indicar uma possível vulnerabilidade a ataques de injeção de SQL. Há dois motivos possíveis para a geração de uma instrução com erro: Um defeito no código do aplicativo construído a instrução SQL com erro. Ou então, o código do aplicativo ou procedimentos armazenados não limpar a entrada do usuário ao construir a instrução SQL com erro, que pode ser explorada para injeção de SQL.|
|**Possível injeção de SQL**|Ocorreu uma exploração ativa em relação a um aplicativo identificado vulnerável a injeção de SQL. Isso significa que um invasor está tentando injetar instruções SQL mal-intencionadas utilizando o código do aplicativo vulnerável ou procedimentos armazenados.|
|**Acesso a partir de um local incomum**|Houve uma alteração no padrão de acesso ao SQL server, onde alguém fez logon para o SQL server de um local geográfico incomum. Em alguns casos, o alerta detecta uma ação legítima (um novo aplicativo ou manutenção do desenvolvedor). Em outros casos, o alerta detecta uma ação mal-intencionada (funcionário antigo, invasor externo).|
|**Acesso de entidade desconhecida**|Houve uma alteração no padrão de acesso para o SQL server - alguém fez logon no SQL server usando uma entidade de segurança incomum (usuário SQL). Em alguns casos, o alerta detecta uma ação legítima (novo aplicativo ou manutenção do desenvolvedor). Em outros casos, o alerta detecta uma ação mal-intencionada (funcionário antigo, invasor externo).|
|**Acesso a partir de um aplicativo potencialmente prejudicial**|Um aplicativo potencialmente prejudicial foi usado para acessar o banco de dados. Em alguns casos, o alerta detecta um teste de segurança que está sendo executado. Em outros casos, o alerta detecta um ataque usando ferramentas comuns de ataque.|
|**Credenciais SQL de força bruta**|Ocorreu um número alto anormal de logons com falha com credenciais diferentes. Em alguns casos, o alerta detecta um teste de segurança que está sendo executado. Em outros casos, o alerta detecta ataques de força bruta.|

Para obter mais informações sobre consulte de alertas de detecção de ameaças SQL,[detecção de ameaças do banco de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)e examine a seção de alertas de detecção de ameaças. Consulte também [como a Central de segurança do Azure ajuda a revelar um ataque cibernético](https://azure.microsoft.com/blog/how-azure-security-center-helps-reveal-a-cyberattack/) para exibir um exemplo de como a Central de segurança usado mal-intencionado detecção de atividade do SQL para descobrir um ataque.

## Armazenamento do Azure<a name="azure-storage"></a>

>[!NOTE]
> Proteção avançada contra ameaças do armazenamento do Azure só está disponível atualmente para o armazenamento de BLOBs. 

A Proteção Avançada contra Ameaças do Armazenamento do Azure oferece uma camada adicional de inteligência de segurança que detecta tentativas incomuns e potencialmente prejudiciais de acessar ou explorar contas de armazenamento. Essa camada de proteção permite que você tratar as ameaças sem a necessidade de você ser um especialista em segurança e gerenciar sistemas de monitoramento de segurança.

Central de segurança analisa os logs de diagnóstico de leitura, gravação e solicitações de exclusão para o armazenamento de BLOBs para detectar ameaças e ele dispara alertas quando ocorrem de anomalias na atividade. Para obter mais informações, consulte [configurar o log de análise de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging) para obter mais informações.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIÇÃO|
|---|---|
|**Anomalias de acesso de um local incomum**|Análise de tráfego de amostra de rede detectou saída anormal comunicação de protocolo de área de trabalho remota (RDP) provenientes de um recurso em sua implantação. Esta atividade é considerada anormal para esse ambiente e pode indicar se o recurso foi comprometido e agora é usado para o ponto de extremidade do RDP externo força bruta. Observe que esse tipo de atividade poderia possivelmente fazer com que seu IP fosse sinalizado como mal-intencionado por entidades externas.|
|**Anomalias de acesso do aplicativo**|Indica que um aplicativo incomum tenha acessado esta conta de armazenamento. Uma possível causa é que um invasor tenha acessado sua conta de armazenamento usando um novo aplicativo.|
|**Anomalias de acesso anônimo**|Indica que há uma alteração no padrão de acesso para uma conta de armazenamento. Por exemplo, a conta foi acessada anonimamente (sem qualquer autenticação), que é inesperado em comparação com o padrão de acesso recente nessa conta. Uma possível causa é que um invasor explorar com acesso de leitura público para um contêiner que contém blobs de armazenamento.|
|**Anomalias de vazamento de dados**|Indica que uma quantidade excepcionalmente grande de dados foram extraída em comparação com a atividade recente neste contêiner de armazenamento. Uma possível causa é que um invasor tem extraído uma grande quantidade de dados de um contêiner que contém blobs de armazenamento.|
|**Anomalias de exclusão inesperado**|Indica que uma ou mais operações de exclusão inesperado ocorreu em uma conta de armazenamento, em comparação com a atividade recente nessa conta. Uma possível causa é que um invasor tenha excluído dados da sua conta de armazenamento.|
|**Carregar pacote de serviço de nuvem do Azure**|Indica que um pacote de serviço de nuvem do Azure (arquivo. cspkg) foi carregado para uma conta de armazenamento de maneira incomum, em comparação comparada a atividade recente nessa conta. Uma possível causa é que um invasor tem sido se preparar para implantar um código mal-intencionado da sua conta de armazenamento para um serviço de nuvem do Azure.|
|**Anomalias de acesso de permissão**|Indica que as permissões de acesso desse contêiner de armazenamento tem sido alteradas de maneira incomum. Possível causa é que um invasor mudou permissões de contêiner para diminuir o nível de sua postura de segurança ou para obter persistência.|
|**Anomalias de acesso de inspeção**|Indica que as permissões de acesso de uma conta de armazenamento tem sido inspecionadas de maneira incomum, em comparação comparada a atividade recente nessa conta. Uma possível causa é que um invasor tenha executado o reconhecimento de um ataque futuro.|
|**Anomalias de exploração de dados**|Indica que blobs ou contêineres em uma conta de armazenamento foram enumerados de forma anormal, em comparação comparada a atividade recente nessa conta. Uma possível causa é que um invasor tenha executado o reconhecimento de um ataque futuro.|

>[!NOTE]
>Proteção avançada contra ameaças do armazenamento do Azure atualmente não está disponível nas regiões de nuvem soberana e o Azure governamental.

Para obter mais informações sobre os alertas para o armazenamento, consulte o [Advanced Threat Protection do armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection) do artigo e, em seguida, examine a seção de alertas de proteção.

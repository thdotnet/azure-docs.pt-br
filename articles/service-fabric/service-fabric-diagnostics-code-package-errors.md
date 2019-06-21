---
title: O Azure Service Fabric diagnosticar erros de código de pacote | Microsoft Docs
description: Saiba como solucionar erros comuns de pacote de código com o Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: grzuber
manager: gkhanna
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/09/2019
ms.author: grzuber
ms.openlocfilehash: 235952388d2c044cc141b3020c67944c4250ea3d
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276945"
---
# <a name="diagnose-common-code-package-errors-with-service-fabric"></a>Diagnosticar erros comuns de pacote de código com o Service Fabric

Este artigo ilustra o que significa que um pacote de código encerrar inesperadamente e fornece informações sobre possíveis causas de códigos de erro comuns, bem como etapas de solução de problemas que potencialmente podem atenuar o problema.

## <a name="when-does-a-process-or-container-terminate-unexpectedly"></a>Quando um processo ou contêiner encerrar inesperadamente?

Quando o Service Fabric recebe uma solicitação para iniciar um pacote de códigos, ele começa a preparar o ambiente no sistema local com base nas opções de configuração definidas no aplicativo e manifestos de serviço. Esses preparativos podem incluir reservar recursos ou pontos de extremidade de rede, configurar regras de firewall ou configurando restrições de governança de recursos. Depois que o ambiente foi configurado corretamente, o Service Fabric tenta abrir o pacote de códigos. Essa etapa é considerada bem-sucedida se o tempo de execução do sistema operacional ou um contêiner relata que o processo ou contêiner tiver sido ativado com êxito. Se a ativação não foi bem-sucedida, você deverá ver uma mensagem de integridade no SFX que diz

```
There was an error during CodePackage activation. Service host failed to activate. Error: 0xXXXXXXXX
```

Depois que o pacote de códigos com êxito acionamento, Service Fabric inicia o monitoramento de seu tempo de vida. Neste ponto, um processo ou contêiner poderá encerrar a qualquer momento por vários motivos. Ele pode ter Falha ao inicializar uma DLL, o sistema operacional pode ter ficado sem espaço de heap da área de trabalho, etc. Se seu pacote de códigos é finalizada, você deverá ver uma mensagem de integridade no SFX que diz

```
The process/container terminated with exit code: XXXXXXXX. Please look at your application logs/dump or debug your code package for more details. For information about common termination errors, please visit https://aka.ms/service-fabric-termination-errors
```

O código de saída fornecido na integridade desse mensagem é a única pista da fornecida pelo processo/contêiner sobre por que ele foi finalizado, e ele poderia ter sido gerado por qualquer nível da pilha. É difícil de entender se esse código de saída estava relacionado, por exemplo, a um erro de sistema operacional, um problema de .NET, ou que foi gerado pelo seu código. Como resultado, este artigo pode ser usado como um ponto de partida para diagnosticar a origem dos códigos de saída de encerramento e possíveis soluções, tendo em mente que essas são soluções gerais para cenários comuns e podem não se aplicar ao erro você está vendo.

## <a name="how-can-i-tell-if-service-fabric-terminated-my-code-package"></a>Como saber se o Service Fabric terminar meu pacote de código?

O Service Fabric pode ser responsável por encerrando seu pacote de código para uma variedade de motivos. Por exemplo, pode decidir colocar o pacote de código em outro nó de balanceamento de carga de finalidades. Você pode informar se o seu pacote de código foi encerrada pelo Service Fabric se você vir qualquer um dos códigos de saída na tabela a seguir:

>[!NOTE]
> Se o seu processo/contêiner é encerrado com um código de saída que não sejam aqueles na tabela a seguir, o Service Fabric não é responsável por encerrá-lo.

Código de Saída | DESCRIÇÃO
--------- | -----------
7147 | Esses códigos de erro indicam que o Service Fabric desligar normalmente o processo/contêiner, enviando-a como um sinal Ctrl + C.
7148 | Esses códigos de erro indicam que o Service Fabric finalizado o processo/contêiner. Às vezes, esse código de erro pode indicar que o processo/contêiner não respondeu de maneira oportuna depois de enviar um sinal Ctrl + C, então ele precisava ser eliminado.


## <a name="other-common-error-codes-and-their-potential-fixes"></a>Outros códigos de erro comuns e suas possíveis correções

Código de Saída | Valor hex. | Descrição breve | Causa raiz | Correção de possíveis
--------- | --------- | ----------------- | ---------- | -------------
3221225794 | 0xc0000142 | STATUS_DLL_INIT_FAILED | Esse erro pode significar que o computador ficar sem espaço de heap da área de trabalho. Essa causa é especialmente provável se você tiver um grande número de processos pertencentes ao seu aplicativo em execução no nó. | Se seu programa não foi criado para responder aos sinais de Ctrl + C, você pode habilitar a configuração de "EnableActivateNoWindow" no manifesto do Cluster. Habilitar essa configuração significa que seu pacote de código seria executado sem uma janela de GUI e não receberia sinais de Ctrl + C, mas seria reduzir a quantidade de espaço de heap da área de trabalho de que cada processo consome. Se o seu pacote de código precisa receber os sinais de Ctrl + C, você pode aumentar o tamanho do heap de área de trabalho do seu nó.
3762504530 | 0xe0434352 | N/D | Esse valor é o código de erro para uma exceção sem tratamento no código gerenciado (ou seja, .NET). | Se você estiver vendo esse código de saída, isso implica que seu aplicativo gerou uma exceção que permanece sem tratamento e terminar o processo. Depuração de logs e despejos de memória de seu aplicativo deve ser a primeira etapa para determinar o que causou o erro.

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre [diagnosticar outros cenários comuns](service-fabric-diagnostics-common-scenarios.md)
* Obtenha uma visão mais detalhada do logs do Azure Monitor e o que ele oferece lendo [What ' s logs do Azure Monitor?](../operations-management-suite/operations-management-suite-overview.md)
* Saiba mais sobre os logs do Azure Monitor [alertas](../log-analytics/log-analytics-alerts.md) para auxiliar na detecção e diagnóstico.
* Familiarize-se com o [pesquisa e consulta de log](../log-analytics/log-analytics-log-searches.md) recursos oferecidos como parte dos logs do Azure Monitor

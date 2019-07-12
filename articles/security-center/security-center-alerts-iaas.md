---
title: Ameaças de detecção para VMs e servidores na Central de segurança do Azure | Microsoft Docs
description: Este tópico apresenta as VM e o servidor alertas disponíveis na Central de segurança do Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: dd2eb069-4c76-4154-96bb-6e6ae553ef46
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 5487b4f49f5dbf7b968cd45d40555c69b54c329a
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571576"
---
# <a name="threat-detection-for-vms--servers-in-azure-security-center"></a>Detecção de ameaças para VMs e servidores na Central de segurança do Azure

Este tópico apresenta os tipos diferentes de métodos de detecção e alertas disponíveis para VMs e servidores com os seguintes sistemas de operação. Para obter uma lista das versões com suporte, consulte [recursos com suporte pela Central de segurança do Azure e plataformas](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Windows <a name="windows-machines"></a>

A Central de segurança se integra com os serviços do Azure para monitorar e proteger seus computadores baseados em Windows.  A Central de segurança apresenta os alertas e sugestões de correção de todos esses serviços em um formato fácil de usar.

### Microsoft Server Defender ATP <a nanme="windows-atp"></a>

A Central de segurança do Azure estende suas plataformas de proteção de carga de trabalho de nuvem, a integração com o Windows Defender Advanced Threat ATP (proteção). Isso fornece recursos abrangentes de ponto de extremidade de detecção e resposta (EDR).

> [!NOTE]
> Sensor de servidor do Windows Defender ATP é habilitado automaticamente em servidores Windows que são integrados à Central de segurança do Azure.

Quando o servidor do Windows Defender ATP detecta uma ameaça, ele dispara um alerta. O alerta é mostrada no painel central de segurança. No painel, você pode dinamizar no console do Windows Defender ATP para realizar uma investigação detalhada para revelar o escopo da invasão. Para obter mais informações sobre o servidor do Windows Defender ATP, consulte [servidores integrados ao serviço do Windows Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints).

### Análise de despejo de falha <a nanme="windows-dump"></a>

Quando o software falha, um despejo de memória captura uma parte da memória no momento da falha.

Uma falha pode ter sido causada por malware ou conter malware. Para evitar serem detectadas por produtos de segurança, várias formas de malware usam um ataque fileless, o que evita a gravação em disco ou criptografando componentes de software gravados no disco. Esse tipo de ataque é difícil de detectar usando as abordagens tradicionais baseadas em disco.

No entanto, esse tipo de ataque pode ser detectado usando a análise de memória. Analisando a memória no despejo de falha, a Central de segurança pode detectar as técnicas de que ataque está usando para explorar vulnerabilidades no software, acessar os dados confidenciais e furtiva em uma máquina comprometida. Isso é feito pelo back-end a Central de segurança com o impacto de desempenho mínimo para os hosts.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIÇÃO|
|---|---|
|**Injeção de código descoberta**|A injeção de código é a inserção de módulos executáveis em processos ou threads em execução. Essa técnica é usada por malware para acessar dados, embora com êxito, ocultando em si para evitar que está sendo encontrado e removido. <br/>Esse alerta indica que um módulo injetado está presente no despejo de memória. Para diferenciar módulos injetados mal-intencionados e não são mal-intencionados, a Central de segurança verifica se o módulo injetado se encaixa em um perfil de comportamento suspeito.|
|**Segmento de código suspeito detectado**|Indica que um segmento de código foi alocado usando métodos não padrão, como injeção refletiva e pelo esvaziamento de processo. O alerta fornece características adicionais do segmento de código que foram processadas para fornecer contexto para os recursos e comportamentos do segmento de código relatado.|
|**ShellCode descoberto**|Shellcode é a carga executada depois que o malware explorou uma vulnerabilidade do software.<br/>Esse alerta indica que a análise de despejo de memória detectou código executável com um comportamento normalmente executado por cargas mal-intencionadas. Embora o software não são mal-intencionados também pode executar esse comportamento, não é típico de práticas de desenvolvimento de software normal.|

### Detecção de ataque fileless <a nanme="windows-fileless"></a>

No Azure, podemos ver regularmente fileless ataques direcionados a pontos de extremidade de nossos clientes.

Para evitar a detecção de ataques fileless injetar cargas mal-intencionadas na memória. Cargas de invasor persistirem em memória de processos comprometidos e realizar uma ampla gama de atividades mal-intencionadas.

Com a detecção de ataque fileless, técnicas forenses de memória automatizado identificam comportamentos, técnicas e kits de ferramentas de ataque fileless. Essa solução periodicamente verifica sua máquina em tempo de execução e extrai insights diretamente da memória de processos de segurança crítica.

Ele localiza a evidência de exploração, injeção de código e execução de cargas mal-intencionadas. Detecção de ataque fileless gera alertas de segurança detalhada para acelerar o processo de triagem de alertas, correlação e tempo de resposta de downstream. Essa abordagem complementa soluções EDR baseado em evento, fornecendo maior cobertura de detecção.

> [!NOTE]
> Você pode simular os alertas do Windows por download [guia estratégico de Central de segurança do Azure](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef): Alertas de segurança e siga as orientações fornecidas

> [!div class="mx-tableFixed"]

|Alerta|DESCRIÇÃO|
|---|---|
|**Técnica de ataque fileless detectada**|A memória do processo especificado abaixo contém um kit de ferramentas de ataque fileless: Meterpreter. Kits de ferramentas de ataque fileless normalmente não tem uma presença no sistema de arquivos, dificultando a detecção por antivírus tradicional.|

### <a name="further-reading"></a>Leitura adicional

Para obter exemplos e obter mais informações sobre a detecção da Central de segurança:

* [Como a Central de segurança do Azure automatiza a detecção de ataque cibernético](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)
* [Como a Central de segurança do Azure detecta vulnerabilidades usando as ferramentas administrativas](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

## Linux <a name="linux-machines"></a>

Registros de computadores Linux usando de auditoria de coleta central de segurança **auditd**, uma auditoria de estruturas do Linux mais comuns. auditd tem a vantagem de ter por aí há muito tempo e mora no kernel do principal. 

### Alertas de auditd do Linux e integração do agente MMA (Microsoft Monitoring) <a name="linux-auditd"></a>

O sistema de auditd consiste em um subsistema de nível de kernel, que é responsável por chamadas do sistema de monitoramento, filtre-os por um determinado conjunto de regras e gravação de mensagens para eles em um soquete. A Central de segurança se integra as funcionalidades do pacote auditd dentro do agente de MMA (Microsoft Monitoring). Essa integração permite que a coleção de eventos de auditd em todas as distribuições do Linux com suporte sem todos os pré-requisitos.  

os registros de auditoria são coletados, aprimorados e agregados em eventos usando o agente MMA de Linux. A Central de segurança constantemente está trabalhando na adição de nova análise, que aproveite Linux sinaliza ao detectar comportamentos mal-intencionados em nuvem e máquinas Linux local. Semelhante aos recursos do Windows, essas análises abrangem processos suspeitos, tentativas duvidosas de logon, carregamento de módulo de kernel e outras atividades, que indicam uma máquina está sob ataque ou foi violada.  

Abaixo estão vários exemplos de análise, que demonstram como podemos abranger em diferentes estágios do ciclo de vida de ataque.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIÇÃO|
|---|---|
|**Processo visto acessando o arquivo de chaves autorizadas do SSH de maneira incomum**|Um arquivo de chaves autorizadas do SSH foi acessado em um método semelhante para as campanhas de malware conhecido. Esse acesso pode indicar que um invasor está tentando obter acesso persistente a um computador|
|**Tentativa de persistência detectado**|Análise de dados de host detectou que um script de inicialização para o modo de usuário único foi instalado. <br/>Uma vez que é raro que qualquer processo legítimo seria necessário para executar nesse modo, isso pode indicar que um invasor tenha adicionado um processo mal-intencionado para cada nível de execução para garantir a persistência.|
|**Manipulação de tarefas agendadas detectado**|Análise de dados de host detectou possível manipulação das tarefas agendadas. Os invasores normalmente adicionam tarefas agendadas para as máquinas que eles já comprometido para obter persistência.|
|**Modificação de carimbo de hora de arquivo suspeito**|Análise de dados de host detectou uma modificação de carimbo de hora suspeitos. Os invasores geralmente copiem carimbos de hora dos arquivos legítimos existentes para novas ferramentas para evitar a detecção desses arquivos recentemente removido|
|**Um novo usuário foi adicionado ao grupo sudoers**|Análise de dados de host detectou que um usuário foi adicionado ao grupo sudoers, que permite que seus membros executem comandos com privilégios altos.|
|**Probabilidade de exploração de DynoRoot vulnerabilidade no cliente dhcp**|Análise de dados de host detectou a execução de um comando incomum com o processo pai do script dhclient.|
|**Módulo de kernel suspeito detectado**|Análise de dados de host detectou um arquivo de objeto compartilhado que está sendo carregado como um módulo de kernel. Isso pode ser atividade legítima, ou uma indicação de que uma das suas máquinas tiver sido comprometida.|
|**Processo associado à mineração de moeda digital detectada**|Análise de dados de host detectou a execução de um processo normalmente associada à mineração de moeda digital|
|**Encaminhamento de porta potencial para endereço IP externo**|Análise de dados de host detectou a inicialização de porta que encaminha para um endereço IP externo.|

> [!NOTE]
> Você pode simular os alertas do Windows, baixando [guia estratégico de Central de segurança do Azure: Alertas de segurança](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef) e siga as orientações fornecidas.


Para obter mais informações, consulte estes artigos:  

* [Central de segurança do Azure aproveite para detectar quando o ataque de máquinas Linux comprometido](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)

* [A Central de segurança do Azure pode detectar vulnerabilidades emergentes no Linux](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

 
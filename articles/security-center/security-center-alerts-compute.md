---
title: Para a computação nativa de nuvem na Central de segurança do Azure a detecção de ameaças | Microsoft Docs
description: Este tópico apresenta a nuvem de computação nativa de alertas disponíveis na Central de segurança do Azure. na Central de segurança do Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: bc3cb66d43e71777e06c6bd63dcff35e2ff19df8
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571680"
---
# <a name="threat-detection-for-cloud-native-compute-in-azure-security-center"></a>Detecção de ameaças para computação nativa na Central de segurança do Azure na nuvem

Como um provedor de nuvem, a Central de segurança aproveita a visibilidade exclusiva, que ele precisa analisar os logs internos e identificar a metodologia de ataque em vários destinos. Este tópico apresenta os alertas disponíveis para os seguintes serviços do Azure:

* [Serviço de Aplicativo do Azure](#app-services)
* [Contêineres](#azure-containers) 

## Serviço de aplicativo do Azure <a name="app-services"></a>

A Central de segurança aproveita a escala da nuvem para identificar ataques direcionados a aplicativos clientes executando o serviço de aplicativo do Azure. Com aplicativos da web que está sendo praticamente em qualquer rede moderna, os invasores de investigação para localizá-los e explorar pontos fracos. Antes de ser roteado para ambientes específicos, as solicitações para aplicativos em execução no Azure passam por vários gateways de onde eles são inspecionados e conectados. Esses dados, em seguida, são usados para identificar explorações, os invasores e para aprender novos padrões serão usados posteriormente.

Aproveitando a visibilidade que o Azure tem como um provedor de nuvem, a Central de segurança analisa logs de serviço de aplicativo internos para identificar a metodologia de ataque em vários destinos. Por exemplo, a varredura generalizada e ataques distribuídos. Esse tipo de ataque geralmente vem de um pequeno subconjunto de IPs e apresenta padrões de rastreamento para pontos de extremidade semelhantes em vários hosts, procurando por uma página vulnerável ou o plug-in. Isso pode ser detectado usando a nuvem, mas não pode ser identificado no ponto de vista de um único host.

A Central de segurança também tem acesso a áreas restritas e VMs subjacentes. Junto com a análise forense de memória, a infraestrutura pode contar a história, de um novo ataque de circulação em uso para comprometimentos em computadores cliente. Portanto, a Central de segurança pode detectar ataques contra aplicativos web muito depois sendo explorada.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIÇÃO|
|---|---|
|**Invocação de tema WordPress suspeita detectada**|O log de atividades do serviço de aplicativo do Azure indica uma atividade de injeção de código possível em seu recurso de serviço de aplicativo.<br/> Essa atividade suspeita é semelhante a atividade que manipula um tema de WordPress para dar suporte à execução do lado do servidor de código, seguido por uma solicitação web diretos para invocar o arquivo de tema manipulado. Esse tipo de atividade foi visto no passado como parte de uma campanha de ataque ao longo do WordPress.|
|**Conexão a página da web do endereço IP anormal detectado**|O log de atividades do serviço de aplicativo do Azure indica uma conexão para uma página da web confidenciais de um endereço de origem que nunca se conectou a ele antes. Isso pode indicar que alguém está tentando um ataque de força bruta em suas páginas de administração do aplicativo Web. Ele também pode ser o resultado de um novo endereço IP que está sendo usado por um usuário legítimo.|
|**Um IP conectados à sua Interface de FTP do serviço de aplicativo do Azure foi encontrado na inteligência contra ameaças**|Análise de logs de FTP do serviço de aplicativo do Azure detectou uma conexão de um endereço de origem foi encontrado no feed de inteligência de ameaça. Durante essa conexão, um usuário acessou as páginas listadas abaixo.|
|**Impressão digital de Web detectado**|O log de atividades do serviço de aplicativo do Azure indica um possível web a impressão digital de atividade em seu recurso de serviço de aplicativo. <br/>Essa atividade suspeita detectada é associada uma ferramenta chamada elefante cegos. A ferramenta de impressões digitais de servidores web e tenta detectar os aplicativos instalados e suas versões. Os invasores geralmente usam essa ferramenta para os aplicativos web de investigação para encontrar vulnerabilidades.|
|**Acesso suspeito a possivelmente vulneráveis página da web detectado**|O log de atividades do serviço de aplicativo do Azure indica que uma página da web que parece ser confidenciais foi acessado. <br/>Essa atividade suspeita, originada de um endereço de origem cujo padrão de acesso é semelhante ao de um scanner de web. Esse tipo de atividade geralmente está associado uma tentativa por um invasor para verificar sua rede para tentar obter acesso a páginas da web vulnerável ou confidenciais.|
|**Arquivo PHP na pasta de carregamento**|O log de atividades do serviço de aplicativo do Azure indica que algo acessou a uma página PHP suspeita localizada na pasta de carregamento. <br/>Este tipo de pasta não contém arquivos PHP normalmente. A existência desse tipo de arquivo pode indicar uma exploração tirando proveito de vulnerabilidades de upload de arquivo arbitrário.|
|**Uma tentativa de executar comandos do Linux em um serviço de aplicativo do Windows**|Análise de processos de serviço de aplicativo detectado ao tentar executar um comando do Linux em um serviço de aplicativo do Windows. Essa ação estava em execução pelo aplicativo web. Esse comportamento geralmente é visto durante campanhas que explora uma vulnerabilidade em um aplicativo web comuns.|
|**Execução de PHP suspeita detectada**|Logs de máquina indicarem uma que uma suspeita processo PHP está em execução. A ação incluída uma tentativa de executar comandos de sistema operacional ou código PHP na linha de comando usando o processo PHP. Embora esse comportamento possa ser legítimo, em aplicativos web esse comportamento é também observado em atividades mal-intencionadas, como tentativas de infectar sites com shells de web.|
|**Execução do processo da pasta temporária**|Análise de processos do serviço de aplicativo detectou uma execução de um processo da pasta temporária do aplicativo. Embora esse comportamento possa ser legítimo, em aplicativos da web que esse comportamento também é observado em atividades mal-intencionadas.|
|**Tentativa de executar o comando com privilégios elevados detectado**|Análise de processos de serviço de aplicativo detectou uma tentativa de executar um comando que requer privilégios altos. O comando foi executado no contexto do aplicativo web. Embora esse comportamento possa ser legítimo, em aplicativos da web que esse comportamento também é observado em atividades mal-intencionadas.|

> [!NOTE]
> Detecção de ameaças de Central de segurança para o serviço de aplicativo não está disponível nas regiões de nuvem soberana e o Azure government no momento.

Para obter mais informações sobre detecção de ameaças do serviço de aplicativo alertas visite proteger o serviço de aplicativo com a Central de segurança do Azure e examine como habilitar o monitoramento e a proteção de suas cargas de trabalho do serviço de aplicativo.

## Contêineres <a name="azure-containers"></a>

Central de segurança fornece detecção de ameaças em tempo real para os contêineres em máquinas Linux com base na estrutura auditd. Os alertas identificam várias atividades suspeitas do Docker, como a criação de um contêiner com privilégios em um host, uma indicação de servidor SSH (Secure Shell) em execução dentro de um contêiner do Docker ou o uso dos mineradores de criptografia. É possível usar essas informações para corrigir os problemas rapidamente e aumentar a segurança de seus contêineres. Além de detecções de Linux, Central de segurança também oferece análise, que é mais específica para implantação de contêineres.

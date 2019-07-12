---
title: Detecção de camada de serviço do Azure na Central de segurança do Azure de ameaças | Microsoft Docs
description: Este tópico apresenta os alertas de camada de serviço do Azure disponíveis na Central de segurança do Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 8c1733877834f82d9ee2524cf8bf54f532e7d9c4
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571719"
---
# <a name="threat-detection-for-azure-service-layer-in-azure-security-center"></a>Detecção de ameaças para a camada de serviço do Azure na Central de segurança do Azure

Este tópico apresenta os alertas da Central de segurança disponíveis ao monitorar as seguintes camadas de serviço do Azure.

* [Camada de rede do Azure](#network-layer)
* [Camada de gerenciamento do Azure (Azure Resource Manager) (visualização)](#management-layer)

>[!NOTE]
>Usando a telemetria que aproveita a Central de segurança de tocar em feeds internos do Azure, as análises fornecidas abaixo se aplicam a todos os tipos de recursos.

## Camada de rede do Azure<a name="network-layer"></a>

Análise de camada de rede da Central de segurança é com base em amostra [dados IPFIX](https://en.wikipedia.org/wiki/IP_Flow_Information_Export), que são coletados pelos roteadores de núcleo do Azure de cabeçalhos de pacote. Com base neste feed de dados, modelos de aprendizado de máquina da Central de segurança identificam e sinalizador de atividades de tráfego mal-intencionado. Para enriquecer os endereços IP, a Central de segurança aproveita o banco de dados de inteligência contra ameaças da Microsoft.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIÇÃO|
|---|---|
|**Atividade suspeita de rede RDP saída**|Análise de tráfego de amostra de rede detectou saída anormal comunicação de protocolo de área de trabalho remota (RDP) provenientes de um recurso em sua implantação. Esta atividade é considerada anormal para esse ambiente e pode indicar se o recurso foi comprometido e agora está sendo usada para o ponto de extremidade do RDP externo força bruta. Observe que esse tipo de atividade poderia possivelmente fazer com que seu IP fosse sinalizado como mal-intencionado por entidades externas.|
|**Suspeita de atividade de rede RDP para vários destinos de saída**|Análise de tráfego de amostra de rede detectou saída anormal protocolo de área de trabalho remota (RDP) de comunicação originado de um recurso em sua implantação para vários destinos. Esta atividade é considerada anormal para esse ambiente e pode indicar se o recurso foi comprometido e agora está sendo usado para pontos de extremidade do RDP externos força bruta. Observe que esse tipo de atividade poderia possivelmente fazer com que seu IP fosse sinalizado como mal-intencionado por entidades externas.|
|**Atividade de rede do SSH saída suspeita**|Análise de tráfego de amostra de rede detectou saída anormal Secure Shell (SSH) de comunicação provenientes de um recurso em sua implantação. Esta atividade é considerada anormal para esse ambiente e pode indicar se o recurso foi comprometido e agora está sendo usada para o ponto de extremidade do SSH externo força bruta. Observe que esse tipo de atividade poderia possivelmente fazer com que seu IP fosse sinalizado como mal-intencionado por entidades externas.|
|**Suspeita de atividade de rede do SSH para vários destinos de saída**|Análise de tráfego de amostra de rede detectou saída anormal Secure Shell (SSH) de comunicação originado de um recurso em sua implantação para vários destinos. Esta atividade é considerada anormal para esse ambiente e pode indicar se o recurso foi comprometido e agora está sendo usado para pontos de extremidade do SSH externos força bruta. Observe que esse tipo de atividade poderia possivelmente fazer com que seu IP fosse sinalizado como mal-intencionado por entidades externas.|
|**Atividade suspeita de entrada SSH rede de várias fontes**|Análise de tráfego de amostra de rede detectou anormais SSH comunicações de entrada de várias fontes para um recurso em sua implantação. Diversos IPs exclusivos conectando-se a seus recursos é considerado anormal para esse ambiente. Essa atividade pode indicar uma tentativa de força bruta sua interface SSH em vários hosts (Botnet).|
|**Atividade suspeita de rede do entrada SSH**|Análise de tráfego de amostra de rede detectou entrada anormal de SSH comunicação a um recurso em sua implantação. Um número relativamente alto de conexões de entrada para o recurso é considerado anormal para esse ambiente. Essa atividade pode indicar uma tentativa de força bruta sua interface SSH.
|**Atividade suspeita de entrada RDP rede de várias fontes**|Análise de tráfego de amostra de rede detectou anormais RDP comunicações de entrada de várias fontes para um recurso em sua implantação. Diversos IPs exclusivos conectando-se a seus recursos é considerado anormal para esse ambiente. Essa atividade pode indicar uma tentativa de força bruta sua interface RDP em vários hosts (Botnet).|
|**Atividade suspeita de rede RDP entrada**|Análise de tráfego de amostra de rede detectou entrada anormal RDP de comunicação a um recurso em sua implantação. Um número relativamente alto de conexões de entrada para o recurso é considerado anormal para esse ambiente. Essa atividade pode indicar uma tentativa de força bruta sua interface SSH.|

Para entender como a Central de segurança pode usar a rede relacionados sinais para aplicar a proteção contra ameaças, consulte [detecções DNS heurística na Central de segurança do Azure](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).
## Camada de gerenciamento do Azure (Azure Resource Manager) (visualização)<a name ="management-layer"></a>

>[!NOTE]
>Camada de proteção da Central de segurança com base no Azure Resource Manager está atualmente em visualização.

Central de segurança oferece uma camada adicional de proteção, aproveitando a eventos do Azure Resource Manager, que é considerado ser o plano de controle para o Azure. Ao analisar os registros do Azure Resource Manager, a Central de segurança detecta operações incomuns ou potencialmente prejudiciais no ambiente de assinatura do Azure.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIÇÃO|
|---|---|
|**Kit de ferramentas de microBurst executar**|Foi detectado um kit de ferramentas de reconhecimento de conhecidos do ambiente de nuvem executado no seu ambiente. A ferramenta "MicroBurst" (consulte https://github.com/NetSPI/MicroBurst) pode ser usado por um invasor (ou testador de penetração) para mapear seus recursos de assinatura (s), identificar configurações inseguras e deixar vazar informações confidenciais.|
|**Kit de ferramentas de azurite executar**|Foi detectado um kit de ferramentas de reconhecimento de conhecidos do ambiente de nuvem executado no seu ambiente. A ferramenta "Azurite" (consulte https://github.com/mwrlabs/Azurite) pode ser usado por um invasor (ou testador de penetração) para mapear seus recursos de assinatura (s) e identificar configurações inseguras.|
|**Sessão de gerenciamento suspeitas usando uma conta inativa**|Análise de logs de atividade de assinatura foi detectado um comportamento suspeito. Uma entidade que não estava em uso por um longo período de tempo, agora está executando as ações que podem proteger a persistência para um invasor.|
|**Sessão de gerenciamento de suspeitas usando o PowerShell**|Análise de logs de atividade de assinatura foi detectado um comportamento suspeito. Uma entidade que não usa regularmente o PowerShell para gerenciar o ambiente de assinatura, agora estão usando o PowerShell e executar as ações que podem proteger a persistência para um invasor.|
|**Uso de técnicas avançadas de persistência do Azure**|Análise de logs de atividade de assinatura foi detectado um comportamento suspeito. Funções personalizadas receberam legitimou identidade entidades. Isso pode levar o invasor obter persistência em um ambiente de cliente do Azure.|
|**Atividade de país pouco frequente**|Atividade de um local que não foi recentemente ou nunca visitado por qualquer usuário na organização tenha ocorrido.<br/>Essa detecção considera os locais de atividades anteriores para determinar os locais novos e pouco frequentes. O mecanismo de detecção de anomalias armazena informações sobre locais anteriores usados por usuários na organização. 
|**Atividade de endereços IP anônimos**|Atividade de usuários de um endereço IP que foi identificado como um endereço IP de proxy anônimo foi detectado. <br/>Esses proxies geralmente são usados por usuários que desejam ocultar o endereço IP de seu dispositivo e podem ser usados com objetivos mal-intencionados. Essa detecção usa um algoritmo que reduz "falsos positivos", como endereços IP marcados incorretamente que são amplamente usados pelos usuários na organização de aprendizado de máquina.|
|**Viagem impossível detectada**|Duas atividades do usuário (é uma única ou várias sessões) ter ocorrido, provenientes de locais geograficamente distantes em um período de tempo menor que o tempo ele teria levado para o trajeto do primeiro local para o segundo. Isso indica que um usuário diferente está usando as mesmas credenciais. <br/>Essa detecção usa um algoritmo que ignora "falsos positivos" óbvios que contribuem para condições impossíveis de viagem, como VPNs e locais regularmente usados por outros usuários na organização de aprendizado de máquina. A detecção tem um período inicial de aprendizado de sete dias, durante o qual aprende o padrão de atividade de um novo usuário.|

>[!NOTE]
> Vários do analytics acima são ativados pelo Microsoft Cloud App Security (MCAS). Para aproveitar essas análises, é necessária uma licença MCAS ativada. Se você tiver uma licença do MCAS, esses alertas são habilitados por padrão. Para desabilitá-los:
>
> 1. Na folha Central de Segurança, selecione **Política de segurança**. Para a assinatura que você deseja alterar, clique em **Editar configurações**.
> 2. Clique em **Detecção de ameaças**.
> 3. Emb **Habilitar integrações**, desmarque a opção **Permitir que o Microsoft Cloud App Security acesse meus dados** e clique em **Salvar**.

>[!NOTE]
>A Central de segurança do Azure armazena os dados do cliente relacionadas à segurança na mesma área geográfica que seu recurso. Se a Microsoft ainda não implantou a Central de segurança do Azure na área de geográfica do recurso, em seguida, ele armazena os dados nos Estados Unidos. Quando a segurança de aplicativo de nuvem da Microsoft (MCAS) estiver habilitado, essas informações são armazenadas de acordo com as regras de localização geográfica do MCAS. Ver [armazenamento de dados para os serviços não regional para obter mais informações](http://azuredatacentermap.azurewebsites.net/).

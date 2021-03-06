---
title: Perguntas frequentes sobre o Gateway de Aplicativo do Azure | Microsoft Docs
description: "Esta página fornece respostas às perguntas frequentes sobre o Gateway de Aplicativo do Azure"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d54ee7ec-4d6b-4db7-8a17-6513fda7e392
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: fe38c16f94faa3e7a5a2622ff4eb8a1ae93fba20
ms.openlocfilehash: 1bf1e323798a702029663953d3a30de174aefc4c
ms.lasthandoff: 02/16/2017


---

# <a name="frequently-asked-questions-for-application-gateway"></a>Perguntas frequentes sobre o Gateway de Aplicativo

## <a name="general"></a>Geral

**P. O que é o Gateway de Aplicativo?**

O Gateway de Aplicativo do Azure é um ADC (Controlador de Entrega de Aplicativos) como um serviço, oferecendo vários recursos de balanceamento de carga de camada 7 para seus aplicativos. Ele oferece um serviço altamente disponível e escalonável e totalmente gerenciado pelo Azure.

**P. Quais recursos recebem suporte do Gateway de Aplicativo?**

O Gateway de Aplicativo oferece suporte ao descarregamento de SSL e SSL de ponta a ponta, Firewall de Aplicativo Web (visualização), afinidade de sessão baseada em cookies, roteamento baseado em caminho de url, hospedagem em múltiplos sites e outros. Para conferir uma lista completa dos recursos com suporte, visite [Introdução ao Gateway de Aplicativo](application-gateway-introduction.md)

**P. Qual é a diferença entre o Gateway de Aplicativo e o Azure Load Balancer?**

O Gateway de Aplicativo do Azure é um balanceador de carga de camada 7. Isso significa que o Gateway de Aplicativo lida apenas com o tráfego de Web (HTTP/HTTPS/WebSocket). Ele oferece suporte a recursos de balanceamento de carga do aplicativo como terminação SSL, a afinidade de sessão baseada em cookie e round robin para tráfego de balanceamento de carga. Balanceador de carga, equilibra a carga do tráfego na camada 4 (TCP/UDP).

**P. Quais protocolos recebem suporte do Gateway de Aplicativo?**

O Gateway de Aplicativo oferece suporte a WebSocket, HTTP e HTTPS.

**P. Quais recursos têm suporte atualmente como parte do pool de back-end?**

Pools de back-end podem ser formados por NICs, VMSS (conjuntos de dimensionamento de máquinas virtuais), IPs públicos, IPs internos e FQDN (nomes de domínio totalmente qualificado). O suporte para Aplicativos Web do Azure não está disponível atualmente. Membros do pool de back-end do Gateway de Aplicativo não estão vinculados a um conjunto de disponibilidade. Os membros de pools de back-end podem existir entre clusters, data centers ou fora do Azure, desde que tenham conectividade IP.

**P. Em quais regiões o serviço está disponível?**

O Gateway de Aplicativo está disponível em todas as regiões do Azure público. Ele também está disponível no [Azure China](https://www.azure.cn/) e no [Azure Governamental](https://azure.microsoft.com/en-us/overview/clouds/government/)

**P. Ele é uma implantação dedicada à minha assinatura ou é compartilhado entre os clientes?**

O Gateway de Aplicativo é uma implementação dedicada em sua rede virtual.

**P. O redirecionamento HTTP-> HTTPS tem suporte?**

Não há suporte para esse recurso no momento.

**P. Onde posso encontrar o IP e o DNS do Gateway de Aplicativo?**

Ao usar um endereço IP público como um ponto de extremidade, essas informações podem ser encontradas no recurso do endereço IP público ou na página Visão geral do Gateway de Aplicativo no portal. Para endereços IP internos, isso pode ser encontrado na página Visão geral.

**P. O IP ou DNS muda durante o tempo de vida do Gateway de Aplicativo?**

O VIP pode mudar se o gateway for interrompido e iniciado pelo cliente. O DNS associado ao Gateway de Aplicativo não muda durante o ciclo de vida do gateway. Por esse motivo, recomendamos o uso um alias do CNAME e apontá-lo para o endereço DNS do Gateway de Aplicativo.

**P. O Gateway de Aplicativo oferece suporte a IP estático?**

Não, o Gateway de Aplicativo não oferece suporte a endereços IP públicos estáticos, mas oferece suporte a IPs estáticos internos.

**P. O Gateway de Aplicativo oferece suporte a vários IPs públicos no gateway?**

Há suporte a apenas um endereço IP público em um Gateway de Aplicativo.

**P. O Gateway de Aplicativo oferece suporte a cabeçalhos x-forwarded-for?**

Sim, o Gateway de Aplicativo insere cabeçalhos x-forwarded-for, x-forwarded-proto e x-forwarded-port na solicitação encaminhada ao back-end. O formato do cabeçalho x-forwarded-for é uma lista separada por vírgulas de IP:Porta. Os valores válidos para x-forwarded-proto são http ou https. X-forwarded-port especifica a porta na qual a solicitação alcançou o Gateway de Aplicativo.

## <a name="configuration"></a>Configuração

**P. O Gateway de Aplicativo é sempre implantado em uma rede virtual?**

Sim, o Gateway de Aplicativo é sempre implantado em uma sub-rede de rede virtual. Essa sub-rede só pode conter Gateways de Aplicativos.

**P. O Gateway de Aplicativo pode se comunicar com instâncias fora de sua rede virtual?**

O Gateway de Aplicativo pode se comunicar com instâncias fora da rede virtual na qual ele está localizado contanto que haja conectividade de IP. Se você planeja usar IPs internos como membros de pool de back-end, será necessário usar o [Emparelhamento VNET](../virtual-network/virtual-network-peering-overview.md) ou o [Gateway de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).

**P. Posso implantar qualquer coisa na sub-rede do Gateway de Aplicativo?**

Não, mas você pode implantar outros gateways de aplicativo na sub-rede

**P. Há suporte para Grupos de segurança de rede na sub-rede do Gateway de Aplicativo?**

Os Grupos de segurança de rede têm suporte na sub-rede do Gateway de Aplicativo, mas é necessário aplicar exceções para as portas 65503-65534 para que a integridade de back-end funcione corretamente. A conectividade de internet de saída não deve ser bloqueada.

**P. Quais são os limites no Gateway de Aplicativo? Posso aumentar esses limites?**

Visite [Limites do Gateway de Aplicativo](../azure-subscription-service-limits.md#application-gateway-limits) para exibir os limites.

**P. Posso usar o Gateway de Aplicativo para tráfego interno e externo ao mesmo tempo?**

Sim, o Gateway de Aplicativo oferece suporte a um endereço IP interno e um IP externo por Gateway de Aplicativo.

**P. Há suporte para o emparelhamento VNet?**

Sim, o emparelhamento VNet tem suporte e é útil para o balanceamento de carga de tráfego em outras redes virtuais.

**P. Posso falar com servidores locais se eles estiverem conectados por túneis de VPN ou ExpressRoute?**

Sim, contanto que o tráfego seja permitido.

**P. Posso ter um pool de back-end que atende a muitos aplicativos em portas diferentes?**

Há suporte para arquitetura de microsserviço. Você precisaria de várias configurações http definidas para investigação em portas diferentes.

**P. Investigações personalizadas têm suporte para curingas/regex nos dados de resposta?**

As investigações personalizadas não têm suporte para curingas/regex nos dados de resposta.

**P. O que significa o campo Host para investigações personalizadas?**

O campo Host especifica o nome ao qual enviar a investigação. Aplicável somente quando vários sites são configurados no Application Gateway; do contrário, use '127.0.0.1'. Esse valor é diferente do nome de host da VM e está no formato \<protocolo\>://\<host\>:\<porta\>\<caminho\>. 

## <a name="performance"></a>Desempenho

**P. Como o Gateway de Aplicativo oferece suporte a alta disponibilidade e escalabilidade?**

O Gateway de Aplicativo oferece suporte a cenários de alta disponibilidade se houver mais de duas instâncias implantadas. O Azure distribui essas instâncias entre domínios de atualização e de falha para garantir que todas as instâncias não falham ao mesmo tempo. O Gateway de Aplicativo dá suporte à escalabilidade adicionando várias instâncias do mesmo gateway para compartilhar a carga.

**P. Como posso obter o cenário de recuperação de desastres em data centers com o Gateway de Aplicativo?**

Os clientes podem usar o Gerenciador de Tráfego para distribuir o tráfego entre vários Gateways de Aplicativo em data centers diferentes.

**P. Há suporte para o dimensionamento automático?**

Não, mas o Gateway de Aplicativo tem uma métrica de taxa de transferência que pode ser usada para enviar um alerta se um limite for atingido. A adição manual de instâncias ou alteração do tamanho não reinicia o gateway e não afeta o tráfego existente.

**P. A escala/redução vertical causa tempo de inatividade?**

Não há tempo de inatividade, as instâncias são distribuídas entre domínios de atualização e domínios de falha.

**P. Posso alterar o tamanho da instância de média para grande sem interrupções?**

Sim, o Azure distribui essas instâncias entre domínios de atualização e de falha para garantir que todas as instâncias não falham ao mesmo tempo. O Gateway de Aplicativo dá suporte à escalabilidade adicionando várias instâncias do mesmo gateway para compartilhar a carga.

## <a name="ssl-configuration"></a>Configuração de SSL

**P. Quais certificados têm suporte no Gateway de Aplicativo?**

Certificados autoassinados, certificados de autoridade de certificação e certificados curinga têm suporte. Não há suporte para certificados EV.

**P. Quais são os conjuntos de criptografia atuais com suporte do Gateway de Aplicativo?**

Veja a seguir conjuntos de criptografia atuais com suporte em ordem de prioridade.

TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P384

TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256

TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256

TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256

TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256

TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256

TLS_RSA_WITH_AES_256_GCM_SHA384

TLS_RSA_WITH_AES_128_GCM_SHA256

TLS_RSA_WITH_AES_256_CBC_SHA256

TLS_RSA_WITH_AES_128_CBC_SHA256

TLS_RSA_WITH_AES_256_CBC_SHA

TLS_RSA_WITH_AES_128_CBC_SHA

TLS_RSA_WITH_3DES_EDE_CBC_SHA

**P. O Gateway de Aplicativo também oferece suporte à nova criptografia de tráfego para o back-end?**

Sim, o Gateway de Aplicativo oferece suporte ao descarregamento SSL, SSL de ponta a ponta, que criptografa novamente o tráfego para o back-end.

**P. Posso configurar a política de SSL para controlar as versões de protocolo SSL?**

Sim, você pode configurar o Gateway de Aplicativo para negar TLS1.0, TLS1.1 e TLS1.2. SSL 2.0 e 3.0 já estão desabilitados por padrão e não são configuráveis.

**P. Posso configurar a política de SSL para controlar os conjuntos de criptografia?**

Não no momento.

**P. Há suporte para quantos certificados SSL?**

Há suporte para até 20 certificados SSL.

**P. Quantos certificados de autenticação para nova criptografia de back-end têm suporte?**

Há suporte para até 10 certificados de autenticação, com um padrão de cinco.

**P. O Gateway de Aplicativo é integrado ao Azure Key Vault de forma nativa?**

Não, ele não é integrado ao Azure Key Vault.

## <a name="web-application-firewall-waf-configuration"></a>Configuração de WAF (Firewall de Aplicativo Web)

**P. O SKU WAF oferece todos os recursos disponíveis com o SKU Standard?**

Sim, o WAF oferece suporte a todos os recursos no SKU Standard.

**P. Qual é a versão do CRS com suporte do Gateway de Aplicativo?**

O Gateway de Aplicativo do Azure dá suporte a CRS 2.2.9

**P. Como monitorar o WAF?**

O WAF é monitorado por meio do log de diagnóstico. Encontre mais informações sobre o log de diagnóstico em [Log de diagnósticos e métricas para o Gateway de Aplicativo](application-gateway-diagnostics.md)

**P. O modo de detecção bloqueia o tráfego?**

Não, o modo de detecção apenas registra o tráfego em log, o que dispara uma regra WAF.

**P. Como posso personalizar regras WAF?**

No momento, as regras WAF não são personalizáveis.

**P. Quais regras estão disponíveis no momento?**

No momento, o WAF oferece suporte a CRS 2.2.9, que fornece segurança de linha de base contra a maioria das dez principais vulnerabilidades identificadas pelo OWASP (Open Web Application Security Project) encontradas aqui [Dez principais vulnerabilidades do OWASP](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* Proteção contra injeção de SQL

* Proteção contra scripts entre sites

* Proteção Contra Ataques Comuns da Web, como a injeção de comandos, as solicitações HTTP indesejadas, a divisão de resposta HTTP e o ataque de inclusão de arquivo remoto

* Proteção contra violações de protocolo HTTP

* Proteção contra anomalias de protocolo HTTP, como ausência de host de agente do usuário e de cabeçalhos de aceitação

* Prevenção contra bots, rastreadores e scanners

* Detecção de problemas de configuração de aplicativo comuns (por exemplo, Apache, IIS etc.)

**P. O WAF também oferece suporte à prevenção de DDos?**

Não, o WAF não oferece prevenção de DDos.

## <a name="diagnostics-and-logging"></a>Diagnóstico e registro em log

**P. Quais tipos de logs estão disponíveis com o Gateway de Aplicativo?**

Há três logs disponíveis para o Gateway de Aplicativo. Para saber mais sobre esses logs e outros recursos de diagnóstico, visite [Integridade de back-end, registro em log e métricas de diagnósticos para o Gateway de Aplicativo](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog** -Esse log contém cada solicitação enviada ao front-end do Gateway de Aplicativo. Os dados incluem o IP do chamador, a URL solicitada, latência da resposta, o código de retorno, bytes de entrada e saída. O log de acesso é coletado a cada 300 segundos. Esse log contém um registro por instância do Application Gateway.
- **ApplicationGatewayPerformanceLog** - Esse log captura informações sobre o desempenho por instância, incluindo a solicitação total atendida, a taxa de transferência em bytes, o total de solicitações atendidas, a contagem de solicitações com falha, a contagem de instâncias de back-end íntegras ou não.
- **ApplicationGatewayFirewallLog** - Esse log contém solicitações registradas por meio do modo de detecção ou prevenção de um gateway de aplicativo configurado com o firewall do aplicativo Web.

**P. Como saber se os membros de meu pool de back-end estão íntegros?**

Use o cmdlet do PowerShell `Get-AzureRmApplicationGatewayBackendHealth` ou verifique a integridade por meio do portal visitando [Diagnóstico do Gateway de Aplicativo](application-gateway-diagnostics.md)

**P. Qual é a política de retenção nos logs de diagnóstico?**

Os logs de diagnóstico fluem para a conta de armazenamento de clientes, e os clientes podem definir a política de retenção com base em suas preferências. Os logs de diagnóstico também podem ser enviados a um Hub de Eventos ou ao Log Analytics. Visite [Diagnósticos do Gateway de Aplicativo](application-gateway-diagnostics.md) para obter mais detalhes.

**P. Como posso obter logs de auditoria para o Gateway de Aplicativo?**

Os logs de auditoria estão disponíveis para o Gateway de Aplicativo. No portal, clique em **Log de Atividades** na folha do menu de um Gateway de Aplicativo para acessar o log de auditoria. 

**P. Posso configurar alertas com o Gateway de Aplicativo?**

Sim, o Gateway de Aplicativo oferece suporte a alertas, e os alertas são configurados com base em métricas.  No momento, o Gateway de Aplicativo tem uma métrica de "taxa de transferência", que pode ser configurada para o alerta. Para saber mais sobre alertas, visite [Receber notificações de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**P. A integridade do back-end retorna um status desconhecido, o que pode estar causando isso?**

O motivo mais comum é o bloqueio ao acesso do back-end por um NSG ou DNS personalizado. Visite [Integridade do back-end, registro em log e métricas de diagnóstico do Gateway de Aplicativo](application-gateway-diagnostics.md) para saber mais.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o Gateway de Aplicativo, visite [Introdução ao Gateway de Aplicativo](application-gateway-introduction.md).

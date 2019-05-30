---
title: Por que as estações de trabalho seguras são importantes - Azure Active Directory
description: Saiba por que as organizações criar estações de trabalho seguras gerenciados do Azure
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7e9ca9fa26e9744eb0a9bfafe692a096825b0b5
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357044"
---
# <a name="building-secure-workstations"></a>Criação de estações de trabalho protegidas

Estações de trabalho isoladas seguras são extremamente importantes para a segurança das funções confidenciais, como administradores, desenvolvedores e operadores de serviços essenciais. Muitos outros controles de segurança e garantias irá falhar ou não têm nenhum efeito se a segurança de estação de trabalho de cliente subjacente foi comprometida.

Este documento explica o que é necessário para criar uma estação de trabalho de cliente segura com instruções passo a passo detalhadas, incluindo como configurar a partir de controles de segurança. Esse tipo de estações de trabalho às vezes é chamado de uma estação de trabalho com acesso privilegiado (PAW), que essa referência é usada e compilada em cima. No entanto, a orientação se parece a tecnologia baseada em nuvem para gerenciar o serviço e apresenta os recursos de segurança introduzidos a partir do Windows 10RS5, Microsoft Defender ATP, Azure Active Directory e do Intune.

## <a name="why-securing-workstation-access-is-important"></a>Por que é importante proteger o acesso de estação de trabalho

A rápida adoção de serviços de nuvem e a capacidade de trabalhar em qualquer lugar criou um novo método para exploração. Os invasores estão explorando os controles de segurança fraca em dispositivos em que os administradores funciona e são capazes de obter acesso a recursos privilegiados.

Conforme documentado na [relatório de ameaças da Verizon](https://enterprise.verizon.com/resources/reports/dbir/), e [Security Intelligence Report](https://aka.ms/sir) usar indevidamente com privilégios e ataques de cadeia de suprimentos estão entre os mecanismos de cinco principais usados para violar as organizações e o em segundo lugar, mais comumente detectada tática em incidentes relatados em 2018.

A maioria dos invasores siga o caminho abaixo:

* Comece com o reconhecimento, geralmente específico a um setor, encontrar uma maneira em
* Analisar as informações coletadas para identificar o melhor meio para obter acesso (infiltração) de uma estação de trabalho de baixo valor percebido
* Persistência e observe os meios para mover [lateralmente](https://en.wikipedia.org/wiki/Network_Lateral_Movement)
* Remover dados confidenciais e críticos

Os invasores frequentemente invadir a dispositivos que parecem baixo risco ou desvalorizadas para reconhecimento. Esses dispositivos vulneráveis, em seguida, são usados para localizar uma oportunidade para a movimentação lateral, localizar usuários administrativos e dispositivos e identificar os dados importantes alta, com êxito exfiltrar informações depois que eles obtêm essas funções de usuário privilegiado.

![Padrão de comprometimento típico](./media/concept-azure-managed-workstation/typical-timeline.png)

Este documento fornece uma solução para ajudar a proteger seus dispositivos de computação, isolando os serviços de gerenciamento e para ajudar a proteger contra a movimentação lateral ou ataques dos dispositivos de produtividade menos valiosos. O design ajuda a reduzir a capacidade de executar com êxito uma violação por interromper a cadeia antes de infiltração do dispositivo usado para gerenciar ou acessar recursos de nuvem confidenciais. A solução descrita utilizarão serviços nativos do Azure que fazem parte de pilha do Microsoft 365 Enterprise, incluindo:

* Intune para gerenciamento de dispositivos, incluindo o aplicativo e a lista de permissões de URL
* AutoPilot para atualização e implantação e configuração do dispositivo 
* Azure AD para gerenciamento de usuário, acesso condicional e autenticação multifator
* Windows 10 (versão atual) para a experiência de usuário e de atestado de integridade do dispositivo
* Microsoft Defender Advanced Threat ATP (proteção) para proteção de ponto de extremidade, detecção e resposta com o gerenciamento de nuvem
* Acesso aos recursos privilegiado do Azure AD PIM para gerenciar a autorização, incluindo tempo JIT (Just In)

## <a name="who-benefit-from-using-a-secure-workstation"></a>Quem se beneficiar do uso de uma estação de trabalho segura

Todos os usuários e os operadores de se beneficiar do uso de uma estação de trabalho segura. Um invasor que compromete um PC ou dispositivo pode fazer várias coisas incluindo representam todas as contas em cache e usar credenciais e tokens usados no dispositivo enquanto estão conectados. Esse risco torna a proteger os dispositivos usados para qualquer função com privilégios, incluindo direitos administrativos tão importantes como os dispositivos em que uma conta privilegiada é usada são alvos de movimento lateral e ataques de elevação de privilégio. Essas contas podem ser usadas para uma variedade de ativos, como:

* Administradores de sistemas baseados em nuvem e locais
* Estações de trabalho do desenvolvedor para sistemas críticos
* Administrador de contas de mídia social com alta exposição
* Estações de trabalho altamente confidenciais, como os terminais de pagamento SWIFT
* Tratamento de segredos comerciais de estações de trabalho

A Microsoft recomenda a implementação de controles de segurança com privilégios elevados para estações de trabalho com privilégios em que essas contas são usadas para reduzir o risco. Orientações adicionais podem ser encontradas na [guia de implantação de recursos do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2), [roteiro do Office 365](https://aka.ms/o365secroadmap), e [roteiro da proteção de acesso privilegiado](https://aka.ms/sparoadmap)).

## <a name="why-dedicated-workstations"></a>Por que dedicado estações de trabalho

Embora seja possível adicionar segurança para um dispositivo existente, é melhor começar com uma base segura. Começando com um dispositivo válidos e um conjunto de segurança conhecidos controles coloca a sua organização na melhor posição para manter o que aumenta o nível de segurança. Com o número cada vez maior de vetores de ataque permitidas por email casual e navegação na web, é cada vez mais difícil garantir que um dispositivo pode ser confiável. Funciona este guia sob a suposição de uma estação de trabalho dedicada separado do padrão de produtividade, navegação, e email tarefas forem concluídas. Remoção de produtividade, navegação na web e email de um dispositivo pode ter um impacto negativo na produtividade, mas essa proteção é geralmente aceitável para cenários em que as tarefas de trabalho não exigem explicitamente e risco de um incidente de segurança é alto.

> [!NOTE]
> Navegação na Web aqui refere-se ao acesso geral a sites arbitrários, que é um alto risco distintamente diferente do uso de um navegador da web para acessar um pequeno número de sites administrativos bem conhecidos para serviços como o Azure, Office 365, outros provedores de nuvem e SaaS aplicativos.

Estratégias de contenção fornecem garantias de segurança maior, aumentando o número e tipo dos controles de que um adversário precisa superar para acessar os ativos confidenciais. O modelo desenvolvido aqui fornece confinamento de privilégios administrativos para dispositivos específicos usando um modelo de privilégio em camadas.

## <a name="supply-chain-management"></a>Gerenciamento da cadeia de fornecedores

Essencial para uma estação de trabalho protegida é uma solução de cadeia de fornecimento em que a estação de trabalho que você usa é confiável, 'raiz de confiança'. Essa solução abordará a raiz de confiança usando o [Autopilot Microsoft](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-autopilot) tecnologia. Para uma estação de trabalho protegida Autopilot Microsoft fornece a capacidade de aproveitar os dispositivos da Microsoft com otimização de OEM do Windows 10 que fornecem um bom estado conhecido do fabricante. Em vez de refazer a imagem de um dispositivo que não seja confiável, Microsoft Autopilot pode transformar um dispositivo do Windows em um estado "pronto", aplicando configurações e políticas, instalação de aplicativos, e até mesmo alterar a edição do Windows 10 que está sendo usado (por exemplo, de Windows 10 Pro para Windows 10 Enterprise, para dar suporte a recursos avançados).

![Níveis de estação de trabalho segura](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Perfis e as funções do dispositivo

Neste guia, vários perfis de segurança e funções serão resolvidas para obter uma solução mais segura para a equipe de operações de TI, desenvolvedores e usuários. Esses perfis alinharam para dar suporte comuns que os usuários em organizações que podem se beneficiar de uma estação de trabalho avançada ou segura, equilibrando risco e usabilidade. A orientação fornecerá a definição das configurações com base nos padrões da indústria aceito. Este guia é usado para ilustrar um método no sistema de proteção de Windows 10 e reduzir os riscos associados ao comprometimento de dispositivo ou usuário usando a política e tecnologia para ajudar a gerenciar riscos e recursos de segurança.
![Níveis de estação de trabalho segura](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Baixa segurança** – uma estação de trabalho gerenciada standard fornece um bom ponto de partida para a maioria dos negócios pequenos e domésticos usa. Esses dispositivos são registrados no Azure AD e gerenciados pelo Intune. O perfil permite que os usuários executem todos os aplicativos e navegue até qualquer site. Como uma solução antimalware [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) deve ser habilitado.
* **Segurança aprimorada** – é uma solução de nível de entrada protegido, BOM para os usuários domésticos, usuários de pequenas empresas, bem como os desenvolvedores gerais.
   * A estação de trabalho avançado fornece uma política com base em meios aprimorar a segurança do perfil de baixa segurança. Esse perfil permite que um meio seguro trabalhar com dados do cliente e ser capaz de usar as ferramentas de produtividade, como verificação de email e navegação na web. Uma estação de trabalho avançado pode ser usada para auditar o comportamento do usuário e o uso do perfil de uma estação de trabalho por habilitar as políticas de auditoria e registro em log para o Intune. Neste perfil, a estação de trabalho permitirá que os controles de segurança e políticas descritas no conteúdo e implantados na estação de trabalho aprimorado - Windows10 script (1809). A implantação também se beneficia do uso de proteção contra malware avançado [proteção avançada de ameaças (ATP)](https://docs.microsoft.com/office365/securitycompliance/office-365-atp)
* **Alta segurança** – o meio mais eficaz para reduzir a superfície de ataque de uma estação de trabalho é remover a capacidade de self-administer a estação de trabalho. Remover direitos administrativos locais é uma etapa que aprimora a segurança e pode afetar a produtividade, se implementado incorretamente. O perfil de segurança alta baseia-se o perfil de segurança aprimorada com uma alteração considerável, a remoção de administrador local. Este perfil foi projetado para ajudar com os usuários que talvez um usuário de alto perfil como um executivo ou usuários que podem ter entre em contato com dados confidenciais, como folha de pagamento, ou a aprovação de serviços e processos.
   * O perfil de usuário de alta segurança exige um ambiente controlado maior enquanto ainda é capaz de executar a atividade de produtividade, como email e mantendo um simples de usar a experiência de navegação na web. Os usuários esperam recursos como cookies, Favoritos e outros atalhos disponíveis para operar. No entanto esses usuários podem não exigir a capacidade de modificar ou depurar o seu dispositivo e não serão necessário instalar drivers. O perfil de segurança alta é implantado usando a segurança de alta - script do Windows 10 (1809).
* **Especializado** – os desenvolvedores e administradores de TI são um alvo atraente para os invasores como essas funções podem alterar sistemas de interesse para os invasores. A estação de trabalho especializada leva o esforço implantado na estação de trabalho de alta segurança e emphases ainda mais sua segurança, gerenciamento de aplicativos locais, limitando a sites da internet, e restringindo os recursos de produtividade que são de altos risco, como ActiveX, Java, o navegador do plug-in e vários outros controles conhecidos de alto risco em um dispositivo do Windows. Neste perfil, a estação de trabalho permitirá que os controles de segurança e políticas descritas no conteúdo e implantados em DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline script.
* **Protegido** – um invasor que pode comprometer uma conta administrativa normalmente pode causar danos de negócio significativas por roubo de dados, a alteração de dados ou a interrupção do serviço. Nesse estado protegido, a estação de trabalho permitirá que todos os controles de segurança e políticas que restringem o controle direto do gerenciamento de aplicativos locais e as ferramentas de produtividade são removidas. Como resultado, comprometer o dispositivo fica mais difícil, como email e mídia social é bloqueada que refletem a maneira mais comum de ataques de phishing podem ser bem-sucedida.  A estação de trabalho protegida pode ser implantada com a estação de trabalho segura - script SecurityBaseline Windows10 (1809).

   ![Estação de trabalho protegida](./media/concept-azure-managed-workstation/secure-workstation.png)

   Uma estação de trabalho segura fornece ao administrador uma estação de trabalho protegida que tem controle de aplicativo não criptografado e a proteção do aplicativo. A estação de trabalho usará a credencial, o dispositivo e o exploit guard para proteger o host de comportamentos mal-intencionados. Além disso, todos os locais discos são criptografados com a criptografia Bitlocker.

* **Isolado** – neste cenário offline personalizado representa final extremo do espectro (nenhum script de instalação é fornecido para esse caso). As organizações podem precisar gerenciar uma função de crítico isolado de negócios como uma linha de produção de alto valor ou sistemas de suporte de vida que requer sem suporte/sem patch os sistemas operacionais herdados. Como a segurança é fundamental e os serviços de nuvem não estão disponíveis, as organizações podem manualmente gerenciar/atualizar esses computadores ou usar uma arquitetura de floresta do Active Directory (como o Enhanced Security Admin ESAE (ambiente)) isolada para gerenciá-los. Esses circunstância removendo todo o acesso, exceto básica do Intune e do ATP a verificação de integridade deve ser considerada.
   * [Requisito de comunicações de rede do Intune](https://docs.microsoft.com/intune/network-bandwidth-use)
   * [Requisito de comunicações de rede do ATP](https://docs.microsoft.com/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Próximas etapas

[Implantação de uma estação de trabalho segura gerenciados do Azure](howto-azure-managed-workstation.md)

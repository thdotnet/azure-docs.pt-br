---
title: Contêineres de perfil FSLogix e arquivos do Azure na área de trabalho de Virtual do Windows - Azure
description: Este artigo descreve os contêineres de perfil FSLogix nos arquivos de área de trabalho Virtual do Windows e o Azure.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: v-chjenk
ms.openlocfilehash: b3032aa796b3c79572bbf8b2beb85efc252ff73b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66497533"
---
# <a name="fslogix-profile-containers-and-azure-files"></a>Contêineres de perfil FSLogix e arquivos do Azure

O serviço de visualização de área de trabalho Virtual do Windows recomenda FSLogix contêineres de perfil como uma solução de perfil do usuário. FSLogix foi projetado para fazer roaming de perfis em ambientes de computação remotas, como área de trabalho Virtual do Windows. Ele armazena um perfil completo do usuário em um único contêiner. Ao entrar, esse contêiner dinamicamente é anexado ao ambiente de computação usando o disco rígido Virtual do Hyper-V e de disco rígido Virtual (VHD) com suporte nativo (VHDX). O perfil do usuário está disponível imediatamente e aparece no sistema exatamente como um perfil de usuário nativo.

Neste artigo, descreveremos FSLogix contêineres de perfil usados com arquivos do Azure. As informações estão no contexto da área de trabalho Virtual do Windows, que era [anunciadas em 3/21](https://www.microsoft.com/microsoft-365/blog/2019/03/21/windows-virtual-desktop-public-preview/).

## <a name="user-profiles"></a>Perfis do usuário

Um perfil de usuário contém elementos de dados sobre uma pessoa, incluindo informações de configuração como configurações da área de trabalho, conexões de rede persistente e as configurações do aplicativo. Por padrão, o Windows cria um perfil de usuário local que está totalmente integrado com o sistema operacional.

Um perfil de usuário remoto fornece uma partição entre o sistema operacional e dados de usuário. Ele permite que o sistema operacional a ser substituído ou alterado sem afetar os dados de usuário. No Host de sessão de área de trabalho remota (RDSH) e infraestruturas de área de trabalho Virtual (VDI), o sistema operacional pode ser substituído pelos seguintes motivos:

- Uma atualização do sistema operacional
- Uma substituição de uma máquina Virtual (VM existente)
- Um usuário que está sendo parte de um ambiente RDSH ou VDI em pool (não persistentes)

Produtos da Microsoft operam com várias tecnologias para perfis de usuário remoto, incluindo essas tecnologias:
- Perfis de usuário móvel (RUP)
- Discos de perfil do usuário (UDP)
- Enterprise state roaming (ESR)

UPD e RUP são as tecnologias mais amplamente usadas para perfis de usuário em ambientes de Host de sessão de área de trabalho remota (RDSH) e o disco rígido Virtual (VHD).

### <a name="challenges-with-previous-user-profile-technologies"></a>Desafios com as tecnologias anteriores de perfil do usuário

Soluções da Microsoft existentes e herdadas para perfis de usuário fornecida com diversos desafios diferentes. Nenhuma solução anterior manipulado todas as necessidades de perfil de usuário que vêm com um ambiente de RDSH ou VDI. Por exemplo, o UPD não pode manipular grandes arquivos OST e RUP não persiste configurações modernos.

#### <a name="functionality"></a>Funcionalidade

A tabela a seguir mostra os benefícios e limitações das tecnologias de perfil de usuário anterior.

| Tecnologia | Configurações modernos | Configurações do Win32 | Configurações do sistema operacional | Dados do usuário | Tem suporte no SKU do servidor | Armazenamento de back-end no Azure | Armazenamento de back-end no local | Suporte de versão | Hora de entrada subsequentes |Observações|
| ---------- | :-------------: | :------------: | :---------: | --------: | :---------------------: | :-----------------------: | :--------------------------: | :-------------: | :---------------------: |-----|
| **Discos de perfil do usuário (UDP)** | Sim | sim | sim | sim | sim | Não | Sim | Win + 7 | Sim | |
| **Roaming (perfil de usuário), o modo de manutenção** | Não | sim | sim | sim | sim| Não | Sim | Win + 7 | Não | |
| **Enterprise State Roaming (ESR)** | Sim | Não | Sim | Não | Consulte as observações | Sim | Não | Win 10 | Não | Funções no SKU do servidor, mas nenhuma interface do usuário de suporte |
| **User Experience Virtualization (UE-V)** | Sim | sim | sim | Não | Sim | Não | Sim | Win + 7 | Não |  |
| **Arquivos de nuvem do OneDrive** | Não | Não | Não | Sim | Consulte as observações | Consulte as observações  | Consulte as observações | Win 10 RS3 | Não | Não é testado no SKU do servidor. Armazenamento de back-end do Azure depende do cliente de sincronização. Local do armazenamento de back-end precisa de um cliente de sincronização. |

#### <a name="performance"></a>Desempenho

Requer o UPD [espaços de armazenamento diretos (S2D)](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment) para atender aos requisitos de desempenho. UPD usa o protocolo de bloco de mensagens de servidor (SMB). Ele copia o perfil para a VM na qual o usuário está conectado. UPD com S2D foi a solução que a equipe RDS é recomendada para a área de trabalho Virtual do Windows durante a visualização do serviço.  

#### <a name="cost"></a>Custo

Enquanto os clusters do S2D atingir o desempenho necessário, o custo é caro para clientes corporativos, mas especialmente dispendiosa para clientes de pequenas e médias empresas (SMB). Para esta solução, as empresas paga pelos discos de armazenamento premium, junto com o custo das VMs que usam os discos para um compartilhamento.

#### <a name="administrative-overhead"></a>Sobrecarga administrativa

Clusters do S2D exigem um sistema operacional que é corrigido, atualizado e mantido em um estado seguro. Esses processos e a complexidade da configuração de recuperação de desastres do S2D tornam S2D viáveis apenas para empresas com uma equipe de TI dedicada.

## <a name="fslogix-profile-containers"></a>Contêineres de perfil FSLogix

Em 19 de novembro de 2018 [a Microsoft adquiriu FSLogix](https://blogs.microsoft.com/blog/2018/11/19/microsoft-acquires-fslogix-to-enhance-the-office-365-virtualization-experience/). FSLogix resolve muitos desafios de contêiner de perfil. A chave entre eles são:

- **Desempenho:** O [contêineres de perfil FSLogix](https://fslogix.com/products/profile-containers) são de alto desempenho e resolver problemas de desempenho que historicamente bloqueados no modo cache do exchange.
- **OneDrive:** Sem contêineres de perfil FSLogix, não há suporte para OneDrive for Business em ambientes de RDSH ou VDI não persistente. [OneDrive for Business, as práticas recomendadas e FSLogix](https://fslogix.com/products/technical-faqs/284-onedrive-for-business-and-fslogix-best-practices) descreve como eles interagem. Para obter mais informações, consulte [usar o cliente de sincronização em áreas de trabalho virtuais](https://docs.microsoft.com/deployoffice/rds-onedrive-business-vdi).
- **Pastas adicionais:** FSLogix fornece a capacidade de estender os perfis de usuário para incluir pastas adicionais.

Desde a aquisição, a Microsoft começaram a substituir as soluções existentes de perfil de usuário, como o UPD, com contêineres do perfil FSLogix.

## <a name="azure-files-integration-with-azure-active-directory"></a>Integração de arquivos do Azure com o Azure Active Directory

Recursos e desempenho de contêineres de perfil FSLogix tirar proveito da nuvem. No dia 24 de setembro de 2018, arquivos do Microsoft Azure lançou uma visualização pública da [os arquivos do Azure que dão suporte a autenticação do Active Directory do Azure](https://azure.microsoft.com/blog/azure-active-directory-integration-for-smb-access-now-in-public-preview/). Tratando de custo e sobrecarga administrativa, arquivos do Azure com autenticação do Active Directory do Azure é uma solução de premium para perfis de usuário no novo serviço de área de trabalho Virtual do Windows.

## <a name="best-practices-for-windows-virtual-desktop"></a>Práticas recomendadas para a área de trabalho Virtual do Windows

Área de trabalho Virtual do Windows oferece controle total sobre o tamanho, tipo e contagem de VMs que estão sendo usados pelos clientes. Para obter mais informações, consulte [qual é a visualização de área de trabalho Virtual do Windows?](https://docs.microsoft.com/azure/virtual-desktop/overview).

Para garantir que sua área de trabalho Virtual do Windows ambiente segue as práticas recomendadas:

- Conta de armazenamento de arquivos do Azure deve estar na mesma região que o host de sessão de VMs.
- Permissões de arquivos do Azure devem corresponder ao permissões descritas em [requisitos – perfil contêineres](https://docs.fslogix.com/display/20170529/Requirements+-+Profile+Containers).
- Cada pool de host deve ser compilado do mesmo tipo e tamanho de VM com base na mesma imagem mestre.
- Cada VM do pool de host deve estar no mesmo grupo de recursos para auxiliar o gerenciamento, dimensionamento e a atualização.
- Para otimizar o desempenho, a solução de armazenamento e o contêiner de perfil deve estar nos mesmos dados de FSLogix center local.
- A conta de armazenamento que contém a imagem mestra deve ser na mesma região e assinatura em que as VMs estão sendo provisionadas.

## <a name="next-steps"></a>Próximas etapas

Use as instruções a seguir para configurar um ambiente de área de trabalho Virtual do Windows.

- Para iniciar a criação de sua solução de virtualização de área de trabalho, consulte [criar um locatário na área de trabalho Virtual do Windows](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory).
- Para criar um pool de host em seu locatário de área de trabalho Virtual do Windows, consulte [criar um pool de host com o Azure Marketplace](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace).
- Para configurar totalmente gerenciado compartilhamentos de arquivos na nuvem, consulte [configurar o compartilhamento de arquivos do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-enable).
- Para configurar contêineres do perfil FSLogix, consulte [configurar um compartilhamento de perfil de usuário para um pool de host](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-user-profile).
- Para atribuir usuários a um pool de host, consulte [gerenciar grupos de aplicativos para área de trabalho Virtual do Windows](https://docs.microsoft.com/azure/virtual-desktop/manage-app-groups).
- Para acessar seus recursos de área de trabalho Virtual do Windows de um navegador da web, consulte [conectar-se a área de trabalho Virtual do Windows](https://docs.microsoft.com/azure/virtual-desktop/connect-web).

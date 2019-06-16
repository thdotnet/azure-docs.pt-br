---
title: O que é o Azure Disk Encryption?
description: Este artigo fornece uma visão geral do Azure Disk Encryption
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: 9b00a5262b1e144aa47cd7fd640906225ff4fecd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068799"
---
# <a name="azure-disk-encryption-overview"></a>Visão geral de criptografia de disco do Azure

O Azure Disk Encryption ajuda a proteger seus dados para atender aos seus compromissos de conformidade e segurança organizacionais. Ele usa o [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) recurso do Windows e o [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) recurso do Linux para fornecer criptografia de volume para os sistema operacional e discos de dados de máquinas virtuais (VMs). Ele também é integrado com [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para ajudá-lo a controlar e gerenciar as chaves de criptografia de disco e segredos e garante que todos os dados nos discos de VM são criptografados em repouso no armazenamento do Azure. Criptografia de disco para o Windows e VMs do Linux do Azure está em disponibilidade geral em todas as regiões públicas do Azure e regiões do Azure governamental para VMs Standard e VMs com armazenamento Premium do Azure. 

Se você usar a Central de Segurança do Azure, receberá um alerta se tiver VMs não criptografadas. Esses alertas são mostrados como Alta Gravidade e a recomendação é criptografar essas VMs.

![Alerta de criptografia de disco na Central de Segurança do Azure](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Determinadas recomendações podem aumentar o uso de recursos de dados, rede ou computação e resultar em custos adicionais de licença ou assinatura.


## <a name="encryption-scenarios"></a>Cenários de criptografia

Com o Azure Disk Encryption, você pode atender a requisitos de conformidade e segurança organizacional, protegendo suas VMs do Azure em repouso usando tecnologia de criptografia padrão do setor. Você também pode configurar máquinas virtuais para inicialização em chaves controladas pelo cliente e políticas (BYOK) e auditar o uso dessas chaves no cofre de chaves.

O Azure Disk Encryption dá suporte a cenários de cliente a seguir:

* Habilitar e desabilitar a criptografia em novas VMs criadas a partir das imagens da Galeria do Azure com suporte.
* Habilitar e desabilitar a criptografia em VMs existentes que são executados no Azure.
* Habilitar e desabilitar a criptografia em novas VMs do Windows criados a partir do VHD pré-criptografado e chaves de criptografia.
* Habilitar e desabilitar a criptografia no dimensionamento de máquinas virtuais do Windows define.
* Habilitar e desabilitar a criptografia em dados de discos para conjuntos de dimensionamento de máquina virtual do Linux.
* Habilitar e desabilitar a criptografia de VMs de disco gerenciado.
* Atualizando configurações de criptografia de um Premium criptografada existente e não - VM de armazenamento Premium.
* Fazer backup e restaurar VMs criptografadas.
* Traga sua própria criptografia (BYOE) e cenários traga seu próprio chave (BYOK), em que os clientes usam suas próprias chaves de criptografia em armazená-los em um cofre de chaves do Azure.

Ele também suporta os seguintes cenários para VMs quando elas são habilitadas no Microsoft Azure:

* Integração com o Azure Key Vault.
* [VMs da camada Standard](https://azure.microsoft.com/pricing/details/virtual-machines/). As [VMs do Linux](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport), dentro dessas camadas, devem atender ao requisito mínimo de memória de 7 GB. 
* Habilitando a criptografia em VMs do Linux e Windows, discos gerenciados e escala definido VMs das imagens da Galeria do Azure com suporte.
* Desabilitar a criptografia no sistema operacional e dados unidades para VMs do Windows, escala do conjunto de VMs e gerenciados de VMs de disco.
* Desabilitar criptografia em unidades de dados para VMs do Linux, dimensionamento do conjunto de VMs e gerenciados VMs de disco.
* Habilitar a criptografia em VMs que executam o sistema operacional cliente do Windows.
* Habilitar a criptografia em volumes com caminhos de montagem.
* Habilitar a criptografia em VMs do Linux configuradas com disk striping (RAID) usando mdadm.
* Habilitar a criptografia em VMs do Linux que usar LVM para discos de dados.
* Habilitar a criptografia nos discos de dados e sistema operacional de VM do Linux.

   > [!NOTE]
   > Não há suporte para criptografia de unidade do sistema operacional para algumas distribuições do Linux. Para obter mais informações, confira o artigo [Perguntas frequentes sobre o Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).
   
* Habilitar a criptografia em VMs que são configuradas com a partir de espaços de armazenamento do Windows no Windows Server 2016.
* Fazer backup e restauração das VMs criptografadas para a chave de criptografia de chave (KEK) e cenários de não KEK.

O Azure Disk Encryption não funciona para os seguintes cenários, recursos e tecnologia:

* Criptografia de VM ou VMs criadas por meio do método de criação de VM clássico de camada básica.
* Desabilitar a criptografia em uma unidade do sistema operacional ou a unidade de dados de uma VM Linux quando a unidade do sistema operacional é criptografada.
* Define a criptografia de unidade do sistema operacional para o dimensionamento de máquinas virtuais do Linux.
* Criptografia de VMs do Windows configuradas com sistemas RAID baseados em software.
* Imagens personalizadas com criptografia em VMs do Linux.
* Integração com um sistema de gerenciamento de chaves local.
* Arquivos do Azure (sistema de arquivo compartilhado).
* NFS (Network File System).
* Volumes dinâmicos.

## <a name="encryption-features"></a>Recursos de criptografia

Quando você habilitar e implantar o Azure Disk Encryption para VMs do Azure, você pode configurar os seguintes recursos para ser habilitado:

* Criptografar o volume do sistema operacional para proteger o volume de inicialização em repouso no seu armazenamento.
* Criptografar volumes de dados para proteger os volumes de dados em repouso no seu armazenamento.
* Desabilitar a criptografia em unidades de sistema operacional e dados para VMs do Windows.
* Desabilitar a criptografia nos dados de unidades para VMs do Linux (somente quando a unidade do sistema operacional não é criptografada).
* Protegendo chaves de criptografia e segredos em sua assinatura do Azure Key Vault.
* Relatório de status de criptografia da VM criptografada.
* Removendo as definições de configuração de criptografia de disco da VM.
* Fazer backup e restaurar VMs criptografadas usando o serviço de Backup do Azure.

O Azure Disk Encryption para VMs para Windows e Linux inclui:

* [A extensão de criptografia de disco para Windows](../virtual-machines/extensions/azure-disk-enc-windows.md).
* [A extensão de criptografia de disco para Linux](../virtual-machines/extensions/azure-disk-enc-linux.md).
* [Os cmdlets de criptografia de disco do PowerShell](/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-2.2.0).
* [Os cmdlets de criptografia de disco da CLI do Azure](/cli/azure/vm/encryption?view=azure-cli-latest).
* [Os modelos de criptografia de disco do Azure Resource Manager](azure-security-disk-encryption-appendix.md#resource-manager-templates).

O Azure Disk Encryption tem suporte em VMs que executam o Windows ou o sistema operacional Linux. Para obter mais informações sobre os sistemas operacionais com suporte, consulte [perguntas frequentes](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).

> [!NOTE]
> Não são cobradas taxas adicionais para criptografar discos de VM com o Azure Disk Encryption. O [preço do Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) padrão aplica-se ao cofre de chaves que é usado para armazenar as chaves de criptografia. 

## <a name="encryption-workflow"></a>Fluxo de trabalho de criptografia

Para habilitar a criptografia de disco para VMs do Windows e do Linux, faça o seguinte:

1. Opte por habilitar a criptografia de disco por meio do modelo do Gerenciador de Recursos de Azure Disk Encryption, cmdlets do PowerShell ou CLI do Azure e especifique a configuração de criptografia.

   * Para o cenário do VHD criptografado pelo cliente, carregue o VHD criptografado para sua conta de armazenamento e o material de chave de criptografia para o cofre de chaves. Em seguida, forneça a configuração de criptografia para habilitar a criptografia em uma nova VM.
   * Para novas VMs que são criadas a partir de imagens da galeria com suporte e VMs existentes que já foi executado no Azure, forneça a configuração de criptografia para habilitar a criptografia na VM.

1. Conceda acesso à plataforma do Azure para ler o material de chave de criptografia (chaves de criptografia do BitLocker para sistemas Windows) e a frase secreta para Linux do Cofre de chaves para habilitar a criptografia na VM.

1. O Azure atualiza o modelo de serviço de VM com a configuração do cofre de chaves e a criptografia e define sua VM criptografada.

   ![Microsoft Antimalware no Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Fluxo de trabalho de descriptografia
Para desabilitar a criptografia de disco para VMs, conclua as seguintes etapas de alto nível:

1. Optar por desabilitar a criptografia (descriptografia) em uma VM em execução no Azure e especificar a configuração da descriptografia. É possível por meio do modelo do Resource Manager do Azure Disk Encryption, cmdlets do PowerShell ou da CLI do Azure.

   Essa etapa desabilita a criptografia do sistema operacional ou o volume de dados ou ambos na VM do Windows em execução. Como mencionado na seção anterior, não há suporte para desabilitar a criptografia de disco do sistema operacional para Linux. A etapa de descriptografia é permitida apenas para unidades de dados em VMs do Linux, desde que o disco do SO não esteja criptografado.

1. Atualizações do Azure, o modelo de serviço da VM e a VM está marcada como descriptografada. O conteúdo da VM não está criptografado em repouso.

   > [!NOTE]
   > A operação de desabilitação da criptografia não exclui seu cofre de chaves nem o material da chave de criptografia (chaves de criptografia do BitLocker para sistemas Windows ou a Frase secreta para Linux).
   >
   > Não há suporte para desativação de criptografia de disco de OS para Linux. A etapa de descriptografia é permitida somente para unidades de dados em VMs Linux.
   >
   > Desabilitar criptografia do disco de dados para Linux não tem suporte se a unidade do SO estiver criptografada.


## <a name="encryption-workflow-previous-release"></a>Fluxo de trabalho de criptografia (versão anterior)

A nova versão do Azure Disk Encryption elimina a necessidade de fornecer um parâmetro de aplicativo do Microsoft Azure Active Directory (Azure AD) para habilitar a criptografia de disco de VM. Com a nova versão, você não precisa fornecer uma credencial do Microsoft Azure Active Directory durante a etapa de habilitação de criptografia. Todas as novas VMs devem ser criptografadas sem os parâmetros do aplicativo do Microsoft Azure Active Directory ao usar a nova versão. As VMs que já foram criptografadas com os parâmetros de aplicativo do Azure AD ainda têm suporte e devem continuar a ser mantidas com a sintaxe do Microsoft Azure Active Directory. Para habilitar a criptografia de disco para VMs do Windows e do Linux (versão anterior), faça o seguinte:

1. Escolha um cenário de criptografia nos cenários listados na seção [Cenários de criptografia](#encryption-scenarios).

1. Opte por habilitar a criptografia de disco por meio do modelo do Gerenciador de Recursos de Azure Disk Encryption, cmdlets do PowerShell ou CLI do Azure e especifique a configuração de criptografia.

   * Para o cenário do VHD criptografado pelo cliente, carregue o VHD criptografado para sua conta de armazenamento e o material de chave de criptografia para o cofre de chaves. Em seguida, forneça a configuração de criptografia para habilitar a criptografia em uma nova VM.
   * Para novas VMs criadas por meio do Marketplace e VMs existentes que já foi executado no Azure, forneça a configuração de criptografia para habilitar a criptografia na VM.

1. Conceda acesso à plataforma do Azure para ler o material de chave de criptografia (chaves de criptografia do BitLocker para sistemas Windows) e a frase secreta para Linux do Cofre de chaves para habilitar a criptografia na VM.

1. Forneça a identidade de aplicativo do Microsoft Azure Active Directory para gravar o material de chave de criptografia no cofre de chaves. Esta etapa habilita a criptografia na VM para os cenários mencionados na etapa 2.

1. O Azure atualiza o modelo de serviço de VM com a configuração do cofre de chaves e a criptografia e define sua VM criptografada.


## <a name="terminology"></a>Terminologia
A tabela a seguir define alguns dos termos comuns usados na documentação de criptografia de disco do Azure:

| Terminologia | Definição |
| --- | --- |
| AD do Azure | Uma conta do [Microsoft Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/) é usada para autenticar, armazenar e recuperar segredos de um cofre de chaves. |
| Cofre da Chave do Azure | Key Vault é um serviço de gerenciamento de chaves criptográfico baseado em módulos de segurança de hardware validados pelo FIPS (Federal Information Processing Standards). Esses padrões ajudam a proteger as chaves criptográficas e os segredos confidenciais. Para obter mais informações, consulte a documentação do [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) é uma reconhecidas no setor Windows volume tecnologia de criptografia que é usada para habilitar a criptografia de disco em VMs do Windows. |
| BEK | As chaves de criptografia do BitLocker (BEK) são usadas para criptografar o volume de inicialização do sistema operacional e os volumes de dados. As BEKs são protegidas em um cofre de chaves como segredos. |
| CLI do Azure | [A CLI do Azure](/cli/azure/install-azure-cli) é otimizada para gerenciar e administrar os recursos do Azure na linha de comando.|
| DM-Crypt |[DM-Crypt](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) é o subsistema de criptografia de disco transparente, baseado em Linux, que é usado para habilitar a criptografia de disco em VMs do Linux. |
| Chave de criptografia de chave (KEK) | A chave assimétrica (RSA 2048) que você pode usar para proteger ou encapsular o segredo. É possível fornecer uma chave protegida por HSM (módulos de segurança de hardware) ou uma chave protegida por software. Para obter mais informações, consulte a documentação do [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| Cmdlets do PowerShell | Para obter mais informações, confira [cmdlets do Azure PowerShell](/powershell/azure/overview). |

## <a name="next-steps"></a>Próximas etapas

Para começar, consulte a [pré-requisitos do Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md).


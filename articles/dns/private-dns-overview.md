---
title: O que é DNS privado do Azure?
description: Uma visão geral do serviço de hospedagem de DNS privado no Microsoft Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: overview
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: a8548b4972d5853f09630ae3e9ded05ed6fee32b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076406"
---
# <a name="what-is-azure-private-dns"></a>O que é DNS privado do Azure?

> [!IMPORTANT]
> O DNS privado do Azure está atualmente em visualização pública.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

O Sistema de Nomes de Domínio ou DNS, é responsável por converter (ou resolver) um nome de serviço em seu endereço IP.  O DNS do Azure é um serviço de hospedagem para domínios DNS, fornecendo a resolução de nomes usando a infraestrutura do Microsoft Azure. Além de dar suporte a domínios DNS para a internet, o DNS do Azure também dá suporte a zonas DNS privadas.

O DNS privado do Azure fornece um serviço DNS confiável e seguro para gerenciar e resolver nomes de domínio em uma rede virtual sem a necessidade de adicionar uma solução DNS personalizada. Ao usar zonas DNS privadas você poderá usar seus próprios nomes de domínio personalizados, em vez dos nomes fornecidos pelo Azure atualmente disponíveis. O uso de nomes de domínio personalizados ajuda você a personalizar a sua arquitetura de rede virtual para melhor satisfazer às necessidades da sua organização. Esse uso fornece resolução de nomes para VMs (máquinas virtuais) em uma rede virtual e entre redes virtuais. Além disso, você pode configurar nomes de zonas com uma exibição de omissão de rotas, o que permite que uma zona DNS privada e pública compartilhe o nome.

Para resolver os registros de uma zona DNS privada de sua rede virtual, você deve vincular a rede virtual com a zona. Redes virtuais vinculadas têm acesso completo e podem resolver todos os registros DNS publicados na zona privada. Além disso, você também pode habilitar o registro automático em um link de rede virtual. Se você habilitar o registro automático em um link de rede virtual, os registros DNS para as máquinas virtuais nessa rede virtual são registrados na zona privada. Quando o registro automático estiver habilitado, o DNS do Azure também atualiza os registros da zona sempre que uma máquina virtual é criada, alterações de seu ' endereço IP, ou é excluída.

![Visão geral de DNS](./media/private-dns-overview/scenario.png)

> [!NOTE]
> Como prática recomendada, não use uma *. local* domínio para sua zona DNS privada. Nem todos os sistemas operacionais dão suporte a isso.

## <a name="benefits"></a>Benefícios

O DNS privado do Azure oferece os seguintes benefícios:

* **Remove a necessidade de soluções personalizadas de DNS**. Anteriormente, muitos clientes criaram soluções personalizadas de DNS para gerenciar zonas DNS em suas redes virtuais. Agora você pode gerenciar zonas DNS usando a infraestrutura do Azure nativa, que remove a sobrecarga de criar e gerenciar soluções personalizadas de DNS.

* **Usar todos os tipos de registros DNS comuns**. O DNS do Azure dá suporte a registros A, AAAA, CNAME, MX, PTR, SOA, SRV e TXT.

* **Gerenciamento automático de registro de nome do host**. Junto com a hospedagem de seus registros DNS personalizados, o Azure manterá automaticamente os registros de nome do host das VMs em redes virtuais especificadas. Nesse cenário, é possível otimizar os nomes de domínio usados sem precisar criar soluções DNS personalizadas ou modificar aplicativos.

* **Resolução de nome do host entre redes virtuais**. Ao contrário dos nomes de host fornecidos pelo Azure, as zonas de DNS privado podem ser compartilhadas entre redes virtuais. Essa capacidade simplifica os cenários entre redes e de descoberta de serviço, como o emparelhamento de rede virtual.

* **Ferramentas familiares e experiência do usuário**. Para reduzir a curva de aprendizado, esse serviço usa ferramentas de DNS do Azure bem estabelecidas (portal do Azure, Azure PowerShell, CLI do Azure, modelos do Azure Resource Manager e a API REST).

* **Suporte a omissão de rotas DNS**. Com o DNS do Azure, você pode criar zonas com o mesmo nome que resolvem respostas diferentes a partir de uma rede virtual e da Internet pública. Um cenário típico para omissão de rotas DNS é o fornecimento de uma versão dedicada de um serviço para uso dentro de sua rede virtual.

* **Disponível em todas as regiões do Azure**. O recurso de zonas privadas do DNS do Azure está disponível em todas as regiões do Azure na nuvem pública do Azure.

## <a name="capabilities"></a>Funcionalidades

O DNS do Azure fornece os seguintes recursos:

* **Registro automático de máquinas virtuais de uma rede virtual que esteja vinculado a uma zona privada com o registro automático habilitado**. As máquinas virtuais são o registradas (adicionadas) à zona privada como registros a apontando para seus endereços IP privados. Quando uma máquina virtual em um link de rede virtual com o registro automático habilitado é excluída, o DNS do Azure remove automaticamente o registro DNS correspondente da zona privada vinculada.

* **Resolução DNS direta é suportada pelas diversas redes virtuais que são vinculadas à zona privada**. Para entre redes virtuais a resolução de DNS, não há nenhuma dependência explícita, de modo que as redes virtuais são emparelhadas umas com as outras. No entanto, você talvez queira emparelhar redes virtuais para outros cenários (por exemplo, o tráfego HTTP).

* **Pesquisa inversa do DNS tem suportada dentro do escopo de rede virtual**. A pesquisa inversa do DNS para um IP privado dentro da rede virtual atribuída a uma zona privada retorna o FQDN que inclui o nome do host/registro e o nome da zona como o sufixo.

## <a name="other-considerations"></a>Outras considerações

O DNS do Azure tem as seguintes limitações:

* Apenas uma rede virtual de registro é permitida por zona privada.
* Até 10 redes virtuais de resolução são permitidas por zona privada. Esse limite será removido quando esse recurso estiver disponível em geral.
* Uma rede virtual específica pode ser vinculada a apenas uma zona privada como uma rede virtual de registro.
* Uma rede virtual específica pode ser vinculada a até 10 zonas privadas como uma rede virtual de resolução. Esse limite será removido quando esse recurso estiver disponível em geral.
* Se você especificar uma rede virtual de registro, os registros DNS das VMs dessa rede virtual, registrados na zona privada, não poderão ser visualizados nem recuperados nas APIs da CLI do Azure e no Azure PowerShell. Os registros da VM são realmente registrados e resolverão com sucesso.
* O DNS inverso funciona apenas para o espaço IP privado na rede virtual de registro.
* O DNS inverso para um IP privado que não está registrado na zona privada (por exemplo, um IP privado para uma máquina virtual em uma rede virtual que está vinculada como uma rede virtual de resolução a uma zona privada) retorna *internal.cloudapp.net* como o sufixo DNS. Entretanto, esse sufixo não é resolvível.
* A rede virtual deve estar totalmente vazia na primeira vez em que você a vincular com uma zona privada como uma rede virtual de registro ou resolução. No entanto, a rede virtual pode não estar vazia para futuras vinculações como uma rede virtual de registro ou de resolução a outras zonas privadas.
* No momento, o encaminhamento condicional não tem suporte (por exemplo, para habilitar a resolução entre as redes OnPrem e Azure). Para obter informações sobre como os clientes podem realizar esse cenário por meio de outros mecanismos, consulte [Resolução de nomes de VMs e instâncias de função](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
* Funciona o DNS reverso somente para o espaço IP privado na rede virtual vinculado
* O DNS reverso para um endereço IP privado para uma rede virtual vinculada retornará "internal.cloudapp.net" como o sufixo padrão para a máquina virtual. Para redes virtuais que estão vinculadas a uma zona privada com o registro automático habilitado, DNS reverso para um endereço IP privado retorna 2 FQDNs, uma com um padrão de sufixo *internal.cloudapp.net* e outra com o sufixo de zona privada.
* Encaminhamento condicional não tem suporte. Por exemplo, para habilitar a resolução de nomes entre redes do Azure e locais. Saiba como você pode habilitar esse cenário usando outros mecanismos. Veja [Resolução de nomes para VMs e instâncias de função](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)
 


## <a name="pricing"></a>Preços

Para obter informações sobre preços, consulte [preços do DNS do Azure](https://azure.microsoft.com/pricing/details/dns/).

## <a name="next-steps"></a>Próximas etapas

* Saiba como criar uma zona privada no DNS do Azure usando o [Azure PowerShell](./private-dns-getstarted-powershell.md) ou a [CLI do Azure](./private-dns-getstarted-cli.md).

* Leia mais sobre alguns [cenários comuns de zona privada](./private-dns-scenarios.md) que podem ser realizados com zonas privadas no DNS do Azure.

* Para perguntas e respostas comuns sobre zonas privadas no DNS do Azure, incluindo o comportamento específico você pode esperar para determinados tipos de operações, consulte [perguntas Frequentes do DNS privado](./dns-faq-private.md).

* Saiba mais sobre zonas e registros DNS, visitando [Visão geral de zonas e registros DNS](dns-zones-records.md).

* Saiba mais sobre alguns dos outros principais [recursos de rede](../networking/networking-overview.md) do Azure.

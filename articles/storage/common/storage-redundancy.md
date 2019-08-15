---
title: Redundância de dados no Armazenamento do Azure | Microsoft Docs
description: Os dados na sua conta do Armazenamento do Microsoft Azure são replicados para garantir durabilidade e alta disponibilidade. As opções de redundância incluem LRS (armazenamento com redundância local), ZRS (armazenamento com redundância de zona), GRS (armazenamento com redundância geográfica), armazenamento com redundância geográfica com acesso de leitura (RA-GRS), armazenamento com redundância de zona geográfica (GZRS) e com acesso de leitura com redundância de zona geográfica armazenamento (RA-GZRS) (visualização).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 07/10/2019
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: a4c624cf06a0e56b30b71c80a6b4a5ad48cb31a9
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69016405"
---
# <a name="azure-storage-redundancy"></a>Redundância do Armazenamento do Azure

Os dados na sua conta de armazenamento do Microsoft Azure sempre são replicados para garantir durabilidade e alta disponibilidade. A redundância do Armazenamento do Azure copia os dados para que eles sejam protegidos contra eventos planejados e não planejados, incluindo falhas de hardware transitórias, interrupções de energia ou rede e desastres naturais de grandes proporções. Você pode escolher replicar os dados no mesmo datacenter, em datacenters zonais na mesma região ou entre regiões separadas geograficamente.

A replicação garante que sua conta de armazenamento atenda ao [SLA (Contrato de Nível de Serviço) do Armazenamento](https://azure.microsoft.com/support/legal/sla/storage/) mesmo diante de falhas. Consulte o SLA para obter informações sobre as garantias do Armazenamento do Azure para disponibilidade e durabilidade.

O armazenamento do Azure verifica regularmente a integridade dos dados armazenados usando verificações de redundância cíclica (CRCs). Se a corrupção de dados for detectada, ela será reparada usando dados redundantes. O armazenamento do Azure também calcula somas de verificação em todo o tráfego de rede para detectar corrupção de pacotes de dados ao armazenar ou recuperar dados.

## <a name="choosing-a-redundancy-option"></a>Escolher uma opção de redundância

Ao criar uma conta de armazenamento, você deve selecionar uma das seguintes opções de redundância:

* [Armazenamento com redundância local (LRS)](storage-redundancy-lrs.md)
* [Armazenamento com redundância de zona (ZRS)](storage-redundancy-zrs.md)
* [Armazenamento com redundância geográfica (GRS)](storage-redundancy-grs.md)
* [Armazenamento com redundância geográfica com acesso de leitura (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)
* [Armazenamento com redundância de zona geográfica (GZRS)](storage-redundancy-gzrs.md)
* [Armazenamento com redundância de zona geográfica com acesso de leitura (RA-GZRS)](storage-redundancy-gzrs.md)

A tabela a seguir fornece uma visão geral rápida do escopo de durabilidade e disponibilidade que cada estratégia de replicação fornecerá a você para um determinado tipo de evento (ou evento de impacto semelhante).

| Cenário                                                                                                 | LRS                             | ZRS                              | GRS/RA-GRS                                  | GZRS/RA-GZRS                               |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Indisponibilidade de nó dentro de um datacenter                                                                 | Sim                             | Sim                              | Sim                                  | Sim                                  |
| Um data center inteiro (zonal ou não zonal) fica indisponível                                           | Não                              | Sim                              | Sim                                  | Sim                                  |
| Uma interrupção em toda a região                                                                                     | Não                              | Não                               | Sim                                  | Sim                                  |
| Acesso de leitura aos dados (em uma região remota e replicada geograficamente) no caso de indisponibilidade em toda a região | Não                              | Não                               | Sim (com RA-GRS)                                   | Sim (com RA-GZRS)                                 |
| Projetado para fornecer \_\_ durabilidade de objetos em um determinado ano                                          | no mínimo 99,999999999% (11 9's) | no mínimo 99,9999999999% (12 9's) | no mínimo 99,99999999999999% (16 9's) | no mínimo 99,99999999999999% (16 9's) |
| Tipos de conta de armazenamento suportados                                                                   | GPv2, GPv1, Blob                | GPv2                             | GPv2, GPv1, Blob                     | GPv2, GPv1, Blob                     |
| SLA de disponibilidade para solicitações de leitura | Pelo menos 99,9% (99% para a camada de acesso esporádico) | Pelo menos 99,9% (99% para a camada de acesso esporádico) | Pelo menos 99,9% (99% para a camada de acesso esporádico) | Pelo menos 99,99% (99,9% para a camada de acesso estática) |
| SLA de disponibilidade para solicitações de gravação | Pelo menos 99,9% (99% para a camada de acesso esporádico) | Pelo menos 99,9% (99% para a camada de acesso esporádico) | Pelo menos 99,9% (99% para a camada de acesso esporádico) | Pelo menos 99,9% (99% para a camada de acesso esporádico) |

Todos os dados em sua conta de armazenamento são replicados, incluindo BLOBs de blocos e blobs de acréscimo, blobs de páginas, filas, tabelas e arquivos. Todos os tipos de contas de armazenamento são replicados, embora ZRS exija uma conta de armazenamento v2 de uso geral.

Para obter informações sobre cada opção de redundância de preços, consulte [preços de armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/). 

Para obter informações sobre o armazenamento do Azure garante para durabilidade e disponibilidade, consulte o [SLA do armazenamento do Azure](https://azure.microsoft.com/support/legal/sla/storage/).

> [!NOTE]
> O armazenamento Premium do Azure dá suporte apenas ao LRS (armazenamento com redundância local).

## <a name="changing-replication-strategy"></a>Alterar estratégia de replicação

Você pode alterar a estratégia de replicação da sua conta de armazenamento usando o [portal do Azure](https://portal.azure.com/), o [Azure PowerShell](storage-powershell-guide-full.md), [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)ou uma das bibliotecas de cliente de [armazenamento do Azure](https://docs.microsoft.com/azure/index#pivot=sdkstools). Alterar o tipo de replicação da sua conta de armazenamento não resulta em tempo de inatividade.

   > [!NOTE]
   > No momento, você não pode usar as bibliotecas de cliente de armazenamento do portal do Azure ou do Azure para converter sua conta em ZRS, GZRS ou RA-GZRS. Para migrar sua conta para o ZRS, consulte [armazenamento com redundância de zona (ZRS) para criar aplicativos de armazenamento do Azure altamente disponíveis](storage-redundancy-zrs.md) para obter detalhes. Para migrar GZRS ou RA-GZRS, consulte [armazenamento com redundância de zona geográfica para alta disponibilidade e durabilidade máxima (versão prévia)](storage-redundancy-zrs.md) para obter detalhes.
    
### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Há algum custo para alterar a estratégia de replicação da minha conta?

Depende do caminho de conversão. Ordenando da menor para a mais cara, as ofertas de redundância de armazenamento do Azure LRS, ZRS, GRS, RA-GRS, GZRS e RA-GZRS. Por exemplo, passar *de* LRS para qualquer outro tipo de replicação incorrerá em encargos adicionais, pois você está mudando para um nível de redundância mais sofisticado. Migrar *para* grs ou ra-grs incorrerá em um encargo de largura de banda de saída, pois seus dados (em sua região primária) estão sendo replicados para sua região secundária remota. Essa cobrança é um custo único na configuração inicial. Depois que os dados são copiados, não há nenhuma cobrança adicional de migração. Você é cobrado somente pela replicação de novas ou atualizações nos dados existentes. Para obter detalhes sobre taxas de largura de banda, visite a [página Preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/).

Se você migrar sua conta de armazenamento de GRS para LRS, não haverá nenhum custo adicional, mas os dados replicados serão excluídos do local secundário.

Se você migrar sua conta de armazenamento de RA-GRS para GRS ou LRS, essa conta será cobrada como RA-GRS por um adicional 30 dias além da data em que foi convertida.

## <a name="see-also"></a>Consulte também

- [Armazenamento com redundância local (LRS): Redundância de dados de baixo custo para o Armazenamento do Microsoft Azure](storage-redundancy-lrs.md)
- [ZRS (armazenamento com redundância de zona): Aplicativos de Armazenamento do Azure altamente disponíveis](storage-redundancy-zrs.md)
- [Armazenamento com redundância geográfica (GRS): Replicação inter-regional para Armazenamento do Microsoft Azure](storage-redundancy-grs.md)
- [Armazenamento com redundância de zona geográfica (GZRS) para alta disponibilidade e durabilidade máxima (versão prévia)](storage-redundancy-gzrs.md)
- [Metas de desempenho e escalabilidade do Armazenamento do Azure](storage-scalability-targets.md)
- [Criando aplicativos altamente disponíveis usando o armazenamento de RA-GRS](../storage-designing-ha-apps-with-ragrs.md)
- [Opções de redundância Armazenamento do Microsoft Azure e armazenamento com redundância geográfica com acesso de leitura](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [SOSP Paper – Armazenamento do Azure: Um serviço de armazenamento em nuvem altamente disponível com coerência forte](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

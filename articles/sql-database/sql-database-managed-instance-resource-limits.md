---
title: Limites de recursos do Banco de Dados SQL do Azure - instância gerenciada | Microsoft Docs
description: Este artigo fornece uma visão geral dos limites de recursos do Banco de Dados SQL do Azure para instâncias gerenciadas.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop, sachinp, sstein
manager: craigg
ms.date: 06/13/2019
ms.openlocfilehash: 15f64c7087ea4d24f271af67b251030a2196fa10
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070365"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Visão geral do Azure SQL Database managed limites de recursos de instância

Este artigo fornece uma visão geral dos limites de recurso para a instância gerenciada do banco de dados SQL e fornece informações sobre como solicitar um aumento nesses limites.

> [!NOTE]
> Para diferenças em recursos suportados e instruções T-SQL, consulte [Diferenças de recursos](sql-database-features.md) e [Suporte à instrução T-SQL](sql-database-managed-instance-transact-sql-information.md).

## <a name="instance-level-resource-limits"></a>Limites de recursos no nível da instância

A instância gerenciada tem características e os limites de recursos que dependem da infraestrutura e a arquitetura subjacente. Os limites dependem da geração de hardware e da camada de serviço.

### <a name="hardware-generation-characteristics"></a>Características de geração de hardware

Instância gerenciada de banco de dados SQL do Azure pode ser implantada nas duas gerações de hardware: Gen4 quanto Gen5. As gerações de hardware têm características diferentes, conforme descrito na tabela a seguir:

|   | **Gen4** | **Gen5** |
| --- | --- | --- |
| Hardware | Processadores Intel E5-2673 v3 (Haswell) de 2,4 GHz, SSD ligado vCore = 1 PP (núcleo físico) | Processadores Intel E5-2673 v4 (Broadwell) 2,3 GHz, SSD NVMe rápido, vCore=1 LP (hyper-thread) |
| vCores | 8, 16, 24 vCores | 4, 8, 16, 24, 32, 40, 64, 80 vCores |
| Memória (taxa de memória/núcleo) | 7 GB por vCore | 5.1 GB por vCore |
| Memória máximo OLTP na memória | Limite de instância: 3 GB por vCore<br/>Limites de banco de dados:<br/> -8-core: 8 GB por banco de dados<br/> -16 núcleos: 20 GB por banco de dados<br/> -24-core: 36 GB por banco de dados | Limite de instância: 2,5 GB por vCore<br/>Limites de banco de dados:<br/> -8-core: 13 GB por banco de dados<br/> -16 núcleos: 32 GB por banco de dados |
| Armazenamento de instância máximo (uso geral) |  8 TB | 8 TB |
| Armazenamento de instância máximo (comercialmente crítico) | 1 TB | 1 TB, 2 TB ou 4 TB, dependendo do número de núcleos |

### <a name="service-tier-characteristics"></a>Características de camada de serviço

A instância gerenciada tem duas camadas de serviço: Uso geral e comercialmente crítico. Essas camadas fornecem recursos diferentes, conforme descrito na tabela a seguir:

| **Recurso** | **Uso geral** | **Comercialmente Crítico** |
| --- | --- | --- |
| Número de vCores\* | Gen4: 8, 16, 24<br/>Gen5: 4, 8, 16, 24, 32, 40, 64, 80 | Gen4: 8, 16, 24, 32 <br/> Gen5: 4, 8, 16, 24, 32, 40, 64, 80 |
| Memória | Gen4: 56 GB - 168 GB (7GB/vCore)<br/>Gen5: 40.8 GB - 408 GB (5.1 GB/vCore) | Gen4: 56 GB - 168 GB (7GB/vCore)<br/>Gen5: 40.8 GB - 408 GB (5.1 GB/vCore) |
| Tamanho máximo de armazenamento de instância | -2 TB para 4 vCores (Gen5)<br/>-8 TB para os outros tamanhos | Gen4: 1 TB <br/> Gen5: <br/>-1 TB para 4, 8, 16 vCores<br/>- 2 TB para 24 vCores<br/>- 4 TB para 32, 40, 64, 80 vCores |
| Armazenamento máximo por banco de dados | Determinado pelo tamanho de armazenamento máximo por instância | Determinado pelo tamanho de armazenamento máximo por instância |
| Número máximo de bancos de dados por instância | 100 | 100 |
| Arquivos de banco de dados máximo por instância | Até 280 | 32\.767 arquivos por banco de dados |
| Dados/Log IOPS (aproximado) | 500 – 7.500 por arquivo<br/>\*[Depende do tamanho do arquivo](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)| 11 mil a 110 mil (1375/vCore) |
| Taxa de transferência de log | 3 MB/s por vCore<br/>Máx de 22 MB/s por instância | 4 MB/s por vCore<br/>Máximo de 48 MB/s por instância|
| Taxa de transferência de dados (aproximada) | 100 – 250 MB/s por arquivo<br/>\*[Depende do tamanho do arquivo](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes) | N/D |
| Latência de E/S (aproximada) | 5-10 ms | 1-2 ms |
| Tamanho máximo de TempDB | 192 – 1.920 GB (24 GB por vCore) | Não há restrições - limitadas pelo tamanho de armazenamento de instância máxima |
| Número máximo de sessões | 30000 | 30000 |

> [!NOTE]
> - O tamanho do arquivo de log e de dados nos bancos de dados de sistema e de usuário são incluídos no tamanho de armazenamento de instância que é comparado ao limite de tamanho de armazenamento máximo. Usar a exibição do sistema <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys. master_files</a> para determinar o total o espaço usado pelos bancos de dados. Logs de erros não são persistentes e não são incluídos no tamanho. Backups não são incluídos no tamanho de armazenamento.
> - IOPS e taxa de transferência também dependem do tamanho de página que não é explicitamente limitado pela instância gerenciada.

## <a name="supported-regions"></a>Regiões com suporte

Instâncias gerenciadas podem ser criadas somente no [regiões com suporte](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Para criar uma instância gerenciada em uma região que não tem suporte atualmente, você pode [enviar uma solicitação de suporte por meio do portal do Azure](#obtaining-a-larger-quota-for-sql-managed-instance).

## <a name="supported-subscription-types"></a>Tipos de assinatura com suporte

A instância gerenciada atualmente dá suporte à implantação apenas nos seguintes tipos de assinaturas:

- [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Pague-conforme-o-uso](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Provedor de serviços de nuvem (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)
- [Desenvolvimento/Teste Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Desenvolvimento/Teste pré-pago](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Assinaturas com crédito Azure mensal para assinantes do Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)

## <a name="regional-resource-limitations"></a>Limitações de recursos regionais

Os tipos de assinatura suportados podem conter um número limitado de recursos por região. A instância gerenciada tem dois limites padrão por região do Azure, dependendo de um tipo do tipo de assinatura:

- **Limite de sub-rede**: O número máximo de sub-redes nas quais as instâncias gerenciadas são implantadas em uma única região.
- **limite de vCore**: O número máximo de vCores que pode ser implantado em todas as instâncias em uma única região.

> [!Note]
> Esses limites são as configurações padrão e limitações não técnicas. Os limites podem ser aumento sob demanda, criando um especial [solicitação de suporte no portal do Azure](#obtaining-a-larger-quota-for-sql-managed-instance) se precisar de mais instâncias gerenciadas na região atual. Como alternativa, você pode criar novas instâncias gerenciadas em outra região do Azure sem enviar solicitações de suporte.

A tabela a seguir mostra os limites regionais de padrão para assinaturas com suporte:

|Tipo de assinatura| Número máximo de sub-redes da instância gerenciada | Número máximo de unidades de vCore * |
| :---| :--- | :--- |
|Pago conforme o uso|3|320|
|CSP |8 (15 em algumas regiões * *)|960 (1440 em algumas regiões * *)|
|Desenvolvimento/Teste pago conforme o uso|3|320|
|Desenvolvimento/Teste Enterprise|3|320|
|EA|8 (15 em algumas regiões * *)|960 (1440 em algumas regiões * *)|
|Visual Studio Enterprise|2 |64|
|Visual Studio Professional e MSDN Platforms|2|32|

\* Ao planejar suas implantações, considere que um vCore de negócios críticos (BC) (devido à redundância adicionado) consome 4 vezes mais capacidade de um vCore de finalidade geral (GP). Portanto, em seus cálculos, 1 vCore GP = 1 vCore unidade e BC 1 vCore = 4 unidades de vCore. Para simplificar sua análise de consumo com os limites padrão, resuma as unidades de vCore em todas as sub-redes na região onde as instâncias gerenciadas são implantadas e comparam os resultados com os limites de unidade de instância para o tipo de assinatura. **Número máximo de unidades de vCore** limite se aplica a cada assinatura em uma região. Há nenhum limite por sub-redes individuais, exceto que a soma de todos os vCores implantado em várias sub-redes deve ser menor ou igual a **número máximo de unidades de vCore**.

\* * Limites de sub-rede e vCore maiores estão disponíveis nas seguintes regiões: Leste da Austrália, Leste dos EUA, Leste dos EUA 2, Europa Setentrional, Centro-Sul dos EUA, Sudeste Asiático, UK Sul, Europa Ocidental, oeste dos EUA 2.

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>Como obter uma cota maior para o SQL instância gerenciada

Se você precisar de mais instâncias gerenciadas em suas regiões atuais, envie uma solicitação de suporte para estender a cota usando o portal do Azure.
Para iniciar o processo de obtenção de uma cota maior:

1. Abra **ajuda + suporte**e clique em **nova solicitação de suporte**.

   ![Ajuda e Suporte](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Na guia Básico da nova solicitação de suporte:
   - Para **tipo de problema**, selecione **Limites de serviço e assinatura (cotas)** .
   - Em **Assinatura**, selecione sua assinatura.
   - Para **tipo de cota**, selecione **instância gerenciada do banco de dados do SQL**.
   - Para **plano de suporte**, selecione o plano de suporte.

     ![Cota de tipo de problema](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. Clique em **Avançar**.
4. Sobre o **guia de problema** para a nova solicitação de suporte:
   - Para **Gravidade**, selecione o nível de gravidade do problema.
   - Para **Detalhes**, forneça informações adicionais sobre o seu problema, incluindo mensagens de erro.
   - Para **Upload de arquivo**, anexe um arquivo com mais informações (até 4 MB).

     ![Detalhes do problema](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Uma solicitação válida deve incluir:
     > - Região na qual assinatura limite precisa ser aumentado.
     > - Aumenta o número necessário de vCores por camada de serviço em sub-redes existentes após a cota (se nenhuma das sub-redes existentes precisa ser expandido.
     > - Necessário o número de novas sub-redes e o número total de vCores por camada de serviço dentro de novas sub-redes (se você precisa implantar instâncias gerenciadas em novas sub-redes).

5. Clique em **Avançar**.
6. Na guia Informações de contato da nova solicitação de suporte, insira o método de contato preferencial (email ou telefone) e os detalhes de contato.
7. Clique em **Criar**.

## <a name="next-steps"></a>Próximas etapas

- Para obter mais informações sobre a instância gerenciada, consulte [o que é uma instância gerenciada?](sql-database-managed-instance.md).
- Para saber mais sobre preços, veja [Preços de instância gerenciada do Banco de Dados SQL](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- Para saber como criar sua primeira instância gerenciada, consulte [o guia de início rápido](sql-database-managed-instance-get-started.md).

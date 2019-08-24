---
title: Limites de recursos e objetos do Azure Analysis Services | Microsoft Docs
description: Descreve os limites de recursos e objetos do Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 08/23/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 40a5b68a12724f2574af19bb10c276c54c5afba0
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997596"
---
# <a name="analysis-services-resource-and-object-limits"></a>Limites de recurso e objeto do Analysis Services

Este artigo descreve os limites de objeto de recurso e modelo.

## <a name="tier-limits"></a>Limites de camada

Para obter limites de QPU e memória para as camadas de desenvolvedor, básica e Standard, consulte a [página de preços Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="object-limits"></a>Limites do objeto

Esses limites são teóricos. Desempenho será ser diminuído em números mais baixos.

|Object|Tamanho máximo/números|  
|------------|----------------------------|  
|Bancos de dados em uma instância|16,000|  
|Número combinado de tabelas e colunas em um banco de dados|16,000|  
|Linhas em uma tabela|Ilimitado<br /><br /> **Aviso:** Com a restrição de que nenhuma coluna única na tabela pode ter mais de 1.999.999.997 valores distintos.|  
|Hierarquias em uma tabela|15,999|  
|Níveis em uma hierarquia|15,999|  
|Relações|8\.000|  
|Colunas de chave na tabela de todos os|15,999|  
|Medidas em tabelas|2^31-1 = 2,147,483,647|  
|Células retornadas por uma consulta|2 ^ 31-1 = 2.147.483.647|  
|Tamanho do registro de consulta de fonte|64 k|  
|Comprimento de nomes de objeto|512 caracteres|  



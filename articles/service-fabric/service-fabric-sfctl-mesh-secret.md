---
title: CLI do Azure Service Fabric – sfctl mesh secret | Microsoft Docs
description: Descreve os comandos sfctl mesh secret da CLI do Service Fabric.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 5e58e6a401cc166c176dc465d58ba9e8a8ed83b0
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035968"
---
# <a name="sfctl-mesh-secret"></a>sfctl mesh secret
Obter e excluir recursos de segredo da malha.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| delete | Exclui o recurso de segredo. |
| lista | Lista todos os recursos de segredo. |
| mostrar | Obtém o recurso de segredo com o nome fornecido. |

## <a name="sfctl-mesh-secret-delete"></a>sfctl mesh secret delete
Exclui o recurso de segredo.

Exclui o recurso de segredo especificado e todos os seus valores nomeados.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --name -n [Obrigatório] | O nome do segredo. |

### <a name="global-arguments"></a>Argumentos globais

|Argumento|DESCRIÇÃO|
| --- | --- |
| --debug | Aumentar o nível de detalhes do log para mostrar todos os logs de depuração. |
| --help -h | Mostrar esta mensagem de ajuda e sair. |
| --output -o | O formato da saída.  Valores permitidos\: json, jsonc, table, tsv.  Padrão\: json. |
| --query | Cadeia de caracteres de consulta JMESPath. Veja http\://jmespath.org/ para saber mais e obter exemplos. |
| --verbose | Aumentar o nível de detalhes do log. Use --debug para logs de depuração completos. |

## <a name="sfctl-mesh-secret-list"></a>sfctl mesh secret list
Lista todos os recursos de segredo.

Obtém as informações sobre todos os recursos de segredo em um determinado grupo de recursos. As informações incluem a descrição e outras propriedades do segredo.

### <a name="global-arguments"></a>Argumentos globais

|Argumento|DESCRIÇÃO|
| --- | --- |
| --debug | Aumentar o nível de detalhes do log para mostrar todos os logs de depuração. |
| --help -h | Mostrar esta mensagem de ajuda e sair. |
| --output -o | O formato da saída.  Valores permitidos\: json, jsonc, table, tsv.  Padrão\: json. |
| --query | Cadeia de caracteres de consulta JMESPath. Veja http\://jmespath.org/ para saber mais e obter exemplos. |
| --verbose | Aumentar o nível de detalhes do log. Use --debug para logs de depuração completos. |

## <a name="sfctl-mesh-secret-show"></a>sfctl mesh secret show
Obtém o recurso de segredo com o nome fornecido.

Obtém as informações sobre o recurso de segredo com o nome fornecido. As informações incluem a descrição e outras propriedades do segredo.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --name -n [Obrigatório] | O nome do segredo. |

### <a name="global-arguments"></a>Argumentos globais

|Argumento|DESCRIÇÃO|
| --- | --- |
| --debug | Aumentar o nível de detalhes do log para mostrar todos os logs de depuração. |
| --help -h | Mostrar esta mensagem de ajuda e sair. |
| --output -o | O formato da saída.  Valores permitidos\: json, jsonc, table, tsv.  Padrão\: json. |
| --query | Cadeia de caracteres de consulta JMESPath. Veja http\://jmespath.org/ para saber mais e obter exemplos. |
| --verbose | Aumentar o nível de detalhes do log. Use --debug para logs de depuração completos. |


## <a name="next-steps"></a>Próximas etapas
- [Configurar](service-fabric-cli.md) a CLI do Service Fabric.
- Saiba como usar a CLI do Service Fabric usando os [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).
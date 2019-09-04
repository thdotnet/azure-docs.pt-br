---
title: Instalar pacotes em um Jupyter Notebook no Azure
description: Como instalar pacotes Python, R e F# de dentro de um Jupyter Notebook em execução no Azure.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 6f089c12-128b-4dbd-96e3-1320d37eeba4
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: b0881cb6dac9ec83d2126942c758508e760f9c83
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274432"
---
# <a name="install-packages-from-within-a-notebook"></a>Instalar pacotes de dentro de um notebook

Embora você possa configurar o [ambiente para o notebook no nível do projeto](configure-manage-azure-notebooks-projects.md#configure-the-project-environment), talvez você queira instalar pacotes diretamente dentro de um notebook individual.

Os pacotes instalados do notebook aplicam-se apenas à sessão atual do servidor. As instalações de pacote não persistem quando o servidor é desligado.

## <a name="python"></a>Python

Os pacotes em Python podem ser instalados usando o pip ou o conda com comandos dentro de células de código:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Se a saída do comando indicar que o requisito já for atendido, os Azure Notebooks poderão incluir o pacote por padrão. O pacote também pode ser instalado por meio de uma [etapa de configuração do ambiente do projeto](configure-manage-azure-notebooks-projects.md#configure-the-project-environment).

## <a name="r"></a>R

Os pacotes em R podem ser instalados do CRAN ou do GitHub usando a função `install.packages` em uma célula de código:

```r
install.packages("package_name")
```

Você também pode instalar versões de pré-lançamento e outros pacotes de desenvolvimento do GitHub usando a biblioteca devtools:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

Os pacotes em F# podem ser instalados do [nuget.org](https://www.nuget.org) chamando o gerenciador de dependências Paket de dentro das células de código. Primeiro, carregue o gerenciador Paket:

```fsharp
#load "Paket.fsx"
```

Em seguida, instale os pacotes:

```fsharp
Paket.Package
  [ "MathNet.Numerics"
    "MathNet.Numerics.FSharp"
  ]
```

Em seguida, carregue o gerador de paket:
```fsharp
#load "Paket.Generated.Refs.fsx"
```

Abra o biblioteca:
```fsharp
open MathNet.Numerics
```

## <a name="next-steps"></a>Próximas etapas

- [Como: Configurar e gerenciar projetos](configure-manage-azure-notebooks-projects.md)
- [Como: exibir uma apresentação de slides](present-jupyter-notebooks-slideshow.md)

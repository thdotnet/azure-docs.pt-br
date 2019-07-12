---
title: Imagens de marca e a versão no registro de contêiner do Azure
description: Práticas recomendadas para a marcação e controle de versão do Docker imagens de contêiner
services: container-registry
author: stevelasker
ms.service: container-registry
ms.topic: article
ms.date: 07/10/2019
ms.author: steve.lasker
ms.openlocfilehash: bd00fd4f8dd247c766eb34849ecf9de603c5171b
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800387"
---
# <a name="recommendations-for-tagging-and-versioning-container-images"></a>Recomendações para as imagens de contêiner de marcação e controle de versão

Ao enviar a implantação de imagens de contêiner para um registro de contêiner e, em seguida, implantá-los, você precisa de uma estratégia de marcação de imagem e o controle de versão. Este artigo aborda duas abordagens e onde cada um se encaixa durante o ciclo de vida do contêiner:

* **Marcas de estável** -marcas que você reutiliza, por exemplo, para indicar a principal ou, como a versão secundária *mycontainerimage:1.0*.
* **Marcas exclusivas** -uma marca diferente para cada imagem que você enviar por push a um registro, como *mycontainerimage:abc123*.

## <a name="stable-tags"></a>Marcas estáveis

**Recomendação**: Usar marcas estáveis para manter **imagens base** para compilações de seu contêiner. Evite implantações com marcas estáveis, pois essas marcas continuam a receber atualizações e podem introduzir inconsistências em ambientes de produção.

*Estável marcas* significa que um desenvolvedor ou um sistema de compilação pode continuar a efetuar pull de uma marca específica, que continua para obter atualizações. Estável não significa que o conteúdo está congelado. Em vez disso, estável implica que a imagem deve ser estável para a intenção dessa versão. Para se manter "estáveis", podem ser atendida para aplicar patches de segurança ou atualizações do framework.

### <a name="example"></a>Exemplo

Uma equipe do framework versão 1.0 é fornecido. Eles sabem que irá enviar atualizações, incluindo atualizações secundárias. Para dar suporte a marcas estáveis para uma determinada versão principal e secundária, eles têm dois conjuntos de marcas estáveis.

* `:1` – uma marca estável para a versão principal. `1` representa a versão 1. * "mais recente" ou "mais recente".
* `:1.0`-uma marca estável para a versão 1.0, permitindo que um desenvolvedor associar às atualizações de 1.0 e não ser rolado para frente até 1.1 quando ele é lançado.

A equipe também usa o `:latest` marca, que aponta para a marca estável mais recente, não importa qual a versão principal atual.

Quando as atualizações da imagem base estão disponíveis ou qualquer tipo de versão do framework, imagens com as marcas estáveis de serviço são atualizados para a compilação mais recente que representa a versão estável mais recente dessa versão.

Nesse caso, as marcas principais e secundárias são continuamente sendo atendidas. De um cenário de imagem base, isso permite que o proprietário de imagem para fornecer imagens atendidas.

## <a name="unique-tags"></a>Marcas exclusivas

**Recomendação**: Usar marcas exclusivas para o **implantações**, especialmente em um ambiente que pode ser dimensionado em vários nós. Você provavelmente deseja implantações deliberadas de uma versão consistente dos componentes. Se seu contêiner será reinicializado ou um orquestrador pode ser dimensionado mais instâncias, os hosts acidentalmente não efetuar pull de uma versão mais recente, inconsistente com os outros nós.

Marcação exclusiva simplesmente significa que cada imagem enviadas por push para um registro tem uma marca exclusiva. As marcas não são reutilizadas. Há vários padrões que você pode seguir para gerar marcas exclusivas, incluindo:

* **Carimbo de data / hora** -essa abordagem é bastante comum, uma vez que você pode informar claramente aos quando a imagem foi criada. Mas, como correlacioná-los de volta para seu sistema de compilação? Você tem que localizar a compilação foi concluída ao mesmo tempo? Você está que fuso horário? Todos os seus sistemas de compilação são calibrados para UTC?
* **Confirmação de Git** – essa abordagem funciona até você começar a dar suporte a atualizações da imagem base. Se ocorrer uma atualização de imagem base, o sistema de compilação inicia com a mesma confirmação de Git como a compilação anterior. No entanto, a imagem base tem conteúdo novo. Em geral, uma confirmação de Git fornece um *semi*-estável marca.
* **Manifesto digest** -cada imagem de contêiner enviados por push para um registro de contêiner está associada com um manifesto, identificado por um hash SHA-256 exclusivo ou digest. Enquanto estiver exclusivo, o resumo é correlacionada com seu ambiente de compilação longos e difíceis de ler.
* **A ID de compilação** -esta opção pode ser melhor, pois é provável incremental, e permite que você correlacione volta para a compilação específica para encontrar todos os logs e artefatos. No entanto, como um resumo de manifesto, pode ser difícil para leitura.

  Se sua organização tiver vários sistemas de compilação, prefixando a marca com o nome do sistema de compilação é uma variação sobre essa opção: `<build-system>-<build-id>`. Por exemplo, você pode diferenciar compilações Jenkins da equipe API sistema de compilação e sistema de compilação do Azure Pipelines da equipe da web.

## <a name="next-steps"></a>Próximas etapas

Para obter uma discussão mais detalhada sobre os conceitos neste artigo, consulte o postagem no blog [marcação do Docker: Práticas recomendadas para a marcação e controle de versão de imagens do docker](https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/).

Para ajudar a maximizar o desempenho e o uso econômico de seu registro de contêiner do Azure, consulte [práticas recomendadas para o registro de contêiner do Azure](container-registry-best-practices.md).

<!-- IMAGES -->


<!-- LINKS - Internal -->


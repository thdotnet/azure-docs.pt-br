---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 98d765e2f6909f00f8dfe76d06aef017aad67adf
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71174925"
---
## <a name="terminology"></a>Terminologia

Uma imagem do Marketplace no Azure tem os seguintes atributos:

* **Editor**: A organização que criou a imagem. Exemplos: Canonical, MicrosoftWindowsServer
* **Oferta**: O nome de um grupo de imagens relacionadas criadas por um editor. Exemplos: UbuntuServer, WindowsServer
* **SKU**: Uma instância de uma oferta, como uma versão principal de uma distribuição. Exemplos: 18.04-LTS, 2019-Datacenter
* **Versão**: O número da versão de uma SKU da imagem. 

Para identificar uma imagem do Marketplace quando implantar uma VM de forma programática, forneça esses valores individualmente como parâmetros, ou algumas ferramentas aceitam a imagem URN. Algumas ferramentas aceitam uma imagem *URN*, que combina esses valores, separados pelo caractere de dois-pontos (:): *Editor*:*Oferta*:*Sku*:*Versão*. Em uma URN, substitua o número de versão por "mais recente", o que seleciona a versão mais recente da imagem. 

Se o editor de imagens fornecer licenças adicionais e termos de compra, você deverá aceitar esses termos e ativar a implantação programática. Você também precisará fornecer *parâmetros do plano de compra* ao implantar uma VM programaticamente. Consulte [Implantar uma imagem com termos do Marketplace](#deploy-an-image-with-marketplace-terms).

---
title: Notas de versão do Microsoft Azure StorSimple Virtual Array atualização 1.2 | Microsoft Docs
description: Descreve problemas em aberto críticos e resoluções para a StorSimple Virtual Array que executa a atualização 1.2.
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 05/29/2019
ms.author: alkohli
ms.openlocfilehash: ea7e4801dfaad533403c0f927a03735ae409cc52
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66420597"
---
# <a name="storsimple-virtual-array-update-12-release-notes"></a>Notas de versão da atualização 1.2 do StorSimple Virtual Array

As notas de versão a seguir identificam as questões críticas em aberto e os problemas resolvidos para as atualizações do Microsoft Azure StorSimple Virtual Array.

As notas de versão são atualizadas continuamente. Conforme são descobertas questões críticas que exijam uma solução alternativa, elas são adicionadas. Antes de implantar o StorSimple Virtual Array, examine cuidadosamente as informações contidas nas notas de versão.

A atualização 1.2 corresponde à versão de software **10.0.10311.0**.

> [!IMPORTANT]
> - As atualizações causam interrupção e reiniciam seu dispositivo. Se processos de E/S estiverem em andamento, o dispositivo sofrerá tempo de inatividade. Para obter instruções detalhadas sobre os pacotes usados para aplicar essa atualização, acesse [baixar atualização 1.2](#download-update-12).
> - A atualização 1.2 está disponível para você por meio do portal do Azure, apenas se o dispositivo está executando a atualização 1.0 ou 1.1.

## <a name="whats-new-in-update-12"></a>Novidades na Atualização 1.2

Esta atualização contém as seguintes correções de bug:

- Melhor resiliência durante o processamento de arquivos excluídos.
- Melhor manipulação de exceções no caminho de inicialização do código, resultando em menos falhas em backups, restaurar, leituras de nuvem e automatizado de reclamação de espaço.

## <a name="download-update-12"></a>Baixe a atualização 1.2

Para baixar essa atualização, vá para o [catálogo do Microsoft Update](https://www.catalog.update.microsoft.com/Home.aspx) server e o pacote de download de KB4502035. Este pacote contém os seguintes pacotes:

 - **KB4493446** que contém as atualizações cumulativas do Windows 2012 R2 até abril de 2019. Para obter mais informações sobre o que está incluído neste pacote cumulativo de atualizações, acesse [rollup de segurança mensal de abril](https://support.microsoft.com/help/4493446/windows-8-1-update-kb4493446).
 - **KB3011067** que é um arquivo de pacote autônomo do Microsoft Update WindowsTH-KB3011067-x64. Esse arquivo é usado para atualizar o software do dispositivo.

Baixe KB4502035 e siga estas instruções para [aplique a atualização por meio da interface do usuário da web local](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="issues-fixed-in-update-12"></a>Problemas corrigidos na Atualização 1.2

A tabela a seguir fornece um resumo dos problemas corrigidos nesta versão.

| Nº | Recurso | Problema |
| --- | --- | --- |
| 1 |Exclusão| Nas versões anteriores do software, houve um problema quando o uso do dispositivo não tiver alterado mesmo quando os arquivos foram excluídos. Esse problema foi corrigido nesta versão. Caminho de código em camadas foi feito mais resiliente durante o processamento de arquivos excluídos.|
| 2 |Manipulação de exceção| Nas versões anteriores do software, houve um problema após a reinicialização do sistema que poderia levar a falhas em backups, restauração, lendo a nuvem e automatizado de reclamação de espaço. Esta versão contém alterações sobre como as exceções eram tratadas no caminho da inicialização.|

## <a name="known-issues-in-update-12"></a>Problemas conhecidos na Atualização 1.2

Nenhum novo problema foram observados na atualização 1.2. Todos os problemas observados são transferidos de versões anteriores. Para ver o resumo dos problemas conhecidos incluídos nas versões anteriores, acesse [problemas conhecidos na atualização 1.1](storsimple-virtual-array-update-11-release-notes.md#known-issues-in-update-11).

## <a name="next-steps"></a>Próximas etapas

Baixar KB4502035 e [aplique a atualização por meio da interface do usuário da web local](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="references"></a>Referências

Procurando uma nota de versão mais antiga? Acesse:
* [Notas de versão 1.1 de atualização do StorSimple Virtual Array](storsimple-virtual-array-update-11-release-notes.md)
* [Notas de versão do Update 1.0 do Array Virtual StorSimple](storsimple-virtual-array-update-1-release-notes.md)
* [Notas de versão da atualização 0.6 da StorSimple Virtual Array](storsimple-virtual-array-update-06-release-notes.md)
* [Notas de versão da Atualização 0.5 do StorSimple Virtual Array](storsimple-virtual-array-update-05-release-notes.md)
* [Notas de versão da Matriz Virtual do StorSimple Atualização 0.4](storsimple-virtual-array-update-04-release-notes.md)
* [Notas de versão da atualização 0.3 da StorSimple Virtual Array](storsimple-ova-update-03-release-notes.md)
* [Notas de versão as Atualizações 0.1 e 0.2 do StorSimple Virtual Array](storsimple-ova-update-01-release-notes.md)
* [Notas de versão de disponibilidade geral do StorSimple Virtual Array](storsimple-ova-pp-release-notes.md)

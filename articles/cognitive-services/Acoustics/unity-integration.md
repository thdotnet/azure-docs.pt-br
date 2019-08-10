---
title: Integração e implantação do Unity acústicas do projeto
titlesuffix: Azure Cognitive Services
description: Este "como" explica a integração do plug-in de Unity do acústica do projeto em seu projeto do Unity.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 54bc98e0ddba0292c6a5dbb07f2bbdfce6a1cb45
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933155"
---
# <a name="project-acoustics-unity-integration"></a>Integração do Unity da acústica do projeto
Este "como" explica a integração do plug-in de Unity do acústica do projeto em seu projeto do Unity.

Requisitos de software:
* [Unity 2018.2+](https://unity3d.com) para Windows
* [Pacote de Unity do projeto acústicos](https://www.microsoft.com/download/details.aspx?id=57346)

## <a name="import-the-plugin"></a>O plug-in de importação
Importe a acústica UnityPackage para o seu projeto. 
* No Unity, vá para **ativos > Importar pacote > pacote personalizado...**

    ![Captura de tela do menu do pacote de importação do Unity](media/import-package.png)  

* Escolha **ProjectAcoustics.unitypackage**

* Clique no botão **importar** para integrar o pacote do Unity ao seu projeto  

    ![Captura de tela da caixa de diálogo Importar pacote do Unity](media/import-dialog.png)  

Se você estiver importando o plug-in para um projeto existente, seu projeto já pode ter um arquivo **mcs.rsp** na raiz do projeto, que especifica opções para o compilador C#. Você precisará mesclar o conteúdo desse arquivo com o arquivo de MCS que vem com o plug-in do projeto acústica.

## <a name="enable-the-plugin"></a>Habilitar o plug-in
A parte de assar do kit de ferramentas de acústica requer a versão de tempo de execução de script do .NET 4.x. A importação de pacotes atualizará as configurações do player do Unity. Reinicialização do Unity para essa configuração tenha efeito.

![Captura de tela do painel Configurações do Player do Unity](media/player-settings.png)

![Captura de tela do painel Configurações do Player do Unity com .NET 4.5 selecionado](media/net45.png)

## <a name="set-up-audio-dsp"></a>Configurar o DSP de áudio
O projeto acústica inclui o DSP de tempo de execução de áudio que se integra à estrutura spatializer do mecanismo de áudio do Unity. Ele inclui a espaciaiização com base em HRTF e panorâmica. Habilite o DSP do projeto acústica abrindo as configurações de áudio do Unity usando **editar > configurações do projeto > áudio**e, em seguida, selecionando o **projeto acústica** como o **plug-in Spatializer** para o seu projeto. Verifique se o **tamanho do buffer do DSP** está definido como o melhor desempenho.

![Captura de tela do painel de configurações do projeto do Unity](media/project-settings.png)  

![Captura de tela do painel de configurações do Unity Spatializer com o projeto acústicos Spatializer selecionados](media/choose-spatializer.png)

Em seguida, abra o mixer de áudio (**janela > mixer de áudio**). Certifique-se de ter pelo menos um Mixer, com um grupo. Se você não fizer isso, clique no botão "+" à direita de **Mixers**. Clique com o botão direito do mouse na parte inferior da faixa do canal na seção de efeitos e adicione o efeito de mixer do misturador acústicos do **projeto** . Observe que apenas um Mixer acústica de projeto é suportado por vez.

![Captura de tela do mixer de áudio do Unity que hospeda o mixer acústicos do projeto](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Habilitar acústicos em fontes de som
Crie uma fonte de áudio. Clique na caixa de seleção na parte inferior do painel de controle do AudioSource que diz **Spatialize**. Certifique-se **Blend espacial** é definido como completo 3D.  

![Captura de tela do painel de origem de áudio do Unity](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Habilitar Design acústico
Anexe o script **AcousticsAdjust** a uma fonte de som em sua cena para habilitar parâmetros de design de origem adicionais, clicando em **Adicionar componente** e escolhendo **scripts > acústicos ajustar**:

![Captura de tela do script AcousticsAdjust do Unity](media/acoustics-adjust.png)

## <a name="next-steps"></a>Próximas etapas
* [Distortar sua cena com o projeto acústicos para o Unity](unity-baking.md)
* [Criar uma conta do lote do Azure](create-azure-account.md) para entortar sua cena na nuvem
* Explore o [processo de design da unidade acústica do projeto](unity-workflow.md).


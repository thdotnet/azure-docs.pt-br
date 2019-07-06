---
title: Solucionar problemas de dispositivos de fala do SDK - serviços de fala
titleSuffix: Azure Cognitive Services
description: Este artigo fornece informações para ajudar a resolver problemas que você pode encontrar ao usar o SDK de dispositivos de fala.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wellsi
ms.openlocfilehash: f55171a177dfcbebb9bc6df5ce125a8f29494946
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606295"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Solução de problemas do SDK de Dispositivos de Fala

Este artigo fornece informações para ajudar a resolver problemas que você pode encontrar ao usar o SDK de dispositivos de fala.

## <a name="certificate-failures"></a>Falhas de certificado

Se você receber falhas de certificado ao usar os serviços de fala, certifique-se de que seu dispositivo tenha a data e hora corretas:

1. Vá para **Configurações**. Em **Sistema**, selecione **Data e hora**.

    ![Em Configurações, selecione a Data e hora](media/speech-devices-sdk/qsg-12.png)

1. Mantenha a opção selecionada **Data e hora automática**. Em **Selecionar fuso horário**, selecione seu fuso horário atual.

    ![Selecione as opções de data e fuso horário](media/speech-devices-sdk/qsg-13.png)

    Quando você vir que o tempo do kit de desenvolvimento corresponde o tempo em seu computador, o kit de desenvolvimento estará conectado à internet.

## <a name="next-steps"></a>Próximas etapas

* [Examinar as notas de versão](devices-sdk-release-notes.md)

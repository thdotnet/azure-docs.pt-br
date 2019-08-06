---
title: Habilitar o serviço Central de Segurança do Azure para IoT no Hub IoT | Microsoft Docs
description: Saiba como habilitar o serviço Central de Segurança do Azure para IoT em seu Hub IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/16/2019
ms.author: mlottner
ms.openlocfilehash: a794ccea13323f38b20906458e216f85652bfc3e
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596952"
---
# <a name="quickstart-onboard-azure-security-center-for-iot-service-in-iot-hub"></a>Início Rápido: Integrar o serviço Central de Segurança do Azure para IoT no Hub IoT

Este artigo oferece uma explicação de como habilitar o serviço Central de Segurança do Azure para IoT em seu Hub IoT existente. Se você não tiver, no momento, um Hub IoT, confira [Criar um Hub IoT usando o portal do Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) para começar a usar. 

> [!NOTE]
> No momento, a Central de Segurança do Azure para IoT só é compatível com a camada standard de Hubs IoT.
> A Central de Segurança do Azure para IoT é uma solução de hub único. Se você exigir vários hubs, serão necessárias várias soluções da Central de Segurança do Azure para IoT. 

## <a name="prerequisites-for-enabling-the-service"></a>Pré-requisitos para habilitar o serviço

- Espaço de trabalho do Log Analytics
  - Dois tipos de informação são armazenados por padrão no workspace do Log Analytics pela Central de Segurança do Azure para IoT; **alertas de segurança** e **recomendações**. 
  - Você pode optar por adicionar armazenamento de um tipo de informações adicionais **eventos brutos**. Observe que armazenar **eventos brutos** no Log Analytics transporta os custos de armazenamento adicionais. 
- Hub IoT (camada standard)
- Atenda a todos os [pré-requisitos de serviço](service-prerequisites.md) 

|Regiões de serviço do Azure com suporte | ||
|---|---|---|
| Centro dos EUA |Leste dos EUA |Leste dos EUA 2 |
| Centro-Oeste dos EUA |Oeste dos EUA |Oeste dos EUA 2 |
| Centro-Sul dos EUA|Centro-Norte dos EUA | Canadá Central|
| Leste do Canadá| Norte da Europa|Sul do Brasil|
| França Central| Oeste do Reino Unido|Sul do Reino Unido|
|Europa Ocidental|Norte da Europa| Oeste do Japão|
|Leste do Japão | Sudeste da Austrália|Leste da Austrália|
|Ásia Oriental| Sudeste Asiático| Coreia Central|
|Sul da Coreia| Índia Central| Sul da Índia|
|

## <a name="enable-azure-security-center-for-iot-on-your-iot-hub"></a>Habilitar a Central de Segurança do Azure para IoT em seu Hub IoT 

Para habilitar perguntas de segurança no seu Hub IoT, faça o seguinte: 

1. Abra o **Hub IoT** no portal do Azure. 
1. No menu **Segurança**, clique em **Proteger sua solução IoT**
1. Deixe **Habilitar** selecionado como o padrão. 
1. Selecione o workspace do Log Analytics.
1. Forneça os detalhes do workspace do Log Analytics. 
   - Opte por habilitar **coleção de gêmeos**, deixando a **coleção de gêmeos** alternada em **Ativar**.
   - Opte por armazenar **eventos brutos** além dos tipos de informações padrão de armazenamento selecionando **Armazenar eventos brutos de segurança do dispositivo** no Log Analytics. Deixe a alternância de **evento bruto** **Ativada**. 
    
1. Clique em **Save** (Salvar). 

Parabéns! Você concluiu a habilitação da Central de Segurança do Azure para IoT em seu Hub IoT. 

## <a name="next-steps"></a>Próximas etapas

Passe para o próximo artigo para configurar sua solução...

> [!div class="nextstepaction"]
> [Configurar sua solução](quickstart-configure-your-solution.md)



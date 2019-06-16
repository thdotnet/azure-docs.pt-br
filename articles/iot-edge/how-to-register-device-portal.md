---
title: Registrar um novo dispositivo no portal do Azure – Azure IoT Edge | Microsoft Docs
description: Use o portal do Azure para registrar um novo dispositivo IoT Edge e recuperar a cadeia de conexão
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 16660fbed465cc70f16cde430024f33b8aa4350e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66495354"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Registrar um novo dispositivo do Azure IoT Edge com portal do Azure

Antes de poder usar os dispositivos do IoT com Azure IoT Edge, será necessário registrá-los no hub IoT. Quando você registra um dispositivo, você receberá uma cadeia de caracteres de conexão que pode ser usada para configurar seu dispositivo para cargas de trabalho de IoT Edge.

Este artigo descreve como registrar um novo dispositivo do IoT Edge usando portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Gratuito para standard [hub IoT](../iot-hub/iot-hub-create-through-portal.md) na sua assinatura do Azure.

## <a name="create-a-device"></a>Criar um dispositivo

No portal do Azure, dispositivos de extremidade IoT são criados e gerenciados separadamente dos dispositivos que se conectar ao seu hub IoT, mas não habilitado de borda.

1. Entre no [Portal do Azure](https://portal.azure.com) e navegue até o Hub IoT.
2. Selecionar **IoT Edge** do menu.
3. Selecione **Adicionar um dispositivo IoT Edge**.
4. Forneça um ID de dispositivo descritivo. Use as configurações padrão para gerar automaticamente as chaves de autenticação e conectar o novo dispositivo ao hub.
5. Clique em **Salvar**.

## <a name="view-all-devices"></a>Exibir todos os dispositivos

Todos os habilitado borda dispositivos que se conectam ao seu hub IoT são listados no **IoT Edge** página.

## <a name="retrieve-the-connection-string"></a>Recuperar a cadeia de conexão

Quando estiver pronto para configurar o dispositivo, você precisará da cadeia de conexão que vincula o dispositivo físico à identidade no hub IoT.

1. Dos **do IoT Edge** no portal, clique na ID do dispositivo na lista de dispositivos do IoT Edge.
2. Copie o valor da **Cadeia de conexão (chave primária)** ou **Cadeia de conexão (chave secundária)** .

## <a name="next-steps"></a>Próximas etapas

Saiba como [implantar módulos em um dispositivo com o portal do Azure](how-to-deploy-modules-portal.md).

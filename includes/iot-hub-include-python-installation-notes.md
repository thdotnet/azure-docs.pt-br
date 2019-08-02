---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 07/24/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 103ad020b8d9f50ffe690f502b7cac08dab9237a
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640437"
---
* Uma conta ativa do Azure. (Se você não tiver uma conta, poderá criar uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) em apenas alguns minutos.)

* **Windows**

    * [Python 2. x ou 3. x](https://www.python.org/downloads/). Certifique-se de usar a instalação de 32 bits ou 64 bits conforme exigido pelo seu programa de instalação. Quando solicitado durante a instalação, certifique-se de adicionar Python à variável de ambiente específica da plataforma. Se você estiver usando o Python 2.x, talvez seja necessário [instalar ou atualizar o *pip*, o sistema de gerenciamento de pacotes do Python](https://pip.pypa.io/en/stable/installing/).

    * Se você estiver usando o sistema operacional Windows, verifique se você tem a versão correta [do C++ pacote redistribuível Visual](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) para permitir o uso de DLLs nativas do Python. Recomendamos que você use a versão mais recente.

    * Se necessário, instale o pacote [Azure-iothub-Device-Client](https://pypi.org/project/azure-iothub-device-client/) , usando o comando`pip install azure-iothub-device-client`

    * Se necessário, instale o pacote [Azure-iothub-Service-Client](https://pypi.org/project/azure-iothub-service-client/) usando o comando`pip install azure-iothub-service-client`

* **Mac OS**

    Por Mac OS, você precisa do Python 3.7.0 (ou 2,7) + libboost-1,67 + ondulação 7.61.1 (todas instaladas via homebrew). Qualquer outra distribuição/sistema operacional provavelmente incorporará versões diferentes do Boost & dependências que não funcionarão e resultará em um ImportError no tempo de execução.

    Os pacotes *pip* para `azure-iothub-service-client` e `azure-iothub-device-client` atualmente estão disponíveis apenas para o sistema operacional Windows. Para o Linux/Mac OS, consulte as seções específicas do Linux e do Mac OS na postagem [preparar seu ambiente de desenvolvimento para o Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) .

> [!NOTE]
> Houve vários relatórios de erros ao importar iothub_client em um exemplo. Para obter mais informações sobre como lidar com problemas de **ImportError** , consulte [lidando com problemas de ImportError](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues).

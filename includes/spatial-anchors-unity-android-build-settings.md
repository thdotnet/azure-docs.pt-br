---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 9798e5f76881be38fb27e1f428565caba6e50bf2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135150"
---
Abra **Configurações de Build** selecionando **Arquivo** > **Configurações de Build**.

Na seção **Plataforma**, selecione **Android**. Altere o **Sistema de compilação** para **Gradle** e certifique-se de que a caixa de seleção **Exportar projeto** não esteja marcada.

Selecione **Mudar Plataforma** para alterar a plataforma para **Android**. O Unity poderá solicitar que você instale componentes de suporte Android, se estiverem faltando.

![Janela Configurações de Build do Unity](./media/spatial-anchors-unity/unity-android-build-settings.png)

Feche a janela **Configurações de Build**.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>Baixar e importar o SDK do ARCore para Unity

Baixe o arquivo `unitypackage` do [SDK do ARCore para versões 1.7 do Unity](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.7.0). De volta no projeto do Unity, selecione **Ativos** > **Importar Pacote** > **Pacote Personalizado...** e, em seguida, selecione o arquivo `unitypackage` baixado anteriormente. Na caixa de diálogo **Importar Pacote do Unity**, verifique se todos os arquivos estão selecionados e, em seguida, selecione **Importar**.

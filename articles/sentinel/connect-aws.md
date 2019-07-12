---
title: Conectar-se dados Symantec AWS para versão prévia do Azure Sentinel | Microsoft Docs
description: Saiba como se conectar a dados Symantec AWS para Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2019
ms.author: rkarlin
ms.openlocfilehash: 214269bc5c854aa4d3bfd508b0adb5a53ec096df
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673967"
---
# <a name="connect-azure-sentinel-to-aws"></a>Conectar-se a Sentinela do Azure com o AWS

Use o conector do AWS para transmitir todos os seus eventos de CloudTrail do AWS para o Azure Sentinel. Esse processo de conexão delega acesso para o Azure Sentinel para seus logs de recursos do AWS, criando uma relação de confiança entre o CloudTrail do AWS e Azure Sentinel. Isso é feito no AWS, criando uma função que conceda permissão ao Azure Sentinel para acessar seus logs do AWS.

## <a name="prerequisites"></a>Pré-requisitos

Você deve ter permissão de gravação no espaço de trabalho do Azure Sentinel.

## <a name="connect-aws"></a>Conectar à AWS 
 
1.  No Amazon Web Services console, sob **identidade de segurança e conformidade**, clique em **IAM**.

    ![AWS1](./media/connect-aws/aws-1.png)

2.  Escolher **funções** e clique em **Criar função**.

    ![AWS2](./media/connect-aws/aws-2.png)

3.  Escolha **outra conta do AWS.** No **ID da conta** , insira o **ID de conta da Microsoft** (**123412341234**) que podem ser encontradas na página de conector do AWS no portal do Azure Sentinel.

    ![AWS3](./media/connect-aws/aws-3.png)

4.  Certifique-se **exigir ID externa** está selecionado e, em seguida e insira a ID externa (ID do espaço de trabalho) que podem ser encontrados na página de conector do AWS no portal do Azure Sentinel.

    ![AWS4](./media/connect-aws/aws-4.png)

5.  Sob **anexar política de permissões** selecionar **AWSCloudTrailReadOnlyAccess**.

    ![AWS5](./media/connect-aws/aws-5.png)

6.  Insira uma marca (opcional).

    ![AWS6](./media/connect-aws/aws-6.png)

7.  Em seguida, insira um **nome da função** e clique no **Criar função** botão.

    ![AWS7](./media/connect-aws/aws-7.png)

8.  Na lista de funções, escolha a função que você criou.

    ![AWS8](./media/connect-aws/aws-8.png)

9.  Cópia de **arn da função** e cole-o na **função adicionar** campo no Portal do Azure Sentinel.

    ![AWS9](./media/connect-aws/aws-9.png)

10. Para usar o esquema relevante no Log Analytics para eventos do AWS, pesquise **AWSCloudTrail**.



## <a name="next-steps"></a>Próximas etapas
Neste documento, você aprendeu como conectar o CloudTrail do AWS para Azure Sentinel. Para saber mais sobre o Azure Sentinel, consulte os seguintes artigos:
- Saiba como [Obtenha visibilidade sobre seus dados e possíveis ameaças](quickstart-get-visibility.md).
- Introdução ao [detecção de ameaças com o Azure Sentinel](tutorial-detect-threats.md).


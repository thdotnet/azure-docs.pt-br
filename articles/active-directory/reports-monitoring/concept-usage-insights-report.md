---
title: Relatam de uso e os insights no portal do Azure Active Directory | Microsoft Docs
description: Introdução ao relatório de uso e insights no portal do Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 3fba300d-18fc-4355-9924-d8662f563a1f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3fe1e72d277bffd4bc9b38ac377e33b341967e17
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65806350"
---
# <a name="usage-and-insights-report-in-the-azure-active-directory-portal"></a>Relatório de uso e insights no portal do Azure Active Directory

Com o relatório de uso e insights, você pode obter uma exibição centrada no aplicativo de seus dados de entrada. Você pode encontrar respostas às seguintes perguntas:

*   Quais são os principais aplicativos usados em minha organização?
*   Quais aplicativos têm as entradas mais com falha? 
*   Quais são os principais erros entrar para cada aplicativo?

## <a name="prerequisites"></a>Pré-requisitos 

Para acessar os dados do relatório de uso e insights, você precisa:

* Um locatário do Azure AD
* Uma licença do Azure AD premium (P1/P2) para exibir os dados de entrada
* Um usuário no administrador global, administrador de segurança, leitor de segurança ou funções de leitor de relatório. Além disso, qualquer usuário (não administradores) pode acessar suas próprias entradas. 

## <a name="access-the-usage-and-insights-report"></a>Acessar o relatório de uso e insights

1. Navegue até o [Portal do Azure](https://portal.azure.com).
2. Selecione o diretório certo e selecione **Azure Active Directory** e escolha **aplicativos empresariais**.
3. Dos **atividade** seção, selecione **uso & insights** para abrir o relatório. 

![Relatório de uso e insights](./media/concept-usage-insights-report/main-menu.png)
                                     

## <a name="use-the-report"></a>Use o relatório

O relatório de uso e insights mostra a lista de aplicativos com o sinal de um ou mais em tentativa e permite que você classifique pelo número de entradas bem-sucedidas, falhas de entradas e a taxa de sucesso.

Clicar em carga mais na parte inferior da lista permite que você exiba aplicativos adicionais na página. Você pode selecionar o intervalo de datas para exibir todos os aplicativos que foram usados dentro do intervalo.

Você também pode definir o foco em um aplicativo específico. Selecione **exibir a atividade de entrada** para ver o sinal na atividade ao longo do tempo para o aplicativo, bem como os principais erros.  

Quando você seleciona um dia no gráfico de uso de aplicativo, você obter uma lista detalhada das atividades de entrada para o aplicativo.  

![Relatório de uso e insights](./media/concept-usage-insights-report/usage-and-insights-report.png)

## <a name="next-steps"></a>Próximas etapas

* [Relatório de entradas](concept-sign-ins.md)
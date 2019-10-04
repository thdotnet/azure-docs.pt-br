---
title: Ferramentas de desenvolvimento
titleSuffix: Azure Data Science Virtual Machine
description: Saiba mais sobre as ferramentas e os ambientes de desenvolvimento integrados disponíveis no Máquina Virtual de Ciência de Dados.
keywords: ferramentas de ciência de dados, máquina virtual de ciência de dados, ferramentas para ciência de dados, ciência de dados do linux
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/3/2019
ms.openlocfilehash: 62eb5f72d4b4395602b2665c0d1b3da4f6bb459b
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71950194"
---
# <a name="development-tools-on-the-azure-data-science-virtual-machine"></a>Ferramentas de desenvolvimento no Máquina Virtual de Ciência de Dados do Azure

O Máquina Virtual de Ciência de Dados (DSVM) agrupa várias ferramentas populares em um ambiente de desenvolvimento integrado (IDE) altamente produtivo. Aqui estão algumas ferramentas oferecidas na DSVM.

## <a name="visual-studio-2017"></a>Visual Studio 2017

|    |           |
| ------------- | ------------- |
| O que é?   | IDE de uso geral      |
| Versões do DSVM com suporte      | Windows      |
| Usos típicos      | Desenvolvimento de software    |
| Como ele é configurado e instalado no DSVM?      | Carga de Trabalho de Ciência de Dados (ferramentas Python e R), carga de trabalho do Azure (Hadoop, Data Lake), Node.js, ferramentas do SQL Server, [Azure Machine Learning para o Visual Studio Code](https://github.com/Microsoft/vs-tools-for-ai)    |
| Como usá-lo e executá-lo      | Atalho da área de trabalho (`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\devenv.exe`)    |
| Ferramentas relacionadas no DSVM      |     Visual Studio Code, RStudio, Juno  |

## <a name="visual-studio-code"></a>Visual Studio Code 

|    |           |
| ------------- | ------------- |
| O que é?   | IDE de uso geral      |
| Versões do DSVM com suporte      | Windows, Linux     |
| Usos típicos      | Editor de código e a integração de Git   |
| Como usá-lo e executá-lo      | Atalho da área de trabalho (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`) no Windows, atalho da área de trabalho ou terminal (`code`) no Linux    |
| Ferramentas relacionadas no DSVM      |     Visual Studio 2017, RStudio, Juno  |

## <a name="rstudio--desktop"></a>Área de trabalho do RStudio 

|    |           |
| ------------- | ------------- |
| O que é?   | IDE de cliente para a linguagem R   |
| Versões do DSVM com suporte      | Windows, Linux      |
| Usos típicos      |  Desenvolvimento em R     |
| Como usá-lo e executá-lo      | Atalho da área de trabalho (`C:\Program Files\RStudio\bin\rstudio.exe`) no Windows, atalho da área de trabalho (`/usr/bin/rstudio`) no Linux      |
| Ferramentas relacionadas no DSVM      |   Visual Studio 2017, Visual Studio Code, Juno      |

## <a name="rstudio--server"></a>RStudio Server 

|    |           |
| ------------- | ------------- |
| O que é?   | IDE de cliente para a linguagem R   |
| O que é?   | IDE baseado na Web para R    |
| Versões do DSVM com suporte      | Linux      |
| Usos típicos      |  Desenvolvimento em R     |
| Como usá-lo e executá-lo      | Habilite o serviço com _systemctl habilite o RStudio-Server_e inicie o serviço com _systemctl Start RStudio-Server_. Em seguida, entre no RStudio Server em http: \//Your-VM-IP: 8787.       |
| Ferramentas relacionadas no DSVM      |   Visual Studio 2017, Visual Studio Code, RStudio Desktop      |

## <a name="juno"></a>Juno 

|    |           |
| ------------- | ------------- |
| O que é?   | IDE do cliente para a linguagem Julia   |
| Versões do DSVM com suporte      | Windows, Linux      |
| Usos típicos      |  Desenvolvimento em Julia     |
| Como usá-lo e executá-lo      | Atalho da área de trabalho (`C:\JuliaPro-0.5.1.1\Juno.bat`) no Windows, atalho da área de trabalho (`/opt/JuliaPro-VERSION/Juno`) no Linux      |
| Ferramentas relacionadas no DSVM      |   Visual Studio 2017, Visual Studio Code, RStudio      |

## <a name="pycharm"></a>Pycharm

|    |           |
| ------------- | ------------- |
| O que é?   | IDE do cliente para a linguagem Python    |
| Versões do DSVM com suporte      | Linux      |
| Usos típicos      |  Desenvolvimento em Python     |
| Como usá-lo e executá-lo      | Atalho da área de trabalho (`/usr/bin/pycharm`) no Linux      |
| Ferramentas relacionadas no DSVM      |   Visual Studio 2017, Visual Studio Code, RStudio      |



## <a name="power-bi-desktop"></a>Power BI Desktop 

|    |           |
| ------------- | ------------- |
| O que é?   | Ferramenta de BI e visualização de dados interativa    |
| Versões do DSVM com suporte      | Windows  |
| Usos típicos      |  Visualização de dados e criação de painéis   |
| Como usá-lo e executá-lo      | Atalho da área de trabalho (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Ferramentas relacionadas no DSVM      |   Visual Studio 2017, Visual Studio Code, Juno      |


---
title: Comparação baseada em recursos dos níveis de gerenciamento de API do Azure | Microsoft Docs
description: Este artigo compara os níveis de gerenciamento de API com base nos recursos que eles oferecem.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: apimpm
ms.openlocfilehash: d881c8de7ecc32be0ca0cc2c5a82e0d2d51a7054
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65780313"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Comparação baseada em recursos dos níveis de gerenciamento de API do Azure

Cada [tipo de preço](https://aka.ms/apimpricing) de Gerenciamento de API oferece um conjunto distinto de recursos e por unidade de [capacidade](api-management-capacity.md). A tabela a seguir resume os principais recursos disponíveis em cada um dos níveis. Alguns recursos podem funcionar de maneira diferente ou ter recursos diferentes dependendo da camada. Em tais casos, as diferenças são mencionadas nos artigos de documentação que descrevem esses recursos individuais.

| Recurso                                                                                      | Consumo<sup>VISUALIZAÇÃO</sup> | Desenvolvedor      | Básico          | Standard       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Integração do Azure AD<sup>1</sup>                                                             | Não                            | Sim            | Não              | sim            | Sim            |
| Suporte de rede virtual (VNet)                                                               | Não                            | Sim            | Não             | Não              | Sim            |
| Implantação em várias regiões                                                                      | Não                            | Não             | Não             | Não              | Sim            |
| Vários nomes de domínio personalizado                                                                 | Não                            | Não             | Não             | Não              | Sim            |
| Portal do desenvolvedor<sup>2</sup>                                                                 | Não                            | sim            | sim            | sim            | Sim            |
| Cache embutido                                                                               | Não                            | sim            | sim            | sim            | Sim            |
| Análise integrada                                                                           | Não                            | sim            | sim            | sim            | Sim            |
| [Configurações de SSL](api-management-howto-manage-protocols-ciphers.md)                             | Não                            | sim            | sim            | sim            | Sim            |
| [Cache externo](https://aka.ms/apimbyoc)                                                    | Sim                           | sim            | sim            | sim            | Sim            |
| [Autenticação de certificado de cliente](api-management-howto-mutual-certificates-for-clients.md) | Nº<sup>3</sup>                | Sim            | sim            | sim            | Sim            |
| [Backup e restauração](api-management-howto-disaster-recovery-backup-restore.md)               | Não                            | sim            | sim            | sim            | Sim            |
| [Gerenciamento via Git](api-management-configuration-repository-git.md)                        | Não                            | sim            | sim            | sim            | Sim            |
| API de Gerenciamento                                                                        | Não                            | sim            | sim            | sim            | Sim            |
| Métricas e logs do Azure Monitor                                                               | Nº<sup>4</sup>                | Sim            | sim            | sim            | Sim            |

<sup>1</sup> permite o uso do Azure AD (e do Azure AD B2C) como uma identidade de provedor para o usuário é entrar no portal do desenvolvedor.<br/>
<sup>2</sup> Incluindo funcionalidade relacionada, por ex. usuários, grupos, problemas, aplicativos e modelos de email e notificações.<br/>
<sup>3</sup> autenticação de certificado do cliente será adicionada para a camada de consumo antes de sua disponibilidade geral.<br/>
<sup>4</sup> suporte completo do Azure Monitor será adicionado para a camada de consumo.

---
title: Segurança dos Hubs de Notificação
description: Este tópico explica a segurança para hubs de notificação do Azure.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 05/31/2019
ms.author: jowargo
ms.openlocfilehash: 3f5b23028094b545262e9c01640890f2c0b989ca
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431245"
---
# <a name="notification-hubs-security"></a>Segurança dos Hubs de Notificação

## <a name="overview"></a>Visão geral

Este tópico descreve o modelo de segurança dos Hubs de notificação do Azure.

## <a name="shared-access-signature-security-sas"></a>Segurança de assinatura de acesso compartilhado (SAS)

Os hubs de notificação implementam um esquema de segurança de nível de entidade chamado SAS (assinatura de acesso compartilhado). Esse esquema permite que as entidades de mensagens declarem até 12 regras de autorização nas suas descrições que concedem direitos naquela entidade.

Cada regra contém um nome, um valor de chave (segredo compartilhado) e um conjunto de direitos, conforme explicado em [declarações de segurança](#security-claims). Ao criar um Hub de notificação, duas regras são criadas automaticamente: uma com **escutar** direitos (que usa o aplicativo cliente) e outra com **todos os** direitos (que usa o back-end do aplicativo).

Ao realizar o gerenciamento de registro dos aplicativos clientes, se as informações enviadas por meio de notificações não forem confidenciais (por exemplo, atualizações de clima), uma maneira comum de acessar um Hub de notificação é fornecer o valor da chave da regra de acesso de somente escuta para o aplicativo cliente e fornecer o valor de chave da regra de acesso completo para o back-end do aplicativo.

Os aplicativos não deve inserir o valor da chave em aplicativos de cliente da Windows Store, em vez disso, ter o aplicativo cliente recuperá-los de back-end do aplicativo na inicialização.

A chave com **escutar** acesso permite que um aplicativo cliente para se registrar para qualquer marca. Se seu aplicativo tiver que restringir registros para marcas específicas para clientes específicos (por exemplo, quando as marcas representam IDs de usuário), o back-end do aplicativo deve executar os registros. Para obter mais informações, consulte [gerenciamento de registro](notification-hubs-push-notification-registration-management.md). Observe que, dessa maneira, o aplicativo cliente não terá acesso direto aos Hubs de notificação.

## <a name="security-claims"></a>Declarações de segurança

Semelhantes a outras entidades, as operações de Hub de notificação são permitidas para três declarações de segurança: **Ouça**, **enviar**, e **gerenciar**.

| Declaração   | DESCRIÇÃO                                          | Operações permitidas |
| ------- | ---------------------------------------------------- | ------------------ |
| Escutar  | Criar/atualizar, ler e excluir registros simples | Criar/Atualizar o registro<br><br>Ler registro<br><br>Ler todos os registros para um identificador<br><br>Excluir registro |
| Enviar    | Enviar mensagens ao hub de notificação                | Enviar mensagem |
| Gerenciar  | CRUDs nos Hubs de notificação (incluindo atualização de credenciais PNS e chaves de segurança) e ler registros baseados em marcas |Criar/Atualizar/Ler/Excluir hubs de notificação<br><br>Ler registros por marca |

Hubs aceita tokens gerados com assinatura de notificação compartilhadas configuradas diretamente no Hub de notificação de chaves.

Não é possível enviar uma notificação para mais de um namespace. Namespaces são um contêiner lógico para os hubs de notificação e não estão envolvidos com o envio de notificações.
As políticas de acesso de nível de namespace (credenciais) podem ser usadas para operações de nível de namespace, por exemplo: listando os hubs de notificação, criação ou exclusão de hubs de notificação, etc. Somente as políticas de acesso de nível de hub seriam permitem que você envie notificações.

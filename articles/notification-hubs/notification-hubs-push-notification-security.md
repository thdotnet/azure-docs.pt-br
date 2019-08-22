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
ms.openlocfilehash: 73a6d0eaab286dec9d02bb55eb75f0781bcffcc4
ms.sourcegitcommit: a3a40ad60b8ecd8dbaf7f756091a419b1fe3208e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69891583"
---
# <a name="notification-hubs-security"></a>Segurança dos Hubs de Notificação

## <a name="overview"></a>Visão geral

Este tópico descreve o modelo de segurança dos Hubs de notificação do Azure.

## <a name="shared-access-signature-security-sas"></a>Segurança de assinatura de acesso compartilhado (SAS)

Os hubs de notificação implementam um esquema de segurança de nível de entidade chamado SAS (assinatura de acesso compartilhado). Cada regra contém um nome, um valor de chave (segredo compartilhado) e um conjunto de direitos, conforme explicado em [declarações de segurança](#security-claims). Ao criar um hub de notificação, duas regras são criadas automaticamente: uma com direitos de **escuta** (que o aplicativo cliente usa) e outra com **todos os** direitos (que o back-end do aplicativo usa).

Ao realizar o gerenciamento de registro dos aplicativos clientes, se as informações enviadas por meio de notificações não forem confidenciais (por exemplo, atualizações de clima), uma maneira comum de acessar um Hub de notificação é fornecer o valor da chave da regra de acesso de somente escuta para o aplicativo cliente e fornecer o valor de chave da regra de acesso completo para o back-end do aplicativo.

Os aplicativos não devem inserir o valor de chave nos aplicativos cliente da Windows Store, em vez disso, fazer com que o aplicativo cliente o recupere do back-end do aplicativo na inicialização.

A chave com acesso de **escuta** permite que um aplicativo cliente se registre em qualquer marca. Se seu aplicativo precisar restringir registros a marcas específicas para clientes específicos (por exemplo, quando as marcas representam IDs de usuário), o back-end do aplicativo deve executar os registros. Para obter mais informações, consulte [Gerenciamento de registro](notification-hubs-push-notification-registration-management.md). Observe que, dessa maneira, o aplicativo cliente não terá acesso direto aos Hubs de notificação.

## <a name="security-claims"></a>Declarações de segurança

Semelhantes a outras entidades, as operações de Hub de notificação são permitidas para três declarações de segurança: **Ouça**, **envie**e **gerencie**.

| Declaração   | Descrição                                          | Operações permitidas |
| ------- | ---------------------------------------------------- | ------------------ |
| Escutar  | Criar/atualizar, ler e excluir registros simples | Criar/Atualizar o registro<br><br>Ler registro<br><br>Ler todos os registros para um identificador<br><br>Excluir registro |
| Enviar    | Enviar mensagens ao hub de notificação                | Enviar mensagem |
| Gerenciar  | CRUDs nos Hubs de notificação (incluindo atualização de credenciais PNS e chaves de segurança) e ler registros baseados em marcas |Criar/Atualizar/Ler/Excluir hubs de notificação<br><br>Ler registros por marca |

Os hubs de notificação aceitam tokens de assinatura gerados com chaves compartilhadas configuradas diretamente no Hub de notificação.

Não é possível enviar uma notificação para mais de um namespace. Os namespaces são contêineres lógicos para hubs de notificação e não estão envolvidos no envio de notificações.
As políticas de acesso no nível de namespace (credenciais) podem ser usadas para operações em nível de namespace, por exemplo: listando hubs de notificação, criando ou excluindo hubs de notificação, etc. Somente as políticas de acesso no nível do Hub permitirão que você envie notificações.

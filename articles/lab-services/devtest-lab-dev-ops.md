---
title: Integração do Azure DevTest Labs e DevOps | Microsoft Docs
description: Saiba como usar o labs do Azure DevTest Labs dentro de uma integração contínua (CI) / pipelines de CD (entrega contínua) em um ambiente corporativo.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: spelluru
ms.openlocfilehash: 62c44bfea28d47d7c32aa7ef440a40d45c314683
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078918"
---
# <a name="integration-of-azure-devtest-labs-and-azure-devops"></a>Integração do Azure DevTest Labs e DevOps do Azure
DevOps é uma metodologia de desenvolvimento de software que se integra a operações (Ops) para um sistema de desenvolvimento de software (Dev). Esse sistema pode fornecer novos recursos, atualizações e correções em alinhamento com as metas de negócios. Essa metodologia engloba tudo, desde a criação de novos recursos com base em objetivos, padrões de uso e os comentários dos clientes; Para corrigir, recuperação e o sistema de proteção quando ocorrem problemas. Um componente facilmente identificado dessa metodologia é a integração contínua (CI) / pipeline de entrega contínua (CD). Um pipeline de CI/CD usa informações, código e recursos de uma confirmação por meio de uma série de etapas que incluem a criação, teste e implantação, para produzir o sistema. Este artigo se concentra em diferentes maneiras de usar efetivamente o labs dentro de um pipeline em um ambiente corporativo. 

## <a name="benefits-of-using-labs-in-devops-workflow"></a>Benefícios do uso de laboratórios no fluxo de trabalho de DevOps 

### <a name="focused-access"></a>Acesso detalhado 
Usando um laboratório como um componente permite que um ecossistema específico a ser associado a um grupo limitado de pessoas. Normalmente, uma equipe ou um grupo que está trabalhando em uma área comum ou um recurso específico terá um laboratório atribuído a eles.   

### <a name="infrastructure-replication-in-the-cloud"></a>Replicação de infraestrutura na nuvem 
Um desenvolvedor pode configurar rapidamente um ecossistema de desenvolvimento que inclui uma caixa de desenvolvimento com o código-fonte e ferramentas. O desenvolvedor também pode criar um ambiente que é quase idêntico à configuração de produção. Isso ajuda com o desenvolvimento mais rápido do loop interno. 

### <a name="pre-production"></a>Pré-produção 
Ter um laboratório no pipeline de CI/CD torna mais fácil de ter vários ambientes de pré-produção diferentes ou máquinas em execução ao mesmo tempo para teste assíncrono. Infra-estruturas de suporte diferente, como agentes de compilação podem ser implantadas e gerenciadas em um laboratório. 

## <a name="devops-with-devtest-labs"></a>DevOps com o DevTest Labs 

### <a name="development--operation"></a>Desenvolvimento / operação 
Um laboratório deve se concentrar em uma equipe que está trabalhando em uma área de recurso. Esse foco comum permite o compartilhamento de recursos de área específica, como as ferramentas, scripts ou modelos do Resource Manager. Ele permite que as alterações mais rápidas enquanto os limita os efeitos negativos a um grupo menor. Esses recursos compartilhados permitem ao desenvolvedor criar VMs de desenvolvimento com o código necessário, ferramentas e configuração. Eles podem ser criados dinamicamente ou tem um sistema que cria imagens de base com as personalizações. Não apenas pode o desenvolvedor criar VMs, mas também que eles podem criar ambientes de laboratórios de desenvolvimento/teste com base nos modelos necessários para criar os recursos do Azure apropriados no laboratório. Quaisquer alterações ou destrutiva work pode ser feito no ambiente de laboratório sem afetar outras pessoas. Considere o cenário em que o produto é um sistema autônomo instalado na máquina do cliente. Nesse cenário, o DevTest Labs melhorou a criação da VM que inclui a instalação de software adicional usando artefatos e pré-compilando as configurações do cliente para o teste mais rápido de loop interno do código. 
  
## <a name="cicd-pipeline"></a>Pipeline de CI/CD 
O pipeline de CI/CD é um dos componentes essenciais do DevOps que mover o código de uma solicitação de pull do desenvolvedor, integra-se com o código existente e a implanta para o ecossistema de produção. Todos os recursos não precisam estar dentro de um laboratório. Por exemplo, um host de Jenkins pode ser definido fora do laboratório como um recurso mais persistente. Aqui estão alguns exemplos específicos de integrar laboratórios em pipeline. 

### <a name="build"></a>Compilação 
O pipeline de compilação está concentrado na criação de um pacote de componentes que serão testados juntos para ser entregue para o pipeline de lançamento. Laboratórios podem ser parte do pipeline de build como o local para agentes de compilação e outros recursos de suporte. Ter a capacidade de criar dinamicamente a infra-estrutura permite maior controle. Com a capacidade de ter vários ambientes em um laboratório, cada build pode ser executada assincronamente enquanto estiver usando a ID de compilação como parte das informações de ambiente para identificar exclusivamente os recursos para a compilação específica.   

Para agentes de compilação, a capacidade do laboratório para restringir o acesso aumenta a segurança e reduz a possibilidade de danos acidentais.  

### <a name="test"></a>Teste 
DevTest Labs permite que um pipeline de CI/CD para automatizar a criação de recursos do Azure (máquinas virtuais, ambientes) que pode ser usado para testes automáticos e manuais. As VMs seriam criadas usando os artefatos ou fórmulas que usam informações do processo de compilação para criar as diferentes configurações personalizadas necessárias para teste.   

### <a name="release"></a>Versão 
DevTest Labs é comumente usado para verificação na seção de versão antes do código é implantado. Ele é semelhante ao teste na seção de Build. Recursos de produção não devem ser implantados em laboratórios de desenvolvimento/teste. 

### <a name="customization"></a>Personalização 
No DevOps do Azure, há tarefas existentes que permitem a manipulação de VMs e os ambientes em laboratórios específicos. Enquanto os serviços de DevOps do Azure são uma maneira de gerenciar o pipeline de CI/CD, você pode integrar o laboratório em qualquer sistema compatível com a capacidade de chamar APIs REST, executar scripts do PowerShell ou usar a CLI do Azure. 

Enquanto alguns gerenciadores de pipeline de CI/CD têm existente plugins de código-fonte aberto que pode gerenciar recursos dentro do Azure e o DevTest Labs. Você talvez precise usar alguns scripts personalizados para atender às necessidades específicas do pipeline.  Com isso em mente, ao executar uma tarefa, uma entidade de serviço é usada com a função apropriada para obter acesso ao laboratório. A entidade de serviço geralmente precisa de acesso da função de Colaborador ao laboratório. Para obter mais informações, consulte [integrar o Azure DevTests Labs em seu pipeline de integração e entrega de contínua do Azure DevOps](devtest-lab-integrate-ci-cd-vsts.md). 
 
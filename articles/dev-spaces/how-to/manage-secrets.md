---
title: Como gerenciar segredos ao trabalhar com um Azure Dev Space
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: conceptual
description: Desenvolvimento rápido de Kubernetes com contêineres e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Serviço de Contêiner do Azure, contêineres
ms.openlocfilehash: 900529d54a26729d9d0fb949d9217d5e2d618254
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66515287"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Como gerenciar segredos ao trabalhar com um Azure Dev Space

Os serviços podem exigir determinadas senhas, cadeias de conexão e outros segredos, como bancos de dados, ou outros serviços seguros do Azure. Ao definir os valores desses segredos nos arquivos de configuração, será possível disponibilizá-los no código como variáveis de ambiente.  Estes devem ser manipulados com cuidado para evitar comprometer a segurança dos segredos.

Espaços de desenvolvimento do Azure fornece duas opções recomendadas e simplificadas para armazenar segredos em gráficos Helm gerados por ferramentas de cliente a espaços de desenvolvimento do Azure: no arquivo values.dev.yaml e embutido diretamente no azds.yaml. Não é recomendável armazenar segredos em values.yaml. Fora as duas abordagens para Helm gráficos gerados por ferramentas de cliente definida neste artigo, se você criar seu próprio gráfico do Helm, você pode usar o gráfico do Helm diretamente para gerenciar e armazenar segredos.

## <a name="method-1-valuesdevyaml"></a>Método 1: values.dev.yaml
1. Abra o VS Code com o projeto que está habilitado para o Azure Dev Spaces.
2. Adicione um arquivo chamado _values.dev.yaml_ na mesma pasta que o existente _azds.yaml_ e definir sua chave secreta e os valores, como no exemplo a seguir:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. _azds.YAML_ já faz referência a _values.dev.yaml_ arquivo se ele existir. Se você preferir um nome de arquivo diferente, atualize a seção de install.values:

    ```yaml
    install:
      values:
      - values.dev.yaml?
      - secrets.dev.yaml?
    ```
 
4. Modifique o código de serviço para referir-se a esses segredos como variáveis de ambiente, como no exemplo a seguir:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Atualize os serviços em execução no cluster com essas alterações. Na linha de comando, execute o comando:

    ```
    azds up
    ```
 
6. (Opcional) Na linha de comando, verifique se estes segredos foram criados:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Certifique-se de adicionar _values.dev.yaml_ ao arquivo _.gitignore_ para evitar confirmar segredos no controle de origem.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Método 2: embutido diretamente no azds.yaml
1.  Em _azds.yaml_, configure os segredos na seção de yaml configurações/desenvolver/instalar. Embora não seja possível inserir valores secretos diretamente, isso não é recomendável porque _azds.yaml_ está selecionado no controle do código-fonte. Em vez disso, adicione espaços reservados usando a sintaxe "$PLACEHOLDER".

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT"
                host: "$REDIS_HOST"
                key: "$REDIS_KEY"
    ```
     
2.  Crie um arquivo _.env_ na mesma pasta que _azds.yaml_. Insira segredos usando a notação chave-valor padrão. Não confirme o arquivo _.env_ para o controle do código-fonte. (Para omitir do controle do código-fonte em sistemas de controle de versão baseados em git, adicione-o no arquivo _.gitignore_.) O exemplo a seguir mostra um arquivo _.env_:

    ```
    REDIS_PORT=3333
    REDIS_HOST=myredishost
    REDIS_KEY=myrediskey
    ```
2.  Modifique o código-fonte de serviço para referenciar esses segredos no código, como no exemplo a seguir:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Atualize os serviços em execução no cluster com essas alterações. Na linha de comando, execute o comando:

    ```
    azds up
    ```

4.  (opcional) Ver segredos do kubectl:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Próximas etapas

Com esses métodos, é possível se conectar com segurança a um banco de dados, um Cache do Azure para Redis ou acessar serviços seguros do Azure.
 

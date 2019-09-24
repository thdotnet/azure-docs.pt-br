---
title: Conectar-se usando o Truffle
description: Conectar-se a uma rede do serviço Azure Blockchain usando o Truffle
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 08/29/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: ea64e3fe4789ac0558463ded81c8179db83469d5
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70932591"
---
# <a name="quickstart-use-truffle-to-connect-to-a-transaction-node"></a>Início Rápido: Usar o Truffle para se conectar a um nó de transação

O Truffle é um ambiente de desenvolvimento de blockchain que pode ser usado para se conectar a um nó de transação do Azure Blockchain Service.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Concluir [Início Rápido: Criar um membro do blockchain usando o portal do Azure](create-member.md) ou [Início Rápido: Criar um membro do blockchain do Azure Blockchain Service usando a CLI do Azure](create-member-cli.md)
* Instale o [Truffle](https://github.com/trufflesuite/truffle). O Truffle exige a instalação de várias ferramentas, incluindo [Node.js](https://nodejs.org) e [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
* Instale o [Python 2.7.15](https://www.python.org/downloads/release/python-2715/). O Python é necessário para o Web3.

## <a name="create-truffle-project"></a>Criar um projeto do Truffle

1. Abra um prompt de comando ou o shell do Node.js.
1. Altere o diretório para o local em que deseja criar o diretório do projeto do Truffle.
1. Crie um diretório para o projeto e altere o caminho para o novo diretório. Por exemplo,

    ``` bash
    mkdir truffledemo
    cd truffledemo
    ```

1. Inicialize o projeto do Truffle.

    ``` bash
    truffle init
    ```

1. Instale o web3 da API de JavaScript do Ethereum na pasta do projeto. Atualmente, a versão do web3 1.0.0-beta.37 é obrigatória.

    ``` bash
    npm install web3@1.0.0-beta.37
    ```

    Você poderá receber avisos do npm durante a instalação.
    
## <a name="configure-truffle-project"></a>Configurar o projeto do Truffle

Para configurar o projeto do Truffle, você precisa de algumas informações sobre o nó de transação do portal do Azure.

### <a name="transaction-node-endpoint-addresses"></a>Endereços do ponto de extremidade do nó de transação

1. No portal do Azure, navegue para o nó de transação padrão e selecione **Nós de transação > Cadeias de conexão**.
1. Copie e salve a URL do ponto de extremidade de **HTTPS (Chave de acesso 1)** . Você precisará dos endereços do ponto de extremidade para o arquivo de configuração do contrato inteligente mais adiante no tutorial.

    ![Endereço do ponto de extremidade da transação](./media/connect-truffle/endpoint.png)

### <a name="edit-configuration-file"></a>Editar o arquivo de configuração

Em seguida, você precisa atualizar o arquivo de configuração do Truffle com o ponto de extremidade do nó de transação.

1. Na pasta do projeto **truffledemo**, abra o arquivo de configuração do Truffle `truffle-config.js` em um editor.
1. Substitua o conteúdo do arquivo pelas informações de configuração a seguir. Adicione uma variável que contém o endereço do ponto de extremidade. Substitua o colchete angular pelos valores coletados da seção anterior.

    ``` javascript
    var defaultnode = "<default transaction node connection string>";   
    var Web3 = require("web3");
    
    module.exports = {
      networks: {
        defaultnode: {
          provider: new Web3.providers.HttpProvider(defaultnode),
          network_id: "*"
        }
      }
    }
    ```

1. Salve as alterações em `truffle-config.js`.

## <a name="connect-to-transaction-node"></a>Conectar-se ao nó de transação

Usaremos o *Web3* para se conectar ao nó de transação.

1. Use o console do Truffle para conectar-se ao nó de transação padrão. No shell ou prompt de comando, execute o comando a seguir:

    ``` bash
    truffle console --network defaultnode
    ```

    O Truffle conecta-se ao nó de transação padrão e oferece um console interativo.

    Você pode chamar métodos no objeto **web3** para interagir com o nó de transação.

1. Chame o método **getBlockNumber** para retornar o número de bloco atual.

    ```bash
    web3.eth.getBlockNumber();
    ```

    Saída de exemplo:

    ```bash
    truffle(defaultnode)> web3.eth.getBlockNumber();
    18567
    ```
1. Saia do console do Truffle.

    ```bash
    .exit
    ```

## <a name="next-steps"></a>Próximas etapas

Neste Início Rápido, você criou um projeto do Truffle para se conectar ao nó de transação padrão do serviço Azure Blockchain.

Experimente o próximo tutorial para usar o Azure Blockchain Development Kit para Ethereum e Truffle para criar, implantar e executar uma função de contrato inteligente por uma transação.

> [!div class="nextstepaction"]
> [Use o Visual Studio Code para criar e implantar contratos inteligentes](send-transaction.md)

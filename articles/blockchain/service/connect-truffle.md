---
title: Conectar-se usando o Truffle
description: Conectar-se a uma rede do serviço Azure Blockchain usando o Truffle
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 9154bc749f7db337de67f501d5e5049dfd466156
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698479"
---
# <a name="quickstart-use-truffle-to-connect-to-an-azure-blockchain-service-network"></a>Início Rápido: Usar o Truffle para se conectar a uma rede do serviço Azure Blockchain

O Truffle é um ambiente de desenvolvimento de blockchain que pode ser usado para se conectar a um nó do serviço Azure Blockchain.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* [Criar um membro do Azure Blockchain](create-member.md)
* Instale o [Truffle](https://github.com/trufflesuite/truffle). O Truffle exige a instalação de várias ferramentas, incluindo [Node.js](https://nodejs.org) e [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
* Instale o [Python 2.7.15](https://www.python.org/downloads/release/python-2715/). O Python é necessário para o Web3.
* Instale o [Visual Studio Code](https://code.visualstudio.com/download).
* Instale a [extensão Solidity do Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity).

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

1. No portal do Azure, navegue para cada nó de transação e selecione **Nós de transação > Cadeias de conexão**.
1. Copie e salve a URL do ponto de extremidade de **HTTPS (Chave de acesso 1)** para cada nó de transação. Você precisará dos endereços do ponto de extremidade para o arquivo de configuração do contrato inteligente mais adiante no tutorial.

    ![Endereço do ponto de extremidade da transação](./media/send-transaction/endpoint.png)

### <a name="edit-configuration-file"></a>Editar o arquivo de configuração

1. Inicie o Visual Studio Code e abra a pasta do diretório do projeto do Truffle usando o menu **Arquivo > Abrir Pasta**.
1. Abra o arquivo de configuração `truffle-config.js` do Truffle.
1. Substitua o conteúdo do arquivo pelas informações de configuração a seguir. Adicione uma variável que contém o endereço do ponto de extremidade. Substitua o colchete angular pelos valores coletados das seções anteriores.

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

1. Use o console do Truffle para conectar-se ao nó de transação padrão.

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
1. Saia do console de desenvolvimento do Truffle.

    ```bash
    .exit
    ```

## <a name="next-steps"></a>Próximas etapas

Neste Início Rápido, você criou um projeto do Truffle para se conectar ao nó de transação padrão do serviço Azure Blockchain.

Experimente o próximo tutorial para usar o Truffle para enviar uma transação à rede de blockchain do consórcio.

> [!div class="nextstepaction"]
> [Enviar uma transação](send-transaction.md)

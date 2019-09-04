---
title: Controle de versão-LUIS
titleSuffix: Azure Cognitive Services
description: As versões no LUIS são semelhantes às versões na programação tradicional. Cada versão é um instantâneo no tempo do aplicativo. Antes de fazer alterações no aplicativo, crie uma nova versão. É mais fácil voltar para o aplicativo exato e, em seguida, tentar remover e retornar a intenção e os enunciados do aplicativo para um estado anterior.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: diberry
ms.openlocfilehash: c519b030aaee58397766ecb8658e7af08b5986e1
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70256868"
---
# <a name="understand-how-and-when-to-use-a-luis-version"></a>Reconhecer como e quando usar uma versão do LUIS

As versões no LUIS são semelhantes às versões na programação tradicional. Cada versão é um instantâneo no tempo do aplicativo. Antes de fazer alterações no aplicativo, crie uma nova versão. É mais fácil voltar para a versão exata e, em seguida, tentar remover intenções e enunciados para um estado anterior.

Crie modelos diferentes do mesmo aplicativo com [versões](luis-how-to-manage-versions.md). 

## <a name="version-id"></a>ID da versão
A ID da versão é composta de caracteres, dígitos ou '.' e não pode ser maior do que 10 caracteres.

## <a name="initial-version"></a>Versão inicial
A versão inicial (0.1) é a versão ativa padrão. 

## <a name="active-version"></a>Versão ativa
[Definir uma versão](luis-how-to-manage-versions.md#set-active-version) como a ativa, significa que atualmente ela é editada e testada no site do [LUIS](luis-reference-regions.md). Defina uma versão como ativa para acessar seus dados, fazer atualizações, além de testá-la e publicá-la.

O nome da versão ativa no momento é exibido no painel superior esquerdo depois do nome do aplicativo. 

[![Alterar versão ativa](./media/luis-concept-version/version-in-nav-bar-inline.png)](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>Versões e slots de publicação
Você publica em slots de estágio e de produto. Cada slot pode ter uma versão diferente ou a mesma versão. Isso é útil para verificar as alterações entre as versões do modelo por meio do ponto de extremidade, disponível para bots ou outros aplicativos de chamada do LUIS. 

## <a name="clone-a-version"></a>Clonar uma versão
Clone uma versão para criar uma cópia de uma versão existente e salve-a como uma nova versão. Clone uma versão para usar o mesmo conteúdo da versão existente como ponto de partida para a nova versão. Após clonar uma versão, a nova versão se tornará a versão **ativa**. 

## <a name="import-and-export-a-version"></a>Importar e exportar uma versão
É possível importar uma versão no nível do aplicativo. Essa versão se tornará a versão ativa e usava a ID de versão na propriedade "versionId" do arquivo do aplicativo. Também é possível importar, no nível da versão, para um aplicativo existente. A nova versão torna-se a versão ativa. 

É possível exportar uma versão no nível do aplicativo ou no nível da versão. A única diferença é que a versão exportada do nível do aplicativo é a versão ativa no momento, enquanto no nível da versão, é possível escolher qualquer versão para ser exportada na página **[Configurações](luis-how-to-manage-versions.md)** . 

O arquivo exportado não contém informações de aprendizado de máquina, porque o aplicativo é treinado novamente após a importação. O arquivo exportado não contém informações de colaborador.

## <a name="export-each-version-as-app-backup"></a>Exportar cada versão como backup do aplicativo
Para fazer backup do aplicativo LUIS, exporte cada versão na página **[Configurações](luis-how-to-manage-versions.md)** .

## <a name="delete-a-version"></a>Excluir uma versão
É possível excluir todas as versões, exceto a versão ativa, da lista de versões na página Configurações. 

## <a name="version-availability-at-the-endpoint"></a>Disponibilidade da versão no ponto de extremidade
Versões treinadas não estão automaticamente disponíveis no [ponto de extremidade](luis-glossary.md#endpoint) do seu aplicativo. É necessário [publicar](luis-how-to-publish-app.md) ou republicar uma versão para que ela esteja disponível no ponto de extremidade do seu aplicativo. É possível publicar em **Preparo** e em **Produção**, fornecendo até duas versões do aplicativo disponível no ponto de extremidade. Se precisar de mais versões do aplicativo disponível em um ponto de extremidade, você deverá exportar a versão e importar novamente a um novo aplicativo. O novo aplicativo tem uma ID do aplicativo diferente.

## <a name="manage-multiple-versions-inside-the-same-app"></a>Gerenciar várias versões dentro do mesmo aplicativo
Comece [clonando](luis-how-to-manage-versions.md#clone-a-version), de uma versão base, para cada autor. 

Cada autor faz alterações em sua própria versão do aplicativo. Depois que cada autor estiver satisfeito com o modelo, exporte as novas versões para os arquivos JSON.  

Aplicativos exportados são arquivos formatados para JSON, que podem ser comparados quanto a alterações. Combine os arquivos para criar um único arquivo JSON da nova versão. Altere a propriedade **versionId** no JSON para significar a nova versão mesclada. Importe essa versão para o aplicativo original. 

Esse método permite que você tenha uma versão ativa, uma versão do estágio e uma versão publicada. Você pode comparar os resultados da versão ativa com uma versão publicada (fase ou produção) no painel de [teste interativo](luis-interactive-test.md).

## <a name="manage-multiple-versions-as-apps"></a>Gerenciar várias versões como aplicativos
[Exporte](luis-how-to-manage-versions.md#export-version) a versão base. Cada autor importa a versão. A pessoa que importa o aplicativo é o proprietário da versão. Quando ela terminar de modificar o aplicativo, exporte a versão. 

Aplicativos exportados são arquivos formatados para JSON, que podem ser comparados com a exportação base quanto a alterações. Combine os arquivos para criar um único arquivo JSON da nova versão. Altere a propriedade **versionId** no JSON para significar a nova versão mesclada. Importe essa versão para o aplicativo original.

## <a name="contributions-from-collaborators"></a>Contribuições de colaboradores

Saiba mais sobre a criação de contribuições de [colaboradores](luis-how-to-collaborate.md).

## <a name="next-steps"></a>Próximas etapas

Veja como adicionar o [controle de versão](luis-how-to-manage-versions.md) na página de configurações do aplicativo. 

Saiba como criar [intenções](luis-concept-intent.md) no modelo.

---
title: Testar a definição de interface do usuário para Aplicativos Gerenciados do Azure | Microsoft Docs
description: Descreve como testar a experiência do usuário para criar o Aplicativo Gerenciado do Azure por meio do portal.
author: tfitzmac
ms.service: managed-applications
ms.topic: conceptual
ms.date: 05/26/2019
ms.author: tomfitz
ms.openlocfilehash: 99ca319910be2cb20214172826eb40361abe72f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66257659"
---
# <a name="test-your-portal-interface-for-azure-managed-applications"></a>Teste sua interface do portal para aplicativos gerenciados do Azure

Após [criando o arquivo Createuidefinition](create-uidefinition-overview.md) para seu aplicativo gerenciado, você precisa testar a experiência do usuário. Para simplificar o teste, use um ambiente de área restrita que carrega o arquivo no portal. Não é necessário realmente implantar o aplicativo gerenciado. A área restrita apresenta sua interface do usuário na experiência do portal atual, a tela inteira. Ou, você pode usar um script do PowerShell para testar a interface, mas ele usa um modo de exibição herdado do portal. As duas abordagens são mostradas neste artigo. A área restrita é a maneira recomendada para a interface de visualização.

## <a name="prerequisites"></a>Pré-requisitos

* Um arquivo **createUiDefinition.json**. Se você não tiver esse arquivo, copie o [arquivo de exemplo](https://github.com/Azure/azure-quickstart-templates/blob/master/100-marketplace-sample/createUiDefinition.json).

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="use-sandbox"></a>Usar área restrita

1. Abra o [criar área restrita de definição de interface do usuário](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade).

   ![Mostrar a área restrita](./media/test-createuidefinition/show-sandbox.png)

1. Substitua a definição vazia com o conteúdo do arquivo Createuidefinition. Selecione **visualização**.

   ![Selecione a versão prévia](./media/test-createuidefinition/select-preview.png)

1. O formulário que você criou é exibido. Você pode percorrer a experiência do usuário e preencha os valores.

   ![Mostrar o formulário](./media/test-createuidefinition/show-ui-form.png)

### <a name="troubleshooting"></a>solução de problemas

Se não for exibido depois de selecionar seu formulário **visualização**, você pode ter um erro de sintaxe. Procure o indicador vermelho na barra de rolagem à direita e navegue até ele.

![Mostrar erro de sintaxe](./media/test-createuidefinition/show-syntax-error.png)

Se o formulário não exibe e, em vez disso, você verá um ícone de uma nuvem com o descarte de desmontagem, seu formulário tem um erro, como uma propriedade ausente. Abra as ferramentas de desenvolvedor da Web em seu navegador. O **Console** exibe mensagens importantes sobre a interface.

![Mostrar erro](./media/test-createuidefinition/show-error.png)

## <a name="use-test-script"></a>Usar script de teste

Para testar a interface no portal, copie um dos scripts a seguir para o computador local:

* [Script de sideload do PowerShell](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-CreateUIDefinition.ps1)
* [Script de sideload do Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/sideload-createuidef.sh)

Para ver o arquivo de interface no portal, execute o script baixado. O script cria uma conta de armazenamento na assinatura do Azure e carrega o arquivo createUiDefinition.json para a conta de armazenamento. A conta de armazenamento é criada na primeira vez que você executa o script ou em caso de exclusão da conta de armazenamento. Se a conta de armazenamento já existe em sua assinatura do Azure, o script a reutiliza. O script abre o portal e carrega o arquivo da conta de armazenamento.

Forneça um local para a conta de armazenamento e especifique a pasta que tem o arquivo createUiDefinition.json.

Para o PowerShell, use:

```powershell
.\SideLoad-CreateUIDefinition.ps1 `
  -StorageResourceGroupLocation southcentralus `
  -ArtifactsStagingDirectory .\100-Marketplace-Sample
```

Para a CLI do Azure, use:

```azurecli
./sideload-createuidef.sh \
  -l southcentralus \
  -a .\100-Marketplace-Sample
```

Se o arquivo createUiDefinition.json estiver na mesma pasta que o script e você já tiver criado a conta de armazenamento, você não precisará fornecer esses parâmetros.

Para o PowerShell, use:

```powershell
.\SideLoad-CreateUIDefinition.ps1
```

Para a CLI do Azure, use:

```azurecli
./sideload-createuidef.sh
```

O script abre uma nova guia no navegador. Em seguida, exibe o portal com a interface para criar o aplicativo gerenciado.

![Exibir portal](./media/test-createuidefinition/view-portal.png)

Forneça valores para os campos. Quando terminar, os valores que são passados para o modelo serão exibidos.

![Mostrar valores](./media/test-createuidefinition/show-json.png)

Você pode usar esses valores como o arquivo de parâmetro para testar seu modelo de implantação.

Se o portal para de responder na tela de resumo, pode haver um bug na seção de saída. Por exemplo, você pode ter referenciado um controle que não existe. Se um parâmetro de saída estiver vazio, o parâmetro pode fazer referência a uma propriedade que não existe. Por exemplo, a referência para o controle é válida, mas a referência da propriedade não é válida.

## <a name="test-your-solution-files"></a>Testar os arquivos de solução

Agora que você verificou que a interface do portal está funcionando conforme o esperado, é hora de validar que o arquivo createUiDefinition está corretamente integrado ao arquivo mainTemplate.json. É possível executar um teste de script de validação para testar o conteúdo dos arquivos de solução, incluindo o arquivo createUiDefinition. O script valida a sintaxe JSON, verifica expressões regex em campos de texto e garante que os valores de saída da interface do portal correspondam aos parâmetros do modelo. Para obter mais informações sobre como executar esse script, consulte [Executar verificações de validação estática para modelos](https://github.com/Azure/azure-quickstart-templates/tree/master/test/template-validation-tests).

## <a name="next-steps"></a>Próximas etapas

Após validar a interface do portal, saiba como tornar o[aplicativo gerenciado do Azure disponível no Marketplace](publish-marketplace-app.md).

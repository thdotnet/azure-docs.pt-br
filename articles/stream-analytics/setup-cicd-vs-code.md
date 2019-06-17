---
title: Integrar e desenvolver com o pacote npm de CI/CD do Azure Stream Analytics continuamente
description: Este artigo descreve como usar o pacote npm de CI/CD do Azure Stream Analytics para configurar um processo de implantação e integração contínua.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: fa5a57afa379c6bbe027be80f400fc176800d289
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66158498"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-cicd-npm-package"></a>Integrar e desenvolver com o pacote npm de Stream Analytics CI/CD continuamente
Este artigo descreve como usar o pacote de npm do Azure Stream Analytics CI/CD para configurar um processo de implantação e integração contínua.

## <a name="build-the-vs-code-project"></a>Compile o projeto do VS Code

Você pode habilitar a integração contínua e implantação para trabalhos do Azure Stream Analytics usando o **asa-streamanalytics-cicd** pacote npm. O pacote npm fornece as ferramentas para gerar modelos do Azure Resource Manager do [projetos do Visual Studio Code do Stream Analytics](quick-create-vs-code.md). Ele pode ser usado no Windows, macOS e Linux sem instalar o Visual Studio Code.

Quando você tiver [baixou o pacote](https://www.npmjs.com/package/azure-streamanalytics-cicd), use o seguinte comando para gerar os modelos do Azure Resource Manager. O **scriptPath** argumento é o caminho absoluto para o **asaql** arquivo em seu projeto. Verifique se que os arquivos de JobConfig.json e asaproj.json estão na mesma pasta com o arquivo de script. Se o **outputPath** não for especificado, os modelos serão colocados na **Deploy** pasta sob o projeto **bin** pasta.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Exemplo (no macOS)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

Quando um projeto do Visual Studio Code do Stream Analytics é compilado com êxito, ele gera os seguintes arquivos de modelo do Azure Resource Manager duas sob o **bin / [depuração/varejo] / implantar** pasta: 

*  Arquivo de modelo do Resource Manager

       [ProjectName].JobTemplate.json 

*  Arquivo de parâmetros do Resource Manager

       [ProjectName].JobTemplate.parameters.json   

Os parâmetros padrão no arquivo Parameters. JSON são das configurações em seu projeto do Visual Studio Code. Se você deseja implantar em outro ambiente, apenas substitua os parâmetros adequadamente.

> [!NOTE]
> Para todas as credenciais, os valores padrão são definidos como nulo. Eles **precisam** ser definidos antes de serem implantados na nuvem.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Saiba mais sobre como [implantar com um arquivo de modelo do Resource Manager e o Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Saiba mais sobre como [usar um objeto como um parâmetro em um modelo do Resource Manager](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

Para usar a Identidade Gerenciada para o Azure Data Lake Storage Gen1 como coletor de saída, você precisará fornecer Acesso à entidade de serviço usando o PowerShell antes da implantação no Azure. Saiba mais sobre como [implantar o ADLS Gen1 com a Identidade Gerenciada e o modelo do Resource Manager](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).
## <a name="next-steps"></a>Próximas etapas

* [Início Rápido: Criar um trabalho de nuvem do Azure Stream Analytics no Visual Studio Code (visualização)](quick-create-vs-code.md)
* [Testar as consultas do Stream Analytics localmente com o Visual Studio Code (visualização)](vscode-local-run.md)
* [Explorar o Azure Stream Analytics com o código do Visual Studio (versão prévia)](vscode-explore-jobs.md)

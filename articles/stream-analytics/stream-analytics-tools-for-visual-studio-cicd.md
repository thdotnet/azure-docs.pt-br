---
title: Integrar e desenvolver com o pacote NuGet de CI/CD do Azure Stream Analytics continuamente
description: Este artigo descreve como usar o pacote NuGet de CI/CD do Azure Stream Analytics para configurar um processo de implantação e integração contínua.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: f34139dafffe3d4890f17988114dffdd8b480d2d
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827306"
---
# <a name="continuously-integrate-and-develop-with-azure-stream-analytics-cicd-nuget-package"></a>Integrar e desenvolver com o pacote NuGet de CI/CD do Azure Stream Analytics continuamente
Este artigo descreve como usar o pacote NuGet de CI/CD do Azure Stream Analytics para configurar um processo de implantação e integração contínua.

Use a versão 2.3.0000.0 ou superior das [ferramentas de Stream Analytics para Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio) para obter suporte para MSBuild.

Um pacote do NuGet está disponível: [Microsoft.Azure.Stream Analytics.CICD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/). Ele fornece o MSBuild, execução local e as ferramentas de implantação que suportam o processo de implantação e integração contínua do [projetos do Stream Analytics para Visual Studio](stream-analytics-vs-tools.md). 
> [!NOTE]
> O pacote NuGet só pode ser usado com a versão 2.3.0000.0 ou superior do Stream Analytics para Visual Studio. Se você tiver projetos criados em versões anteriores das ferramentas do Visual Studio, basta abri-los com o 2.3.0000.0 ou versão superior e salvar. Em seguida, os novos recursos são habilitados. 

Para obter mais informações, veja [Ferramentas do Stream Analytics para Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="msbuild"></a>MSBuild
Assim como na experiência padrão do MSBuild do Visual Studio, há duas opções para compilar um projeto. Você pode clicar com o botão direito do mouse no projeto e, em seguida, escolher **Compilar**. Você também pode usar o **MSBuild** no pacote NuGet da linha de comando.
```
./build/msbuild /t:build [Your Project Full Path] /p:CompilerTaskAssemblyFile=Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll  /p:ASATargetsFilePath="[NuGet Package Local Path]\build\StreamAnalytics.targets"

```

Após o build bem-sucedido de um projeto do Stream Analytics para Visual Studio, ele gera os dois arquivos de modelo do Azure Resource Manager a seguir na pasta **bin/[Depuração/Versão Comercial]/Deploy**: 

*  Arquivo de modelo do Resource Manager

       [ProjectName].JobTemplate.json 

*  Arquivo de parâmetros do Resource Manager

       [ProjectName].JobTemplate.parameters.json   

Os parâmetros padrão no arquivo parameters.json das configurações em seu projeto do Visual Studio. Se você deseja implantar em outro ambiente, apenas substitua os parâmetros adequadamente.

> [!NOTE]
> Para todas as credenciais, os valores padrão são definidos como nulo. Eles **precisam** ser definidos antes de serem implantados na nuvem.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Saiba mais sobre como [implantar com um arquivo de modelo do Resource Manager e o Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Saiba mais sobre como [usar um objeto como um parâmetro em um modelo do Resource Manager](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

Para usar a Identidade Gerenciada para o Azure Data Lake Storage Gen1 como coletor de saída, você precisará fornecer Acesso à entidade de serviço usando o PowerShell antes da implantação no Azure. Saiba mais sobre como [implantar o ADLS Gen1 com a Identidade Gerenciada e o modelo do Resource Manager](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).


## <a name="command-line-tool"></a>Ferramenta de linha de comando

### <a name="build-the-project"></a>Compilar o projeto
O pacote NuGet tem uma ferramenta de linha de comando chamada **SA.exe**. Ela dá suporte ao build do projeto e a teste local em um computador aleatório, que você pode usar em seu processo de integração contínua e de entrega contínua. 

Por padrão, os arquivos de implantação são colocados no diretório atual. Você pode especificar o caminho de saída usando o parâmetro -OutputPath a seguir:

```
./tools/SA.exe build -Project [Your Project Full Path] [-OutputPath <outputPath>] 
```

### <a name="test-the-script-locally"></a>Testar o script localmente

Se o projeto tiver especificado arquivos de entrada locais no Visual Studio, você poderá executar o teste de script automatizado usando o comando *localrun*. O resultado da saída é colocado no diretório atual.
 
```
localrun -Project [ProjectFullPath]
```

### <a name="generate-a-job-definition-file-to-use-with-the-stream-analytics-powershell-api"></a>Gere um arquivo de definição de trabalho para usar com a API do PowerShell do Stream Analytics

O comando *arm* usa o modelo de trabalho e os arquivos de parâmetro de modelo do trabalho gerados por meio da compilação como entrada. Em seguida, ele os combina em um arquivo JSON de definição de trabalho que pode ser usado com a API do PowerShell do Stream Analytics.

```powershell
arm -JobTemplate <templateFilePath> -JobParameterFile <jobParameterFilePath> [-OutputFile <asaArmFilePath>]
```
Exemplo:
```powershell
./tools/SA.exe arm -JobTemplate "ProjectA.JobTemplate.json" -JobParameterFile "ProjectA.JobTemplate.parameters.json" -OutputFile "JobDefinition.json" 
```



## <a name="next-steps"></a>Próximas etapas

* [Início Rápido: Criar um trabalho de nuvem do Azure Stream Analytics no Visual Studio](stream-analytics-quick-create-vs.md)
* [Testar as consultas do Stream Analytics localmente com o Microsoft Visual Studio](stream-analytics-vs-tools-local-run.md)
* [Explore os trabalhos do Azure Stream Analytics com o Visual Studio](stream-analytics-vs-tools.md)

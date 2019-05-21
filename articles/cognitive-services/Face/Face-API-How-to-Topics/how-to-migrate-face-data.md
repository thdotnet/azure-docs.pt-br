---
title: Como migrar seus dados de detecção facial entre assinaturas — API de Detecção Facial
titleSuffix: Azure Cognitive Services
description: Este guia mostra como migrar seus dados de detecção facial armazenados de uma assinatura de API de Detecção Facial para outra.
services: cognitive-services
author: lewlu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: lewlu
ms.openlocfilehash: 702aed12860c090e83b997e6b56d56e06b416568
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65913795"
---
# <a name="migrate-your-face-data-to-a-different-face-subscription"></a>Migrar seus dados de detecção facial para uma assinatura diferente de Detecção Facial

Este guia mostra como mover dados de face, como um objeto PersonGroup salvo com faces, para uma assinatura diferente da API de detecção facial de serviços Cognitivos do Azure. Para mover os dados, você deve usar o recurso de instantâneo. Dessa forma você evitar repetidamente, criar e treinar um objeto PersonGroup ou FaceList quando você move ou expandir suas operações. Por exemplo, talvez você criou um objeto PersonGroup usando uma assinatura de avaliação gratuita e agora quiser migrá-la à sua assinatura paga. Ou talvez você precise sincronizar dados de face entre regiões para uma operação de grandes empresas.

Essa mesma estratégia de migração também se aplica aos objetos LargePersonGroup e LargeFaceList. Se você não estiver familiarizado com os conceitos neste guia, consulte suas definições na [enfrentam os conceitos de reconhecimento](../concepts/face-recognition.md) guia. Este guia usa a biblioteca cliente .NET da API de Detecção Facial com C#.

## <a name="prerequisites"></a>Pré-requisitos

Você precisa dos seguintes itens:

- Dois API de detecção facial chaves de assinatura, um com os dados existentes e outro para migrar para o. Para assinar o serviço de API de detecção facial e obtenha sua chave, siga as instruções em [criar uma conta de serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
- A cadeia de ID de assinatura de API de detecção facial que corresponde à assinatura de destino. Para encontrá-lo, selecione **visão geral** no portal do Azure. 
- Qualquer edição do [Visual Studio 2015 ou 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

Este guia usa um aplicativo de console simples para executar a migração de dados de face. Para uma implementação completa, consulte o [exemplo de instantâneo de API de detecção facial](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) no GitHub.

1. No Visual Studio, crie um novo projeto de .NET Framework do aplicativo de Console. Denomine **FaceApiSnapshotSample**.
1. Obtenha os pacotes necessários do NuGet. Clique em seu projeto no Gerenciador de soluções e selecione **gerenciar pacotes NuGet**. Selecione o **navegue** guia e, em seguida, selecione **incluir pré-lançamento**. Localizar e instalar o pacote a seguir:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.3.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="create-face-clients"></a>Criar clientes de detecção facial

No **principal** método na *Program.cs*, crie duas [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) instâncias de suas assinaturas de origem e destino. Este exemplo usa uma assinatura de Face na região da Ásia Oriental como a origem e uma assinatura do Oeste dos EUA como o destino. Este exemplo demonstra como migrar dados de uma região do Azure para outra. Se suas assinaturas estão em regiões diferentes, altere o `Endpoint` cadeias de caracteres.

```csharp
var FaceClientEastAsia = new FaceClient(new ApiKeyServiceClientCredentials("<East Asia Subscription Key>"))
    {
        Endpoint = "https://southeastasia.api.cognitive.microsoft.com/>"
    };

var FaceClientWestUS = new FaceClient(new ApiKeyServiceClientCredentials("<West US Subscription Key>"))
    {
        Endpoint = "https://westus.api.cognitive.microsoft.com/"
    };
```

Preencha os valores de chave de assinatura e URLs de ponto de extremidade para suas assinaturas de origem e destino.


## <a name="prepare-a-persongroup-for-migration"></a>Preparar um PersonGroup para migração

Você precisa da ID do PersonGroup em sua assinatura de código-fonte para migrá-lo para a assinatura de destino. Use o [PersonGroupOperationsExtensions.ListAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperationsextensions.listasync?view=azure-dotnet) método para recuperar uma lista dos seus objetos PersonGroup. Em seguida, obtenha o [PersonGroup.PersonGroupId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.persongroup.persongroupid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_Models_PersonGroup_PersonGroupId) propriedade. Esse processo parece diferente com base em quais PersonGroup objetos você possui. Neste guia, a origem PersonGroup ID é armazenada em `personGroupId`.

> [!NOTE]
> O [código de exemplo](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) cria e treina um novo PersonGroup para migrar. Na maioria dos casos, você já deve ter um PersonGroup usar.

## <a name="take-a-snapshot-of-a-persongroup"></a>Tirar um instantâneo de um PersonGroup

Um instantâneo é um armazenamento remoto temporário para determinados tipos de dados de Face. Ele funciona como um tipo de área de transferência para copiar dados de uma assinatura para outra. Primeiro, você tirar um instantâneo dos dados na assinatura do código-fonte. Em seguida, aplicá-la a um novo objeto de dados na assinatura de destino.

Use FaceClient instância assinaturas da fonte para tirar um instantâneo do PersonGroup. Use [TakeAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperationsextensions.takeasync?view=azure-dotnet) com a ID de PersonGroup e ID. da assinatura de destino Se você tiver várias assinaturas de destino, você deve adicioná-los como entradas de matriz no terceiro parâmetro.

```csharp
var takeSnapshotResult = await FaceClientEastAsia.Snapshot.TakeAsync(
    SnapshotObjectType.PersonGroup,
    personGroupId,
    new[] { "<Azure West US Subscription ID>" /* Put other IDs here, if multiple target subscriptions wanted */ });
```

> [!NOTE]
> O processo de aplicação de instantâneos não interromper todas as chamadas regulares para a origem ou destino PersonGroups ou FaceLists. Não faça chamadas simultâneas que alteram o objeto de origem, como [chamadas de gerenciamento FaceList](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet) ou o [PersonGroup Train](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet) chamar, por exemplo. A operação de instantâneo pode ser executado antes ou depois que essas operações, ou poderá encontrar erros.

## <a name="retrieve-the-snapshot-id"></a>Recuperar a ID do instantâneo

O método usado para tirar instantâneos é assíncrono, portanto, você deve aguardar sua conclusão. Operações de instantâneo não podem ser canceladas. Nesse código, o `WaitForOperation` método monitora a chamada assíncrona. Ele verifica o status de cada 100 ms. Após a conclusão da operação, recuperar uma ID de operação analisando o `OperationLocation` campo. 

```csharp
var takeOperationId = Guid.Parse(takeSnapshotResult.OperationLocation.Split('/')[2]);
var operationStatus = await WaitForOperation(FaceClientEastAsia, takeOperationId);
```

Um típico `OperationLocation` valor parecido com este:

```csharp
"/operations/a63a3bdd-a1db-4d05-87b8-dbad6850062a"
```

O método auxiliar `WaitForOperation` está aqui:

```csharp
/// <summary>
/// Waits for the take/apply operation to complete and returns the final operation status.
/// </summary>
/// <returns>The final operation status.</returns>
private static async Task<OperationStatus> WaitForOperation(IFaceClient client, Guid operationId)
{
    OperationStatus operationStatus = null;
    do
    {
        if (operationStatus != null)
        {
            Thread.Sleep(TimeSpan.FromMilliseconds(100));
        }

        // Get the status of the operation.
        operationStatus = await client.Snapshot.GetOperationStatusAsync(operationId);

        Console.WriteLine($"Operation Status: {operationStatus.Status}");
    }
    while (operationStatus.Status != OperationStatusType.Succeeded
            && operationStatus.Status != OperationStatusType.Failed);

    return operationStatus;
}
```

Depois do status da operação mostra `Succeeded`, obtenha a ID do instantâneo analisando o `ResourceLocation` campo de instância OperationStatus retornada.

```csharp
var snapshotId = Guid.Parse(operationStatus.ResourceLocation.Split('/')[2]);
```

Um típico `resourceLocation` valor parecido com este:

```csharp
"/snapshots/e58b3f08-1e8b-4165-81df-aa9858f233dc"
```

## <a name="apply-a-snapshot-to-a-target-subscription"></a>Aplicar um instantâneo para uma assinatura de destino

Em seguida, crie o novo PersonGroup na assinatura de destino usando uma ID de gerado aleatoriamente. Em seguida, use a instância de FaceClient da assinatura de destino para aplicar o instantâneo para essa PersonGroup. Passe no instantâneo de ID e a nova ID PersonGroup.

```csharp
var newPersonGroupId = Guid.NewGuid().ToString();
var applySnapshotResult = await FaceClientWestUS.Snapshot.ApplyAsync(snapshotId, newPersonGroupId);
```


> [!NOTE]
> Um objeto de instantâneo é válido apenas por 48 horas. Somente tirar um instantâneo, se você pretende usá-lo para a migração de dados logo depois.

Solicitação de aplicar um instantâneo retorna outra ID da operação. Para obter essa ID, analisar o `OperationLocation` campo da instância applySnapshotResult retornado. 

```csharp
var applyOperationId = Guid.Parse(applySnapshotResult.OperationLocation.Split('/')[2]);
```

O processo de aplicação do instantâneo também é assíncrono, use novamente `WaitForOperation` aguardar sua conclusão.

```csharp
operationStatus = await WaitForOperation(FaceClientWestUS, applyOperationId);
```

## <a name="test-the-data-migration"></a>Testar a migração de dados

Depois de aplicar o instantâneo, o novo PersonGroup na assinatura de destino preenche com os dados de face original. Por padrão, os resultados de treinamento também são copiados. O novo PersonGroup está pronto para chamadas de identificação de face sem a necessidade de novos treinamentos.

Para testar a migração de dados, execute as seguintes operações e comparar os resultados ao imprimir no console:

```csharp
await DisplayPersonGroup(FaceClientEastAsia, personGroupId);
await IdentifyInPersonGroup(FaceClientEastAsia, personGroupId);

await DisplayPersonGroup(FaceClientWestUS, newPersonGroupId);
// No need to retrain the person group before identification,
// training results are copied by snapshot as well.
await IdentifyInPersonGroup(FaceClientWestUS, newPersonGroupId);
```

Use os seguintes métodos auxiliares:

```csharp
private static async Task DisplayPersonGroup(IFaceClient client, string personGroupId)
{
    var personGroup = await client.PersonGroup.GetAsync(personGroupId);
    Console.WriteLine("Person Group:");
    Console.WriteLine(JsonConvert.SerializeObject(personGroup));

    // List persons.
    var persons = await client.PersonGroupPerson.ListAsync(personGroupId);

    foreach (var person in persons)
    {
        Console.WriteLine(JsonConvert.SerializeObject(person));
    }

    Console.WriteLine();
}
```

```csharp
private static async Task IdentifyInPersonGroup(IFaceClient client, string personGroupId)
{
    using (var fileStream = new FileStream("data\\PersonGroup\\Daughter\\Daughter1.jpg", FileMode.Open, FileAccess.Read))
    {
        var detectedFaces = await client.Face.DetectWithStreamAsync(fileStream);

        var result = await client.Face.IdentifyAsync(detectedFaces.Select(face => face.FaceId.Value).ToList(), personGroupId);
        Console.WriteLine("Test identify against PersonGroup");
        Console.WriteLine(JsonConvert.SerializeObject(result));
        Console.WriteLine();
    }
}
```

Agora você pode usar o novo PersonGroup na assinatura de destino. 

Para atualizar o destino PersonGroup novamente no futuro, crie um novo PersonGroup para receber o instantâneo. Para fazer isso, siga as etapas neste guia. Um único objeto PersonGroup pode ter um instantâneo aplicado a ele apenas uma vez.

## <a name="clean-up-resources"></a>Limpar recursos

Após concluir a migração enfrentam os dados, exclua manualmente o objeto de instantâneo.

```csharp
await FaceClientEastAsia.Snapshot.DeleteAsync(snapshotId);
```

## <a name="next-steps"></a>Próximas etapas

Em seguida, consulte a documentação de referência de API relevante, explorar um aplicativo de exemplo que usa o recurso de instantâneo ou seguir um guia de instruções para começar a usar as operações de API que mencionei aqui:

- [Documentação de referência do instantâneo (SDK do .NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)
- [Exemplo de instantâneo de API de detecção facial](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample)
- [Adicionar faces](how-to-add-faces.md)
- [Detecte rostos em uma imagem](HowtoDetectFacesinImage.md)
- [Identifique rostos em uma imagem](HowtoIdentifyFacesinImage.md)

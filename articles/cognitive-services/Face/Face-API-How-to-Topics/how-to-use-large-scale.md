---
title: 'Exemplo: usar o recurso em larga escala – API de Detecção Facial'
titleSuffix: Azure Cognitive Services
description: Use o recurso em larga escala na API de Detecção Facial.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 05/01/2019
ms.author: sbowles
ms.openlocfilehash: 35ab2d36a5d6c9977398fdbc16ba22eb1d9656a4
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65229842"
---
# <a name="example-how-to-use-the-large-scale-feature"></a>Exemplo: Como usar o recurso de larga escala

Este guia é um artigo avançado sobre como ampliar do **PersonGroup** e **FaceList** para **LargePersonGroup** e **LargeFaceList**, respectivamente. Este guia demonstra o processo de migração e pressupõe uma familiaridade básica com o **PersonGroup**, **FaceList**, a operação [Treinar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4) e as funções de reconhecimento de face. Confira o guia conceitual [​​Reconhecimento facial](../concepts/face-recognition.md) para saber mais sobre isso.

LargePersonGroup e LargeFaceList são coletivamente denominados operações em larga escala. O LargePersonGroup pode conter até 1.000.000 de pessoas, cada uma com um máximo de 248 faces, e a LargeFaceList pode conter até 1.000.000 de faces. As operações de grande escala são semelhantes às do PersonGroup e FaceList convencionais, mas apresentam algumas diferenças notáveis ​​devido à nova arquitetura. 

As amostras são gravadas em C# usando a biblioteca de clientes da API de Detecção Facial.

> [!NOTE]
> Para habilitar o desempenho da pesquisa de Face para identificação e FindSimilar em grande escala, você precisa introduzir uma operação de treinamento para pré-processar o LargeFaceList e LargePersonGroup. O tempo de treinamento varia de segundos para cerca de meia hora dependendo da capacidade real. Durante o período de treinamento, ainda é possível executar identificação e FindSimilar se for feito um treinamento com êxito antes. No entanto, a desvantagem é que as novas pessoas/faces adicionadas não aparecerão no resultado até que uma nova migração de publicação para treinamento em larga escala seja concluída.

## <a name="step-1-initialize-the-client-object"></a>Etapa 1: Inicializar o objeto de cliente

Ao usar uma biblioteca de clientes da API de Detecção Facial, a chave de assinatura e o ponto de extremidade de assinatura são passados para o construtor da classe FaceServiceClient. Por exemplo: 

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

A chave de assinatura com o ponto de extremidade correspondente pode ser obtida na página do Marketplace do seu portal do Microsoft Azure.
Consulte [Assinaturas](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="step-2-code-migration"></a>Etapa 2: Migração de código

Esta seção somente se concentra na implementação de migração  PersonGroup/FaceList para LargePersonGroup/LargeFaceList. Embora LargePersonGroup/LargeFaceList seja diferente de PersonGroup/FaceList no design e implementação interna, as interfaces de API são semelhantes para compatibilidade com versões anteriores.

Não há suporte para a migração de dados, você tem de recriar o LargePersonGroup/LargeFaceList em vez disso.

### <a name="migrate-persongroup-to-largepersongroup"></a>Migrar PersonGroup para LargePersonGroup

A migração do PersonGroup para o LargePersonGroup é simples, pois compartilham exatamente as mesmas operações no nível do grupo.

Para PersonGroup/Pessoa relacionado à implementação, só é necessário alterar os caminhos de API ou o módulo da classe SDK para LargePersonGroup e LargePersonGroup Pessoa.

Você precisa adicionar todas as faces e pessoas do PersonGroup ao novo LargePersonGroup. Confira [Como adicionar faces](how-to-add-faces.md) para referência.

### <a name="migrate-facelist-to-largefacelist"></a>Migrar FaceList para LargeFaceList

| APIs de FaceList | APIs de LargeFaceList |
|:---:|:---:|
| Criar | Criar |
| Excluir | Excluir |
| Obter | Obter |
| Listar | Listar |
| Atualizar | Atualizar |
| - | Treinar |
| - | Obter Status de Treinamento |

A tabela anterior é uma comparação de operações de nível de lista entre FaceList e LargeFaceList. Como é mostrado, o LargeFaceList vem com novas operações, Treinar e Obter Status de Treinamento, quando comparado com FaceList. Obter o LargeFaceList treinado é uma pré-condição da operação [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) enquanto não há nenhum Treinamento ecessário para FaceList. O snippet a seguir é uma função auxiliar para aguardar o treinamento de um LargeFaceList.

```CSharp
/// <summary>
/// Helper function to train LargeFaceList and wait for finish.
/// </summary>
/// <remarks>
/// The time interval can be adjusted considering the following factors:
/// - The training time which depends on the capacity of the LargeFaceList.
/// - The acceptable latency for getting the training status.
/// - The call frequency and cost.
///
/// Estimated training time for LargeFaceList in different scale:
/// -     1,000 faces cost about  1 to  2 seconds.
/// -    10,000 faces cost about  5 to 10 seconds.
/// -   100,000 faces cost about  1 to  2 minutes.
/// - 1,000,000 faces cost about 10 to 30 minutes.
/// </remarks>
/// <param name="largeFaceListId">The Id of the LargeFaceList for training.</param>
/// <param name="timeIntervalInMilliseconds">The time interval for getting training status in milliseconds.</param>
/// <returns>A task of waiting for LargeFaceList training finish.</returns>
private static async Task TrainLargeFaceList(
    string largeFaceListId,
    int timeIntervalInMilliseconds = 1000)
{
    // Trigger a train call.
    await FaceServiceClient.TrainLargeFaceListAsync(largeFaceListId);

    // Wait for training finish.
    while (true)
    {
        Task.Delay(timeIntervalInMilliseconds).Wait();
        var status = await FaceServiceClient.GetLargeFaceListTrainingStatusAsync(largeFaceListId);

        if (status.Status == Status.Running)
        {
            continue;
        }
        else if (status.Status == Status.Succeeded)
        {
            break;
        }
        else
        {
            throw new Exception("The train operation is failed!");
        }
    }
}
```

Anteriormente, um uso típico do FaceList com adição de faces e FindSimilar seria semelhante ao seguinte:

```CSharp
// Create a FaceList.
const string FaceListId = "myfacelistid_001";
const string FaceListName = "MyFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateFaceListAsync(FaceListId, FaceListName).Wait();

// Add Faces to the FaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToFaceListAsync(FaceListId, stream);
            }
        });

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, FaceListId, 20));
    }
}
```

Ao migrar para o LargeFaceList, ele deve se tornar o seguinte:

```CSharp
// Create a LargeFaceList.
const string LargeFaceListId = "mylargefacelistid_001";
const string LargeFaceListName = "MyLargeFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateLargeFaceListAsync(LargeFaceListId, LargeFaceListName).Wait();

// Add Faces to the LargeFaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToLargeFaceListAsync(LargeFaceListId, stream);
            }
        });

// Train() is newly added operation for LargeFaceList.
// Must call it before FindSimilarAsync() to ensure the newly added faces searchable.
await TrainLargeFaceList(LargeFaceListId);

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, largeFaceListId: LargeFaceListId));
    }
}
```

Como é mostrado acima, o gerenciamento de dados e a parte FindSimilar são quase os mesmos. A única exceção é que uma nova operação de Treino pré-processando deve concluir o LargeFaceList antes de FindSimilar funcionar.

## <a name="step-3-train-suggestions"></a>Etapa 3: Treinar sugestões

Embora a operação de treinamento acelere o [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) e [Identificação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), o tempo de treinamento é afetado principalmente quando se originar para grande escala. O tempo de treinamento estimado em escalas diferentes está listado na tabela a seguir:

| Escala (faces ou pessoas) | Tempo de treinamento estimado |
|:---:|:---:|
| 1.000 | 1-2 s |
| 10.000 | 5-10 s |
| 100.000 | 1 - 2 min |
| 1.000.000 | 10 - 30 min |

Para melhor utilizar o recurso de larga escala, algumas estratégias são recomendadas para levar em consideração.

## <a name="step-31-customize-time-interval"></a>Etapa 3.1: Personalizar o intervalo de tempo

Como é mostrado em `TrainLargeFaceList()`, há um `timeIntervalInMilliseconds` para atrasar o processo de verificação de status de treinamento infinito. Para LargeFaceList com mais faces, usando um intervalo maior que reduz as contas e custo de chamada. O intervalo de tempo deve ser personalizado de acordo com a capacidade esperada do LargeFaceList.

A mesma estratégia também se aplica ao LargePersonGroup. Por exemplo, durante o treinamento de um LargePersonGroup com 1.000.000 pessoas, o `timeIntervalInMilliseconds` pode ser 60.000 (intervalo de 1 minuto).

## <a name="step-32-small-scale-buffer"></a>Etapa 3.2: Buffer de pequena escala

Pessoas/Faces em LargePersonGroup/LargeFaceList são pesquisáveis apenas após ser treinado. Em um cenário dinâmico, novas pessoas/faces constantemente são adicionadas e precisam ser imediatamente pesquisáveis, ainda treinando pode levar mais tempo do que o desejado. Para atenuar esse problema, você pode usar um LargePersonGroup/LargeFaceList extra em pequena escala como um buffer somente para as entradas recém-adicionadas. Esse buffer leva menos tempo para treinar devido ao tamanho muito menor e a pesquisa imediata nesse buffer temporário deve funcionar. Use esse buffer em combinação com um treinamento no LargePersonGroup/LargeFaceList mestre executando o treinamento mestre em um intervalo mais esparso, por exemplo, a meia-noite e diariamente.

Fluxo de trabalho de exemplo:
1. Crie um LargePersonGroup/LargeFaceList mestre (conjunto mestre) e um buffer LargePersonGroup/LargeFaceList (coleção de buffer). A coleção de buffer é somente para as Pessoas/Faces recém-adicionadas.
1. Adicione novas Pessoas/Faces para a coleção mestre e a coleção de buffer.
1. Treine apenas a coleção de buffer com um intervalo de tempo curto para garantir que as entradas recém-adicionadas tenham efeito.
1. Chame Identificação/FindSimilar contra a coleção mestre e a coleção de buffer e mesclar os resultados.
1. Quando o tamanho do buffer de coleção aumenta a um limite ou em um tempo ocioso do sistema, crie uma nova coleção de buffer e dispare o treino em conjunto mestre.
1. Exclua a coleção do buffer antigo após o término de treinamento na coleção mestre.

## <a name="step-33-standalone-training"></a>Etapa 3.3: Treinamento autônomo

Se uma latência de longo prazo relativamente for aceitável, não é necessário disparar a operação de treinamento logo após a adição de novos dados. Em vez disso, a operação de treinamento pode dividir da lógica principal e disparada regularmente. Essa estratégia é adequada para cenários dinâmicos com uma latência aceitável e pode ser aplicada a cenários estáticos para reduzir ainda mais a frequência de Treinamento.

Suponha que há uma `TrainLargePersonGroup` função semelhante a `TrainLargeFaceList`. Uma implementação típica de Treinamento autônomo no LargePersonGroup invocando a [ `Timer` ](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx) classe em `System.Timers` seria:

```CSharp
private static void Main()
{
    // Create a LargePersonGroup.
    const string LargePersonGroupId = "mylargepersongroupid_001";
    const string LargePersonGroupName = "MyLargePersonGroupDisplayName";
    FaceServiceClient.CreateLargePersonGroupAsync(LargePersonGroupId, LargePersonGroupName).Wait();

    // Setup a standalone training at regular intervals.
    const int TimeIntervalForStatus = 1000 * 60; // 1 minute interval for getting training status.
    const double TimeIntervalForTrain = 1000 * 60 * 60; // 1 hour interval for training.
    var trainTimer = new Timer(TimeIntervalForTrain);
    trainTimer.Elapsed += (sender, args) => TrainTimerOnElapsed(LargePersonGroupId, TimeIntervalForStatus);
    trainTimer.AutoReset = true;
    trainTimer.Enabled = true;

    // Other operations like creating persons, adding faces and Identification except for Train.
    // ...
}

private static void TrainTimerOnElapsed(string largePersonGroupId, int timeIntervalInMilliseconds)
{
    TrainLargePersonGroup(largePersonGroupId, timeIntervalInMilliseconds).Wait();
}
```

Para obter mais informações sobre o gerenciamento de dados e implementações de identificação relacionada, consulte [Como Adicionar Faces](how-to-add-faces.md) e [Como Identificar Faces na Imagem](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a>Resumo

Neste guia, você aprendeu como migrar o código existente do PersonGroup/FaceList (não de dados) para o LargePersonGroup/LargeFaceList:

- LargePersonGroup e LargeFaceList funciona de modo semelhante ao PersonGroup/FaceList, exceto a operação de treinamento é exigida pelo LargeFaceList.
- Realize a estratégia de treinamento adequada para atualização de dados dinâmicos para o conjunto de dados em larga escala.

## <a name="next-steps"></a>Próximas etapas

Siga um guia de instruções para aprender como adicionar faces a um PersonGroup ou executar a operação Identificar em um PersonGroup.

- [Como Adicionar Faces](how-to-add-faces.md)
- [Como identificar faces em imagem](HowtoIdentifyFacesinImage.md)
---
title: Configurações de contêiner do Docker
titleSuffix: Language Understanding - Azure Cognitive Services
description: O ambiente de tempo de execução do contêiner do LUIS é configurado usando argumentos de comando `docker run`. O LUIS tem várias configurações obrigatórias e outras configurações opcionais.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: dapine
ms.openlocfilehash: 4a9f7762b7960c74acad8203f70bc1e7c7cbd90f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063228"
---
# <a name="configure-language-understanding-docker-containers"></a>Configurar contêineres do Docker do Serviço Inteligente de Reconhecimento Vocal 

O ambiente de tempo de execução do contêiner do LUIS (**Serviço Inteligente de Reconhecimento Vocal**) é configurado usando argumentos de comando `docker run`. O LUIS tem várias configurações obrigatórias e outras configurações opcionais. Há vários [exemplos](#example-docker-run-commands) do comando disponíveis. As configurações específicas do contêiner são as [configurações de montagem](#mount-settings) de entrada e as configurações de cobrança. 

## <a name="configuration-settings"></a>Definições de configuração

Esse contêiner tem as seguintes configurações:

|Obrigatório|Configuração|Finalidade|
|--|--|--|
|Sim|[ApiKey](#apikey-setting)|Usado para rastrear informações de cobrança.|
|Não|[ApplicationInsights](#applicationinsights-setting)|Permite que você adicione suporte a dados telemétricos do [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) para seu contêiner.|
|Sim|[Billing](#billing-setting)|Especifica o URI do ponto de extremidade do recurso de serviços no Azure.|
|Sim|[Eula](#eula-setting)| Indica que você aceitou a licença para o contêiner.|
|Não|[Fluentd](#fluentd-settings)|Gravar log e, opcionalmente, dados telemétricos em um servidor do Fluentd.|
|Não|[Proxy HTTP](#http-proxy-credentials-settings)|Configure um proxy HTTP para fazer solicitações de saída.|
|Não|[Logging](#logging-settings)|Fornece suporte a registro de log do ASP.NET Core para seu contêiner. |
|Sim|[Mounts](#mount-settings)|Lê e grava dados do computador host para o contêiner e do contêiner de volta para o computador host.|

> [!IMPORTANT]
> As configurações [`ApiKey`](#apikey-setting), [`Billing`](#billing-setting) e [`Eula`](#eula-setting) são usadas juntas e você deve fornecer valores válidos para todos os três; caso contrário, seu contêiner não será iniciado. Para obter mais informações sobre como usar essas configurações para instanciar um contêiner, consulte [Faturamento](luis-container-howto.md#billing).

## <a name="apikey-setting"></a>Configuração ApiKey

A configuração `ApiKey` especifica a chave de recurso do Azure usada para rastrear informações de cobrança do contêiner. Você deve especificar um valor para o ApiKey e o valor deve ser uma chave válida para o _dos serviços Cognitivos_ recurso especificado para o [ `Billing` ](#billing-setting) definição de configuração.

Essa configuração pode ser localizada no seguinte local:

* Portal do Azure: **Serviços cognitivos** gerenciamento de recursos, em **chaves**
* Portal do LUIS: página **Configurações de Ponto de Extremidade e Chaves**. 

Não use a chave inicial ou de criação. 

## <a name="applicationinsights-setting"></a>Configuração applicationInsights

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-setting"></a>Configuração de cobrança

O `Billing` configuração especifica o URI do ponto de extremidade do _os serviços Cognitivos_ recursos no Azure é usado para monitorar informações de cobrança para o contêiner. Você deve especificar um valor para este parâmetro de configuração e o valor deve ser um URI de ponto de extremidade válido para um _dos serviços Cognitivos_ recursos no Azure. O contêiner relata o uso a cada 10 a 15 minutos.

Essa configuração pode ser localizada no seguinte local:

* Portal do Azure: **Serviços cognitivos** visão geral, rotulado `Endpoint`
* Portal do LUIS: página **Chaves e configurações de Ponto de Extremidade**, como parte do URI do ponto de extremidade.

Lembre-se de incluir o `luis/v2.0` roteamento na URL, conforme mostrado na tabela a seguir:


|Obrigatório| NOME | Tipo de dados | DESCRIÇÃO |
|--|------|-----------|-------------|
|Sim| `Billing` | Cadeia de caracteres | URI do ponto de extremidade de cobrança<br><br>Exemplo:<br>`Billing=https://westus.api.cognitive.microsoft.com/luis/v2.0` |

## <a name="eula-setting"></a>Configuração de EULA

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Configurações de Fluentd


[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Configurações das credenciais de proxy HTTP

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Configurações de registro em log
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Configurações de montagem

Use montagens de associação para ler e gravar dados do contêiner e para ele. Você pode especificar uma montagem de entrada ou saída especificando a opção `--mount` no comando [docker run](https://docs.docker.com/engine/reference/commandline/run/). 

O contêiner do LUIS não usa montagens de entrada ou saída para armazenar dados de treinamento ou de serviço. 

A sintaxe exata do local da montagem do host varia de acordo com o sistema operacional do host. Além disso, o local de montagem do [computador host](luis-container-howto.md#the-host-computer) pode não estar acessível devido a um conflito entre as permissões usadas pela conta de serviço do Docker e as permissões do local de montagem do host. 

A tabela a seguir descreve as configurações com suporte.

|Obrigatório| NOME | Tipo de dados | DESCRIÇÃO |
|-------|------|-----------|-------------|
|Sim| `Input` | Cadeia de caracteres | O destino de montagem de entrada. O valor padrão é `/input`. Esse é o local dos arquivos de pacote do LUIS. <br><br>Exemplo:<br>`--mount type=bind,src=c:\input,target=/input`|
|Não| `Output` | Cadeia de caracteres | O destino de montagem de saída. O valor padrão é `/output`. Esse é o local dos logs. Isso inclui logs de consulta do LUIS e logs do contêiner. <br><br>Exemplo:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Comandos docker run de exemplo

Os exemplos a seguir usam as definições de configuração para ilustrar como escrever e usar comandos `docker run`.  Quando em execução, o contêiner continuará a ser executado até que você o [pare](luis-container-howto.md#stop-the-container).

* Esses exemplos usam o diretório de desativar o `c:` unidade para evitar conflitos de permissão no Windows. Se você precisar usar um diretório específico como o diretório de entrada, talvez seja necessário conceder ao Docker permissão de serviço. 
* não altere a ordem dos argumentos, a menos que você esteja familiarizado com contêineres do Docker.
* Se você estiver usando um sistema operacional diferente, use o console/terminal correto, a sintaxe de pasta para montagens e o caractere de continuação de linha para o seu sistema. Estes exemplos supõem que um console do Windows com um caractere de continuação de linha `^`. Como o contêiner é um sistema operacional Linux, a montagem de destino usa uma sintaxe de pasta de estilo do Linux.

Lembre-se de incluir o `luis/v2.0` roteamento na URL, conforme mostrado na tabela a seguir.

Substitua {_argument_name_} pelos seus próprios valores:

| Placeholder | Value | Formato ou exemplo |
|-------------|-------|---|
|{ENDPOINT_KEY} | A chave do ponto de extremidade do aplicativo LUIS treinado. |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT} | O valor de ponto de extremidade de cobrança está disponível no Azure `Cognitive Services` página de visão geral. |https://westus.api.cognitive.microsoft.com/luis/v2.0|

> [!IMPORTANT]
> As opções `Eula`, `Billing` e `ApiKey` devem ser especificadas para executar o contêiner; caso contrário, o contêiner não será iniciado.  Para mais informações, consulte [Faturamento](luis-container-howto.md#billing).
> O valor de ApiKey é o **chave** das chaves e os pontos de extremidade de página no portal do LUIS e também está disponível no Azure `Cognitive Services` página chaves de recurso. 

### <a name="basic-example"></a>Exemplo básico

O exemplo abaixo tem o menor número de argumentos possível para executar o contêiner:

```console
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={BILLING_ENDPOINT} ^
ApiKey={ENDPOINT_KEY}
```

### <a name="applicationinsights-example"></a>Exemplo de ApplicationInsights

O exemplo a seguir define o argumento ApplicationInsights para enviar telemetria ao Application Insights enquanto o contêiner está em execução:

```console
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={BILLING_ENDPOINT} ^
ApiKey={ENDPOINT_KEY} ^
InstrumentationKey={INSTRUMENTATION_KEY}
```

### <a name="logging-example"></a>Exemplo de registro em log 

O comando abaixo define o nível de registro em log, `Logging:Console:LogLevel`, para configurar o nível de registro em log como [`Information`](https://msdn.microsoft.com). 

```console
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis:latest ^
Eula=accept ^
Billing={BILLING_ENDPOINT} ^
ApiKey={ENDPOINT_KEY} ^
Logging:Console:LogLevel:Default=Information
```

## <a name="next-steps"></a>Próximas etapas

* Reveja [Como instalar e executar contêineres](luis-container-howto.md)
* Consulte a [Solução de problemas](troubleshooting.md) para resolver problemas relacionados à funcionalidade LUIS.
* Use mais [Contêineres de Serviços Cognitivos](../cognitive-services-container-support.md)

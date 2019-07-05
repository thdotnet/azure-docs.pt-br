---
title: Agendar tarefas de registro de contêiner do Azure
description: Definir temporizadores para executar uma tarefa de registro de contêiner do Azure em um agendamento definido.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/27/2019
ms.author: danlep
ms.openlocfilehash: a1123a30025f9be6e994e69703f5ee1aa05d1b49
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509697"
---
# <a name="run-an-acr-task-on-a-defined-schedule"></a>Executar uma tarefa ACR em um agendamento definido

Este artigo mostra como executar uma [tarefa ACR](container-registry-tasks-overview.md) em um agendamento. Agendar uma tarefa com a configuração de um ou mais *gatilhos de temporizador*. 

Agendamento de uma tarefa é útil para cenários como o seguinte:

* Execute uma carga de trabalho de contêiner para operações de manutenção agendada. Por exemplo, execute um aplicativo em contêineres para remover imagens desnecessárias do seu registro.
* Execute um conjunto de testes em uma imagem de produção durante o dia de trabalho como parte do seu monitoramento de site ativo.

Você pode usar o Azure Cloud Shell ou uma instalação local da CLI do Azure para executar os exemplos neste artigo. Se você gostaria de usá-lo localmente, versão 2.0.68 ou posterior é necessário. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure][azure-cli-install].


## <a name="about-scheduling-a-task"></a>Sobre o agendamento de uma tarefa

* **Disparador com expressão cron** -o gatilho de temporizador para uma tarefa usa um *expressão cron*. A expressão é uma cadeia de caracteres com cinco campos de especificação de minuto, hora, dia, mês e dia da semana para disparar a tarefa. Há suporte para as frequências de até uma vez por minuto. 

  Por exemplo, a expressão `"0 12 * * Mon-Fri"` dispara uma tarefa ao meio-dia UTC em cada dia da semana. Ver [detalhes](#cron-expressions) mais adiante neste artigo.
* **Vários gatilhos de temporizador** -adicionar vários temporizadores para uma tarefa é permitido, desde que as agendas são diferentes. 
    * Especifique vários gatilhos de temporizador quando você cria a tarefa ou adicioná-los posteriormente.
    * Opcionalmente, nomear os disparadores para facilitar o gerenciamento, ou tarefas de ACR fornecerá nomes de gatilho padrão.
    * Se as agendas de timer se sobrepõem por vez, tarefas de ACR dispara a tarefa no horário agendado para cada temporizador. 
* **Outros disparadores de tarefa** -uma tarefa disparada por temporizador, você também pode habilitar gatilhos com base no [confirmação de código fonte](container-registry-tutorial-build-task.md) ou [atualizações da imagem de base](container-registry-tutorial-base-image-update.md). Assim como outras tarefas ACR, você também pode [disparar manualmente][az-acr-task-run] uma tarefa agendada.

## <a name="create-a-task-with-a-timer-trigger"></a>Criar uma tarefa com um gatilho de temporizador

Quando você cria uma tarefa com o [az acr tarefa criar][az-acr-task-create] de comando, você pode opcionalmente adicionar um gatilho de temporizador. Adicionar o `--schedule` parâmetro e passar uma expressão cron para o temporizador. 

Como um exemplo simples, o seguinte comando em execução de gatilhos a `hello-world` imagem do Hub do Docker todos os dias às 21:00 UTC. A tarefa é executada sem um contexto de código fonte.

```azurecli
az acr task create \
  --name mytask \
  --registry myregistry \
  --context /dev/null \
  --cmd hello-world \
  --schedule "0 21 * * *"
```

Execute o [show do az acr tarefa][az-acr-task-show] comando para ver se o gatilho de timer é configurado. Por padrão, o gatilho de atualização de imagem de base também é habilitado.

```console
$ az acr task show --name mytask --registry registry --output table
NAME      PLATFORM    STATUS    SOURCE REPOSITORY       TRIGGERS
--------  ----------  --------  -------------------     -----------------
mytask    linux       Enabled                           BASE_IMAGE, TIMER
```

Disparar a tarefa manualmente com [tarefa de acr az][az-acr-task-run] para garantir que ele está configurado corretamente:

```azurecli
az acr task run --name mytask --registry myregistry
```

Se o contêiner é executado com êxito, a saída é semelhante ao seguinte:

```console
Queued a run with ID: cf2a
Waiting for an agent...
2019/06/28 21:03:36 Using acb_vol_2ca23c46-a9ac-4224-b0c6-9fde44eb42d2 as the home volume
2019/06/28 21:03:36 Creating Docker network: acb_default_network, driver: 'bridge'
[...]
2019/06/28 21:03:38 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

Após a hora agendada, execute as [az acr lista-será executada][az-acr-task-list-runs] para verificar que o timer acionado a tarefa, conforme o esperado:

```azurecli
az acr task list runs --name mytask --registry myregistry --output table
``` 

Quando o temporizador for bem-sucedido, a saída é semelhante ao seguinte:

```console
RUN ID    TASK     PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  -------- ----------  ---------  ---------  --------------------  ----------
[...]
cf2b      mytask   linux       Succeeded  Timer      2019-06-28T21:00:23Z  00:00:06
cf2a      mytask   linux       Succeeded  Manual     2019-06-28T20:53:23Z  00:00:06
```
            
## <a name="manage-timer-triggers"></a>Gerenciar gatilhos de temporizador

Use o [az acr tarefa de temporizador][az-acr-task-timer] comandos para gerenciar os gatilhos de temporizador para uma tarefa ACR.

### <a name="add-or-update-a-timer-trigger"></a>Adicionar ou atualizar um gatilho de temporizador

Depois que uma tarefa é criada, opcionalmente, adicionar um gatilho de temporizador, usando o [az acr tarefa de temporizador adicionar][az-acr-task-timer-add] comando. O exemplo a seguir adiciona um nome de gatilho de temporizador *timer2* à *mytask* criado anteriormente. Esse temporizador dispara a tarefa de todos os dias às 10:30 UTC.

```azurecli
az acr task timer add \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 10 * * *"
```

Atualizar o agendamento de um gatilho existente ou alterar seu status, usando o [atualização de timer de tarefa de acr az][az-acr-task-timer-update] comando. Por exemplo, atualize o disparador chamado *timer2* para disparar a tarefa às 11:30 UTC:

```azurecli
az acr task timer update \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 11 * * *"
```

### <a name="list-timer-triggers"></a>Gatilhos de temporizador de lista

O [lista de timer de tarefas de acr az][az-acr-task-timer-list] comando mostra os gatilhos de temporizador configurado para uma tarefa:

```azurecli
az acr task timer list --name mytask --registry myregistry
```

Saída de exemplo:

```JSON
[
  {
    "name": "timer2",
    "schedule": "30 11 * * *",
    "status": "Enabled"
  },
  {
    "name": "t1",
    "schedule": "0 21 * * *",
    "status": "Enabled"
  }
]
```

### <a name="remove-a-timer-trigger"></a>Remover um gatilho de temporizador 

Use o [remover de timer de tarefa de acr az][az-acr-task-timer-remove] comando para remover um gatilho de temporizador de uma tarefa. O exemplo a seguir remove a *timer2* disparar *mytask*:

```azurecli
az acr task timer remove \
  --name mytask \
  --registry myregistry \
  --timer-name timer2
```

## <a name="cron-expressions"></a>Expressões cron

Tarefas de ACR usa o [NCronTab](https://github.com/atifaziz/NCrontab) biblioteca interpretar expressões cron. Expressões com suporte nas tarefas de ACR tem cinco campos obrigatórios, separados por espaços em branco:

`{minute} {hour} {day} {month} {day-of-week}`

O fuso horário usado com expressões cron é o tempo Universal Coordenado (UTC). Horas estão no formato de 24 horas.

> [!NOTE]
> Tarefas de ACR não oferece suporte a `{second}` ou `{year}` campo em expressões de cron. Se você copiar uma expressão cron usada em outro sistema, certifique-se de remover esses campos, se eles forem usados.

Cada campo pode ter um dos seguintes tipos de valores:

|Type  |Exemplo  |Quando disparado  |
|---------|---------|---------|
|Um valor específico |<nobr>"5 * * * *"</nobr>|5 minutos após a hora a cada hora|
|Todos os valores (`*`)|<nobr>"* 5 * * *"</nobr>|a cada minuto de UTC hora início 5:00 (60 vezes por dia)|
|Um intervalo (`-` operador)|<nobr>"0 1-3 * * *"</nobr>|3 vezes por dia, às 1:00, 2:00 e 3:00 UTC|  
|Um conjunto de valores (`,` operador)|<nobr>"20,30,40 * * * *"</nobr>|3 vezes por hora, de 20 minutos, 30 minutos e 40 minutos depois da hora|
|Um valor de intervalo (`/` operador)|<nobr>"*/10 * * * *"</nobr>|6 vezes por hora, em 10 minutos, 20 minutos e assim por diante, depois da hora

[!INCLUDE [functions-cron-expressions-months-days](../../includes/functions-cron-expressions-months-days.md)]

### <a name="cron-examples"></a>Exemplos de cron

|Exemplo|Quando disparado  |
|---------|---------|
|`"*/5 * * * *"`|uma vez a cada cinco minutos|
|`"0 * * * *"`|uma vez a cada hora|
|`"0 */2 * * *"`|uma vez a cada duas horas|
|`"0 9-17 * * *"`|uma vez por hora de 9:00 para 17:00 UTC|
|`"30 9 * * *"`|em 9:30 UTC diariamente|
|`"30 9 * * 1-5"`|em 9:30 UTC cada dia da semana|
|`"30 9 * Jan Mon"`|em 9:30 UTC toda segunda-feira em janeiro|


## <a name="next-steps"></a>Próximas etapas

Para exemplos de tarefas disparada por confirmações de código de origem ou a imagem base, atualizações, confira a [série de tutoriais de ACR tarefas](container-registry-tutorial-quick-task.md).



<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks


<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-timer]: /cli/azure/acr/task/timer
[az-acr-task-timer-add]: /cli/azure/acr/task/timer#az-acr-task-timer-add
[az-acr-task-timer-remove]: /cli/azure/acr/task/timer#az-acr-task-timer-remove
[az-acr-task-timer-list]: /cli/azure/acr/task/timer#az-acr-task-timer-list
[az-acr-task-timer-update]: /cli/azure/acr/task/timer#az-acr-task-timer-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task]: /cli/azure/acr/task
[azure-cli-install]: /cli/azure/install-azure-cli
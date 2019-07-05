---
title: Referência de erro para verificação de integridade - registro de contêiner do Azure
description: Códigos de erro e possíveis soluções para problemas encontrados ao executar o comando de diagnóstico de verificação de integridade de acr az no registro de contêiner do Azure
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: fc29b27cbb7eea983140c59529d981ad95c27ae8
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555105"
---
# <a name="health-check-error-reference"></a>Referência de erro de verificação de integridade

A seguir está os detalhes sobre códigos de erro retornados pelo [az acr verificação de integridade][az-acr-check-health] comando. Para cada erro, as soluções possíveis estão listadas.

## <a name="dockercommanderror"></a>DOCKER_COMMAND_ERROR

Esse erro significa que o cliente do Docker para a CLI não pôde ser encontrado. Como resultado, as seguintes verificações adicionais não são executadas: encontrar status do daemon de versão do Docker, avaliar o Docker e executar um Docker pull comando.

*Possíveis soluções*: Instalar o cliente do Docker; Adicione o caminho de Docker para as variáveis do sistema.

## <a name="dockerdaemonerror"></a>DOCKER_DAEMON_ERROR

Esse erro significa que o status do daemon de Docker não está disponível ou que ele não pôde ser acessado usando a CLI. Como resultado, as operações de Docker (como `docker login` e `docker pull`) não estão disponíveis por meio da CLI.

*Possíveis soluções*: Reinicie o daemon do Docker ou validar que ele está instalado corretamente.

## <a name="dockerversionerror"></a>DOCKER_VERSION_ERROR

Esse erro significa que a CLI não foi capaz de executar o comando `docker --version`.

*Possíveis soluções*: Tente executar o comando manualmente, verifique se você tiver a versão mais recente da CLI e investigar a mensagem de erro.

## <a name="dockerpullerror"></a>DOCKER_PULL_ERROR

Esse erro significa que a CLI não pôde efetuar pull em uma imagem de exemplo para seu ambiente.

*Possíveis soluções*: Valide se todos os componentes necessários para efetuar pull de uma imagem estiverem sendo executado corretamente.

## <a name="helmcommanderror"></a>HELM_COMMAND_ERROR

Esse erro significa que o cliente Helm não foi possível encontrar pela CLI, que impede outras operações do Helm.

*Possíveis soluções*: Verifique se que o cliente do Helm está instalado e que seu caminho é adicionado às variáveis de ambiente do sistema.

## <a name="helmversionerror"></a>HELM_VERSION_ERROR

Esse erro significa que a CLI não pôde determinar a versão do Helm instalada. Isso pode acontecer se a versão da CLI do Azure (ou, se a versão do Helm) que está sendo usada está obsoleta.

*Possíveis soluções*: Atualizar para a versão mais recente da CLI do Azure ou para a versão recomendada do Helm; Execute o comando manualmente e investigar a mensagem de erro.

## <a name="connectivitydnserror"></a>CONNECTIVITY_DNS_ERROR

Esse erro significa que o DNS para o servidor de logon do registro específica foi um ping, mas não respondeu, que significa que ele não estiver disponível. Isso pode indicar alguns problemas de conectividade. Como alternativa, o registro pode não existir, o usuário pode não ter as permissões do registro (para recuperar seu servidor de logon corretamente) ou o registro de destino está em uma nuvem diferente daquele usado na CLI do Azure.

*Possíveis soluções*: Validar a conectividade; Verifique a ortografia do registro, e que o registro existe; Verifique se que o usuário tem as permissões corretas nele e que a nuvem do registro é o mesmo que é usado na CLI do Azure.

## <a name="connectivityforbiddenerror"></a>CONNECTIVITY_FORBIDDEN_ERROR

Esse erro significa que o ponto de extremidade de desafio para o registro de determinado respondeu com o status 403 Proibido HTTP. Esse erro significa que os usuários não têm acesso ao registro, provavelmente devido a uma configuração de rede virtual.

*Possíveis soluções*: Remover as regras de rede virtual ou adicionar o endereço IP do cliente atual à lista de permissões.

## <a name="connectivitychallengeerror"></a>CONNECTIVITY_CHALLENGE_ERROR

Esse erro significa que o ponto de extremidade do desafio de registro de destino não emitiu um desafio.

*Possíveis soluções*: Tente novamente após algum tempo. Se o erro persistir, abra um problema em https://aka.ms/acr/issues.

## <a name="connectivityaadloginerror"></a>CONNECTIVITY_AAD_LOGIN_ERROR

Esse erro significa que o ponto de extremidade do desafio de registro de destino emitiu um desafio, mas o registro não oferece suporte a autenticação do Active Directory do Azure.

*Possíveis soluções*: Tente outra maneira de autenticar, por exemplo, com credenciais de administrador. Se os usuários precisam se autenticar usando o Azure Active Directory, abra um problema em https://aka.ms/acr/issues.

## <a name="connectivityrefreshtokenerror"></a>CONNECTIVITY_REFRESH_TOKEN_ERROR

Esse erro significa que o servidor de logon do registro não respondeu com um token de atualização, portanto, o acesso negado ao registro de destino. Esse erro pode ocorrer se o usuário não tem as permissões corretas no registro ou se as credenciais do usuário para a CLI do Azure estão obsoletas.

*Possíveis soluções*: Verifique se o usuário tem as permissões corretas no registro. executar `az login` para atualizar as credenciais, tokens e permissões.

## <a name="connectivityaccesstokenerror"></a>CONNECTIVITY_ACCESS_TOKEN_ERROR

Esse erro significa que o servidor de logon do registro não respondeu com um token de acesso, para que o acesso ao registro de destino foi negado. Esse erro pode ocorrer se o usuário não tem as permissões corretas no registro ou se as credenciais do usuário para a CLI do Azure estão obsoletas.

*Possíveis soluções*: Verifique se o usuário tem as permissões corretas no registro. executar `az login` para atualizar as credenciais, tokens e permissões.

## <a name="loginservererror"></a>LOGIN_SERVER_ERROR

Esse erro significa que a CLI não foi possível localizar o servidor de logon do registro especificado e nenhum sufixo padrão foi encontrado para a nuvem atual. Esse erro pode ocorrer se o registro não existir, se o usuário não tem as permissões corretas no registro, se a nuvem do registro e a nuvem atual da CLI do Azure não correspondem, ou se a versão da CLI do Azure está obsoleta.

*Possíveis soluções*: Verifique se a ortografia está correta e se o registro existe; Verifique se que o usuário tem as permissões corretas no registro e as nuvens do registro e do ambiente de CLI corresponderem; Atualize a CLI do Azure para a versão mais recente.

## <a name="next-steps"></a>Próximas etapas

Para obter opções para verificar a integridade de um registro, consulte [verificar a integridade de um registro de contêiner do Azure](container-registry-check-health.md).

Consulte a [perguntas frequentes sobre](container-registry-faq.md) para perguntas frequentes e outros problemas conhecidos sobre o registro de contêiner do Azure.





<!-- LINKS - internal -->
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health

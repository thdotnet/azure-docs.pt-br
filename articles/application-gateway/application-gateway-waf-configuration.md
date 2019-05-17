---
title: Limites de tamanho de solicitação do Web application firewall e listas de exclusão no Gateway de aplicativo do Azure - portal do Azure
description: Este artigo fornece informações sobre limites de tamanho de solicitação e listas de exclusões de firewall de aplicativo Web no Gateway de Aplicativo com o Portal do Azure.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 5/15/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 5ddcdeca41e2f21fa27db25f7e0721c7ef87e491
ms.sourcegitcommit: 3675daec6c6efa3f2d2bf65279e36ca06ecefb41
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65620280"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists"></a>Limites de tamanho de solicitação e listas de exclusões de firewall do aplicativo Web

O WAF (firewall de aplicativo Web) do Gateway de Aplicativo do Azure fornece proteção para aplicativos Web. Este artigo descreve a configuração dos limites de tamanho de solicitação e listas de exclusões de WAF.

## <a name="waf-request-size-limits"></a>Limites de tamanho de solicitação de WAF

![Limites de tamanho de solicitação](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

O firewall do aplicativo Web permite a você configurar limites de tamanho de solicitação dentro de limites inferior e superior. As duas configurações de limites de tamanho a seguir estão disponíveis:

- O campo de tamanho de corpo de solicitação máxima é especificado em quilobytes e controles de limite de tamanho de solicitação geral excluindo qualquer arquivo carrega. Este campo pode variar de mínimo de 1 KB para o valor máximo de 128 KB. O valor padrão para o tamanho do corpo da solicitação é 128 KB.
- O campo de limite de carregamento de arquivo é especificado em MB e controla o tamanho máximo de carregamento de arquivos permitido. Esse campo pode ter um valor mínimo de 1 MB e um máximo de 500 MB para instâncias de SKU grande, enquanto o SKU médio tem um máximo de 100 MB. O valor padrão para o limite de carregamento de arquivos é 100 MB.

O WAF também oferece um botão configurável para ativar ou desativar a inspeção de corpo da solicitação. Por padrão, a inspeção de corpo da solicitação está habilitada. Se a inspeção de corpo de solicitação estiver desativada, o WAF não avalie o conteúdo do corpo da mensagem HTTP. Nesses casos, o WAF continua a impor regras de WAF no URI, cookies e cabeçalhos. Se a inspeção do corpo da solicitação estiver desativada, o campo de tamanho máximo do corpo da solicitação não será aplicável e não poderá ser definido. Desativar a inspeção de corpo da solicitação permite que mensagens maiores que 128 KB sejam enviadas ao WAF, mas o corpo da mensagem não é inspecionado quanto a vulnerabilidades.

## <a name="waf-exclusion-lists"></a>Listas de exclusão de WAF

![waf-exclusion.png](media/application-gateway-waf-configuration/waf-exclusion.png)

As listas de exclusões do WAF permitem a você omitir certos atributos de solicitação de uma avaliação do WAF. Um exemplo comum são os tokens inseridos do Active Directory que são usados para autenticação ou campos de senha. Esses atributos são propensos a conter os caracteres especiais que podem disparar um falso positivo das regras de WAF. Depois que um atributo é adicionado à lista de exclusões do WAF, ele não é levado em consideração por nenhuma regra do WAF configurada e ativa. As listas de exclusão são globais em escopo.

Os seguintes atributos podem ser adicionados a listas de exclusão:

* Cabeçalhos de solicitação
* Cookies de solicitação
* Nome do atributo de solicitação (args)

   * Dados de várias partes do formulário
   * XML
   * JSON
   * Args de consulta de URL

Você pode especificar um cabeçalho de solicitação exato, corpo, cookie ou uma correspondência de atributo de cadeia de consulta.  Outra opção é especificar correspondências parciais. A exclusão está sempre em um campo de cabeçalho, nunca em seu valor. Regras de exclusão são de escopo global e se aplicam a todas as páginas e todas as regras.

A seguir estão os operadores de critérios de correspondência com suporte:

- **Igual a**:  Esse operador é usado para uma correspondência exata. Como exemplo, para a seleção de um cabeçalho chamado **bearerToken**, use o operador equals com seletor definido como **bearerToken**.
- **Começa com**: Esse operador corresponde com todos os campos que começam com o valor do seletor especificado.
- **Termina com**:  Esse operador corresponde com todos os campos de solicitação que terminam com o valor do seletor especificado.
- **Contém**: Esse operador corresponde com todos os campos de solicitação que contenham o valor do seletor especificado.
- **Igual a qualquer**: Esse operador corresponde a todos os campos de solicitação. * será o valor do seletor.

Em todos os casos, a correspondência diferencia maiúsculas de minúsculas e a expressão regular não é permitida como seletor.

### <a name="examples"></a>Exemplos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Os exemplos a seguir demonstram o uso de exclusões.

### <a name="example-1"></a>Exemplo 1

Neste exemplo, você deseja excluir o cabeçalho de agente do usuário. O cabeçalho de solicitação do agente do usuário contém uma cadeia de caracteres de característica que permite que os pares de protocolo de rede identificar o tipo de aplicativo, o sistema operacional, o fornecedor de software ou a versão do software do agente do usuário solicitante do software. Para obter mais informações, consulte [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent).

Pode haver vários motivos para desabilitar a avaliar esse cabeçalho. Pode haver uma cadeia de caracteres que o WAF vê e suponha que ele seja mal-intencionado. Por exemplo, o ataque clássico de SQL "x = x" em uma cadeia de caracteres. Em alguns casos, isso pode ser um tráfego legítimo. Portanto, talvez seja necessário excluir esse cabeçalho de avaliação do WAF.

O seguinte cmdlet do PowerShell do Azure exclui o cabeçalho de agente do usuário da avaliação:

```azurepowershell
$exclusion1 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestHeaderNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "User-Agent"
```

### <a name="example-2"></a>Exemplo 2

Este exemplo exclui o valor de *usuário* parâmetro que é passado na solicitação por meio da URL. Por exemplo, é comum em seu ambiente para o campo de usuário conter uma cadeia de caracteres que o WAF exibições como conteúdo mal-intencionado, portanto, bloqueia-lo.  Você pode excluir o parâmetro de usuário nesse caso, para que o WAF não foi avaliada nada no campo.

O seguinte cmdlet do PowerShell do Azure exclui o parâmetro de usuário de avaliação:

```azurepowershell
$exclusion2 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestArgNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "user"
```
Portanto, se a URL **http://www.contoso.com/?user=fdafdasfda** é passado para o WAF, ele não avalia a cadeia de caracteres **fdafdasfda**.

## <a name="next-steps"></a>Próximas etapas

Depois de configurar as configurações de WAF, você pode aprender como exibir os logs de WAF. Para obter mais informações, consulte [Diagnósticos do Gateway de Aplicativo](application-gateway-diagnostics.md#diagnostic-logging).

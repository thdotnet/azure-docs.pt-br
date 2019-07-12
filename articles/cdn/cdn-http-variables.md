---
title: Variáveis HTTP para o mecanismo de regras de CDN do Azure |Microsoft Docs
description: As variáveis HTTP permitem recuperar metadados de solicitação e resposta HTTP.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus
ms.openlocfilehash: 53ad0c516547e17801bd57c2fd6b0d1704383797
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593810"
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>Variáveis HTTP para o mecanismo de regras de CDN do Azure
As variáveis HTTP fornecem os meios através dos quais você pode recuperar metadados de solicitação e resposta HTTP. Esses metadados, em seguida, podem ser usados para alterar dinamicamente uma solicitação ou resposta. O uso de variáveis HTTP é restrito aos recursos de mecanismo de regras a seguir:

- [Regravação da Chave de Cache](cdn-verizon-premium-rules-engine-reference-features.md#cache-key-rewrite)
- [Modificar Cabeçalho de Solicitação do Cliente](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-request-header)
- [Modificar Cabeçalho de Resposta do Cliente](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-response-header)
- [Redirecionamento de URL](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect)
- [Regravação de URL](cdn-verizon-premium-rules-engine-reference-features.md#url-rewrite)

## <a name="definitions"></a>Definições
A tabela a seguir descreve as variáveis HTTP compatíveis. Um valor em branco é retornado quando os metadados geográficos (por exemplo, código postal) não estão disponíveis para uma determinada solicitação.


| NOME | Variável | DESCRIÇÃO | Valor de exemplo |
| ---- | -------- | ----------- | ------------ |
| ASN (Solicitante) | %{geo_asnum} | Indica o número do sistema autônomo do solicitante. <br /><br />**Preterido:** %{virt_dst_asnum}. <br />Essa variável foi substituída por %{geo_asnum}. Embora uma regra que use essa variável preterida continue a funcionar, você deve atualizá-la para usar a nova variável. | AS15133 |
| Cidade (Solicitante) | %{geo_city} | Indica a cidade do solicitante. | Los Angeles |
| Continente (Solicitante) | %{geo_continent} | Indica o continente do solicitante por meio de sua abreviação. <br />Os valores válidos são: <br />AF: África<br />AS: Ásia<br />EU: Europa<br />NA: América do Norte<br />OC: Oceania<br />SA: América do Sul<br /><br />**Preterido:** %{virt_dst_continent}. <br />Essa variável foi preterida em favor de % {geo_continent}. <br />Embora uma regra que use essa variável preterida continue a funcionar, você deve atualizá-la para usar a nova variável.| N/D |
| Valor do cookie | %{cookie_Cookie} | Retorna o valor correspondente à chave do cookie identificada pelo termo Cookie. | Exemplo de uso: <br />%{cookie__utma}<br /><br />Valor de exemplo:<br />111662281.2.10.1222100123 |
| País (Solicitante) | %{geo_country} | Indica o país do solicitante de origem por meio de seu código de país. <br />**Preterido:** %{virt_dst_country}. <br /><br />Essa variável foi substituída por %{geo_country}. Embora uma regra que use essa variável preterida continue a funcionar, você deve atualizá-la para usar a nova variável. | EUA |
| Área de Mercado Designada (Solicitante) | %{geo_dma_code} |Indica o mercado de mídia do solicitante pelo seu código de região. <br /><br />Este campo só é aplicável às solicitações que se originam dos Estados Unidos.| 745 |
| Método de Solicitação HTTP | %{request_method} | Indica o método de solicitação HTTP. | GET |
| Código de status HTTP | %{status} | Indica o código de status HTTP da resposta. | 200 |
| Endereço IP (Solicitante) | %{virt_dst_addr} | Indica o endereço IP do solicitante. | 192.168.1.1 |
| Latitude (Solicitante) | %{geo_latitude} | Indica a latitude do solicitante. | 34.0995 |
| Longitude (Solicitante) | %{geo_longitude} | Indica a longitude do solicitante. | -118.4143 |
| Área Metropolitana Estatística (Solicitante) | %{geo_metro_code} | Indica a área metropolitana do solicitante. <br /><br />Este campo só é aplicável às solicitações que se originam dos Estados Unidos.<br />| 745 |
| Porta (Solicitante) | %{virt_dst_port} | Indica a porta efêmera do solicitante. | 55885 |
| Código Postal (Solicitante) | %{geo_postal_code} | Indica o código postal do solicitante. | 90210 |
| Cadeia de Consulta Encontrada | %{is_args} | O valor desta variável varia conforme a solicitação contém ou não uma cadeia de consulta.<br /><br />- Cadeia de Consulta Encontrada: ?<br />-Nenhuma cadeia de caracteres de consulta: NULO | ? |
| Parâmetro Cadeia de Consulta Encontrado | %{is_amp} | O valor desta variável será alterado, com variações, se a solicitação contiver pelo menos um parâmetro de cadeia de consulta.<br /><br />- Parâmetro Encontrado: &<br />-Sem parâmetros: NULO | & |
| Valor do Parâmetro Cadeia de Consulta | %{arg_&lt;parameter&gt;} | Retorna o valor correspondente ao parâmetro de cadeia de caracteres de consulta identificado pelo termo &lt;parameter&gt;. | Exemplo de uso: <br />%{arg_language}<br /><br />Exemplo de Parâmetro de Cadeia de Consulta: <br />?language=en<br /><br />Valor de exemplo: en |
| Valor da Cadeia de Consulta | %{query_string} | Indica o valor de cadeia de consulta definido na URL da solicitação. |key1=val1&key2=val2&key3=val3 |
| Domínio Referenciador | %{referring_domain} | Indica o domínio definido no cabeçalho de solicitação de referenciador. | <www.google.com> |
| Região (Solicitante) | %{geo_region} | Indica a região do solicitante (por exemplo, estado ou província) por meio de sua abreviação alfanumérica. | CA |
| Valor do Cabeçalho da Solicitação | %{http_RequestHeader} | Retorna o valor correspondente ao cabeçalho de solicitação identificado pelo termo RequestHeader. <br /><br />Se o nome do cabeçalho da solicitação contém um traço (por exemplo, User-Agent), substitua-o por um sublinhado (por exemplo, User_Agent).| Exemplo de Uso: % {http_Connection}<br /><br />Valor de exemplo: Keep-Alive | 
| Host de Solicitação | %{host} | Indica o host definido na URL da solicitação. | <www.mydomain.com> |
| Protocolo de Solicitação | %{request_protocol} | Indica o protocolo de solicitação. | HTTP/1.1 |
| Esquema de Solicitação | %{scheme} | Indica o esquema de solicitação. |http |
| URI de Solicitação (Relativo) | %{request_uri} | Indica o caminho relativo, incluindo a cadeia de consulta definida no URI de solicitação. | /marketing/foo.js?loggedin=true |
| URI de Solicitação (Relativo sem a cadeia de consulta) | %{uri} | Indica o caminho relativo para o conteúdo solicitado. <br /><br/>Informações de chave:<br />- Este caminho relativo exclui a cadeia de consulta.<br />- Este caminho relativo reflete regravações de URL. Uma URL será regravada sob as seguintes condições:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-Recurso de reconfiguração de URL: Esse recurso regrava o caminho relativo definido no URI da solicitação.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-URL de CNAME de borda: Este tipo de solicitação é reescrito para a URL da CDN correspondente. |/800001/corigin/rewrittendir/foo.js |
| URI da solicitação | %{request} | Descreve a solicitação. <br />Sintaxe: &lt;Método HTTP&gt; &lt;caminho relativo&gt; &lt;protocolo HTTP&gt; | GET /marketing/foo.js?loggedin=true HTTP/1.1 |
| Valor do Cabeçalho de Resposta | %{resp_&lt;ResponseHeader&gt;} | Retorna o valor correspondente ao cabeçalho da resposta identificado pelo termo &lt;ResponseHeader&gt;. <br /><br />Se o nome do cabeçalho da resposta contém um traço (por exemplo, User-Agent), substitua-o por um sublinhado (por exemplo, User_Agent). | Exemplo de uso: %{resp_Content_Length}<br /><br />Valor de exemplo: 100 |

## <a name="usage"></a>Uso
A tabela a seguir descreve a sintaxe adequada para especificar uma variável HTTP.


| Sintaxe | Exemplo | DESCRIÇÃO |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | %{host} | Use esta sintaxe para obter o valor inteiro correspondente à &lt;HTTPVariable&gt; especificada. |
| %{&lt;HTTPVariableDelimiter&gt;} | %{host,} | Use esta sintaxe para o valor inteiro correspondente ao &lt;HTTPVariableDelimiter&gt; especificado. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80} | Use uma expressão regular para &lt;HTTPVariableDelimiterExpression&gt; para substituir, excluir ou manipular o valor de uma variável HTTP. |

Nomes de variável HTTP suportam apenas caracteres alfabéticos e sublinhados. Converta caracteres sem suporte em sublinhados.

## <a name="delimiter-reference"></a>Referência de delimitador
Um delimitador pode ser especificado depois de uma variável HTTP para obter os seguintes efeitos:

- Transformar o valor associado à variável.

     Exemplo: Converte o valor inteiro em minúsculas.

- Excluir o valor associado à variável.

- Manipular o valor associado à variável.

     Exemplo: Use expressões regulares para alterar o valor associado à variável HTTP.

Os delimitadores estão descritos na tabela a seguir.

| Delimitador | DESCRIÇÃO |
| --------- | ----------- |
| := | Indica que um valor padrão será atribuído à variável quando ele for: <br />- Ausente <br />- Definido como NULL. |
| :+ | Indica que um valor padrão será atribuído à variável quando um valor tiver sido atribuído a ela. |
| : | Indica que uma subcadeia de caracteres do valor atribuído à variável será expandida. |
| # | Indica que o padrão especificado após este delimitador deve ser excluído quando ele é encontrado no início do valor associado à variável. |
| % | Indica que o padrão especificado após este delimitador deve ser excluído quando ele é encontrado no final do valor associado à variável. <br />Essa definição se aplica somente quando o símbolo % é usado como um delimitador. |
| / | Delimita uma variável HTTP ou um padrão. |
| // | Localiza e substitui todas as instâncias do padrão especificado. |
| /= | Localiza, copia e substitui todas as ocorrências do padrão especificado. |
| , | Converte o valor associado à variável HTTP em minúsculas. |
| ^ | Converte o valor associado à variável HTTP em maiúsculas. |
| ,, | Converte todas as instâncias do caractere especificado no valor associado à variável HTTP em minúsculas. |
| ^^ | Converte todas as instâncias do caractere especificado no valor associado à variável HTTP em maiúsculas. |

## <a name="exceptions"></a>Exceções
A tabela a seguir descreve as circunstâncias em que o texto especificado não é tratado como uma variável HTTP.

| Condição | DESCRIÇÃO | Exemplo |
| --------- | ----------- | --------|
| Símbolo de escape % | O símbolo de porcentagem pode ser ignorado com o uso de uma barra invertida. <br />O valor de exemplo à direita será tratado como um valor literal e não como uma variável HTTP.| \%{host} |
| Unknown variables | An empty string is always returned for unknown variables. | %{unknown_variable} |
| Invalid characters or syntax | Variables that contain invalid characters or syntax are treated as literal values. <br /><br />Example #1: The specified value contains an invalid character (for example, -). <br /><br />Example #2: The specified value contains a double set of curly braces. <br /><br />Example #3: The specified value is missing a closing curly brace.<br /> | Example #1: %{resp_user-agent} <br /><br />Example #2: %{{host}} <br /><br />Example #3: %{host |
| Missing variable name | A NULL value is always returned when a variable is not specified. | %{} |
| Trailing characters | Characters that trail a variable are treated as literal values. <br />The sample value to the right contains a trailing curly brace that will be treated as a literal value. | %{host}} |

## Setting default header values
A default value can be assigned to a header when it meets any of the following conditions:
- Missing/unset
- Set to NULL.

The following table describes how to define a default value.

| Condition | Syntax | Example | Description |
| --------- | ------ | --------| ----------- |
| Set a header to a default value when it meets any of the following conditions: <br /><br />- Missing Header <br /><br />- Header value is set to NULL.| %{Variable:=Value} | %{http_referrer:=unspecified} | The Referrer header will only be set to *unspecified* when it is either missing or set to NULL. No action will take place if it has been set. |
| Set a header to a default value when it is missing. | %{Variable=Value} | %{http_referrer=unspecified} | The Referrer header will only be set to *unspecified* when it is missing. No action will take place if it has been set. |
| Set the header to a default value when it does not meet any of the following conditions: <br /><br />- Missing<br /><br /> - Set to NULL. | %{Variable:+Value} | %{http_referrer:+unspecified} | The Referrer header will only be set to *unspecified* when a value has been assigned to it. No action will take place if it is either missing or set to NULL. |

## Manipulating variables
Variables can be manipulated in the following ways:

- Expanding substrings
- Removing patterns

### Substring expansion
By default, a variable will expand to its full value. Use the following syntax to only expand a substring of the variable's value.

`%<Variable>:<Offset>:<Length>}`

Key information:

- The value assigned to the Offset term determines the starting character of the substring:

     - Positive: The starting character of the substring is calculated from the first character in the string.
     - Zero: The starting character of the substring is the first character in the string.
     - Negative: The starting character of the substring is calculated from the last character in the string.

- The length of the substring is determined by the *Length* term:

     - Omitted: Omitting the Length term allows the substring to include all characters between the starting character and the end of the string.
     - Positive: Determines the length of the substring from the starting character to the right.
     - Negative: Determines the length of the substring from the starting character to the left.

#### Example:

The following example relies on the following sample request URL:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

The following string demonstrates various methods for manipulating variables:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Based on the sample request URL, the above variable manipulation will produce the following value:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### Pattern removal
Text that matches a specific pattern can be removed from either the beginning or the end of a variable's value.

| Syntax | Action |
| ------ | ------ |
| %{Variable#Pattern} | Remove text when the specified pattern is found at the beginning of a variable's value. |
| %{Variable%Pattern} | Remove text when the specified pattern is found at the end of a variable's value. |

#### Example:

In this sample scenario, the *request_uri* variable is set to:

`/800001/myorigin/marketing/product.html?language=en-US`

The following table demonstrates how this syntax works.

| Sample syntax | Results | |
| ------------- | ------- | --- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.html?language=en-US | Because the variable starts with the pattern, it was replaced. |
| %{request_uri%html}htm | /800001/myorigin/marketing/product.html?language=en-US | Because the variable doesn't end with the pattern, there was no change.|

### Find and replace
The find and replace syntax is described in the following table.

| Syntax | Action |
| ------ | ------ |
| %{Variable/Find/Replace} | Find and replace first occurrence of the specified pattern. |
| %{Variable//Find/Replace} | Find and replace all occurrences of the specified pattern. |
| %{Variable^} |Convert the entire value to uppercase. |
| %{Variable^Find} | Convert the first occurrence of the specified pattern to uppercase. |
| %{Variable,} | Convert the entire value to lowercase. |
| %{Variable,Find} | Convert the first occurrence of the specified pattern to lowercase. |

### Find and rewrite
For a variation on find and replace, use the text that matches the specified pattern when rewriting it. The find and rewrite syntax is described in the following table.

| Syntax | Action |
| ------ | ------ |
| %{Variable/=Find/Rewrite} | Find, copy, and rewrite all occurrences of the specified pattern. |
| %{Variable/^Find/Rewrite} | Find, copy, and rewrite the specified pattern when it occurs at the start of the variable. |
| %{Variable/$Find/Rewrite} | Find, copy, and rewrite the specified pattern when it occurs at the end of the variable. |
| %{Variable/Find} | Find and delete all occurrences of the specified pattern. |

Key information:

- Expand text that matches the specified pattern by specifying a dollar sign followed by a whole integer (for example, $1).

- Multiple patterns can be specified. The order in which the pattern is specified determines the integer that will be assigned to it. In the following example, the first pattern matches "www.," the second pattern matches the second-level domain, and the third pattern matches the top-level domain:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- The rewritten value can consist of any combination of text and these placeholders.

    In the previous example, the hostname is rewritten to `cdn.$2.$3:80` (for example, cdn.mydomain.com:80).

- The case of a pattern placeholder (for example, $1) can be modified through the following flags:
     - U: Uppercase the expanded value.

         Sample syntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: Lowercase the expanded value.

         Sample syntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- An operator must be specified before the pattern. The specified operator determines the pattern-capturing behavior:

     - `=`: Indicates that all occurrences of the specified pattern must be captured and rewritten.
     - `^`: Indicates that only text that starts with the specified pattern will be captured.
     - `$`: Indicates that only text that ends with the specified pattern will be capture.
 
- If you omit the */Rewrite* value, the text that matches the pattern is deleted.

\`{host}le: HTTP variables for Azure CDN rules engine | Microsoft Docs
description: HTTP variables allow you to retrieve HTTP request and response metadata.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''

ms.assetid: 
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus


---
# HTTP variables for Azure CDN rules engine
HTTP variables provide the means through which you can retrieve HTTP request and response metadata. This metadata can then be used to dynamically alter a request or a response. The use of HTTP variables is restricted to the following rules engine features:

- [Cache-Key Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#cache-key-rewrite)
- [Modify Client Request Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-request-header)
- [Modify Client Response Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-response-header)
- [URL Redirect](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect)
- [URL Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#url-rewrite)

## Definitions
The following table describes the supported HTTP variables. A blank value is returned when GEO metadata (for example, postal code) is unavailable for a particular request.


| Name | Variable | Description | Sample value |
| ---- | -------- | ----------- | ------------ |
| ASN (Requester) | %{geo_asnum} | Indicates the requester's AS number. <br /><br />**Deprecated:** %{virt_dst_asnum}. <br />This variable has been deprecated in favor of %{geo_asnum}. Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable. | AS15133 |
| City (Requester) | %{geo_city} | Indicates the requester's city. | Los Angeles |
| Continent (Requester) | %{geo_continent} | Indicates the requester's continent through its abbreviation. <br />Valid values are: <br />AF: Africa<br />AS: Asia<br />EU: Europe<br />NA: North America<br />OC: Oceania<br />SA: South America<br /><br />**Deprecated:** %{virt_dst_continent}. <br />This variable has been deprecated in favor of %{geo_continent}. <br />Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable.| N/A |
| Cookie Value | %{cookie_Cookie} | Returns the value corresponding to the cookie key identified by the Cookie term. | Sample Usage: <br />%{cookie__utma}<br /><br />Sample Value:<br />111662281.2.10.1222100123 |
| Country (Requester) | %{geo_country} | Indicates the requester's country of origin through its country code. <br />**Deprecated:** %{virt_dst_country}. <br /><br />This variable has been deprecated in favor of %{geo_country}. Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable. | US |
| Designated Market Area (Requester) | %{geo_dma_code} |Indicates the requester's media market by its region code. <br /><br />This field is only applicable to requests that originate from the United States.| 745 |
| HTTP Request Method | %{request_method} | Indicates the HTTP request method. | GET |
| HTTP Status Code | %{status} | Indicates the HTTP status code for the response. | 200 |
| IP Address (Requester) | %{virt_dst_addr} | Indicates the requester's IP address. | 192.168.1.1 |
| Latitude (Requester) | %{geo_latitude} | Indicates the requester's latitude. | 34.0995 |
| Longitude (Requester) | %{geo_longitude} | Indicates the requester's longitude. | -118.4143 |
| Metropolitan Statistical Area (Requester) | %{geo_metro_code} | Indicates the requester's metropolitan area. <br /><br />This field is only applicable to requests that originate from the United States.<br />| 745 |
| Port (Requester) | %{virt_dst_port} | Indicates the requester's ephemeral port. | 55885 |
| Postal Code (Requester) | %{geo_postal_code} | Indicates the requester's postal code. | 90210 |
| Query String Found | %{is_args} | The value for this variable varies according to whether the request contains a query string.<br /><br />- Query String Found: ?<br />- No Query String: NULL | ? |
| Query String Parameter Found | %{is_amp} | The value for this variable varies according to whether the request contains at least one query string parameter.<br /><br />- Parameter Found: &<br />- No Parameters: NULL | & |
| Query String Parameter Value | %{arg_&lt;parameter&gt;} | Returns the value corresponding to the query string parameter identified by the &lt;parameter&gt; term. | Sample Usage: <br />%{arg_language}<br /><br />Sample Query String Parameter: <br />?language=en<br /><br />Sample Value: en |
| Query String Value | %{query_string} | Indicates the entire query string value defined in the request URL. |key1=val1&key2=val2&key3=val3 |
| Referrer Domain | %{referring_domain} | Indicates the domain defined in the Referrer request header. | <www.google.com> |
| Region (Requester) | %{geo_region} | Indicates the requester's region (for example, state or province) through its alphanumeric abbreviation. | CA |
| Request Header Value | %{http_RequestHeader} | Returns the value corresponding to the request header identified by the RequestHeader term. <br /><br />If the name of the request header contains a dash (for example, User-Agent), replace it with an underscore (for example, User_Agent).| Sample Usage: %{http_Connection}<br /><br />Sample Value: Keep-Alive | 
| Request Host | %{host} | Indicates the host defined in the request URL. | <www.mydomain.com> |
| Request Protocol | %{request_protocol} | Indicates the request protocol. | HTTP/1.1 |
| Request Scheme | %{scheme} | Indicates the request scheme. |http |
| Request URI (Relative) | %{request_uri} | Indicates the relative path, including the query string, defined in the request URI. | /marketing/foo.js?loggedin=true |
| Request URI (Relative without query string) | %{uri} | Indicates the relative path to the requested content. <br /><br/>Key information:<br />- This relative path excludes the query string.<br />- This relative path reflects URL rewrites. A URL will be rewritten under the following conditions:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- URL Rewrite Feature: This feature rewrites the relative path defined in the request URI.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Edge CNAME URL: This type of request is rewritten to the corresponding CDN URL. |/800001/corigin/rewrittendir/foo.js |
| Request URI | %{request} | Describes the request. <br />Syntax: &lt;HTTP method&gt; &lt;relative path&gt; &lt;HTTP protocol&gt; | GET /marketing/foo.js?loggedin=true HTTP/1.1 |
| Response Header Value | %{resp_&lt;ResponseHeader&gt;} | Returns the value corresponding to the response header identified by the &lt;ResponseHeader&gt; term. <br /><br />If the name of the response header contains a dash (for example, User-Agent), replace it with an underscore (for example, User_Agent). | Sample Usage: %{resp_Content_Length}<br /><br />Sample Value: 100 |

## Usage
The following table describes the proper syntax for specifying an HTTP variable.


| Syntax | Example | Description |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | %{host} | Use this syntax to get the entire value corresponding to the specified &lt;HTTPVariable&gt;. |
| %{&lt;HTTPVariableDelimiter&gt;} | %{host,} | Use this syntax to set the case for the entire value corresponding to the specified  &lt;HTTPVariableDelimiter&gt;. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80} | Use a regular expression for &lt;HTTPVariableDelimiterExpression&gt; to replace, delete, or manipulate an HTTP variable's value. |

HTTP variable names only support alphabetic characters and underscores. Convert unsupported characters to underscores.

## Delimiter reference
A delimiter can be specified after an HTTP variable to achieve any of the following effects:

- Transform the value associated with the variable.

     Example: Convert the entire value to lowercase.

- Delete the value associated with the variable.

- Manipulate the value associated with the variable.

     Example: Use regular expressions to change the value associated with the HTTP variable.

The delimiters are described in the following table.

| Delimiter | Description |
| --------- | ----------- |
| := | Indicates that a default value will be assigned to the variable when it is either: <br />- Missing <br />- Set to NULL. |
| :+ | Indicates that a default value will be assigned to the variable when a value has been assigned to it. |
| : | Indicates that a substring of the value assigned to the variable will be expanded. |
| # | Indicates that the pattern specified after this delimiter should be deleted when it is found at the beginning of the value associated with the variable. |
| % | Indicates that the pattern specified after this delimiter should be deleted when it is found at the end of the value associated with the variable. <br />This definition is only applicable when the % symbol is used as a delimiter. |
| / | Delimits an HTTP variable or a pattern. |
| // | Find and replace all instances of the specified pattern. |
| /= | Find, copy, and rewrite all occurrences of the specified pattern. |
| , | Convert the value associated with the HTTP variable to lowercase. |
| ^ | Convert the value associated with the HTTP variable to uppercase. |
| ,, | Convert all instances of the specified character in the value associated with the HTTP variable to lowercase. |
| ^^ | Convert all instances of the specified character in the value associated with the HTTP variable to uppercase. |

## Exceptions
The following table describes circumstances under which the specified text isn't treated as an HTTP variable.

| Condition | Description | Example |
| --------- | ----------- | --------|
| Escaping % symbol | The percentage symbol can be escaped through the use of a backslash. <br />The sample value to the right will be treated as a literal value and not as an HTTP variable.| \%{host} |
| Variáveis desconhecidas | Uma cadeia de caracteres vazia é sempre retornada para variáveis desconhecidas. | %{unknown_variable} |
| Caracteres ou sintaxe inválidos | Variáveis que contêm caracteres ou sintaxe inválidos são tratadas como valores literais. <br /><br />Exemplo 1 #: O valor especificado contém um caractere inválido (por exemplo,-). <br /><br />Exemplo 2 de #: O valor especificado contém um conjunto duplo de chaves. <br /><br />Exemplo 3 #: O valor especificado não tem uma chave de fechamento.<br /> | Exemplo 1: %{resp_user-agent} <br /><br />Exemplo #2: %{{host}} <br /><br />Exemplo #3: %{host |
| Nome da variável ausente | Um valor NULL sempre é retornado quando uma variável não for especificada. | %{} |
| Caracteres à direita | Caracteres à direita de uma variável são tratados como valores literais. <br />O valor de exemplo à direita contém uma chave à direita que será tratada como um valor literal. | %{host}} |

## <a name="setting-default-header-values"></a>Definir valores de cabeçalho padrão
Um valor padrão pode ser atribuído a um cabeçalho quando ele atender a qualquer uma das seguintes condições:
- Ausente/não definido
- Definido como NULL.

A tabela a seguir descreve como definir um valor padrão.

| Condição | Sintaxe | Exemplo | DESCRIÇÃO |
| --------- | ------ | --------| ----------- |
| Defina um cabeçalho para um valor padrão quando ele atender a qualquer uma das seguintes condições: <br /><br />- Faltando cabeçalho <br /><br />- O valor do cabeçalho é definido como NULL.| %{Variable:=Value} | %{http_referrer:=unspecified} | O cabeçalho de referenciador só será definido como *não especificado* quando ele está ausente ou definido como NULL. Nenhuma ação ocorrerá se ele tiver sido definido. |
| Defina um cabeçalho para um valor padrão quando ele estiver ausente. | %{Variable=Value} | %{http_referrer=unspecified} | O cabeçalho de referenciador só será definido como *não especificado* quando estiver ausente. Nenhuma ação ocorrerá se ele tiver sido definido. |
| Defina o cabeçalho para um valor padrão quando ele não atender a qualquer uma das seguintes condições: <br /><br />- Ausente<br /><br /> - Definido como NULL. | %{Variable:+Value} | %{http_referrer:+unspecified} | O cabeçalho de referenciador só será definido como *não especificado* quando um valor foi atribuído a ele. Nenhuma ação ocorrerá se ele estiver ausente ou definido como NULL. |

## <a name="manipulating-variables"></a>Manipulação de variáveis
Variáveis podem ser manipuladas das seguintes maneiras:

- Expandindo subcadeias de caracteres
- Removendo padrões

### <a name="substring-expansion"></a>Expansão de subcadeia de caracteres
Por padrão, uma variável será expandida para seu valor total. Use a seguinte sintaxe para expandir somente uma subcadeia de caracteres do valor da variável.

`%<Variable>:<Offset>:<Length>}`

Informações de chave:

- O valor atribuído ao termo Deslocamento determina o caractere inicial da subcadeia de caracteres:

     - Positivo: O caractere inicial da subcadeia de caracteres é calculado a partir do primeiro caractere na cadeia de caracteres.
     - Zero: O caractere inicial da subcadeia de caracteres é o primeiro caractere na cadeia de caracteres.
     - Negativo: O caractere inicial da subcadeia de caracteres é calculado a partir do último caractere na cadeia de caracteres.

- O comprimento da subcadeia de caracteres é determinado pelo termo *Comprimento*:

     - Omitido: Omitir o termo de comprimento permite que a subcadeia de caracteres incluir todos os caracteres entre o caractere inicial e final da cadeia de caracteres.
     - Positivo: Determina o comprimento da subcadeia de caracteres do caractere inicial para a direita.
     - Negativo: Determina o comprimento da subcadeia de caracteres do caractere inicial para a esquerda.

#### <a name="example"></a>Exemplo:

O exemplo a seguir se baseia na URL de solicitação de exemplo a seguir:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

A cadeia de caracteres a seguir demonstra vários métodos para manipular variáveis:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Com base na URL de solicitação de exemplo, a manipulação de variável acima produzirá o seguinte valor:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### <a name="pattern-removal"></a>Remoção de padrão
O texto que coincide com um padrão específico pode ser removido do início ou final do valor da variável.

| Sintaxe | Ação |
| ------ | ------ |
| %{Variable#Pattern} | Remove o texto quando o padrão especificado é encontrado no início do valor da variável. |
| %{Variable%Pattern} | Remove o texto quando o padrão especificado é encontrado no final do valor da variável. |

#### <a name="example"></a>Exemplo:

Neste cenário de exemplo, a variável *request_uri* é definida como:

`/800001/myorigin/marketing/product.html?language=en-US`

A tabela a seguir demonstra como funciona essa sintaxe.

| Sintaxe de exemplo | Resultados | |
| ------------- | ------- | --- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.html?language=en-US | Como a variável inicia com o padrão, ela foi substituída. |
| %{request_uri%html}htm | /800001/myorigin/marketing/product.html?language=en-US | Como a variável não termina com o padrão, não houve nenhuma alteração.|

### <a name="find-and-replace"></a>Localizar e substituir
A sintaxe de localizar e substituir é descrita na tabela a seguir.

| Sintaxe | Ação |
| ------ | ------ |
| %{Variable/Find/Replace} | Localiza e substitui a primeira ocorrência do padrão especificado. |
| %{Variable//Find/Replace} | Localiza e substitui todas as ocorrências do padrão especificado. |
| %{Variable^} |Converte o valor inteiro em maiúsculas. |
| %{Variable^Find} | Converte a primeira ocorrência do padrão especificado em maiúsculas. |
| %{Variable,} | Converte o valor inteiro em minúsculas. |
| %{Variable,Find} | Converte a primeira ocorrência do padrão especificado em minúsculas. |

### <a name="find-and-rewrite"></a>Localizar e reescrever
Para uma variação de localizar e substituir, use o texto que corresponde ao padrão especificado quando reescrevê-los. A sintaxe de localizar e reescrever é descrita na tabela a seguir.

| Sintaxe | Ação |
| ------ | ------ |
| %{Variable/=Find/Rewrite} | Localiza, copia e substitui todas as ocorrências do padrão especificado. |
| %{Variable/^Find/Rewrite} | Localiza, copia e reescreve o padrão especificado quando ele ocorre no início da variável. |
| %{Variable/$Find/Rewrite} | Localizar, copiar e reescrever o padrão especificado quando ele ocorre no final da variável. |
| %{Variable/Find} | Localiza e exclui todas as ocorrências do padrão especificado. |

Informações de chave:

- Expanda o texto que corresponde ao padrão especificado, especificando um sinal de cifrão seguido por um inteiro (por exemplo, $1).

- Podem ser especificados vários padrões. A ordem na qual o padrão é especificado determina o número inteiro que será atribuído a ele. No exemplo a seguir, o primeiro padrão corresponde à “www.”, o segundo padrão corresponde ao domínio de segundo nível e o terceiro padrão corresponde ao domínio de nível superior:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- O valor reescrito pode consistir em qualquer combinação de texto e esses espaços reservados.

    No exemplo anterior, o nome do host foi reescrito para `cdn.$2.$3:80` (por exemplo, cdn.mydomain.com:80).

- O espaço reservado do padrão (por exemplo, $1) pode ser modificado para maiúsculas ou minúsculas através dos sinalizadores a seguir:
     - U: O valor expandido de letra maiuscula.

         Sintaxe de exemplo:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: Minúsculo o valor expandido.

         Sintaxe de exemplo:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- Um operador deve ser especificado antes do padrão. O operador especificado determina o comportamento padrão de captura:

     - `=`: Indica que todas as ocorrências de um padrão especificado devem ser capturadas e reescritas.
     - `^`: Indica que somente o texto que começa com o padrão especificado será capturado.
     - `$`: Indica que somente o texto que termina com o padrão especificado será captura.
 
- Se você omitir o valor */Reescrever* valor, o texto que corresponde ao padrão é excluído.



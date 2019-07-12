---
title: Condições de correspondência do mecanismo de regras de CDN do Azure da Verizon Premium | Microsoft Docs
description: Condições de correspondência do mecanismo de regras de documentação de referência para o Azure Content Delivery Network da Verizon Premium.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 1660dca34b2f128ef5889145fcdeed0d2523b9bb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593213"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-match-conditions"></a>Condições de correspondência CDN do Azure do mecanismo de regras da Verizon Premium

Este artigo lista descrições detalhadas das condições de correspondência disponíveis para o Azure Content Delivery Network (CDN) da Verizon Premium [mecanismo de regras](cdn-verizon-premium-rules-engine.md).

A segunda parte de uma regra é a condição de correspondência. Uma condição de correspondência identifica tipos específicos de solicitações para os quais um conjunto de recursos será executado.

Por exemplo, você pode usar uma condição de correspondência para:

- Filtrar solicitações para conteúdo em um local específico.
- Filtrar solicitações geradas de um determinado endereço IP ou país/região.
- Filtrar solicitações por informações de cabeçalho.

## <a name="always-match-condition"></a>Condição de correspondência Sempre

A condição de correspondência Sempre aplica um conjunto de recursos padrão a todas as solicitações.

Nome | Finalidade
-----|--------
[Sempre](#always) | Aplica um conjunto de recursos padrão para todas as solicitações.

## <a name="device-match-condition"></a>Condição de correspondência ispositivo

A condição de correspondência Dispositivo identifica solicitações feitas de um dispositivo móvel baseadas em suas propriedades.  

Nome | Finalidade
-----|--------
[Dispositivo](#device) | Identifica solicitações feitas de um dispositivo móvel baseadas em suas propriedades.

## <a name="location-match-conditions"></a>Condições de correspondência Localização

As condições de correspondência Localização identificam solicitações baseadas na localização do solicitante.

Nome | Finalidade
-----|--------
[Número AS](#as-number) | Identifica solicitações originadas de uma rede específica.
[País](#country) | Identifica solicitações originadas de países/regiões especificados.

## <a name="origin-match-conditions"></a>Condições de correspondência Origem

As condições de correspondência Origem identificam solicitações que apontem para o armazenamento da Rede de Distribuição de Conteúdo ou para um servidor de origem do cliente.

Nome | Finalidade
-----|--------
[Origem CDN](#cdn-origin) | Identifica solicitações para conteúdo armazenado no armazenamento da Rede de Distribuição de Conteúdo.
[Origem do Cliente](#customer-origin) | Identifica solicitações de conteúdo armazenado em um servidor de origem do cliente específico.

## <a name="request-match-conditions"></a>Condições de correspondência Solicitação

As condições de correspondência Solicitação identificam solicitações baseadas em suas propriedades.

Nome | Finalidade
-----|--------
[Endereço IP do cliente](#client-ip-address) | Identifica solicitações originadas de um endereço IP específico.
[Parâmetro de Cookie](#cookie-parameter) | Verifica o valor especificado nos cookies associados a cada solicitação.
[Regex de Parâmetro de Cookie](#cookie-parameter-regex) | Verifica a expressão regular especificada nos cookies associados a cada solicitação.
[Cname de Borda](#edge-cname) | Identifica solicitações que apontam para um CNAME de borda específico.
[Domínio de Referência](#referring-domain) | Identifica solicitações referenciadas dos nomes do host especificados.
[Literal de Cabeçalho de Solicitação](#request-header-literal) | Identifica solicitações que contêm o cabeçalho especificado definido como um valor especificado.
[Regex do Cabeçalho da Solicitação](#request-header-regex) | Identifica solicitações que contêm o cabeçalho especificado definido como um valor que corresponde à expressão regular especificada.
[Curinga de Cabeçalho de Solicitação](#request-header-wildcard) | Identifica solicitações que contêm o cabeçalho especificado definido como um valor que corresponda ao padrão especificado.
[Método de Solicitação](#request-method) | Identifica solicitações pelo método HTTP.
[Esquema de Solicitação](#request-scheme) | Identifica solicitações pelo protocolo HTTP.

## <a name="url-match-conditions"></a>Condições de correspondência URL

As condições de correspondência URL identificam solicitações baseadas em suas URLs.

Nome | Finalidade
-----|--------
[Diretório do Caminho da URL](#url-path-directory) | Identifica solicitações pelo caminho relativo.
[Extensão do Caminho da URL](#url-path-extension) | Identifica solicitações pela extensão de nome do arquivo.
[Nome do Arquivo do Caminho da URL](#url-path-filename) | Identifica solicitações pelo nome do arquivo.
[Literal do Caminho da URL](#url-path-literal) | Compara a caminho relativo da solicitação com o valor especificado.
[Regex do Caminho da URL](#url-path-regex) | Compara o caminho relativo da solicitação com a expressão regular especificada.
[Curinga do Caminho da URL](#url-path-wildcard) | Compara o caminho relativo da solicitação com o padrão especificado.
[Literal da Consulta da URL](#url-query-literal) | Compara a sequência da cadeia de caracteres de consulta da solicitação com o valor especificado.
[Parâmetro da Consulta da URL](#url-query-parameter) | Identifica solicitações que contêm o parâmetro de cadeia de caracteres de consulta especificado definido como um valor que corresponda a um padrão especificado.
[Regex da consulta da URL](#url-query-regex) | Identifica solicitações que contêm o parâmetro de cadeia de caracteres de consulta especificado definido como um valor que corresponda a uma expressão regular especificada.
[Curinga da consulta da URL](#url-query-wildcard) | Compara o valor especificado com a cadeia de caracteres de consulta da solicitação.

## <a name="reference-for-rules-engine-match-conditions"></a>Referência para condições de correspondência do mecanismo de regras

---

### <a name="always"></a>Sempre

A condição de correspondência Sempre aplica um conjunto de recursos padrão a todas as solicitações.

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="as-number"></a>Número AS

A rede de número AS é definida por seu número de sistema autônomo (ASN). 

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Número e Sistema Automático é atendida:

- **Corresponde**: exige que o ASN da rede do cliente corresponda a um dos ASNs especificados. 
- **Não corresponde**: exige que o ASN da rede do cliente não corresponda a nenhum dos ASNs especificados.

Informações de chave:

- Especifique vários ASNs delimitando cada um deles com um único espaço. Por exemplo, 64514 64515 corresponde a solicitações que chegam de 64514 ou 64515.
- Determinadas solicitações podem não retornar um ASN válido. Um ponto de interrogação (?) corresponderá a solicitações para as quais um ASN válido não pôde ser determinado.
- Especifique o ASN inteiro para a rede desejada. Não haverá correspondência de valores parciais.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="cdn-origin"></a>Origem CDN

A condição de correspondência de origem CDN é atendida quando as condições a seguir são atendidas:

- O conteúdo do armazenamento CDN foi solicitado.
- A URI de solicitação usa o tipo do ponto de acesso ao conteúdo (por exemplo, /000001) que está definido nessa condição de correspondência:
  - URL da CDN: o URI da solicitação deve conter o ponto de acesso ao conteúdo selecionado.
  - URL CNAME de borda: a configuração do CNAME de borda correspondente deve apontar para o ponto de acesso ao conteúdo selecionado.
  
Informações de chave:

- O ponto de acesso ao conteúdo identifica o serviço que deve ser o conteúdo solicitado.
- Não use uma instrução AND IF para combinar determinadas condições de correspondência. Por exemplo, combinar uma condição de correspondência de Origem CDN com uma condição de correspondência de Origem do Cliente cria um padrão de correspondência que não poderia ser satisfeito. Por esse motivo, as duas condições de correspondência de Origem de CDN não podem ser combinadas por meio de uma instrução AND IF.

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="client-ip-address"></a>Endereço IP do Cliente

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência de Endereço de IP de Cliente são atendidas:

- **Corresponde**: requer que o endereço IP do cliente corresponda a um dos endereços IP especificados. 
- **Não corresponde**: requer que o endereço IP do cliente não corresponda a nenhum dos endereços IP especificados. 

Informações de chave:

- Use a notação CIDR.
- Especifique vários endereços IP e/ou blocos de endereços IP delimitando cada um com um único espaço. Por exemplo:
  - **Exemplo de IPv4**: 1.2.3.4 10.20.30.40 corresponde a qualquer solicitação que chega dos endereços 1.2.3.4 ou 10.20.30.40.
  - **Exemplo de IPv6**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 corresponde a qualquer solicitação que chega dos endereços 1:2:3:4:5:6:7:8 ou 10:20:30:40:50:60:70:80.
- A sintaxe para um bloco de endereços IP é o endereço IP básico seguido por uma barra invertida e o tamanho do prefixo. Por exemplo:
  - **Exemplo de IPv4**: 5.5.5.64/26 corresponde a qualquer solicitação que chega dos endereços 5.5.5.64 a 5.5.5.127.
  - **Exemplo de IPv6**: 1:2:3:/48 corresponde a qualquer solicitação que chega dos endereços 1:2:3:0:0:0:0:0 por meio de 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="cookie-parameter"></a>Parâmetro de Cookie

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Parâmetro de Cookie são atendidas.

- **Corresponde**: requer que uma solicitação contenha o cookie especificado com um valor que corresponde a pelo menos um dos valores definidos nessa condição de correspondência.
- **Não corresponde**: requer que a solicitação atenda qualquer um dos critérios a seguir:
  - Ela não contém o cookie especificado.
  - Ela contém o cookie especificado, mas seu valor não corresponde a nenhum dos valores que são definidos nessa condição de correspondência.
  
Informações de chave:

- Nome do cookie:
  - Como não há suporte para valores curingas, incluindo asteriscos (*), quando você estiver especificando um nome de cookie, apenas as correspondências do nome exato de cookie são elegíveis para comparação.
  - Somente um nome de cookie único pode ser especificado por instância dessa condição de correspondência.
  - Comparações de nome de cookie diferenciam maiusculas de minúsculas.
- Valor do cookie:
  - Especifique vários valores de cookie delimitando cada um deles com um único espaço.
  - Um valor de cookie pode aproveitar os [valores curinga](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Se um valor curinga não tiver sido especificado, apenas uma correspondência exata vai satisfazer essa condição de correspondência. Por exemplo, especificar "Valor" corresponderá a "Valor", mas não "Valor1" ou "Valor2".
  - Use a opção **Ignorar maiúsculas** para controlar se uma comparação que diferencia maiúsculas de minúsculas será feita no valor do cookie da solicitação.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)
</br>

---

### <a name="cookie-parameter-regex"></a>Regex de Parâmetro de Cookie

A condição de correspondência de Regex de Parâmetro de Cookie define o valor e o nome de um cookie. Você pode usar [expressões regulares](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) para definir o valor do cookie desejado.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Regex de Parâmetro de Cookie são atendidas.

- **Corresponde**: requer que uma solicitação contenha o cookie especificado com um valor que corresponde à expressão regular especificada.
- **Não corresponde**: requer que a solicitação atenda qualquer um dos critérios a seguir:
  - Ela não contém o cookie especificado.
  - Ela contém o cookie especificado, mas seu valor não corresponde à expressão regular especificada.
  
Informações de chave:

- Nome do cookie:
  - Como não há suporte para expressões regulares e valores curingas, incluindo asteriscos (*), quando você estiver especificando um nome de cookie, apenas as correspondências do nome exato de cookie são elegíveis para comparação.
  - Somente um nome de cookie único pode ser especificado por instância dessa condição de correspondência.
  - Comparações de nome de cookie diferenciam maiusculas de minúsculas.
- Valor do cookie:
  - Um valor de cookie pode aproveitar expressões regulares.
  - Use a opção **Ignorar maiúsculas** para controlar se uma comparação que diferencia maiúsculas de minúsculas será feita no valor do cookie da solicitação.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="country"></a>País

Você pode especificar um país por meio de seu código de país. 

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência País são atendidas:

- **Corresponde**: requer que a solicitação contenha os valores de código de país especificados. 
- **Não corresponde**: requer que a solicitação não contenha os valores de código de país especificados.

Informações de chave:

- Especifique vários códigos de país delimitando cada um deles com um único espaço.
- Não há suporte para curingas quando você está especificando um código de país.
- Os códigos de país "EU" e "PA" não abrangem todos os endereços IP nessas regiões.
- Determinadas solicitações podem não retornar um código de país válido. Um ponto de interrogação (?) corresponderá a solicitações para as quais um código de pais válido não pôde ser determinado.
- Os códigos de país diferenciam maiúsculas de minúsculas.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

#### <a name="implementing-country-filtering-by-using-the-rules-engine"></a>Implementando a filtragem de país/região, usando o mecanismo de regras

Essa condição de correspondência permite que você execute uma variedade de personalizações com base no local de origem de uma solicitação. Por exemplo, o comportamento do recurso de filtragem de país pode ser replicado por meio da configuração a seguir:

- Correspondência do Curinga de Caminho da URL: defina a [condição de correspondência do Curinga de Caminho da URL](#url-path-wildcard) para o diretório que será protegido. 
    Acrescente um asterisco no final do caminho relativo para garantir que o acesso a todos os seus elementos filho sejam restritos por essa regra.

- Correspondência de país: defina a condição de correspondência de país para o conjunto desejado de países.
  - Permitir: defina a condição de correspondência de país como **Não corresponde** para permitir que apenas os países especificados tenham acesso ao conteúdo armazenado no local definido pela condição de correspondência do Curinga de Caminho da URL.
  - Bloquear: defina a condição de correspondência de país como **Corresponde** para bloquear o acesso dos países especificados ao conteúdo armazenado no local definido pela condição de correspondência do Curinga de Caminho da URL.

- Recurso do negar acesso (403): habilite o [Recurso negar acesso (403)](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) para replicar a porção permitir ou bloquear do recurso de filtragem de país.

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="customer-origin"></a>Origem do Cliente

Informações de chave:

- A condição de correspondência Origem do Cliente será atendida independentemente se o conteúdo for solicitado por meio de uma URL de CDN ou uma URL CNAME de borda que aponta para a origem de cliente selecionada.
- Uma configuração de origem de cliente que é referenciada por uma regra não pode ser excluída da página Origem do Cliente. Antes de tentar excluir uma configuração de origem do cliente, certifique-se de que as configurações a seguir fazem referência a ela:
  - Uma condição de correspondência Origem do Cliente
  - Uma configuração CNAME de borda
- Não use uma instrução AND IF para combinar determinadas condições de correspondência. Por exemplo, combinar uma condição de correspondência de Origem do Cliente com uma condição de correspondência de Origem de CDN cria um padrão de correspondência que não poderia ser satisfeito. Por esse motivo, as duas condições de correspondência de Origem do Cliente não podem ser combinadas por meio de uma instrução AND IF.

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="device"></a>Dispositivo

A condição de correspondência Dispositivo identifica solicitações feitas de um dispositivo móvel baseadas em suas propriedades. A detecção de dispositivos móveis é alcançada por meio de [WURFL](http://wurfl.sourceforge.net/). 

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Dispositivo são atendidas:

- **Corresponde**: requer que o dispositivo do solicitante corresponda ao valor especificado. 
- **Não corresponde**: requer que o dispositivo do solicitante não corresponda ao valor especificado.

Informações de chave:

- Use a opção **Ignorar maiúsculas** para especificar se o valor especificado diferencia maiúsculas de minúsculas.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

#### <a name="string-type"></a>Tipo de cadeia de caracteres

Normalmente, um recurso WURFL aceita qualquer combinação de números, letras e símbolos. Devido à natureza flexível dessa funcionalidade, você deve escolher como o valor associado a essa condição de correspondência é interpretado. A tabela a seguir descreve o conjunto disponível de opções:

Tipo     | DESCRIÇÃO
---------|------------
Literal  | Selecione esta opção para impedir que a maioria dos caracteres tenham um significado especial usando seus [valores literal](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Curinga | Selecione esta opção para aproveitar todos os [caracteres curinga] ([valores curinga](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Selecione esta opção para usar [Expressões regulares](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Expressões regulares são úteis para definir um padrão de caracteres.

#### <a name="wurfl-capabilities"></a>Recursos WURFL

Um recurso WURFL se refere a uma categoria que descreve dispositivos móveis. O recurso selecionado determina o tipo de descrição do dispositivo móvel que é usado para identificar solicitações.

A tabela a seguir lista recursos WURFL e suas variáveis para o mecanismo de regras.

> [!NOTE]
> As variáveis a seguir têm suporte nos recursos **Modificar Cabeçalho de Solicitação do Cliente** e **Modificar Cabeçalho de Resposta do Cliente**.

Recurso | Variável | DESCRIÇÃO | Valores de exemplo
-----------|----------|-------------|----------------
Nome da marca | %{wurfl_cap_brand_name} | Uma cadeia de caracteres que indica o nome da marca do dispositivo. | Samsung
SO do dispositivo | %{wurfl_cap_device_os} | Uma cadeia de caracteres que indica o sistema operacional instalado no dispositivo. | IOS
Versão do SO do dispositivo | %{wurfl_cap_device_os_version} | Uma cadeia de caracteres que indica o número de versão do sistema operacional instalado no dispositivo. | 1.0.1
Orientação dupla | %{wurfl_cap_dual_orientation} | Um booliano que indica se o dispositivo dá suporte a orientação dupla. | true
DTD preferencial HTML | %{wurfl_cap_html_preferred_dtd} | Uma cadeia de caracteres que indica a DTD (definição de tipo de documento) do dispositivo móvel para conteúdo HTML. | nenhum<br/>xhtml_basic<br/>html5
Inlining de Imagem | %{wurfl_cap_image_inlining} | Um booliano que indica se o dispositivo dá suporte a imagens codificadas de Base64. | false
É Android | %{wurfl_vcap_is_android} | Um booliano que indica se o dispositivo usa o sistema operacional Android. | true
É IOS | %{wurfl_vcap_is_ios} | Um booliano que indica se o dispositivo usa iOS. | false
É Smart TV | %{wurfl_cap_is_smarttv} | Um booliano que indica se o dispositivo é uma Smart TV. | false
É Smartphone | %{wurfl_vcap_is_smartphone} | Um booliano que indica se o dispositivo é um smartphone. | true
É Tablet | %{wurfl_cap_is_tablet} | Um booliano que indica se o dispositivo é um tablet. Essa descrição é independente do sistema operacional. | true
É dispositivo sem fio | %{wurfl_cap_is_wireless_device} | Um booliano que indica se o dispositivo é considerado um dispositivo sem fio. | true
Nome de marketing | %{wurfl_cap_marketing_name} | Uma cadeia de caracteres que indica o nome de marketing do dispositivo. | BlackBerry 8100 Pearl
Navegador de dispositivo móvel | %{wurfl_cap_mobile_browser} | Uma cadeia de caracteres que indica o navegador que é usado para solicitar o conteúdo do dispositivo. | Chrome
Versão do navegador móvel | %{wurfl_cap_mobile_browser_version} | Uma cadeia de caracteres que indica a versão do navegador que é usado para solicitar conteúdo do dispositivo. | 31
Nome do modelo | %{wurfl_cap_model_name} | Uma cadeia de caracteres que indica o nome do modelo do dispositivo. | s3
Download progressivo | %{wurfl_cap_progressive_download} | Um booliano que indica se o dispositivo dá suporte a reprodução de áudio e vídeo enquanto ele ainda está sendo baixado. | true
Data do lançamento | %{wurfl_cap_release_date} | Uma cadeia de caracteres que indica o ano e o mês em que o dispositivo foi adicionado ao banco de dados WURFL.<br/><br/>Formato: `yyyy_mm` | 2013_december
Altura de resolução | %{wurfl_cap_resolution_height} | Um inteiro que indica a altura do dispositivo em pixels. | 768
Largura de resolução | %{wurfl_cap_resolution_width} | Um inteiro que indica a largura do dispositivo em pixels. | 1024

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="edge-cname"></a>Cname de Borda

Informações de chave:

- A lista de CNAMEs de borda disponível é limitada aos CNAMEs de borda que foram configuradas na página dos CNAMEs de borda para a plataforma onde o mecanismo de regras está sendo configurado.
- Antes de tentar excluir uma configuração de CNAME de borda, certifique-se de que uma condição de correspondência de Cname de borda não faz referência a ela. Configurações de CNAME de borda que foram definidas em uma regra não podem ser excluídas da página de CNAMEs de borda.
- Não use uma instrução AND IF para combinar determinadas condições de correspondência. Por exemplo, combinar uma condição de correspondência de Cname de Borda com uma condição de correspondência de Origem do Cliente cria um padrão de correspondência que não poderia ser satisfeito. Por esse motivo, as duas condições de correspondência de Cname de Borda não podem ser combinadas por meio de uma instrução AND IF.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="referring-domain"></a>Domínio de Referência

O nome do host associado com o referenciador por meio do qual o conteúdo foi solicitado determina se a condição do domínio de referência é atendida.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Domínio de Referência são atendidas:

- **Corresponde**: requer que o nome do host de referência corresponda aos valores especificados. 
- **Não corresponde**: requer que o nome do host de referência não corresponda ao valor especificado.

Informações de chave:

- Especifique vários nomes de host delimitando cada um deles com um único espaço.
- Essa condição de correspondência dá suporte aos [valores curinga](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- Se o valor especificado não contiver um asterisco, ele deverá ser uma correspondência exata para o nome de host do referenciador. Por exemplo, especificar "mydomain.com" não corresponderia a "www.mydomain.com."
- Use a opção **Ignorar maiúsculas** para controlar se uma comparação que diferencia maiúsculas de minúsculas será feita.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---  

### <a name="request-header-literal"></a>Literal de Cabeçalho de Solicitação

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Literal do Cabeçalho da Solicitação será atendida.

- **Corresponde**: requer que a solicitação contenha o cabeçalho especificado. O valor deve corresponder ao que está definido nessa condição de correspondência.
- **Não corresponde**: requer que a solicitação atenda qualquer um dos critérios a seguir:
  - Ela não contém o cabeçalho especificado.
  - Ela contém o cabeçalho especificado, mas seu valor não corresponde ao que está definido nessa condição de correspondência.
  
Informações de chave:

- Comparações de nome de cabeçalho sempre diferenciam maiúsculas de minúsculas. Use a opção **Ignorar maiúsculas** para controlar a diferenciação de maiúsculas e minúsculas de comparações de valores de cabeçalho.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---  

### <a name="request-header-regex"></a>Regex do Cabeçalho da Solicitação

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Regex do Cabeçalho da Solicitação será atendida.

- **Corresponde**: requer que a solicitação contenha o cabeçalho especificado. O valor deve corresponder ao padrão que é definido na [expressão regular](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) especificada.
- **Não corresponde**: requer que a solicitação atenda qualquer um dos critérios a seguir:
  - Ela não contém o cabeçalho especificado.
  - Ela contém o cabeçalho especificado, mas seu valor não corresponde à expressão regular especificada.

Informações de chave:

- Nome do cabeçalho:
  - Comparações de nome de cabeçalho diferenciam maiúsculas de minúsculas.
  - Substitua espaços no nome do cabeçalho por "%20".
- Valor do cabeçalho:
  - Um valor de cabeçalho pode aproveitar expressões regulares.
  - Use a opção **Ignorar maiúsculas** para controlar a diferenciação de maiúsculas e minúsculas de comparações de valores de cabeçalho.
  - O critério de correspondência é atendido somente quando um valor de cabeçalho corresponde exatamente a pelo menos um dos padrões especificados.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-header-wildcard"></a>Curinga de Cabeçalho de Solicitação

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Curinga do Cabeçalho da Solicitação será atendida.

- **Corresponde**: requer que a solicitação contenha o cabeçalho especificado. Seu valor deve corresponder a pelo menos um dos valores que são definidos nessa condição de correspondência.
- **Não corresponde**: requer que a solicitação atenda qualquer um dos critérios a seguir:
  - Ela não contém o cabeçalho especificado.
  - Ela contém o cabeçalho especificado, mas seu valor não corresponde a nenhum dos valores especificados.
  
Informações de chave:

- Nome do cabeçalho:
  - Comparações de nome de cabeçalho diferenciam maiúsculas de minúsculas.
  - Espaços no nome do cabeçalho devem ser substituídos por "%20". Você também pode usar esse valor para especificar os espaços em um valor de cabeçalho.
- Valor do cabeçalho:
  - Um valor de cabeçalho pode aproveitar os [valores curinga](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Use a opção **Ignorar maiúsculas** para controlar a diferenciação de maiúsculas e minúsculas de comparações de valores de cabeçalho.
  - Esse critério de correspondência é atendido quando um valor de cabeçalho corresponde exatamente a pelo menos um dos padrões especificados.
  - Especifique vários valores delimitando cada um deles com um único espaço.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-method"></a>Método de Solicitação

A condição de correspondência do Método de Solicitação é atendido somente quando os ativos são solicitados por meio do método de solicitação selecionado. Os métodos de solicitação disponíveis são:

- OBTER
- HEAD
- POSTAR
- OPÇÕES
- PUT
- DELETE
- RASTREAMENTO
- CONECTAR

Informações de chave:

- Por padrão, o método de solicitação GET pode gerar o conteúdo armazenado em cache na rede. Todos os outros métodos de solicitação são solicitados por proxy através da rede.
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-scheme"></a>Esquema de Solicitação

A condição de correspondência do Esquema de Solicitação é atendido somente quando os ativos são solicitados por meio do protocolo selecionado. Os protocolos disponíveis são:

- HTTP
- HTTPS

Informações de chave:

- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
  - Concluir o Preenchimento do Cache
  - Idade Máxima Interna Padrão
  - Forçar Idade Máxima Interna
  - Ignorar Ausência de Cache de Origem
  - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-directory"></a>Diretório do Caminho da URL

Identifica uma solicitação por seu caminho relativo, que exclui o nome do arquivo do ativo solicitado.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Diretório de Caminho de URL são atendidas.

- **Corresponde**: requer que a solicitação contenha um caminho de URL relativa, exceto pelo nome do arquivo, que corresponda ao padrão de URL especificado.
- **Não corresponde**: requer que a solicitação contenha um caminho de URL relativa, exceto pelo nome do arquivo, que não corresponda ao padrão de URL especificado.

Informações de chave:

- Use a opção **Relativo a** para especificar se a comparação de URL começa antes ou após o ponto de acesso do conteúdo. O ponto de acesso de conteúdo é a parte do caminho que aparece entre o nome do host Verizon CDN e o caminho relativo ao ativo solicitado (por exemplo, /800001/CustomerOrigin). Ele identifica um local por tipo de servidor (por exemplo, origem de cliente ou CDN) e seu número de conta do cliente.

   Os valores a seguir estão disponíveis para a opção **Relativo a**:
  - **Raiz**: indica que o ponto de comparação de URL começa logo após o nome de host da CDN. 

  Por exemplo: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origem**: indica que o ponto de comparação de URL começa após o ponto de acesso do conteúdo (por exemplo, /000001 ou /800001/myorigin). Como o \*.azureedge.net CNAME é criado em relação ao diretório de origem no nome do host Verizon CDN por padrão, os usuários do Azure CDN devem usar o valor de **Origem**. 

  Por exemplo: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  Essa URL aponta para o seguinte nome do host da CDN da Verizon: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- Uma URL CNAME de borda é reescrita para uma URL CDN antes da comparação de URL.

    Por exemplo, ambas as URLs a seguir apontam para o mesmo ativo e, portanto, têm o mesmo caminho de URL.
  - URL da CDN: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - URL de CNAME de borda: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Informações adicionais:
  - Domínio personalizado: https:\//my.domain.com/path/asset.htm
    
    - Caminho da URL (relativo à raiz): /800001/CustomerOrigin/path/
    
    - Caminho da URL (relativo à origem): /path/

- A parte da URL que é usada para a comparação de URL termina antes do nome do arquivo do ativo solicitado. Uma barra invertida é o último caractere nesse tipo de caminho.

- Substitua os espaços no padrão de caminho de URL por "%20".

- Cada padrão de caminho de URL pode conter um ou mais asteriscos (*), onde cada asterisco corresponde a uma sequência de um ou mais caracteres.

- Especifique vários caminhos de URL no padrão delimitando cada um deles com um único espaço.

    Por exemplo: */sales/ */marketing/

- Uma especificação de caminho de URL pode aproveitar os [valores curinga](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Use a opção **Ignorar maiúsculas** para controlar se uma comparação que diferencia maiúsculas de minúsculas será feita.

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-extension"></a>Extensão do Caminho da URL

Identifica solicitações pela extensão de arquivo do ativo solicitado.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Extensão de Caminho de URL são atendidas.

- **Corresponde**: requer que a URL de solicitação contenha uma extensão de arquivo que corresponda exatamente ao padrão especificado.

   Por exemplo, se você especificar "htm", os ativos "htm" serão correspondidos, mas não os ativos "html".  

- **Não corresponde**: requer que a URL de solicitação contenha uma extensão de arquivo que não corresponda ao padrão especificado.

Informações de chave:

- Especifique as extensões de arquivo para que correspondam à caixa **Valor**. Não inclua um ponto à esquerda; Por exemplo, use htm em vez de .htm.

- Use a opção **Ignorar maiúsculas** para controlar se uma comparação que diferencia maiúsculas de minúsculas será feita.

- Especifica várias extensões de arquivo delimitando cada extensão com um único espaço. 

    Por exemplo: htm html

- Por exemplo, especificando que "htm" corresponde aos ativos "htm", mas não aos ativos "html".

#### <a name="sample-scenario"></a>Cenário de exemplo

A configuração de exemplo a seguir supõe que essa condição de correspondência é atendida quando uma solicitação corresponde a uma das extensões especificadas.

Especificação de valor: asp aspx php html

Esta condição de correspondência é atendida quando encontra URLs que terminam com as seguintes extensões:

- .asp
- .aspx
- .php
- .html

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-filename"></a>Nome do Arquivo do Caminho da URL

Identifica solicitações pelo nome de arquivo do ativo solicitado. Para a finalidade dessa condição de correspondência, um nome de arquivo consiste no nome do ativo solicitado, um ponto e a extensão do arquivo (por exemplo, index.html).

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Nome de Arquivo de Caminho de URL são atendidas.

- **Corresponde**: requer que a solicitação contenha um nome de arquivo em seu caminho de URL que corresponda ao padrão especificado.
- **Não corresponde**: requer que a solicitação contenha um nome de arquivo em seu caminho de URL que não corresponda ao padrão especificado.

Informações de chave:

- Use a opção **Ignorar maiúsculas** para controlar se uma comparação que diferencia maiúsculas de minúsculas será feita.

- Para especificar várias extensões de arquivo, separe cada extensão com um único espaço.

    Por exemplo: index.htm index.html

- Substitua os espaços em um valor de nome de arquivo por "%20".

- Um valor de nome de arquivo pode aproveitar os [valores curinga](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Por exemplo, cada padrão de nome de arquivo pode consistir em um ou mais asteriscos (*), onde cada asterisco corresponde a uma sequência de um ou mais caracteres.

- Se um caractere curinga não tiver sido especificado, apenas uma correspondência exata vai satisfazer essa condição de correspondência.

    Por exemplo, especificar "presentation.ppt" corresponde a um ativo chamado "presentation.ppt", mas não a um chamado "presentation.pptx."

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-literal"></a>Literal do Caminho da URL

Compara o caminho da URL da solicitação, incluindo o nome de arquivo, para o valor especificado.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Literal de Caminho de URL são atendidas.

- **Corresponde**: requer que a solicitação contenha um caminho de URL que corresponda ao padrão especificado.
- **Não corresponde**: requer que a solicitação contenha um caminho de URL que não corresponda ao padrão especificado.

Informações de chave:

- Use a opção **Relativo a** para especificar se o ponto de comparação de URL começa antes ou após o ponto de acesso do conteúdo. 

    Os valores a seguir estão disponíveis para a opção **Relativo a**:
  - **Raiz**: indica que o ponto de comparação de URL começa logo após o nome de host da CDN.

    Por exemplo: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origem**: indica que o ponto de comparação de URL começa após o ponto de acesso do conteúdo (por exemplo, /000001 ou /800001/myorigin). Como o \*.azureedge.net CNAME é criado em relação ao diretório de origem no nome do host Verizon CDN por padrão, os usuários do Azure CDN devem usar o valor de **Origem**. 

    Por exemplo: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  Essa URL aponta para o seguinte nome do host da CDN da Verizon: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- Uma URL CNAME de borda é reescrita para uma URL CDN antes de uma comparação de URL.

Por exemplo, ambas as URLs a seguir apontam para o mesmo ativo e, portanto, têm o mesmo caminho de URL:

- URL da CDN: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- URL de CNAME de borda: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Informações adicionais:
    
    - Caminho de URL path (relativo à raiz): /800001/CustomerOrigin/path/asset.htm
   
    - Caminho de URL (relativo à origem): /path/asset.htm

- Cadeias de caracteres de consulta na URL são ignoradas.
- Use a opção **Ignorar maiúsculas** para controlar se uma comparação que diferencia maiúsculas de minúsculas será feita.
- O valor especificado para essa condição de correspondência é comparado ao caminho relativo da solicitação exata feita pelo cliente.

- Para combinar todas as solicitações feitas a um diretório específico, use a condição de correspondência do [Diretório de caminho de URL](#url-path-directory) ou do [Curinga de caminho de URL](#url-path-wildcard).

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-regex"></a>Regex do Caminho da URL

Compara o caminho de URL da solicitação com a [expressão regular](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) especificada.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Regex de Caminho de URL são atendidas.

- **Corresponde**: requer que a solicitação contenha um caminho de URL que corresponda à expressão regular especificada.
- **Não corresponde**: requer que a solicitação contenha um caminho de URL que não corresponda à expressão regular especificada.

Informações de chave:

- Uma URL CNAME de borda é reescrita para uma URL CDN antes da comparação de URL.

    Por exemplo, ambas as URLs apontam para o mesmo ativo e, portanto, têm o mesmo caminho de URL.

     - URL da CDN: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - URL de CNAME de borda: http:\//my.domain.com/path/asset.htm

    Informações adicionais:
    
     - Caminho de URL: /800001/CustomerOrigin/path/asset.htm

- Cadeias de caracteres de consulta na URL são ignoradas.
    
- Use a opção **Ignorar maiúsculas** para controlar se uma comparação que diferencia maiúsculas de minúsculas será feita.
    
- Espaços no caminho de URL devem ser substituídos por "%20".

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-wildcard"></a>Curinga do Caminho da URL

Compara o caminho de URL relativo da solicitação com o padrão de curinga especificado.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Curinga de Caminho de URL são atendidas.

- **Corresponde**: requer que a solicitação contenha um caminho de URL que corresponda ao padrão de curinga especificado.
- **Não corresponde**: requer que a solicitação contenha um caminho de URL que não corresponda ao padrão de curinga especificado.

Informações de chave:

- Opção **Relativo a**: esta opção determina se o ponto de comparação de URL começa antes ou após o ponto de acesso do conteúdo.

   Essa opção pode ter os seguintes valores:
     - **Raiz**: indica que o ponto de comparação de URL começa logo após o nome de host da CDN.

       Por exemplo: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origem**: indica que o ponto de comparação de URL começa após o ponto de acesso do conteúdo (por exemplo, /000001 ou /800001/myorigin). Como o \*.azureedge.net CNAME é criado em relação ao diretório de origem no nome do host Verizon CDN por padrão, os usuários do Azure CDN devem usar o valor de **Origem**. 

       Por exemplo: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     Essa URL aponta para o seguinte nome do host da CDN da Verizon: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- Uma URL CNAME de borda é reescrita para uma URL CDN antes da comparação de URL.

    Por exemplo, ambas as URLs a seguir apontam para o mesmo ativo e, portanto, têm o mesmo caminho de URL:
     - URL da CDN: http://wpc.0001.&lt ;domain&gt; /800001/CustomerOrigin/path/asset.htm
     - URL de CNAME de borda: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Informações adicionais:
    
     - Caminho de URL path (relativo à raiz): /800001/CustomerOrigin/path/asset.htm
    
     - Caminho de URL (relativo à origem): /path/asset.htm
    
- Especifique vários caminhos de URL delimitando cada um deles com um único espaço.

   Por exemplo: /marketing/asset.* /sales/\*.htm

- Cadeias de caracteres de consulta na URL são ignoradas.
    
- Use a opção **Ignorar maiúsculas** para controlar se uma comparação que diferencia maiúsculas de minúsculas será feita.
    
- Substitua os espaços no caminho da URL por "%20".
    
- O valor especificado para um caminho de URL pode aproveitar os [valores curinga](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Cada padrão de caminho de URL pode conter um ou mais asteriscos (*), onde cada asterisco pode corresponder a uma sequência de um ou mais caracteres.

#### <a name="sample-scenarios"></a>Cenários de Exemplo

A configuração de exemplo na tabela a seguir supõe que essa condição de correspondência é atendida quando uma solicitação corresponde ao padrão de URL especificado:

Valor                   | Relativo a    | Resultado 
------------------------|----------------|-------
\*/test.html \*/test.php  | Raiz ou origem | Esse padrão é correspondido por solicitações para ativos nomeados "test.html" ou "test.php" em qualquer pasta.
/80ABCD/origin/text/*   | Raiz           | Esse padrão é correspondido quando o ativo solicitado atende aos seguintes critérios: <br />- Ele deve estar em uma origem de cliente chamada "origem". <br />- O caminho relativo deve começar com uma pasta chamada "texto". Ou seja, o ativo solicitado pode residir na pasta "texto" ou em uma de suas subpastas recursivas.
*/css/* */js/*          | Raiz ou origem | Esse padrão é correspondido por todas as URLs CDN ou CNAME de borda que contenham uma pasta css ou js.
*.jpg *.gif *.png       | Raiz ou origem | Esse padrão é correspondido por todas as URLs CDN ou CNAME de borda terminando com .jpg, .gif, ou .png. Uma maneira alternativa de especificar esse padrão é com a [condição de correspondência de Extensão de Caminho de URL](#url-path-extension).
/images/\* /media/\*      | Origem         | Esse padrão é correspondido pelas URLs CNAME de borda ou CDN cujo caminho relativo começa com uma pasta "imagens" ou "mídia". <br />- URL da CDN: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- URL CNAME de borda de exemplo: http:\//cdn.mydomain.com/images/sales/event1.png

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-literal"></a>Literal da Consulta da URL

Compara a sequência da cadeia de caracteres de consulta da solicitação com o valor especificado.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Literal de Consulta de URL são atendidas.

- **Corresponde**: requer que a solicitação contenha uma cadeia de caracteres de consulta de URL que corresponda à cadeia de caracteres de consulta especificada.
- **Não corresponde**: requer que a solicitação contenha uma cadeia de caracteres de consulta de URL que não corresponda à cadeia de caracteres de consulta especificada.

Informações de chave:

- Somente correspondências de cadeia de caracteres de consulta exatas satisfazem essa condição de correspondência.
    
- Use a opção **Ignorar maiúsculas** para controlar a diferenciação de maiúsculas e minúsculas de comparações de cadeia de caracteres de consulta.
    
- Não inclua um ponto de interrogação (?) no texto do valor de cadeia de caracteres da consulta.
    
- Determinados caracteres exigem codificação de URL. Use o símbolo de porcentagem para a URL codificar os seguintes caracteres:

   Character | Codificação de URL
   ----------|---------
   Espaço     | %20
   &         | %25

- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
   - Concluir o Preenchimento do Cache
   - Idade Máxima Interna Padrão
   - Forçar Idade Máxima Interna
   - Ignorar Ausência de Cache de Origem
   - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-parameter"></a>Parâmetro da Consulta da URL

Identifica as solicitações que contêm o parâmetro de cadeia de caracteres de consulta especificado. Esse parâmetro é definido como um valor que corresponde a um padrão especificado. Parâmetros de cadeia de caracteres de consulta (por exemplo, parâmetro = valor) na URL de solicitação determinam se essa condição é atendida. Essa condição de correspondência identifica um parâmetro de cadeia de caracteres de consulta por seu nome e aceita um ou mais valores para o valor do parâmetro. 

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Parâmetro de Consulta de URL são atendidas.

- **Corresponde**: requer uma solicitação para conter o parâmetro especificado com um valor que corresponde a pelo menos um dos valores definidos nessa condição de correspondência.
- **Não corresponde**: requer que a solicitação atenda qualquer um dos critérios a seguir:
  - Ela não contém o parâmetro especificado.
  - Ela contém o parâmetro especificado, mas seu valor não corresponde a nenhum dos valores que são definidos nessa condição de correspondência.

Essa condição de correspondência fornece uma maneira fácil de especificar combinações de nome/valor do parâmetro. Para obter mais flexibilidade se você estiver correspondendo um parâmetro de cadeia de caracteres de consulta, considere usar a condição de correspondência [Curinga de consulta de URL](#url-query-wildcard).

Informações de chave:

- Somente um nome de parâmetro de consulta de URL único pode ser especificado por instância dessa condição de correspondência.
    
- Porque valores curinga não são suportados quando um nome de parâmetro for especificado, apenas as correspondências de nome exato de parâmetro são elegíveis para comparação.
- Os valores de parâmetros podem incluir [valores curinga](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Cada padrão de valor do parâmetro pode consistir em um ou mais asteriscos (*), onde cada asterisco pode corresponder a uma sequência de um ou mais caracteres.
   - Determinados caracteres exigem codificação de URL. Use o símbolo de porcentagem para a URL codificar os seguintes caracteres:

       Character | Codificação de URL
       ----------|---------
       Espaço     | %20
       &         | %25

- Especifique vários valores de parâmetro de cadeia de caracteres de consulta delimitando cada um deles com um único espaço. Essa condição de correspondência é atendida quando uma solicitação contém uma das combinações de nome/valor especificadas.

   - Exemplo 1:

     - Configuração:

       ValueA ValueB

     - Essa configuração corresponde aos parâmetros de cadeia de caracteres de consulta a seguir:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Exemplo 2:

     - Configuração: 

        Value%20A Value%20B

     - Essa configuração corresponde aos parâmetros de cadeia de caracteres de consulta a seguir:

       Parameter1=Value%20A

       Parameter1=Value%20B

- Essa condição de correspondência é atendida somente quando há uma correspondência exata para pelo menos uma das combinações especificadas de nome/valor de cadeia de caracteres de consulta.

   Por exemplo, se você usar a configuração no exemplo anterior, o parâmetro da combinação nome/valor "Parameter1=ValueAdd" não seria considerada uma correspondência. No entanto, se você especificar um dos valores a seguir, ele corresponderá a essa combinação de nome/valor:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Use a opção **Ignorar maiúsculas** para controlar a diferenciação de maiúsculas e minúsculas de comparações de cadeia de caracteres de consulta.
    
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
   - Concluir o Preenchimento do Cache
   - Idade Máxima Interna Padrão
   - Forçar Idade Máxima Interna
   - Ignorar Ausência de Cache de Origem
   - Máximo de Estado Obsoleto Interno

#### <a name="sample-scenarios"></a>Cenários de exemplo

O exemplo a seguir demonstra como essa opção funciona em situações específicas:

Nome  | Valor |  Resultado
------|-------|--------
User  | Joe   | Esse padrão é correspondido quando a cadeia de caracteres de consulta para uma URL solicitada é "?user=joe."
User  | *     | Esse padrão é correspondido quando a cadeia de caracteres de consulta para uma URL solicitada contém um parâmetro de Usuário.
Email | Joe\* | Esse padrão é correspondido quando a cadeia de caracteres de consulta para uma URL solicitada contém um parâmetro de E-mail que começa com “Joe”.

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-regex"></a>Regex da consulta da URL

Identifica as solicitações que contêm o parâmetro de cadeia de caracteres de consulta especificado. Esse parâmetro é definido como um valor que corresponde a uma [expressão regular](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) especificada.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Regex de Consulta de URL são atendidas.

- **Corresponde**: requer que a solicitação contenha uma cadeia de caracteres de consulta de URL que corresponda à expressão regular especificada.
- **Não corresponde**: requer que a solicitação contenha uma cadeia de caracteres de consulta de URL que não corresponda à expressão regular especificada.

Informações de chave:

- Apenas correspondências exatas para a expressão regular especificada satisfazem essa condição de correspondência.
    
- Use a opção **Ignorar maiúsculas** para controlar a diferenciação de maiúsculas e minúsculas de comparações de cadeia de caracteres de consulta.
    
- Para os fins desta opção, uma cadeia de caracteres de consulta começa com o primeiro caractere após o delimitador de ponto de interrogação (?) para a cadeia de caracteres de consulta.
    
- Determinados caracteres exigem codificação de URL. Use o símbolo de porcentagem para a URL codificar os seguintes caracteres:

   Character | Codificação de URL | Valor
   ----------|--------------|------
   Espaço     | %20          | \%20
   &         | %25          | \%25

   Note that percentage symbols must be escaped.

- Double-escape special regular expression characters (for example, \^$.+) to include a backslash in the regular expression.

   For example:

   Value | Interpreted As 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Wildcard

Compares the specified value(s) against the request's query string.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified wildcard value.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified wildcard value.

Key information:

- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
- Parameter values can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

     Character | URL Encoding
     ----------|---------
     Space     | %20
     &         | %25

- Specify multiple values by delimiting each one with a single space.

   For example: *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Only exact matches to at least one of the specified query string patterns satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

 Name                 | Description
 ---------------------|------------
user=joe              | This pattern is matched when the query string for a requested URL is "?user=joe."
\*user=\* \*optout=\* | This pattern is matched when the CDN URL query contains either the user or optout parameter.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

## Next steps

- [Azure Content Delivery Network overview](cdn-overview.md)
- [Rules engine reference](cdn-verizon-premium-rules-engine-reference.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Overriding default HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)\`20
title: Azure CDN from Verizon Premium rules engine match conditions | Microsoft Docs
description: Reference documentation for Azure Content Delivery Network from Verizon Premium rules engine match conditions.
services: cdn
author: mdgattuso

ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus

---

# Azure CDN from Verizon Premium rules engine match conditions

This article lists detailed descriptions of the available match conditions for the Azure Content Delivery Network (CDN) from Verizon Premium [rules engine](cdn-verizon-premium-rules-engine.md).

The second part of a rule is the match condition. A match condition identifies specific types of requests for which a set of features will be performed.

For example, you can use a match condition to:

- Filter requests for content at a particular location.
- Filter requests generated from a particular IP address or country/region.
- Filter requests by header information.

## Always match condition

The Always match condition applies a default set of features to all requests.

Name | Purpose
-----|--------
[Always](#always) | Applies a default set of features to all requests.

## Device match condition

The Device match condition identifies requests made from a mobile device based on its properties.  

Name | Purpose
-----|--------
[Device](#device) | Identifies requests made from a mobile device based on its properties.

## Location match conditions

The Location match conditions identify requests based on the requester's location.

Name | Purpose
-----|--------
[AS Number](#as-number) | Identifies requests that originate from a particular network.
[Country](#country) | Identifies requests that originate from the specified countries/regions.

## Origin match conditions

The Origin match conditions identify requests that point to Content Delivery Network storage or a customer origin server.

Name | Purpose
-----|--------
[CDN Origin](#cdn-origin) | Identifies requests for content stored in Content Delivery Network storage.
[Customer Origin](#customer-origin) | Identifies requests for content stored on a specific customer origin server.

## Request match conditions

The Request match conditions identify requests based on their properties.

Name | Purpose
-----|--------
[Client IP Address](#client-ip-address) | Identifies requests that originate from a particular IP address.
[Cookie Parameter](#cookie-parameter) | Checks the cookies associated with each request for the specified value.
[Cookie Parameter Regex](#cookie-parameter-regex) | Checks the cookies associated with each request for the specified regular expression.
[Edge Cname](#edge-cname) | Identifies requests that point to a specific edge CNAME.
[Referring Domain](#referring-domain) | Identifies requests that were referred from the specified host names.
[Request Header Literal](#request-header-literal) | Identifies requests that contain the specified header set to a specified value.
[Request Header Regex](#request-header-regex) | Identifies requests that contain the specified header set to a value that matches the specified regular expression.
[Request Header Wildcard](#request-header-wildcard) | Identifies requests that contain the specified header set to a value that matches the specified pattern.
[Request Method](#request-method) | Identifies requests by their HTTP method.
[Request Scheme](#request-scheme) | Identifies requests by their HTTP protocol.

## URL match conditions

The URL match conditions identify requests based on their URLs.

Name | Purpose
-----|--------
[URL Path Directory](#url-path-directory) | Identifies requests by their relative path.
[URL Path Extension](#url-path-extension) | Identifies requests by their file name extension.
[URL Path Filename](#url-path-filename) | Identifies requests by their file name.
[URL Path Literal](#url-path-literal) | Compares a request's relative path to the specified value.
[URL Path Regex](#url-path-regex) | Compares a request's relative path to the specified regular expression.
[URL Path Wildcard](#url-path-wildcard) | Compares a request's relative path to the specified pattern.
[URL Query Literal](#url-query-literal) | Compares a request's query string to the specified value.
[URL Query Parameter](#url-query-parameter) | Identifies requests that contain the specified query string parameter set to a value that matches a specified pattern.
[URL Query Regex](#url-query-regex) | Identifies requests that contain the specified query string parameter set to a value that matches a specified regular expression.
[URL Query Wildcard](#url-query-wildcard) | Compares the specified value against the request's query string.

## Reference for rules engine match conditions

---

### Always

The Always match condition applies a default set of features to all requests.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### AS Number

The AS Number network is defined by its autonomous system number (ASN). 

The **Matches**/**Does Not Match** option determines the conditions under which the AS Number match condition is met:

- **Matches**: Requires that the ASN of the client network matches one of the specified ASNs. 
- **Does Not Match**: Requires that the ASN of the client network does not match any of the specified ASNs.

Key information:

- Specify multiple ASNs by delimiting each one with a single space. For example, 64514 64515 matches requests that arrive from either 64514 or 64515.
- Certain requests might not return a valid ASN. A question mark (?) will match requests for which a valid ASN could not be determined.
- Specify the entire ASN for the desired network. Partial values will not be matched.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### CDN Origin

The CDN Origin match condition is met when both of the following conditions are met:

- Content from CDN storage was requested.
- The request URI uses the type of content access point (for example, /000001) that's defined in this match condition:
  - CDN URL: The request URI must contain the selected content access point.
  - Edge CNAME URL: The corresponding edge CNAME configuration must point to the selected content access point.
  
Key information:

- The content access point identifies the service that should serve the requested content.
- Don't use an AND IF statement to combine certain match conditions. For example, combining a CDN Origin match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two CDN Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Client IP Address

The **Matches**/**Does Not Match** option determines the conditions under which the Client IP Address match condition is met:

- **Matches**: Requires that the client's IP address matches one of the specified IP addresses. 
- **Does Not Match**: Requires that the client's IP address does not match any of the specified IP addresses. 

Key information:

- Use CIDR notation.
- Specify multiple IP addresses and/or IP address blocks by delimiting each one with a single space. For example:
  - **IPv4 example**: 1.2.3.4 10.20.30.40 matches any requests that arrive from either address 1.2.3.4 or 10.20.30.40.
  - **IPv6 example**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 matches any requests that arrive from either address 1:2:3:4:5:6:7:8 or 10:20:30:40:50:60:70:80.
- The syntax for an IP address block is the base IP address followed by a forward slash and the prefix size. For example:
  - **IPv4 example**: 5.5.5.64/26 matches any requests that arrive from addresses 5.5.5.64 through 5.5.5.127.
  - **IPv6 example**: 1:2:3:/48 matches any requests that arrive from addresses 1:2:3:0:0:0:0:0 through 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Cookie Parameter

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match any of the values that are defined in this match condition.
  
Key information:

- Cookie name:
  - Because wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - Specify multiple cookie values by delimiting each one with a single space.
  - A cookie value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - If a wildcard value has not been specified, then only an exact match will satisfy this match condition. For example, specifying "Value" will match "Value," but not "Value1" or "Value2."
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)
</br>

---

### Cookie Parameter Regex

The Cookie Parameter Regex match condition defines a cookie name and value. You can use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) to define the desired cookie value.

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter Regex match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches the specified regular expression.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match the specified regular expression.
  
Key information:

- Cookie name:
  - Because regular expressions and wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - A cookie value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Country

You can specify a country through its country code. 

The **Matches**/**Does Not Match** option determines the conditions under which the Country match condition is met:

- **Matches**: Requires the request to contain the specified country code values. 
- **Does Not Match**: Requires that the request does not contain the specified country code values.

Key information:

- Specify multiple country codes by delimiting each one with a single space.
- Wildcards are not supported when you're specifying a country code.
- The "EU" and "AP" country codes do not encompass all IP addresses in those regions.
- Certain requests might not return a valid country code. A question mark (?) will match requests for which a valid country code could not be determined.
- Country codes are case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### Implementing Country Filtering by using the rules engine

This match condition allows you to perform a multitude of customizations based on the location from which a request originated. For example, the behavior of the Country Filtering feature can be replicated through the following configuration:

- URL Path Wildcard match: Set the [URL Path Wildcard match condition](#url-path-wildcard) to the directory that will be secured. 
    Append an asterisk to the end of the relative path to ensure that access to all of its children will be restricted by this rule.

- Country match: Set the Country match condition to the desired set of countries.
  - Allow: Set the Country match condition to **Does Not Match** to allow only the specified countries access to content stored in the location defined by the URL Path Wildcard match condition.
  - Block: Set the Country match condition to **Matches** to block the specified countries from accessing content stored in the location defined by the URL Path Wildcard match condition.

- Deny Access (403) Feature: Enable the [Deny Access (403) feature](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) to replicate the allow or block portion of the Country Filtering feature.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Customer Origin

Key information:

- The Customer Origin match condition is met regardless of whether content is requested through a CDN URL or an edge CNAME URL that points to the selected customer origin.
- A customer origin configuration that's referenced by a rule cannot be deleted from the Customer Origin page. Before you attempt to delete a customer origin configuration, make sure that the following configurations do not reference it:
  - A Customer Origin match condition
  - An edge CNAME configuration
- Don't use an AND IF statement to combine certain match conditions. For example, combining a Customer Origin match condition with a CDN Origin match condition would create a match pattern that could never be matched. For this reason, two Customer Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Device

The Device match condition identifies requests made from a mobile device based on its properties. Mobile device detection is achieved through [WURFL](http://wurfl.sourceforge.net/). 

The **Matches**/**Does Not Match** option determines the conditions under which the Device match condition is met:

- **Matches**: Requires the requester's device to match the specified value. 
- **Does Not Match**: Requires that the requester's device does not match the specified value.

Key information:

- Use the **Ignore Case** option to specify whether the specified value is case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### String Type

A WURFL capability typically accepts any combination of numbers, letters, and symbols. Due to the flexible nature of this capability, you must choose how the value associated with this match condition is interpreted. The following table describes the available set of options:

Type     | Description
---------|------------
Literal  | Select this option to prevent most characters from taking on special meaning by using their [literal value](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Wildcard | Select this option to take advantage of all [wildcard characters]([wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Select this option to use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Regular expressions are useful for defining a pattern of characters.

#### WURFL capabilities

A WURFL capability refers to a category that describes mobile devices. The selected capability determines the type of mobile device description that is used to identify requests.

The following table lists WURFL capabilities and their variables for the rules engine.

> [!NOTE]
> The following variables are supported in the **Modify Client Request Header** and **Modify Client Response Header** features.

Capability | Variable | Description | Sample values
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | A string that indicates the brand name of the device. | Samsung
Device OS | %{wurfl_cap_device_os} | A string that indicates the operating system installed on the device. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | A string that indicates the version number of the operating system installed on the device. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | A Boolean that indicates whether the device supports dual orientation. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | A string that indicates the mobile device's preferred document type definition (DTD) for HTML content. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | A Boolean that indicates whether the device supports Base64 encoded images. | false
Is Android | %{wurfl_vcap_is_android} | A Boolean that indicates whether the device uses the Android OS. | true
Is IOS | %{wurfl_vcap_is_ios} | A Boolean that indicates whether the device uses iOS. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | A Boolean that indicates whether the device is a smart TV. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | A Boolean that indicates whether the device is a smartphone. | true
Is Tablet | %{wurfl_cap_is_tablet} | A Boolean that indicates whether the device is a tablet. This description is  OS-independent. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | A Boolean that indicates whether the device is considered a wireless device. | true
Marketing Name | %{wurfl_cap_marketing_name} | A string that indicates the device's marketing name. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | A string that indicates the browser that's used to request content from the device. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | A string that indicates the version of the browser that's used to request content from the device. | 31
Model Name | %{wurfl_cap_model_name} | A string that indicates the device's model name. | s3
Progressive Download | %{wurfl_cap_progressive_download} | A Boolean that indicates whether the device supports the playback of audio and video while it is still being downloaded. | true
Release Date | %{wurfl_cap_release_date} | A string that indicates the year and month on which the device was added to the WURFL database.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | An integer that indicates the device's height in pixels. | 768
Resolution Width | %{wurfl_cap_resolution_width} | An integer that indicates the device's width in pixels. | 1024

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Edge Cname

Key information:

- The list of available edge CNAMEs is limited to those edge CNAMEs that have been configured on the Edge CNAMEs page for the platform on which the rules engine is being configured.
- Before you attempt to delete an edge CNAME configuration, make sure that an Edge Cname match condition does not reference it. Edge CNAME configurations that have been defined in a rule cannot be deleted from the Edge CNAMEs page.
- Don't use an AND IF statement to combine certain match conditions. For example, combining an Edge Cname match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two Edge Cname match conditions cannot be combined through an AND IF statement.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Referring Domain

The host name associated with the referrer through which content was requested determines whether the Referring Domain condition is met.

The **Matches**/**Does Not Match** option determines the conditions under which the Referring Domain match condition is met:

- **Matches**: Requires the referring host name to match the specified values. 
- **Does Not Match**: Requires that the referring host name does not match the specified value.

Key information:

- Specify multiple host names by delimiting each one with a single space.
- This match condition supports [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- If the specified value does not contain an asterisk, it must be an exact match for the referrer's host name. For example, specifying "mydomain.com" would not match "www.mydomain.com."
- Use the **Ignore Case** option to control whether a case-sensitive comparison is made.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Literal

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Literal match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the one that's defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the one that's defined in this match condition.
  
Key information:

- Header name comparisons are always case-insensitive. Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Regex

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Regex match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the pattern that's defined in the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the specified regular expression.

Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Replace spaces in the header name with "%20."
- Header value:
  - A header value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - The match condition is met only when a header value exactly matches at least one of the specified patterns.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Header Wildcard

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Wildcard match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match any of the specified values.
  
Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Spaces in the header name should be replaced with "%20." You can also use this value to specify spaces in a header value.
- Header value:
  - A header value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - This match condition is met when a header value exactly matches to at least one of the specified patterns.
  - Specify multiple values by delimiting each one with a single space.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Method

The Request Method match condition is met only when assets are requested through the selected request method. The available request methods are:

- GET
- HEAD
- POST
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

Key information:

- By default, only the GET request method can generate cached content on the network. All other request methods are proxied through the network.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Scheme

The Request Scheme match condition is met only when assets are requested through the selected protocol. The available protocols are:

- HTTP
- HTTPS

Key information:

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Directory

Identifies a request by its relative path, which excludes the file name of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Directory match condition is met.

- **Matches**: Requires the request to contain a relative URL path, excluding the file name, that matches the specified URL pattern.
- **Does Not Match**: Requires the request to contain a relative URL path, excluding file name, that does not match the specified URL pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison starts before or after the content access point. The content access point is the portion of the path that appears between the Verizon CDN hostname and the relative path to the requested asset (for example, /800001/CustomerOrigin). It identifies a location by server type (for example, CDN or customer origin) and your customer account number.

   The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname. 

  For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

  For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- An edge CNAME URL is rewritten to a CDN URL prior to the URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path.
  - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
  - Custom domain: https:\//my.domain.com/path/asset.htm
    
    - URL path (relative to root): /800001/CustomerOrigin/path/
    
    - URL path (relative to origin): /path/

- The portion of the URL that is used for the URL comparison ends just before the file name of the requested asset. A trailing forward slash is the last character in this type of path.

- Replace any spaces in the URL path pattern with "%20."

- Each URL path pattern can contain one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- Specify multiple URL paths in the pattern by delimiting each one with a single space.

    For example: */sales/ */marketing/

- A URL path specification can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Extension

Identifies requests by the file extension of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Extension match condition is met.

- **Matches**: Requires the URL of the request to contain a file extension that exactly matches the specified pattern.

   For example, if you specify "htm", "htm" assets are matched, but not "html" assets.  

- **Does Not Match**: Requires the URL request to contain a file extension that does not match the specified pattern.

Key information:

- Specify the file extensions to match in the **Value** box. Do not include a leading period; for example, use htm instead of .htm.

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- Specify multiple file extensions by delimiting each extension with a single space. 

    For example: htm html

- For example, specifying "htm" matches "htm" assets, but not "html" assets.

#### Sample Scenario

The following sample configuration assumes that this match condition is met when a request matches one of the specified extensions.

Value specification: asp aspx php html

This match condition is met when it finds URLs that end with the following extensions:

- .asp
- .aspx
- .php
- .html

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Filename

Identifies requests by the file name of the requested asset. For the purposes of this match condition, a file name consists of the name of the requested asset, a period, and the file extension (for example, index.html).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Filename match condition is met.

- **Matches**: Requires the request to contain a file name in its URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a file name in its URL path that does not match the specified pattern.

Key information:

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- To specify multiple file extensions, separate each extension with a single space.

    For example: index.htm index.html

- Replace spaces in a file name value with "%20."

- A file name value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). For example, each file name pattern can consist of one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- If wildcard characters are not specified, then only an exact match will satisfy this match condition.

    For example, specifying "presentation.ppt" matches an asset named "presentation.ppt," but not one named "presentation.pptx."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Literal

Compares a request's URL path, including file name, to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Literal match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison point begins before or after the content access point. 

    The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

    For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

    For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to a URL comparison.

For example, both of the following URLs point to the same asset and therefore have the same URL path:

- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Additional information:
    
    - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
   
    - URL path (relative to origin): /path/asset.htm

- Query strings in the URL are ignored.
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
- The value specified for this match condition is compared against the relative path of the exact request made by the client.

- To match all requests made to a particular directory, use the [URL Path Directory](#url-path-directory) or the [URL Path Wildcard](#url-path-wildcard) match condition.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Regex

Compares a request's URL path to the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Regex match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified regular expression.

Key information:

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both URLs point to the same asset and therefore have the same URL path.

     - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME URL: http:\//my.domain.com/path/asset.htm

    Additional information:
    
     - URL path: /800001/CustomerOrigin/path/asset.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Spaces in the URL path should be replaced with "%20."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Wildcard

Compares a request's relative URL path to the specified wildcard pattern.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified wildcard pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified wildcard pattern.

Key information:

- **Relative to** option: This option determines whether the URL comparison point begins before or after the content access point.

   This option can have the following values:
     - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

       For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

       For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path:
     - CDN URL: http://wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
    
     - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
    
     - URL path (relative to origin): /path/asset.htm
    
- Specify multiple URL paths by delimiting each one with a single space.

   For example: /marketing/asset.* /sales/*.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Replace spaces in the URL path with "%20."
    
- The value specified for a URL path can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Each URL path pattern can contain one or more asterisks (*), where each asterisk can match a sequence of one or more characters.

#### Sample Scenarios

The sample configurations in the following table assume that this match condition is met when a request matches the specified URL pattern:

Value                   | Relative to    | Result 
------------------------|----------------|-------
*/test.html */test.php  | Root or Origin | This pattern is matched by requests for assets named "test.html" or "test.php" in any folder.
/80ABCD/origin/text/*   | Root           | This pattern is matched when the requested asset meets the following criteria: <br />- It must reside on a customer origin called "origin." <br />- The relative path must start with a folder called "text." That is, the requested asset can either reside in the "text" folder or one of its recursive subfolders.
*/css/* */js/*          | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs that contain a css or js folder.
*.jpg *.gif *.png       | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs ending with .jpg, .gif, or .png. An alternative way to specify this pattern is with the [URL Path Extension match condition](#url-path-extension).
/images/* /media/*      | Origin         | This pattern is matched by CDN or edge CNAME URLs whose relative path starts with an "images" or "media" folder. <br />- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- Sample edge CNAME URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Literal

Compares a request's query string to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Literal match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified query string.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified query string.

Key information:

- Only exact query string matches satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Do not include a leading question mark (?) in the query string value text.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding
   ----------|---------
   Space     | %20
   &         | %25

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Parameter

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified pattern. Query string parameters (for example, parameter=value) in the request URL determine whether this condition is met. This match condition identifies a query string parameter by its name and accepts one or more values for the parameter value. 

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Parameter match condition is met.

- **Matches**: Requires a request to contain the specified parameter with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified parameter.
  - It contains the specified parameter, but its value does not match any of the values that are defined in this match condition.

This match condition provides an easy way to specify parameter name/value combinations. For more flexibility if you are matching a query string parameter, consider using the [URL Query Wildcard](#url-query-wildcard) match condition.

Key information:

- Only a single URL query parameter name can be specified per instance of this match condition.
    
- Because wildcard values are not supported when a parameter name is specified, only exact parameter name matches are eligible for comparison.
- Parameter value(s) can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

       Character | URL Encoding
       ----------|---------
       Space     | %20
       &         | %25

- Specify multiple query string parameter values by delimiting each one with a single space. This match condition is met when a request contains one of the specified name/value combinations.

   - Example 1:

     - Configuration:

       ValueA ValueB

     - This configuration matches the following query string parameters:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Example 2:

     - Configuration: 

        Value%20A Value%20B

     - This configuration matches the following query string parameters:

       Parameter1=Value%20A

       Parameter1=Value%20B

- This match condition is met only when there is an exact match to at least one of the specified query string name/value combinations.

   For example, if you use the configuration in the previous example, the parameter name/value combination "Parameter1=ValueAdd" would not be considered a match. However, if you specify either of the following values, it will match that name/value combination:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

Name  | Value |  Result
------|-------|--------
User  | Joe   | This pattern is matched when the query string for a requested URL is "?user=joe."
User  | *     | This pattern is matched when the query string for a requested URL contains a User parameter.
Email | Joe\* | This pattern is matched when the query string for a requested URL contains an Email parameter that starts with "Joe."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Regex

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Regex match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified regular expression.

Key information:

- Only exact matches to the specified regular expression satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding | Value
   ----------|--------------|------
   Space     | %20          | \%20
   &         | %25          | \%25

   Note that percentage symbols must be escaped.

- Double-escape special regular expression characters (for example, \^$.+) to include a backslash in the regular expression.

   For example:

   Value | Interpreted As 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Wildcard

Compares the specified value(s) against the request's query string.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified wildcard value.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified wildcard value.

Key information:

- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
- Parameter values can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

     Character | URL Encoding
     ----------|---------
     Space     | %20
     &         | %25

- Specify multiple values by delimiting each one with a single space.

   For example: *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Only exact matches to at least one of the specified query string patterns satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

 Name                 | Description
 ---------------------|------------
user=joe              | This pattern is matched when the query string for a requested URL is "?user=joe."
\*user=\* \*optout=\* | This pattern is matched when the CDN URL query contains either the user or optout parameter.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

## Next steps

- [Azure Content Delivery Network overview](cdn-overview.md)
- [Rules engine reference](cdn-verizon-premium-rules-engine-reference.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Overriding default HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)\`25
title: Azure CDN from Verizon Premium rules engine match conditions | Microsoft Docs
description: Reference documentation for Azure Content Delivery Network from Verizon Premium rules engine match conditions.
services: cdn
author: mdgattuso

ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus

---

# Azure CDN from Verizon Premium rules engine match conditions

This article lists detailed descriptions of the available match conditions for the Azure Content Delivery Network (CDN) from Verizon Premium [rules engine](cdn-verizon-premium-rules-engine.md).

The second part of a rule is the match condition. A match condition identifies specific types of requests for which a set of features will be performed.

For example, you can use a match condition to:

- Filter requests for content at a particular location.
- Filter requests generated from a particular IP address or country/region.
- Filter requests by header information.

## Always match condition

The Always match condition applies a default set of features to all requests.

Name | Purpose
-----|--------
[Always](#always) | Applies a default set of features to all requests.

## Device match condition

The Device match condition identifies requests made from a mobile device based on its properties.  

Name | Purpose
-----|--------
[Device](#device) | Identifies requests made from a mobile device based on its properties.

## Location match conditions

The Location match conditions identify requests based on the requester's location.

Name | Purpose
-----|--------
[AS Number](#as-number) | Identifies requests that originate from a particular network.
[Country](#country) | Identifies requests that originate from the specified countries/regions.

## Origin match conditions

The Origin match conditions identify requests that point to Content Delivery Network storage or a customer origin server.

Name | Purpose
-----|--------
[CDN Origin](#cdn-origin) | Identifies requests for content stored in Content Delivery Network storage.
[Customer Origin](#customer-origin) | Identifies requests for content stored on a specific customer origin server.

## Request match conditions

The Request match conditions identify requests based on their properties.

Name | Purpose
-----|--------
[Client IP Address](#client-ip-address) | Identifies requests that originate from a particular IP address.
[Cookie Parameter](#cookie-parameter) | Checks the cookies associated with each request for the specified value.
[Cookie Parameter Regex](#cookie-parameter-regex) | Checks the cookies associated with each request for the specified regular expression.
[Edge Cname](#edge-cname) | Identifies requests that point to a specific edge CNAME.
[Referring Domain](#referring-domain) | Identifies requests that were referred from the specified host names.
[Request Header Literal](#request-header-literal) | Identifies requests that contain the specified header set to a specified value.
[Request Header Regex](#request-header-regex) | Identifies requests that contain the specified header set to a value that matches the specified regular expression.
[Request Header Wildcard](#request-header-wildcard) | Identifies requests that contain the specified header set to a value that matches the specified pattern.
[Request Method](#request-method) | Identifies requests by their HTTP method.
[Request Scheme](#request-scheme) | Identifies requests by their HTTP protocol.

## URL match conditions

The URL match conditions identify requests based on their URLs.

Name | Purpose
-----|--------
[URL Path Directory](#url-path-directory) | Identifies requests by their relative path.
[URL Path Extension](#url-path-extension) | Identifies requests by their file name extension.
[URL Path Filename](#url-path-filename) | Identifies requests by their file name.
[URL Path Literal](#url-path-literal) | Compares a request's relative path to the specified value.
[URL Path Regex](#url-path-regex) | Compares a request's relative path to the specified regular expression.
[URL Path Wildcard](#url-path-wildcard) | Compares a request's relative path to the specified pattern.
[URL Query Literal](#url-query-literal) | Compares a request's query string to the specified value.
[URL Query Parameter](#url-query-parameter) | Identifies requests that contain the specified query string parameter set to a value that matches a specified pattern.
[URL Query Regex](#url-query-regex) | Identifies requests that contain the specified query string parameter set to a value that matches a specified regular expression.
[URL Query Wildcard](#url-query-wildcard) | Compares the specified value against the request's query string.

## Reference for rules engine match conditions

---

### Always

The Always match condition applies a default set of features to all requests.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### AS Number

The AS Number network is defined by its autonomous system number (ASN). 

The **Matches**/**Does Not Match** option determines the conditions under which the AS Number match condition is met:

- **Matches**: Requires that the ASN of the client network matches one of the specified ASNs. 
- **Does Not Match**: Requires that the ASN of the client network does not match any of the specified ASNs.

Key information:

- Specify multiple ASNs by delimiting each one with a single space. For example, 64514 64515 matches requests that arrive from either 64514 or 64515.
- Certain requests might not return a valid ASN. A question mark (?) will match requests for which a valid ASN could not be determined.
- Specify the entire ASN for the desired network. Partial values will not be matched.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### CDN Origin

The CDN Origin match condition is met when both of the following conditions are met:

- Content from CDN storage was requested.
- The request URI uses the type of content access point (for example, /000001) that's defined in this match condition:
  - CDN URL: The request URI must contain the selected content access point.
  - Edge CNAME URL: The corresponding edge CNAME configuration must point to the selected content access point.
  
Key information:

- The content access point identifies the service that should serve the requested content.
- Don't use an AND IF statement to combine certain match conditions. For example, combining a CDN Origin match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two CDN Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Client IP Address

The **Matches**/**Does Not Match** option determines the conditions under which the Client IP Address match condition is met:

- **Matches**: Requires that the client's IP address matches one of the specified IP addresses. 
- **Does Not Match**: Requires that the client's IP address does not match any of the specified IP addresses. 

Key information:

- Use CIDR notation.
- Specify multiple IP addresses and/or IP address blocks by delimiting each one with a single space. For example:
  - **IPv4 example**: 1.2.3.4 10.20.30.40 matches any requests that arrive from either address 1.2.3.4 or 10.20.30.40.
  - **IPv6 example**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 matches any requests that arrive from either address 1:2:3:4:5:6:7:8 or 10:20:30:40:50:60:70:80.
- The syntax for an IP address block is the base IP address followed by a forward slash and the prefix size. For example:
  - **IPv4 example**: 5.5.5.64/26 matches any requests that arrive from addresses 5.5.5.64 through 5.5.5.127.
  - **IPv6 example**: 1:2:3:/48 matches any requests that arrive from addresses 1:2:3:0:0:0:0:0 through 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Cookie Parameter

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match any of the values that are defined in this match condition.
  
Key information:

- Cookie name:
  - Because wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - Specify multiple cookie values by delimiting each one with a single space.
  - A cookie value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - If a wildcard value has not been specified, then only an exact match will satisfy this match condition. For example, specifying "Value" will match "Value," but not "Value1" or "Value2."
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)
</br>

---

### Cookie Parameter Regex

The Cookie Parameter Regex match condition defines a cookie name and value. You can use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) to define the desired cookie value.

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter Regex match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches the specified regular expression.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match the specified regular expression.
  
Key information:

- Cookie name:
  - Because regular expressions and wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - A cookie value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Country

You can specify a country through its country code. 

The **Matches**/**Does Not Match** option determines the conditions under which the Country match condition is met:

- **Matches**: Requires the request to contain the specified country code values. 
- **Does Not Match**: Requires that the request does not contain the specified country code values.

Key information:

- Specify multiple country codes by delimiting each one with a single space.
- Wildcards are not supported when you're specifying a country code.
- The "EU" and "AP" country codes do not encompass all IP addresses in those regions.
- Certain requests might not return a valid country code. A question mark (?) will match requests for which a valid country code could not be determined.
- Country codes are case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### Implementing Country Filtering by using the rules engine

This match condition allows you to perform a multitude of customizations based on the location from which a request originated. For example, the behavior of the Country Filtering feature can be replicated through the following configuration:

- URL Path Wildcard match: Set the [URL Path Wildcard match condition](#url-path-wildcard) to the directory that will be secured. 
    Append an asterisk to the end of the relative path to ensure that access to all of its children will be restricted by this rule.

- Country match: Set the Country match condition to the desired set of countries.
  - Allow: Set the Country match condition to **Does Not Match** to allow only the specified countries access to content stored in the location defined by the URL Path Wildcard match condition.
  - Block: Set the Country match condition to **Matches** to block the specified countries from accessing content stored in the location defined by the URL Path Wildcard match condition.

- Deny Access (403) Feature: Enable the [Deny Access (403) feature](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) to replicate the allow or block portion of the Country Filtering feature.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Customer Origin

Key information:

- The Customer Origin match condition is met regardless of whether content is requested through a CDN URL or an edge CNAME URL that points to the selected customer origin.
- A customer origin configuration that's referenced by a rule cannot be deleted from the Customer Origin page. Before you attempt to delete a customer origin configuration, make sure that the following configurations do not reference it:
  - A Customer Origin match condition
  - An edge CNAME configuration
- Don't use an AND IF statement to combine certain match conditions. For example, combining a Customer Origin match condition with a CDN Origin match condition would create a match pattern that could never be matched. For this reason, two Customer Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Device

The Device match condition identifies requests made from a mobile device based on its properties. Mobile device detection is achieved through [WURFL](http://wurfl.sourceforge.net/). 

The **Matches**/**Does Not Match** option determines the conditions under which the Device match condition is met:

- **Matches**: Requires the requester's device to match the specified value. 
- **Does Not Match**: Requires that the requester's device does not match the specified value.

Key information:

- Use the **Ignore Case** option to specify whether the specified value is case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### String Type

A WURFL capability typically accepts any combination of numbers, letters, and symbols. Due to the flexible nature of this capability, you must choose how the value associated with this match condition is interpreted. The following table describes the available set of options:

Type     | Description
---------|------------
Literal  | Select this option to prevent most characters from taking on special meaning by using their [literal value](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Wildcard | Select this option to take advantage of all [wildcard characters]([wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Select this option to use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Regular expressions are useful for defining a pattern of characters.

#### WURFL capabilities

A WURFL capability refers to a category that describes mobile devices. The selected capability determines the type of mobile device description that is used to identify requests.

The following table lists WURFL capabilities and their variables for the rules engine.

> [!NOTE]
> The following variables are supported in the **Modify Client Request Header** and **Modify Client Response Header** features.

Capability | Variable | Description | Sample values
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | A string that indicates the brand name of the device. | Samsung
Device OS | %{wurfl_cap_device_os} | A string that indicates the operating system installed on the device. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | A string that indicates the version number of the operating system installed on the device. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | A Boolean that indicates whether the device supports dual orientation. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | A string that indicates the mobile device's preferred document type definition (DTD) for HTML content. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | A Boolean that indicates whether the device supports Base64 encoded images. | false
Is Android | %{wurfl_vcap_is_android} | A Boolean that indicates whether the device uses the Android OS. | true
Is IOS | %{wurfl_vcap_is_ios} | A Boolean that indicates whether the device uses iOS. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | A Boolean that indicates whether the device is a smart TV. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | A Boolean that indicates whether the device is a smartphone. | true
Is Tablet | %{wurfl_cap_is_tablet} | A Boolean that indicates whether the device is a tablet. This description is  OS-independent. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | A Boolean that indicates whether the device is considered a wireless device. | true
Marketing Name | %{wurfl_cap_marketing_name} | A string that indicates the device's marketing name. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | A string that indicates the browser that's used to request content from the device. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | A string that indicates the version of the browser that's used to request content from the device. | 31
Model Name | %{wurfl_cap_model_name} | A string that indicates the device's model name. | s3
Progressive Download | %{wurfl_cap_progressive_download} | A Boolean that indicates whether the device supports the playback of audio and video while it is still being downloaded. | true
Release Date | %{wurfl_cap_release_date} | A string that indicates the year and month on which the device was added to the WURFL database.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | An integer that indicates the device's height in pixels. | 768
Resolution Width | %{wurfl_cap_resolution_width} | An integer that indicates the device's width in pixels. | 1024

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Edge Cname

Key information:

- The list of available edge CNAMEs is limited to those edge CNAMEs that have been configured on the Edge CNAMEs page for the platform on which the rules engine is being configured.
- Before you attempt to delete an edge CNAME configuration, make sure that an Edge Cname match condition does not reference it. Edge CNAME configurations that have been defined in a rule cannot be deleted from the Edge CNAMEs page.
- Don't use an AND IF statement to combine certain match conditions. For example, combining an Edge Cname match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two Edge Cname match conditions cannot be combined through an AND IF statement.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Referring Domain

The host name associated with the referrer through which content was requested determines whether the Referring Domain condition is met.

The **Matches**/**Does Not Match** option determines the conditions under which the Referring Domain match condition is met:

- **Matches**: Requires the referring host name to match the specified values. 
- **Does Not Match**: Requires that the referring host name does not match the specified value.

Key information:

- Specify multiple host names by delimiting each one with a single space.
- This match condition supports [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- If the specified value does not contain an asterisk, it must be an exact match for the referrer's host name. For example, specifying "mydomain.com" would not match "www.mydomain.com."
- Use the **Ignore Case** option to control whether a case-sensitive comparison is made.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Literal

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Literal match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the one that's defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the one that's defined in this match condition.
  
Key information:

- Header name comparisons are always case-insensitive. Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Regex

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Regex match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the pattern that's defined in the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the specified regular expression.

Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Replace spaces in the header name with "%20."
- Header value:
  - A header value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - The match condition is met only when a header value exactly matches at least one of the specified patterns.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Header Wildcard

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Wildcard match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match any of the specified values.
  
Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Spaces in the header name should be replaced with "%20." You can also use this value to specify spaces in a header value.
- Header value:
  - A header value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - This match condition is met when a header value exactly matches to at least one of the specified patterns.
  - Specify multiple values by delimiting each one with a single space.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Method

The Request Method match condition is met only when assets are requested through the selected request method. The available request methods are:

- GET
- HEAD
- POST
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

Key information:

- By default, only the GET request method can generate cached content on the network. All other request methods are proxied through the network.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Scheme

The Request Scheme match condition is met only when assets are requested through the selected protocol. The available protocols are:

- HTTP
- HTTPS

Key information:

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Directory

Identifies a request by its relative path, which excludes the file name of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Directory match condition is met.

- **Matches**: Requires the request to contain a relative URL path, excluding the file name, that matches the specified URL pattern.
- **Does Not Match**: Requires the request to contain a relative URL path, excluding file name, that does not match the specified URL pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison starts before or after the content access point. The content access point is the portion of the path that appears between the Verizon CDN hostname and the relative path to the requested asset (for example, /800001/CustomerOrigin). It identifies a location by server type (for example, CDN or customer origin) and your customer account number.

   The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname. 

  For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

  For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- An edge CNAME URL is rewritten to a CDN URL prior to the URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path.
  - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
  - Custom domain: https:\//my.domain.com/path/asset.htm
    
    - URL path (relative to root): /800001/CustomerOrigin/path/
    
    - URL path (relative to origin): /path/

- The portion of the URL that is used for the URL comparison ends just before the file name of the requested asset. A trailing forward slash is the last character in this type of path.

- Replace any spaces in the URL path pattern with "%20."

- Each URL path pattern can contain one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- Specify multiple URL paths in the pattern by delimiting each one with a single space.

    For example: */sales/ */marketing/

- A URL path specification can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Extension

Identifies requests by the file extension of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Extension match condition is met.

- **Matches**: Requires the URL of the request to contain a file extension that exactly matches the specified pattern.

   For example, if you specify "htm", "htm" assets are matched, but not "html" assets.  

- **Does Not Match**: Requires the URL request to contain a file extension that does not match the specified pattern.

Key information:

- Specify the file extensions to match in the **Value** box. Do not include a leading period; for example, use htm instead of .htm.

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- Specify multiple file extensions by delimiting each extension with a single space. 

    For example: htm html

- For example, specifying "htm" matches "htm" assets, but not "html" assets.

#### Sample Scenario

The following sample configuration assumes that this match condition is met when a request matches one of the specified extensions.

Value specification: asp aspx php html

This match condition is met when it finds URLs that end with the following extensions:

- .asp
- .aspx
- .php
- .html

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Filename

Identifies requests by the file name of the requested asset. For the purposes of this match condition, a file name consists of the name of the requested asset, a period, and the file extension (for example, index.html).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Filename match condition is met.

- **Matches**: Requires the request to contain a file name in its URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a file name in its URL path that does not match the specified pattern.

Key information:

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- To specify multiple file extensions, separate each extension with a single space.

    For example: index.htm index.html

- Replace spaces in a file name value with "%20."

- A file name value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). For example, each file name pattern can consist of one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- If wildcard characters are not specified, then only an exact match will satisfy this match condition.

    For example, specifying "presentation.ppt" matches an asset named "presentation.ppt," but not one named "presentation.pptx."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Literal

Compares a request's URL path, including file name, to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Literal match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison point begins before or after the content access point. 

    The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

    For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

    For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to a URL comparison.

For example, both of the following URLs point to the same asset and therefore have the same URL path:

- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Additional information:
    
    - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
   
    - URL path (relative to origin): /path/asset.htm

- Query strings in the URL are ignored.
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
- The value specified for this match condition is compared against the relative path of the exact request made by the client.

- To match all requests made to a particular directory, use the [URL Path Directory](#url-path-directory) or the [URL Path Wildcard](#url-path-wildcard) match condition.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Regex

Compares a request's URL path to the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Regex match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified regular expression.

Key information:

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both URLs point to the same asset and therefore have the same URL path.

     - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME URL: http:\//my.domain.com/path/asset.htm

    Additional information:
    
     - URL path: /800001/CustomerOrigin/path/asset.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Spaces in the URL path should be replaced with "%20."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Wildcard

Compares a request's relative URL path to the specified wildcard pattern.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified wildcard pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified wildcard pattern.

Key information:

- **Relative to** option: This option determines whether the URL comparison point begins before or after the content access point.

   This option can have the following values:
     - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

       For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

       For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path:
     - CDN URL: http://wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
    
     - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
    
     - URL path (relative to origin): /path/asset.htm
    
- Specify multiple URL paths by delimiting each one with a single space.

   For example: /marketing/asset.* /sales/*.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Replace spaces in the URL path with "%20."
    
- The value specified for a URL path can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Each URL path pattern can contain one or more asterisks (*), where each asterisk can match a sequence of one or more characters.

#### Sample Scenarios

The sample configurations in the following table assume that this match condition is met when a request matches the specified URL pattern:

Value                   | Relative to    | Result 
------------------------|----------------|-------
*/test.html */test.php  | Root or Origin | This pattern is matched by requests for assets named "test.html" or "test.php" in any folder.
/80ABCD/origin/text/*   | Root           | This pattern is matched when the requested asset meets the following criteria: <br />- It must reside on a customer origin called "origin." <br />- The relative path must start with a folder called "text." That is, the requested asset can either reside in the "text" folder or one of its recursive subfolders.
*/css/* */js/*          | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs that contain a css or js folder.
*.jpg *.gif *.png       | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs ending with .jpg, .gif, or .png. An alternative way to specify this pattern is with the [URL Path Extension match condition](#url-path-extension).
/images/* /media/*      | Origin         | This pattern is matched by CDN or edge CNAME URLs whose relative path starts with an "images" or "media" folder. <br />- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- Sample edge CNAME URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Literal

Compares a request's query string to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Literal match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified query string.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified query string.

Key information:

- Only exact query string matches satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Do not include a leading question mark (?) in the query string value text.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding
   ----------|---------
   Space     | %20
   &         | %25

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Parameter

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified pattern. Query string parameters (for example, parameter=value) in the request URL determine whether this condition is met. This match condition identifies a query string parameter by its name and accepts one or more values for the parameter value. 

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Parameter match condition is met.

- **Matches**: Requires a request to contain the specified parameter with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified parameter.
  - It contains the specified parameter, but its value does not match any of the values that are defined in this match condition.

This match condition provides an easy way to specify parameter name/value combinations. For more flexibility if you are matching a query string parameter, consider using the [URL Query Wildcard](#url-query-wildcard) match condition.

Key information:

- Only a single URL query parameter name can be specified per instance of this match condition.
    
- Because wildcard values are not supported when a parameter name is specified, only exact parameter name matches are eligible for comparison.
- Parameter value(s) can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

       Character | URL Encoding
       ----------|---------
       Space     | %20
       &         | %25

- Specify multiple query string parameter values by delimiting each one with a single space. This match condition is met when a request contains one of the specified name/value combinations.

   - Example 1:

     - Configuration:

       ValueA ValueB

     - This configuration matches the following query string parameters:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Example 2:

     - Configuration: 

        Value%20A Value%20B

     - This configuration matches the following query string parameters:

       Parameter1=Value%20A

       Parameter1=Value%20B

- This match condition is met only when there is an exact match to at least one of the specified query string name/value combinations.

   For example, if you use the configuration in the previous example, the parameter name/value combination "Parameter1=ValueAdd" would not be considered a match. However, if you specify either of the following values, it will match that name/value combination:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

Name  | Value |  Result
------|-------|--------
User  | Joe   | This pattern is matched when the query string for a requested URL is "?user=joe."
User  | *     | This pattern is matched when the query string for a requested URL contains a User parameter.
Email | Joe\* | This pattern is matched when the query string for a requested URL contains an Email parameter that starts with "Joe."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Regex

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Regex match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified regular expression.

Key information:

- Only exact matches to the specified regular expression satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding | Value
   ----------|--------------|------
   Space     | %20          | \%20
   &         | %25          | \%25

   Observe que os símbolos de porcentagem devem ser substituídos.

- Caracteres de expressão regular especiais de escape duplo (por exemplo, \^$.+) para incluir uma barra invertida na expressão regular.

   Por exemplo:

   Valor | Interpretado como 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
   - Concluir o Preenchimento do Cache
   - Idade Máxima Interna Padrão
   - Forçar Idade Máxima Interna
   - Ignorar Ausência de Cache de Origem
   - Máximo de Estado Obsoleto Interno

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-wildcard"></a>Curinga da consulta da URL

Compara os valores especificados com a cadeia de caracteres de consulta da solicitação.

A opção **Corresponde**/**Não corresponde** determina as condições sob as quais a condição de correspondência Curinga de Consulta de URL são atendidas.

- **Corresponde**: requer que a solicitação contenha uma cadeia de caracteres de consulta de URL que corresponda ao valor do curinga especificado.
- **Não corresponde**: requer que a solicitação contenha uma cadeia de caracteres de consulta de URL que não corresponda ao valor do curinga especificado.

Informações de chave:

- Para os fins desta opção, uma cadeia de caracteres de consulta começa com o primeiro caractere após o delimitador de ponto de interrogação (?) para a cadeia de caracteres de consulta.
- Os valores de parâmetros podem incluir [valores curinga](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Cada padrão de valor do parâmetro pode consistir em um ou mais asteriscos (*), onde cada asterisco pode corresponder a uma sequência de um ou mais caracteres.
   - Determinados caracteres exigem codificação de URL. Use o símbolo de porcentagem para a URL codificar os seguintes caracteres:

     Character | Codificação de URL
     ----------|---------
     Espaço     | %20
     &         | %25

- Especifique vários valores delimitando cada um deles com um único espaço.

   Por exemplo:  *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Apenas as correspondências exatas a pelo menos um dos padrões de cadeia de caracteres de consulta especificados satisfazem essa condição de correspondência.
    
- Use a opção **Ignorar maiúsculas** para controlar a diferenciação de maiúsculas e minúsculas de comparações de cadeia de caracteres de consulta.
    
- Devido à maneira como as configurações de cache são acompanhadas, essa condição de correspondência é incompatível com os recursos a seguir:
   - Concluir o Preenchimento do Cache
   - Idade Máxima Interna Padrão
   - Forçar Idade Máxima Interna
   - Ignorar Ausência de Cache de Origem
   - Máximo de Estado Obsoleto Interno

#### <a name="sample-scenarios"></a>Cenários de exemplo

O exemplo a seguir demonstra como essa opção funciona em situações específicas:

 Nome                 | DESCRIÇÃO
 ---------------------|------------
user=joe              | Esse padrão é correspondido quando a cadeia de caracteres de consulta para uma URL solicitada é "?user=joe."
\*user=\* \*optout=\* | Esse padrão é correspondido quando a consulta de URL CDN contém o parâmetro opcional ou de usuário.

[Voltar ao início](#reference-for-rules-engine-match-conditions)

</br>

## <a name="next-steps"></a>Próximas etapas

- [Visão geral da Rede de Distribuição de Conteúdo do Azure](cdn-overview.md)
- [Referência do mecanismo de regras](cdn-verizon-premium-rules-engine-reference.md)
- [Expressões condicionais do mecanismo de regras](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Recursos do mecanismo de regras](cdn-verizon-premium-rules-engine-reference-features.md)
- [Substituindo o comportamento HTTP padrão usando o mecanismo de regras](cdn-verizon-premium-rules-engine.md)

---
title: Configurar aplicativos no portal – serviço de aplicativo do Azure
description: Saiba como configurar as configurações comuns para um aplicativo de serviço de aplicativo no portal do Azure.
keywords: o serviço de aplicativo do Azure, aplicativo web, configurações do aplicativo, variáveis de ambiente
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: bcc970375120f76e4ec8a90f487d251296f92dba
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957904"
---
# <a name="configure-an-app-service-app-in-the-azure-portal"></a>Configurar um aplicativo de serviço de aplicativo no portal do Azure

Este tópico explica como definir as configurações comuns para aplicativos web, back-end móvel ou aplicativo de API usando o [portal do Azure].

## <a name="configure-app-settings"></a>Definir configurações de aplicativo

Serviço de aplicativo, você usará as configurações de aplicativo como variáveis de ambiente. No [portal do Azure], navegue até a página de gerenciamento do seu aplicativo. No menu da esquerda do aplicativo, clique em **Configuration** > **configurações do aplicativo**.

![Configurações do Aplicativo](./media/configure-common/open-ui.png)

Para os desenvolvedores ASP.NET e ASP.NET Core, as configurações de aplicativo de configuração no serviço de aplicativo são como defini-las na `<appSettings>` na *Web. config*, mas os valores no serviço de aplicativo substituem aquelas na *Web. config*. Você pode manter as configurações de desenvolvimento (por exemplo, senha local do MySQL) no *Web. config*, mas os segredos de produção (por exemplo, senha de banco de dados MySQL do Azure) seguros no serviço de aplicativo. O mesmo código usa as configurações de desenvolvimento quando você depura localmente e, em seguida, usa seus segredos de produção quando implantado no Azure.

As pilhas de outros idiomas, da mesma forma, obtém as configurações de aplicativo como variáveis de ambiente em tempo de execução. Para etapas específicas da pilha de idiomas, consulte:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Contêineres personalizados](containers/configure-custom-container.md#configure-environment-variables)

As configurações do aplicativo são sempre criptografadas quando armazenadas (criptografadas em repouso).

> [!NOTE]
> Configurações do aplicativo também podem ser resolvidas a partir [Key Vault](/azure/key-vault/) usando [faz referência a Key Vault](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Mostrar valores ocultos

Por padrão, os valores para as configurações de aplicativo ficam ocultos no portal para a segurança. Para ver um valor oculto de uma configuração de aplicativo, clique o **valor** campo da configuração. Para ver os valores de todas as configurações de aplicativo, clique o **Mostrar valor** botão.

### <a name="add-or-edit"></a>Adicionar ou editar

Para adicionar uma nova configuração de aplicativo, clique em **nova configuração de aplicativo**. Na caixa de diálogo, você pode [continuar a configuração ao slot atual](deploy-staging-slots.md#which-settings-are-swapped).

Para editar uma configuração, clique o **editar** botão no lado direito.

Quando terminar, clique em **atualização**. Não se esqueça de clicar **salvar** volta a **configuração** página.

> [!NOTE]
> Em um contêiner do Linux padrão ou um contêiner do Linux personalizado, qualquer estrutura de chave JSON aninhada no nome da configuração de aplicativo como `ApplicationInsights:InstrumentationKey` precisa ser configurado no serviço de aplicativo como `ApplicationInsights__InstrumentationKey` para o nome da chave. Em outras palavras, qualquer `:` devem ser substituídos por `__` (sublinhado duplo).
>

### <a name="edit-in-bulk"></a>Editar em massa

Para adicionar ou editar as configurações de aplicativo em massa, clique no **avançadas de edição** botão. Quando terminar, clique em **atualização**. Não se esqueça de clicar **salvar** volta a **configuração** página.

Configurações do aplicativo tem a formatação do seguinte JSON:

```json
[
  {
    "name": "<key-1>",
    "value": "<value-1>",
    "slotSetting": false
  },
  {
    "name": "<key-2>",
    "value": "<value-2>",
    "slotSetting": false
  },
  ...
]
```

## <a name="configure-connection-strings"></a>Configurar cadeias de conexão

No [portal do Azure], navegue até a página de gerenciamento do aplicativo. No menu da esquerda do aplicativo, clique em **Configuration** > **configurações do aplicativo**.

![Configurações do Aplicativo](./media/configure-common/open-ui.png)

Para os desenvolvedores ASP.NET e ASP.NET Core, cadeias de caracteres de conexão de configuração no serviço de aplicativo são como defini-las na `<connectionStrings>` na *Web. config*, mas os valores definidos no serviço de aplicativo substituem aquelas na *Web. config*. Você pode manter as configurações de desenvolvimento (por exemplo, um arquivo de banco de dados) em *Web. config* e segredos de produção (por exemplo, credenciais de banco de dados SQL) seguros no serviço de aplicativo. O mesmo código usa as configurações de desenvolvimento quando você depura localmente e, em seguida, usa seus segredos de produção quando implantado no Azure.

Para as pilhas de outros idiomas, é melhor usar [configurações do aplicativo](#configure-app-settings) em vez disso, como cadeias de caracteres de conexão exigem formatação especial nas chaves variável no pedido para acessar os valores. Aqui está uma exceção, no entanto: certos tipos de banco de dados do Azure são obtidos em backup junto com o aplicativo se você configurar suas cadeias de conexão em seu aplicativo. Para obter mais informações, consulte [que é feito backup](manage-backup.md#what-gets-backed-up). Se você não precisa esse backup automatizado, em seguida, use as configurações do aplicativo.

Em tempo de execução, as cadeias de caracteres de conexão estão disponíveis como variáveis de ambiente, prefixadas com os seguintes tipos de conexão:

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* Banco de Dados SQL: `SQLAZURECONNSTR_`
* Personalizado: `CUSTOMCONNSTR_`

Por exemplo, uma cadeia de caracteres de conexão do MySql denominado *CadeiaDeCaracteresDeConexao1* pode ser acessado como a variável de ambiente `MYSQLCONNSTR_connectionString1`. Para etapas específicas da pilha de idiomas, consulte:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Contêineres personalizados](containers/configure-custom-container.md#configure-environment-variables)

As cadeias de conexão são sempre criptografadas quando armazenadas (criptografadas em repouso).

> [!NOTE]
> Cadeias de caracteres de Conexão também podem ser resolvidas a partir [Key Vault](/azure/key-vault/) usando [faz referência a Key Vault](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Mostrar valores ocultos

Por padrão, os valores para cadeias de caracteres de conexão estão ocultos no portal para a segurança. Para ver um valor oculto de uma cadeia de conexão, basta clicar o **valor** campo dessa cadeia de caracteres. Para ver os valores de todas as cadeias de caracteres de conexão, clique o **Mostrar valor** botão.

### <a name="add-or-edit"></a>Adicionar ou editar

Para adicionar uma nova cadeia de caracteres de conexão, clique em **nova cadeia de caracteres de conexão**. Na caixa de diálogo, você pode [fique a cadeia de caracteres de conexão para o slot atual](deploy-staging-slots.md#which-settings-are-swapped).

Para editar uma configuração, clique o **editar** botão no lado direito.

Quando terminar, clique em **atualização**. Não se esqueça de clicar **salvar** volta a **configuração** página.

### <a name="edit-in-bulk"></a>Editar em massa

Para adicionar ou editar cadeias de caracteres de conexão em massa, clique no **avançadas de edição** botão. Quando terminar, clique em **atualização**. Não se esqueça de clicar **salvar** volta a **configuração** página.

Cadeias de caracteres de Conexão tem a formatação do seguinte JSON:

```json
[
  {
    "name": "name-1",
    "value": "conn-string-1",
    "type": "SQLServer",
    "slotSetting": false
  },
  {
    "name": "name-2",
    "value": "conn-string-2",
    "type": "PostgreSQL",
    "slotSetting": false
  },
  ...
]
```

<a name="platform"></a>
<a name="alwayson"></a>

## <a name="configure-general-settings"></a>Definir as configurações gerais

No [portal do Azure], navegue até a página de gerenciamento do aplicativo. No menu da esquerda do aplicativo, clique em **Configuration** > **configurações do aplicativo**.

![Configurações gerais](./media/configure-common/open-general.png)

Aqui, você pode configurar algumas configurações comuns para o aplicativo. Algumas configurações exigem que você [escalar verticalmente para tipos de preço mais alto](web-sites-scale.md).

- **Configurações de pilha**: A pilha de software para executar o aplicativo, incluindo o idioma e as versões do SDK. Para aplicativos Linux e aplicativos de contêiner personalizado, você também pode definir um comando de inicialização opcional ou um arquivo.
- **As configurações de plataforma**: Permite que você definir as configurações para a plataforma de hospedagem, incluindo:
    - **Número de bits**: 32 bits ou 64 bits.
    - **Protocolo WebSocket**: Para [ASP.NET SignalR] ou [socket.io](https://socket.io/), por exemplo.
    - **Sempre ativo**: Mantenha o aplicativo carregado mesmo quando não há nenhum tráfego. Você precisa habilitá-lo para WebJobs contínuos ou para trabalhos Web disparados usando uma expressão CRON.
    - **Versão do pipeline gerenciada**: O IIS [modo de pipeline]. Defina-o como **clássico** se você tiver um aplicativo herdado que requer uma versão mais antiga do IIS.
    - **Versão HTTP**: Defina como **2.0** para habilitar suporte para o protocolo [HTTPS/2](https://wikipedia.org/wiki/HTTP/2).
    > [!NOTE]
    > A maioria dos navegadores modernos dá suporte para protocolo HTTP/2 somente em TLS, enquanto o tráfego não criptografado continua usando HTTP / 1.1. Para garantir que o cliente navegadores se conectar ao seu aplicativo com HTTP/2, tanto [compre um certificado de serviço de aplicativo](web-sites-purchase-ssl-web-site.md) para o domínio personalizado do aplicativo ou [associar um certificado de terceiros](app-service-web-tutorial-custom-ssl.md).
    - **Afinidade ARR**: Em uma implantação de várias instâncias, certifique-se de que o cliente é roteado para a mesma instância durante a vida útil da sessão. Você pode definir essa opção como **desativar** para aplicativos sem monitoração de estado.
- **Depuração**: Habilitar a depuração remota para [ASP.NET](troubleshoot-dotnet-visual-studio.md#remotedebug), [ASP.NET Core](/visualstudio/debugger/remote-debugging-azure), ou [Node. js](containers/configure-language-nodejs.md#debug-remotely) aplicativos. Esta opção desativa automaticamente após 48 horas.
- **Certificados de cliente de entrada**: exigir certificados de cliente no [autenticação mútua](app-service-web-configure-tls-mutual-auth.md).

## <a name="configure-default-documents"></a>Configurar os documentos padrão

Essa configuração é apenas para aplicativos do Windows.

No [portal do Azure], navegue até a página de gerenciamento do aplicativo. No menu da esquerda do aplicativo, clique em **Configuration** > **documentos padrão**.

![Configurações gerais](./media/configure-common/open-documents.png)

O documento padrão é a página da web que é exibido na URL da raiz de um site. O primeiro arquivo correspondente na lista é usado. Para adicionar um novo documento padrão, clique em **novo documento**. Não se esqueça de clicar **salvar**.

Se o aplicativo usa módulos de rota com base na URL em vez de servir conteúdo estático, não é necessário para os documentos padrão.

## <a name="configure-path-mappings"></a>Configurar mapeamentos de caminho

No [portal do Azure], navegue até a página de gerenciamento do aplicativo. No menu da esquerda do aplicativo, clique em **Configuration** > **mapeamentos de caminho**.

![Configurações gerais](./media/configure-common/open-path.png)

O **mapeamentos de caminho** página mostra as coisas diferentes com base no tipo de sistema operacional.

### <a name="windows-apps-uncontainerized"></a>Aplicativos do Windows (uncontainerized)

Para aplicativos do Windows, você pode personalizar os mapeamentos de manipulador do IIS e diretórios e aplicativos virtuais.

Mapeamentos de manipulador permitem que você adicione processadores de script personalizado para manipular solicitações para extensões de arquivo específico. Para adicionar um manipulador personalizado, clique em **novo manipulador**. Configure o manipulador da seguinte maneira:

- **Extensão**. A extensão de arquivo que você deseja manipular, tais como  *\*. PHP* ou *fcgi*.
- **Processador de script**. O caminho absoluto do processador de script para você. As solicitações para arquivos que correspondem a extensão de arquivo são processadas pelo processador de script. Use o caminho `D:\home\site\wwwroot` para se referir ao diretório raiz do seu aplicativo.
- **Argumentos**. Argumentos de linha de comando opcionais para o processador de script.

Cada aplicativo tem o caminho de raiz padrão (`/`) mapeada para `D:\home\site\wwwroot`, onde seu código é implantado por padrão. Se a raiz do seu aplicativo estiver em uma pasta diferente, ou se o repositório tiver mais de um aplicativo, você pode editar ou adicionar diretórios aqui e aplicativos virtuais. Clique em **novo aplicativo virtual ou o diretório**.

Para configurar diretórios e aplicativos virtuais, especifique cada diretório virtual e seu caminho físico correspondente em relação à raiz do site (`D:\home`). Opcionalmente, você pode marcar a caixa de seleção **Aplicativo** para marcar um diretório virtual como um aplicativo.

### <a name="containerized-apps"></a>Aplicativos em contêineres

Você pode [adicionar armazenamento personalizado para seu aplicativo em contêineres](containers/how-to-serve-content-from-azure-storage.md). Aplicativos em contêineres incluem todos os aplicativos do Linux e também os contêineres personalizados Windows e Linux em execução no serviço de aplicativo. Clique em **monte de armazenamento do Azure novo** e configurar o armazenamento de personalizados da seguinte maneira:

- **Nome**: O nome de exibição.
- **Opções de configuração**: **Básica** ou **Advanced**.
- **Contas de armazenamento**: A conta de armazenamento com o contêiner desejado.
- **Tipo de armazenamento**: **Blobs do Azure** ou **os arquivos do Azure**.
  > [!NOTE]
  > Aplicativos de contêiner do Windows dão suporte apenas a arquivos do Azure.
- **Contêiner de armazenamento**: Para a configuração básica, o contêiner desejado.
- **Nome do compartilhamento**: Para configuração avançada, o compartilhamento de arquivos nome.
- **Chave de acesso**: Para configuração avançada, a chave de acesso.
- **Caminho de montagem**: O caminho absoluto no seu contêiner para montar o armazenamento personalizado.

Para obter mais informações, consulte [servir conteúdo do armazenamento do Azure no serviço de aplicativo no Linux](containers/how-to-serve-content-from-azure-storage.md).

## <a name="configure-language-stack-settings"></a>Definir as configurações de pilha de linguagem

Para aplicativos do Linux, consulte:

- [ASP.NET Core](containers/configure-language-dotnetcore.md)
- [Node.js](containers/configure-language-nodejs.md)
- [PHP](containers/configure-language-php.md)
- [Python](containers/how-to-configure-python.md)
- [Java](containers/configure-language-java.md)
- [Ruby](containers/configure-language-ruby.md)

## <a name="configure-custom-containers"></a>Configurar contêineres personalizados

Consulte [configurar um contêiner do Linux personalizado para o serviço de aplicativo do Azure](containers/configure-custom-container.md)

## <a name="next-steps"></a>Próximas etapas

- [Configurar um nome de domínio personalizado no Serviço de Aplicativo do Azure]
- [Configurar ambientes de preparo no serviço de aplicativo do Azure]
- [Habilitar HTTPS para um aplicativo no Serviço de Aplicativo do Azure]
- [Habilitar logs de diagnóstico](troubleshoot-diagnostic-logs.md)
- [Dimensionar um aplicativo no Serviço de Aplicativo do Azure]
- [Conceitos básicos de monitoramento no Serviço de Aplicativo do Azure]
- [Alterar configurações de applicationHost. config com XDT](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Portal do Azure]: https://portal.azure.com/
[Configurar um nome de domínio personalizado no Serviço de Aplicativo do Azure]: ./app-service-web-tutorial-custom-domain.md
[Configurar ambientes de preparo no serviço de aplicativo do Azure]: ./deploy-staging-slots.md
[Habilitar HTTPS para um aplicativo no Serviço de Aplicativo do Azure]: ./app-service-web-tutorial-custom-ssl.md
[How to: Monitor web endpoint status]: https://go.microsoft.com/fwLink/?LinkID=279906
[Conceitos básicos de monitoramento no Serviço de Aplicativo do Azure]: ./web-sites-monitor.md
[modo de pipeline]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Dimensionar um aplicativo no Serviço de Aplicativo do Azure]: ./web-sites-scale.md

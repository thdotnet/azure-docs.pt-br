---
title: Configurar aplicativos Java do Linux - serviço de aplicativo do Azure | Microsoft Docs
description: Saiba como configurar aplicativos Java em execução no Serviço de Aplicativo do Azure no Linux.
keywords: o serviço de aplicativo do Azure, aplicativo web, linux, oss, java, java ee, jee, javaee
services: app-service
author: rloutlaw
manager: angerobe
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 03/28/2019
ms.author: routlaw
ms.custom: seodec18
ms.openlocfilehash: cf9356c2792781558c4403608ff5de0e3aaddb6a
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254459"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Configurar um aplicativo Linux Java para o serviço de aplicativo do Azure

O Serviço de Aplicativo do Azure no Linux permite que desenvolvedores de Java criem, implantem e escalonem rapidamente os aplicativos Web empacotados do Tomcat ou do Java Standard Edition (SE) em um serviço baseado em Linux totalmente gerenciado. Implante aplicativos com o plug-ins do Maven na linha de comando ou em editores, como IntelliJ, Eclipse ou Visual Studio Code.

Este guia fornece instruções para desenvolvedores de Java que usam um contêiner interno do Linux no serviço de aplicativo e principais conceitos. Se você nunca usou o serviço de aplicativo do Azure, siga as [início rápido de Java](quickstart-java.md) e [Java com o tutorial do PostgreSQL](tutorial-java-enterprise-postgresql-app.md) primeiro.

## <a name="deploying-your-app"></a>Implantação do aplicativo

Você pode usar [plug-in do Maven para serviço de aplicativo do Azure](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) para implantar os arquivos. jar e. war. Também há suporte para implantação com IDEs populares com [Kit de ferramentas do Azure para IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij) ou [Kit de ferramentas do Azure para Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse).

Caso contrário, o método de implantação será dependem de seu tipo de arquivo morto:

- Para implantar arquivos .war para Tomcat, use o ponto de extremidade `/api/wardeploy/` para executar POST de seu arquivo morto. Para obter mais informações sobre essa API, confira [essa documentação](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file).
- Para implantar arquivos .jar nas imagens do Java SE, use o ponto de extremidade `/api/zipdeploy/` do site do Kudu. Para obter mais informações sobre essa API, confira [essa documentação](https://docs.microsoft.com/azure/app-service/deploy-zip#rest).

Não implante seu .war ou .jar usando FTP. A ferramenta FTP foi projetada para carregar os scripts de inicialização, dependências ou outros arquivos de tempo de execução. Não é a opção ideal para a implantação de aplicativos Web.

## <a name="logging-and-debugging-apps"></a>Aplicativos de registro em log e depuração

Relatórios de desempenho, visualizações de tráfego e verificações de integridade estão disponíveis para cada aplicativo por meio do portal do Azure. Para obter mais informações, consulte [visão geral de diagnóstico do serviço de aplicativo do Azure](../overview-diagnostics.md).

### <a name="ssh-console-access"></a>Acesso ao console SSH

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Logs de diagnóstico de fluxo

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

Para saber mais, confira [Transmitir logs com a CLI do Azure](../troubleshoot-diagnostic-logs.md#streaming-with-azure-cli).

### <a name="app-logging"></a>Registro em log do aplicativo

Habilite o [registro em log de aplicativos](../troubleshoot-diagnostic-logs.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#enablediag) por meio do portal do Azure ou da [CLI do Azure](/cli/azure/webapp/log#az-webapp-log-config) para configurar o Serviço de Aplicativo do Azure para gravar a saída do console padrão do aplicativo e os fluxos de erro do console padrão no sistema de arquivos local ou no Armazenamento de Blobs do Azure. O registro em log na instância do sistema de arquivos do Serviço de Aplicativo local será desabilitado 12 horas após ser configurado. Se você precisar de uma retenção mais longa, configure o aplicativo para gravar a saída em um contêiner do armazenamento de blobs. Seus logs de aplicativos Java e Tomcat podem ser encontradas no `/home/LogFiles/Application/` directory.

Se o aplicativo usar [Logback](https://logback.qos.ch/) ou [Log4j](https://logging.apache.org/log4j) para rastreamento, será possível encaminhá-los para revisão no Azure Application Insights usando as instruções de configuração de estrutura de registros em [Explorar os logs de rastreamento de Java no Application Insights](/azure/application-insights/app-insights-java-trace-logs).

### <a name="troubleshooting-tools"></a>Solucionando problemas de ferramentas

Imagens internas do Java se baseiam os [Alpine Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html) sistema operacional. Use o `apk` Gerenciador de pacotes para instalar qualquer solução de problemas de ferramentas ou comandos.

## <a name="customization-and-tuning"></a>Personalização e ajuste

O serviço de aplicativo do Azure para Linux dá suporte a fora a caixa de ajuste e a personalização por meio do portal do Azure e da CLI. Examine os seguintes artigos para a configuração de aplicativo web não específico de Java:

- [Definir configurações de aplicativo](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)
- [Configurar um domínio personalizado](../app-service-web-tutorial-custom-domain.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Habilitar SSL](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Adicionar uma CDN](../../cdn/cdn-add-to-web-app.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Configurar o site do Kudu](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Definir opções de tempo de execução do Java

Para definir a memória alocada ou outras opções de tempo de execução da JVM em ambientes de Java SE e o Tomcat, crie uma [configuração de aplicativo](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) denominado `JAVA_OPTS` com as opções. O Serviço de Aplicativo no Linux passa essa configuração como uma variável de ambiente para o tempo de execução do Java quando ele é iniciado.

No portal do Azure, em **Configurações do aplicativo** para o aplicativo Web, crie uma configuração de aplicativo denominada `JAVA_OPTS` que inclui as configurações adicionais, como `-Xms512m -Xmx1204m`.

Para configurar a configuração de aplicativo do plug-in do Maven, adicione marcas/valor da configuração na seção de plug-in do Azure. O exemplo a seguir define um tamanho de heap de Java mínimo e máximo específico:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Os desenvolvedores que executam um único aplicativo com um slot de implantação no Plano do Serviço de Aplicativo podem usar as seguintes opções:

- Instâncias B1 e S1: `-Xms1024m -Xmx1024m`
- Instâncias B2 e S2: `-Xms3072m -Xmx3072m`
- Instâncias B3 e S3: `-Xms6144m -Xmx6144m`

Ao ajustar as configurações de heap do aplicativo, examine os detalhes do Plano do Serviço de Aplicativo e considere os vários aplicativos e slots de implantação necessários para encontrar a alocação de memória ideal.

Se você estiver implantando um aplicativo de JAR, ele deve ser chamado `app.jar` para que a imagem interna possa identificar corretamente o seu aplicativo. (O plug-in do Maven faz essa renomeação automaticamente.) Se você não quiser renomear seu JAR do `app.jar`, você pode carregar um script de shell com o comando para executar o JAR. Em seguida, cole o caminho completo para esse script na [arquivo de inicialização](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-faq#startup-file) caixa de texto na seção de configuração do portal.

### <a name="turn-on-web-sockets"></a>Ativar os soquetes da Web

Ative o suporte para soquetes da Web no portal do Azure, nas **Configurações de aplicativo**. Será necessário reiniciar o aplicativo para a configuração entrar em vigor.

Ative o suporte para soquete da Web usando a CLI do Azure com o seguinte comando:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

Depois, reinicie o aplicativo:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Definir a codificação de caractere padrão

No portal do Azure, em **Configurações do aplicativo** para o aplicativo Web, crie uma configuração de aplicativo denominada `JAVA_OPTS` com o valor `-Dfile.encoding=UTF-8`.

Como alternativa, é possível definir a configuração do aplicativo usando o plug-in do Maven do Serviço de Aplicativo. Adicione as marcas de nome e valor de configuração à configuração do plug-in:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="adjust-startup-timeout"></a>Ajustar o tempo limite de inicialização

Se seu aplicativo Java é especialmente grande, você deverá aumentar o limite de tempo de inicialização. Para fazer isso, crie uma configuração de aplicativo, `WEBSITES_CONTAINER_START_TIME_LIMIT` e defini-lo como o número de segundos que o serviço de aplicativo deve aguardar antes de atingir o tempo limite. O valor máximo é `1800` segundos.

## <a name="secure-applications"></a>Aplicativos seguros

Os aplicativos Java em execução no Serviço de Aplicativo para Linux têm o mesmo conjunto de [melhores práticas de segurança](/azure/security/security-paas-applications-using-app-services) que outros aplicativos. 

### <a name="authenticate-users"></a>Autenticar usuários

Configurar a autenticação do aplicativo no portal do Azure com o **autenticação e autorização** opção. Em seguida, será possível habilitar a autenticação usando o Azure Active Directory ou os logons de redes sociais, como Facebook, Google ou GitHub. A configuração do portal do Azure só funciona ao configurar um único provedor de autenticação. Para saber mais, confira [Configurar o aplicativo do Serviço de Aplicativo para usar o logon do Azure Active Directory](../configure-authentication-provider-aad.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) e os artigos relacionados para outros provedores de identidade. Se você precisar habilitar vários provedores de entrada, siga as instruções no artigo [Personalizar a autenticação do Serviço de Aplicativo](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

#### <a name="tomcat"></a>Tomcat

Seu aplicativo Tomcat pode acessar o usuário declarações diretamente do servlet Tomcat convertendo a entidade de segurança do objeto para um objeto de mapa. O objeto de mapa mapeará cada tipo de declaração para uma coleção de declarações para esse tipo. No código a seguir, `request` é uma instância de `HttpServletRequest`.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Agora você pode inspecionar o `Map` objeto para qualquer declaração específica. Por exemplo, o trecho de código a seguir itera em todos os tipos de declaração e imprime o conteúdo de cada coleção.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Para desconectar os usuários e executar outras ações, consulte a documentação sobre [uso do aplicativo de serviço de autenticação e autorização](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to). Também há documentação oficial no Tomcat [HttpServletRequest interface](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) e seus métodos. O servlet seguir métodos também são hidratados com base em sua configuração de serviço de aplicativo:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Para desabilitar esse recurso, crie uma configuração de aplicativo chamada `WEBSITE_AUTH_SKIP_PRINCIPAL` com um valor de `1`. Para desabilitar todos os filtros de servlet adicionados pelo serviço de aplicativo, crie uma configuração denominada `WEBSITE_SKIP_FILTERS` com um valor de `1`.

#### <a name="spring-boot"></a>Spring Boot

Os desenvolvedores do Spring Boot podem usar o [inicializador do Spring Boot do Azure Active Directory](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) para proteger aplicativos usando APIs e anotações conhecidas do Spring Security. Lembre-se de aumentar o tamanho máximo do cabeçalho no arquivo `application.properties`. Sugerimos um valor igual a `16384`.

### <a name="configure-tlsssl"></a>Configurar TLS/SSL

Siga as instruções em [Associar um certificado SSL personalizado existente](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) para carregar um certificado SSL existente e associá-lo ao nome de domínio do aplicativo. Por padrão, o aplicativo ainda permitirá conexões HTTP – siga as etapas específicas no tutorial para impor o SSL e o TLS.

### <a name="use-keyvault-references"></a>Usar referências de Cofre de chaves

[Azure KeyVault](../../key-vault/key-vault-overview.md) fornece gerenciamento centralizado de segredos com histórico de auditoria e políticas de acesso. Você pode armazenar segredos (como senhas ou cadeias de caracteres de conexão) no cofre de chaves e acessar esses segredos em seu aplicativo por meio de variáveis de ambiente.

Primeiro, siga as instruções para [concedendo acesso ao aplicativo para o Key Vault](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) e [fazer uma referência ao seu segredo do KeyVault em uma configuração de aplicativo](../app-service-key-vault-references.md#reference-syntax). Você pode validar que a referência será resolvida para o segredo, imprimindo a variável de ambiente ao acessar remotamente o terminal de serviço de aplicativo.

Para injetar esses segredos no arquivo de configuração Spring ou Tomcat, use a sintaxe de injeção de variável de ambiente (`${MY_ENV_VAR}`). Para arquivos de configuração de Spring, consulte esta documentação sobre [externalizados configurações](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

## <a name="configure-apm-platforms"></a>Configurar plataformas do APM

Esta seção mostra como conectar aplicativos Java implantados no serviço de aplicativo do Azure no Linux com o desempenho do aplicativo NewRelic e da AppDynamics monitoramento plataformas (APM).

[Configurar o New Relic](#configure-new-relic)
[configurar AppDynamics](#configure-appdynamics)

### <a name="configure-new-relic"></a>Configurar o New Relic

1. Criar uma conta de NewRelic em [NewRelic.com](https://newrelic.com/signup)
2. Fazer o download do agente de Java do NewRelic, ele terá um nome de arquivo semelhante ao `newrelic-java-x.x.x.zip`.
3. Copie sua chave de licença, você precisará dela para configurar o agente mais tarde.
4. [SSH em sua instância do serviço de aplicativo](app-service-linux-ssh-support.md) e crie um novo diretório `/home/site/wwwroot/apm`.
5. Carregue os arquivos do agente NewRelic Java descompactados em um diretório em `/home/site/wwwroot/apm`. Os arquivos para seu agente devem estar em `/home/site/wwwroot/apm/newrelic`.
6. Modifique o arquivo YAML em `/home/site/wwwroot/apm/newrelic/newrelic.yml` e substitua o valor de licença de espaço reservado com sua própria chave de licença.
7. No portal do Azure, navegue até seu aplicativo no serviço de aplicativo e criar uma nova configuração de aplicativo.
    - Se seu aplicativo estiver usando **Java SE**, crie uma variável de ambiente denominada `JAVA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Se você estiver usando **Tomcat**, crie uma variável de ambiente denominada `CATALINA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Se você estiver usando **WildFly**, consulte a documentação do New Relic [aqui](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) para obter orientação sobre como instalar o agente de Java e a configuração do JBoss.
    - Se você já tiver uma variável de ambiente para `JAVA_OPTS` ou `CATALINA_OPTS`, acrescente o `javaagent` opção ao final do valor atual.

### <a name="configure-appdynamics"></a>Configurar o AppDynamics

1. Criar uma conta do AppDynamics em [AppDynamics.com](https://www.appdynamics.com/community/register/)
1. Fazer o download do agente Java do site do AppDynamics, o nome do arquivo será semelhante a `AppServerAgent-x.x.x.xxxxx.zip`
1. [SSH em sua instância do serviço de aplicativo](app-service-linux-ssh-support.md) e crie um novo diretório `/home/site/wwwroot/apm`.
1. Carregue os arquivos do agente Java descompactados em um diretório em `/home/site/wwwroot/apm`. Os arquivos para seu agente devem estar em `/home/site/wwwroot/apm/appdynamics`.
1. No portal do Azure, navegue até seu aplicativo no serviço de aplicativo e criar uma nova configuração de aplicativo.
    - Se seu aplicativo estiver usando **Java SE**, crie uma variável de ambiente denominada `JAVA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` quando `<app-name>` for o nome do seu Serviço de Aplicativo.
    - Se você estiver usando **Java SE**, crie uma variável de ambiente denominada `CATALINA_OPTS` com o valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` quando `<app-name>` for o nome do seu Serviço de Aplicativo.
    - Se você estiver usando **WildFly**, consulte a documentação do AppDynamics [aqui](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) para obter orientação sobre como instalar o agente de Java e a configuração do JBoss.
    
## <a name="configure-jar-applications"></a>Configurar aplicativos de JAR

### <a name="starting-jar-apps"></a>Iniciando aplicativos JAR

Por padrão, o serviço de aplicativo espera que seu aplicativo JAR seja nomeado `app.jar`. Se ele tem esse nome, ele será executado automaticamente. Para usuários do Maven, você pode definir o nome do arquivo JAR, incluindo `<finalName>app</finalName>` no `<build>` seção do seu `pom.xml`. [Você pode fazer o mesmo no Gradle](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName) definindo o `archiveFileName` propriedade.

Se você quiser usar um nome diferente para o JAR, você também deve fornecer o [o comando de inicialização](app-service-linux-faq.md#built-in-images) que executa o arquivo JAR. Por exemplo: `java -jar my-jar-app.jar`. Você pode definir o valor para o comando de inicialização no Portal, na configuração > Configurações gerais, ou com uma configuração de aplicativo denominado `STARTUP_COMMAND`.

### <a name="server-port"></a>Porta do servidor

Serviço de aplicativo Linux roteia as solicitações de entrada para a porta 80, para que seu aplicativo deve escutar na porta 80. Você pode fazer isso na configuração do seu aplicativo (como o do Spring `application.properties` arquivo), ou em seu comando de inicialização (por exemplo, `java -jar spring-app.jar --server.port=80`). Consulte a documentação para estruturas comuns de Java a seguir:

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [SparkJava](http://sparkjava.com/documentation#embedded-web-server)
- [Micronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Reproduzir Framework](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Quarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>Fontes de dados

### <a name="tomcat"></a>Tomcat

Essas instruções se aplicam a todas as conexões de banco de dados. Você precisará preencher os espaços reservados com nome de classe do driver do banco de dados escolhido e o arquivo JAR. É fornecida uma tabela com nomes de classe e downloads de driver para bancos de dados comuns.

| Banco de dados   | Nome de Classe do Driver                             | Driver JDBC                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [Baixar](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [Baixar](https://dev.mysql.com/downloads/connector/j/) (Selecione "Independente de Plataforma") |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [Baixar](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#available-downloads-of-jdbc-driver-for-sql-server)                                                           |

Para configurar o Tomcat para usar a API de persistência de Java (JPA) ou o banco de dados de conectividade de JDBC (Java), primeiro personalizar o `CATALINA_OPTS` variável de ambiente que é lido em Tomcat na inicialização. Defina esses valores por meio de uma configuração de aplicativo no [plug-in Maven do Serviço de Aplicativo](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md):

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

Ou defina as variáveis de ambiente na **Configuration** > **configurações do aplicativo** página no portal do Azure.

Em seguida, determine se a fonte de dados deve estar disponível para um aplicativo ou para todos os aplicativos em execução no servlet do Tomcat.

#### <a name="application-level-data-sources"></a>Fontes de dados de nível de aplicativo

1. Crie um arquivo `context.xml` no diretório `META-INF/` do seu projeto. Crie o diretório `META-INF/` se ele não existir.

2. Em `context.xml`, adicione um elemento `Context` para vincular a fonte de dados a um endereço JNDI. Substitua o espaço reservado `driverClassName` pelo nome de classe do seu driver da tabela acima.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection" 
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}" 
            password="${connURL}"
        />
    </Context>
    ```

3. Atualize o `web.xml` do aplicativo para usar a fonte de dados no aplicativo.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Recursos de nível de servidor compartilhados

1. Copie o conteúdo de `/usr/local/tomcat/conf` para `/home/tomcat/conf` na instância do Serviço de Aplicativo no Linux usando SSH se já não houver uma configuração.
    ```
    mkdir -p /home/tomcat
    cp -a /usr/local/tomcat/conf /home/tomcat/conf
    ```

2. Adicione um elemento de Contexto em seu `server.xml` dentro do elemento `<Server>`.

    ```xml
    <Server>
    ...
    <Context>
        <Resource
            name="jdbc/dbconnection" 
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}" 
            password="${connURL}"
        />
    </Context>
    ...
    </Server>
    ```

3. Atualize o `web.xml` do aplicativo para usar a fonte de dados no aplicativo.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="finalize-configuration"></a>Finalizar a configuração

Por fim, coloque os JARs de driver no classpath do Tomcat e reinicie o serviço de aplicativo.

1. Verifique se os arquivos do driver JDBC estão disponíveis para o carregador de classes do Tomcat colocando-os no diretório `/home/tomcat/lib`. (Crie esse diretório se ele ainda não existir.) Para carregar esses arquivos na instância do Serviço de Aplicativo, siga estas etapas:
    1. No [Cloud Shell](https://shell.azure.com), instale a extensão de aplicativo Web:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. Execute o seguinte comando CLI para criar um túnel SSH de seu sistema local para o serviço de aplicativo:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. Faça a conexão da porta de túnel local com o cliente SFTP e carregue os arquivos na pasta `/home/tomcat/lib`.

    Como alternativa, você pode usar um cliente de FTP para carregar o driver JDBC. Siga estas [instruções para obter suas credenciais FTP](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

2. Se você tiver uma fonte de dados no nível do servidor, reinicie o aplicativo Linux Serviço de Aplicativo. O Tomcat redefinirá `CATALINA_HOME` como `/home/tomcat/conf` e usará a configuração atualizada.

### <a name="spring-boot"></a>Spring Boot

Para se conectar a fontes de dados em aplicativos Spring Boot, sugerimos criar cadeias de caracteres de conexão e injetando-as em seu `application.properties` arquivo.

1. Na seção "Configuração" da página serviço de aplicativo, defina um nome para a cadeia de caracteres, cole a cadeia de conexão JDBC no campo de valor e defina o tipo como "Custom". Opcionalmente, você pode definir essa cadeia de caracteres de conexão como configuração do slot.

    Essa cadeia de caracteres de conexão é acessível ao nosso aplicativo como uma variável de ambiente denominada `CUSTOMCONNSTR_<your-string-name>`. Por exemplo, a cadeia de caracteres de conexão que criamos acima será nomeada `CUSTOMCONNSTR_exampledb`.

2. No seu `application.properties` de arquivos, fazer referência a essa cadeia de caracteres de conexão com o nome da variável de ambiente. Para nosso exemplo, usaríamos a seguir.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Consulte a [documentação do Spring Boot no acesso a dados](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) e [externalizados configurações](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) para obter mais informações sobre esse tópico.

## <a name="configure-java-ee-wildfly"></a>Configurar o Java EE (WildFly)

> [!NOTE]
> Java Enterprise Edition no serviço de aplicativo Linux está atualmente em visualização. Essa pilha está **não** recomendado para o trabalho voltado para a produção. informações sobre nossos pilhas de Java SE e Tomcat.

Serviço de aplicativo do Azure no Linux permite aos desenvolvedores de Java para compilar, implantar e dimensionar aplicativos empresariais de Java (Java EE) em um serviço totalmente gerenciado baseado em Linux.  O ambiente de tempo de execução do Java Enterprise subjacente é o servidor de aplicativos de software livre [Wildfly](https://wildfly.org/).

[Escala com serviço de aplicativo](#scale-with-app-service)
[configuração do servidor de aplicativos personalizar](#customize-application-server-configuration)
[módulos e dependências](#modules-and-dependencies)
[dados fontes](#data-sources)
[habilitar provedores de sistema de mensagens](#enable-messaging-providers)
[configurar o cache do gerenciamento de sessão](#configure-session-management-caching)

### <a name="scale-with-app-service"></a>Escalonar com o Serviço de Aplicativo

O servidor de aplicativos WildFly em execução no Serviço de Aplicativo no Linux é executado em modo autônomo, não em uma configuração de domínio. Quando você escala horizontalmente o Plano do Serviço de Aplicativo, cada instância de WildFly é configurada como um servidor autônomo.

 Dimensione seu aplicativo vertical ou horizontalmente com [regras de dimensionamento](../../monitoring-and-diagnostics/monitoring-autoscale-get-started.md) e [aumentando sua contagem de instâncias](../web-sites-scale.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

### <a name="customize-application-server-configuration"></a>Personalizar a configuração do servidor de aplicativos

Instâncias do aplicativo Web são sem monitoração de estado, portanto, cada nova instância iniciada deve ser configurada na inicialização para suportar a configuração de Wildfly exigida pelo aplicativo.
Você pode escrever uma script Bash para chamar a CLI WildFly para inicialização:

- Principais fontes de dados
- Configurar provedores de mensagens
- Adicionar outros módulos e dependências à configuração do servidor Wildfly.

 O script é executado quando o Wildfly está em execução, mas antes que o aplicativo seja iniciado. O script deve usar a [CLI JBOSS](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) chamada de `/opt/jboss/wildfly/bin/jboss-cli.sh` para configurar o servidor de aplicativos com qualquer configuração ou com alterações necessárias após o servidor ser iniciado.

Não use o modo interativo da CLI para configurar o Wildfly. Em vez disso, é possível fornecer um script de comandos da CLI JBoss usando o comando `--file`, por exemplo:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Faça upload do script de inicialização para `/home/site/deployments/tools` na instância do Serviço de Aplicativo. Confira [este documento](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#userscope) para obter instruções sobre como obter as credenciais de FTP.

Defina o campo **Script de inicialização** no portal do Azure como a localização do seu script de shell de inicialização, por exemplo `/home/site/deployments/tools/your-startup-script.sh`.

Fornecer [configurações do aplicativo](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) na configuração do aplicativo para passar as variáveis de ambiente para usar no script. As configurações de aplicativo mantém cadeias de caracteres de conexão e outros segredos necessários para configurar seu aplicativo fora do controle de versão.

### <a name="modules-and-dependencies"></a>Módulos e dependências

Para instalar módulos e suas dependências no classpath do Wildfly por meio da CLI JBoss, será necessário criar os seguintes arquivos em seu próprio diretório. Alguns módulos e dependências talvez precisem de configuração adicional, como nomenclatura JNDI ou outra configuração específica de API; portanto, essa lista é um conjunto mínimo do que será necessário para configurar uma dependência na maioria dos casos.

- Um [descritor do módulo XML](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Esse arquivo XML define o nome, os atributos e as dependências do seu módulo. Esse [arquivo module.xml de exemplo](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) define um módulo Postgres, sua dependência do JDBC de arquivo JAR e outras dependências de módulo necessárias.
- Quaisquer dependências de arquivo JAR necessárias para seu módulo.
- Um script com seus comandos da CLI JBoss para configurar o novo módulo. Esse arquivo conterá seus comandos para serem executados pela CLI JBoss para configurar o servidor para usar a dependência. Para obter documentação sobre os comandos para adicionar módulos e provedores de mensagens, veja [este documento](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
- Um script de inicialização de Bash para chamar a CLI JBoss e executar o script na etapa anterior. Esse arquivo será executado quando a instância do Serviço de Aplicativo for reiniciada ou quando novas instâncias forem provisionadas durante uma expansão. Nesse script de inicialização, você pode executar quaisquer outras configurações para seu aplicativo conforme os comandos JBoss são passados para a CLI JBoss. No mínimo, esse arquivo pode ser um único comando para passar seu script de comando da CLI JBoss para a CLI JBoss:

```bash
`/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli`
```

Depois que você tiver os arquivos e o conteúdo do seu módulo, siga as etapas abaixo para adicionar o módulo ao servidor de aplicativos Wildfly.

1. Carregue seus arquivos por FTP para o `/home/site/deployments/tools` na sua instância do Serviço de Aplicativo. Confira este documento para obter instruções sobre como obter as credenciais de FTP.
2. No **Configuration** > **configurações gerais** do campo de página do portal, defina o "Script de inicialização" Azure para o local do seu script de shell de inicialização, por exemplo `/home/site/deployments/tools/your-startup-script.sh` .
3. Reinicie a instância do serviço de aplicativo pressionando a **reinicie** botão na **visão geral** seção do portal ou usando a CLI do Azure.

### <a name="configure-data-source-connections"></a>Configurar conexões de fonte de dados

Para configurar o Wildfly para uma conexão de fonte de dados, siga o mesmo procedimento descrito acima na seção Instalando módulos e dependências. É possível seguir as mesmas etapas para qualquer serviço de Banco de dados do Azure.

1. Baixe o driver JDBC para seu tipo de banco de dados. Para sua conveniência, confira os drivers do [Postgres](https://jdbc.postgresql.org/download.html) e do [MySQL](https://dev.mysql.com/downloads/connector/j/). Desempacote o download para obter o arquivo .jar.
2. Siga as etapas descritas em "Módulos e dependências" para criar e fazer upload do descritor de módulo XML, script da CLI JBoss, script de inicialização e dependência .jar do JDBC.

Estão disponíveis mais informações sobre como configurar o Wildfly com [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource) e [Banco de Dados SQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898). É possível usar essas instruções personalizadas juntamente com a abordagem generalizada acima para adicionar definições de fonte de dados ao seu servidor.

### <a name="enable-messaging-providers"></a>Permitir que os provedores de sistema de mensagens

Para habilitar Beans controlado por mensagens usando o Barramento de Serviço como o mecanismo de mensagens:

1. Use a [biblioteca de mensagens do Apache QPId JMS](https://qpid.apache.org/proton/index.html). Inclua essa dependência em seu pom.xml (ou outro arquivo de build) para o aplicativo.

2. Crie os [recursos do Barramento de Serviço](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Crie um namespace do Barramento de Serviço do Azure, uma fila dentro desse namespace e uma Política de acesso compartilhada com funcionalidades de envio e recebimento.

3. Passe a chave da política de acesso compartilhada para seu código por meio da codificação de URL da chave primária de sua política ou [Use o SDK do Barramento de Serviço](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. Siga as etapas descritas na seção Instalando Módulos e Dependências com seu descritor XML do módulo, dependências .jar, comandos da CLI JBoss e script de inicialização do provedor JMS. Além dos quatro arquivos, também será necessário criar um arquivo XML que define o nome JNDI para a fila JMS e para o tópico. Confira [este repositório](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig) para arquivos de configuração de referência.

### <a name="configure-session-management-caching"></a>Configurar o cache de gerenciamento de sessão

Por padrão, o Serviço de Aplicativo no Linux usará cookies de afinidade de sessão para garantir que as solicitações de cliente com sessões existentes sejam roteadas para a mesma instância do seu aplicativo. Esse comportamento padrão não requer nenhuma configuração, mas tem algumas limitações:

- Se uma instância do aplicativo for reiniciada ou reduzida verticalmente, o estado de sessão do usuário no servidor de aplicativos será perdido.
- Se os aplicativos tiverem configurações de tempo limite de sessão longo ou um número fixo de usuários, poderá levar algum tempo para que novas instâncias com dimensionamento automático recebam carga, uma vez que apenas novas sessões serão roteadas a instâncias recém-iniciadas.

É possível configurar o Wildfly para usar um repositório de sessão externa como o [Cache Redis do Azure](/azure/azure-cache-for-redis/). Será necessário [desabilitar a configuração de afinidade de instância do ARR existente](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) para desligar o roteamento baseado em cookie de sessão e permitir que o armazenamento de sessão do Wildfly configurado opere sem interferência.

## <a name="docker-containers"></a>Contêineres do Docker

Para usar o JDK do Zulu com suporte do Azure em seus contêineres, efetue pull e use as imagens predefinidas conforme documentado em [página de download do Azul Zulu Enterprise para Azure com suporte](https://www.azul.com/downloads/azure-only/zulu/) ou use os exemplos do `Dockerfile` dos [Repositório GitHub do Microsoft Java](https://github.com/Microsoft/java/tree/master/docker).

## <a name="statement-of-support"></a>Declaração de suporte

### <a name="runtime-availability"></a>Disponibilidade de tempo de execução

O Serviço de Aplicativo para Linux dá suporte a dois tempos de execução para hospedagem gerenciada de aplicativos Web do Java:

- O [contêiner do servlet do Tomcat](https://tomcat.apache.org/) para execução de aplicativos empacotados como arquivos Web (WAR). As versões com suporte são 8.5 e 9.0.
- O ambiente de tempo de execução do Java SE para executar aplicativos empacotados como arquivos Java (JAR). Versões com suporte são Java 8 e 11.

### <a name="jdk-versions-and-maintenance"></a>Versões e manutenção do JDK

O JDK (Java Development Kit) com suporte do Azure é o [Zulu](https://www.azul.com/downloads/azure-only/zulu/) fornecido pela [Azul Systems](https://www.azul.com/).

Atualizações de versão principal serão fornecidas por meio de novas opções de tempo de execução no Serviço de Aplicativo do Azure para Linux. Os clientes podem atualizar para essas versões mais recentes do Java configurando a implantação do Serviço de Aplicativo. Além disso, são responsáveis por testar e garantir que a atualização da versão principal atenda às suas necessidades.

Os JDKs com suporte são corrigidos automaticamente a cada trimestre em janeiro, abril, julho e outubro de cada ano.

### <a name="security-updates"></a>Atualizações de segurança

Os patches e as correções para grandes vulnerabilidades de segurança serão liberados assim que forem disponibilizados pela Azul Systems. Uma "grande" vulnerabilidade é definida por uma pontuação básica de 9,0 ou mais na [versão 2 do NIST Common Vulnerability Scoring System](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Reprovação e desativação

Se um tempo de execução do Java com suporte for desativado, os desenvolvedores do Azure que usam o tempo de execução afetado receberão um aviso de reprovação pelo menos seis meses antes de o tempo de execução ser desativado.

### <a name="local-development"></a>Desenvolvimento local

Os desenvolvedores podem baixar a Production Edition do Azul Zulu Enterprise JDK para desenvolvimento local do [site de download da Azul](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Suporte ao desenvolvimento

O suporte ao produto do [Azul Zulu JDK com suporte do Azure](https://www.azul.com/downloads/azure-only/zulu/) está disponível durante o desenvolvimento para Azure ou [Microsoft Azure Stack](https://azure.microsoft.com/overview/azure-stack/) com um [plano de suporte qualificado do Azure](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Suporte de tempo de execução

Os desenvolvedores podem [abrir um problema](/azure/azure-supportability/how-to-create-azure-support-request) com o Azul Zulu JDK por meio do Suporte do Azure se tiverem um [plano de suporte qualificado](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Próximas etapas

Acesse a central para [Desenvolvedores do Azure para Java](/java/azure/) para conferir inícios rápidos, tutoriais e documentação de referência do Java.

Dúvidas gerais sobre como usar o Serviço de Aplicativo para Linux que não são específicas do desenvolvimento em Java são respondidas nas [Perguntas frequentes do Serviço de Aplicativo no Linux](app-service-linux-faq.md).


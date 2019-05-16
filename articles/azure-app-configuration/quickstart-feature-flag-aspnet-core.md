---
title: Início Rápido para adição de sinalizadores de recurso ao ASP.NET Core | Microsoft Docs
description: Um Início Rápido para adição de sinalizadores de recurso a aplicativos ASP.NET Core e seu gerenciamento na Configuração de Aplicativos do Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 95f702b1d85dc8fe22b1800df3f7b0ebc987bee5
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411832"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Início Rápido: Adicionar sinalizadores de recurso a um aplicativo ASP.NET Core

O gerenciamento de recursos no ASP.NET Core pode ser habilitado pela conexão de seu aplicativo à Configuração de Aplicativos do Azure. Use esse serviço gerenciado para armazenar todos os sinalizadores de recurso e controlar seus estados centralmente. Este Início Rápido mostra como incorporar o serviço em um aplicativo Web ASP.NET Core para criar uma implementação de ponta a ponta do gerenciamento de recursos.

As bibliotecas do Gerenciamento de Recursos do .NET Core estendem a estrutura com suporte abrangente para sinalizadores de recursos. Elas se baseiam no sistema de configuração do .NET Core. Elas são integradas diretamente à Configuração de Aplicativos por meio de seu provedor de configuração do .NET Core.

Você pode usar qualquer editor de código para executar as etapas deste início rápido. O [Visual Studio Code](https://code.visualstudio.com/) é uma excelente opção disponível nas plataformas Windows, macOS e Linux.

## <a name="prerequisites"></a>Pré-requisitos

Para fazer este início rápido, instale o [SDK do .NET Core](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Criar um repositório de configurações de aplicativo

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Selecione **Gerenciador de Recursos** > **+ Criar** para adicionar os seguintes sinalizadores de recursos:

    | Chave | Estado |
    |---|---|
    | Beta | Desativar |

## <a name="create-an-aspnet-core-web-app"></a>Criar um aplicativo Web ASP.NET Core

Você usa a [CLI (interface de linha de comando) do .NET Core](https://docs.microsoft.com/dotnet/core/tools/) para criar um novo projeto de aplicativo Web do MVC do ASP.NET Core. A vantagem de usar a CLI do .NET Core em relação ao Visual Studio é que ela está disponível nas plataformas Windows, MacOS e Linux.

1. Crie uma nova pasta para o seu projeto. Para este Início Rápido, nomeie-o *TestFeatureFlags*.

2. Na nova pasta, execute o seguinte comando para criar um novo projeto de aplicativo Web do MVC do ASP.NET Core:

        dotnet new mvc

## <a name="add-secret-manager"></a>Adicionar o Gerenciador de Segredos

Adicione a [ferramenta Gerenciador de Segredos](https://docs.microsoft.com/aspnet/core/security/app-secrets) ao projeto. A ferramenta Gerenciador de Segredos armazena dados confidenciais para o trabalho de desenvolvimento fora da árvore do seu projeto. Essa abordagem ajuda a impedir o compartilhamento acidental de segredos do aplicativo no código-fonte.

- Abra o arquivo *.csproj*. Adicione um elemento `UserSecretsId`, conforme mostrado aqui e substitua o valor pelo seu próprio, que geralmente é um GUID. Salve o arquivo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.App" />
        <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
    </ItemGroup>

    </Project>
    ```

## <a name="connect-to-an-app-configuration-store"></a>Conectar um repositório de configurações de aplicativo

1. Adicione referências aos pacotes NuGet `Microsoft.Extensions.Configuration.AzureAppConfiguration` e `Microsoft.FeatureManagement` executando os seguintes comandos:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration --version 1.0.0-preview-008520001

        dotnet add package Microsoft.FeatureManagement.AspNetCore --version 1.0.0-preview-008560001-910

2. Execute o seguinte comando para restaurar pacotes do projeto:

        dotnet restore

3. Adicione um segredo chamado *ConnectionStrings:AppConfig* ao Gerenciador de Segredos.

    Esse segredo contém a cadeia de conexão para acessar o repositório de configurações de aplicativo. Substitua o valor no seguinte comando pela cadeia de conexão do repositório de configurações de aplicativo.

    Este comando deve ser executado no mesmo diretório que o arquivo *.csproj*.

        dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>

    O Gerenciador de Segredos é usado apenas para testar o aplicativo Web localmente. Quando o aplicativo for implantado no [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/web), por exemplo, você usará uma configuração de aplicativo **Cadeias de Conexão** no Serviço de Aplicativo, em vez de armazenar a cadeia de conexão no Gerenciador de Segredos.

    Esse segredo é acessado com a API de configuração. Um sinal de dois pontos (:) funciona no nome da configuração com a API de configuração em todas as plataformas com suporte. Consulte [Configuração por ambiente](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0).

4. Abra *Program.cs* e adicione uma referência ao provedor da Configuração de Aplicativos do .NET Core.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

5. Atualize o método `CreateWebHostBuilder` para usar a Configuração de Aplicativos chamando o método `config.AddAzureAppConfiguration()`.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options => {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                           .UseFeatureFlags();
                });
            })
            .UseStartup<Startup>();
    ```

6. Abra *Startup.cs* e adicione referências ao gerenciador de recursos do .NET Core.

    ```csharp
    using Microsoft.FeatureManagement.AspNetCore;
    ```

7. Atualize o método `ConfigureServices` para adicionar o suporte a sinalizadores de recurso chamando o método `services.AddFeatureManagement()` e, opcionalmente, inclua os filtros a serem usados com sinalizadores de recurso chamando `services.AddFeatureFilter<FilterType>()`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
    ```

8. Adicione um arquivo *MyFeatureFlags.cs*.

    ```csharp
    namespace TestFeatureFlags
    {
        public enum MyFeatureFlags
        {
            Beta
        }
    }
    ```

9. Adicione *BetaController.cs* ao diretório Controllers:

    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.FeatureManagement.AspNetCore;

    namespace TestFeatureFlags.Controllers
    {
        public class BetaController: Controller
        {
            private readonly IFeatureManager _featureManager;

            public BetaController(IFeatureManagerSnapshot featureManager)
            {
                _featureManager = featureManager;
            }

            [Feature(MyFeatureFlags.Beta)]
            public IActionResult Index()
            {
                return View();
            }
        }
    }
    ```

10. Abra *_ViewImports.cshtml* no diretório Views e adicione o auxiliar de marca do gerenciador de recursos:

    ```html
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

11. Abra *_Layout.cshtml* no diretório Exibições > Compartilhado e substitua a barra `<nav>` em `<body>` > `<header>` pelo seguinte código:

    ```html
    <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
        <div class="container">
            <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">TestFeatureFlags</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".navbar-collapse" aria-controls="navbarSupportedContent"
            aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>
            <div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">
                <ul class="navbar-nav flex-grow-1">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
                    </li>
                    <feature name="Beta">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Beta" asp-action="Index">Beta</a>
                    </li>
                    </feature>
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    ```

12. Crie um diretório Beta em Views e adicione *Index.cshtml* a ele:

    ```html
    @{
        ViewData["Title"] = "Beta Home Page";
    }

    <h1>
        This is the beta website.
    </h1>
    ```

## <a name="build-and-run-the-app-locally"></a>Compilar e executar o aplicativo localmente

1. Para criar o aplicativo usando a CLI do .NET Core, execute o seguinte comando no shell de comando:

        dotnet build

2. Depois que a construção for concluída com êxito, execute o seguinte comando para executar o aplicativo Web localmente:

        dotnet run

3. Abra uma janela do navegador e vá para `https://localhost:5001`, que é a URL padrão do aplicativo Web hospedado localmente.

    ![Inicialização local do aplicativo do Início Rápido](./media/quickstarts/aspnet-core-feature-flag-local-before.png)

4. Entre no [Portal do Azure](https://aka.ms/azconfig/portal). Selecione **Todos os recursos** e selecione a instância do repositório de configurações do aplicativo que você criou no início rápido.

5. Selecione **Gerenciador de Recursos** e altere o valor de *Beta* para *Ativado*:

    | Chave | Estado |
    |---|---|
    | Beta | Por |

6. Atualize a página do navegador para ver as novas definições de configuração.

    ![Inicialização local do aplicativo do Início Rápido](./media/quickstarts/aspnet-core-feature-flag-local-after.png)

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Próximas etapas

Neste Início Rápido, você criou um repositório de configurações de aplicativos e o usou para gerenciar recursos em um aplicativo Web ASP.NET Core por meio das [bibliotecas do Gerenciamento de Recursos](https://go.microsoft.com/fwlink/?linkid=2074664).

* Saiba mais sobre o [gerenciamento de recursos](./concept-feature-management.md)
* [Gerenciar sinalizadores de recursos](./manage-feature-flags.md)
* [Usar sinalizadores de recurso em um aplicativo ASP.NET Core](./use-feature-flags-dotnet-core.md)

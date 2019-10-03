<!-- markdownlint-disable MD002 MD041 -->

首先，使用[.Net CORE CLI](/dotnet/core/tools/?tabs=netcore2x)创建一个新的 .net core 控制台项目。

1. 在要创建项目的目录中打开命令行界面（CLI）。 运行以下命令。

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. 创建项目后，通过将当前目录更改为**GraphTutorial**目录并在 CLI 中运行以下命令来验证它是否正常工作。

    ```Shell
    dotnet run
    ```

    如果它正常运行，则应用应`Hello World!`输出。

## <a name="install-dependencies"></a>安装依赖项

在继续之前，请添加一些以后将使用的其他依赖项。

- 用于从 JSON 文件中读取应用程序配置的[Microsoft Extensions 配置](https://github.com/aspnet/Extensions)。
- [用于 .net 的 Microsoft 身份验证库（MSAL）](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)对用户进行身份验证并获取访问令牌。
- [Microsoft graph .Net 客户端库](https://github.com/microsoftgraph/msgraph-sdk-dotnet)，以调用 microsoft graph。
- [Microsoft graph .NET SDK 的身份验证提供程序](https://github.com/microsoftgraph/msgraph-sdk-dotnet-auth)，以使 Graph 客户端库能够在进行 API 调用时自动请求令牌。

在 CLI 中运行以下命令来安装依赖项。

```Shell
dotnet add package Microsoft.Extensions.Configuration --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.FileExtensions --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.Json --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.Binder --version 2.2.0
dotnet add package Microsoft.Identity.Client --version 4.3.1
dotnet add package Microsoft.Graph --version 1.17.0
```

## <a name="design-the-app"></a>设计应用程序

在本节中，您将创建一个简单的基于控制台的菜单。

在文本编辑器（如[Visual Studio Code](https://code.visualstudio.com/)）中打开**Program.cs** ，并将其全部内容替换为以下代码。

[!code-csharp[](../demos/01-create-app/GraphTutorial/Program.cs)]

这将实现一个基本菜单，并从命令行读取用户的选择。

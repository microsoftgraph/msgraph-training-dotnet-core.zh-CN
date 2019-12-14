<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5ced7-101">首先，使用[.Net CORE CLI](/dotnet/core/tools/?tabs=netcore2x)创建一个新的 .net core 控制台项目。</span><span class="sxs-lookup"><span data-stu-id="5ced7-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/?tabs=netcore2x).</span></span>

1. <span data-ttu-id="5ced7-102">在要创建项目的目录中打开命令行界面（CLI）。</span><span class="sxs-lookup"><span data-stu-id="5ced7-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="5ced7-103">运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="5ced7-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="5ced7-104">创建项目后，通过将当前目录更改为**GraphTutorial**目录并在 CLI 中运行以下命令来验证它是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="5ced7-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="5ced7-105">如果它正常运行，则应用应`Hello World!`输出。</span><span class="sxs-lookup"><span data-stu-id="5ced7-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="5ced7-106">安装依赖项</span><span class="sxs-lookup"><span data-stu-id="5ced7-106">Install dependencies</span></span>

<span data-ttu-id="5ced7-107">在继续之前，请添加一些以后将使用的其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="5ced7-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="5ced7-108">用于从 JSON 文件中读取应用程序配置的[Microsoft Extensions 配置](https://github.com/aspnet/Extensions)。</span><span class="sxs-lookup"><span data-stu-id="5ced7-108">[Microsoft.Extensions.Configuration](https://github.com/aspnet/Extensions) to read application configuration from a JSON file.</span></span>
- <span data-ttu-id="5ced7-109">[用于 .net 的 Microsoft 身份验证库（MSAL）](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)对用户进行身份验证并获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="5ced7-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="5ced7-110">[Microsoft graph .Net 客户端库](https://github.com/microsoftgraph/msgraph-sdk-dotnet)，以调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="5ced7-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="5ced7-111">[Microsoft graph .NET SDK 的身份验证提供程序](https://github.com/microsoftgraph/msgraph-sdk-dotnet-auth)，以使 Graph 客户端库能够在进行 API 调用时自动请求令牌。</span><span class="sxs-lookup"><span data-stu-id="5ced7-111">[Authentication Providers for Microsoft Graph .NET SDK](https://github.com/microsoftgraph/msgraph-sdk-dotnet-auth) to enable the Graph client library to request tokens automatically when making API calls.</span></span>

<span data-ttu-id="5ced7-112">在 CLI 中运行以下命令来安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="5ced7-112">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration --version 3.1.0
dotnet add package Microsoft.Extensions.Configuration.FileExtensions --version 3.1.0
dotnet add package Microsoft.Extensions.Configuration.Json --version 3.1.0
dotnet add package Microsoft.Extensions.Configuration.Binder --version 3.1.0
dotnet add package Microsoft.Identity.Client --version 4.7.1
dotnet add package Microsoft.Graph --version 1.21.0
```

## <a name="design-the-app"></a><span data-ttu-id="5ced7-113">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="5ced7-113">Design the app</span></span>

<span data-ttu-id="5ced7-114">在本节中，您将创建一个简单的基于控制台的菜单。</span><span class="sxs-lookup"><span data-stu-id="5ced7-114">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="5ced7-115">在文本编辑器（如[Visual Studio Code](https://code.visualstudio.com/)）中打开**Program.cs** ，并将其全部内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="5ced7-115">Open **Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

[!code-csharp[](../demos/01-create-app/GraphTutorial/Program.cs)]

<span data-ttu-id="5ced7-116">这将实现一个基本菜单，并从命令行读取用户的选择。</span><span class="sxs-lookup"><span data-stu-id="5ced7-116">This implements a basic menu and reads the user's choice from the command line.</span></span>

<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="88fd5-101">首先，使用[.Net CORE CLI](/dotnet/core/tools/)创建一个新的 .net core 控制台项目。</span><span class="sxs-lookup"><span data-stu-id="88fd5-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/).</span></span>

1. <span data-ttu-id="88fd5-102">在要创建项目的目录中打开命令行界面（CLI）。</span><span class="sxs-lookup"><span data-stu-id="88fd5-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="88fd5-103">运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="88fd5-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="88fd5-104">创建项目后，通过将当前目录更改为**GraphTutorial**目录并在 CLI 中运行以下命令来验证它是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="88fd5-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="88fd5-105">如果它正常运行，则应用应`Hello World!`输出。</span><span class="sxs-lookup"><span data-stu-id="88fd5-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="88fd5-106">安装依赖项</span><span class="sxs-lookup"><span data-stu-id="88fd5-106">Install dependencies</span></span>

<span data-ttu-id="88fd5-107">在继续之前，请添加一些以后将使用的其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="88fd5-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="88fd5-108">[UserSecrets](https://github.com/aspnet/extensions)要从[.net 开发密钥存储](https://docs.microsoft.com/aspnet/core/security/app-secrets)读取应用程序配置的扩展名为。</span><span class="sxs-lookup"><span data-stu-id="88fd5-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="88fd5-109">[用于 .net 的 Microsoft 身份验证库（MSAL）](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)对用户进行身份验证并获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="88fd5-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="88fd5-110">[Microsoft graph .Net 客户端库](https://github.com/microsoftgraph/msgraph-sdk-dotnet)，以调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="88fd5-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>

<span data-ttu-id="88fd5-111">在 CLI 中运行以下命令来安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="88fd5-111">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 3.1.2
dotnet add package Microsoft.Identity.Client --version 4.10.0
dotnet add package Microsoft.Graph --version 3.0.1
```

## <a name="design-the-app"></a><span data-ttu-id="88fd5-112">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="88fd5-112">Design the app</span></span>

<span data-ttu-id="88fd5-113">在本节中，您将创建一个简单的基于控制台的菜单。</span><span class="sxs-lookup"><span data-stu-id="88fd5-113">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="88fd5-114">在文本编辑器（如[Visual Studio Code](https://code.visualstudio.com/)）中打开**Program.cs** ，并将其全部内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="88fd5-114">Open **Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

```csharp
using Microsoft.Extensions.Configuration;
using System;

namespace GraphTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(".NET Core Graph Tutorial\n");

            int choice = -1;

            while (choice != 0) {
                Console.WriteLine("Please choose one of the following options:");
                Console.WriteLine("0. Exit");
                Console.WriteLine("1. Display access token");
                Console.WriteLine("2. List calendar events");

                try
                {
                    choice = int.Parse(Console.ReadLine());
                }
                catch (System.FormatException)
                {
                    // Set to invalid value
                    choice = -1;
                }

                switch(choice)
                {
                    case 0:
                        // Exit the program
                        Console.WriteLine("Goodbye...");
                        break;
                    case 1:
                        // Display access token
                        break;
                    case 2:
                        // List the calendar
                        break;
                    default:
                        Console.WriteLine("Invalid choice! Please try again.");
                        break;
                }
            }
        }
    }
}
```

<span data-ttu-id="88fd5-115">这将实现一个基本菜单，并从命令行读取用户的选择。</span><span class="sxs-lookup"><span data-stu-id="88fd5-115">This implements a basic menu and reads the user's choice from the command line.</span></span>

<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2e2fd-101">首先，使用 [.Net CORE CLI](/dotnet/core/tools/)创建一个新的 .net core 控制台项目。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/).</span></span>

1. <span data-ttu-id="2e2fd-102">在要创建项目的目录中打开命令行界面 (CLI) 。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="2e2fd-103">运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="2e2fd-104">创建项目后，通过将当前目录更改为 **GraphTutorial** 目录并在 CLI 中运行以下命令来验证它是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="2e2fd-105">如果它正常运行，则应用应输出 `Hello World!` 。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="2e2fd-106">安装依赖项</span><span class="sxs-lookup"><span data-stu-id="2e2fd-106">Install dependencies</span></span>

<span data-ttu-id="2e2fd-107">在继续之前，请添加一些以后将使用的其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="2e2fd-108">[Microsoft.Extensions.Configuration。UserSecrets](https://github.com/aspnet/extensions) 从 [.net 开发密钥存储](https://docs.microsoft.com/aspnet/core/security/app-secrets)读取应用程序配置。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="2e2fd-109">[Microsoft 身份验证库 (MSAL) for .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) 对用户进行身份验证并获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="2e2fd-110">[Microsoft graph .Net 客户端库](https://github.com/microsoftgraph/msgraph-sdk-dotnet) ，以调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="2e2fd-111">用于将 Windows 时区标识符转换为 IANA 标识符的[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) 。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-111">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

<span data-ttu-id="2e2fd-112">在 CLI 中运行以下命令来安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-112">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 3.1.8
dotnet add package Microsoft.Identity.Client --version 4.19.0
dotnet add package Microsoft.Graph --version 3.15.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a><span data-ttu-id="2e2fd-113">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="2e2fd-113">Design the app</span></span>

<span data-ttu-id="2e2fd-114">在本节中，您将创建一个简单的基于控制台的菜单。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-114">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="2e2fd-115">在文本编辑器中打开 **./Program.cs** (如 [Visual Studio Code](https://code.visualstudio.com/)) 并将其全部内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-115">Open **./Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

```csharp
using Microsoft.Extensions.Configuration;
using System;
using System.Collections.Generic;

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
                Console.WriteLine("2. View this week's calendar");
                Console.WriteLine("3. Add an event");

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
                    case 3:
                        // Create a new event
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

<span data-ttu-id="2e2fd-116">这将实现一个基本菜单，并从命令行读取用户的选择。</span><span class="sxs-lookup"><span data-stu-id="2e2fd-116">This implements a basic menu and reads the user's choice from the command line.</span></span>

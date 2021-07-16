<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7dbbb-101">首先，使用 .NET Core CLI 创建新的 [.NET Core](/dotnet/core/tools/)控制台项目。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/).</span></span>

1. <span data-ttu-id="7dbbb-102">在要创建项目的 (CLI) 打开命令行接口。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="7dbbb-103">运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="7dbbb-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="7dbbb-104">创建项目后，验证其是否正常工作，方法为将当前目录更改到 **GraphTu一l** 目录，并运行 CLI 中的以下命令。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="7dbbb-105">如果运行正常，应用应输出 `Hello World!` 。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="7dbbb-106">安装依赖项</span><span class="sxs-lookup"><span data-stu-id="7dbbb-106">Install dependencies</span></span>

<span data-ttu-id="7dbbb-107">在继续之前，添加一些你稍后将使用的附加依赖项。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="7dbbb-108">[Microsoft.Extensions.Configuration。UserSecrets，](https://github.com/aspnet/extensions) 用于读取 .NET 开发密码存储 [中的应用程序配置](https://docs.microsoft.com/aspnet/core/security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="7dbbb-109">[Azure Identity 的 Azure SDK](https://github.com/Azure/azure-sdk-for-net) 客户端库，用于对用户进行身份验证并获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-109">[Azure SDK Client Library for Azure Identity](https://github.com/Azure/azure-sdk-for-net) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="7dbbb-110">[Microsoft Graph .NET 客户端](https://github.com/microsoftgraph/msgraph-sdk-dotnet)库来调用 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="7dbbb-111">[TimeZoneConverter，](https://github.com/mj1856/TimeZoneConverter)用于Windows时区标识符转换为 IANA 标识符。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-111">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

<span data-ttu-id="7dbbb-112">在 CLI 中运行以下命令以安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-112">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 5.0.0
dotnet add package Azure.Identity --version 1.4.0
dotnet add package Microsoft.Graph --version 4.0.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a><span data-ttu-id="7dbbb-113">设计应用</span><span class="sxs-lookup"><span data-stu-id="7dbbb-113">Design the app</span></span>

<span data-ttu-id="7dbbb-114">在此部分中，你将创建一个简单的基于控制台的菜单。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-114">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="7dbbb-115">在 **文本编辑器中打开 ./Program.cs** (如 Visual Studio Code [](https://code.visualstudio.com/)) ，并将其全部内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-115">Open **./Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Microsoft.Extensions.Configuration;

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

<span data-ttu-id="7dbbb-116">这将实现一个基本菜单，并读取命令行中的用户选择。</span><span class="sxs-lookup"><span data-stu-id="7dbbb-116">This implements a basic menu and reads the user's choice from the command line.</span></span>

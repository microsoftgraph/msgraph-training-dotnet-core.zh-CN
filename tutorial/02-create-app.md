<!-- markdownlint-disable MD002 MD041 -->

首先，使用 .NET Core CLI 创建新的 [.NET Core](/dotnet/core/tools/)控制台项目。

1. 在要创建项目的 (CLI) 打开命令行接口。 运行以下命令：

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. 创建项目后，验证其是否正常工作，方法为将当前目录更改到 **GraphTu一l** 目录，并运行 CLI 中的以下命令。

    ```Shell
    dotnet run
    ```

    如果运行正常，应用应输出 `Hello World!` 。

## <a name="install-dependencies"></a>安装依赖项

在继续之前，添加一些你稍后将使用的附加依赖项。

- [Microsoft.Extensions.Configuration。UserSecrets，](https://github.com/aspnet/extensions) 用于读取 .NET 开发密码存储 [中的应用程序配置](https://docs.microsoft.com/aspnet/core/security/app-secrets)。
- [Azure Identity 的 Azure SDK](https://github.com/Azure/azure-sdk-for-net) 客户端库，用于对用户进行身份验证并获取访问令牌。
- [Microsoft Graph .NET 客户端](https://github.com/microsoftgraph/msgraph-sdk-dotnet)库来调用 Microsoft Graph。
- [TimeZoneConverter，](https://github.com/mj1856/TimeZoneConverter)用于Windows时区标识符转换为 IANA 标识符。

在 CLI 中运行以下命令以安装依赖项。

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 5.0.0
dotnet add package Azure.Identity --version 1.4.0
dotnet add package Microsoft.Graph --version 4.0.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a>设计应用

在此部分中，你将创建一个简单的基于控制台的菜单。

在 **文本编辑器中打开 ./Program.cs** (如 Visual Studio Code [](https://code.visualstudio.com/)) ，并将其全部内容替换为以下代码。

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

这将实现一个基本菜单，并读取命令行中的用户选择。

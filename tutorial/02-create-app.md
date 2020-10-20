<!-- markdownlint-disable MD002 MD041 -->

首先，使用 [.Net CORE CLI](/dotnet/core/tools/)创建一个新的 .net core 控制台项目。

1. 在要创建项目的目录中打开命令行界面 (CLI) 。 运行以下命令。

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. 创建项目后，通过将当前目录更改为 **GraphTutorial** 目录并在 CLI 中运行以下命令来验证它是否正常工作。

    ```Shell
    dotnet run
    ```

    如果它正常运行，则应用应输出 `Hello World!` 。

## <a name="install-dependencies"></a>安装依赖项

在继续之前，请添加一些以后将使用的其他依赖项。

- [Microsoft.Extensions.Configuration。UserSecrets](https://github.com/aspnet/extensions) 从 [.net 开发密钥存储](https://docs.microsoft.com/aspnet/core/security/app-secrets)读取应用程序配置。
- [Microsoft 身份验证库 (MSAL) for .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) 对用户进行身份验证并获取访问令牌。
- [Microsoft graph .Net 客户端库](https://github.com/microsoftgraph/msgraph-sdk-dotnet) ，以调用 microsoft graph。
- 用于将 Windows 时区标识符转换为 IANA 标识符的[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) 。

在 CLI 中运行以下命令来安装依赖项。

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 3.1.8
dotnet add package Microsoft.Identity.Client --version 4.19.0
dotnet add package Microsoft.Graph --version 3.15.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a>设计应用程序

在本节中，您将创建一个简单的基于控制台的菜单。

在文本编辑器中打开 **./Program.cs** (如 [Visual Studio Code](https://code.visualstudio.com/)) 并将其全部内容替换为以下代码。

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

这将实现一个基本菜单，并从命令行读取用户的选择。

<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将从上一练习中扩展应用程序，以支持使用 Azure AD 进行身份验证。 这是必需的，才能获取必要的 OAuth 访问令牌来调用 Microsoft Graph。 在此步骤中，将 [Microsoft 身份验证库 (MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)) .NET 集成到应用程序中。

1. 在包含 **GraphTu一l.csproj** 的目录中打开 CLI 并运行以下命令，初始化 [.NET](/aspnet/core/security/app-secrets)开发密码存储。

    ```Shell
    dotnet user-secrets init
    ```

1. 使用下列命令将应用程序 ID 和所需范围列表添加到密码存储。 将 `YOUR_APP_ID_HERE` 替换为你在 Azure 门户中创建的应用程序 ID。

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    让我们看一下刚设置的权限范围。

    - **User.Read** 将允许应用读取已登录用户的配置文件，以便获取用户显示名称和电子邮件地址等信息。
    - **MailboxSettings.Read** 将允许应用读取用户的首选时区、日期格式和时间格式。
    - **Calendars.ReadWrite** 将允许应用读取用户日历上的现有事件并添加新事件。

## <a name="implement-sign-in"></a>实施登录

在此部分中，你将使用 `DeviceCodeCredential` 类通过设备代码流 请求访问 [令牌](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)。

1. 在 **GraphTu一** l 目录中新建一个名为 Graph **的目录**。
1. 在名为 **GraphHelper.cs** Graph新文件，并添加以下代码至该文件。 

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Azure.Core;
    using Azure.Identity;
    using Microsoft.Graph;
    using TimeZoneConverter;

    namespace GraphTutorial
    {
        public class GraphHelper
        {
            private static DeviceCodeCredential tokenCredential;
            private static GraphServiceClient graphClient;

            public static void Initialize(string clientId,
                                          string[] scopes,
                                          Func<DeviceCodeInfo, CancellationToken, Task> callBack)
            {
                tokenCredential = new DeviceCodeCredential(callBack, clientId);
                graphClient = new GraphServiceClient(tokenCredential, scopes);
            }

            public static async Task<string> GetAccessTokenAsync(string[] scopes)
            {
                var context = new TokenRequestContext(scopes);
                var response = await tokenCredential.GetTokenAsync(context);
                return response.Token;
            }
        }
    }
    ```

1. 将以下函数添加到 `Program` 类。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. 将以下代码添加到 `Main` 紧接在 行后的 `Console.WriteLine(".NET Core Graph Tutorial\n");` 函数中。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. 将以下代码添加到 `Main` 紧接在 行后的 `// Display access token` 函数中。

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. 生成并运行应用。 应用程序显示 URL 和设备代码。

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > 如果遇到错误，请将 **Program.cs** 与 GitHub [进行比较](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs)。

1. 打开浏览器并浏览到显示的 URL。 输入提供的代码并登录。 完成后，返回到应用程序并选择 **1。显示访问** 令牌选项以显示访问令牌。

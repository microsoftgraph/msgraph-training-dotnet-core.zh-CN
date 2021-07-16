<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="42cd4-101">在此练习中，你将从上一练习中扩展应用程序，以支持使用 Azure AD 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="42cd4-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="42cd4-102">这是必需的，才能获取必要的 OAuth 访问令牌来调用 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="42cd4-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="42cd4-103">在此步骤中，将 [Microsoft 身份验证库 (MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)) .NET 集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="42cd4-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

1. <span data-ttu-id="42cd4-104">在包含 **GraphTu一l.csproj** 的目录中打开 CLI 并运行以下命令，初始化 [.NET](/aspnet/core/security/app-secrets)开发密码存储。</span><span class="sxs-lookup"><span data-stu-id="42cd4-104">Initialize the [.NET development secret store](/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="42cd4-105">使用下列命令将应用程序 ID 和所需范围列表添加到密码存储。</span><span class="sxs-lookup"><span data-stu-id="42cd4-105">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="42cd4-106">将 `YOUR_APP_ID_HERE` 替换为你在 Azure 门户中创建的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="42cd4-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    <span data-ttu-id="42cd4-107">让我们看一下刚设置的权限范围。</span><span class="sxs-lookup"><span data-stu-id="42cd4-107">Let's look at the permission scopes you just set.</span></span>

    - <span data-ttu-id="42cd4-108">**User.Read** 将允许应用读取已登录用户的配置文件，以便获取用户显示名称和电子邮件地址等信息。</span><span class="sxs-lookup"><span data-stu-id="42cd4-108">**User.Read** will allow the app to read the signed-in user's profile to get information such as display name and email address.</span></span>
    - <span data-ttu-id="42cd4-109">**MailboxSettings.Read** 将允许应用读取用户的首选时区、日期格式和时间格式。</span><span class="sxs-lookup"><span data-stu-id="42cd4-109">**MailboxSettings.Read** will allow the app to read the user's preferred time zone, date format, and time format.</span></span>
    - <span data-ttu-id="42cd4-110">**Calendars.ReadWrite** 将允许应用读取用户日历上的现有事件并添加新事件。</span><span class="sxs-lookup"><span data-stu-id="42cd4-110">**Calendars.ReadWrite** will allow the app to read the existing events on the user's calendar and add new events.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="42cd4-111">实施登录</span><span class="sxs-lookup"><span data-stu-id="42cd4-111">Implement sign-in</span></span>

<span data-ttu-id="42cd4-112">在此部分中，你将使用 `DeviceCodeCredential` 类通过设备代码流 请求访问 [令牌](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)。</span><span class="sxs-lookup"><span data-stu-id="42cd4-112">In this section you will use the `DeviceCodeCredential` class to request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

1. <span data-ttu-id="42cd4-113">在 **GraphTu一** l 目录中新建一个名为 Graph **的目录**。</span><span class="sxs-lookup"><span data-stu-id="42cd4-113">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="42cd4-114">在名为 **GraphHelper.cs** Graph新文件，并添加以下代码至该文件。 </span><span class="sxs-lookup"><span data-stu-id="42cd4-114">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

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

1. <span data-ttu-id="42cd4-115">将以下函数添加到 `Program` 类。</span><span class="sxs-lookup"><span data-stu-id="42cd4-115">Add the following function to the `Program` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. <span data-ttu-id="42cd4-116">将以下代码添加到 `Main` 紧接在 行后的 `Console.WriteLine(".NET Core Graph Tutorial\n");` 函数中。</span><span class="sxs-lookup"><span data-stu-id="42cd4-116">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. <span data-ttu-id="42cd4-117">将以下代码添加到 `Main` 紧接在 行后的 `// Display access token` 函数中。</span><span class="sxs-lookup"><span data-stu-id="42cd4-117">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="42cd4-118">生成并运行应用。</span><span class="sxs-lookup"><span data-stu-id="42cd4-118">Build and run the app.</span></span> <span data-ttu-id="42cd4-119">应用程序显示 URL 和设备代码。</span><span class="sxs-lookup"><span data-stu-id="42cd4-119">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="42cd4-120">如果遇到错误，请将 **Program.cs** 与 GitHub [进行比较](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs)。</span><span class="sxs-lookup"><span data-stu-id="42cd4-120">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="42cd4-121">打开浏览器并浏览到显示的 URL。</span><span class="sxs-lookup"><span data-stu-id="42cd4-121">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="42cd4-122">输入提供的代码并登录。</span><span class="sxs-lookup"><span data-stu-id="42cd4-122">Enter the provided code and sign in.</span></span> <span data-ttu-id="42cd4-123">完成后，返回到应用程序并选择 **1。显示访问** 令牌选项以显示访问令牌。</span><span class="sxs-lookup"><span data-stu-id="42cd4-123">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

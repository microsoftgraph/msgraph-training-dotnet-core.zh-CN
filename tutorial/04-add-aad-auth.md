<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="cf400-101">在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="cf400-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="cf400-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。</span><span class="sxs-lookup"><span data-stu-id="cf400-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="cf400-103">在此步骤中，将[.net 的 Microsoft 身份验证库（MSAL）](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="cf400-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

<span data-ttu-id="cf400-104">在名为**appsettings**的**GraphTutorial**目录中创建一个新文件。</span><span class="sxs-lookup"><span data-stu-id="cf400-104">Create a new file in the **GraphTutorial** directory named **appsettings.json**.</span></span> <span data-ttu-id="cf400-105">将以下文本添加到该文件中。</span><span class="sxs-lookup"><span data-stu-id="cf400-105">Add the following text in that file.</span></span>

```json
{
  "appId": "YOUR_APP_ID_HERE",
  "scopes": [
    "User.Read",
    "Calendars.Read"
  ]
}
```

<span data-ttu-id="cf400-106">将`YOUR_APP_ID_HERE`替换为你在 Azure 门户中创建的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="cf400-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cf400-107">如果您使用的是源代码管理（如 git），现在是从源代码控制中排除**appsettings**文件以避免无意中泄漏您的应用程序 ID 的最佳时机。</span><span class="sxs-lookup"><span data-stu-id="cf400-107">If you're using source control such as git, now would be a good time to exclude the **appsettings.json** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="cf400-108">实施登录</span><span class="sxs-lookup"><span data-stu-id="cf400-108">Implement sign-in</span></span>

<span data-ttu-id="cf400-109">在本节中，您将创建可与 Graph SDK 一起使用的身份验证提供程序，还可用于使用[设备代码流](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)显式请求访问令牌。</span><span class="sxs-lookup"><span data-stu-id="cf400-109">In this section you will create an authentication provider that can be used with the Graph SDK and can also be used to explicitly request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

### <a name="create-an-authentication-provider"></a><span data-ttu-id="cf400-110">创建身份验证提供程序</span><span class="sxs-lookup"><span data-stu-id="cf400-110">Create an authentication provider</span></span>

1. <span data-ttu-id="cf400-111">在名为**Authentication**的**GraphTutorial**目录中创建一个新目录。</span><span class="sxs-lookup"><span data-stu-id="cf400-111">Create a new directory in the **GraphTutorial** directory named **Authentication**.</span></span>
1. <span data-ttu-id="cf400-112">在名为 " **DeviceCodeAuthProvider.cs** " 的**身份验证**目录中创建一个新文件，并将以下代码添加到该文件中。</span><span class="sxs-lookup"><span data-stu-id="cf400-112">Create a new file in the **Authentication** directory named **DeviceCodeAuthProvider.cs** and add the following code to that file.</span></span>

    ```csharp
    using Microsoft.Graph;
    using Microsoft.Identity.Client;
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace GraphTutorial
    {
        public class DeviceCodeAuthProvider : IAuthenticationProvider
        {
            private IPublicClientApplication _msalClient;
            private string[] _scopes;
            private IAccount _userAccount;

            public DeviceCodeAuthProvider(string appId, string[] scopes)
            {
                _scopes = scopes;

                _msalClient = PublicClientApplicationBuilder
                    .Create(appId)
                    // Set the tenant ID to "organizations" to disable personal accounts
                    // Azure OAuth does not support device code flow for personal accounts
                    // See https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code
                    .WithTenantId("organizations")
                    .Build();
            }

            public async Task<string> GetAccessToken()
            {
                // If there is no saved user account, the user must sign-in
                if (_userAccount == null)
                {
                    try
                    {
                        // Invoke device code flow so user can sign-in with a browser
                        var result = await _msalClient.AcquireTokenWithDeviceCode(_scopes, callback => {
                            Console.WriteLine(callback.Message);
                            return Task.FromResult(0);
                        }).ExecuteAsync();

                        _userAccount = result.Account;
                        return result.AccessToken;
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"Error getting access token: {exception.Message}");
                        return null;
                    }
                }
                else
                {
                    // If there is an account, call AcquireTokenSilent
                    // By doing this, MSAL will refresh the token automatically if
                    // it is expired. Otherwise it returns the cached token.

                        var result = await _msalClient
                            .AcquireTokenSilent(_scopes, _userAccount)
                            .ExecuteAsync();

                       return result.AccessToken;
                }
            }

            // This is the required function to implement IAuthenticationProvider
            // The Graph SDK will call this function each time it makes a Graph
            // call.
            public async Task AuthenticateRequestAsync(HttpRequestMessage requestMessage)
            {
                requestMessage.Headers.Authorization =
                    new AuthenticationHeaderValue("bearer", await GetAccessToken());
            }
        }
    }
    ```

<span data-ttu-id="cf400-113">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="cf400-113">Consider what this code does.</span></span>

- <span data-ttu-id="cf400-114">它使用 MSAL `IPublicClientApplication`实现来请求和管理令牌。</span><span class="sxs-lookup"><span data-stu-id="cf400-114">It uses the MSAL `IPublicClientApplication` implementation to request and manage tokens.</span></span>
- <span data-ttu-id="cf400-115">`GetAccessToken`函数：</span><span class="sxs-lookup"><span data-stu-id="cf400-115">The `GetAccessToken` function:</span></span>
  - <span data-ttu-id="cf400-116">登录用户（如果他们尚未使用设备代码流登录）。</span><span class="sxs-lookup"><span data-stu-id="cf400-116">Signs in the user if they are not already signed in using the device code flow.</span></span>
  - <span data-ttu-id="cf400-117">确保返回的令牌总是使用`AcquireTokenSilent`函数（如果未过期，则返回缓存的令牌）进行全新，并在令牌过期时刷新令牌。</span><span class="sxs-lookup"><span data-stu-id="cf400-117">Ensures that the token returned is always fresh by using the `AcquireTokenSilent` function, which returns the cached token if it not expired, and refreshes the token if it is expired.</span></span>
- <span data-ttu-id="cf400-118">它实现`IAuthenticationProvider`接口，以便 graph SDK 可以使用类对 graph 调用进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="cf400-118">It implements the `IAuthenticationProvider` interface so that the Graph SDK can use the class to authenticate Graph calls.</span></span>

## <a name="sign-in-and-display-the-access-token"></a><span data-ttu-id="cf400-119">登录并显示访问令牌</span><span class="sxs-lookup"><span data-stu-id="cf400-119">Sign in and display the access token</span></span>

<span data-ttu-id="cf400-120">在本节中，您将更新应用程序以调用`GetAccessToken`该函数，这将对用户进行登录。</span><span class="sxs-lookup"><span data-stu-id="cf400-120">In this section you will update the application to call the `GetAccessToken` function, which will sign in the user.</span></span> <span data-ttu-id="cf400-121">您还将添加代码以显示令牌。</span><span class="sxs-lookup"><span data-stu-id="cf400-121">You will also add code to display the token.</span></span>

1. <span data-ttu-id="cf400-122">打开**Program.cs** ，并将以下`using`语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="cf400-122">Open **Program.cs** and add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="cf400-123">将以下函数添加到 `Program` 类。</span><span class="sxs-lookup"><span data-stu-id="cf400-123">Add the following function to the `Program` class.</span></span>

    ```csharp
    static IConfigurationRoot LoadAppSettings()
    {
        var appConfig = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json", false, true)
            .Build();

        // Check for required settings
        if (string.IsNullOrEmpty(appConfig["appId"]) ||
            // Make sure there's at least one value in the scopes array
            string.IsNullOrEmpty(appConfig["scopes:0"]))
        {
            return null;
        }

        return appConfig;
    }
    ```

1. <span data-ttu-id="cf400-124">将以下代码添加到紧跟`Main`在`Console.WriteLine(".NET Core Graph Tutorial\n");`该行后面的函数中。</span><span class="sxs-lookup"><span data-stu-id="cf400-124">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    ```csharp
    var appConfig = LoadAppSettings();

    if (appConfig == null)
    {
        Console.WriteLine("Missing or invalid appsettings.json...exiting");
        return;
    }

    var appId = appConfig["appId"];
    var scopes = appConfig.GetSection("scopes").Get<string[]>();

    // Initialize the auth provider with values from appsettings.json
    var authProvider = new DeviceCodeAuthProvider(appId, scopes);

    // Request a token to sign in the user
    var accessToken = authProvider.GetAccessToken().Result;
    ```

1. <span data-ttu-id="cf400-125">将以下代码添加到紧跟`Main`在`// Display access token`该行后面的函数中。</span><span class="sxs-lookup"><span data-stu-id="cf400-125">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="cf400-126">生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="cf400-126">Build and run the app.</span></span> <span data-ttu-id="cf400-127">应用程序将显示 URL 和设备代码。</span><span class="sxs-lookup"><span data-stu-id="cf400-127">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="cf400-128">如果遇到错误，请将您的**Program.cs**与[GitHub 上的示例](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs)进行比较。</span><span class="sxs-lookup"><span data-stu-id="cf400-128">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="cf400-129">打开浏览器并浏览到显示的 URL。</span><span class="sxs-lookup"><span data-stu-id="cf400-129">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="cf400-130">输入提供的代码并登录。</span><span class="sxs-lookup"><span data-stu-id="cf400-130">Enter the provided code and sign in.</span></span> <span data-ttu-id="cf400-131">完成后，返回到应用程序并选择 " **1"。显示**访问令牌选项以显示访问令牌。</span><span class="sxs-lookup"><span data-stu-id="cf400-131">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

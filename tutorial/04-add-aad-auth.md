<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1a38b-101">在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="1a38b-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="1a38b-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。</span><span class="sxs-lookup"><span data-stu-id="1a38b-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="1a38b-103">在此步骤中，将 [Microsoft 身份验证库 (MSAL) for .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) 集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="1a38b-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

1. <span data-ttu-id="1a38b-104">通过在包含**GraphTutorial**的目录中打开 CLI 并运行以下命令来初始化[.net 开发密钥存储](/aspnet/core/security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="1a38b-104">Initialize the [.NET development secret store](/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="1a38b-105">使用以下命令将应用程序 ID 和所需范围的列表添加到机密存储区。</span><span class="sxs-lookup"><span data-stu-id="1a38b-105">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="1a38b-106">`YOUR_APP_ID_HERE`将替换为你在 Azure 门户中创建的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="1a38b-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    <span data-ttu-id="1a38b-107">让我们看一下您刚刚设置的权限范围。</span><span class="sxs-lookup"><span data-stu-id="1a38b-107">Let's look at the permission scopes you just set.</span></span>

    - <span data-ttu-id="1a38b-108">**用户读取** 将允许应用读取登录用户的配置文件，以获取显示名称和电子邮件地址等信息。</span><span class="sxs-lookup"><span data-stu-id="1a38b-108">**User.Read** will allow the app to read the signed-in user's profile to get information such as display name and email address.</span></span>
    - <span data-ttu-id="1a38b-109">**MailboxSettings** 将允许应用读取用户的首选时区、日期格式和时间格式。</span><span class="sxs-lookup"><span data-stu-id="1a38b-109">**MailboxSettings.Read** will allow the app to read the user's preferred time zone, date format, and time format.</span></span>
    - <span data-ttu-id="1a38b-110">"**读写**" 将允许应用读取用户日历上的现有事件并添加新事件。</span><span class="sxs-lookup"><span data-stu-id="1a38b-110">**Calendars.ReadWrite** will allow the app to read the existing events on the user's calendar and add new events.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="1a38b-111">实施登录</span><span class="sxs-lookup"><span data-stu-id="1a38b-111">Implement sign-in</span></span>

<span data-ttu-id="1a38b-112">在本节中，您将创建可与 Graph SDK 一起使用的身份验证提供程序，还可用于使用 [设备代码流](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)显式请求访问令牌。</span><span class="sxs-lookup"><span data-stu-id="1a38b-112">In this section you will create an authentication provider that can be used with the Graph SDK and can also be used to explicitly request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

### <a name="create-an-authentication-provider"></a><span data-ttu-id="1a38b-113">创建身份验证提供程序</span><span class="sxs-lookup"><span data-stu-id="1a38b-113">Create an authentication provider</span></span>

1. <span data-ttu-id="1a38b-114">在名为**Authentication**的**GraphTutorial**目录中创建一个新目录。</span><span class="sxs-lookup"><span data-stu-id="1a38b-114">Create a new directory in the **GraphTutorial** directory named **Authentication**.</span></span>
1. <span data-ttu-id="1a38b-115">在名为 " **DeviceCodeAuthProvider.cs** " 的**身份验证**目录中创建一个新文件，并将以下代码添加到该文件中。</span><span class="sxs-lookup"><span data-stu-id="1a38b-115">Create a new file in the **Authentication** directory named **DeviceCodeAuthProvider.cs** and add the following code to that file.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Authentication/DeviceCodeAuthProvider.cs" id="AuthProviderSnippet":::

<span data-ttu-id="1a38b-116">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="1a38b-116">Consider what this code does.</span></span>

- <span data-ttu-id="1a38b-117">它使用 MSAL `IPublicClientApplication` 实现来请求和管理令牌。</span><span class="sxs-lookup"><span data-stu-id="1a38b-117">It uses the MSAL `IPublicClientApplication` implementation to request and manage tokens.</span></span>
- <span data-ttu-id="1a38b-118">`GetAccessToken`函数：</span><span class="sxs-lookup"><span data-stu-id="1a38b-118">The `GetAccessToken` function:</span></span>
  - <span data-ttu-id="1a38b-119">登录用户（如果他们尚未使用设备代码流登录）。</span><span class="sxs-lookup"><span data-stu-id="1a38b-119">Signs in the user if they are not already signed in using the device code flow.</span></span>
  - <span data-ttu-id="1a38b-120">确保返回的令牌总是使用 `AcquireTokenSilent` 函数（如果未过期，则返回缓存的令牌）进行全新，并在令牌过期时刷新令牌。</span><span class="sxs-lookup"><span data-stu-id="1a38b-120">Ensures that the token returned is always fresh by using the `AcquireTokenSilent` function, which returns the cached token if it not expired, and refreshes the token if it is expired.</span></span>
- <span data-ttu-id="1a38b-121">它实现 `IAuthenticationProvider` 接口，以便 GRAPH SDK 可以使用类对 graph 调用进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="1a38b-121">It implements the `IAuthenticationProvider` interface so that the Graph SDK can use the class to authenticate Graph calls.</span></span>

## <a name="sign-in-and-display-the-access-token"></a><span data-ttu-id="1a38b-122">登录并显示访问令牌</span><span class="sxs-lookup"><span data-stu-id="1a38b-122">Sign in and display the access token</span></span>

<span data-ttu-id="1a38b-123">在本节中，您将更新应用程序以调用该 `GetAccessToken` 函数，这将对用户进行登录。</span><span class="sxs-lookup"><span data-stu-id="1a38b-123">In this section you will update the application to call the `GetAccessToken` function, which will sign in the user.</span></span> <span data-ttu-id="1a38b-124">您还将添加代码以显示令牌。</span><span class="sxs-lookup"><span data-stu-id="1a38b-124">You will also add code to display the token.</span></span>

1. <span data-ttu-id="1a38b-125">将以下函数添加到 `Program` 类。</span><span class="sxs-lookup"><span data-stu-id="1a38b-125">Add the following function to the `Program` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. <span data-ttu-id="1a38b-126">将以下代码添加到 `Main` 紧跟在该行后面的函数中 `Console.WriteLine(".NET Core Graph Tutorial\n");` 。</span><span class="sxs-lookup"><span data-stu-id="1a38b-126">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. <span data-ttu-id="1a38b-127">将以下代码添加到 `Main` 紧跟在该行后面的函数中 `// Display access token` 。</span><span class="sxs-lookup"><span data-stu-id="1a38b-127">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="1a38b-128">生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="1a38b-128">Build and run the app.</span></span> <span data-ttu-id="1a38b-129">应用程序将显示 URL 和设备代码。</span><span class="sxs-lookup"><span data-stu-id="1a38b-129">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="1a38b-130">如果遇到错误，请将您的 **Program.cs** 与 [GitHub 上的示例](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs)进行比较。</span><span class="sxs-lookup"><span data-stu-id="1a38b-130">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="1a38b-131">打开浏览器并浏览到显示的 URL。</span><span class="sxs-lookup"><span data-stu-id="1a38b-131">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="1a38b-132">输入提供的代码并登录。</span><span class="sxs-lookup"><span data-stu-id="1a38b-132">Enter the provided code and sign in.</span></span> <span data-ttu-id="1a38b-133">完成后，返回到应用程序并选择 " **1"。显示** 访问令牌选项以显示访问令牌。</span><span class="sxs-lookup"><span data-stu-id="1a38b-133">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

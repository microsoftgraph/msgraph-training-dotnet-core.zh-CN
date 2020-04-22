<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="44735-101">在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="44735-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="44735-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。</span><span class="sxs-lookup"><span data-stu-id="44735-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="44735-103">在此步骤中，将[.net 的 Microsoft 身份验证库（MSAL）](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="44735-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

1. <span data-ttu-id="44735-104">通过在包含**GraphTutorial**的目录中打开 CLI 并运行以下命令来初始化[.net 开发密钥存储](/aspnet/core/security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="44735-104">Initialize the [.NET development secret store](/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="44735-105">使用以下命令将应用程序 ID 和所需范围的列表添加到机密存储区。</span><span class="sxs-lookup"><span data-stu-id="44735-105">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="44735-106">将`YOUR_APP_ID_HERE`替换为你在 Azure 门户中创建的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="44735-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="44735-107">实施登录</span><span class="sxs-lookup"><span data-stu-id="44735-107">Implement sign-in</span></span>

<span data-ttu-id="44735-108">在本节中，您将创建可与 Graph SDK 一起使用的身份验证提供程序，还可用于使用[设备代码流](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)显式请求访问令牌。</span><span class="sxs-lookup"><span data-stu-id="44735-108">In this section you will create an authentication provider that can be used with the Graph SDK and can also be used to explicitly request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

### <a name="create-an-authentication-provider"></a><span data-ttu-id="44735-109">创建身份验证提供程序</span><span class="sxs-lookup"><span data-stu-id="44735-109">Create an authentication provider</span></span>

1. <span data-ttu-id="44735-110">在名为**Authentication**的**GraphTutorial**目录中创建一个新目录。</span><span class="sxs-lookup"><span data-stu-id="44735-110">Create a new directory in the **GraphTutorial** directory named **Authentication**.</span></span>
1. <span data-ttu-id="44735-111">在名为 " **DeviceCodeAuthProvider.cs** " 的**身份验证**目录中创建一个新文件，并将以下代码添加到该文件中。</span><span class="sxs-lookup"><span data-stu-id="44735-111">Create a new file in the **Authentication** directory named **DeviceCodeAuthProvider.cs** and add the following code to that file.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Authentication/DeviceCodeAuthProvider.cs" id="AuthProviderSnippet":::

<span data-ttu-id="44735-112">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="44735-112">Consider what this code does.</span></span>

- <span data-ttu-id="44735-113">它使用 MSAL `IPublicClientApplication`实现来请求和管理令牌。</span><span class="sxs-lookup"><span data-stu-id="44735-113">It uses the MSAL `IPublicClientApplication` implementation to request and manage tokens.</span></span>
- <span data-ttu-id="44735-114">`GetAccessToken`函数：</span><span class="sxs-lookup"><span data-stu-id="44735-114">The `GetAccessToken` function:</span></span>
  - <span data-ttu-id="44735-115">登录用户（如果他们尚未使用设备代码流登录）。</span><span class="sxs-lookup"><span data-stu-id="44735-115">Signs in the user if they are not already signed in using the device code flow.</span></span>
  - <span data-ttu-id="44735-116">确保返回的令牌总是使用`AcquireTokenSilent`函数（如果未过期，则返回缓存的令牌）进行全新，并在令牌过期时刷新令牌。</span><span class="sxs-lookup"><span data-stu-id="44735-116">Ensures that the token returned is always fresh by using the `AcquireTokenSilent` function, which returns the cached token if it not expired, and refreshes the token if it is expired.</span></span>
- <span data-ttu-id="44735-117">它实现`IAuthenticationProvider`接口，以便 graph SDK 可以使用类对 graph 调用进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="44735-117">It implements the `IAuthenticationProvider` interface so that the Graph SDK can use the class to authenticate Graph calls.</span></span>

## <a name="sign-in-and-display-the-access-token"></a><span data-ttu-id="44735-118">登录并显示访问令牌</span><span class="sxs-lookup"><span data-stu-id="44735-118">Sign in and display the access token</span></span>

<span data-ttu-id="44735-119">在本节中，您将更新应用程序以调用`GetAccessToken`该函数，这将对用户进行登录。</span><span class="sxs-lookup"><span data-stu-id="44735-119">In this section you will update the application to call the `GetAccessToken` function, which will sign in the user.</span></span> <span data-ttu-id="44735-120">您还将添加代码以显示令牌。</span><span class="sxs-lookup"><span data-stu-id="44735-120">You will also add code to display the token.</span></span>

1. <span data-ttu-id="44735-121">将以下函数添加到 `Program` 类。</span><span class="sxs-lookup"><span data-stu-id="44735-121">Add the following function to the `Program` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. <span data-ttu-id="44735-122">将以下代码添加到紧跟`Main`在`Console.WriteLine(".NET Core Graph Tutorial\n");`该行后面的函数中。</span><span class="sxs-lookup"><span data-stu-id="44735-122">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. <span data-ttu-id="44735-123">将以下代码添加到紧跟`Main`在`// Display access token`该行后面的函数中。</span><span class="sxs-lookup"><span data-stu-id="44735-123">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="44735-124">生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="44735-124">Build and run the app.</span></span> <span data-ttu-id="44735-125">应用程序将显示 URL 和设备代码。</span><span class="sxs-lookup"><span data-stu-id="44735-125">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="44735-126">如果遇到错误，请将您的**Program.cs**与[GitHub 上的示例](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs)进行比较。</span><span class="sxs-lookup"><span data-stu-id="44735-126">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="44735-127">打开浏览器并浏览到显示的 URL。</span><span class="sxs-lookup"><span data-stu-id="44735-127">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="44735-128">输入提供的代码并登录。</span><span class="sxs-lookup"><span data-stu-id="44735-128">Enter the provided code and sign in.</span></span> <span data-ttu-id="44735-129">完成后，返回到应用程序并选择 " **1"。显示**访问令牌选项以显示访问令牌。</span><span class="sxs-lookup"><span data-stu-id="44735-129">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

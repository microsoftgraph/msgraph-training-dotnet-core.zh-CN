# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="82b75-101">如何运行已完成的项目</span><span class="sxs-lookup"><span data-stu-id="82b75-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82b75-102">先决条件</span><span class="sxs-lookup"><span data-stu-id="82b75-102">Prerequisites</span></span>

<span data-ttu-id="82b75-103">若要在此文件夹中运行已完成的项目，您需要：</span><span class="sxs-lookup"><span data-stu-id="82b75-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="82b75-104">[安装在开发计算机上安装的 .NET Core SDK。](https://dotnet.microsoft.com/download)</span><span class="sxs-lookup"><span data-stu-id="82b75-104">The [.NET Core SDK](https://dotnet.microsoft.com/download) installed on your development machine.</span></span> <span data-ttu-id="82b75-105"> (**注意：** 本教程是使用 .NET Core SDK 版本 3.1.402 编写的。</span><span class="sxs-lookup"><span data-stu-id="82b75-105">(**Note:** This tutorial was written with .NET Core SDK version 3.1.402.</span></span> <span data-ttu-id="82b75-106">本指南中的步骤可能与其他版本一起运行，但尚未经过测试) </span><span class="sxs-lookup"><span data-stu-id="82b75-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="82b75-107">Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="82b75-107">A Microsoft work or school account.</span></span>

<span data-ttu-id="82b75-108">如果你没有 Microsoft 帐户，你可以注册开发人员计划Office 365免费[](https://developer.microsoft.com/office/dev-program)订阅Office 365订阅。</span><span class="sxs-lookup"><span data-stu-id="82b75-108">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="82b75-109">向管理中心Azure Active Directory Web 应用程序</span><span class="sxs-lookup"><span data-stu-id="82b75-109">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="82b75-110">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。然后，使用 **个人帐户**（亦称为“Microsoft 帐户”）或 **工作或学校帐户** 登录。</span><span class="sxs-lookup"><span data-stu-id="82b75-110">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="82b75-111">选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。</span><span class="sxs-lookup"><span data-stu-id="82b75-111">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="82b75-112">应用注册屏幕截图</span><span class="sxs-lookup"><span data-stu-id="82b75-112">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="82b75-113">选择“新注册”。</span><span class="sxs-lookup"><span data-stu-id="82b75-113">Select **New registration**.</span></span> <span data-ttu-id="82b75-114">在“注册应用”页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="82b75-114">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="82b75-115">将“名称”设置为“`.NET Core Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="82b75-115">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="82b75-116">将 **支持的帐户类型** 设置为 **任何组织目录中的帐户**。</span><span class="sxs-lookup"><span data-stu-id="82b75-116">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="82b75-117">保留“重定向 URI”为空。</span><span class="sxs-lookup"><span data-stu-id="82b75-117">Leave **Redirect URI** empty.</span></span>

    ![注册应用程序页面的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="82b75-119">选择“**注册**”。</span><span class="sxs-lookup"><span data-stu-id="82b75-119">Select **Register**.</span></span> <span data-ttu-id="82b75-120">在 **".NET Core Graph 教程**"页上，复制 Application **(客户端) ID** 的值并保存它，你将在下一步中需要该值。</span><span class="sxs-lookup"><span data-stu-id="82b75-120">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="82b75-122">选择" **添加重定向 URI"** 链接。</span><span class="sxs-lookup"><span data-stu-id="82b75-122">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="82b75-123">在" **重定向 URI"** 页上，找到"建议为公共客户端重定向 **URI (移动、桌面**) 部分。</span><span class="sxs-lookup"><span data-stu-id="82b75-123">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="82b75-124">选择 `https://login.microsoftonline.com/common/oauth2/nativeclient` URI。</span><span class="sxs-lookup"><span data-stu-id="82b75-124">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![重定向 URI 页面的屏幕截图](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="82b75-126">找到"**高级设置"** 部分，将"**允许公共客户端** 流"开关更改为 **"是"，** 然后选择"保存 **"。**</span><span class="sxs-lookup"><span data-stu-id="82b75-126">Locate the **Advanced settings** section and change the **Allow public client flows** toggle to **Yes**, then choose **Save**.</span></span>

    !["默认客户端类型"部分屏幕截图](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="82b75-128">配置示例</span><span class="sxs-lookup"><span data-stu-id="82b75-128">Configure the sample</span></span>

1. <span data-ttu-id="82b75-129">在包含 **GraphTu一l.csproj** 的目录中打开 CLI 并运行以下命令，初始化 [.NET](https://docs.microsoft.com/aspnet/core/security/app-secrets)开发密码存储。</span><span class="sxs-lookup"><span data-stu-id="82b75-129">Initialize the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="82b75-130">使用下列命令将应用程序 ID 和所需范围列表添加到密码存储。</span><span class="sxs-lookup"><span data-stu-id="82b75-130">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="82b75-131">将 `YOUR_APP_ID_HERE` 替换为你在 Azure 门户中创建的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="82b75-131">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="build-and-run-the-sample"></a><span data-ttu-id="82b75-132">生成和运行示例</span><span class="sxs-lookup"><span data-stu-id="82b75-132">Build and run the sample</span></span>

<span data-ttu-id="82b75-133">在命令行界面中 (CLI) ，导航到项目目录并运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="82b75-133">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
dotnet restore
dotnet build
dotnet run
```

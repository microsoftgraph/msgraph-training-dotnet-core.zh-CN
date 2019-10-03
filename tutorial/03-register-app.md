<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0f207-101">在本练习中，将使用 Azure Active Directory 管理中心创建新的 Azure AD 应用程序。</span><span class="sxs-lookup"><span data-stu-id="0f207-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="0f207-102">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。然后，使用**个人帐户**（亦称为“Microsoft 帐户”）或**工作或学校帐户**登录。</span><span class="sxs-lookup"><span data-stu-id="0f207-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="0f207-103">在左侧导航栏中选择 " **Azure Active Directory** "，然后选择 "**管理**" 下的 "**应用程序注册**"。</span><span class="sxs-lookup"><span data-stu-id="0f207-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="0f207-104">应用注册的屏幕截图</span><span class="sxs-lookup"><span data-stu-id="0f207-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="0f207-105">选择“新注册”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="0f207-105">Select **New registration**.</span></span> <span data-ttu-id="0f207-106">在“注册应用”\*\*\*\* 页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="0f207-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="0f207-107">将“名称”\*\*\*\* 设置为“`.NET Core Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="0f207-107">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="0f207-108">将**支持的帐户类型**设置为**任何组织目录中的帐户**。</span><span class="sxs-lookup"><span data-stu-id="0f207-108">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="0f207-109">保留“重定向 URI”\*\*\*\* 为空。</span><span class="sxs-lookup"><span data-stu-id="0f207-109">Leave **Redirect URI** empty.</span></span>

    !["注册应用程序" 页的屏幕截图](./images/aad-register-an-app.png)

1. <span data-ttu-id="0f207-111">选择 "**注册**"。</span><span class="sxs-lookup"><span data-stu-id="0f207-111">Select **Register**.</span></span> <span data-ttu-id="0f207-112">在 " **.Net Core Graph 教程**" 页上，复制**应用程序（客户端） ID**的值并保存它，下一步将需要它。</span><span class="sxs-lookup"><span data-stu-id="0f207-112">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](./images/aad-application-id.png)

1. <span data-ttu-id="0f207-114">选择 "**添加重定向 URI** " 链接。</span><span class="sxs-lookup"><span data-stu-id="0f207-114">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="0f207-115">在 "**重定向 uri** " 页上，找到 "**公共客户端（移动、桌面）** " 部分的 "建议的重定向 uri" 部分。</span><span class="sxs-lookup"><span data-stu-id="0f207-115">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="0f207-116">选择`https://login.microsoftonline.com/common/oauth2/nativeclient` URI。</span><span class="sxs-lookup"><span data-stu-id="0f207-116">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    !["重定向 Uri" 页的屏幕截图](./images/aad-redirect-uris.png)

1. <span data-ttu-id="0f207-118">找到 "**默认客户端类型**" 部分，将 "将**应用程序视为公用客户端**" 切换为 **"是"**，然后选择 "**保存**"。</span><span class="sxs-lookup"><span data-stu-id="0f207-118">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![默认 "客户端类型" 部分的屏幕截图](./images/aad-default-client-type.png)
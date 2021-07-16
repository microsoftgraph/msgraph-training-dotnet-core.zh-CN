<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="57623-101">在此练习中，你将使用管理中心Azure Active Directory Azure AD 应用程序。</span><span class="sxs-lookup"><span data-stu-id="57623-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="57623-102">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。然后，使用 **个人帐户**（亦称为“Microsoft 帐户”）或 **工作或学校帐户** 登录。</span><span class="sxs-lookup"><span data-stu-id="57623-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="57623-103">选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。</span><span class="sxs-lookup"><span data-stu-id="57623-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="57623-104">应用注册屏幕截图</span><span class="sxs-lookup"><span data-stu-id="57623-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="57623-105">选择“新注册”。</span><span class="sxs-lookup"><span data-stu-id="57623-105">Select **New registration**.</span></span> <span data-ttu-id="57623-106">在“注册应用”页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="57623-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="57623-107">将“名称”设置为“`.NET Core Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="57623-107">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="57623-108">将“受支持的帐户类型”设置为“任何组织目录中的帐户和个人 Microsoft 帐户”。</span><span class="sxs-lookup"><span data-stu-id="57623-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="57623-109">在 **"重定向 URI"** 下，将下拉列表更改为"公共客户端 **(移动&桌面) "，** 将值设置为 `https://login.microsoftonline.com/common/oauth2/nativeclient` 。</span><span class="sxs-lookup"><span data-stu-id="57623-109">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>

    ![注册应用程序页面的屏幕截图](./images/aad-register-an-app.png)

1. <span data-ttu-id="57623-111">选择“**注册**”。</span><span class="sxs-lookup"><span data-stu-id="57623-111">Select **Register**.</span></span> <span data-ttu-id="57623-112">在 **".NET Core Graph 教程**"页上，复制 Application **(客户端) ID** 的值并保存它，你将在下一步中需要该值。</span><span class="sxs-lookup"><span data-stu-id="57623-112">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](./images/aad-application-id.png)

1. <span data-ttu-id="57623-114">选择“**管理**”下的“**身份验证**”。</span><span class="sxs-lookup"><span data-stu-id="57623-114">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="57623-115">找到"**高级设置"** 部分，将"**允许公共客户端** 流"开关更改为 **"是"，** 然后选择"保存 **"。**</span><span class="sxs-lookup"><span data-stu-id="57623-115">Locate the **Advanced settings** section and change the **Allow public client flows** toggle to **Yes**, then choose **Save**.</span></span>

    ![允许公共客户端流切换的屏幕截图](./images/aad-default-client-type.png)

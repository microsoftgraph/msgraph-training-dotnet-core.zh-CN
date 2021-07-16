# <a name="how-to-run-the-completed-project"></a>如何运行已完成的项目

## <a name="prerequisites"></a>先决条件

若要在此文件夹中运行已完成的项目，您需要：

- [安装在开发计算机上安装的 .NET Core SDK。](https://dotnet.microsoft.com/download)  (**注意：** 本教程是使用 .NET Core SDK 版本 3.1.402 编写的。 本指南中的步骤可能与其他版本一起运行，但尚未经过测试) 
- Microsoft 工作或学校帐户。

如果你没有 Microsoft 帐户，你可以注册开发人员计划Office 365免费[](https://developer.microsoft.com/office/dev-program)订阅Office 365订阅。

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>向管理中心Azure Active Directory Web 应用程序

1. 打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。然后，使用 **个人帐户**（亦称为“Microsoft 帐户”）或 **工作或学校帐户** 登录。

1. 选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。

    ![应用注册屏幕截图 ](/tutorial/images/aad-portal-app-registrations.png)

1. 选择“新注册”。 在“注册应用”页上，按如下方式设置值。

    - 将“名称”设置为“`.NET Core Graph Tutorial`”。
    - 将 **支持的帐户类型** 设置为 **任何组织目录中的帐户**。
    - 保留“重定向 URI”为空。

    ![注册应用程序页面的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. 选择“**注册**”。 在 **".NET Core Graph 教程**"页上，复制 Application **(客户端) ID** 的值并保存它，你将在下一步中需要该值。

    ![新应用注册的应用程序 ID 的屏幕截图](/tutorial/images/aad-application-id.png)

1. 选择" **添加重定向 URI"** 链接。 在" **重定向 URI"** 页上，找到"建议为公共客户端重定向 **URI (移动、桌面**) 部分。 选择 `https://login.microsoftonline.com/common/oauth2/nativeclient` URI。

    ![重定向 URI 页面的屏幕截图](/tutorial/images/aad-redirect-uris.png)

1. 找到"**高级设置"** 部分，将"**允许公共客户端** 流"开关更改为 **"是"，** 然后选择"保存 **"。**

    !["默认客户端类型"部分屏幕截图](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>配置示例

1. 在包含 **GraphTu一l.csproj** 的目录中打开 CLI 并运行以下命令，初始化 [.NET](https://docs.microsoft.com/aspnet/core/security/app-secrets)开发密码存储。

    ```Shell
    dotnet user-secrets init
    ```

1. 使用下列命令将应用程序 ID 和所需范围列表添加到密码存储。 将 `YOUR_APP_ID_HERE` 替换为你在 Azure 门户中创建的应用程序 ID。

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="build-and-run-the-sample"></a>生成和运行示例

在命令行界面中 (CLI) ，导航到项目目录并运行以下命令。

```Shell
dotnet restore
dotnet build
dotnet run
```

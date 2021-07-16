<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将使用管理中心Azure Active Directory Azure AD 应用程序。

1. 打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。然后，使用 **个人帐户**（亦称为“Microsoft 帐户”）或 **工作或学校帐户** 登录。

1. 选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。

    ![应用注册屏幕截图 ](./images/aad-portal-app-registrations.png)

1. 选择“新注册”。 在“注册应用”页上，按如下方式设置值。

    - 将“名称”设置为“`.NET Core Graph Tutorial`”。
    - 将“受支持的帐户类型”设置为“任何组织目录中的帐户和个人 Microsoft 帐户”。
    - 在 **"重定向 URI"** 下，将下拉列表更改为"公共客户端 **(移动&桌面) "，** 将值设置为 `https://login.microsoftonline.com/common/oauth2/nativeclient` 。

    ![注册应用程序页面的屏幕截图](./images/aad-register-an-app.png)

1. 选择“**注册**”。 在 **".NET Core Graph 教程**"页上，复制 Application **(客户端) ID** 的值并保存它，你将在下一步中需要该值。

    ![新应用注册的应用程序 ID 的屏幕截图](./images/aad-application-id.png)

1. 选择“**管理**”下的“**身份验证**”。 找到"**高级设置"** 部分，将"**允许公共客户端** 流"开关更改为 **"是"，** 然后选择"保存 **"。**

    ![允许公共客户端流切换的屏幕截图](./images/aad-default-client-type.png)

<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将 Microsoft Graph 合并到应用程序中。 对于此应用程序，你将使用 Microsoft [Graph .NET 客户端库](https://github.com/microsoftgraph/msgraph-sdk-dotnet) 调用 Microsoft Graph。

## <a name="get-user-details"></a>获取用户详细信息

1. 在 **GraphTu一** l 目录中新建一个名为 Graph 的 **目录**。
1. 在 Graph 目录中新建 **一个名为****GraphHelper.cs** 文件，并添加以下代码。

    ```csharp
    using Microsoft.Graph;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using TimeZoneConverter;

    namespace GraphTutorial
    {
        public class GraphHelper
        {
            private static GraphServiceClient graphClient;
            public static void Initialize(IAuthenticationProvider authProvider)
            {
                graphClient = new GraphServiceClient(authProvider);
            }

            public static async Task<User> GetMeAsync()
            {
                try
                {
                    // GET /me
                    return await graphClient.Me
                        .Request()
                        .Select(u => new{
                            u.DisplayName,
                            u.MailboxSettings
                        })
                        .GetAsync();
                }
                catch (ServiceException ex)
                {
                    Console.WriteLine($"Error getting signed-in user: {ex.Message}");
                    return null;
                }
            }
        }
    }
    ```

1. 在 `Main` **./Program.cs** 中添加以下代码，以获取用户并输出 `GetAccessToken` 用户显示名称。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

如果现在运行应用，则登录应用后欢迎你按名称登录。

## <a name="get-a-calendar-view"></a>获取日历视图

1. 将以下函数 `GraphHelper` 添加到类，以从用户日历获取事件。

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

考虑此代码正在做什么。

- 将调用的 URL 为 `/me/calendarview` 。
- 和 `startDateTime` `endDateTime` 参数定义日历视图的开始和结束。
- `Prefer: outlook.timezone`标头使事件和事件在用户的 `start` `end` 时区中返回。
- `Top`函数最多请求 50 个事件。
- `Select`该函数将每个事件返回的字段限制为仅应用将实际使用的字段。
- `OrderBy`该函数按开始日期和时间对结果进行排序。

## <a name="display-the-results"></a>显示结果

1. 将以下函数添加到类中，将 `Program` Microsoft Graph [中的 dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) 属性格式化为用户友好格式。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. 将以下函数 `Program` 添加到类，获取用户的事件，并输出到控制台。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. 在函数中的注释 `// List the calendar` 后添加 `Main` 以下内容。

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. 保存所有更改并运行应用。 选择 **"查看这一周的日历** "选项以查看用户事件的列表。

    ```Shell
    Welcome Lynne Robbins!

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Meeting
      Organizer: Lynne Robbins
      Start: 9/28/2020 10:00 AM
      End: 9/28/2020 11:30 AM
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 9/28/2020 2:00 PM
      End: 9/28/2020 3:00 PM
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 9/28/2020 4:00 PM
      End: 9/28/2020 5:30 PM
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 9/29/2020 12:00 PM
      End: 9/29/2020 1:00 PM
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 9/29/2020 2:00 PM
      End: 9/29/2020 3:00 PM
    Subject: Project Tailspin
    ```

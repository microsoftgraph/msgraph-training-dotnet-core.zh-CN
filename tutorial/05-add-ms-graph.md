<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将把 Microsoft Graph 合并到应用程序中。 对于此应用程序，您将使用 [Microsoft graph .Net 客户端库](https://github.com/microsoftgraph/msgraph-sdk-dotnet) 对 Microsoft graph 进行调用。

## <a name="get-user-details"></a>获取用户详细信息

1. 在名为**Graph**的**GraphTutorial**目录中创建一个新目录。
1. 在名为 " **GraphHelper.cs** " 的**Graph**目录中创建一个新文件，并将以下代码添加到该文件中。

    ```csharp
    using Microsoft.Graph;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;

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

1. `Main`在调用获取用户并输出用户的显示名称后立即将以下代码添加到 in **./Program.cs**中。 `GetAccessToken`

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

如果现在运行应用程序，则在登录后，应用程序会欢迎你使用名称。

## <a name="get-a-calendar-view"></a>获取日历视图

1. 将以下函数添加到 `GraphHelper` 类，以从用户的日历中获取事件。

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

考虑此代码执行的操作。

- 将调用的 URL 为 `/me/calendarview` 。
- `startDateTime`和 `endDateTime` 参数定义日历视图的开始和结束。
- `Prefer: outlook.timezone`标头会导致 `start` 和 `end` 事件在用户的时区中返回。
- `Top`函数请求最多为50个事件。
- `Select`函数将为每个事件返回的字段限制为仅应用程序实际使用的字段。
- `OrderBy`函数按开始日期和时间对结果进行排序。

## <a name="display-the-results"></a>显示结果

1. 将以下函数添加到 `Program` 类，以将 Microsoft Graph 中的 [datetimetimezone type](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) 属性设置为用户友好的格式。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. 将以下函数添加到 `Program` 类，以获取用户的事件并将其输出到控制台。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. 在函数中的注释后面添加以下项 `// List the calendar` `Main` 。

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. 保存所有更改并运行应用程序。 选择 " **查看本周的日历** " 选项可查看用户事件的列表。

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

<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将把 Microsoft Graph 合并到应用程序中。 对于此应用程序，您将使用[Microsoft graph .Net 客户端库](https://github.com/microsoftgraph/msgraph-sdk-dotnet)对 Microsoft graph 进行调用。

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
                    return await graphClient.Me.Request().GetAsync();
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

1. 紧接`Main`在`GetAccessToken`调用以获取用户并输出用户的显示名称之后，将下面的代码添加到**Program.cs**中。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

如果现在运行应用程序，则在登录后，应用程序会欢迎你使用名称。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

1. 将以下函数添加到`GraphHelper`类，以从用户的日历中获取事件。

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

考虑此代码执行的操作。

- 将调用的 URL 为`/me/events`。
- `Select`函数将为每个事件返回的字段限制为仅应用程序实际使用的字段。
- `OrderBy`函数按其创建日期和时间对结果进行排序，最新项目最先开始。

## <a name="display-the-results"></a>显示结果

1. 将以下函数添加到`Program`类，以将 Microsoft Graph 中的[datetimetimezone type](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)属性设置为用户友好的格式。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. 将以下函数添加到`Program`类，以获取用户的事件并将其输出到控制台。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. 在`// List the calendar` `Main`函数中的注释后面添加以下项。

    ```csharp
    ListCalendarEvents();
    ```

1. 保存所有更改并运行应用程序。 选择 "**列出日历事件**" 选项以查看用户事件的列表。

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List calendar events
    2
    Events:
    Subject: Team meeting
      Organizer: Adele Vance
      Start: 5/22/19, 3:00 PM
      End: 5/22/19, 4:00 PM
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM
      End: 5/24/19, 8:00 PM
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM
      End: 5/26/19, 7:00 PM
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM
      End: 5/27/19, 10:30 PM
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM
      End: 5/28/19, 5:00 PM
    ```

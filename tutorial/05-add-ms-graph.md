<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将将 Microsoft Graph合并到应用程序中。 对于此应用程序，你将使用 Microsoft [Graph .NET 客户端](https://github.com/microsoftgraph/msgraph-sdk-dotnet)库调用 Microsoft Graph。

## <a name="get-user-details"></a>获取用户详细信息

1. 打开 **./Graph/GraphHelper.cs，** 将以下函数添加到 **GraphHelper** 类。

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetMeSnippet":::

1. 调用后，在 `Main` **./Program.cs** 中添加以下代码，以获取用户并输出 `GetAccessToken` 用户的 显示名称。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

如果现在运行应用，登录应用后，将按名称欢迎使用。

## <a name="get-a-calendar-view"></a>获取日历视图

1. 将以下函数 `GraphHelper` 添加到 类，以从用户日历获取事件。

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

考虑此代码将执行什么工作。

- 将调用的 URL 为 `/me/calendarview`。
- `startDateTime`和 `endDateTime` 参数定义日历视图的起始和结束。
- `Prefer: outlook.timezone`标头将导致 `start` 在用户的时区中返回事件的 和 `end` 。
- `Top`函数最多请求 50 个事件。
- `Select`函数将每个事件返回的字段限定为应用将实际使用的字段。
- `OrderBy`函数按开始日期和时间对结果进行排序。

## <a name="display-the-results"></a>显示结果

1. 将以下函数添加到 类，以 `Program` 将 Microsoft 中的[dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)属性Graph用户友好格式。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. 将以下函数 `Program` 添加到 类，获取用户的事件，并输出到控制台。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. 在 函数中的注释 `// List the calendar` 后添加 `Main` 以下内容。

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. 保存所有更改并运行应用。 选择 **"查看本周的日历"** 选项以查看用户事件的列表。

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

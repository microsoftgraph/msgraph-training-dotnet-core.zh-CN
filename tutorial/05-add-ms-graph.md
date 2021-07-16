<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="bb732-101">在此练习中，你将将 Microsoft Graph合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="bb732-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="bb732-102">对于此应用程序，你将使用 Microsoft [Graph .NET 客户端](https://github.com/microsoftgraph/msgraph-sdk-dotnet)库调用 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="bb732-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="bb732-103">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="bb732-103">Get user details</span></span>

1. <span data-ttu-id="bb732-104">打开 **./Graph/GraphHelper.cs，** 将以下函数添加到 **GraphHelper** 类。</span><span class="sxs-lookup"><span data-stu-id="bb732-104">Open **./Graph/GraphHelper.cs** and add the following function to the **GraphHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetMeSnippet":::

1. <span data-ttu-id="bb732-105">调用后，在 `Main` **./Program.cs** 中添加以下代码，以获取用户并输出 `GetAccessToken` 用户的 显示名称。</span><span class="sxs-lookup"><span data-stu-id="bb732-105">Add the following code in `Main` in **./Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

<span data-ttu-id="bb732-106">如果现在运行应用，登录应用后，将按名称欢迎使用。</span><span class="sxs-lookup"><span data-stu-id="bb732-106">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="bb732-107">获取日历视图</span><span class="sxs-lookup"><span data-stu-id="bb732-107">Get a calendar view</span></span>

1. <span data-ttu-id="bb732-108">将以下函数 `GraphHelper` 添加到 类，以从用户日历获取事件。</span><span class="sxs-lookup"><span data-stu-id="bb732-108">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

<span data-ttu-id="bb732-109">考虑此代码将执行什么工作。</span><span class="sxs-lookup"><span data-stu-id="bb732-109">Consider what this code is doing.</span></span>

- <span data-ttu-id="bb732-110">将调用的 URL 为 `/me/calendarview`。</span><span class="sxs-lookup"><span data-stu-id="bb732-110">The URL that will be called is `/me/calendarview`.</span></span>
- <span data-ttu-id="bb732-111">`startDateTime`和 `endDateTime` 参数定义日历视图的起始和结束。</span><span class="sxs-lookup"><span data-stu-id="bb732-111">The `startDateTime` and `endDateTime` parameters define the start and end of the calendar view.</span></span>
- <span data-ttu-id="bb732-112">`Prefer: outlook.timezone`标头将导致 `start` 在用户的时区中返回事件的 和 `end` 。</span><span class="sxs-lookup"><span data-stu-id="bb732-112">The `Prefer: outlook.timezone` header causes the `start` and `end` of the events to be returned in the user's time zone.</span></span>
- <span data-ttu-id="bb732-113">`Top`函数最多请求 50 个事件。</span><span class="sxs-lookup"><span data-stu-id="bb732-113">The `Top` function requests at most 50 events.</span></span>
- <span data-ttu-id="bb732-114">`Select`函数将每个事件返回的字段限定为应用将实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="bb732-114">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="bb732-115">`OrderBy`函数按开始日期和时间对结果进行排序。</span><span class="sxs-lookup"><span data-stu-id="bb732-115">The `OrderBy` function sorts the results by the start date and time.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="bb732-116">显示结果</span><span class="sxs-lookup"><span data-stu-id="bb732-116">Display the results</span></span>

1. <span data-ttu-id="bb732-117">将以下函数添加到 类，以 `Program` 将 Microsoft 中的[dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)属性Graph用户友好格式。</span><span class="sxs-lookup"><span data-stu-id="bb732-117">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. <span data-ttu-id="bb732-118">将以下函数 `Program` 添加到 类，获取用户的事件，并输出到控制台。</span><span class="sxs-lookup"><span data-stu-id="bb732-118">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. <span data-ttu-id="bb732-119">在 函数中的注释 `// List the calendar` 后添加 `Main` 以下内容。</span><span class="sxs-lookup"><span data-stu-id="bb732-119">Add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. <span data-ttu-id="bb732-120">保存所有更改并运行应用。</span><span class="sxs-lookup"><span data-stu-id="bb732-120">Save all of your changes and run the app.</span></span> <span data-ttu-id="bb732-121">选择 **"查看本周的日历"** 选项以查看用户事件的列表。</span><span class="sxs-lookup"><span data-stu-id="bb732-121">Choose the **View this week's calendar** option to see a list of the user's events.</span></span>

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

<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4fddd-101">在本练习中，将把 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="4fddd-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="4fddd-102">对于此应用程序，您将使用[Microsoft graph .Net 客户端库](https://github.com/microsoftgraph/msgraph-sdk-dotnet)对 Microsoft graph 进行调用。</span><span class="sxs-lookup"><span data-stu-id="4fddd-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="4fddd-103">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="4fddd-103">Get user details</span></span>

1. <span data-ttu-id="4fddd-104">在名为**Graph**的**GraphTutorial**目录中创建一个新目录。</span><span class="sxs-lookup"><span data-stu-id="4fddd-104">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="4fddd-105">在名为 " **GraphHelper.cs** " 的**Graph**目录中创建一个新文件，并将以下代码添加到该文件中。</span><span class="sxs-lookup"><span data-stu-id="4fddd-105">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

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

1. <span data-ttu-id="4fddd-106">紧接`Main`在`GetAccessToken`调用以获取用户并输出用户的显示名称之后，将下面的代码添加到**Program.cs**中。</span><span class="sxs-lookup"><span data-stu-id="4fddd-106">Add the following code in `Main` in **Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

<span data-ttu-id="4fddd-107">如果现在运行应用程序，则在登录后，应用程序会欢迎你使用名称。</span><span class="sxs-lookup"><span data-stu-id="4fddd-107">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="4fddd-108">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="4fddd-108">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="4fddd-109">将以下函数添加到`GraphHelper`类，以从用户的日历中获取事件。</span><span class="sxs-lookup"><span data-stu-id="4fddd-109">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

<span data-ttu-id="4fddd-110">考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="4fddd-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="4fddd-111">将调用的 URL 为`/me/events`。</span><span class="sxs-lookup"><span data-stu-id="4fddd-111">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="4fddd-112">`Select`函数将为每个事件返回的字段限制为仅应用程序实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="4fddd-112">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="4fddd-113">`OrderBy`函数按其创建日期和时间对结果进行排序，最新项目最先开始。</span><span class="sxs-lookup"><span data-stu-id="4fddd-113">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="4fddd-114">显示结果</span><span class="sxs-lookup"><span data-stu-id="4fddd-114">Display the results</span></span>

1. <span data-ttu-id="4fddd-115">将以下函数添加到`Program`类，以将 Microsoft Graph 中的[datetimetimezone type](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)属性设置为用户友好的格式。</span><span class="sxs-lookup"><span data-stu-id="4fddd-115">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. <span data-ttu-id="4fddd-116">将以下函数添加到`Program`类，以获取用户的事件并将其输出到控制台。</span><span class="sxs-lookup"><span data-stu-id="4fddd-116">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. <span data-ttu-id="4fddd-117">在`// List the calendar` `Main`函数中的注释后面添加以下项。</span><span class="sxs-lookup"><span data-stu-id="4fddd-117">Add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

    ```csharp
    ListCalendarEvents();
    ```

1. <span data-ttu-id="4fddd-118">保存所有更改并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="4fddd-118">Save all of your changes and run the app.</span></span> <span data-ttu-id="4fddd-119">选择 "**列出日历事件**" 选项以查看用户事件的列表。</span><span class="sxs-lookup"><span data-stu-id="4fddd-119">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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

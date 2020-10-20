<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3f163-101">在本节中，您将添加在用户日历上创建事件的功能。</span><span class="sxs-lookup"><span data-stu-id="3f163-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="3f163-102">打开 **/Graph/GraphHelper.cs** ，并将以下函数添加到 **microsoft.windowsazure.activedirectory.graphhelper** 类中。</span><span class="sxs-lookup"><span data-stu-id="3f163-102">Open **./Graph/GraphHelper.cs** and add the following function to the **GraphHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="CreateEventSnippet":::

    <span data-ttu-id="3f163-103">此代码初始化 **Event** 对象，并使用 Graph SDK 将其添加到用户的日历中。</span><span class="sxs-lookup"><span data-stu-id="3f163-103">This code initializes an **Event** object and uses the Graph SDK to add it to the user's calendar.</span></span>

1. <span data-ttu-id="3f163-104">打开 **./Program.cs** ，并将以下函数添加到 **Program** 类。</span><span class="sxs-lookup"><span data-stu-id="3f163-104">Open **./Program.cs** and add the following functions to the **Program** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="UserInputSnippet":::

    <span data-ttu-id="3f163-105">这些函数是用于读取用户输入的帮助程序函数。</span><span class="sxs-lookup"><span data-stu-id="3f163-105">These functions are helper functions for reading user input.</span></span>

1. <span data-ttu-id="3f163-106">将以下函数添加到 **Program** 类。</span><span class="sxs-lookup"><span data-stu-id="3f163-106">Add the following function to the **Program** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="CreateEventSnippet":::

    <span data-ttu-id="3f163-107">此函数将提示用户提供主题、与会者、开始、结束和正文，然后使用这些值进行调用 `GraphHelper.CreateEvent` 。</span><span class="sxs-lookup"><span data-stu-id="3f163-107">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `GraphHelper.CreateEvent`.</span></span>

1. <span data-ttu-id="3f163-108">在函数中的注释后面添加以下项 `// Create a new event` `Main` 。</span><span class="sxs-lookup"><span data-stu-id="3f163-108">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```csharp
    CreateEvent(user.MailboxSettings.TimeZone);
    ```

1. <span data-ttu-id="3f163-109">保存所有更改并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="3f163-109">Save all of your changes and run the app.</span></span> <span data-ttu-id="3f163-110">选择 " **添加事件** " 选项。</span><span class="sxs-lookup"><span data-stu-id="3f163-110">Choose the **Add an event** option.</span></span> <span data-ttu-id="3f163-111">响应提示以在用户日历上创建新事件。</span><span class="sxs-lookup"><span data-stu-id="3f163-111">Respond to the prompts to create a new event on the user's calendar.</span></span>

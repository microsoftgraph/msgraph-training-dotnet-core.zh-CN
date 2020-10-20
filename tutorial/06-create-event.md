<!-- markdownlint-disable MD002 MD041 -->

在本节中，您将添加在用户日历上创建事件的功能。

1. 打开 **/Graph/GraphHelper.cs** ，并将以下函数添加到 **microsoft.windowsazure.activedirectory.graphhelper** 类中。

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="CreateEventSnippet":::

    此代码初始化 **Event** 对象，并使用 Graph SDK 将其添加到用户的日历中。

1. 打开 **./Program.cs** ，并将以下函数添加到 **Program** 类。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="UserInputSnippet":::

    这些函数是用于读取用户输入的帮助程序函数。

1. 将以下函数添加到 **Program** 类。

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="CreateEventSnippet":::

    此函数将提示用户提供主题、与会者、开始、结束和正文，然后使用这些值进行调用 `GraphHelper.CreateEvent` 。

1. 在函数中的注释后面添加以下项 `// Create a new event` `Main` 。

    ```csharp
    CreateEvent(user.MailboxSettings.TimeZone);
    ```

1. 保存所有更改并运行应用程序。 选择 " **添加事件** " 选项。 响应提示以在用户日历上创建新事件。

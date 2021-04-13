---
ms.openlocfilehash: 397d564fc3389f341e06977bd4cba861dd1c9e5b
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695804"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7b891-101">Neste exercício, você incorporará o Microsoft Graph ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7b891-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="7b891-102">Para este aplicativo, você usará o [SDK](https://github.com/microsoftgraph/msgraph-sdk-java) do Microsoft Graph para Java fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="7b891-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="7b891-103">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="7b891-103">Get user details</span></span>

1. <span data-ttu-id="7b891-104">Crie um novo arquivo no **diretório ./graphtutorial/src/main/java/graphtutorial** chamado **Graph.java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="7b891-104">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetUserSnippet":::

1. <span data-ttu-id="7b891-105">Adicione a `import` instrução a seguir na parte superior de **App.java**.</span><span class="sxs-lookup"><span data-stu-id="7b891-105">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.User;
    ```

1. <span data-ttu-id="7b891-106">Adicione o código a seguir **em App.java** pouco antes da linha para obter o usuário e exibir o nome de `Scanner input = new Scanner(System.in);` exibição do usuário.</span><span class="sxs-lookup"><span data-stu-id="7b891-106">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser();
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="7b891-107">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7b891-107">Run the app.</span></span> <span data-ttu-id="7b891-108">Depois de fazer logoff, o aplicativo recebe você pelo nome.</span><span class="sxs-lookup"><span data-stu-id="7b891-108">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="7b891-109">Obtenha eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="7b891-109">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="7b891-110">Adicione a seguinte função à `Graph` classe em **Graph.java** para obter eventos do calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="7b891-110">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="7b891-111">Considere o que este código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="7b891-111">Consider what this code is doing.</span></span>

- <span data-ttu-id="7b891-112">O URL que será chamado é `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="7b891-112">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="7b891-113">`QueryOption` são usados para adicionar os `startDateTime` `endDateTime` parâmetros e, definindo o início e o fim do exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="7b891-113">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="7b891-114">Um `QueryOption` objeto é usado para adicionar o `$orderby` parâmetro, classificação dos resultados por hora de início.</span><span class="sxs-lookup"><span data-stu-id="7b891-114">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="7b891-115">Um `HeaderOption` objeto é usado para adicionar o header, fazendo com que os horários de início e término sejam ajustados ao `Prefer: outlook.timezone` fuso horário do usuário.</span><span class="sxs-lookup"><span data-stu-id="7b891-115">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="7b891-116">A `select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.</span><span class="sxs-lookup"><span data-stu-id="7b891-116">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="7b891-117">A `top` função limita o número de eventos na resposta a um máximo de 25.</span><span class="sxs-lookup"><span data-stu-id="7b891-117">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="7b891-118">A função é usada para solicitar páginas adicionais de resultados se houver mais de `getNextPage` 25 eventos na semana atual.</span><span class="sxs-lookup"><span data-stu-id="7b891-118">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="7b891-119">Crie um novo arquivo no **diretório ./graphtutorial/src/main/java/graphtutorial** chamado **GraphToIana.java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="7b891-119">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="7b891-120">Essa classe implementa uma simples análise para converter nomes de fuso horário do Windows em identificadores IANA e gerar uma **ZoneId** com base em um nome de fuso horário do Windows.</span><span class="sxs-lookup"><span data-stu-id="7b891-120">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="7b891-121">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="7b891-121">Display the results</span></span>

1. <span data-ttu-id="7b891-122">Adicione as instruções `import` a seguir **em App.java**.</span><span class="sxs-lookup"><span data-stu-id="7b891-122">Add the following `import` statements in **App.java**.</span></span>

    ```java
    import java.time.DayOfWeek;
    import java.time.LocalDateTime;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.DateTimeParseException;
    import java.time.format.FormatStyle;
    import java.time.temporal.ChronoUnit;
    import java.time.temporal.TemporalAdjusters;
    import java.util.HashSet;
    import java.util.List;
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.Event;
    ```

1. <span data-ttu-id="7b891-123">Adicione a seguinte função à classe para formatar as propriedades `App` [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) do Microsoft Graph em um formato amigável.</span><span class="sxs-lookup"><span data-stu-id="7b891-123">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="7b891-124">Adicione a função a seguir à classe para obter os eventos do usuário e `App` de saída para o console.</span><span class="sxs-lookup"><span data-stu-id="7b891-124">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="7b891-125">Adicione o seguinte logo após `// List the calendar` o comentário na `main` função.</span><span class="sxs-lookup"><span data-stu-id="7b891-125">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="7b891-126">Salve todas as alterações, crie o aplicativo e execute-o.</span><span class="sxs-lookup"><span data-stu-id="7b891-126">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="7b891-127">Escolha a **opção Listar eventos de calendário** para ver uma lista dos eventos do usuário.</span><span class="sxs-lookup"><span data-stu-id="7b891-127">Choose the **List calendar events** option to see a list of the user's events.</span></span>

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 12/7/20, 2:00 PM (Pacific Standard Time)
      End: 12/7/20, 3:00 PM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/7/20, 4:00 PM (Pacific Standard Time)
      End: 12/7/20, 5:30 PM (Pacific Standard Time)
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 12/8/20, 12:00 PM (Pacific Standard Time)
      End: 12/8/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/8/20, 3:00 PM (Pacific Standard Time)
      End: 12/8/20, 4:30 PM (Pacific Standard Time)
    Subject: Company Meeting
      Organizer: Christie Cline
      Start: 12/9/20, 8:30 AM (Pacific Standard Time)
      End: 12/9/20, 11:00 AM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/9/20, 4:00 PM (Pacific Standard Time)
      End: 12/9/20, 5:30 PM (Pacific Standard Time)
    Subject: Project Team Meeting
      Organizer: Lidia Holloway
      Start: 12/10/20, 8:00 AM (Pacific Standard Time)
      End: 12/10/20, 9:30 AM (Pacific Standard Time)
    Subject: Weekly Marketing Lunch
      Organizer: Adele Vance
      Start: 12/10/20, 12:00 PM (Pacific Standard Time)
      End: 12/10/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/10/20, 3:00 PM (Pacific Standard Time)
      End: 12/10/20, 4:30 PM (Pacific Standard Time)
    Subject: Lunch?
      Organizer: Lynne Robbins
      Start: 12/11/20, 12:00 PM (Pacific Standard Time)
      End: 12/11/20, 1:00 PM (Pacific Standard Time)
    Subject: Friday Unwinder
      Organizer: Megan Bowen
      Start: 12/11/20, 4:00 PM (Pacific Standard Time)
      End: 12/11/20, 5:00 PM (Pacific Standard Time)
    ```

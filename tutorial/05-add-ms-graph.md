---
ms.openlocfilehash: c26e5b8ab0b7c5c62b926e3f5416b94e3f10b601
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661043"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="19ff0-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="19ff0-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="19ff0-102">Para este aplicativo, você usará o [SDK do Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="19ff0-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="19ff0-103">Implementar um provedor de autenticação</span><span class="sxs-lookup"><span data-stu-id="19ff0-103">Implement an authentication provider</span></span>

<span data-ttu-id="19ff0-104">O SDK do Microsoft Graph para Java requer uma implementação da `IAuthenticationProvider` interface para criar uma instância do `GraphServiceClient` objeto.</span><span class="sxs-lookup"><span data-stu-id="19ff0-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="19ff0-105">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/graphtutorial** chamado **SimpleAuthProvider. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="19ff0-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="19ff0-106">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="19ff0-106">Get user details</span></span>

1. <span data-ttu-id="19ff0-107">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/graphtutorial** chamado **Graph. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="19ff0-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.graph.requests.extensions.IEventCollectionRequestBuilder;

    /**
     * Graph
     */
    public class Graph {

        private static IGraphServiceClient graphClient = null;
        private static SimpleAuthProvider authProvider = null;

        private static void ensureGraphClient(String accessToken) {
            if (graphClient == null) {
                // Create the auth provider
                authProvider = new SimpleAuthProvider(accessToken);

                // Create default logger to only log errors
                DefaultLogger logger = new DefaultLogger();
                logger.setLoggingLevel(LoggerLevel.ERROR);

                // Build a Graph client
                graphClient = GraphServiceClient.builder()
                    .authenticationProvider(authProvider)
                    .logger(logger)
                    .buildClient();
            }
        }

        public static User getUser(String accessToken) {
            ensureGraphClient(accessToken);

            // GET /me to get authenticated user
            User me = graphClient
                .me()
                .buildRequest()
                .select("displayName,mailboxSettings")
                .get();

            return me;
        }
    }
    ```

1. <span data-ttu-id="19ff0-108">Adicione a seguinte `import` instrução na parte superior de **app. java**.</span><span class="sxs-lookup"><span data-stu-id="19ff0-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="19ff0-109">Adicione o código a seguir no **app. java** antes da `Scanner input = new Scanner(System.in);` linha para obter o usuário e saída o nome de exibição do usuário.</span><span class="sxs-lookup"><span data-stu-id="19ff0-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="19ff0-110">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="19ff0-110">Run the app.</span></span> <span data-ttu-id="19ff0-111">Depois que você fizer logon no aplicativo, o nome será bem-vindo.</span><span class="sxs-lookup"><span data-stu-id="19ff0-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="19ff0-112">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="19ff0-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="19ff0-113">Adicione a função a seguir à `Graph` classe em **Graph. java** para obter eventos do calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="19ff0-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="19ff0-114">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="19ff0-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="19ff0-115">A URL que será chamada é `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="19ff0-115">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="19ff0-116">`QueryOption` os objetos são usados para adicionar `startDateTime` os `endDateTime` parâmetros e, ao definir o início e o fim do modo de exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="19ff0-116">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="19ff0-117">Um `QueryOption` objeto é usado para adicionar o `$orderby` parâmetro, classificando os resultados por hora de início.</span><span class="sxs-lookup"><span data-stu-id="19ff0-117">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="19ff0-118">Um `HeaderOption` objeto é usado para adicionar o `Prefer: outlook.timezone` cabeçalho, fazendo com que as horas de início e de término sejam ajustadas para o fuso horário do usuário.</span><span class="sxs-lookup"><span data-stu-id="19ff0-118">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="19ff0-119">A `select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.</span><span class="sxs-lookup"><span data-stu-id="19ff0-119">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="19ff0-120">A `top` função limita o número de eventos na resposta a um máximo de 25.</span><span class="sxs-lookup"><span data-stu-id="19ff0-120">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="19ff0-121">A `getNextPage` função é usada para solicitar páginas adicionais de resultados se houver mais de 25 eventos na semana atual.</span><span class="sxs-lookup"><span data-stu-id="19ff0-121">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="19ff0-122">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/graphtutorial** chamado **GraphToIana. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="19ff0-122">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="19ff0-123">Essa classe implementa uma pesquisa simples para converter os nomes de fuso horário do Windows para identificadores da IANA e para gerar um **Identificação_da_Zona** baseado em um nome de fuso horário do Windows.</span><span class="sxs-lookup"><span data-stu-id="19ff0-123">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="19ff0-124">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="19ff0-124">Display the results</span></span>

1. <span data-ttu-id="19ff0-125">Adicione as seguintes `import` instruções em **app. java**.</span><span class="sxs-lookup"><span data-stu-id="19ff0-125">Add the following `import` statements in **App.java**.</span></span>

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
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. <span data-ttu-id="19ff0-126">Adicione a função a seguir à `App` classe para formatar as propriedades [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) do Microsoft Graph em um formato amigável.</span><span class="sxs-lookup"><span data-stu-id="19ff0-126">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="19ff0-127">Adicione a função a seguir à `App` classe para obter os eventos do usuário e a saída para o console.</span><span class="sxs-lookup"><span data-stu-id="19ff0-127">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="19ff0-128">Adicione o seguinte logo após o `// List the calendar` comentário na `main` função.</span><span class="sxs-lookup"><span data-stu-id="19ff0-128">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken);
    ```

1. <span data-ttu-id="19ff0-129">Salve todas as suas alterações, crie o aplicativo e, em seguida, execute-o.</span><span class="sxs-lookup"><span data-stu-id="19ff0-129">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="19ff0-130">Escolha a opção **listar eventos de calendário** para ver uma lista dos eventos do usuário.</span><span class="sxs-lookup"><span data-stu-id="19ff0-130">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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

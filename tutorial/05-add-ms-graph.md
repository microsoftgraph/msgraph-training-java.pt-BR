---
ms.openlocfilehash: 93688a97872ad640c12c7137f4cc09ede4a98416
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612065"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="84b9f-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="84b9f-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="84b9f-102">Para este aplicativo, você usará o [SDK do Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="84b9f-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="84b9f-103">Implementar um provedor de autenticação</span><span class="sxs-lookup"><span data-stu-id="84b9f-103">Implement an authentication provider</span></span>

<span data-ttu-id="84b9f-104">O SDK do Microsoft Graph para Java requer uma implementação da `IAuthenticationProvider` interface para criar uma `GraphServiceClient` instância do objeto.</span><span class="sxs-lookup"><span data-stu-id="84b9f-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="84b9f-105">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/graphtutorial** chamado **SimpleAuthProvider. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="84b9f-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="84b9f-106">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="84b9f-106">Get user details</span></span>

1. <span data-ttu-id="84b9f-107">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/graphtutorial** chamado **Graph. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="84b9f-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.util.LinkedList;
    import java.util.List;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;

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
                .get();

            return me;
        }
    }
    ```

1. <span data-ttu-id="84b9f-108">Adicione a seguinte `import` instrução na parte superior de **app. java**.</span><span class="sxs-lookup"><span data-stu-id="84b9f-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="84b9f-109">Adicione o código a seguir no **app. java** antes da `Scanner input = new Scanner(System.in);` linha para obter o usuário e saída o nome de exibição do usuário.</span><span class="sxs-lookup"><span data-stu-id="84b9f-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println();
    ```

1. <span data-ttu-id="84b9f-110">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="84b9f-110">Run the app.</span></span> <span data-ttu-id="84b9f-111">Depois que você fizer logon no aplicativo, o nome será bem-vindo.</span><span class="sxs-lookup"><span data-stu-id="84b9f-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="84b9f-112">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="84b9f-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="84b9f-113">Adicione a função a seguir à `Graph` classe em **Graph. java** para obter eventos do calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="84b9f-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="84b9f-114">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="84b9f-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="84b9f-115">A URL que será chamada é `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="84b9f-115">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="84b9f-116">A `select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.</span><span class="sxs-lookup"><span data-stu-id="84b9f-116">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="84b9f-117">A `QueryOption` é usada para classificar os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="84b9f-117">A `QueryOption` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="84b9f-118">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="84b9f-118">Display the results</span></span>

1. <span data-ttu-id="84b9f-119">Adicione as seguintes `import` instruções em **app. java**.</span><span class="sxs-lookup"><span data-stu-id="84b9f-119">Add the following `import` statements in **App.java**.</span></span>

    ```java
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.FormatStyle;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. <span data-ttu-id="84b9f-120">Adicione a função a seguir à `App` classe para formatar as propriedades [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) do Microsoft Graph em um formato amigável.</span><span class="sxs-lookup"><span data-stu-id="84b9f-120">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="84b9f-121">Adicione a função a seguir à `App` classe para obter os eventos do usuário e a saída para o console.</span><span class="sxs-lookup"><span data-stu-id="84b9f-121">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="84b9f-122">Adicione o seguinte logo após o `// List the calendar` comentário na `main` função.</span><span class="sxs-lookup"><span data-stu-id="84b9f-122">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken);
    ```

1. <span data-ttu-id="84b9f-123">Salve todas as suas alterações, crie o aplicativo e, em seguida, execute-o.</span><span class="sxs-lookup"><span data-stu-id="84b9f-123">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="84b9f-124">Escolha a opção **listar eventos de calendário** para ver uma lista dos eventos do usuário.</span><span class="sxs-lookup"><span data-stu-id="84b9f-124">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
      Start: 5/22/19, 3:00 PM (UTC)
      End: 5/22/19, 4:00 PM (UTC)
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM (UTC)
      End: 5/24/19, 8:00 PM (UTC)
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM (UTC)
      End: 5/26/19, 7:00 PM (UTC)
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM (UTC)
      End: 5/27/19, 10:30 PM (UTC)
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM (UTC)
      End: 5/28/19, 5:00 PM (UTC)
    ```

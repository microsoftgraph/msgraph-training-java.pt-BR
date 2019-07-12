---
ms.openlocfilehash: 3fb56d613dbaa56474c9af58ebdd0b157c53bf1a
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634763"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="01791-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="01791-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="01791-102">Para este aplicativo, você usará o [SDK do Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="01791-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="01791-103">Implementar um provedor de autenticação</span><span class="sxs-lookup"><span data-stu-id="01791-103">Implement an authentication provider</span></span>

<span data-ttu-id="01791-104">O SDK do Microsoft Graph para Java requer uma implementação da `IAuthenticationProvider` interface para criar uma `GraphServiceClient` instância do objeto.</span><span class="sxs-lookup"><span data-stu-id="01791-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span> <span data-ttu-id="01791-105">Comece criando uma classe simples para adicionar o token de acesso às solicitações de saída.</span><span class="sxs-lookup"><span data-stu-id="01791-105">Start by creating a simple class to add the access token to outgoing requests.</span></span> <span data-ttu-id="01791-106">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/com/contoso** chamado **SimpleAuthProvider. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="01791-106">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

```java
package com.contoso;

import com.microsoft.graph.authentication.IAuthenticationProvider;
import com.microsoft.graph.http.IHttpRequest;

/**
 * SimpleAuthProvider
 */
public class SimpleAuthProvider implements IAuthenticationProvider {

    private String accessToken = null;

    public SimpleAuthProvider(String accessToken) {
        this.accessToken = accessToken;
    }

    @Override
    public void authenticateRequest(IHttpRequest request) {
        // Add the access token in the Authorization header
        request.addHeader("Authorization", "Bearer " + accessToken);
    }
}
```

## <a name="get-user-details"></a><span data-ttu-id="01791-107">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="01791-107">Get user details</span></span>

<span data-ttu-id="01791-108">Primeiro, adicione uma nova classe para conter todas as funcionalidades de gráfico.</span><span class="sxs-lookup"><span data-stu-id="01791-108">First, add a new class to contain all of the Graph functionality.</span></span> <span data-ttu-id="01791-109">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/com/contoso** chamado **Graph. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="01791-109">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Graph.java** and add the following code.</span></span>

```java
package com.contoso;

import com.microsoft.graph.logger.DefaultLogger;
import com.microsoft.graph.logger.LoggerLevel;
import com.microsoft.graph.models.extensions.IGraphServiceClient;
import com.microsoft.graph.models.extensions.User;
import com.microsoft.graph.requests.extensions.GraphServiceClient;
import java.util.LinkedList;
import java.util.List;import com.microsoft.graph.models.extensions.Event;import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
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

<span data-ttu-id="01791-110">Adicione o código a seguir no **app. java** antes da `Scanner input = new Scanner(System.in);` linha para obter o usuário e saída o nome de exibição do usuário.</span><span class="sxs-lookup"><span data-stu-id="01791-110">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

```java
// Greet the user
User user = Graph.getUser(accessToken);
System.out.println("Welcome " + user.displayName);
System.out.println();
```

<span data-ttu-id="01791-111">Se você executar o aplicativo agora, após fazer logon no aplicativo, o nome será bem-vindo.</span><span class="sxs-lookup"><span data-stu-id="01791-111">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="01791-112">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="01791-112">Get calendar events from Outlook</span></span>

<span data-ttu-id="01791-113">Adicione as seguintes `import` instruções ao **Graph. java**.</span><span class="sxs-lookup"><span data-stu-id="01791-113">Add the following `import` statements to **Graph.java**.</span></span>

```java
import java.util.LinkedList;
import java.util.List;
import com.microsoft.graph.models.extensions.Event;
import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
```

<span data-ttu-id="01791-114">Adicione a função a seguir à `Graph` classe em **Graph. java** para obter eventos do calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="01791-114">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

```java
public static List<Event> getEvents(String accessToken) {
    ensureGraphClient(accessToken);

    // Use QueryOption to specify the $orderby query parameter
    final List<Option> options = new LinkedList<Option>();
    // Sort results by createdDateTime, get newest first
    options.add(new QueryOption("orderby", "createdDateTime DESC"));

    // GET /me/events
    IEventCollectionPage eventPage = graphClient
        .me()
        .events()
        .buildRequest(options)
        .select("subject,organizer,start,end")
        .get();

    return eventPage.getCurrentPage();
}
```

<span data-ttu-id="01791-115">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="01791-115">Consider what this code is doing.</span></span>

- <span data-ttu-id="01791-116">A URL que será chamada é `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="01791-116">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="01791-117">A `select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.</span><span class="sxs-lookup"><span data-stu-id="01791-117">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="01791-118">A `QueryOption` é usada para classificar os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="01791-118">A `QueryOption` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="01791-119">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="01791-119">Display the results</span></span>

<span data-ttu-id="01791-120">Comece adicionando as seguintes `import` instruções em **app. java**.</span><span class="sxs-lookup"><span data-stu-id="01791-120">Start by adding the following `import` statements in **App.java**.</span></span>

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.List;
```

<span data-ttu-id="01791-121">Em seguida, adicione a função a `App` seguir à classe para formatar as propriedades [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) do Microsoft Graph em um formato amigável.</span><span class="sxs-lookup"><span data-stu-id="01791-121">Then add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

```java
private static String formatDateTimeTimeZone(DateTimeTimeZone date) {
    LocalDateTime dateTime = LocalDateTime.parse(date.dateTime);

    return dateTime.format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT)) + " (" + date.timeZone + ")";
}
```

<span data-ttu-id="01791-122">Em seguida, adicione a função a seguir `App` à classe para obter os eventos do usuário e a saída para o console.</span><span class="sxs-lookup"><span data-stu-id="01791-122">Next, add the following function to the `App` class to get the user's events and output them to the console.</span></span>

```java
private static void listCalendarEvents(String accessToken) {
    // Get the user's events
    List<Event> events = Graph.getEvents(accessToken);

    System.out.println("Events:");

    for (Event event : events) {
        System.out.println("Subject: " + event.subject);
        System.out.println("  Organizer: " + event.organizer.emailAddress.name);
        System.out.println("  Start: " + formatDateTimeTimeZone(event.start));
        System.out.println("  End: " + formatDateTimeTimeZone(event.end));
    }

    System.out.println();
}
```

<span data-ttu-id="01791-123">Por fim, adicione o seguinte logo após `// List the calendar` o comentário na `main` função.</span><span class="sxs-lookup"><span data-stu-id="01791-123">Finally, add the following just after the `// List the calendar` comment in the `main` function.</span></span>

```java
listCalendarEvents(accessToken);
```

<span data-ttu-id="01791-124">Salve todas as suas alterações e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="01791-124">Save all of your changes and run the app.</span></span> <span data-ttu-id="01791-125">Escolha a opção **listar eventos de calendário** para ver uma lista dos eventos do usuário.</span><span class="sxs-lookup"><span data-stu-id="01791-125">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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

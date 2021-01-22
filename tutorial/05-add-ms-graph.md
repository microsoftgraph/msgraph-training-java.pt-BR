---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919526"
---
<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph ao aplicativo. Para esse aplicativo, você usará o [SDK](https://github.com/microsoftgraph/msgraph-sdk-java) do Microsoft Graph para Java para fazer chamadas para o Microsoft Graph.

## <a name="implement-an-authentication-provider"></a>Implementar um provedor de autenticação

O SDK do Microsoft Graph para Java requer uma implementação da `IAuthenticationProvider` interface para inciar seu `GraphServiceClient` objeto.

1. Crie um novo arquivo no diretório **./graphtutorial/src/main/java/graphtutorial** chamado **SimpleAuthProvider.java** e adicione o código a seguir.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>Obter detalhes do usuário

1. Crie um novo arquivo no **diretório ./graphtutorial/src/main/java/graphtutorial** chamado **Graph.java** e adicione o código a seguir.

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

1. Adicione a `import` instrução a seguir na parte superior de **App.java**.

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. Adicione o código a seguir **em App.java** antes da linha para obter o usuário e exibir o `Scanner input = new Scanner(System.in);` nome de exibição do usuário.

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. Execute o aplicativo. Depois que você fizer logoff no aplicativo, receberá você pelo nome.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

1. Adicione a seguinte função à `Graph` classe em **Graph.java** para obter eventos do calendário do usuário.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Considere o que este código está fazendo.

- A URL que será chamada é `/me/calendarview` .
  - `QueryOption` são usados para adicionar os `startDateTime` `endDateTime` parâmetros, definindo o início e o fim da exibição de calendário.
  - Um `QueryOption` objeto é usado para adicionar o `$orderby` parâmetro, classificação dos resultados por hora de início.
  - Um `HeaderOption` objeto é usado para adicionar o header, fazendo com que as horas de início e de término sejam ajustadas ao `Prefer: outlook.timezone` fuso horário do usuário.
  - A `select` função limita os campos retornados para cada evento apenas àqueles que o aplicativo realmente usará.
  - A `top` função limita o número de eventos na resposta a um máximo de 25.
- A função é usada para solicitar páginas adicionais de resultados se houver mais de `getNextPage` 25 eventos na semana atual.

1. Crie um novo arquivo no **diretório ./graphtutorial/src/main/java/graphtutorial** chamado **GraphToIana.java** e adicione o código a seguir.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    Essa classe implementa uma simples análise para converter nomes de fuso horário do Windows em identificadores IANA e gerar uma **ZoneId** com base em um nome de fuso horário do Windows.

## <a name="display-the-results"></a>Exibir os resultados

1. Adicione as instruções `import` a seguir **em App.java**.

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

1. Adicione a função a seguir à classe para formatar as `App` propriedades [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) do Microsoft Graph em um formato amigável.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Adicione a função a seguir à classe para obter os eventos do usuário e de saída `App` para o console.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Adicione o seguinte logo após `// List the calendar` o comentário na `main` função.

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. Salve todas as alterações, crie o aplicativo e execute-o. Escolha a **opção Listar** eventos de calendário para ver uma lista dos eventos do usuário.

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

---
ms.openlocfilehash: 5e5168a6ccaf1374bc720f38a71a72d80d9d2b32
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695790"
---
<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a Biblioteca de Autenticação [da Microsoft (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java) para Java ao aplicativo.

1. Crie um novo diretório chamado **graphtutorial** no **diretório ./src/main/resources.**

1. Crie um novo arquivo no **diretório ./src/main/resources/graphtutorial** chamado **oAuth.properties** e adicione o seguinte texto nesse arquivo. Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    O valor de `app.scopes` contém os escopos de permissão que o aplicativo requer.

    - **User.Read** permite que o aplicativo acesse o perfil do usuário.
    - **MailboxSettings.Read** permite que o aplicativo acesse configurações da caixa de correio do usuário, incluindo o fuso horário configurado pelo usuário.
    - **Calendars.ReadWrite** permite que o aplicativo liste o calendário do usuário e adicione novos eventos ao calendário.

    > [!IMPORTANT]
    > Se você estiver usando o controle de origem, como git, agora seria um bom momento para excluir o arquivo **oAuth.properties** do controle de origem para evitar o vazamento inadvertida da ID do aplicativo.

1. Abra **App.java** e adicione as instruções `import` a seguir.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Adicione o código a seguir pouco antes `Scanner input = new Scanner(System.in);` da linha para carregar o arquivo **oAuth.properties.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Implementar login

1. Crie um novo arquivo no **diretório ./graphtutorial/src/main/java/graphtutorial** chamado **Graph.java** e adicione o código a seguir.

    ```java
    package graphtutorial;

    import java.net.URL;
    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import okhttp3.Request;

    import com.azure.identity.DeviceCodeCredential;
    import com.azure.identity.DeviceCodeCredentialBuilder;

    import com.microsoft.graph.authentication.TokenCredentialAuthProvider;
    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.Attendee;
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.EmailAddress;
    import com.microsoft.graph.models.Event;
    import com.microsoft.graph.models.ItemBody;
    import com.microsoft.graph.models.User;
    import com.microsoft.graph.models.AttendeeType;
    import com.microsoft.graph.models.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.GraphServiceClient;
    import com.microsoft.graph.requests.EventCollectionPage;
    import com.microsoft.graph.requests.EventCollectionRequestBuilder;

    public class Graph {

        private static GraphServiceClient<Request> graphClient = null;
        private static TokenCredentialAuthProvider authProvider = null;

        public static void initializeGraphAuth(String applicationId, List<String> scopes) {
            // Create the auth provider
            final DeviceCodeCredential credential = new DeviceCodeCredentialBuilder()
                .clientId(applicationId)
                .challengeConsumer(challenge -> System.out.println(challenge.getMessage()))
                .build();

            authProvider = new TokenCredentialAuthProvider(scopes, credential);

            // Create default logger to only log errors
            DefaultLogger logger = new DefaultLogger();
            logger.setLoggingLevel(LoggerLevel.ERROR);

            // Build a Graph client
            graphClient = GraphServiceClient.builder()
                .authenticationProvider(authProvider)
                .logger(logger)
                .buildClient();
        }

        public static String getUserAccessToken()
        {
            try {
                URL meUrl = new URL("https://graph.microsoft.com/v1.0/me");
                return authProvider.getAuthorizationTokenAsync(meUrl).get();
            } catch(Exception ex) {
                return null;
            }
        }
    }
    ```

1. Em **App.java**, adicione o código a seguir pouco antes da `Scanner input = new Scanner(System.in);` linha para obter um token de acesso.

    ```java
    // Initialize Graph with auth settings
    Graph.initializeGraphAuth(appId, appScopes);
    final String accessToken = Graph.getUserAccessToken();
    ```

1. Adicione a seguinte linha após o `// Display access token` comentário.

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. Execute o aplicativo. O aplicativo exibe uma URL e um código de dispositivo.

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. Abra um navegador e navegue até a URL exibida. Insira o código fornecido e entre. Depois de concluído, retorne ao aplicativo e escolha **o 1. Exibir a opção de token** de acesso para exibir o token de acesso.

> [!TIP]
> Tokens de acesso para contas de estudante ou de trabalho da Microsoft podem ser analisados para fins de solução de problemas em [https://jwt.ms](https://jwt.ms) . Tokens de acesso para contas pessoais da Microsoft usam um formato proprietário e não podem ser analisados.

---
ms.openlocfilehash: 5e5168a6ccaf1374bc720f38a71a72d80d9d2b32
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695790"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c6acb-101">Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6acb-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="c6acb-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c6acb-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="c6acb-103">Nesta etapa, você integrará a Biblioteca de Autenticação [da Microsoft (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java) para Java ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c6acb-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="c6acb-104">Crie um novo diretório chamado **graphtutorial** no **diretório ./src/main/resources.**</span><span class="sxs-lookup"><span data-stu-id="c6acb-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="c6acb-105">Crie um novo arquivo no **diretório ./src/main/resources/graphtutorial** chamado **oAuth.properties** e adicione o seguinte texto nesse arquivo.</span><span class="sxs-lookup"><span data-stu-id="c6acb-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="c6acb-106">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="c6acb-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="c6acb-107">O valor de `app.scopes` contém os escopos de permissão que o aplicativo requer.</span><span class="sxs-lookup"><span data-stu-id="c6acb-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="c6acb-108">**User.Read** permite que o aplicativo acesse o perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="c6acb-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="c6acb-109">**MailboxSettings.Read** permite que o aplicativo acesse configurações da caixa de correio do usuário, incluindo o fuso horário configurado pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="c6acb-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="c6acb-110">**Calendars.ReadWrite** permite que o aplicativo liste o calendário do usuário e adicione novos eventos ao calendário.</span><span class="sxs-lookup"><span data-stu-id="c6acb-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c6acb-111">Se você estiver usando o controle de origem, como git, agora seria um bom momento para excluir o arquivo **oAuth.properties** do controle de origem para evitar o vazamento inadvertida da ID do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c6acb-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="c6acb-112">Abra **App.java** e adicione as instruções `import` a seguir.</span><span class="sxs-lookup"><span data-stu-id="c6acb-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="c6acb-113">Adicione o código a seguir pouco antes `Scanner input = new Scanner(System.in);` da linha para carregar o arquivo **oAuth.properties.**</span><span class="sxs-lookup"><span data-stu-id="c6acb-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="c6acb-114">Implementar login</span><span class="sxs-lookup"><span data-stu-id="c6acb-114">Implement sign-in</span></span>

1. <span data-ttu-id="c6acb-115">Crie um novo arquivo no **diretório ./graphtutorial/src/main/java/graphtutorial** chamado **Graph.java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c6acb-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

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

1. <span data-ttu-id="c6acb-116">Em **App.java**, adicione o código a seguir pouco antes da `Scanner input = new Scanner(System.in);` linha para obter um token de acesso.</span><span class="sxs-lookup"><span data-stu-id="c6acb-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Initialize Graph with auth settings
    Graph.initializeGraphAuth(appId, appScopes);
    final String accessToken = Graph.getUserAccessToken();
    ```

1. <span data-ttu-id="c6acb-117">Adicione a seguinte linha após o `// Display access token` comentário.</span><span class="sxs-lookup"><span data-stu-id="c6acb-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="c6acb-118">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c6acb-118">Run the app.</span></span> <span data-ttu-id="c6acb-119">O aplicativo exibe uma URL e um código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="c6acb-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="c6acb-120">Abra um navegador e navegue até a URL exibida.</span><span class="sxs-lookup"><span data-stu-id="c6acb-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="c6acb-121">Insira o código fornecido e entre.</span><span class="sxs-lookup"><span data-stu-id="c6acb-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="c6acb-122">Depois de concluído, retorne ao aplicativo e escolha **o 1. Exibir a opção de token** de acesso para exibir o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="c6acb-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="c6acb-123">Tokens de acesso para contas de estudante ou de trabalho da Microsoft podem ser analisados para fins de solução de problemas em [https://jwt.ms](https://jwt.ms) .</span><span class="sxs-lookup"><span data-stu-id="c6acb-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="c6acb-124">Tokens de acesso para contas pessoais da Microsoft usam um formato proprietário e não podem ser analisados.</span><span class="sxs-lookup"><span data-stu-id="c6acb-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>

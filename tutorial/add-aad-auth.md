---
ms.openlocfilehash: 44426ee29265e8730ea3bc19f21091aa0180fd6e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634742"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="99be5-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99be5-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="99be5-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="99be5-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="99be5-103">Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="99be5-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

<span data-ttu-id="99be5-104">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/com/contoso** chamado **OAuth. Properties**.</span><span class="sxs-lookup"><span data-stu-id="99be5-104">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **oAuth.properties**.</span></span> <span data-ttu-id="99be5-105">Adicione o seguinte texto nesse arquivo.</span><span class="sxs-lookup"><span data-stu-id="99be5-105">Add the following text in that file.</span></span>

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

<span data-ttu-id="99be5-106">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="99be5-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="99be5-107">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir o arquivo **OAuth. Properties** do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="99be5-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="99be5-108">Abra **app. java** e adicione as instruções `import` a seguir.</span><span class="sxs-lookup"><span data-stu-id="99be5-108">Open **App.java** and add the following `import` statements.</span></span>

```java
import java.io.IOException;
import java.util.Properties;
```

<span data-ttu-id="99be5-109">Em seguida, adicione o código a seguir `Scanner input = new Scanner(System.in);` antes da linha carregar o arquivo **OAuth. Properties** .</span><span class="sxs-lookup"><span data-stu-id="99be5-109">Then add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

```java
// Load OAuth settings
final Properties oAuthProperties = new Properties();
try {
    oAuthProperties.load(App.class.getResourceAsStream("oAuth.properties"));
} catch (IOException e) {
    System.out.println("Unable to read OAuth configuration. Make sure you have a properly formatted oAuth.properties file. See README for details.");
    return;
}

final String appId = oAuthProperties.getProperty("app.id");
final String[] appScopes = oAuthProperties.getProperty("app.scopes").split(",");
```

## <a name="implement-sign-in"></a><span data-ttu-id="99be5-110">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="99be5-110">Implement sign-in</span></span>

<span data-ttu-id="99be5-111">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/com/contoso** chamado **Authentication. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="99be5-111">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Authentication.java** and add the following code.</span></span>

```java
package com.contoso;
import java.net.MalformedURLException;
import java.util.Set;
import java.util.function.Consumer;

import com.microsoft.aad.msal4j.DeviceCode;
import com.microsoft.aad.msal4j.DeviceCodeFlowParameters;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.PublicClientApplication;

/**
 * Authentication
 */
public class Authentication {

    private static String applicationId;
    // Set authority to allow only organizational accounts
    // Device code flow only supports organizational accounts
    private final static String authority = "https://login.microsoftonline.com/organizations/";

    public static void initialize(String applicationId) {
        Authentication.applicationId = applicationId;
    }

    public static String getUserAccessToken(String[] scopes) {
        if (applicationId == null) {
            System.out.println("You must initialize Authentication before calling getUserAccessToken");
            return null;
        }

        Set<String> scopeSet = Set.of(scopes);

        PublicClientApplication app;
        try {
            // Build the MSAL application object with
            // app ID and authority
            app = PublicClientApplication.builder(applicationId)
                .authority(authority)
                .build();
        } catch (MalformedURLException e) {
            return null;
        }

        // Create consumer to receive the DeviceCode object
        // This method gets executed during the flow and provides
        // the URL the user logs into and the device code to enter
        Consumer<DeviceCode> deviceCodeConsumer = (DeviceCode deviceCode) -> {
            // Print the login information to the console
            System.out.println(deviceCode.message());
        };

        // Request a token, passing the requested permission scopes
        IAuthenticationResult result = app.acquireToken(
            DeviceCodeFlowParameters
                .builder(scopeSet, deviceCodeConsumer)
                .build()
        ).exceptionally(ex -> {
            System.out.println("Unable to authenticate - " + ex.getMessage());
            return null;
        }).join();

        if (result != null) {
            return result.accessToken();
        }

        return null;
    }
}
```

<span data-ttu-id="99be5-112">Em **app. java**, adicione o seguinte código antes da `Scanner input = new Scanner(System.in);` linha para obter um token de acesso.</span><span class="sxs-lookup"><span data-stu-id="99be5-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

<span data-ttu-id="99be5-113">Em seguida, adicione a seguinte linha `// Display access token` após o comentário.</span><span class="sxs-lookup"><span data-stu-id="99be5-113">Then add the following line after the `// Display access token` comment.</span></span>

```java
System.out.println("Access token: " + accessToken);
```

<span data-ttu-id="99be5-114">Criar e executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="99be5-114">Build and run the app.</span></span> <span data-ttu-id="99be5-115">O aplicativo exibe uma URL e um código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="99be5-115">The application displays a URL and device code.</span></span>

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

<span data-ttu-id="99be5-116">Abra um navegador e navegue até a URL exibida.</span><span class="sxs-lookup"><span data-stu-id="99be5-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="99be5-117">Insira o código fornecido e entre.</span><span class="sxs-lookup"><span data-stu-id="99be5-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="99be5-118">Depois de concluído, volte para o aplicativo e escolha o **1. Exibir** opção de token de acesso para exibir o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="99be5-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

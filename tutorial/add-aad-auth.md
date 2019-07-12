---
ms.openlocfilehash: 44426ee29265e8730ea3bc19f21091aa0180fd6e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634742"
---
<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) no aplicativo.

Crie um novo arquivo no diretório **./graphtutorial/src/main/java/com/contoso** chamado **OAuth. Properties**. Adicione o seguinte texto nesse arquivo.

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.

> [!IMPORTANT]
> Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir o arquivo **OAuth. Properties** do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.

Abra **app. java** e adicione as instruções `import` a seguir.

```java
import java.io.IOException;
import java.util.Properties;
```

Em seguida, adicione o código a seguir `Scanner input = new Scanner(System.in);` antes da linha carregar o arquivo **OAuth. Properties** .

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

## <a name="implement-sign-in"></a>Implementar logon

Crie um novo arquivo no diretório **./graphtutorial/src/main/java/com/contoso** chamado **Authentication. java** e adicione o código a seguir.

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

Em **app. java**, adicione o seguinte código antes da `Scanner input = new Scanner(System.in);` linha para obter um token de acesso.

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

Em seguida, adicione a seguinte linha `// Display access token` após o comentário.

```java
System.out.println("Access token: " + accessToken);
```

Criar e executar o aplicativo. O aplicativo exibe uma URL e um código de dispositivo.

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

Abra um navegador e navegue até a URL exibida. Insira o código fornecido e entre. Depois de concluído, volte para o aplicativo e escolha o **1. Exibir** opção de token de acesso para exibir o token de acesso.

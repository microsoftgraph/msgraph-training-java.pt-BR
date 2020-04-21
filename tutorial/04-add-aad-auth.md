---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612058"
---
<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) no aplicativo.

1. Crie um novo diretório chamado **graphtutorial** no diretório **./src/main/resources** .

1. Crie um novo arquivo no diretório **./src/main/resources/graphtutorial** chamado **OAuth. Properties**e adicione o seguinte texto nesse arquivo.

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.

    > [!IMPORTANT]
    > Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir o arquivo **OAuth. Properties** do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.

1. Abra **app. java** e adicione as instruções `import` a seguir.

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. Adicione o código a seguir antes da `Scanner input = new Scanner(System.in);` linha carregar o arquivo **OAuth. Properties** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>Implementar logon

1. Crie um novo arquivo no diretório **./graphtutorial/src/main/java/graphtutorial** chamado **Authentication. java** e adicione o código a seguir.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. Em **app. java**, adicione o seguinte código antes da `Scanner input = new Scanner(System.in);` linha para obter um token de acesso.

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
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

1. Abra um navegador e navegue até a URL exibida. Insira o código fornecido e entre. Depois de concluído, volte para o aplicativo e escolha o **1. Exibir** opção de token de acesso para exibir o token de acesso.

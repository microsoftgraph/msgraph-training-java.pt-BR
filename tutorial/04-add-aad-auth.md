---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612058"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="dee46-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dee46-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="dee46-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="dee46-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="dee46-103">Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="dee46-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="dee46-104">Crie um novo diretório chamado **graphtutorial** no diretório **./src/main/resources** .</span><span class="sxs-lookup"><span data-stu-id="dee46-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="dee46-105">Crie um novo arquivo no diretório **./src/main/resources/graphtutorial** chamado **OAuth. Properties**e adicione o seguinte texto nesse arquivo.</span><span class="sxs-lookup"><span data-stu-id="dee46-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="dee46-106">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="dee46-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="dee46-107">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir o arquivo **OAuth. Properties** do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="dee46-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="dee46-108">Abra **app. java** e adicione as instruções `import` a seguir.</span><span class="sxs-lookup"><span data-stu-id="dee46-108">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="dee46-109">Adicione o código a seguir antes da `Scanner input = new Scanner(System.in);` linha carregar o arquivo **OAuth. Properties** .</span><span class="sxs-lookup"><span data-stu-id="dee46-109">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="dee46-110">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="dee46-110">Implement sign-in</span></span>

1. <span data-ttu-id="dee46-111">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/graphtutorial** chamado **Authentication. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="dee46-111">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="dee46-112">Em **app. java**, adicione o seguinte código antes da `Scanner input = new Scanner(System.in);` linha para obter um token de acesso.</span><span class="sxs-lookup"><span data-stu-id="dee46-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="dee46-113">Adicione a seguinte linha após o `// Display access token` comentário.</span><span class="sxs-lookup"><span data-stu-id="dee46-113">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="dee46-114">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="dee46-114">Run the app.</span></span> <span data-ttu-id="dee46-115">O aplicativo exibe uma URL e um código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="dee46-115">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="dee46-116">Abra um navegador e navegue até a URL exibida.</span><span class="sxs-lookup"><span data-stu-id="dee46-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="dee46-117">Insira o código fornecido e entre.</span><span class="sxs-lookup"><span data-stu-id="dee46-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="dee46-118">Depois de concluído, volte para o aplicativo e escolha o **1. Exibir** opção de token de acesso para exibir o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="dee46-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

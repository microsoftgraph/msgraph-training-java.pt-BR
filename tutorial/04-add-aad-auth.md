---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661064"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ea93b-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ea93b-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="ea93b-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ea93b-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="ea93b-103">Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ea93b-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="ea93b-104">Crie um novo diretório chamado **graphtutorial** no diretório **./src/main/resources** .</span><span class="sxs-lookup"><span data-stu-id="ea93b-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="ea93b-105">Crie um novo arquivo no diretório **./src/main/resources/graphtutorial** chamado **OAuth. Properties** e adicione o seguinte texto nesse arquivo.</span><span class="sxs-lookup"><span data-stu-id="ea93b-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="ea93b-106">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="ea93b-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="ea93b-107">O valor de `app.scopes` contém os escopos de permissão que o aplicativo exige.</span><span class="sxs-lookup"><span data-stu-id="ea93b-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="ea93b-108">**User. Read** permite que o aplicativo acesse o perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="ea93b-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="ea93b-109">**MailboxSettings. Read** permite que o aplicativo acesse as configurações da caixa de correio do usuário, incluindo o fuso horário configurado do usuário.</span><span class="sxs-lookup"><span data-stu-id="ea93b-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="ea93b-110">**Calendars. ReadWrite** permite que o aplicativo liste o calendário do usuário e adicione novos eventos ao calendário.</span><span class="sxs-lookup"><span data-stu-id="ea93b-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ea93b-111">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir o arquivo **OAuth. Properties** do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ea93b-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="ea93b-112">Abra **app. java** e adicione as instruções a seguir `import` .</span><span class="sxs-lookup"><span data-stu-id="ea93b-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="ea93b-113">Adicione o código a seguir antes da `Scanner input = new Scanner(System.in);` linha carregar o arquivo **OAuth. Properties** .</span><span class="sxs-lookup"><span data-stu-id="ea93b-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="ea93b-114">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="ea93b-114">Implement sign-in</span></span>

1. <span data-ttu-id="ea93b-115">Crie um novo arquivo no diretório **./graphtutorial/src/main/java/graphtutorial** chamado **Authentication. java** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ea93b-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="ea93b-116">Em **app. java**, adicione o seguinte código antes da `Scanner input = new Scanner(System.in);` linha para obter um token de acesso.</span><span class="sxs-lookup"><span data-stu-id="ea93b-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="ea93b-117">Adicione a seguinte linha após o `// Display access token` comentário.</span><span class="sxs-lookup"><span data-stu-id="ea93b-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="ea93b-118">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ea93b-118">Run the app.</span></span> <span data-ttu-id="ea93b-119">O aplicativo exibe uma URL e um código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="ea93b-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="ea93b-120">Abra um navegador e navegue até a URL exibida.</span><span class="sxs-lookup"><span data-stu-id="ea93b-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="ea93b-121">Insira o código fornecido e entre.</span><span class="sxs-lookup"><span data-stu-id="ea93b-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="ea93b-122">Depois de concluído, volte para o aplicativo e escolha o **1. Exibir** opção de token de acesso para exibir o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="ea93b-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="ea93b-123">Tokens de acesso para contas corporativas ou de estudante da Microsoft podem ser analisados para fins de solução de problemas em [https://jwt.ms](https://jwt.ms) .</span><span class="sxs-lookup"><span data-stu-id="ea93b-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="ea93b-124">Tokens de acesso para contas pessoais da Microsoft usam um formato proprietário e não podem ser analisados.</span><span class="sxs-lookup"><span data-stu-id="ea93b-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>

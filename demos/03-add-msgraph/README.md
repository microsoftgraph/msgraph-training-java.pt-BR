---
ms.openlocfilehash: a890c296f4269b674356118463578c9b79001b0e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634714"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="3c873-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="3c873-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c873-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="3c873-102">Prerequisites</span></span>

<span data-ttu-id="3c873-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="3c873-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="3c873-104">[Java se Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) e [Maven](https://maven.apache.org/) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="3c873-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="3c873-105">Se você não tiver o JDK ou o Maven, visite os links anteriores para opções de download.</span><span class="sxs-lookup"><span data-stu-id="3c873-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span> <span data-ttu-id="3c873-106">(**Observação:** este tutorial foi escrito com o OpenJDK versão 12.0.1 e o Maven 3.6.1.</span><span class="sxs-lookup"><span data-stu-id="3c873-106">(**Note:** This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="3c873-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="3c873-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="3c873-108">Uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3c873-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="3c873-109">Se você não tiver uma conta da Microsoft, poderá [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="3c873-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="3c873-110">Registrar um aplicativo Web com o centro de administração do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3c873-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="3c873-111">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3c873-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="3c873-112">Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="3c873-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="3c873-113">Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="3c873-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="3c873-114">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="3c873-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="3c873-115">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="3c873-115">Select **New registration**.</span></span> <span data-ttu-id="3c873-116">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="3c873-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="3c873-117">Defina **Nome** para `Java Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="3c873-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="3c873-118">Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional**.</span><span class="sxs-lookup"><span data-stu-id="3c873-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="3c873-119">Deixe o **URI de Redirecionamento** vazio.</span><span class="sxs-lookup"><span data-stu-id="3c873-119">Leave **Redirect URI** empty.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="3c873-121">Escolha **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="3c873-121">Choose **Register**.</span></span> <span data-ttu-id="3c873-122">Na página **tutorial do Java Graph** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="3c873-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="3c873-124">Selecione o link **Adicionar um URI** de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="3c873-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="3c873-125">Na página **redirecionar URIs** , localize a seção redirecionar **URIs sugeridos para clientes públicos (móvel, área de trabalho)** .</span><span class="sxs-lookup"><span data-stu-id="3c873-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="3c873-126">Selecione o `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="3c873-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Captura de tela da página URIs de redirecionamento](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="3c873-128">Localize a seção **tipo de cliente padrão** e altere o **aplicativo tratar como um cliente público** alternar para **Sim**e, em seguida, escolha **salvar**.</span><span class="sxs-lookup"><span data-stu-id="3c873-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Uma captura de tela da seção tipo de cliente padrão](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="3c873-130">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="3c873-130">Configure the sample</span></span>

1. <span data-ttu-id="3c873-131">Renomear `oAuth.properties.example` o `oAuth.properties`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="3c873-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="3c873-132">Edite `oAuth.properties` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="3c873-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="3c873-133">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3c873-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="3c873-134">Criar e executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="3c873-134">Build and run the sample</span></span>

<span data-ttu-id="3c873-135">Na sua interface de linha de comando (CLI), navegue até o diretório do projeto e execute os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="3c873-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

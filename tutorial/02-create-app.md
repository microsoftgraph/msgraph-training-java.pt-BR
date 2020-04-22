---
ms.openlocfilehash: 381e4166f07e1dbc51c072645f17002e43f6cc16
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612016"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="95caf-101">Nesta seção, você criará um aplicativo de console Java básico.</span><span class="sxs-lookup"><span data-stu-id="95caf-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="95caf-102">Abra a interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="95caf-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="95caf-103">Execute o seguinte comando para criar um novo projeto do gradle.</span><span class="sxs-lookup"><span data-stu-id="95caf-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="95caf-104">Depois que o projeto for criado, verifique se ele funciona executando o seguinte comando para executar o aplicativo em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="95caf-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="95caf-105">Se funcionar, o aplicativo deve gerar saída `Hello World.`.</span><span class="sxs-lookup"><span data-stu-id="95caf-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="95caf-106">Instalar dependências</span><span class="sxs-lookup"><span data-stu-id="95caf-106">Install dependencies</span></span>

<span data-ttu-id="95caf-107">Antes de prosseguir, adicione algumas dependências adicionais que você usará posteriormente.</span><span class="sxs-lookup"><span data-stu-id="95caf-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="95caf-108">[Biblioteca de autenticação da Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) para autenticar o usuário e adquirir tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="95caf-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="95caf-109">[SDK do Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="95caf-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="95caf-110">[SLF4J Nop Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) para suprimir o registro em log do MSAL.</span><span class="sxs-lookup"><span data-stu-id="95caf-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="95caf-111">Abra **./Build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="95caf-111">Open **./build.gradle**.</span></span> <span data-ttu-id="95caf-112">Atualize a `dependencies` seção para adicionar essas dependências.</span><span class="sxs-lookup"><span data-stu-id="95caf-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="95caf-113">Adicione o seguinte ao final de **./Build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="95caf-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="95caf-114">Na próxima vez que você criar o projeto, o gradle baixará essas dependências.</span><span class="sxs-lookup"><span data-stu-id="95caf-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="95caf-115">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="95caf-115">Design the app</span></span>

1. <span data-ttu-id="95caf-116">Abra o arquivo **./src/main/java/graphtutorial/app.java** e substitua o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="95caf-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

    ```java
    package graphtutorial;

    import java.util.InputMismatchException;
    import java.util.Scanner;

    /**
     * Graph Tutorial
     *
     */
    public class App {
        public static void main(String[] args) {
            System.out.println("Java Graph Tutorial");
            System.out.println();

            Scanner input = new Scanner(System.in);

            int choice = -1;

            while (choice != 0) {
                System.out.println("Please choose one of the following options:");
                System.out.println("0. Exit");
                System.out.println("1. Display access token");
                System.out.println("2. List calendar events");

                try {
                    choice = input.nextInt();
                } catch (InputMismatchException ex) {
                    // Skip over non-integer input
                    input.nextLine();
                }

                // Process user choice
                switch(choice) {
                    case 0:
                        // Exit the program
                        System.out.println("Goodbye...");
                        break;
                    case 1:
                        // Display access token
                        break;
                    case 2:
                        // List the calendar
                        break;
                    default:
                        System.out.println("Invalid choice");
                }
            }

            input.close();
        }
    }
    ```

    <span data-ttu-id="95caf-117">Isso implementa um menu básico e lê a opção do usuário na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="95caf-117">This implements a basic menu and reads the user's choice from the command line.</span></span>

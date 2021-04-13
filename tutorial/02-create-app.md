---
ms.openlocfilehash: b80de156a5ed1708ccafbaabf34b49b119f4099c
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695797"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="93baa-101">Nesta seção, você criará um aplicativo básico Java console.</span><span class="sxs-lookup"><span data-stu-id="93baa-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="93baa-102">Abra sua interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="93baa-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="93baa-103">Execute o seguinte comando para criar um novo projeto Gradle.</span><span class="sxs-lookup"><span data-stu-id="93baa-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="93baa-104">Depois que o projeto for criado, verifique se ele funciona executando o seguinte comando para executar o aplicativo em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="93baa-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="93baa-105">Se funcionar, o aplicativo deverá ter `Hello World.` saída .</span><span class="sxs-lookup"><span data-stu-id="93baa-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="93baa-106">Instalar dependências</span><span class="sxs-lookup"><span data-stu-id="93baa-106">Install dependencies</span></span>

<span data-ttu-id="93baa-107">Antes de continuar, adicione algumas dependências adicionais que você usará mais tarde.</span><span class="sxs-lookup"><span data-stu-id="93baa-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="93baa-108">[Biblioteca de clientes do Azure Identity para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) autenticar o usuário e adquirir tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="93baa-108">[Azure Identity client library for Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="93baa-109">[Microsoft Graph SDK para Java](https://github.com/microsoftgraph/msgraph-sdk-java) fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="93baa-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="93baa-110">Abra **./build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="93baa-110">Open **./build.gradle**.</span></span> <span data-ttu-id="93baa-111">Atualize `dependencies` a seção para adicionar essas dependências.</span><span class="sxs-lookup"><span data-stu-id="93baa-111">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-8":::

1. <span data-ttu-id="93baa-112">Adicione o seguinte ao final de **./build.gradle**.</span><span class="sxs-lookup"><span data-stu-id="93baa-112">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="93baa-113">Na próxima vez que você criar o projeto, Gradle baixará essas dependências.</span><span class="sxs-lookup"><span data-stu-id="93baa-113">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="93baa-114">Design do aplicativo</span><span class="sxs-lookup"><span data-stu-id="93baa-114">Design the app</span></span>

1. <span data-ttu-id="93baa-115">Abra o **arquivo ./src/main/java/graphtutorial/App.java** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="93baa-115">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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
                System.out.println("2. View this week's calendar");
                System.out.println("3. Add an event");

                try {
                    choice = input.nextInt();
                } catch (InputMismatchException ex) {
                    // Skip over non-integer input
                }

                input.nextLine();

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
                    case 3:
                        // Create a new event
                        break;
                    default:
                        System.out.println("Invalid choice");
                }
            }

            input.close();
        }
    }
    ```

    <span data-ttu-id="93baa-116">Isso implementa um menu básico e lê a escolha do usuário na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="93baa-116">This implements a basic menu and reads the user's choice from the command line.</span></span>

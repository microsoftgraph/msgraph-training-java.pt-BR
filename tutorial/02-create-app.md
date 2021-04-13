---
ms.openlocfilehash: b80de156a5ed1708ccafbaabf34b49b119f4099c
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695797"
---
<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você criará um aplicativo básico Java console.

1. Abra sua interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto. Execute o seguinte comando para criar um novo projeto Gradle.

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. Depois que o projeto for criado, verifique se ele funciona executando o seguinte comando para executar o aplicativo em sua CLI.

    ```Shell
    ./gradlew --console plain run
    ```

    Se funcionar, o aplicativo deverá ter `Hello World.` saída .

## <a name="install-dependencies"></a>Instalar dependências

Antes de continuar, adicione algumas dependências adicionais que você usará mais tarde.

- [Biblioteca de clientes do Azure Identity para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) autenticar o usuário e adquirir tokens de acesso.
- [Microsoft Graph SDK para Java](https://github.com/microsoftgraph/msgraph-sdk-java) fazer chamadas para o Microsoft Graph.

1. Abra **./build.gradle**. Atualize `dependencies` a seção para adicionar essas dependências.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-8":::

1. Adicione o seguinte ao final de **./build.gradle**.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

Na próxima vez que você criar o projeto, Gradle baixará essas dependências.

## <a name="design-the-app"></a>Design do aplicativo

1. Abra o **arquivo ./src/main/java/graphtutorial/App.java** e substitua seu conteúdo pelo seguinte.

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

    Isso implementa um menu básico e lê a escolha do usuário na linha de comando.

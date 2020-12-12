---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661078"
---
<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você criará um aplicativo de console Java básico.

1. Abra a interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto. Execute o seguinte comando para criar um novo projeto do gradle.

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. Depois que o projeto for criado, verifique se ele funciona executando o seguinte comando para executar o aplicativo em sua CLI.

    ```Shell
    ./gradlew --console plain run
    ```

    Se funcionar, o aplicativo deve gerar saída `Hello World.` .

## <a name="install-dependencies"></a>Instalar dependências

Antes de prosseguir, adicione algumas dependências adicionais que você usará posteriormente.

- [Biblioteca de autenticação da Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) para autenticar o usuário e adquirir tokens de acesso.
- [SDK do Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para fazer chamadas para o Microsoft Graph.
- [SLF4J Nop Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) para suprimir o registro em log do MSAL.

1. Abra **./Build.gradle**. Atualize a `dependencies` seção para adicionar essas dependências.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. Adicione o seguinte ao final de **./Build.gradle**.

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

Na próxima vez que você criar o projeto, o gradle baixará essas dependências.

## <a name="design-the-app"></a>Projetar o aplicativo

1. Abra o arquivo **./src/main/java/graphtutorial/app.java** e substitua o conteúdo pelo seguinte.

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

    Isso implementa um menu básico e lê a opção do usuário na linha de comando.

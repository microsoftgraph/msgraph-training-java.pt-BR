---
ms.openlocfilehash: 298c78d0626e16899de402b3e9b537c8b8386886
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018778"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="048eb-101">Abra a interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="048eb-101">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="048eb-102">Execute o seguinte comando para criar um novo projeto Maven.</span><span class="sxs-lookup"><span data-stu-id="048eb-102">Run the following command to create a new Maven project.</span></span>

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> <span data-ttu-id="048eb-103">Você pode inserir valores diferentes para a ID do grupo`DgroupId` (parâmetro) e ID do`DartifactId` artefato (parâmetro) do que os valores especificados acima.</span><span class="sxs-lookup"><span data-stu-id="048eb-103">You can enter different values for the group ID (`DgroupId` parameter) and artifact ID (`DartifactId` parameter) than the values specified above.</span></span> <span data-ttu-id="048eb-104">O código de exemplo neste tutorial pressupõe que a ID `com.contoso` do grupo foi usada.</span><span class="sxs-lookup"><span data-stu-id="048eb-104">The sample code in this tutorial assumes that the group ID `com.contoso` was used.</span></span> <span data-ttu-id="048eb-105">Se você usar um valor diferente, certifique-se de `com.contoso` substituir qualquer código de exemplo pela ID do grupo.</span><span class="sxs-lookup"><span data-stu-id="048eb-105">If you use a different value, be sure to replace `com.contoso` in any sample code with your group ID.</span></span>

<span data-ttu-id="048eb-106">Quando solicitado, confirme a configuração e espere o projeto seja criado.</span><span class="sxs-lookup"><span data-stu-id="048eb-106">When prompted, confirm the configuration, then wait for the project to be created.</span></span> <span data-ttu-id="048eb-107">Depois que o projeto for criado, verifique se ele funciona executando os seguintes comandos para empacotar e executar o aplicativo em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="048eb-107">Once the project is created, verify that it works by running the following commands to package and run the app in your CLI.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

<span data-ttu-id="048eb-108">Se funcionar, o aplicativo deve gerar saída `Hello World!`.</span><span class="sxs-lookup"><span data-stu-id="048eb-108">If it works, the app should output `Hello World!`.</span></span> <span data-ttu-id="048eb-109">Antes de prosseguir, adicione algumas dependências adicionais que você usará posteriormente.</span><span class="sxs-lookup"><span data-stu-id="048eb-109">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="048eb-110">[Biblioteca de autenticação da Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) para autenticar o usuário e adquirir tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="048eb-110">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="048eb-111">[SDK do Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="048eb-111">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="048eb-112">[SLF4J Nop Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) para suprimir o registro em log do MSAL.</span><span class="sxs-lookup"><span data-stu-id="048eb-112">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

<span data-ttu-id="048eb-113">Abra **./graphtutorial/pom.xml**.</span><span class="sxs-lookup"><span data-stu-id="048eb-113">Open **./graphtutorial/pom.xml**.</span></span> <span data-ttu-id="048eb-114">Adicione o seguinte no `<dependencies>` elemento.</span><span class="sxs-lookup"><span data-stu-id="048eb-114">Add the following inside the `<dependencies>` element.</span></span>

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-nop</artifactId>
  <version>1.8.0-beta4</version>
</dependency>

<dependency>
  <groupId>com.microsoft.graph</groupId>
  <artifactId>microsoft-graph</artifactId>
  <version>1.6.0</version>
</dependency>

<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>msal4j</artifactId>
  <version>1.1.0</version>
</dependency>
```

<span data-ttu-id="048eb-115">Na próxima vez que você criar o projeto, o Maven baixará essas dependências.</span><span class="sxs-lookup"><span data-stu-id="048eb-115">The next time you build the project, Maven will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="048eb-116">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="048eb-116">Design the app</span></span>

<span data-ttu-id="048eb-117">Abra o arquivo **./graphtutorial/src/main/java/com/contoso/app.java** e substitua o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="048eb-117">Open the **./graphtutorial/src/main/java/com/contoso/App.java** file and replace its contents with the following.</span></span>

```java
package com.contoso;

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

<span data-ttu-id="048eb-118">Isso implementa um menu básico e lê a opção do usuário na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="048eb-118">This implements a basic menu and reads the user's choice from the command line.</span></span>

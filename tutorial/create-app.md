---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634756"
---
<!-- markdownlint-disable MD002 MD041 -->

Abra a interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto. Execute o seguinte comando para criar um novo projeto Maven.

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> Você pode inserir valores diferentes para a ID do grupo`DgroupId` (parâmetro) e ID do`DartifactId` artefato (parâmetro) do que os valores especificados acima. O código de exemplo neste tutorial pressupõe que a ID `com.contoso` do grupo foi usada. Se você usar um valor diferente, certifique-se de `com.contoso` substituir qualquer código de exemplo pela ID do grupo.

Quando solicitado, confirme a configuração e espere o projeto seja criado. Depois que o projeto for criado, verifique se ele funciona executando os seguintes comandos para empacotar e executar o aplicativo em sua CLI.

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

Se funcionar, o aplicativo deve gerar saída `Hello World!`. Antes de prosseguir, adicione algumas dependências adicionais que você usará posteriormente.

- [Biblioteca de autenticação da Microsoft (MSAL) para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) para autenticar o usuário e adquirir tokens de acesso.
- [SDK do Microsoft Graph para Java](https://github.com/microsoftgraph/msgraph-sdk-java) para fazer chamadas para o Microsoft Graph.
- [SLF4J Nop Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) para suprimir o registro em log do MSAL.

Abra **./graphtutorial/pom.xml**. Adicione o seguinte no `<dependencies>` elemento.

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-nop</artifactId>
  <version>1.8.0-beta4</version>
</dependency>

<dependency>
  <groupId>com.microsoft.graph</groupId>
  <artifactId>microsoft-graph</artifactId>
  <version>1.4.0</version>
</dependency>

<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>msal4j</artifactId>
  <version>0.4.0-preview</version>
</dependency>
```

Na próxima vez que você criar o projeto, o Maven baixará essas dependências.

## <a name="design-the-app"></a>Projetar o aplicativo

Abra o arquivo **./graphtutorial/src/main/java/com/contoso/app.java** e substitua o conteúdo pelo seguinte.

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

Isso implementa um menu básico e lê a opção do usuário na linha de comando.

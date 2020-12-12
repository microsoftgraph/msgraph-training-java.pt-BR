---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661096"
---
<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.

1. Abra **./graphtutorial/src/main/java/graphtutorial/Graph.java** e adicione a função a seguir à classe do **gráfico** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. Abra **./graphtutorial/src/main/java/graphtutorial/app.java** e adicione a função a seguir à classe de **aplicativo** .

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    Essa função solicita ao usuário assunto, participantes, início, fim e corpo e, em seguida, usa esses valores para chamar `Graph.createEvent` .

1. Adicione o seguinte logo após o `// Create a new event` comentário na `Main` função.

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. Salve todas as suas alterações e execute o aplicativo. Escolha a opção **Adicionar um evento** . Responda aos prompts para criar um novo evento no calendário do usuário.

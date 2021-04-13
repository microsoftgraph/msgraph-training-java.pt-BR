---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695783"
---
<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.

1. Abra **./graphtutorial/src/main/java/graphtutorial/Graph.java** e adicione a seguinte função à classe **Graph.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. Abra **./graphtutorial/src/main/java/graphtutorial/App.java** e adicione a seguinte função à classe **App.**

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    Essa função solicita ao usuário assunto, participantes, início, fim e corpo, em seguida, usa esses valores para chamar `Graph.createEvent` .

1. Adicione o seguinte logo após `// Create a new event` o comentário na `Main` função.

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. Salve todas as alterações e execute o aplicativo. Escolha a **opção Adicionar um** evento. Responda aos prompts para criar um novo evento no calendário do usuário.

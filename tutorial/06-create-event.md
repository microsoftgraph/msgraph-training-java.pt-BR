---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661096"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a6f7b-101">Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="a6f7b-102">Abra **./graphtutorial/src/main/java/graphtutorial/Graph.java** e adicione a função a seguir à classe do **gráfico** .</span><span class="sxs-lookup"><span data-stu-id="a6f7b-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="a6f7b-103">Abra **./graphtutorial/src/main/java/graphtutorial/app.java** e adicione a função a seguir à classe de **aplicativo** .</span><span class="sxs-lookup"><span data-stu-id="a6f7b-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="a6f7b-104">Essa função solicita ao usuário assunto, participantes, início, fim e corpo e, em seguida, usa esses valores para chamar `Graph.createEvent` .</span><span class="sxs-lookup"><span data-stu-id="a6f7b-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="a6f7b-105">Adicione o seguinte logo após o `// Create a new event` comentário na `Main` função.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="a6f7b-106">Salve todas as suas alterações e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="a6f7b-107">Escolha a opção **Adicionar um evento** .</span><span class="sxs-lookup"><span data-stu-id="a6f7b-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="a6f7b-108">Responda aos prompts para criar um novo evento no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="a6f7b-108">Respond to the prompts to create a new event on the user's calendar.</span></span>

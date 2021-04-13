---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695783"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="468f2-101">Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="468f2-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="468f2-102">Abra **./graphtutorial/src/main/java/graphtutorial/Graph.java** e adicione a seguinte função à classe **Graph.**</span><span class="sxs-lookup"><span data-stu-id="468f2-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="468f2-103">Abra **./graphtutorial/src/main/java/graphtutorial/App.java** e adicione a seguinte função à classe **App.**</span><span class="sxs-lookup"><span data-stu-id="468f2-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="468f2-104">Essa função solicita ao usuário assunto, participantes, início, fim e corpo, em seguida, usa esses valores para chamar `Graph.createEvent` .</span><span class="sxs-lookup"><span data-stu-id="468f2-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="468f2-105">Adicione o seguinte logo após `// Create a new event` o comentário na `Main` função.</span><span class="sxs-lookup"><span data-stu-id="468f2-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="468f2-106">Salve todas as alterações e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="468f2-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="468f2-107">Escolha a **opção Adicionar um** evento.</span><span class="sxs-lookup"><span data-stu-id="468f2-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="468f2-108">Responda aos prompts para criar um novo evento no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="468f2-108">Respond to the prompts to create a new event on the user's calendar.</span></span>

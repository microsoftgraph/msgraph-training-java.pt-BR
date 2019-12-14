---
ms.openlocfilehash: 3eec5a727d21e481525e2a2892e33a0a30a9bb25
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018779"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c2dd9-101">Este tutorial ensina como criar um aplicativo de console Java que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.</span><span class="sxs-lookup"><span data-stu-id="c2dd9-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="c2dd9-102">Se preferir baixar o tutorial concluído, você poderá baixar ou clonar o repositório do [GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="c2dd9-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2dd9-103">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c2dd9-103">Prerequisites</span></span>

<span data-ttu-id="c2dd9-104">Antes de iniciar este tutorial, você deve ter o [Java se Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) e o [Maven](https://maven.apache.org/) instalados em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="c2dd9-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="c2dd9-105">Se você não tiver o JDK ou o Maven, visite os links anteriores para opções de download.</span><span class="sxs-lookup"><span data-stu-id="c2dd9-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="c2dd9-106">Este tutorial foi escrito com o OpenJDK versão 12.0.1 e o Maven 3.6.1.</span><span class="sxs-lookup"><span data-stu-id="c2dd9-106">This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="c2dd9-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="c2dd9-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="c2dd9-108">Feedback</span><span class="sxs-lookup"><span data-stu-id="c2dd9-108">Feedback</span></span>

<span data-ttu-id="c2dd9-109">Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="c2dd9-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

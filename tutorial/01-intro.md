---
ms.openlocfilehash: f77f0518aa22d015cf5e915886f8f24a487e7bab
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612072"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="54938-101">Este tutorial ensina como criar um aplicativo de console Java que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.</span><span class="sxs-lookup"><span data-stu-id="54938-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="54938-102">Se preferir baixar o tutorial concluído, você poderá baixar ou clonar o repositório do [GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="54938-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54938-103">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="54938-103">Prerequisites</span></span>

<span data-ttu-id="54938-104">Antes de iniciar este tutorial, você deve ter o [Java se Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) e o [gradle](https://gradle.org/) instalados em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="54938-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="54938-105">Se você não tiver o JDK ou o Maven, visite os links anteriores para opções de download.</span><span class="sxs-lookup"><span data-stu-id="54938-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

<span data-ttu-id="54938-106">Você também deve ter uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="54938-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="54938-107">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="54938-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="54938-108">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="54938-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="54938-109">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="54938-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="54938-110">Este tutorial foi escrito com o OpenJDK versão 14.0.0.36 e o gradle 6,3.</span><span class="sxs-lookup"><span data-stu-id="54938-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.3.</span></span> <span data-ttu-id="54938-111">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="54938-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="54938-112">Comentários</span><span class="sxs-lookup"><span data-stu-id="54938-112">Feedback</span></span>

<span data-ttu-id="54938-113">Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-java).</span><span class="sxs-lookup"><span data-stu-id="54938-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

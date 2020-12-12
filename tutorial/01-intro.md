---
ms.openlocfilehash: 3b1e366ded1e9d59f048eec8da23a686a121dedf
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661071"
---
<!-- markdownlint-disable MD002 MD041 -->

Este tutorial ensina como criar um aplicativo de console Java que usa a API do Microsoft Graph para recuperar informações de calendário de um usuário.

> [!TIP]
> Se preferir baixar o tutorial concluído, você poderá baixar ou clonar o repositório do [GitHub](https://github.com/microsoftgraph/msgraph-training-java).

## <a name="prerequisites"></a>Pré-requisitos

Antes de iniciar este tutorial, você deve ter o [Java se Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) e o [gradle](https://gradle.org/) instalados em sua máquina de desenvolvimento. Se você não tiver o JDK ou o gradle, visite os links anteriores para obter opções de download.

Você também deve ter uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft. Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:

- Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.

> [!NOTE]
> Este tutorial foi escrito com OpenJDK versão 14.0.0.36 e gradle 6.7.1. As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.

## <a name="feedback"></a>Comentários

Forneça comentários sobre este tutorial no [repositório do GitHub](https://github.com/microsoftgraph/msgraph-training-java).

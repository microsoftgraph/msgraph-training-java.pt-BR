---
ms.openlocfilehash: 7118c8300b17adb66893324dd2f2269d9fb8d00b
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634700"
---
# <a name="completed-module-add-azure-ad-authentication"></a>Módulo concluído: Adicionar autenticação do Azure AD

A versão do projeto nesse diretório reflete a conclusão do tutorial para a [adição da autenticação do Azure ad](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=3). Se você usar esta versão do projeto, precisará concluir o restante do tutorial em [obter dados do calendário](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=4).

> **Observação:** Presume-se que você já tenha registrado um aplicativo no portal de registro de aplicativo conforme especificado em [registrar o aplicativo no portal](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=2). Você precisa configurar esta versão do exemplo da seguinte maneira:
>
> 1. Renomear `oAuth.properties.example` o `oAuth.properties`arquivo para.
> 1. Edite `oAuth.properties` o arquivo e faça as seguintes alterações.
>     1. Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.

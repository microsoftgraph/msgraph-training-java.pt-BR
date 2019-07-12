---
ms.openlocfilehash: a890c296f4269b674356118463578c9b79001b0e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634714"
---
# <a name="how-to-run-the-completed-project"></a>Como executar o projeto concluído

## <a name="prerequisites"></a>Pré-requisitos

Para executar o projeto concluído nessa pasta, você precisará do seguinte:

- [Java se Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) e [Maven](https://maven.apache.org/) instalado em sua máquina de desenvolvimento. Se você não tiver o JDK ou o Maven, visite os links anteriores para opções de download. (**Observação:** este tutorial foi escrito com o OpenJDK versão 12.0.1 e o Maven 3.6.1. As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.
- Uma conta corporativa ou de estudante da Microsoft.

Se você não tiver uma conta da Microsoft, poderá [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrar um aplicativo Web com o centro de administração do Azure Active Directory

1. Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com). Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.

1. Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo** em **gerenciar**.

    ![Uma captura de tela dos registros de aplicativo ](/tutorial/images/aad-portal-app-registrations.png)

1. Selecione **Novo registro**. Na página **Registrar um aplicativo**, defina os valores da seguinte forma.

    - Defina **Nome** para `Java Graph Tutorial`.
    - Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional**.
    - Deixe o **URI de Redirecionamento** vazio.

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. Escolha **Registrar**. Na página **tutorial do Java Graph** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. Selecione o link **Adicionar um URI** de redirecionamento. Na página **redirecionar URIs** , localize a seção redirecionar **URIs sugeridos para clientes públicos (móvel, área de trabalho)** . Selecione o `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.

    ![Captura de tela da página URIs de redirecionamento](/tutorial/images/aad-redirect-uris.png)

1. Localize a seção **tipo de cliente padrão** e altere o **aplicativo tratar como um cliente público** alternar para **Sim**e, em seguida, escolha **salvar**.

    ![Uma captura de tela da seção tipo de cliente padrão](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>Configurar o exemplo

1. Renomear `oAuth.properties.example` o `oAuth.properties`arquivo para.
1. Edite `oAuth.properties` o arquivo e faça as seguintes alterações.
    1. Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

Na sua interface de linha de comando (CLI), navegue até o diretório do projeto e execute os seguintes comandos.

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

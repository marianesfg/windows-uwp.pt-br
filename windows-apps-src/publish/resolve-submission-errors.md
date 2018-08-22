---
author: jnHs
Description: If you encounter errors after submitting your app to the Store, you must resolve them in order to continue the certification process.
title: Resolver erros de envio
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.author: wdg-dev-content
ms.date: 09/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9d027e35f8fe76a0d4139301f1a7dabc7798348a
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2790085"
---
# <a name="resolve-submission-errors"></a>Resolver erros de envio

Se você encontrar erros depois de enviar seu aplicativo para a Loja, deverá resolvê-los para continuar o [processo de certificação](the-app-certification-process.md). A mensagem de erro indicará qual é o problema e o que talvez você precise fazer para corrigi-lo. Aqui estão algumas informações adicionais que podem ajudar a resolver esses erros.

## <a name="uwp-apps"></a>Aplicativos UWP

Se estiver enviando um aplicativo UWP, você poderá ver um erro durante o pré-processamento se o arquivo do pacote não for um arquivo .appxupload gerado pelo Visual Studio para a Loja. Certifique-se de que você siga as etapas no [pacote de um aplicativo UWP com o Visual Studio](../packaging/packaging-uwp-apps.md) ao criar o arquivo de pacote do seu aplicativo e carregar somente o arquivo .appxupload na página de [pacotes](upload-app-packages.md) do envio, não um appx ou .appxbundle.

Caso um erro de compilação seja exibido, certifique-se de que você consiga compilar o seu aplicativo no modo Versão com êxito. Para obter mais informações, consulte [Erros do compilador interno nativo .NET](http://go.microsoft.com/fwlink/p/?LinkID=613098).

## <a name="desktop-application"></a>Aplicativo de área de trabalho

Se você planeja enviar um pacote que contém os binários Win32 e UWP, certifique-se de que você cria esse pacote usando-se o projeto de empacotamento do Windows que está disponível no Visual Studio 2017 atualização 4. Se você criar o pacote usando um modelo de projeto UWP, você não poderá enviar que pacote para o repositório ou sideload-lo em outros PCs. Mesmo se o pacote publica com êxito, talvez se comportam de maneiras inesperadas no PC do usuário. Para obter mais informações, consulte o [pacote de um aplicativo usando o Visual Studio (ponte de área de trabalho)]( https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net).

## <a name="windows-phone-apps"></a>Aplicativos do Windows Phone

Você poderá ver **erro 2001** quando os problemas com pacotes do Windows Phone forem detectados durante o pré-processamento. Na maioria dos casos, você precisará recompilar o pacote do aplicativo para corrigir o erro. Depois de terminar, substitua o pacote anterior pelo novo na página [Pacotes](upload-app-packages.md) do envio antes de clicar em **Enviar à Loja** novamente.

Existem alguns problemas que podem causar esse erro. Examine a lista abaixo para determinar quais podem se aplicar a seus pacotes.

-   **Um ou mais assemblies no pacote estão ofuscados incorretamente:** use uma ferramenta diferente para realizar a ofuscação ou removê-la. O processo de compilação otimiza os assemblies ofuscados, mas, às vezes, alguns assemblies são ofuscados usando uma ferramenta que modifica o MSIL de maneira não compatível, o que causará um erro.
-   **O tamanho de um ou mais métodos no aplicativo excede 256 KB de IL:** refatore o método ofensivo em funções menores. O tamanho de MSIL para métodos em um assembly pode ser determinado usando-se a ferramenta ILDASM.
-   **A validação de assinatura de nome forte falhou em um ou mais assemblies:** esse erro normalmente ocorre quando a assinatura de nome forte foi realizada usando-se uma chave diferente da esperada nos metadados do assembly. Assine usando a chave correta ou remova assinaturas de nome forte.
-   **O pacote contém assemblies de modo misto (com códigos gerenciado e nativo):** assemblies de modo misto não são compatíveis no Windows Phone. Remova os assemblies de modo misto do pacote e reenvie o aplicativo.
-   **Um XAP do Windows Phone 8.1 ou um assembly appx/appxbundle não é válido:** certifique-se de que o arquivo .winmd tenha pelo menos um ponto de entrada público. Você pode usar qualquer aplicativo descompilador para examinar o código e verificar se há pontos de entrada públicos, se necessário.

Outro erro que você poderá ver depois de enviar seu aplicativo é **erro 1300**. Isso ocorre quando um ou mais assemblies (ou todo o pacote) já estão pré-compilados. Para corrigir esse problema, recompile o pacote do aplicativo no Microsoft Visual Studio e, em seguida, envie o pacote recém-gerado.

## <a name="nameidentity-errors"></a>Erros de nome/identidade

Se você vir um erro que diz **The name found in the package is not one of your reserved app names. Please reserve the app name and/or update your package with the correct app name for this language**, talvez seja porque você inseriu um nome incorreto no seu pacote. Esse erro também pode ocorrer se você estiver usando um nome de aplicativo que ainda não tenha reservado no Centro de Desenvolvimento. Você geralmente pode resolver esse erro seguindo estas etapas:

- Vá até a página [Identidade do aplicativo](view-app-identity-details.md) para seu aplicativo (em **Gerenciamento de Aplicativos**) para confirmar se o seu aplicativo tem uma identidade atribuída. Se isso não acontecer, você verá uma opção para criar uma. Você precisará reservar um nome para seu aplicativo a fim de criar a identidade. Verifique se esse é o nome que você usou em seu pacote.
- Se o seu aplicativo já tiver uma identidade, você talvez ainda precise reservar o nome que você deseja usar no seu pacote. Em **Gerenciamento de Aplicativos**, clique em [Gerenciar nomes de aplicativos](manage-app-names.md). Digite o nome que você gostaria de usar e clique em **Reservar o nome do aplicativo**.

> [!IMPORTANT]
>  Se o nome que você deseja usar não estiver disponível, outro aplicativo pode ter já reservada esse nome. Se seu aplicativo já está publicado sob esse nome, ou se você acha que tem o direito de usá-lo, [contate o suporte](https://go.microsoft.com/fwlink/p/?LinkId=331509).  

 

 





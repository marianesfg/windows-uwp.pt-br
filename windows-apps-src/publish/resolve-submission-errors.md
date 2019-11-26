---
Description: Se você encontrar erros depois de enviar seu aplicativo para a Loja, deverá resolvê-los para continuar o processo de certificação.
title: Resolver erros de envio
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f67474905f4c689153af4dec22cf05f8399db535
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258984"
---
# <a name="resolve-submission-errors"></a>Resolver erros de envio

Se você encontrar erros depois de enviar seu aplicativo para a Loja, deverá resolvê-los para continuar o [processo de certificação](the-app-certification-process.md). A mensagem de erro indicará qual é o problema e o que talvez você precise fazer para corrigi-lo. Aqui estão algumas informações adicionais que podem ajudar a resolver esses erros.

## <a name="uwp-apps"></a>Aplicativos UWP

Se você estiver enviando um aplicativo UWP, poderá ver um erro durante o pré-processamento se o arquivo de pacote não for um arquivo. msixupload ou. appxupload gerado pelo Visual Studio para a loja. Certifique-se de seguir as etapas em [empacotar um aplicativo UWP com o Visual Studio](/windows/msix/package/packaging-uwp-apps) ao criar o arquivo de pacote do aplicativo e carregar apenas o arquivo. msixupload ou. appxupload na página [pacotes](upload-app-packages.md) do envio, não de um. msix/Appx ou. msixbundle/appxbundle.

Caso um erro de compilação seja exibido, certifique-se de que você consiga compilar o seu aplicativo no modo Versão com êxito. Para obter mais informações, consulte [Erros do compilador interno nativo .NET](https://github.com/dotnet/core/blob/master/Documentation/ilcRepro.md).

## <a name="desktop-application"></a>Aplicativo de desktop

Se você planeja enviar um pacote que contém os binários Win32 e UWP, certifique-se de criar esse pacote usando o projeto de empacotamento do Windows que está disponível no Visual Studio 2017 atualização 4 e versões posteriores. Se você criar o pacote usando um modelo de projeto UWP, talvez não seja possível enviar esse pacote para a loja ou Sideload-lo em outros PCs. Mesmo que o pacote seja publicado com êxito, ele pode se comportar de maneiras inesperadas no PC do usuário. Para obter mais informações, consulte [empacotar um aplicativo usando o Visual Studio (ponte de desktop)]( https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net).

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 8. x e anteriores

> [!IMPORTANT]
> A partir de 31 de outubro de 2018, os produtos recém-criados não podem incluir pacotes destinados a Windows Phone 8. x ou anterior. Para obter mais informações, consulte esta [postagem no blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Você poderá ver **erro 2001** quando os problemas com pacotes do Windows Phone forem detectados durante o pré-processamento. Na maioria dos casos, você precisará recompilar o pacote do aplicativo para corrigir o erro. Depois de terminar, substitua o pacote anterior pelo novo na página [Pacotes](upload-app-packages.md) do envio antes de clicar em **Enviar à Loja** novamente.

Existem alguns problemas que podem causar esse erro. Examine a lista abaixo para determinar quais podem se aplicar a seus pacotes.

-   **Um ou mais assemblies no pacote estão ofuscados incorretamente:** use uma ferramenta diferente para realizar a ofuscação ou removê-la. O processo de compilação otimiza os assemblies ofuscados, mas, às vezes, alguns assemblies são ofuscados usando uma ferramenta que modifica o MSIL de maneira não compatível, o que causará um erro.
-   **O tamanho de um ou mais métodos no aplicativo excede 256 KB de IL:** refatore o método ofensivo em funções menores. O tamanho de MSIL para métodos em um assembly pode ser determinado usando-se a ferramenta ILDASM.
-   **A validação de assinatura de nome forte falhou em um ou mais assemblies:** esse erro normalmente ocorre quando a assinatura de nome forte foi realizada usando-se uma chave diferente da esperada nos metadados do assembly. Assine usando a chave correta ou remova assinaturas de nome forte.
-   **O pacote contém assemblies de modo misto (com códigos gerenciado e nativo):** assemblies de modo misto não são compatíveis no Windows Phone. Remova os assemblies de modo misto do pacote e reenvie o aplicativo.
-   **Um XAP do Windows Phone 8.1 ou um assembly appx/appxbundle não é válido:** certifique-se de que o arquivo .winmd tenha pelo menos um ponto de entrada público. Você pode usar qualquer aplicativo descompilador para examinar o código e verificar se há pontos de entrada públicos, se necessário.

Outro erro que você poderá ver depois de enviar seu aplicativo é **erro 1300**. Isso ocorre quando um ou mais assemblies (ou todo o pacote) já estão pré-compilados. Para corrigir esse problema, recompile o pacote do aplicativo no Microsoft Visual Studio e, em seguida, envie o pacote recém-gerado.

## <a name="nameidentity-errors"></a>Erros de nome/identidade

Se você vir um erro que diz **The name found in the package is not one of your reserved app names. Please reserve the app name and/or update your package with the correct app name for this language**, talvez seja porque você inseriu um nome incorreto no seu pacote. Esse erro também pode ocorrer se você estiver usando um nome de aplicativo que você não reservou no Partner Center. Você geralmente pode resolver esse erro seguindo estas etapas:

- Vá até a página [Identidade do aplicativo](view-app-identity-details.md) para seu aplicativo (em **Gerenciamento de Aplicativos**) para confirmar se o seu aplicativo tem uma identidade atribuída. Se isso não acontecer, você verá uma opção para criar uma. Você precisará reservar um nome para seu aplicativo a fim de criar a identidade. Verifique se esse é o nome que você usou em seu pacote.
- Se o seu aplicativo já tiver uma identidade, você talvez ainda precise reservar o nome que você deseja usar no seu pacote. Em **Gerenciamento de Aplicativos**, clique em [Gerenciar nomes de aplicativos](manage-app-names.md). Digite o nome que você gostaria de usar e clique em **Reservar o nome do aplicativo**.

> [!IMPORTANT]
>  Se o nome que você deseja usar não estiver disponível, outro aplicativo talvez já tenha reservado esse nome. Se seu aplicativo já tiver sido publicado sob esse nome ou se você acha que tem o direito de usá-lo, [contate o suporte](https://support.microsoft.com/getsupport/hostpage.aspx?locale=EN-US&supportregion=EN-US&ccfcode=US&ln=EN-US&pesid=14654&oaspworkflow=start_1.0.0.0&tenant=store&supporttopic_L1=31762156&supporttopic_L2=31762179).  

 

 





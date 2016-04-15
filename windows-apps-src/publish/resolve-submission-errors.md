---
Description: Se você encontrar erros depois de enviar seu aplicativo para a Loja, deverá resolvê-los para continuar o processo de certificação.
title: Resolver erros de envio
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
---

# Resolver erros de envio


Se você encontrar erros depois de enviar seu aplicativo para a Loja, deverá resolvê-los para continuar o [processo de certificação](the-app-certification-process.md). A mensagem de erro indicará qual é o problema e o que talvez você precise fazer para corrigi-lo. Aqui estão algumas informações adicionais que podem ajudar a resolver esses erros.

## Aplicativos UWP


Se estiver enviando um aplicativo UWP, você poderá ver um erro durante o pré-processamento se o arquivo do pacote não for um arquivo .appxupload gerado pelo Visual Studio para a Loja. Não se esqueça de seguir as etapas em [Empacotando Aplicativos Universais do Windows para Windows 10](../packaging/packaging-uwp-apps.md) ao criar o arquivo do pacote do aplicativo e carregar o arquivo .appxupload na página [Pacotes](upload-app-packages.md) do envio, e não um appx ou .appxbundle.

Caso um erro de compilação seja exibido, certifique-se de que você consiga compilar o seu aplicativo no modo Versão com êxito. Para obter mais informações, consulte [Erros do compilador interno nativo .NET](http://go.microsoft.com/fwlink/p/?LinkID=613098).

## Aplicativos do Windows Phone


Você poderá ver **erro 2001** quando os problemas com pacotes do Windows Phone forem detectados durante o pré-processamento. Na maioria dos casos, você precisará recompilar o pacote do aplicativo para corrigir o erro. Depois de terminar, substitua o pacote anterior pelo novo na página [Pacotes](upload-app-packages.md) do envio antes de clicar em **Enviar à Loja** novamente.

Existem alguns problemas que podem causar esse erro. Examine a lista abaixo para determinar quais podem se aplicar a seus pacotes.

-   **Um ou mais assemblies no pacote estão ofuscados incorretamente:** use uma ferramenta diferente para realizar a ofuscação ou removê-la. O processo de compilação otimiza os assemblies ofuscados, mas, às vezes, alguns assemblies são ofuscados usando uma ferramenta que modifica o MSIL de maneira não compatível, o que causará um erro.
-   **O tamanho de um ou mais métodos no aplicativo excede 256 KB de IL:** refatore o método ofensivo em funções menores. O tamanho de MSIL para métodos em um assembly pode ser determinado usando-se a ferramenta ILDASM.
-   **A validação de assinatura de nome forte falhou em um ou mais assemblies:** esse erro normalmente ocorre quando a assinatura de nome forte foi realizada usando-se uma chave diferente da esperada nos metadados do assembly. Assine usando a chave correta ou remova assinaturas de nome forte.
-   **O pacote contém assemblies de modo misto (com códigos gerenciado e nativo):** assemblies de modo misto não são compatíveis no Windows Phone. Remova os assemblies de modo misto do pacote e reenvie o aplicativo.
-   **Um XAP do Windows Phone 8.1 ou um assembly appx/appxbundle não é válido:** certifique-se de que o arquivo .winmd tenha pelo menos um ponto de entrada público. Você pode usar qualquer aplicativo descompilador para examinar o código e verificar se há pontos de entrada públicos, se necessário.

Outro erro que você poderá ver depois de enviar seu aplicativo é **erro 1300**. Isso ocorre quando um ou mais assemblies (ou todo o pacote) já estão pré-compilados. Para corrigir esse problema, recompile o pacote do aplicativo no Microsoft Visual Studio e, em seguida, envie o pacote recém-gerado.

 

 






<!--HONumber=Mar16_HO1-->



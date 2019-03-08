---
title: Compilar o código-fonte do XDK API do Xbox Live
description: Saiba como compilar a fonte de API do Xbox Live é fornecida com o Kit de desenvolvedor do Xbox (XDK).
ms.assetid: 78425e82-c132-4f6b-9db3-2536862f1ce5
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, xdk
ms.localizationpriority: medium
ms.openlocfilehash: 9a98af637c8c60449cd2005c4fc6f83f9b0719cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660991"
---
# <a name="compile-the-xbox-developer-kit-xdk-xbox-live-api-source"></a>Compilar o código-fonte do Kit de desenvolvedor do Xbox (XDK) API do Xbox Live

O Kit de desenvolvedor do Xbox (XDK) inclui código-fonte para a criação de Microsoft.Xbox.Services.dll (XSAPI). Os desenvolvedores podem seguir estas instruções para atualizar seus projetos para usar uma compilação local da DLL.

Você talvez queira criar XSAPI por conta própria se:
1. Se você quiser depurar um problema para entender onde um código de erro é provenientes.
1. Se nós fornecemos um patch do código fonte para corrigir um problema para você, antes de nós pode distribuir um QFE.

## <a name="to-compile-the-xdk-c-xsapi-project-for-yourself"></a>Para compilar o projeto do XDK C++ XSAPI por conta própria

<ol>
  <li> Obtenha o código-fonte Microsoft.Xbox.Services. Para fazer isso, extraia todos os arquivos de "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox API\8.0\SourceDist\Xbox.Services.zip serviços" para uma pasta gravável fora "C:\Program Files (x86)", ou você pode clonar a origem do <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> Se seu projeto referencia a DLL de pré-compilação, você precisará remover a referência</li>
    <ul>
      <li> Para o Visual Studio 2012: Escolha "Projeto -> referências..." no Visual Studio. Se a API de serviços do Xbox está listado como uma referência, selecione-o e clique em "Remove Reference". Clique em "Okey" e salve o arquivo de projeto.</li>
      <li> Para o Visual Studio 2015 ou 2017: Escolha "projeto -> Adicionar referências..." no Visual Studio. Se a API de serviços do Xbox estiver marcada, desmarque-a. Clique em "Okey" e salve o arquivo de projeto.</li>
    </ul>
  <li> Se você estiver criando com o XDK, escolha "arquivo -> Adicionar -> projeto existente..." no Visual Studio para adicionar os seguintes dois projetos à solução do seu aplicativo. Os arquivos vcxproj serão ser localizados na pasta que você extraiu a fonte.</li>
Para o Visual Studio 2017: <ul>
      <li>\Build\Microsoft.Xbox.Services.141.XDK.Cpp\Microsoft.Xbox.Services.141.XDK.Cpp.vcxproj</li>   <li>\External\cpprestsdk\Release\src\build\vs15.xbox\casablanca141.Xbox.vcxproj</li>
    </ul>
Para Visual Studio 2015: <ul>
      <li>\Build\Microsoft.Xbox.Services.140.XDK.Cpp\Microsoft.Xbox.Services.140.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs14.xbox\casablanca140.Xbox.vcxproj</li>
    </ul>
Para o Visual Studio 2012: <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.Cpp\Microsoft.Xbox.Services.110.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
    <li> Adicione os projetos de código-fonte como uma referência ao escolher o projeto -> referências... e selecione "Add Reference". Em "Soluções -> projetos", verifique as entradas para ambos os projetos acima, em seguida, clique em Okey.</li>
    <li> Adicione o arquivo de propriedades ao seu projeto, clicando em "Exibir -> outras Windows -> Gerenciador de propriedades", o botão direito em seu projeto, selecionando "Adicionar folha de propriedades existente", e finalmente selecionando o arquivo xsapi.staticlib.props na raiz do SDK sourch.  Se seu sistema de compilação não dá suporte a arquivos de objetos, você deve adicionar manualmente as definições de pré-processador e bibliotecas, como visto no %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\ Xbox.Services.API.Cpp.props</li>
    <li> Adicionar o arquivo services.h à sua fonte de aplicativo, clique com o botão direito no projeto Adicionar -> Item existente e escolhendo o {SDK root}\Include\xsapi\services.h arquivo de origem</li>
    <li> Certifique-se de que "Pasta de saída" do projeto de aplicativo e o projeto de serviços do Xbox são os mesmos. Essa configuração pode ser encontrada no projeto do Visual Studio Propriedades -> Propriedades de configuração -> Geral -> diretório de saída.</li>
    <li> Recompile sua solução do Visual Studio</li>
</ol>

## <a name="to-compile-the-xdk-winrt-xsapi-project-for-yourself"></a>Para compilar o projeto do XDK WinRT XSAPI por conta própria

<ol>
  <li> Obtenha o código-fonte Microsoft.Xbox.Services. Para fazer isso, você extrair todos os arquivos de "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox API\8.0\SourceDist\Xbox.Services.zip serviços" para uma pasta gravável fora "C:\Program Files (x86)", ou você pode clonar a origem do <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> Se seu projeto referencia a DLL de pré-compilação, você precisará remover a referência</li>
    <ul>
      <li> Para o Visual Studio 2012: Escolha "Projeto -> referências..." no Visual Studio. Se a API de serviços do Xbox está listado como uma referência, selecione-o e clique em "Remove Reference". Clique em "Okey" e salve o arquivo de projeto.</li>
      <li> Para o Visual Studio 2015 ou 2017: Escolha "projeto -> Adicionar referências..." no Visual Studio. Se a API de serviços do Xbox estiver marcada, desmarque-a. Clique em "Okey" e salve o arquivo de projeto.</li>
    </ul>
  <li> Se você estiver criando com o XDK, escolha "arquivo -> Adicionar -> projeto existente..." no Visual Studio para adicionar os seguintes dois projetos à solução do seu aplicativo. Os arquivos vcxproj serão ser localizados na pasta que você extraiu a fonte.  Para Visual Studio 2015, os projetos atualizará automaticamente para o formato do VS2015.</li>
    <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.WinRT\Microsoft.Xbox.Services.110.XDK.WinRT.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
  <li> No Visual Studio, adicione as referências:</li>
    <ul>
      <li> Para o Visual Studio 2012: Escolha "Projeto -> referências..." e selecione "Add Reference" no Visual Studio. Em soluções -> projetos, chceck as entradas para ambos os projetos acima e clique em Okey.</li>
      <li> Para o Visual Studio 2015 ou 2017: Escolha "projeto -> Adicionar referências..." no Visual Studio. Em projetos, verifique as entradas para ambos os projetos acima e clique em Okey.</li>
    </ul>
  <li> Certifique-se de que "Pasta de saída" do projeto de aplicativo e o projeto de serviços do Xbox são os mesmos. Essa configuração pode ser encontrada no projeto do Visual Studio Propriedades -> Propriedades de configuração -> Geral -> diretório de saída.</li>
  <li> Recompile sua solução do Visual Studio</li>
</ol>

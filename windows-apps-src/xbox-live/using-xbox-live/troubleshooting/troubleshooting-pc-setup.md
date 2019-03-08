---
title: Solução de problemas de instalação do Xbox Live no PC do Windows
description: Saiba como solucionar problemas de seu ambiente de desenvolvimento do Xbox Live em um PC com Windows.
ms.assetid: 9cfebdcd-0c1c-4fc2-9457-e7e434b6374c
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, solucionar problemas
ms.localizationpriority: medium
ms.openlocfilehash: c1f055a49fe34be35335e50dc8b1efbfb7b9b922
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647731"
---
# <a name="troubleshooting-xbox-live-setup-on-windows-pc"></a>Solução de problemas de instalação do Xbox Live no PC do Windows

No computador com Windows 10, você pode garantir que seu computador estiver configurado corretamente com estas etapas:

1. Altere o seu computador para apontar para a área restrita XDKS.1 onde os exemplos são projetados para execução.  Fazer isso, execute este script:

        {*SDK source root*}\Tools\SwitchSandbox.cmd XDKS.1

1. Extraia o conteúdo do arquivo zip "SourcesAndSamples.zip" encontrado dentro do SDK.
1. Abra uma solução de exemplo:
    1. Para a API do C++: {*raiz do código-fonte do SDK*} \Samples\Social\UWP\Cpp\Social.Cpp.140.sln
    1. Para a API do WinRT com C#: {*raiz do código-fonte do SDK*} \Samples\Social\UWP\CSharp\Social.CSharp.140.sln
    1. Para a API do WinRT com C + + c++ /CX: {*raiz do código-fonte do SDK*} \Samples\TitleStorage\UWP\CppCx\TitleStorageUniversal.sln
1. Altere a plataforma de destino de compilação para "Win32" ou "x64".
1. Com o botão direito na solução e novamente compilar tudo.
1. Inicie o aplicativo no depurador.
1. Entrar com a conta de desenvolvimento que você criou na [Portal do desenvolvedor do Xbox](https://xdp.xboxlive.com), ou com uma conta de desenvolvedor do varejo autorizado [Partner Center](https://partner.microsoft.com/dashboard).
1. Conceda ao aplicativo permissão para acessar suas informações do Xbox Live.
1. Verifique se o aplicativo pode recuperar as informações e você pode ver seu nome de jogador.
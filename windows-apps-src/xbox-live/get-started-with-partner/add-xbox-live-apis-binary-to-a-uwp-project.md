---
title: Adicionar o Xbox Live APIs binária a um projeto UWP
description: Saiba como usar o NuGet para adicionar o pacote de binários de APIs do Xbox Live para seu projeto de UWP.
ms.assetid: 1e77ce9f-8a0e-402c-9f46-e37f9cda90ed
ms.date: 11/28/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, o nuget
ms.localizationpriority: medium
ms.openlocfilehash: 20b4d4ae27282ccf71964d31da4d1f577c280de8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593941"
---
# <a name="add-xbox-live-apis-binary-package-to-your-uwp-project"></a>Adicione o pacote de binários de APIs do Xbox Live ao seu projeto UWP

## <a name="requirements"></a>Requisitos

2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**. Aplicativos UWP podem ser criados com o Visual Studio 2015 atualização 3 ou posterior. É recomendável que você use a versão mais recente do Visual Studio para desenvolvedores e atualizações de segurança.
4. **[SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** ou posterior.

## <a name="add-the-binary-package-via-nuget"></a>Adicionar o pacote de binários por meio do NuGet

Para usar a API do Xbox Live, do seu projeto, você pode adicionar referências para os binários usando pacotes do NuGet ou adicionando a origem de API. A adição de pacotes NuGet torna a compilação mais rápida enquanto a adição da fonte torna a depuração mais rápida. Este artigo irá percorrer usando pacotes NuGet. Se você quiser usar o código-fonte, em seguida, consulte [Compilando o Xbox Live APIs fonte no seu projeto UWP](add-xbox-live-apis-source-to-a-uwp-project.md).

A API de serviços do Xbox possui tipos para UWP e XDK, em C++ e o WinRT e ter seu namespace estruturado como **Microsoft.Xbox.Live.SDK.*. UWP** e **Microsoft.Xbox.Live.SDK.*. XboxOneXDK**.

1. **UWP** é para desenvolvedores que estão criando um jogo UWP, que pode ser executados no PC, o Xbox One ou Windows Phone.
2. **XboxOneXDK** destina-se ID@Xbox e desenvolvedores que estão usando um Xbox XDK gerenciados.
3. O SDK do C++ pode ser usado para os mecanismos de jogos C++, enquanto que o SDK do WinRT é para mecanismos de jogos escritos com C++, C#, ou JavaScript.
4. Ao usar o WinRT com um mecanismo de C++, você deve usar C + + c++ /CX que usa chapéus (^). C++ é a API recomendada para uso para mecanismos de jogos do C++.  

> [!TIP]
> Você pode ler mais sobre a execução de UWP no Xbox One em [Introdução ao desenvolvimento de aplicativos UWP no Xbox One](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

Você pode adicionar o pacote NuGet do Xbox Live SDK por:

1. No Visual Studio, vá para **ferramentas** > **Gerenciador de pacotes NuGet** > **gerenciar pacotes NuGet para solução...** .
2. No Gerenciador de pacotes do NuGet, clique em **navegue** e insira **Xbox.Live.SDK** na caixa de pesquisa.
3. Selecione a versão do Xbox Live SDK que você deseja usar na lista à esquerda.
3. No lado direito da janela, marque a caixa ao lado do seu projeto e clique em **instalar**.

> [!NOTE]
> Os desenvolvedores de programa de criadores do Xbox Live devem usar uma das versões do UWP do Xbox Live SDK, pois não há suporte para o XDK.

![Adicionar XBL por meio do NuGet](../images/getting_started/vs-add-nuget-xbl.gif)

> [!IMPORTANT]
> Para `Microsoft.Xbox.Live.SDK.Cpp.*` projetos baseados em, certifique-se de incluir o cabeçalho `#include <xsapi\services.h>` na fonte do seu projeto.

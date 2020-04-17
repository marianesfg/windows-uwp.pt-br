---
description: O Editor de Fotos é um aplicativo de exemplo UWP que mostra o desenvolvimento com a projeção de linguagem C++/WinRT. O aplicativo de exemplo permite recuperar fotos da biblioteca Imagens e, em seguida, editar a imagem selecionada com efeitos fotográficos variados.
title: Exemplo de aplicativo Editor de fotos em C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, exemplo, aplicativo, foto, editor
ms.localizationpriority: medium
ms.openlocfilehash: dcefe2ad8321ae85fcb814bbaead0bb0e5373300
ms.sourcegitcommit: 8b7b677c7da24d4f39e14465beec9c4a3779927d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81266904"
---
# <a name="photo-editor-cwinrt-sample-application"></a>Exemplo de aplicativo Editor de fotos em C++/WinRT

> [!NOTE]
> O exemplo é tem como alvo o Windows 10, versão 1903 (10.0; Build 18362) e o Visual Studio 2019, e foi testado em ambos. Se preferir, use as propriedades do projeto para redirecionar o(s) projeto(s) para o Windows 10, versão 1809 (10.0; Build 17763) e/ou abrir o exemplo com o Visual Studio 2017.

Para clonar ou baixar o aplicativo de exemplo, consulte o [aplicativo de exemplo do Editor de fotos do C++/WinRT](/samples/microsoft/windows-appsample-photo-editor/photo-editor-cwinrt-sample-application/) na galeria de exemplos de código.

O Editor de fotos é um exemplo de aplicativo UWP (Plataforma Universal do Windows) que mostra o desenvolvimento com a projeção de linguagem [C++/WinRT](intro-to-using-cpp-with-winrt.md). O aplicativo de exemplo permite que você recupere fotos da biblioteca de **Imagens** e, em seguida, edite a imagem selecionada com efeitos fotográficos diferentes. No código-fonte do exemplo, você verá várias práticas comuns, como [associação de dados](binding-property.md) e [ações e operações assíncronas](concurrency.md), realizadas usando a projeção do C++/WinRT. Eis alguns dos recursos específicos demonstrados pela amostra.

- Uso da sintaxe e das bibliotecas padrão do C++17 com as APIs do Windows Runtime (WinRT).
- Uso de corrotinas, incluindo o uso de co_await, co_return, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) e [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation-1).
- Criação e uso de tipos projetados da classe personalizada do Windows Runtime (classe de tempo de execução) e tipos de implementação. Para saber mais sobre estes termos, confira [Utilizar APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).
- [Processamento de eventos](handle-events.md), incluindo o uso de tokens de evento de revogação automática.
- Uso do pacote NuGet Win2D externo e [Windows::UI::Composition](/uwp/api/windows.ui.composition) para efeitos de imagem.
- Associação de dados XAML, incluindo a [extensão de marcação {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension).
- Aplicação de estilos XAML e personalização da interface do usuário, incluindo [animações conectadas](../design/motion/connected-animation.md).

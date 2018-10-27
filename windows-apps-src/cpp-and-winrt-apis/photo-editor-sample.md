---
author: JoshuaPartlow
description: O Editor de Fotos é um aplicativo de exemplo UWP que mostra o desenvolvimento com a projeção de linguagem C++/WinRT. O aplicativo de exemplo permite que você recupere fotos a partir da biblioteca de Imagens e, em seguida, edite a imagem selecionada com efeitos fotográficos diferentes.
title: Aplicativos de exemplo de C++/WinRT do Editor de Fotos
ms.author: wdg-dev-content
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, amostra, aplicativo, foto, editor
ms.localizationpriority: medium
ms.openlocfilehash: 60bfcd79ed2d659aff8d435bd397df05eb45af72
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5691065"
---
# <a name="photo-editor-cwinrt-sample-application"></a>Aplicativos de exemplo de C++/WinRT do Editor de Fotos
Você pode clonar ou baixar o aplicativo de exemplo do repositório do GitHub para o [aplicativo de exemplo do Editor de fotos do C++/WinRT](https://github.com/Microsoft/Windows-appsample-photo-editor).

O Editor de Fotos é um aplicativo de exemplo da UWP (Plataforma Universal do Windows) que mostra o desenvolvimento com a projeção de linguagem [C++/WinRT](intro-to-using-cpp-with-winrt.md). O aplicativo de exemplo permite que você recupere fotos a partir da biblioteca de **Imagens** e, em seguida, edite a imagem selecionada com efeitos fotográficos diferentes. No código-fonte do exemplo, você verá um número de práticas comuns&mdash;como [associação de dados](binding-property.md) e [ações e operações assíncronas](concurrency.md)&mdash;realizadas usando a projeção do C++/WinRT. Veja alguns dos recursos específicos demonstrados pela amostra.
    
- Use a sintaxe e as bibliotecas do C++17 com as APIs do Windows Runtime (WinRT).
- Use de corrotinas, incluindo o uso de co_await, co_return, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) e [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_).
- Criação e o uso de tipos projetados da classe personalizada do Windows Runtime (classe do tempo de execução) e tipos de implementação. Para obter mais informações sobre estes termos, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).
- [Processamento de eventos](handle-events.md), incluindo o uso de tokens de evento de revogação automática.
- Uso do pacote NuGet Win2D externo e [Windows::UI::Composition](/uwp/api/windows.ui.composition) para efeitos de imagem.
- Associação de dados XAML, incluindo a [extensão de marcação {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension).
- Estilo XAML e personalização da interface do usuário, incluindo [animações conectadas](../design/motion/connected-animation.md).

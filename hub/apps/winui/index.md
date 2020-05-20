---
title: Biblioteca de Interface do Usuário do Windows (WinUI)
description: Bibliotecas WinUI para o desenvolvimento de aplicativos do Windows.
ms.topic: article
ms.date: 05/11/2020
keywords: windows 10, uwp, sdk de kit de ferramentas, winui, Biblioteca de Interface do Usuário do Windows
ms.openlocfilehash: 2afa6b1eadc98300e3de76a1dfc6ede66a2a56e5
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580173"
---
# <a name="windows-ui-library-winui"></a>Biblioteca de Interface do Usuário do Windows (WinUI)

![Imagem de destaque dos kits de ferramentas](../images/logo-winui.png)

A Biblioteca de Interface do Usuário do Windows (WinUI) é a plataforma da interface do usuário nativa e moderna para aplicativos do Windows no Win32 e na UWP.

Incorporando o [Sistema Design Fluente](https://www.microsoft.com/design/fluent/#/) em todos os controles e estilos, o WinUI fornece experiências uniformes, intuitivas e acessíveis usando os padrões de interface do usuário mais recentes.

E com suporte para aplicativos Win32 e UWP, você pode criar com o WinUI do zero ou migrar seus aplicativos MFC, WinForms ou WPF existentes em seu próprio ritmo e usando linguagens conhecidas como C++, C#, Visual Basic e até mesmo Javascript via [React Native para Windows](https://microsoft.github.io/react-native-windows/).

Há duas versões do WinUI a serem consideradas, o **WinUI 2.x** e o **WinUI 3.0**.

## <a name="windows-ui-2x-library"></a>Biblioteca de Interface do Usuário do Windows 2.x

O WinUI 2.x pode ser usado agora mesmo em aplicativos UWP e incorporado aos seus aplicativos da área de trabalho existentes por meio de [Ilhas XAML](/windows/apps/desktop/modernize/xaml-islands).

A Biblioteca WinUI 2.x está intimamente associada ao SDK da UWP e fornece controles de interface do usuário nativos do Windows, bem como outros elementos de interface do usuário para aplicativos UWP.

Mantendo a compatibilidade com versões anteriores do Windows 10, os controles do WinUI 2.x funcionam mesmo que os usuários não tenham o sistema operacional mais recente.

![Suporte a plataformas do WinUI 2.x](../images/platforms-winui2.png)

> [!NOTE]
> O WinUI 2.4 é a versão mais recente do WinUI 2.x. Consulte a [Etapa WinUI 2.5](https://github.com/microsoft/microsoft-ui-xaml/milestone/10) para ver uma lista de trabalhos planejados na próxima versão.

Para obter instruções de instalação, confira [Introdução à Biblioteca de Interface do Usuário do Windows](winui2/getting-started.md).

### <a name="related-links"></a>Links relacionados

- [Visão geral da Biblioteca WinUI 2.x](winui2/index.md)
- [Documentos de API](https://docs.microsoft.com/uwp/api/overview/winui/)
- [Código-fonte](https://aka.ms/winui)
- [Aplicativo da Galeria de Controles XAML](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-30-library-preview-1"></a>Biblioteca da Interface do Usuário do Windows 3.0 (Versão prévia 1)

O WinUI 3 é a próxima versão do WinUI, uma plataforma de interface do usuário do Windows 10 nativa, completamente desassociada do SDK da UWP.

Ao separar completamente o XAML, a composição e as APIs de entrada do SDK da UWP, a Biblioteca de Interface do Usuário do Windows 3.0 pode expandir bastante o escopo do WinUI para incluir a plataforma de interface do usuário nativa do Windows 10 completa.

O WinUI serve como o caminho de avanço para todos os aplicativos do Windows. Você pode usá-lo como a camada de interface do usuário em seu aplicativo UWP ou Win32 nativo ou pode usá-lo para modernizar seu aplicativo da área de trabalho elemento por elemento com as [Ilhas XAML](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands).
 
> [!NOTE]
> O WinUI 3.0 Versão prévia 1 é um build de pré-lançamento do WinUI 3.0. Seus comentários são bem-vindos no [repositório do WinUI no GitHub](https://github.com/microsoft/microsoft-ui-xaml).

Todos os novos recursos XAML serão fornecidos como parte do WinUI. As APIs XAML da UWP existentes que são fornecidas como parte do sistema operacional não receberão mais novas atualizações de recursos. No entanto, elas continuarão recebendo atualizações de segurança e correções críticas, de acordo com o ciclo de vida de suporte do Windows 10.

A Plataforma Universal do Windows contém mais do que apenas a estrutura XAML (como os modelos de aplicativo e de segurança, o pipeline de mídia, as integrações de shell do Xbox e do Windows 10 e amplo suporte a dispositivos) e continuará tendo suporte.

![Suporte a plataformas do WinUI 3.0](../images/platforms-winui3.png)

> [!Important]
> A finalidade do WinUI 3.0 Versão prévia 1 é fazer uma avaliação inicial e reunir comentários da comunidade de desenvolvedores. Ele **NÃO** deve ser usado para aplicativos de produção.

### <a name="related-links"></a>Links relacionados

- [WinUI 3.0 Versão prévia 1 (maio de 2020)](winui3/index.md)
- [Aplicativo da Galeria de Controles XAML (WinUI 3.0 Versão prévia 1)](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3alpha)

## <a name="winui-resources"></a>Recursos do WinUI

**Github**: O WinUI é um projeto de software livre hospedado no GitHub. No [Repositório do WinUI](https://github.com/microsoft/microsoft-ui-xaml), você pode registrar solicitações de recursos ou bugs, interagir com a equipe do WinUI e ver os planos da equipe para o WinUI 3 e muito mais no [roteiro](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) deles.

**Site**: o [site do WinUI](https://aka.ms/winui) tem informações úteis sobre comparações de produtos, as vantagens do WinUI e maneiras de se manter envolvido com o produto e a equipe dele.

---
title: WinUI 3.0 Versão prévia 1 (maio de 2020)
description: Visão geral da versão prévia do WinUI 3.0.
ms.date: 05/14/2020
ms.topic: article
ms.openlocfilehash: 3aac14807f8455eb9a9294c40ddc76ddfa224659
ms.sourcegitcommit: 7e8c7f89212c88dcc0274c69d2c3365194c0954a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83688483"
---
# <a name="windows-ui-library-30-preview-1-may-2020"></a>Biblioteca de Interface do Usuário do Windows 3.0 Versão prévia 1 (maio de 2020)

A Biblioteca de Interface do Usuário do Windows (WinUI) 3.0 é uma atualização importante que transforma o WinUI em uma estrutura completa de UX para todos os tipos de aplicativos do Windows, do Win32 à UWP.

> [!Important]
> A finalidade desta versão prévia do WinUI 3.0 é fazer uma avaliação inicial e reunir comentários da comunidade de desenvolvedores. Ele **NÃO** deve ser usado para aplicativos de produção.
>
> **Confira [Limitações e problemas conhecidos da versão prévia 1](#preview-1-limitations-and-known-issues)** .
## <a name="new-features-in-winui-30-preview-1"></a>Novos recursos no WinUI 3.0 Versão prévia 1

- Capacidade de criar aplicativos da Área de Trabalho com o WinUI, incluindo [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0) para aplicativos Win32
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [Atualizações de TabView](/windows/uwp/design/controls-and-patterns/tab-view)
- Atualizações do tema escuro
- Melhorias e atualizações de [WebView2](https://docs.microsoft.com/microsoft-edge/hosting/webview2)
  - Suporte para DPI Alto
  - Suporte para redimensionamento e movimentação de janela
  - Atualizado para ter como destino a versão mais recente do Edge
  - Deixou de ser necessário fazer referência a um pacote NuGet específico do WebView2
- SwapChainPanel
- Melhorias necessárias para a migração de software livre

Para obter mais informações sobre os benefícios do WinUI 3.0, bem como o roteiro do WinUI, confira o [Roteiro da Biblioteca de Interface do Usuário do Windows](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) no GitHub.

### <a name="provide-feedback-and-suggestions"></a>Fornecer comentários e sugestões

Seus comentários são bem-vindos no [repositório do WinUI no GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

## <a name="try-winui-30-preview-1"></a>Experimentar o WinUI 3.0 Versão prévia 1

Configure seu ambiente de desenvolvimento (para obter instruções detalhadas, confira [Configurar seu ambiente de desenvolvimento](#configure-your-dev-environment)), instale o VSIX do WinUI 3.0 Versão prévia 1 clicando no link a seguir e experimente os modelos de projeto do WinUI 3.0.

<table>
<tr>
<td align="center">
<a href="https://aka.ms/winui3/previewdownload"><img src="images/downloadbuttontx.png" alt="Download the WinUI 3.0 Preview 1 VSIX"/></a>
<!--
<br/>
<a href="https://aka.ms/winui3/previewdownload">Download the WinUI 3.0 Preview 1 VSIX</a>
-->
</td>
</tr>
</table>

Você também pode clonar e compilar a versão do WinUI 3.0 Versão prévia 1 da [Galeria de Controles XAML](#xaml-controls-gallery-winui-30-preview-1-branch).

### <a name="configure-your-dev-environment"></a>Configurar seu ambiente de desenvolvimento

Verifique se o computador de desenvolvimento tem a Atualização de abril de 2018 para o Windows 10 (versão 1803 – build 17134) ou mais recente instalada.

Instale o Visual Studio 2019, versão 16.7 versão prévia 1. Você pode baixá-lo no [Visual Studio Preview](https://visualstudio.microsoft.com/vs/preview).

Você precisa incluir as seguintes cargas de trabalho ao instalar o Visual Studio Preview:

- Desenvolvimento para desktop com .NET
- Desenvolvimento para a Plataforma Universal do Windows

Para compilar aplicativos em C++, você também precisa incluir as seguintes cargas de trabalho:

- Desenvolvimento para desktop com C++
- O componente opcional *Ferramentas da Plataforma Universal do Windows para C++ (v142)* para a carga de trabalho da Plataforma Universal do Windows

### <a name="visual-studio-project-templates"></a>Modelos de projeto do Visual Studio

Para acessar o WinUI 3.0 Versão prévia 1 e os modelos de projeto, acesse **https://aka.ms/winui3/previewdownload**

Baixe a Extensão do Visual Studio (`.vsix`) para adicionar os modelos de projeto do WinUI e o pacote NuGet ao Visual Studio 2019.

Para obter instruções de como adicionar o `.vsix` ao Visual Studio, confira [Como encontrar e usar extensões do Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box).

Após instalar a extensão do `.vsix`, você pode criar um projeto do WinUI 3.0 pesquisando por "WinUI" e selecionando um dos modelos de C# ou de C++ disponíveis.

![Modelos do Visual Studio para o WinUI 3.0](images/WinUI3Templates.png)<br/>
*Exemplo dos modelos do Visual Studio para o WinUI 3.0*

### <a name="visual-studio-project-configuration"></a>Configuração do projeto do Visual Studio

Ao criar um projeto usando um dos modelos do WinUI 3.0 Versão prévia 1, defina a **Versão de destino** como o Windows 10, versão 1903 (build 18362) e a **Versão mínima** como o Windows 10, versão 1803 (build 17134).

Para alterar esses valores depois de criar um projeto, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Propriedades**.

### <a name="creating-a-desktop-win32-app-with-winui-30-preview-1"></a>Criando um aplicativo Win32 da Área de Trabalho com o WinUI 3.0 Versão prévia 1

Confira [Introdução ao WinUI 3.0 para aplicativos da área de trabalho](get-started-winui3-for-desktop.md).

Além das limitações e dos problemas conhecidos descritos abaixo, compilar um aplicativo usando o WinUI 3.0 Versão prévia 1 é muito semelhante a compilar um aplicativo UWP com XAML e o WinUI 2.x, de modo que a maior parte das orientações e da documentação dos aplicativos UWP e das APIs do `Windows.UI` é aplicável.

## <a name="preview-1-limitations-and-known-issues"></a>Limitações e problemas conhecidos da versão prévia 1

A versão prévia 1 é apenas uma versão de teste. Os cenários relacionados a aplicativos Win32 da Área de Trabalho são especialmente novos. Espere bugs, limitações e problemas.

Os itens a seguir são alguns dos problemas conhecidos do WinUI 3.0 Versão prévia 1. Se encontrar algum problema que não está listado abaixo, informe-nos contribuindo com um problema existente ou criando um no [Repositório do WinUI no GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="platform-and-os-support"></a>Suporte para plataformas e SOs

O WinUI 3.0 Versão prévia 1 é compatível com computadores que executam a Atualização de abril de 2018 para o Windows 10 (versão 1803 – build 17134) e versões mais recentes.

### <a name="developer-tools"></a>Ferramentas de desenvolvedor

- Os aplicativos de área de trabalho têm suporte para .NET 5 e C# 8 e precisam ser empacotados.
- Os aplicativos UWP têm suporte para .NET Native e C# 7.3
- O IntelliSense está incompleto
- Não há designer visual
- Não há recarga dinâmica de XAML
- Não há árvore visual dinâmica
- Ainda não há suporte para desenvolvimento com o VS Code
- Não há suporte para novos aplicativos C++/CX, mas os aplicativos existentes continuarão funcionando (mude para o C++/WinRT assim que possível)
- O conteúdo do WinUI 3.0 pode estar em apenas uma janela por processo ou em um ApplicationView por aplicativo
- Não há suporte para implantação na área de trabalho sem estar empacotado
- Não há suporte para ARM64

### <a name="missing-platform-features"></a>Recursos de plataforma ausentes

- Não há suporte para Xbox
- Não há suporte para HoloLens
- Não há suporte para pop-ups em janelas
- Não há suporte para entrada à tinta
- Acrílico de fundo
- MediaElement e MediaPlayerElement
- RenderTargetBitmap
- MapControl
- Navegação hierárquica com o NavigationView
- SwapChainPanel não tem suporte para transparência
- A Revelação Global usa comportamento de fallback, um pincel sólido
- Não há suporte para Ilhas XAML nesta versão
- Bibliotecas de ecossistemas de terceiros não funcionam completamente
- IMEs não funcionam
- Não é possível chamar métodos no namespace Windows.UI.Text

### <a name="known-issues"></a>Problemas conhecidos

- Para aplicativos da área de trabalho em C#:
   - Você precisa usar `WinRT.WeakReference<T>` em vez de `System.WeakReference<T>` para referências fracas a objetos do Windows (incluindo objetos XAML).
   - Os structs [Point](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [Rect](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect) e [Size](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size) têm membros que são do tipo Float em vez de Double.


## <a name="xaml-controls-gallery-winui-30-preview-1-branch"></a>Galeria de Controles XAML (branch do WinUI 3.0 Versão prévia 1)

Confira em [Branch do WinUI 3.0 Versão prévia 1 da Galeria de Controles XAML](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview) um aplicativo de exemplo que inclui todos os controles e recursos do WinUI 1 3.0 Versão prévia 1.

![Aplicativo da Galeria de Controles XAML do WinUI 3.0 Versão prévia 1](images/WinUI3XamlControlsGallery.png)<br/>
*Aplicativo de exemplo da Galeria de Controles XAML do WinUI 3.0 Versão prévia 1*

Para baixar a amostra, clone a ramificação **winui3preview** usando o seguinte comando:

> `git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git`

Depois de clonar, mude para a ramificação **winui3preview** no seu ambiente do Git local:

> `git checkout winui3preview`

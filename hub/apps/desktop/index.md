---
Description: Saiba como começar a criar aplicativos de desktop para PCs Windows, incluindo como escolher a plataforma de aplicativo certa para novos aplicativos e como modernizar aplicativos existentes para o Windows 10.
title: Crie aplicativos da área de trabalho para PCs Windows
ms.topic: article
ms.date: 11/04/2019
keywords: Windows, win32, desenvolvimento de área de trabalho
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: c27826980127674b4a4356af8a3c36056e86bd83
ms.sourcegitcommit: cf88f5e8e1de476ed2635e791a5e5e82ae4bd8cf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74056866"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>Crie aplicativos da área de trabalho para PCs Windows

Este artigo fornece as informações de que você precisa para começar a criar aplicativos de área de trabalho para Windows ou atualizar aplicativos de área de trabalho existentes para adotar as experiências mais recentes do Windows 10.

## <a name="platforms-for-desktop-apps"></a>Plataformas para aplicativos de área de trabalho

Há quatro plataformas principais para a criação de aplicativos de área de trabalho para computadores Windows. Cada plataforma oferece um modelo de aplicativo que define o ciclo de vida do aplicativo, um conjunto completo de controles de interface do usuário e acesso a um conjunto abrangente de APIs gerenciadas ou nativas para o uso de recursos do Windows.

A tabela a seguir apresenta as plataformas. Para uma comparação detalhada dessas plataformas, juntamente com recursos adicionais de cada uma delas, consulte [Escolher sua plataforma de aplicativo](choose-your-platform.md).

<br/>

<table>
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>Plataforma</th>
<th>Descrição</th>
<th>Documentos e recursos</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://docs.microsoft.com/windows/uwp/">UWP (Plataforma Universal do Windows)</a></td>
<td><p>A plataforma de ponta para jogos e aplicativos do Windows 10. Você pode criar aplicativos UWP que usam exclusivamente controles e APIs da UWP ou pode usar controles e APIs da UWP em aplicativos de área de trabalho criados com uma das outras plataformas.</p></td>
<td><a href="/windows/uwp/get-started/">Introdução</a><br/><a href="/uwp/">Referência de API</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">Exemplos</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/windows/win32/">Win32</a></td>
<td><p>A plataforma preferencial para aplicativos nativos do Windows em C/C++ que exigem acesso direto ao Windows e ao hardware.</p></td>
<td><a href="/windows/win32/desktop-programming/">Introdução</a><br/><a href="/windows/win32/apiindex/windows-api-list/">Referência de API</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">Exemplos</a></td>
</tr>
<tr class="odd">
<td><a href="https://docs.microsoft.com/dotnet/framework/wpf/">WPF</a></td>
<td><p>A plataforma baseada em .NET bem estabelecida para aplicativos do Windows gerenciados e avançados graficamente com um modelo de interface do usuário em XAML. Esses aplicativos podem ter como destino o <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> ou o .NET Framework completo.</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">Introdução</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">Referência de API (.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">Exemplos</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/dotnet/framework/winforms/">Windows Forms</a></td>
<td><p>Uma plataforma baseada em .NET criada para aplicativos de linha de negócios gerenciados com um modelo leve de interface do usuário. Esses aplicativos podem ter como destino o <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> ou o .NET Framework completo.</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">Introdução</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">Referência de API (.NET)</a><br/><a href="https://code.msdn.microsoft.com/windowsdesktop/site/search?f%5B0%5D.Type=Technology&f%5B0%5D.Value=Windows%20Forms">Exemplos</a></td>
</tr>
</tbody>
</table>

## <a name="update-existing-desktop-apps-for-windows-10"></a>Atualizar aplicativos existentes da área de trabalho para o Windows 10

Se você tem um aplicativo da área de trabalho do WPF (Windows Presentation Foundation), do Windows Forms ou do Win32 nativo, o Windows 10 e a UWP (Plataforma Universal do Windows) oferecem muitos recursos que você pode usar para fornecer uma experiência moderna em seus aplicativos. A maioria desses recursos está disponível como componentes modulares que você pode adotar para seus aplicativos em seu próprio ritmo, sem reescrever o aplicativo para uma plataforma diferente.

Aqui estão apenas alguns dos recursos disponíveis para aprimorar seus aplicativos de área de trabalho existentes:

* Use o [MSIX](/windows/msix/) para empacotar e implantar aplicativos de área de trabalho. O MSIX é um formato moderno de pacote do aplicativo do Windows que fornece uma experiência de empacotamento universal para todos os aplicativos do Windows. O MSIX reúne os melhores aspectos das tecnologias de instalação MSI, .appx, App-V e ClickOnce e é criado para ser seguro, protegido e confiável.
* Integre seu aplicativo da área de trabalho com experiências do Windows 10 usando as [extensões de pacote](/windows/apps/desktop/modernize/desktop-to-uwp-extensions). Por exemplo, aponte Blocos da tela inicial para seu aplicativo, torne o aplicativo um destino de compartilhamento ou envie notificações do sistema por meio do aplicativo.
* Use [Ilhas XAML](/windows/apps/desktop/modernize/xaml-islands) para hospedar controles XAML da UWP em seu aplicativo da área de trabalho. Muitos dos recursos mais recentes da interface do usuário do Windows 10 só estão disponíveis para controles XAML da UWP.

Para saber mais, consulte estes artigos.

<br/>

| Artigo | Descrição |
|---------|-------------|
| [Modernizar aplicativos da área de trabalho](/windows/apps/desktop/modernize) | Descreve os recursos de desenvolvimento mais recentes do Windows 10 e da UWP que podem ser usados em qualquer aplicativo da área de trabalho, incluindo aplicativos do WPF, do Windows Forms e do C++ Win32. |
| [Tutorial: Modernizar um aplicativo do WPF](/windows/apps/desktop/modernize/modernize-wpf-tutorial) | Siga instruções passo a passo para modernizar um aplicativo existente de linha de negócios de exemplo do WPF adicionando controles de tinta e de calendário da UWP ao aplicativo e empacotando-o em MSIX.  |

## <a name="create-new-desktop-apps"></a>Criar aplicativos de área de trabalho

Se você está criando um aplicativo da área de trabalho para o Windows, veja alguns recursos para ajudá-lo a começar.

<br/>

| Artigo | Descrição |
|---------|-------------|
| [Escolher sua plataforma de aplicativo](choose-your-platform.md) | Fornece uma comparação detalhada das principais plataformas de aplicativos da área de trabalho e pode ajudá-lo a escolher a plataforma certa para suas necessidades. Este artigo também fornece links úteis para documentos de cada plataforma. |
| [Modernizar aplicativos da área de trabalho](/windows/apps/desktop/modernize) | Descreve os recursos de desenvolvimento mais recentes do Windows 10 e da UWP que podem ser usados em qualquer aplicativo da área de trabalho, incluindo aplicativos do WPF, do Windows Forms e do C++ Win32. |
| [Recursos e tecnologias](/windows/apps/features-and-technologies) | Oferece uma visão geral dos recursos do Windows que podem ser acessados por meio de cada uma das principais plataformas de aplicativos da área de trabalho e links para os documentos relacionados. |

## <a name="related-documentation-and-technologies"></a>Documentação e tecnologias relacionadas

| Recurso | Descrição |
|---------|-------------|
| [.NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0) | Saiba mais sobre os recursos mais recentes do .NET Core 3.0, incluindo os aprimoramentos para aplicativos do WPF e do Windows Forms. |
| [Guia da área de trabalho para WPF e .NET Core 3.0](https://docs.microsoft.com/dotnet/desktop-wpf/overview/index) | Desenvolva aplicativos do WPF direcionados ao .NET Core 3.0 em vez do .NET Framework completo.  |
| [Azure](https://docs.microsoft.com/azure/) | Estenda o alcance de seus aplicativos com os serviços de nuvem do Azure. |
| [Visual Studio](https://docs.microsoft.com/visualstudio/) | Saiba como usar o Visual Studio para desenvolver aplicativos e serviços. |
| [MSIX](https://docs.microsoft.com/windows/msix/) | Empacote e implante qualquer aplicativo do Windows em um formato de empacotamento moderno e universal. |
| [IA do Windows](https://docs.microsoft.com/windows/ai/) | Use a IA do Windows para criar soluções inteligentes para problemas complexos em seus aplicativos. |
| [Contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/) | Empacote seus aplicativos com as respectivas dependências em ambientes do Windows rápidos e totalmente isolados. |
| [Aplicativos Web Progressivos](https://docs.microsoft.com/microsoft-edge/progressive-web-apps) | Converta seus aplicativos Web em Aplicativos Web Progressivos que podem ser distribuídos e executados como aplicativos UWP no Windows 10. |
| [Xamarin](https://docs.microsoft.com/xamarin/) | Crie aplicativos multiplataforma para Windows, Android, iOS e macOS usando o código .NET e interfaces de usuário específicas da plataforma. |
| [Arquivos de docs para Windows 8.x e versões anteriores](https://docs.microsoft.com/previous-versions/windows/) | Acesse a documentação arquivada sobre a criação de aplicativos para o Windows 8.x e versões anteriores. |

---
Description: Quando você deseja criar um aplicativo da área de trabalho, a primeira decisão que você toma é se deve usar o Win32 e a API COM ou o .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Escolha sua plataforma de aplicativo
ms.topic: article
ms.date: 11/04/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
keywords: Windows, win32, desenvolvimento de área de trabalho
ms.openlocfilehash: bf8a5970d1999427023592f919ef0b92737fa934
ms.sourcegitcommit: cab95379459ad378163aa4469c9dc6c509cc8c43
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79510979"
---
# <a name="choose-your-app-platform"></a>Escolha sua plataforma de aplicativo

Quando você deseja criar um aplicativo da área de trabalho para computadores Windows, a primeira decisão que você toma é sobre a plataforma de aplicativo a ser usada. O Windows fornece quatro plataformas de aplicativos principais, cada uma com pontos fortes diferentes:

* [UWP (Plataforma Universal do Windows)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

Todas essas plataformas de aplicativos permitem criar aplicativos da área de trabalho como o Word, o Excel e o Photoshop executados na área de trabalho clássica do Windows e aproveitar ao máximo os recursos específicos desse ambiente. No entanto, algumas dessas plataformas compartilham características e são mais adequadas para determinados tipos de aplicativos:

* **UWP**. A plataforma conta com um Common Type System, APIs e um modelo de aplicativo para todos os dispositivos que executam o Windows 10. Os aplicativos UWP podem ser nativos ou gerenciados. 

* **WPF e Windows Forms**. Essas plataformas fornecem um Common Type System, APIs e um modelo de aplicativo para aplicativos gerenciados em execução no .NET.

* **API do Win32**. A API do Win32 (também chamada de API do Windows) é plataforma original para aplicativos nativos do Windows em C/C++ que exigem acesso direto ao Windows e ao hardware. Isso torna a API do Win32 a plataforma preferida para aplicativos que precisam do nível mais alto de desempenho e acesso direto ao hardware do sistema.

Tanto a UWP quanto o .NET têm integração profunda com o Visual Studio. Isso gera muitos benefícios, principalmente nas áreas de produtividade do desenvolvedor, interface do usuário sofisticada e personalizável e segurança do aplicativo. Como essas estruturas dão suporte a designers visuais e marcação de interface do usuário para criar rapidamente a interface do usuário, elas são especialmente adequadas para aplicativos de linha de negócios.

Este artigo descreve essas plataformas com mais detalhes e ajuda a determinar a melhor para seu aplicativo. 

> [!NOTE]
> Não importa qual a plataforma de aplicativo escolhida, você pode usar muitos recursos da UWP (Plataforma Universal do Windows) para proporcionar uma experiência moderna em seu aplicativo no Windows 10. Por exemplo, mesmo que seu aplicativo da área de trabalho seja criado usando o WPF, Windows Forms ou a API do Win32, você ainda pode usar muitos recursos introduzidos pela primeira vez com a UWP, como a implantação de pacote MSIX e os controles UWP XAML. Para obter mais informações, consulte [Modernize seus aplicativos da área de trabalho](modernize/index.md).

## <a name="uwp"></a>UWP

A UWP é a plataforma de ponta para jogos e aplicativos do Windows 10. É uma plataforma altamente personalizável que usa marcação XAML para separar a interface do usuário (apresentação) do código (lógica de negócios). A UWP é adequada para aplicativos da área de trabalho que exigem uma interface do usuário sofisticada, personalização de estilos e cenários com uso intensivo de gráficos. A UWP também tem suporte interno para o [Fluent Design System](/windows/uwp/design/fluent-design-system/) para a experiência padrão do UX e fornece acesso às APIs do [Windows Runtime (WinRT)](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Ao adotar o Fluent, a UWP dá suporte automático a métodos de entrada comuns, como tinta, toque, gamepad, teclado e mouse.

Você não só pode usar a UWP para criar aplicativos da área de trabalho para computadores Windows, mas a UWP também é a única plataforma com suporte para aplicativos Xbox, HoloLens e Surface Hub. A UWP é nossa plataforma de aplicativo mais recente e de ponta.

Para obter mais informações sobre a UWP, confira os seguintes artigos:

* [Introdução](/windows/uwp/get-started/)
* [Design e interface do usuário](/windows/uwp/design/)
* [Recursos e tecnologias](/windows/uwp/develop/)
* [Referência de API](/uwp/)
* [Amostras](https://github.com/Microsoft/Windows-universal-samples)

## <a name="wpf"></a>WPF

O WPF (Windows Presentation Foundation) é a plataforma estabelecida para aplicativos gerenciados do Windows com acesso ao .NET Core ou ao .NET Framework completo, e que usa marcação XAML para separar a interface do usuário do código. Essa plataforma é adequada para aplicativos da área de trabalho que exigem uma interface do usuário sofisticada, personalização de estilos e cenários com uso intensivo de gráficos. As habilidades de desenvolvimento do WPF são semelhantes às habilidades de desenvolvimento da UWP, portanto, a migração do WPF para aplicativos UWP é mais fácil do que a migração de Windows Forms.

Para obter mais informações sobre o WPF, confira os seguintes artigos:

* [Introdução (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).
* [Criar seu primeiro aplicativo (.NET Core)](/visualstudio/get-started/csharp/tutorial-wpf/)
* [Criar seu primeiro aplicativo (.NET Framework)](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application/)
* [Migrar aplicativos do WPF para o .NET Core](/dotnet/desktop-wpf/migration/convert-project-from-net-framework/)
* [Referência de API (.NET)](https://docs.microsoft.com/dotnet/api/index)
* [Amostras](https://github.com/Microsoft/WPF-Samples)

## <a name="windows-forms"></a>Windows Forms

O Windows Forms é a plataforma original para aplicativos gerenciados do Windows com um modelo de interface do usuário leve e acesso ao .NET Core ou ao .NET Framework completo. Ele permite que os desenvolvedores comecem rapidamente a criar aplicativos, mesmo para os desenvolvedores novos na plataforma. Esta é uma plataforma que usa o método RAD, baseada em formulários, com uma grande coleção interna de controles do tipo "arrastar e soltar" de visual e não visual. O Windows Forms não usa XAML. Portanto, decidir posteriormente estender seu aplicativo à UWP implica uma recriação completa da interface do usuário.

Para obter mais informações sobre o Windows Forms, confira os seguintes artigos:

* [Introdução ao Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)
* [Criar seu primeiro aplicativo do Windows Forms](/dotnet/framework/winforms/creating-a-new-windows-form)
* [Tutorial: Criar um visualizador de imagens](/visualstudio/ide/tutorial-1-create-a-picture-viewer?view=vs-2019)
* [Referência de API (.NET)](https://docs.microsoft.com/dotnet/api/index)
* [Aprimorar aplicativos do Windows Forms](/dotnet/framework/winforms/advanced/)

## <a name="win32"></a>Win32

Usar a API do Win32 com C++ permite alcançar os mais altos níveis de desempenho e eficiência ao assumir mais controle da plataforma de destino com código não gerenciado do que é possível em um ambiente de runtime gerenciado como WinRT e .NET. No entanto, exercer esse nível de controle sobre a execução de seu aplicativo requer maior cuidado e atenção para dar certo, além de negociar a produtividade de desenvolvimento para o desempenho do runtime.

Aqui estão alguns destaques do que a API do Win32 e C++ oferece para permitir que você crie aplicativos de alto desempenho.

* Otimizações de nível de hardware, incluindo controle rígido sobre alocação de recurso, tempos de vida do objeto, layout de dados, alinhamento, empacotamento de bytes e muito mais.
* Acesso a conjuntos de instruções orientadas por desempenho como SSE e AVX por meio de funções intrínsecas.
* Programação genérica eficiente e fortemente tipada usando modelos.
* Contêineres e algoritmos eficientes e seguros.
* DirectX, especialmente Direct3D e DirectCompute (observe que a UWP também oferece interoperabilidade com DirectX).

Para obter mais informações, confira os seguintes artigos:

* [Introdução](/windows/win32/desktop-programming/)
* [Criar seu primeiro aplicativo Win32 e C++](/windows/win32/learnwin32/learn-to-program-for-windows/)
* [Recursos e tecnologias](/windows/win32/desktop-app-technologies)
* [Referência de API](/windows/win32/apiindex/windows-api-list/)
* [Amostras](https://github.com/Microsoft/Windows-classic-samples)

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparação de plataformas: UWP, WPF e Windows Forms

A tabela a seguir compara várias características do Windows Forms, do WPF e da UWP em detalhes.

| Recurso ou cenário  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versões compatíveis**      |  Windows 10   |  Windows 7 e posterior |  Windows 7 e posterior  |
| **Linguagens**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (Managed Extensions for C++), F\#, VB |  C\#, C++/CLI (Managed Extensions for C++), F\#, VB   |
| **Runtime da interface do usuário** |    Nativo (C++/WinRT e C++/CX) e gerenciado (.NET Native)  |  Gerenciado (.NET Framework e .NET Core 3) |   Gerenciado (.NET Framework e .NET Core 3)   |
| **Software livre** | [Sim (somente na biblioteca de interface do usuário do Windows)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Sim (somente no .NET Core)](https://github.com/dotnet/wpf) | [Sim (somente no .NET Core)](https://github.com/dotnet/winforms)  |
| **Dá suporte a XAML** |   Sim   |  Sim  |   Não   |
| **Pontos fortes**  |  <ul><li>Marcação XAML para interface do usuário</li><li>UX rica e personalizável</li><li>Suas bases de código existentes estão em conformidade com .NET Standard</li><li>Suporte a DPI alto</li><li>Suporte para vários tipos de entrada em dispositivos Windows (incluindo toque, caneta, gamepad, mouse e teclado)</li><li>Suporte para Xbox, HoloLens, IoT ou Surface Hub</li><li>Interface do usuário densa (compacta) opcional</li><li>Suporte para C++ nativo</li><li>Duração da bateria otimizada</li><li>Suporte à acessibilidade moderna (como leitores de tela)</li><li>Funcionalidades de dados de Rich Text (como a verificação ortográfica interna)</li><li>Suporte à escrita à tinta</li><li>Execução segura por meio de contêineres de aplicativo (por exemplo, conteúdo não confiável está em área restrita)</li></ul>  |  <ul><li>Marcação XAML para interface do usuário</li><li>UX rica e personalizável</li><li>Grande coleção de controles da Microsoft e de parceiros</li><li>Interface do usuário densa</li><li>Suporte para o Windows 7</li><li>Suporte de plataforma para validação de entrada</li></ul> | <ul><li>Método RAD</li><li>Editor WYSIWYG para criar a interface do usuário</li><li>Grande coleção de controles da Microsoft e de parceiros</li><li>Interface do usuário densa</li><li>Suporte para o Windows 7</li><li>Entrada de mouse e teclado</li></ul>          |
| **Cenários com suporte limitado** |  <ul><li>Suporte a várias janelas<sup>1</sup></li><li>Suporte de plataforma para validação de entrada<sup>1</sup></li><li>Não há suporte para Windows 7</li><li>Algumas APIs da UWP exigem versões mínimas específicas do Windows 10</li><li>Suporte completo à plataforma e integração de shell (por exemplo, a UWP atualmente não dá suporte à integração da bandeja do sistema nem acesso completo a todos os dispositivos)</li><li>Acesso direto a todos os arquivos em disco</li><li>ADO.NET</li><li>Bibliotecas de classes base de código existentes que usam APIs em conformidade com o Kit de Certificação de Aplicativos não .NET Standard ou não Windows</li><li>Suporte a loopback de rede local (ou seja, se o seu aplicativo precisar se comunicar com o localhost sem criar uma isenção de loopback no dispositivo de destino)</li><li>E/S de arquivo intensiva</li></ul>     |  <ul><li>Suporte a DPI alto<sup>2</sup></li><li>Entrada por toque<sup>2</sup></li></ul>  |  <ul><li>Suporte a DPI alto<sup>2</sup></li><li>Entrada por toque<sup>2</sup></li><li>Interface do usuário personalizável</li><li>Gráficos avançados e experiências do usuário (como toque e animações)</li><li>Abstração avançada de exibições e modelos de dados</li></ul>    |   |

<sup>1</sup> Anunciamos recursos publicamente que abordarão esse cenário em uma versão futura do Windows 10.

<sup>2</sup> Embora a plataforma não tenha suporte de API de primeira classe para esse cenário, os desenvolvedores podem oferecer suporte a esse cenário com soluções alternativas.

## <a name="other-app-platforms"></a>Outras plataformas de aplicativos

### <a name="progressive-web-apps-pwas"></a>PWAs (Aplicativos Web Progressivos)

PWAs permitem que os desenvolvedores empacotem o código do site para que ele possa ser instalado e executado como um aplicativo em computadores e tablets com Windows 10. Para obter mais informações, consulte [Aplicativos Web Progressivos](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Use o Xamarin para criar aplicativos multiplataforma para Windows 10 que também podem ser executados no iOS e no Android. Para obter mais informações, consulte [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).

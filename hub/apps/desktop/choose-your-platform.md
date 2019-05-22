---
Description: Quando você deseja criar um novo aplicativo de área de trabalho, a primeira decisão de que fazer é se deseja usar o Win32 e a API COM ou .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Escolha sua plataforma de aplicativo
ms.topic: article
ms.date: 03/18/2019
ms.openlocfilehash: 960dda5e4cb7e8edc1edf7ce2e81da8306555f1c
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984486"
---
# <a name="choose-your-app-platform"></a>Escolha sua plataforma de aplicativo

Quando você deseja criar um novo aplicativo de desktop para PCs Windows, a primeira decisão que você faz é qual plataforma de aplicativo para usar. Windows fornece quatro plataformas do aplicativo principal, cada um com diferentes pontos fortes:

* [UWP (Plataforma Universal do Windows)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

Todas essas plataformas de aplicativo permitem que você crie aplicativos da área de trabalho, como Word, Excel e Photoshop que são executados no clássico Windows desktop e execute todas as vantagens de recursos específicos do ambiente. No entanto, algumas dessas plataformas compartilham algumas características e são mais adequadas para determinados tipos de aplicativos:

* **Windows Forms, WPF e UWP**. Essas plataformas fornecem ambientes de tempo de execução gerenciado (o Windows Runtime para UWP e o .NET para Windows Forms e WPF) com muitos benefícios, especialmente nas áreas de produtividade do desenvolvedor, sofisticado e personalizável da interface do usuário e segurança do aplicativo. Como essas estruturas oferecem suporte a designers visuais e marcação da interface do usuário para criar rapidamente a interface do usuário, elas são especialmente adequadas para aplicativos de linha de negócios.

* **API do Win32**. A API do Win32 (também chamada de API do Windows) é a plataforma original para nativo C /C++ aplicativos do Windows que exigem acesso direto ao hardware e do Windows. Ele fornece uma experiência de desenvolvimento de primeira classe sem dependendo de um ambiente de tempo de execução gerenciado, como .NET e do WinRT. Isso torna a API do Win32 para a plataforma ideal para aplicativos que precisam de mais alto nível de desempenho e acesso direto ao hardware do sistema.

Este artigo descreve essas plataformas mais detalhadamente e ajuda você a determinar a melhor para seu aplicativo.

## <a name="uwp"></a>UWP

UWP é a plataforma de ponta para jogos e aplicativos do Windows 10. É uma plataforma altamente personalizável que usa a marcação XAML para separar a experiência do usuário (apresentação) do código (lógica de negócios). UWP é adequada para aplicativos da área de trabalho que exigem uma interface do usuário sofisticada, personalização de estilos e cenários intensivos de elementos gráficos. UWP também tem suporte interno para o [Fluent Design System](/windows/uwp/design/fluent-design-system/) para o padrão UX experiência e fornece acesso para o [tempo de execução do Windows (WinRT) APIs](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Ao adotar Fluent, UWP automaticamente dá suporte a métodos comuns de entrada como tinta, toque, gamepad, teclado e mouse.

Não só você pode usar UWP para criar aplicativos de desktop para PCs Windows, mas UWP também é a única plataforma com suporte para aplicativos Xbox, HoloLens e Surface Hub. UWP é nossa plataforma de aplicativo mais recente, a borda esquerda.

Para obter mais informações sobre a UWP, consulte [Introdução aos aplicativos do Windows 10](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

O WPF é a plataforma estabelecida para aplicativos gerenciados do Windows com acesso para o .NET Framework completo, e ele também usa marcação XAML para separar a experiência do usuário do código. Essa plataforma foi projetada para aplicativos da área de trabalho que exigem uma interface do usuário sofisticada, personalização de estilos e cenários intensivos de elementos gráficos. Habilidades de desenvolvimento do WPF são semelhantes às habilidades de desenvolvimento de UWP, portanto, a migração de WPF para aplicativos UWP é mais fácil do que a migração do Windows Forms.

Para obter mais informações sobre o WPF, consulte [guia de Introdução (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms é a plataforma original para aplicativos gerenciados do Windows com um modelo de interface do usuário leve e acesso para o .NET Framework completo. Ele se destaca em permitindo que os desenvolvedores a começar rapidamente a criação de aplicativos, mesmo para os desenvolvedores de novos na plataforma. Isso é uma plataforma de desenvolvimento de aplicativo baseada em formulários e rápida com uma grande coleção interna de controles de arrastar e soltar de visuais e não visuais. Windows Forms não usa XAML, portanto, decidir mais tarde estender seu aplicativo para UWP envolve uma reescrita completa da sua interface do usuário.

Para obter mais informações sobre o Windows Forms, consulte [Introdução ao Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparação de plataforma: UWP, WPF e Windows Forms

A tabela a seguir compara várias características dos Windows Forms, WPF e UWP em detalhes.

| Cenário ou recurso  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versões com suporte**      |  Windows 10   |  Windows 7 e posterior |  Windows 7 e posterior  |
| **Idiomas**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (Managed Extensions para C++), F\#, VB |  C\#, C++/CLI (Managed Extensions para C++), F\#, VB   |
| **Tempo de execução da interface do usuário** |    Nativo (C++/WinRT e C++/CX) e gerenciados (.NET nativo)  |  Gerenciado (.NET Framework)<br/><br/>Suporte para .NET Core 3 está disponível em breve  |   Gerenciado (.NET Framework)<br/><br/>Suporte para .NET Core 3 está disponível em breve    |
| **Código-fonte aberto** | [Sim (somente para biblioteca de interface do usuário Windows)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Sim (somente .NET Core)](https://github.com/dotnet/wpf) | [Sim (somente .NET Core)](https://github.com/dotnet/winforms)  |
| **Dá suporte a XAML** |   Sim   |  Sim  |   Não   |
| **Pontos fortes**  |  <ul><li>Marcação XAML para interface do usuário</li><li>Experiência de usuário rica e personalizável</li><li>As bases de código existentes são compatíveis com .NET Standard</li><li>Suporte para DPI alto</li><li>Suporte para vários tipos de entrada em todos os dispositivos do Windows (incluindo toque, caneta, gamepad, mouse e teclado)</li><li>Suporte para Xbox, HoloLens, IoT ou Surface Hub</li><li>Suporte nativosC++</li><li>Vida útil da bateria otimizada</li><li>Suporte de acessibilidade moderna (como leitores de tela)</li><li>Recursos de dados de texto avançados (como de verificação ortográfica interno)</li><li>Suporte a tintas</li><li>Proteger a execução por meio de contêineres de aplicativo (por exemplo, não-confiáveis de conteúdo é uma área restrita)</li></ul>  |  <ul><li>Marcação XAML para interface do usuário</li><li>Experiência de usuário rica e personalizável</li><li>Grande coleção de controles da Microsoft e parceiros</li><li>Interface do usuário densa</li><li>Suporte para o Windows 7</li><li>Suporte de plataforma para validação de entrada</li></ul> | <ul><li>desenvolvimento rápido de aplicativos</li><li>Editor WYSIWYG para criar a interface do usuário</li><li>Grande coleção de controles da Microsoft e parceiros</li><li>Interface do usuário densa</li><li>Suporte para o Windows 7</li><li>Entrada de mouse e teclado</li></ul>          |
| **Cenários que têm suporte limitado** |  <ul><li>Interface do usuário densa (Criando uma interface do usuário densa requer estilos personalizados)<sup>1</sup></li><li>Suporte a vários janela<sup>1</sup></li><li>Suporte de plataforma para validação de entrada<sup>1</sup></li><li>Não há suporte para o Windows 7</li><li>Algumas APIs de UWP exigem versões específicas de mínimas do Windows 10</li><li>Total integração de shell e suporte de plataforma (por exemplo, UWP atualmente não dá suporte a integração de bandeja do sistema ou acesso completo a todos os dispositivos)</li><li>Acesso direto a todos os arquivos no disco</li><li>ADO.NET</li><li>Bibliotecas de classe base de código existentes que usam não - .NET Standard ou APIs de não - Windows Kit de certificação em conformidade</li><li>Suporte a loopback de rede local (ou seja, se seu aplicativo precisa para se comunicar com o host local sem criar uma isenção de loopback no dispositivo de destino)</li><li>Arquivo com uso intensivo de e/s</li></ul>     |  <ul><li>Suporte para DPI alto<sup>2</sup></li><li>Entrada por toque<sup>2</sup></li></ul>  |  <ul><li>Suporte para DPI alto<sup>2</sup></li><li>Entrada por toque<sup>2</sup></li><li>Interface do usuário personalizável</li><li>Experiências de usuário e gráficos avançados (como toque e animações)</li><li>Abstração avançada de modos de exibição e modelos de dados</li></ul>    |   |

<sup>1</sup> publicamente foram anunciadas recursos que solucionará esse cenário em uma versão futura do Windows 10.

<sup>2</sup> Embora a plataforma não tem suporte de API de primeira classe para esse cenário, os desenvolvedores podem dar suporte a esse cenário com soluções alternativas.

## <a name="win32"></a>Win32

Usando a API do Win32 com C++ torna possível alcançar os mais altos níveis de desempenho e eficiência fazendo mais controle da plataforma de destino com código não gerenciado que é possível em um ambiente de tempo de execução gerenciado como WinRT e .NET. No entanto, a exercitar um nível de controle sobre a execução do seu aplicativo requer maior cuidado e atenção para acertar e negocia a produtividade de desenvolvimento para o desempenho de tempo de execução.

Aqui estão alguns destaques da API do Win32 e C++ oferece para que você possa criar aplicativos de alto desempenho.

-   Otimizações de nível de hardware, incluindo um controle rígido sobre a alocação de recursos, tempos de vida do objeto, layout de dados, alinhamento, bytes de remessa e muito mais.
-   Acesso a instrução orientado ao desempenho define como o SSE e AVX por meio de funções intrínsecas.
-   Programação genérica eficiente e fortemente tipada usando modelos.
-   Seguros e eficientes de contêineres e algoritmos.
-   DirectX, em particular Direct3D e DirectCompute (Observe que a UWP também oferece interoperabilidade DirectX).
-   C++ AMP.

Para obter mais informações, consulte [Introdução aos aplicativos da área de trabalho do Windows que usam a API do Win32](/windows/desktop/desktop-programming) e [tecnologias de aplicativo da área de trabalho](/windows/desktop/desktop-app-technologies).

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 e C++ para os aplicativos tradicionais da área de trabalho

Ao escrever um aplicativo da área de trabalho no C++, você pode escolher o MFC ou Win32 para a interface do usuário, ou uma série de estruturas de aplicativo de terceiros que também dão suporte a plataformas não Windows.

-   **Win32:** Essa é a API baseado no identificador, linguagem C da plataforma Windows, incluindo, mas não limitado à funcionalidade da interface do usuário, como janelas, o desenho e a interface do usuário, controles. Porque é uma API de nível baixo de linguagem C com base em identificadores, é uma opção pouco frequente para a criação de aplicativos modernos e intensivo de interface do usuário. No entanto, ele fornece as APIs básicas necessárias para interagir com a plataforma Windows e é uma escolha adequada para aplicativos que têm requisitos de interface do usuário simples ou que deseja apenas a Windows interface do usuário para se manter fora do caminho tanto quanto possível, por exemplo, jogos.
-   **MFC (biblioteca de classes Microsoft Foundation):** Esta é a estrutura de aplicativo respeitável e a biblioteca de interface do usuário que tenha atendido aos desenvolvedores do Windows desde 1992. É um thin C++ wrapper de API do Win32 com base no identificador, linguagem C e fornece interfaces orientadas a objeto para muitos do windows predefinido, controles comuns e outros objetos do Windows. Embora muitas estruturas de interface do usuário modernas no ecossistema do .NET ultrapassam o MFC em sua conveniência, ainda é a estrutura de interface do usuário nativa de escolha para muitos C++ os desenvolvedores que criam aplicativos para a área de trabalho do Windows.
-   **Estruturas de aplicativo de terceiros:** Porque C++ pode ser executado em uma ampla variedade de plataformas e não está vinculado ao Windows ou o tempo de execução do .NET, por terceiros desenvolveram novo aplicativo e estruturas de interface do usuário para o C++ para facilitar o desenvolvimento de aplicativos de plataforma cruzada com interfaces de usuário avançadas. Algumas dessas estruturas fornecem suas próprias aparência e sensação, enquanto outros como wxWidgets ou Qt usam ou emulam o conjunto de controles nativos da plataforma. Usando essas bibliotecas, é possível compartilhar praticamente todo o código-fonte do aplicativo entre as versões do aplicativo que são executados no Windows ou em outras plataformas, como OSX ou Linux.

## <a name="other-app-platforms"></a>Outras plataformas de aplicativos

### <a name="progressive-web-apps-pwas"></a>Aplicativos Web progressivo (PWAs)

PWAs permitem que o pacote de desenvolvedores, seu site de código para que ele pode ser instalado e executado como um aplicativo em computadores Windows 10. Para obter mais informações, consulte [aplicativos de Web progressivo](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Use o Xamarin para criar aplicativos de plataforma cruzada para Windows 10 que também podem ser executados no iOS e Android. Para obter mais informações, consulte [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).

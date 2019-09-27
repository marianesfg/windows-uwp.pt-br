---
Description: Quando você deseja criar um novo aplicativo de área de trabalho, a primeira decisão que você toma é usar o Win32 e a API COM ou o .NET.
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: Escolha sua plataforma de aplicativo
ms.topic: article
ms.date: 03/18/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: ff32e20c42f613bd9ac3dba9eada2cced0baa64c
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316998"
---
# <a name="choose-your-app-platform"></a>Escolha sua plataforma de aplicativo

Quando você deseja criar um novo aplicativo de área de trabalho para computadores Windows, a primeira decisão que você faz é a plataforma de aplicativo a ser usada. O Windows fornece quatro plataformas de aplicativos principais, cada uma com pontos fortes diferentes:

* [UWP (Plataforma Universal do Windows)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

Todas essas plataformas de aplicativos permitem criar aplicativos de área de trabalho como o Word, o Excel e o Photoshop executados na área de trabalho clássica do Windows e aproveitar ao máximo os recursos específicos desse ambiente. No entanto, algumas dessas plataformas compartilham algumas características e são mais adequadas para determinados tipos de aplicativos:

* **UWP, WPF e Windows Forms**. Essas plataformas fornecem ambientes de tempo de execução gerenciados (o Windows Runtime para UWP e .NET para Windows Forms e WPF) com muitos benefícios, especialmente nas áreas de produtividade do desenvolvedor, interface do usuário sofisticada e personalizável e segurança de aplicativos. Como essas estruturas dão suporte a designers visuais e marcação de interface do usuário para criar rapidamente a interface do usuário, elas são especialmente adequadas para aplicativos de linha de negócios.

* **API do Win32**. A API do Win32 (também chamada de API do Windows) é a plataforma original para aplicativosC++ nativos C/Windows que exigem acesso direto ao Windows e ao hardware. Ele fornece uma experiência de desenvolvimento de primeira classe sem depender de um ambiente de tempo de execução gerenciado como .NET e WinRT. Isso torna a API do Win32 a plataforma preferida para aplicativos que precisam do nível mais alto de desempenho e acesso direto ao hardware do sistema.

Este artigo descreve essas plataformas com mais detalhes e ajuda a determinar a melhor para seu aplicativo. 

> [!NOTE]
> Não importa qual plataforma de aplicativo você escolher, você pode usar muitos recursos do Plataforma Universal do Windows (UWP) para fornecer uma experiência moderna em seu aplicativo no Windows 10. Por exemplo, mesmo que seu aplicativo de desktop seja criado usando o WPF, Windows Forms ou a API do Win32, você ainda pode usar muitos recursos introduzidos pela primeira vez com UWP, como a implantação de pacote MSIX e os controles UWP XAML. Para obter mais informações, consulte [modernizar seus aplicativos de desktop](modernize/index.md).

## <a name="uwp"></a>UWP

A UWP é a plataforma de ponta para aplicativos e jogos do Windows 10. É uma plataforma altamente personalizável que usa marcação XAML para separar a UX (apresentação) do código (lógica de negócios). O UWP é adequado para aplicativos de área de trabalho que exigem uma interface do usuário sofisticada, personalização de estilos e cenários com uso intensivo de gráficos. O UWP também tem suporte interno para o [sistema de design fluente](/windows/uwp/design/fluent-design-system/) para a experiência padrão de UX e fornece acesso às [APIs de Windows Runtime (WinRT)](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis). Ao adotar o Fluent, o UWP oferece suporte automático a métodos de entrada comuns, como tinta, toque, gamepad, teclado e mouse.

Você não só pode usar o UWP para criar aplicativos de área de trabalho para computadores Windows, mas a UWP também é a única plataforma com suporte para aplicativos Xbox, HoloLens e Surface Hub. A UWP é nossa plataforma de aplicativo mais recente e de ponta.

Para obter mais informações sobre o UWP, consulte Introdução [aos aplicativos do Windows 10](/windows/uwp/get-started/).

## <a name="wpf"></a>WPF

O WPF é a plataforma estabelecida para aplicativos gerenciados do Windows com acesso ao .NET Framework completo e também usa marcação XAML para separar a UX do código. Essa plataforma é projetada para aplicativos de área de trabalho que exigem uma interface do usuário sofisticada, personalização de estilos e cenários com uso intensivo de gráficos. As habilidades de desenvolvimento do WPF são semelhantes às habilidades de desenvolvimento da UWP, portanto, a migração do WPF para aplicativos UWP é mais fácil do que a migração de Windows Forms.

Para obter mais informações sobre o WPF, consulte [introdução (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/).

## <a name="windows-forms"></a>Windows Forms

Windows Forms é a plataforma original para aplicativos gerenciados do Windows com um modelo de interface do usuário leve e acesso ao .NET Framework completo. Ele permite que os desenvolvedores comecem rapidamente a criar aplicativos, mesmo para os desenvolvedores novos na plataforma. Esta é uma plataforma de desenvolvimento de aplicativos rápida, baseada em formulários, com uma grande coleção interna de controles de arrastar e soltar de Visual e não visual. Windows Forms não usa XAML, portanto, decidir posteriormente estender seu aplicativo ao UWP implica uma regravação completa da interface do usuário.

Para obter mais informações sobre Windows Forms, consulte [introdução ao Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms).

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>Comparação de plataforma: UWP, WPF e Windows Forms

A tabela a seguir compara várias características de Windows Forms, WPF e UWP em detalhes.

| Recurso ou cenário  |    UWP     |      WPF     |   Windows Forms  |
|--------|--------|--------|--------|
| **Versões com suporte**      |  Windows 10   |  Windows 7 e posterior |  Windows 7 e posterior  |
| **Idiomas**      |   C\#, C++/WinRT, C++/CX, VB, JavaScript   |  C\#, C++/CLI (extensões gerenciadas C++para),\#F, VB |  C\#, C++/CLI (extensões gerenciadas C++para),\#F, VB   |
| **Tempo de execução da IU** |    Nativo (C++/WinRT e C++/CX) e gerenciado (.net Native)  |  Gerenciado (.NET Framework e .NET Core 3) |   Gerenciado (.NET Framework e .NET Core 3)   |
| **Código-fonte aberto** | [Sim (somente biblioteca da interface do usuário do Windows)](https://github.com/Microsoft/microsoft-ui-xaml)  |  [Sim (somente no .NET Core)](https://github.com/dotnet/wpf) | [Sim (somente no .NET Core)](https://github.com/dotnet/winforms)  |
| **Dá suporte a XAML** |   Sim   |  Sim  |   Não   |
| **Negocia**  |  <ul><li>Marcação XAML para interface do usuário</li><li>UX rica e personalizável</li><li>Suas bases de código existentes estão em conformidade .NET Standard</li><li>Suporte a DPI alto</li><li>Suporte para vários tipos de entrada em dispositivos Windows (incluindo toque, caneta, gamepad, mouse e teclado)</li><li>Suporte para Xbox, HoloLens, IoT ou Surface Hub</li><li>Suporte para nativoC++</li><li>Vida útil otimizada da bateria</li><li>Suporte à acessibilidade moderna (como leitores de tela)</li><li>Funcionalidades de dados de Rich Text (como a verificação ortográfica interna)</li><li>Suporte à tinta</li><li>Execução segura por meio de contêineres de aplicativo (por exemplo, conteúdo não confiável está em área restrita)</li></ul>  |  <ul><li>Marcação XAML para interface do usuário</li><li>UX rica e personalizável</li><li>Grande coleção de controles da Microsoft e de parceiros</li><li>Interface do usuário densa</li><li>Suporte para o Windows 7</li><li>Suporte de plataforma para validação de entrada</li></ul> | <ul><li>Desenvolvimento rápido de aplicativos</li><li>Editor WYSIWYG para criar a interface do usuário</li><li>Grande coleção de controles da Microsoft e de parceiros</li><li>Interface do usuário densa</li><li>Suporte para o Windows 7</li><li>Entrada de teclado e mouse</li></ul>          |
| **Cenários com suporte limitado** |  <ul><li>Interface do usuário densa (a criação de uma interface de usuário densada requer estilos personalizados)<sup>1</sup></li><li>Suporte a várias janelas<sup>1</sup></li><li>Suporte de plataforma para validação de entrada<sup>1</sup></li><li>Não há suporte para o Windows 7</li><li>Algumas APIs UWP exigem versões mínimas específicas do Windows 10</li><li>Suporte completo à plataforma e integração de Shell (por exemplo, UWP atualmente não dá suporte à integração da bandeja do sistema ou acesso completo a todos os dispositivos)</li><li>Acesso direto a todos os arquivos em disco</li><li>ADO.NET</li><li>Bibliotecas de classes de base de código existentes que usam APIs compatíveis com o kit de certificação de aplicativos non-.NET Standard ou não Windows</li><li>Suporte a loopback de rede local (ou seja, se seu aplicativo precisar se comunicar com o localhost sem criar uma isenção de loopback no dispositivo de destino)</li><li>E/s de arquivo intensiva</li></ul>     |  <ul><li>Suporte a DPI alto<sup>2</sup></li><li>Entrada por toque<sup>2</sup></li></ul>  |  <ul><li>Suporte a DPI alto<sup>2</sup></li><li>Entrada por toque<sup>2</sup></li><li>Interface do usuário personalizável</li><li>Gráficos avançados e experiências do usuário (como toque e animações)</li><li>Abstração avançada de exibições e modelos de dados</li></ul>    |   |

<sup>1</sup> anunciamos recursos publicamente que abordarão esse cenário em uma versão futura do Windows 10.

<sup>2</sup> embora a plataforma não tenha suporte de API de primeira classe para esse cenário, os desenvolvedores podem dar suporte a esse cenário com soluções alternativas.

## <a name="win32"></a>Win32

Usar a API do Win32 C++ com o torna possível alcançar os mais altos níveis de desempenho e eficiência ao assumir mais controle da plataforma de destino com código não gerenciado do que é possível em um ambiente de tempo de execução gerenciado como WinRT e .net. No entanto, exercer esse nível de controle sobre a execução de seu aplicativo requer maior cuidado e atenção para ficar certo, além de negociar a produtividade de desenvolvimento para o desempenho do tempo de execução.

Aqui estão alguns destaques do que a API do Win32 e C++ oferece para permitir que você crie aplicativos de alto desempenho.

-   Otimizações de nível de hardware, incluindo controle rígido sobre alocação de recursos, tempos de vida de objeto, layout de dados, alinhamento, empacotamento de bytes e muito mais.
-   Acesso a conjuntos de instruções orientadas a desempenho como SSE e AVX por meio de funções intrínsecas.
-   Programação genérica eficiente e segura de tipo usando modelos.
-   Contêineres e algoritmos eficientes e seguros.
-   DirectX, especialmente em Direct3D e DirectCompute (Observe que o UWP também oferece interoperabilidade com DirectX).
-   C++BLOGS.

Para obter mais informações, consulte Introdução [aos aplicativos da área de trabalho do Windows que usam a API do Win32](/windows/desktop/desktop-programming) e [as tecnologias de aplicativo da área de trabalho](/windows/desktop/desktop-app-technologies).

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 e C++ para aplicativos de área de trabalho tradicionais

Ao escrever um aplicativo de área C++de trabalho no, você pode escolher Win32 ou MFC para a interface do usuário ou um host de estruturas de aplicativos de terceiros que também dão suporte a plataformas não Windows.

-   **Win32** Essa é a API C-Language baseada em identificadores da plataforma Windows, incluindo, mas não se limitando a funcionalidades da interface do usuário, como janelas, desenho e controles de interface do usuário. Como se trata de uma API de linguagem C de nível baixo baseada em identificadores, é uma opção pouco frequente para a criação de aplicativos modernos de uso intensivo de interface do usuário. No entanto, ele fornece as APIs básicas necessárias para interagir com a plataforma Windows e é uma opção adequada para aplicativos que têm requisitos de interface do usuário simples ou que apenas querem que a interface do usuário do Windows fique fora do caminho o mais possível, por exemplo, jogos.
-   **MFC (biblioteca MFC):** Esta é a estrutura de aplicativo respeitável e a biblioteca de interface do usuário que servia os desenvolvedores do Windows desde 1992. É um wrapper fino C++ sobre a API Win32 C-Language, baseada em identificadores, e fornece interfaces orientadas a objeto para muitas das janelas predefinidas, controles comuns e outros objetos do Windows. Embora muitas estruturas de interface do usuário modernas no ecossistema do .NET superem o MFC de conveniência, ela ainda é a estrutura de interface C++ do usuário nativa escolhida para muitos desenvolvedores que criam aplicativos para a área de trabalho do Windows.
-   **Estruturas de aplicativo de terceiros:** Como C++ o pode ser executado em uma ampla variedade de plataformas e não está vinculado ao tempo de execução do .net, terceiros desenvolveram novas estruturas de aplicativo e de C++ interface do usuário para facilitar o desenvolvimento de aplicativos de plataforma cruzada com interfaces de usuário avançadas. Algumas dessas estruturas fornecem sua própria aparência & se sentem, enquanto outras, como wxWidgets ou Qt, usam ou emulam o conjunto de controle nativo da plataforma. Usando essas bibliotecas, é possível compartilhar quase todos os códigos-fonte de um aplicativo entre as versões do aplicativo que são executadas no Windows ou em outras plataformas, como OSX ou Linux.

## <a name="other-app-platforms"></a>Outras plataformas de aplicativos

### <a name="progressive-web-apps-pwas"></a>Aplicativos Web progressivos (PWAs)

O PWAs permite que os desenvolvedores Empacotem seu código de site para que possam ser instalados e executados como um aplicativo em PCs com Windows 10. Para obter mais informações, consulte [aplicativos Web progressivos](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started).

### <a name="xamarin"></a>Xamarin

Use o Xamarin para criar aplicativos de plataforma cruzada para o Windows 10 que também podem ser executados no iOS e no Android. Para obter mais informações, consulte [Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index).

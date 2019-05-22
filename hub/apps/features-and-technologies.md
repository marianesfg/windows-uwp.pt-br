---
Description: Esta seção ajuda você a entender como determinados recursos principais do Windows têm suporte em plataformas diferentes do aplicativo e como começar a usar os recursos em seu código.
title: Recursos e tecnologias
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: ff91d8c01e6832e645cc857b638851e1833fc3f9
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984976"
---
# <a name="features-and-technologies-for-windows-apps"></a>Recursos e tecnologias para aplicativos do Windows

Independentemente de qual tipo de aplicativo que você está criando ou dispositivo de que destino, o Windows dá suporte a muitos recursos que são os principais blocos de construção para cenários de aplicativo importantes. Alguns desses recursos são expostos para o Universal Windows Platform (UWP), Win32 (API do Windows) e outras plataformas de aplicativo de maneiras diferentes. Os seguintes artigos ajudarão você a entender como determinados recursos do Windows têm suporte em plataformas diferentes do aplicativo e como começar a usar os recursos em seu código.

Este artigo fornece uma lista personalizada de artigos para ler mais sobre como você pode acessar recursos importantes do Windows e tecnologias na UWP, Win32 (API do Windows), o WPF e plataformas de aplicativo do Windows Forms. Para obter informações completas sobre os recursos de desenvolvimento de cada plataforma, consulte os seguintes recursos:

* [Documentação da UWP](/windows/uwp/index)
* [Documentação do Win32 (API do Windows)](/windows/desktop/index)
* [Documentação do WPF](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Documentação do Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Tecnologias e recursos do Windows de chave

As seções a seguir destacam vários recursos do Windows e tecnologias que permitem que você forneça modernos e entregar experiências atraentes para seus clientes importantes.

### <a name="windows-ink"></a>Windows Ink

![Caneta Surface](images/hero-small.png)  

A plataforma Windows Ink, juntamente com um dispositivo de caneta, oferece uma maneira natural de criar anotações manuscritas digitais, desenhos e anotações. A plataforma dá suporte à captura de dados de tinta por entrada de digitalizador, gerenciamento e geração de dados de tinta, renderização desses dados como traços de tinta no dispositivo de saída e conversão de tinta em texto por meio do reconhecimento de manuscrito.

Para obter mais informações sobre as diferentes maneiras de usar o Windows Ink em aplicativos do Windows, consulte [tinta Windows](/windows/uwp/design/input/pen-and-stylus-interactions).

### <a name="speech-interactions"></a>Interações de controle por voz

![tela de reconhecimento inicial para uma restrição baseada em um arquivo de gramática sgrs](images/speech-listening-initial.png)

![tela de reconhecimento final para uma restrição baseada em um arquivo de gramática sgrs](images/speech-listening-complete.png)

Windows fornece várias maneiras de integrar o reconhecimento de fala e texto em fala (também conhecido como TTS ou síntese de fala) diretamente na experiência do usuário do seu aplicativo. Conversão de fala pode ser uma maneira agradável e robusta para que as pessoas interagem com seu aplicativo, complementando ou até mesmo substituir, teclado, mouse, toque e gestos.

Para obter mais informações sobre as diferentes maneiras de usar as interações de fala em aplicativos do Windows, consulte [interações de voz](/windows/uwp/design/input/speech-interactions).

### <a name="windows-ai"></a>IA do Windows

![IA do Windows](images/windows-ai.png)

Oferecemos várias soluções de inteligência Artificial diferentes que você pode usar para aprimorar seus aplicativos do Windows. Com o aprendizado de máquina do Windows, você pode integrar modelos de aprendizado em seus aplicativos de máquina treinada e executá-los localmente no dispositivo. Habilidades de visão do Windows permite que você use bibliotecas criadas previamente para realizar tarefas de processamento de imagens comuns ou crie suas próprias soluções personalizadas. DirectML fornece baixo nível, estilo de DirectX APIs que permitem que você aproveitar ao máximo o hardware.

Para obter mais informações sobre as diferentes maneiras de integrar a inteligência Artificial em aplicativos do Windows, consulte [AI Windows](https://docs.microsoft.com/windows/ai/).

## <a name="features-and-technologies-by-platform"></a>Recursos e as tecnologias de plataforma

As seções a seguir fornece links úteis para saber mais sobre como integrar com os principais recursos do Windows e tecnologias de nossas plataformas de aplicativos principal: UWP, Win32 (Windows API), Windows Forms e WPF.

### <a name="user-interface-and-accessibility"></a>Acessibilidade e interface do usuário

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Projetar](/windows/uwp/design/basics/)<br/><br/>[Layout](/windows/uwp/design/layout/)<br/><br/>[Controles](/windows/uwp/design/controls-and-patterns/)<br/><br/>[entrada](/windows/uwp/design/input/)<br/><br/>[Blocos](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Camada visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/)<br/><br/>[Início, retomada e tarefas em segundo plano](/windows/uwp/launch-resume/)<br/><br/>[Acessibilidade do Windows](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [Interface do usuário da área de trabalho](/windows/desktop/windows-application-ui-development)<br/><br/>[Shell e o ambiente de área de trabalho](/windows/desktop/user-interface)<br/><br/>[controles do Windows](/windows/desktop/controls/window-controls)<br/><br/>[Controles da UWP em aplicativos da área de trabalho (Ilhas de XAML)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[Camada Visual do UWP em aplicativos da área de trabalho](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Mensagens e do Windows](/windows/desktop/winmsg/windowing)<br/><br/>[Menus e outros recursos](/windows/desktop/menurc/resources)<br/><br/>[Alto DPI](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Acessibilidade](/windows/desktop/accessibility)<br/><br/>  |  [Windows no WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Visão geral da navegação](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[XAML no WPF](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Controles](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[Programação de camada Visual](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[entrada](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Acessibilidade](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [Criar um formulário do Windows](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Controles](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[Caixas de diálogo](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Entrada do usuário](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Acessibilidade dos Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>Áudio, vídeo e imagens

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Áudio, vídeo e câmera](/windows/uwp/audio-video-camera/)<br/><br/>[Reprodução de mídia](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Camada visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/) |  [Áudio e vídeo](/windows/desktop/audio-and-video)<br/><br/>[Gráficos e jogos](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Elementos gráficos](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Elementos gráficos e desenho](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms?view=netframework-4.8)<br/><br/>[Classe SoundPlayer](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Recursos de aplicativo e acesso de dados

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Acesso a dados](/windows/uwp/data-access/)<br/><br/>[Vinculação de dados](/windows/uwp/data-binding/)<br/><br/>[Arquivos, pastas e bibliotecas](/windows/uwp/files/)<br/><br/>[Recursos do aplicativo](/windows/uwp/app-resources/) |  [Acesso a dados e armazenamento](/windows/desktop/data-access-and-storage)<br/><br/>[Sistemas de arquivos locais](/windows/desktop/fileio/file-systems)<br/><br/>[Visões gerais de recursos](/windows/desktop/menurc/resources-overviews)</li>  |  [Dados e modelagem](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Vinculação de dados](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Recursos em aplicativos .NET](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[Arquivos de recurso, conteúdo e dados de aplicativo](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Dados e modelagem](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Vinculação de dados](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Recursos em aplicativos .NET](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[Configurações do aplicativo](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Dispositivos, documentos e impressão

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Habilitar as funcionalidades do dispositivo](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Sensores](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Impressão e digitalização](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [API do sensor](/windows/desktop/sensorsapi/portal)<br/><br/>[Impressão](/desktop/printdocs/printdocs-printing)<br/><br/>[APIs de UPnP](/desktop/upnp/universal-plug-and-play-start-page) |  [Gerenciamento de sistema de impressão e imprimir](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [suporte à impressão](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>Energia, rede e sistema

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Obter informações de bateria](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Threading e programação assíncrona](/windows/uwp/threading-async/)<br/><br/>[Serviços de rede e Web](/windows/uwp/networking/) | [Serviços do sistema](/windows/desktop/system-services)<br/><br/>[Gerenciamento de memória](/windows/desktop/memory/memory-management)<br/><br/>[Gerenciamento de energia](/windows/desktop/power/power-management-portal)<br/><br/>[Processos e threads](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Sistema de rede e Internet](/windows/desktop/networking)<br/><br/>[Informações de sistema do Windows](/windows/desktop/sysinfo/windows-system-information) |  [Modelo de Threading](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Programação de rede no .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [Informações do sistema](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Gerenciamento de energia](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Programação de rede no .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Sistema de rede do Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>Empacotamento e implantação

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Empacotando aplicativos](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[Esquema de manifesto de pacote de aplicativo](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Empacotar aplicativos da área de trabalho do Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Instalação de aplicativos e serviços](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Empacotar aplicativos da área de trabalho do Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implantando o .NET Framework e aplicativos](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Implantando um aplicativo WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Empacotar aplicativos da área de trabalho do Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implantando o .NET Framework e aplicativos](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Implantação de ClickOnce para Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |

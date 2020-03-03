---
Description: Esta seção ajuda você a entender como determinados recursos essenciais do Windows são compatíveis com diferentes plataformas de aplicativo e como começar a usar esses recursos em seu código.
title: Recursos e tecnologias
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 6bae7fdef4e9cdf05dfa6f56160f1021033123e6
ms.sourcegitcommit: 7f1b64f62bc3a82ebcd3807c809363df46919195
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77705771"
---
# <a name="features-and-technologies-for-windows-apps"></a>Recursos e tecnologias para aplicativos do Windows

Independentemente do tipo de aplicativo que você está criando ou do dispositivo que você está pretendendo adotar, o Windows é compatível com muitos recursos que são blocos de construção essenciais para importantes cenários de aplicativos. Alguns desses recursos são expostos à UWP (Plataforma Universal do Windows), Win32 (API do Windows) e outras plataformas de aplicativos de diferentes maneiras. Os artigos a seguir ajudam você a entender como determinados recursos do Windows são compatíveis com diferentes plataformas de aplicativo e como começar a usar esses recursos em seu código.

Este artigo fornece uma lista personalizada de artigos para permitir que você leia mais sobre como é possível acessar importantes recursos e tecnologias do Windows nas plataformas de aplicativos UWP, Win32 (API do Windows), WPF e Windows Forms. Para obter informações completas sobre os recursos de desenvolvimento de cada plataforma, consulte os seguintes recursos:

* [Documentação da UWP](/windows/uwp/index)
* [Documentação da Win32 (API do Windows)](/windows/desktop/index)
* [Documentação da WPF](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Documentação do Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Recursos e tecnologias essenciais do Windows

As seções a seguir destacam vários recursos e tecnologias importantes do Windows que permitem fornecer experiências modernas e atraentes para seus clientes.

### <a name="windows-ink"></a>Windows Ink

![Caneta Surface](images/hero-small.png)  

A plataforma Windows Ink, juntamente com um dispositivo de caneta, oferece uma maneira natural de criar anotações manuscritas digitais, desenhos e anotações. A plataforma dá suporte à captura de dados de tinta por entrada de digitalizador, gerenciamento e geração de dados de tinta, renderização desses dados como traços de tinta no dispositivo de saída e conversão de tinta em texto por meio do reconhecimento de manuscrito.

Para obter mais informações sobre as diferentes maneiras de usar o Windows Ink nos aplicativos do Windows, consulte [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

### <a name="speech-interactions"></a>Interações de controle por voz

![tela de reconhecimento inicial para uma restrição baseada em um arquivo de gramática sgrs](images/speech-listening-initial.png)

![tela de reconhecimento final para uma restrição baseada em um arquivo de gramática sgrs](images/speech-listening-complete.png)

O Windows oferece muitas maneiras de integrar o reconhecimento de fala e a conversão de texto em fala (também conhecida como TTS ou sintetização de voz) diretamente à experiência do usuário do seu aplicativo. A fala pode ser um modo robusto e agradável para as pessoas interagirem com seu aplicativo, complementando ou até mesmo substituindo o teclado, o mouse, o toque e os gestos.

Para obter mais informações sobre as diferentes maneiras de usar interações de fala em aplicativos do Windows, confira [Fala, voz e conversa no Windows 10](speech.md).

### <a name="windows-ai"></a>IA do Windows

![IA do Windows](images/windows-ai.png)

Oferecemos várias soluções de IA diferentes que você pode usar para aprimorar seus aplicativos do Windows. Com o Windows Machine Learning, você pode integrar modelos treinados de aprendizado de máquina a seus aplicativos e executá-los localmente no dispositivo. As Windows Vision Skills permitem que você use bibliotecas predefinidas para realizar tarefas comuns de processamento de imagens ou crie suas próprias soluções personalizadas. O DirectML fornece APIs de baixo nível em estilo DirectX que lhe permitem aproveitar ao máximo o hardware.

Para obter mais informações sobre as diferentes maneiras de integrar a IA aos aplicativos do Windows, consulte [Windows IA](https://docs.microsoft.com/windows/ai/).

## <a name="features-and-technologies-by-platform"></a>Recursos e tecnologias por plataforma

As seções a seguir fornecem links úteis para saber mais sobre como integrar aos recursos e as tecnologias essenciais do Windows nossas principais plataformas de aplicativos: UWP, Win32 (API do Windows), WPF e Windows Forms.

### <a name="user-interface-and-accessibility"></a>Interface do usuário e acessibilidade

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Projetar](/windows/uwp/design/basics/)<br/><br/>[Layout](/windows/uwp/design/layout/)<br/><br/>[Controles](/windows/uwp/design/controls-and-patterns/)<br/><br/>[Entrada](/windows/uwp/design/input/)<br/><br/>[Blocos](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Camada visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/)<br/><br/>[Início, retomada e tarefas em segundo plano](/windows/uwp/launch-resume/)<br/><br/>[Acessibilidade do Windows](/windows/uwp/design/accessibility/accessibility)<br/><br/>[Interações de controle por voz](https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)<br/><br/> |  [Interface do usuário da Área de Trabalho](/windows/desktop/windows-application-ui-development)<br/><br/>[Ambiente e Shell da Área de Trabalho](/windows/desktop/user-interface)<br/><br/>[Controles do Windows](/windows/desktop/controls/window-controls)<br/><br/>[Controles da UWP em aplicativos da área de trabalho (ilhas de XAML)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[Camada visual da UWP em aplicativos da área de trabalho](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows e mensagens](/windows/desktop/winmsg/windowing)<br/><br/>[Menus e outros recursos](/windows/desktop/menurc/resources)<br/><br/>[DPI alto](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Acessibilidade](/windows/desktop/accessibility)<br/><br/>[Microsoft Speech Platform – SDK (Software Development Kit) (versão 11)](https://www.microsoft.com/download/details.aspx?id=27226)<br/><br/>[SDK do Microsoft Speech, versão 5.1](https://www.microsoft.com/download/details.aspx?id=10121)<br/><br/>  |  [Windows na WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Visão geral de navegação](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[XAML na WPF](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Controles](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[Programação da camada visual](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[Entrada](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Acessibilidade](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>[Guia de programação do System.Speech para .NET Framework](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/>  | [Criar um Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Controles](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[Caixas de diálogo](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Entrada do usuário](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Acessibilidade do Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/>[Guia de programação do System.Speech para .NET Framework](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/> |

### <a name="audio-video-and-graphics"></a>Áudio, vídeo e elementos gráficos

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Áudio, vídeo e câmera](/windows/uwp/audio-video-camera/)<br/><br/>[Reprodução de mídia](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Camada visual](/windows/uwp/composition/visual-layer)<br/><br/>[Plataforma XAML](/windows/uwp/xaml-platform/) |  [Áudio e vídeo](/windows/desktop/audio-and-video)<br/><br/>[Elementos gráficos e jogos](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[GDI do Windows](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Elementos gráficos](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Multimídia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Elementos gráficos e desenhos](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[Classe SoundPlayer](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Acesso a dados e recursos de aplicativo

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Acesso a dados](/windows/uwp/data-access/)<br/><br/>[Vinculação de dados](/windows/uwp/data-binding/)<br/><br/>[Arquivos, pastas e bibliotecas](/windows/uwp/files/)<br/><br/>[Recursos de aplicativo](/windows/uwp/app-resources/) |  [Acesso a dados e armazenamento](/windows/desktop/data-access-and-storage)<br/><br/>[Sistemas de arquivos locais](/windows/desktop/fileio/file-systems)<br/><br/>[Visões gerais de recursos](/windows/desktop/menurc/resources-overviews)</li>  |  [Dados e modelagem](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Vinculação de dados](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Recursos em aplicativos .NET](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[Arquivos de recurso, conteúdo e dados de aplicativo](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Dados e modelagem](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Vinculação de dados](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Recursos em aplicativos .NET](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[Configurações de aplicativo](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Dispositivos, documentos e impressão

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Habilitar as funcionalidades do dispositivo](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Sensores](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Impressão e digitalização](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [API de sensor](/windows/desktop/sensorsapi/portal)<br/><br/>[Impressão](/desktop/printdocs/printdocs-printing)<br/><br/>[APIs de UPnP](/desktop/upnp/universal-plug-and-play-start-page) |  [Gerenciamento de impressão e do sistema de impressão](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [Suporte a impressão](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>Sistema, rede e energia

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Enumerar dispositivos](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Obter informações de bateria](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Threading e programação assíncrona](/windows/uwp/threading-async/)<br/><br/>[Serviços de rede e Web](/windows/uwp/networking/) | [Serviços do sistema](/windows/desktop/system-services)<br/><br/>[Gerenciamento de memória](/windows/desktop/memory/memory-management)<br/><br/>[Gerenciamento de energia](/windows/desktop/power/power-management-portal)<br/><br/>[Processos e threads](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Rede e Internet](/windows/desktop/networking)<br/><br/>[Informações sobre o sistema Windows](/windows/desktop/sysinfo/windows-system-information) |  [Modelo de threading](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Programação de rede no .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [Informações do sistema](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Gerenciamento de energia](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Programação de rede no .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Rede no Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="debugging-and-performance"></a>Depuração e desempenho

|  UWP  |  Win32 (API do Windows) |  WPF e Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Depuração, teste e desempenho](/windows/uwp/debug-test-perf)<br/><br/>[Implantando e depurando aplicativos UWP](/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)<br/><br/>[Kit de Certificação de Aplicativos Windows](/windows/uwp/debug-test-perf/windows-app-certification-kit)<br/><br/>[Desempenho](/windows/uwp/debug-test-perf/performance-and-xaml-ui)| [Depuração e tratamento de erro](https://docs.microsoft.com/windows/win32/debugging-and-error-handling)<br/><br/>[Ferramentas de Depuração para Windows](https://docs.microsoft.com/windows-hardware/drivers/debugger/)<br/><br/>[ETW (Rastreamento de Eventos para Windows)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal)<br/><br/>[API TraceProcessing do .NET](/windows/apps/trace-processing/)<br/><br/>[TraceLogging](https://docs.microsoft.com/windows/win32/tracelogging/trace-logging-portal)<br/><br/>[Contadores de desempenho](https://docs.microsoft.com/windows/win32/perfctrs/performance-counters-portal) |  [Depuração, rastreamento e criação de perfil](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/)<br/><br/>[Rastreamento e instrumentação de aplicativos](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications)<br/><br/>[Diagnosticar erros com assistentes de depuração gerenciados](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants)<br/><br/>[Criação de perfil de runtime](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/runtime-profiling)<br/><br/>[Contadores de desempenho](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/performance-counters)<br/><br/>[Implantação do ClickOnce para o Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |

### <a name="packaging-and-deployment"></a>Empacotamento e implantação

|  UWP  |  Win32 (API do Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Empacotando aplicativos](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[Esquema de manifesto do pacote do aplicativo](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Pacote de aplicativos da área de trabalho do Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Instalação e manutenção de aplicativos](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Pacote de aplicativos da área de trabalho do Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implantando o .NET Framework e aplicativos](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Implantando um aplicativo WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Pacote de aplicativos da área de trabalho do Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Implantando o .NET Framework e aplicativos](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Implantação do ClickOnce para o Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |

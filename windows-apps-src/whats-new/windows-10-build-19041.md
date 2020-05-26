---
title: Novidades no Windows 10, build 19041
description: O Windows 10 build 18362 e as novas ferramentas para desenvolvedores fornecem as ferramentas, os recursos e as experiências do Windows 10.
keywords: novidades, novidade, Windows, Windows 10, atualização, atualizações, recursos, novos, mais recente, desenvolvedores, 19041, maio
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bb7630afd6cc69497494a2e86e6c5e3544acefec
ms.sourcegitcommit: f806d5f3b0c1e046c903d3388092c0e059d21858
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790983"
---
# <a name="whats-new-for-developers-in-windows-10-build-19041"></a>Novidades para desenvolvedores no Windows 10 build 19041

O Windows 10 build 19041 (também conhecido como SDK versão 2004), em combinação com o Visual Studio 2019 e ferramentas e recursos relacionados, fornece tudo o que você precisa para criar aplicativos notáveis do Windows. [Instale as ferramentas e o SDK](https://developer.microsoft.com/windows/downloads#_blank) no Windows 10 e você estará pronto para [criar um aplicativo universal do Windows](../get-started/create-uwp-apps.md) ou explorar como você pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

Esta é uma coleção de recursos novos e aprimorados e diretrizes de interesse para os desenvolvedores Windows neste lançamento. Para obter uma lista completa de namespaces novos adicionados ao SDK do Windows, confira as [Alterações na API do Windows 10 build 19041](windows-10-build-19041-api-diff.md). Para saber mais sobre os recursos em destaque do Windows 10, confira [Novidades no Windows 10](https://developer.microsoft.com/windows/windows-10-for-developers).

## <a name="windows-10-apps"></a>Aplicativos do Windows 10

Recurso | Descrição
:------ | :------
Reprodução de áudio Bluetooth | [Habilitar reprodução de áudio de dispositivos remotos conectados por Bluetooth](../audio-video-camera/enable-remote-audio-playback.md) mostra como usar o [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) para permitir que dispositivos remotos conectados por Bluetooth reproduzam áudio no computador local, o que possibilita cenários como configurar um PC para se comportar como um alto-falante Bluetooth e permitir que os usuários ouçam o áudio do telefone deles.
Portabilidade de aplicativo em C# | Documentamos o processo de portabilidade de um aplicativo em C# para C++/WinRT. O tópico [Portar a amostra de Clipboard de C# para C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp) é contextual e baseia-se em uma experiência específica de portabilidade real. Seu tópico complementar, [Mover do C# para C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp), apresenta uma visão mais enciclopédica dos detalhes técnicos e das etapas envolvidas na portabilidade. 
C++/WinRT | Leia sobre as atualizações do C++/WinRT relacionadas às melhorias de desempenho em tempo de compilação e tempo de execução (obtidas em parceria com a equipe do compilador Visual C++), no [Pacote cumulativo de melhorias/adições recentes](/windows/uwp/cpp-and-winrt-apis/news#rollup-of-recent-improvementsadditions-as-of-march-2020). </br> Para C++/WinRT, adicionamos mais informações a estes tópicos: [portabilidade de C++/CX ](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#boxing-and-unboxing), [portabilidade de C#](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp#boxing-and-unboxing), [Exemplo simples da Biblioteca de Interface do Usuário do Windows em C++/WinRT](/windows/uwp/cpp-and-winrt-apis/simple-winui-example), [Simultaneidade](/windows/uwp/cpp-and-winrt-apis/concurrency), [get_unknown()](/uwp/cpp-ref-for-winrt/get-unknown) e [Controles personalizados XAML (modelos) com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl).
DirectX | Atualizamos vários tópicos "Novidades" relacionados ao DirectX para diversas versões anteriores do Windows, da Atualização para Criadores ao Windows 10, versão 1903. [Novidades no DirectWrite](/windows/win32/directwrite/what-s-new-in-directwrite-for-windows-8-consumer-preview), [Melhorias no DXGI 1.6](/windows/win32/direct3ddxgi/dxgi-1-6-improvements) e [Novidades no Direct3D 12](/windows/win32/direct3d12/new-releases).
DirectXMath | Publicamos 21 novos tópicos do DirectXMath, abordando duas estruturas de matriz e suas funções de membro e funções livres. [Estrutura do XMFLOAT3X4](/windows/win32/api/directxmath/ns-directxmath-xmfloat3x4) é um exemplo.
Direct3D | [Usar o DirectX com telas de alto alcance dinâmico e cores avançadas](/windows/win32/direct3darticles/high-dynamic-range) fornece uma lista de práticas recomendadas para aplicativos de alto alcance dinâmico do Windows. </br> Uma nova interface [ID3D11On12Device2](/windows/win32/api/d3d11on12/nn-d3d11on12-id3d11on12device2) e seus métodos permitem pegar os recursos criados por meio das APIs do Direct3D 11 e usá-los no Direct3D 12.
Direct3D 12 | [O Nível de recursos do Core 1.0 do Direct3D 12](/windows/win32/direct3d12/core-feature-levels) foi adicionado, para uso em dispositivos *apenas de computação*. </br> Novos tópicos foram adicionados para a [interface ID3D12Debug3](/windows/win32/api/d3d12sdklayers/nn-d3d12sdklayers-id3d12debug3).
Direct ML | Foram adicionados 18 operadores ao DirectML, a API acelerada por hardware de baixo nível na qual o WinML é criado. Um exemplo é a [estrutura DML_ACTIVATION_SHRINK_OPERATOR_DESC](/windows/win32/api/directml/ns-directml-dml_activation_shrink_operator_desc).
Relatório de erros | A função RoFailFastWithErrorContextInternal2 foi adicionada ao Win32, o que gera uma exceção que pode conter um contexto de erro adicional.
Machine Learning | O Windows Machine Learning [agora oferece suporte ao ONNX versão 1.4 e opset 9](/windows/ai/windows-ml/release-notes). </br>  A API [CloseModelOnSessionCreation](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions.closemodelonsessioncreation?view=winrt-19041) permite economizar memória, fechando um modelo de aprendizado automaticamente quando ele não é mais necessário.
Wi-Fi | Várias novas funções e estruturas Wi-Fi nativas foram adicionadas, como a função [WlanDeviceServiceCommand](/windows/win32/api/wlanapi/nf-wlanapi-wlandeviceservicecommand).
Hotspot Wi-Fi 2 | [Provisionar um perfil Wi-Fi por meio de um site](/windows/win32/nativewifi/prov-wifi-profile-via-website) descreve as novas funcionalidades do Hotspot Wi-Fi 2.
Interoperabilidade do Windows Holographic | O cabeçalho [`windows.graphics.holographic.interop.h`](/windows/win32/api/windows.graphics.holographic.interop) foi adicionado com 17 APIs do Win32. As APIs são para interoperação entre o Win32 e o Windows Runtime. Enquanto as APIs foram adicionadas no Windows 10 build 18362, o cabeçalho é novo no build 19041.
Windows Sockets | Foram feitos aprimoramentos no conteúdo do Windows Sockets 2 SPI. Um exemplo dos muitos tópicos que aprimoramos e ampliamos é o tópico [Função de retorno de chamada LPWSPEVENTSELECT](/windows/win32/api/ws2spi/nc-ws2spi-lpwspeventselect).
Ilhas XAML – noções básicas | Hospede controles XAML UWP nos aplicativos Windows da área de trabalho com ilhas XAML. Saiba como [hospedar um controle UWP padrão em um aplicativo WPF](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands) e [hospedar um controle UWP padrão em um aplicativo C++ Win32](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands-cpp).
Ilhas XAML – controles personalizados | Os pacotes NuGet [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) e [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) facilitam a hospedagem de controles XAML UWP personalizados em aplicativos .NET e C++ Win32. </br> Para obter instruções passo a passo, confira [Hospedar um controle UWP personalizado em um aplicativo WPF](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands) e [Hospedar um controle UWP personalizado em um aplicativo C++ Win32](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands-cpp). </br> Por fim, para obter diretrizes sobre cenários mais complicados do C++ Win32, confira [Cenários avançados para ilhas XAML](/windows/apps/desktop/modernize/advanced-scenarios-xaml-islands-cpp).

## <a name="build-with-windows"></a>Criar com o Windows

Recurso | Descrição
:------ | :------
Ambiente de desenvolvimento do Windows | Os documentos do [ambiente de desenvolvimento do Windows](/windows/dev-environment/) fornecem recursos para o uso do Windows em diversas plataformas, para atingir quaisquer objetivos de desenvolvimento que você possa ter.
Python no Windows | A seção [Python no Windows](/windows/python/) fornece informações para desenvolvedores iniciantes na linguagem Python, bem como desenvolvedores que desejam otimizar seu desenvolvimento em Python com outras ferramentas disponíveis no Windows. Saiba como configurar seu ambiente do Python para [desenvolvimento na Web](/windows/python/web-frameworks) e [interação com o banco de dados](/windows/python/databases).
NodeJS no Windows | A [configuração recomendada para o seu ambiente de desenvolvimento Node.js.](/windows/nodejs/setup-on-wsl2) fornece diretrizes detalhadas para desenvolvedores avançados que estão implementando em servidores Linux. Também estão disponíveis instruções de configuração para [estruturas da Web populares do Node.js.](/windows/nodejs/web-frameworks), [interação com o banco de dados](/windows/nodejs/databases) e [contêineres do Docker](/windows/nodejs/containers).
Mac para Windows | Nosso [guia para alterar seu ambiente de desenvolvimento](/windows/dev-environment/mac-to-windows) é voltado para usuários que fazem a transição da plataforma de desenvolvimento do Mac para o Windows e fornece mapeamentos para atalhos comparáveis e utilitários de desenvolvimento.
Terminal do Windows | É um [aplicativo de terminal moderno](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab) para usuários de ferramentas de linha de comando e shells como o Prompt de Comando, o PowerShell e o WSL (Subsistema do Windows para Linux). Seus principais recursos incluem suporte a várias guias, painéis, caracteres Unicode e UTF-8, um mecanismo de renderização de texto acelerado por GPU e a capacidade de criar seus próprios temas e personalizar texto, cores, telas de fundo e associações de teclas de atalho.
WSL 2 | [Uma nova versão do WSL (Subsistema do Windows para Linux)](/windows/wsl/wsl2-about) já está disponível. O WSL 2 apresenta arquitetura reconfigurada para executar um kernel Linux real no Windows, aumentando o desempenho do sistema de arquivos e adicionando compatibilidade total com a chamada do sistema. Essa nova arquitetura altera o modo como os binários do Linux interagem com o Windows e o hardware do computador, mas ainda fornece a mesma experiência do usuário da versão anterior do WSL. Cada distribuição Linux individual pode ser executada como uma distribuição WSL1 ou WSL2, pode ser executada lado a lado e pode ser alterada a qualquer momento. </br> [Instale o WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-install) para começar. </br> Explore mais informações sobre [alterações na experiência do usuário entre o WSL 1 e o WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-ux-changes). </br> Confira as [Perguntas frequentes sobre o WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-faq).

## <a name="msix-packaging-and-deployment"></a>MSIX, empacotamento, e implantação

Recurso | Descrição
:------ | :------
MSIX | Atualizações significativas no [formato de empacotamento MSIX](/windows/msix/overview) foram feitas desde a última versão do Windows 10 SDK. 
Empacotamento com serviços | O MSIX e a Ferramenta de Empacotamento MSIX [agora oferecem suporte a pacotes de aplicativos que contêm serviços](/windows/msix/packaging-tool/convert-an-installer-with-services).
Scripts em pacotes do MSIX | Você pode [usar o PSF (Package Support Framework) para executar scripts em um pacote de aplicativos MSIX](/windows/msix/psf/run-scripts-with-package-support-framework), permitindo que os profissionais de TI personalizem um aplicativo dinamicamente para o ambiente do usuário após o empacotamento usando o MSIX.
Integridade de pacote imposta | Agora você pode impor a integridade do pacote no conteúdo dos pacotes MSIX usando o elemento [uap10:PackageIntegrity](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity) no manifesto do pacote. Você também pode impor a integridade do pacote ao criar pacotes MSIX por meio da ferramenta de empacotamento MSIX.
Pacotes esparsos | É possível [conceder um identificador de pacote a aplicativos da área de trabalho que não estão em um pacote MSIX](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) por meio da criação e registro de um *pacote esparso* com o aplicativo. Esse recurso permite que os aplicativos da área de trabalho que ainda não são capazes de adotar o empacotamento MSIX para implantação usem recursos de extensibilidade do Windows 10 que exigem o identificador de pacote.
Aplicativos hospedados | Agora você pode [criar aplicativos hospedados](/windows/uwp/launch-resume/hosted-apps). Os aplicativos hospedados compartilham o mesmo executável e a definição de um aplicativo host pai, mas têm a aparência e o comportamento de um aplicativo separado no sistema. Aplicativos hospedados são úteis para cenários em que você deseja que um componente (como um arquivo executável ou um arquivo de script) se comporte como um aplicativo autônomo do Windows 10, mas o componente requer um processo de host para ser executado. Um aplicativo hospedado pode ter seu próprio bloco inicial, identidade e profunda integração com os recursos do Windows 10, como tarefas em segundo plano, notificações, blocos e destinos de compartilhamento.

## <a name="windows-ui-library-winui"></a>Biblioteca de Interface do Usuário do Windows (WinUI)

Recurso | Descrição
:------ | :------
WinUI 2.4 | O [WinUI 2.4](/uwp/toolkits/winui/release-notes/winui-2.4) é a versão pública mais recente da Biblioteca de Interface do Usuário do Windows. Todas as versões do WinUI fornecem uma ampla variedade de controles oficiais da interface do usuário para seus aplicativos do Windows e são fornecidas como um pacote NuGet independente do SDK do Windows, para que funcionem nas versões anteriores do Windows 10. [Siga as instruções](/uwp/toolkits/winui) para instalar o WinUI.
RadialGradientBrush | Novo no WinUI 2.4, um [RadialGradientBrush](../design/style/brushes.md#radial-gradient-brushes) é desenhado em uma elipse definida pelas propriedades Center, RadiusX e RadiusY. As cores do gradiente começam no centro da elipse e terminam no raio.
ProgressRing | Novo no WinUI 2.4, o [ controle ProgressRing ](../design/controls-and-patterns/progress-controls.md) é usado para interações modais em que o usuário é bloqueado até que o ProgressRing desapareça. Use esse controle se uma operação exige que a maior parte da interação com o aplicativo seja suspensa até que a operação seja concluída.
TabView | As atualizações no [controle TabView](../design/controls-and-patterns/tab-view.md) fornecem mais controle sobre como renderizar guias. Você pode definir a largura das guias não selecionadas e mostrar apenas um ícone para economizar espaço na tela, além de ocultar o botão Fechar nas guias não selecionadas até que o usuário passe o mouse sobre a guia.
Controles TextBox | Quando o tema escuro está habilitado, a cor do plano de fundo dos controles da família TextBox agora permanece escura por padrão na inserção de texto. Os controles afetados são [TextBox](/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock), [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox), [Editable ComboBox](/uwp/api/windows.ui.xaml.controls.combobox) e [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox).
NavigationView | O controle [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) agora oferece suporte à navegação hierárquica e inclui os modos de exibição Left, Top e LeftCompact. Um NavigationView hierárquico é útil para exibir categorias de páginas, para identificar páginas com páginas secundárias relacionadas ou para usar em aplicativos que têm páginas no estilo de hub que vinculam a muitas outras páginas.
Galeria da interface do usuário do Windows | Exemplos de cada recurso do WinUI estão disponíveis na XAML Controls Gallery. Baixe na [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) ou [confira o código-fonte no Github](https://github.com/Microsoft/Xaml-Controls-Gallery).
Versões anteriores | Desde a versão principal anterior do SDK do Windows 10, o [WinUI 2.3](/uwp/toolkits/winui/release-notes/winui-2.3) e o [WinUI 2.2](/uwp/toolkits/winui/release-notes/winui-2.2) também foram lançados, fornecendo novos recursos de interface do usuário para desenvolvedores do Windows.

## <a name="samples"></a>Amostras

Os aplicativos de amostra a seguir foram atualizados para serem direcionados ao Windows 10 build 19041.

* [Sessões remotas (jogo de perguntas)](https://github.com/microsoft/Windows-appsample-remote-system-sessions)
* [Banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
* [Leitor RSS](https://github.com/Microsoft/Windows-appsample-rssreader)
* [Labirinto](https://github.com/Microsoft/Windows-appsample-marble-maze)
* [Editor de Fotos](https://github.com/Microsoft/Windows-appsample-photo-editor)
* [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
* [Livro de Colorir](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Controlador de luz da matiz](https://github.com/Microsoft/Windows-appsample-huelightcontroller)
* [PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab)
* [FamilyNotes](https://github.com/Microsoft/Windows-appsample-familynotes)

## <a name="videos"></a>Vídeos

### <a name="windows-terminal-the-secret-to-command-line-happiness"></a>Terminal do Windows: o segredo para a felicidade na linha de comando!

Saiba mais sobre como personalizar o Terminal do Windows para seu fluxo de trabalho e confira demonstrações de seus recursos em ação. [Assista ao vídeo](https://www.youtube.com/watch?v=2dsnwlnNBzs) e [confira os documentos](https://github.com/microsoft/terminal#terminal--console-overview) para obter mais informações.

### <a name="wsl2-code-faster-on-the-windows-subsystem-for-linux"></a>WSL2: codificar mais rapidamente no Subsistema do Windows para Linux

Saiba tudo sobre o WSL2, a nova versão do subsistema do Windows para Linux, e quais alterações foram feitas para melhorar o desempenho. [Assista ao vídeo](https://www.youtube.com/watch?v=MrZolfGm8Zk) e [confira os documentos](/windows/wsl/wsl2-about) para obter mais informações.

### <a name="msix-package-desktop-apps-for-windows-10-replace-outdated-installers"></a>MSIX: empacotar aplicativos da área de trabalho para o Windows 10. Substitua instaladores desatualizados.

Saiba mais sobre o MSIX, o formato do pacote para instalar aplicativos do Windows, incluindo como empacotar seu código existente com o Visual Studio e como implantar e distribuir seu aplicativo. [Assista ao vídeo](https://www.youtube.com/watch?v=yhOnClQrvBk) e [confira os documentos](/windows/msix/) para obter mais informações.

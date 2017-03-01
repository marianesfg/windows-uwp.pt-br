---
author: awkoren
Description: "Conheça a ponte da área de trabalho para UWP e converta o aplicativo da área de trabalho do Windows (como Win32, WPF e Windows Forms) em um aplicativo da Plataforma Universal do Windows (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: "Traga o aplicativo da área de trabalho para a Plataforma Universal do Windows (UWP) usando a Ponte de Desktop"
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: dd9f45b0ddcc201053ed8e35908da66443e47d72
ms.lasthandoff: 02/08/2017

---

# <a name="bring-your-desktop-app-to-the-universal-windows-platform-uwp-with-the-desktop-bridge"></a>Traga o aplicativo da área de trabalho para a Plataforma Universal do Windows (UWP) usando a Ponte de Desktop

Conheça a ponte da área de trabalho para UWP e converta o aplicativo da área de trabalho do Windows em um aplicativo da Plataforma Universal do Windows (UWP).

A ponte da área de trabalho é um conjunto de tecnologias que permite converter o aplicativo da área de trabalho do Windows (por exemplo, Win32, Windows Forms e WPF) ou o jogo em um aplicativo UWP ou jogo. Após a conversão, seu aplicativo de área de trabalho do Windows é empacotado, reparado e implantado na forma de um pacote de aplicativo UWP (.appx ou .appxbundle) na área de trabalho do Windows 10.

Há duas partes na tecnologia que permite que os aplicativos da área de trabalho sejam convertidos em pacotes UWP. A primeira é o processo de conversão, que utiliza os binários existentes e os reempacota como um pacote UWP. O código ainda é o mesmo, apenas é empacotado de forma diferente. A segunda parte compreende tecnologias de tempo de execução na atualização de aniversário do Windows que permitem que um pacote UWP tenha arquivos executáveis que são executados com confiança total, em vez de em um contêiner de aplicativo. Essa tecnologia também dá um identificador de pacote a um aplicativo convertido, que é necessário para usar algumas APIs de UWP.

## <a name="benefits"></a>Benefícios

Aqui estão alguns dos benefícios da conversão do aplicativo da área de trabalho do Windows: 

**Implantação simplificada**. Aplicativos e jogos que usam a ponte têm uma experiência excelente de implantação que garante que os usuários possam instalar e atualizar com confiança. Se um usuário optar por desinstalar o aplicativo, ele será removido por completo sem deixar rastreamento. Isso reduz o tempo para criar experiências de instalação e manter os usuários atualizados.

**Atualizações automáticas e licenciamento**. Seu aplicativo pode participar dos recursos internos de atualização automática e de licenciamento da Windows Store. A atualização automática é um mecanismo altamente confiável e eficiente, pois somente as partes alteradas dos arquivos são baixadas.

**Maior alcance e monetização simplificada**. Se optar por distribuir por meio da Windows Store, você poderá alcançar milhões de usuários do Windows 10, que podem adquirir aplicativos, jogos e compras realizadas em aplicativo com opções de pagamento locais.

**Adicione recursos UWP**.  Em seu próprio ritmo, você pode adicionar recursos UWP ao pacote do seu aplicativo, como uma interface de usuário XAML, atualizações de bloco dinâmico, tarefas de UWP em segundo plano, serviços de aplicativo, e muito mais.

**Casos de uso ampliados em dispositivos**. Usando a ponte, você pode migrar gradualmente o código para a Plataforma Universal do Windows a fim de alcançar todos os dispositivos Windows 10, inclusive telefones, Xbox One e HoloLens.

## <a name="prepare"></a>Preparar

A ponte da área de trabalho para UWP foi projetada para facilidade de uso, e você talvez não precise fazer muito para preparar o aplicativo para o processo de conversão. No entanto, existem algumas restrições e situações específicas a serem consideradas antes da conversão. Consulte o artigo [Preparar o aplicativo para a ponte da área de trabalho para UWP](desktop-to-uwp-prepare.md) e resolva todos os problemas que se apliquem ao aplicativo antes de continuar.

## <a name="convert"></a>Converter

Você tem algumas opções diferentes para converter seu aplicativo.

**Desktop App Converter (DAC)**. O DAC é uma ferramenta que converte automaticamente e assina seu aplicativo para você. Usar o DAC é conveniente e automático, e é útil se seu aplicativo faz várias modificações no sistema ou se houver dúvidas sobre o instalador faz. Para começar, consulte o artigo sobre o [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md). 

**Conversão manual**. Se seu aplicativo é instalado usando o xcopy ou você estiver familiarizado com as alterações que o instalador faz no sistema, a conversão manual pode ser uma opção mais simples. Isso envolve a criação de um arquivo de manifesto, a execução da ferramenta MakeAppx.exe e a assinatura do pacote do aplicativo. Para saber mais detalhes sobre como converter manualmente, consulte [Converter manualmente o aplicativo em UWP usando a ponte da área de trabalho](desktop-to-uwp-manual-conversion.md). 

**Instalador de terceiros**. Agora vários produtos e instaladores populares de terceiros dão suporte para a ponte da área de trabalho e podem gerar instaladores MSI ou pacotes de aplicativos convertidos com apenas alguns cliques. Algumas opções incluem: 

* [InstallShield by Flexera](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer)
* [WiX by FireGiant](https://www.firegiant.com/r/appx) 
* [Advanced Installer by Caphyon](http://www.advancedinstaller.com/uwp-app-package)
* [RAD Studio by Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge) 
* [InstallAware](https://www.installaware.com/appx.htm)

Para obter mais informações, visite o site correspondente de cada instalador. 

## <a name="enhance"></a>Aprimorar 

É possível destacar o aplicativo da área de trabalho convertido com uma ampla variedade de APIs UWP para adicionar recursos como blocos dinâmicos, notificações por push e muito mais. Para obter exemplos de códigos completos, consulte os repositórios [Exemplos de ponte de aplicativo da área de trabalho para UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) e [Exemplos de aplicativo da Plataforma Universal do Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples) em GitHub. Para ver uma lista completa de APIs compatíveis, consulte [APIs UWP compatíveis para aplicativos convertidos com a ponte da área de trabalho](desktop-to-uwp-supported-api.md). 

Além de chamar APIs UWP, é possível estender o aplicativo com recursos acessíveis somente a aplicativos convertidos. Entre eles estão cenários como iniciar um processo quando o usuário faz logon e integração com o Explorador de Arquivos e projetados para suavizar a transição entre o aplicativo da área de trabalho original e um pacote do aplicativo UWP completo. Para saber mais detalhes, consulte [Extensões de aplicativo da ponte da área de trabalho](desktop-to-uwp-extensions.md). 

## <a name="migrate"></a>Migrar

Usando a ponte, você pode migrar gradualmente o código anterior para a UWP ao mesmo tempo em que mantém a capacidade de executar e publicar o aplicativo na área de trabalho do Windows. Depois que tiver migrado totalmente para a UWP (e o aplicativo não contiver mais nenhum componente Win32/WPF), você poderá atingir todos os dispositivos Windows, inclusive telefones, Xbox One e HoloLens.

## <a name="debug"></a>Depurar

Você pode depurar o aplicativo usando o Visual Studio. Consulte [Depurar aplicativos convertidos usando a ponte da área de trabalho](desktop-to-uwp-debug.md) para obter ajuda detalhada. 

Se você tiver interesse nos detalhes de como a ponte da área de trabalho funciona nos bastidores, confira [Nos bastidores da ponte da área de trabalho](desktop-to-uwp-behind-the-scenes.md). 

## <a name="distribute"></a>Distribuir

Você pode distribuir o aplicativo usando a Windows Store ou por meio de sideload. Para saber mais detalhes, consulte [Distribuir aplicativos convertidos usando a ponte da área de trabalho](desktop-to-uwp-distribute.md). Lembre-se de que você precisará assinar o aplicativo antes de implantá-lo para usuários. Consulte [Assinar um aplicativo convertido usando a ponte da área de trabalho](desktop-to-uwp-signing.md) para obter instruções detalhadas. 

## <a name="support-and-feedback"></a>Suporte e comentários

Se você estiver com problemas de conversão do seu aplicativo, você pode visitar os [fóruns](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) para obter ajuda. 

Para fazer comentários ou dar sugestões de recursos, envie ou vote a favor de itens em [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial). 

## <a name="in-this-section"></a>Nesta seção

| Tópico | Descrição |
|-------|-------------|
| [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | Mostra como executar o Desktop App Converter. |
| [Converter manualmente o aplicativo em UWP usando a ponte da área de trabalho](desktop-to-uwp-manual-conversion.md) | Saiba como criar um pacote de aplicativo e manifestá-lo manualmente. |
| [Extensões de aplicativo da ponte da área de trabalho](desktop-to-uwp-extensions.md) | Aprimore o aplicativo convertido com extensões para habilitar recursos como tarefas de inicialização e integração com o Explorador de Arquivos. |
| [APIs UWP compatíveis para aplicativos convertidos com a ponte da área de trabalho](desktop-to-uwp-supported-api.md) | Veja quais APIs de UWP estão disponíveis para serem usadas pelo seu aplicativo da área de trabalho convertido. |
| [Guia de empacotamento da Ponte de Desktop para aplicativos da área de trabalho .NET com o Visual Studio](desktop-to-uwp-packaging-dot-net.md) | Configure a solução do Visual Studio para que poder editar, depurar e empacotar seu aplicativo .NET. | 
| [Depurar aplicativos convertidos usando a Ponte de Desktop](desktop-to-uwp-debug.md) | Explica as opções para depurar o aplicativo convertido. | 
| [Assinar um aplicativo convertido usando a ponte da área de trabalho](desktop-to-uwp-signing.md) | Saiba como assinar o pacote do aplicativo convertido usando um certificado. |
| [Distribuir aplicativos convertidos usando a ponte da área de trabalho](desktop-to-uwp-distribute.md) | Veja como é possível distribuir o aplicativo convertido para usuários.  |
| [Nos bastidores da ponte da área de trabalho](desktop-to-uwp-behind-the-scenes.md) | Aprofunde-se em como a ponte da área de trabalho para UWP funciona nos bastidores. | 
| [Problemas conhecidos com a ponte da área de trabalho](desktop-to-uwp-known-issues.md) | Listas problemas conhecidos com a ponte da área de trabalho para UWP. | 
| [Exemplos de código de ponte de aplicativo de área de trabalho para UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Exemplos de código em GitHub demonstrando recursos dos aplicativos convertidos. |

---
description: If you’re a developer with a Windows Phone Silverlight app, then you can make great use of your skill set and your source code in the move to Windows 10.
title: Move from Windows Phone Silverlight to UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d37e40d58d8825d4361efbd10108c1169b8df87f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260084"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>Move from Windows Phone Silverlight to UWP


If you’re a developer with a Windows Phone Silverlight app, then you can make great use of your skill set and your source code in the move to Windows 10. With Windows 10, you can create a Universal Windows Platform (UWP) app, which is a single app package that your customers can install onto every kind of device. For more background on Windows 10, UWP apps, and the concepts of adaptive code and adaptive UI that we'll mention in this porting guide, see the [Guide to Universal Windows Platform (UWP) apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

When you port your Windows Phone Silverlight app to a Windows 10 app, you'll be able to catch up on the mobile features that were [introduced in Windows Phone 8.1](https://docs.microsoft.com/previous-versions/windows/apps/ff402535(v=vs.105)), and go far beyond them to use the Universal Windows Platform (UWP) whose app model and UI framework are universal across all Windows 10 devices. Isso possibilita o suporte a computadores, tablets, telefones e vários outros tipos de dispositivos, de uma base de código e com um pacote do aplicativo. E isso multiplicará o público-alvo potencial do seu aplicativo e criará novas possibilidades com dados compartilhados, produtos consumíveis comprados etc. For more info on new features, see [What's new for developers in Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest).

If you choose to, the Windows Phone Silverlight version of your app and the Windows 10 version of it can both be available to customers at the same time.

**Note**  This guide is designed to help you port your Windows Phone Silverlight app to Windows 10 manually. Além de usar as informações neste guia para fazer a portabilidade do aplicativo, você pode testar a visualização de desenvolvedor do **Mobilize.NET's Silverlight Bridge** para ajudar a automatizar o processo de portabilidade. This tool analyzes your app's source code and converts references to Windows Phone Silverlight controls and APIs to their UWP counterparts. Como ainda está em visualização de desenvolvedor, essa ferramenta ainda não manipula todos os cenários de conversão. No entanto, a maioria dos desenvolvedores deve ser capaz de economizar tempo e esforço começando por essa ferramenta. Para testar a visualização de desenvolvedor, visite o [site do Mobilize.NET](https://www.mobilize.net/uwp-bridge).

## <a name="xaml-and-net-or-html"></a>XAML e .NET ou HTML?

Windows Phone Silverlight has a XAML UI framework based on Silverlight 4.0, and you program against a version of the .NET Framework and a small subset of UWP APIs. Since you used Extensible Application Markup Language (XAML) in your Windows Phone Silverlight app, it's likely that XAML will be your choice for your Windows 10 version because most of your knowledge and experience will transfer, as will much of your source code and the software patterns you use. Até mesmo sua marcação da interface do usuário e o design podem ser prontamente portados. Você achará as APIs gerenciadas, a marcação XAML, a estrutura da IU e as ferramentas muito familiares, e poderá usar C++, C#, ou Visual Basic juntamente com XAML em um aplicativo UWP. Você pode se surpreender com a relativa facilidade do processo, mesmo se houver um desafio ou dois ao longo do caminho.

Veja [Mapa de aplicativos UWP (Plataforma Universal do Windows) usando C# ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10)).

**Note**  Windows 10 supports much more of the .NET Framework than a Windows Phone Store app does. For example, Windows 10 has several System.ServiceModel.\* namespaces as well as System.Net, System.Net.NetworkInformation, and System.Net.Sockets. So, now is a great time to port your Windows Phone Silverlight and have your .NET code just compile and work on the new platform. Consulte [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md).
Another great reason to recompile your existing .NET source code into a Windows 10 app is that you will benefit from .NET Native, which an ahead-of-time compilation technology that converts MSIL into natively-runnable machine code. Aplicativos .NET Native iniciam com mais rapidez, usam menos memória e bateria do que seus equivalentes MSIL.

Este guia de portabilidade se concentrará em XAML, mas, como alternativa, você poderá compilar um aplicativo funcionalmente equivalente (chamando muitas das mesmas APIs da UWP) usando JavaScript, CSS (Folhas de Estilos em Cascata) e HTML5 em conjunto com a Biblioteca do Windows para JavaScript. Embora as estruturas da IU do Tempo de Execução do Windows de XAML e HTML sejam diferentes umas das outras, qualquer uma delas que você escolher funcionará universalmente em toda a gama de dispositivos Windows.

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>Direcionando a família de dispositivos móveis ou universais

Uma opção que você tem é portar seu aplicativo para um aplicativo direcionado à família de dispositivos universais. Nesse caso, o aplicativo pode ser instalado na mais ampla variedade de dispositivos. Caso o aplicativo chame APIs implementadas somente na família de dispositivos móveis, você pode proteger essas chamadas usando um código adaptável. Também é possível optar por portar o aplicativo para um aplicativo que segmenta a família de dispositivos móveis e, nesse caso, não precisa escrever código adaptável.

## <a name="adapting-your-app-to-multiple-form-factors"></a>Adaptando seu aplicativo a vários fatores forma

A opção que você escolher na seção anterior determinará a gama de dispositivos em que seu aplicativo ou aplicativos serão executados, e pode ser uma ampla variedade de dispositivos. Até mesmo limitar seu aplicativo à família de dispositivos móveis ainda exige uma ampla variedade de tamanhos de tela para as quais dar suporte. Sendo assim, uma vez que seu aplicativo será executado em fatores forma aos quais ele não dava suporte anteriormente, teste sua interface do usuário nesses fatores forma e faça qualquer alteração necessária para que sua interface do usuário se adapte adequadamente em cada um. Você pode considerar essa uma tarefa pós-portabilidade ou uma meta além da portabilidade, e há um exemplo prático disso no estudo de caso [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md).

## <a name="approaching-porting-layer-by-layer"></a>Abordagem de portabilidade camada por camada

-   **Modo de Exibição**. O modo de exibição (juntamente com o modelo de exibição) compõe a interface do usuário do seu aplicativo. Idealmente, o modo de exibição consiste em marcação associada às propriedades observáveis de um modelo de exibição. Outro padrão (comum e conveniente, mas somente a curto prazo) destina-se ao código imperativo em um arquivo code-behind para manipular elementos de interface do usuário diretamente. Em ambos os casos, grande parte da marcação da interface do usuário e do design (e até mesmo do código imperativo que manipula os elementos da interface do usuário) terá uma portabilidade simples.
-   **Modelos de exibição e modelos de dados**. Mesmo se você não adotar formalmente padrões de separação de preocupações (como MVVM), inevitavelmente haverá presente em seu aplicativo código que execute a função de modelo de exibição e de modelo de dados. O código do modelo de exibição usa tipos nos namespaces da estrutura da IU. O código dos modelos de exibição e de dados também usa APIs de sistema operacional não visual e do .NET (incluindo APIs para acesso a dados). E a maioria delas está [disponível para um aplicativo UWP](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), portanto, você pode esperar fazer a portabilidade de grande parte desse código sem alteração. Lembre-se: um modelo de exibição é um modelo, ou uma *abstração*, de um modo de exibição. Um modelo de exibição oferece o estado e o comportamento da interface do usuário, enquanto o modo de exibição propriamente dito oferece os elementos visuais. Por esse motivo, qualquer interface do usuário que você adaptar aos diferentes fatores forma em que a UWP permite execução provavelmente precisará de alterações correspondentes no modelo de exibição. Para redes e chamadas de serviços de nuvem, você geralmente tem a opção de usar APIs UWP ou .NET. Para saber os fatores envolvidos na tomada dessa decisão, consulte [Serviços de nuvem, redes e bancos de dados](wpsl-to-uwp-business-and-data.md).
-   **Serviços de nuvem**. É provável que alguma parte do aplicativo (talvez uma grande parte dele) seja executada na nuvem na forma de serviços. A parte do aplicativo em execução no dispositivo cliente se conecta a elas. Essa é a parte de um aplicativo distribuído que mais provavelmente permanecerá inalterada durante a portabilidade da parte cliente. Se você ainda não tiver, uma boa opção de serviços de nuvem para seu aplicativo UWP são os [Serviços Móveis do Microsoft Azure](https://azure.microsoft.com/services/mobile-services/) que oferecem poderosos componentes de back-end que aplicativos Universais do Windows podem chamar para serviços, desde notificações simples para atualizações de blocos dinâmicos até o tipo de escalabilidade pesada que um farm de servidores pode oferecer.

Antes ou durante a portabilidade, considere se o seu aplicativo pode ser melhorado por meio de refatoração, de forma que o código com finalidade semelhante seja agrupado em camadas e não fique espalhado arbitrariamente. A fatoração de seu aplicativo UWP em camadas como as descritas acima facilita a correção do seu aplicativo, a aplicação de testes nele e, subsequentemente, a leitura e a manutenção dele. Você pode tornar a funcionalidade mais reutilizável - e evitar alguns dos problemas de diferenças de API da interface do usuário entre as plataformas - seguindo o padrão MVVM (Model-View-ViewModel) ([MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)). Esse padrão mantém partes de dados, comercial e da interface do usuário de seu aplicativo separadas umas das outras. Mesmo na interface do usuário, ele mantém o estado e o comportamento separados, e testáveis separadamente, dos elementos visuais. Com o MVVM, você pode escrever seus dados e sua lógica de negócios uma vez e usá-los em todos os dispositivos, independentemente da interface do usuário. É provável que você também consiga reutilizar grande parte do modelo de exibição e do modo de exibição entre dispositivos.

## <a name="one-or-two-exceptions-to-the-rule"></a>Uma ou duas exceções à regra

Enquanto você lê este guia de portabilidade, consulte [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md). O mapeamento razoavelmente simples é a regra geral, e a tabela de mapeamentos de namespace e de classe descreve todas as exceções.

No nível do recurso, a boa notícia é que há poucos itens sem suporte na UWP. A maior parte do conjunto de habilidades e do código-fonte é convertida muito bem em aplicativos UWP, como você lerá no restante deste guia de portabilidade. But, here are the few Windows Phone Silverlight features that you may have used for which there is no UWP equivalent.

| Recurso para o qual não há equivalente na UWP. | Windows Phone Silverlight documentation for the feature |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. Em geral, [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) em C++ é o substituto. Consulte [Desenvolvendo jogos](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) e [Interoperabilidade entre DirectX e XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). | [XNA Framework Class Library](https://docs.microsoft.com/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 
|Aplicativos de fotos | [Lenses for Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/jj206990(v=vs.105)) |

&nbsp;

| Tópico| Descrição|
|------|------------| 
| [Namespace and class mappings](wpsl-to-uwp-namespace-and-class-mappings.md) | This topic provides a comprehensive mapping of Windows Phone Silverlight APIs to their UWP equivalents. |
| [Porting the project](wpsl-to-uwp-porting-to-a-uwp-project.md) | You begin the porting process by creating a new Windows 10 project in Visual Studio and copying your files into it. |
| [Solução de problemas](wpsl-to-uwp-troubleshooting.md) | É altamente recomendável ler este guia de portabilidade até o final, mas também entendemos que você esteja ansioso para avançar e chegar ao estágio em que o seu projeto é compilado e executado. Para essa finalidade, você pode fazer um progresso temporário comentando ou eliminando qualquer código não essencial e, em seguida, retornando para saldar esse débito mais tarde. A tabela de solução de problemas de sintomas e soluções deste tópico pode ser útil para você neste estágio, embora não seja uma substituição para a leitura dos próximos tópicos. Você sempre pode consultar novamente a tabela conforme avança pelos próximos tópicos. |
| [Porting XAML and UI](wpsl-to-uwp-porting-xaml-and-ui.md) | The practice of defining UI in the form of declarative XAML markup translates extremely well from Windows Phone Silverlight to UWP apps. Você descobrirá que grandes seções da sua marcação serão compatíveis assim que tiver atualizado as referências de chave de recurso do sistema, alterado alguns nomes de tipo de elemento e alterado "clr-namespace" para "using". |
| [Porting for I/O, device, and app model](wpsl-to-uwp-input-and-sensors.md) | O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele. Ele também pode envolver o processamento de dados. Porém, esse código normalmente não é considerado a camada da interface do usuário ou a camada de dados. Este código inclui a integração com o controlador de vibração, o acelerômetro, o giroscópio, o microfone e o alto-falante (que interceptam o reconhecimento de fala e a síntese), a localização geográfica e as modalidades de entrada, como toque, mouse, teclado e caneta. |
| [Porting business and data layers](wpsl-to-uwp-business-and-data.md) | Por trás de sua interface do usuário estão as camadas de negócios e dados. O código dessas camadas chama as APIs do sistema operacional e do .NET Framework (por exemplo, processamento em segundo plano, localização, a câmera, o sistema de arquivos, rede e outros acessos a dados). A maioria delas está [disponível para um aplicativo UWP](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), portanto, você pode esperar fazer a portabilidade de grande parte desse código sem alteração. |
| [Porting for form factor and UX](wpsl-to-uwp-form-factors-and-ux.md) | Os aplicativos do Windows compartilham uma aparência comum em computadores, dispositivos móveis e muitos outros tipos de dispositivos. Os padrões da interface do usuário, de entrada e de interação são muito semelhantes, e um usuário que alterne entre dispositivos ficará contente com a experiência familiar.|
|[Case study: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | This topic presents a case study of porting a very simple Windows Phone Silverlight app to a Windows 10 UWP app. With Windows 10, you can create a single app package that your customers can install onto a wide range of devices, and that's what we'll do in this case study. |
| [Case study: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | This case study—which builds on the info given in [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)—begins with a Windows Phone Silverlight app that displays grouped data in a **LongListSelector**. No modelo de exibição, cada instância da classe **Author** representa o grupo dos livros escritos por esse autor e, no **LongListSelector**, podemos exibir a lista de livros agrupados por autor ou reduzir o zoom para ver uma lista de atalhos de autores. |

## <a name="related-topics"></a>Tópicos relacionados

**Documentação**
* [What's new for developers in Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest)
* [Guia para aplicativos da Plataforma Universal do Windows (UWP)](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [Roadmap for Universal Windows Platform (UWP) apps using C# or Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))
* [O que mais aguarda os desenvolvedores do Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/dn655121(v=vs.105))

**Artigos de revista**
* [Visual Studio Magazine: Windows Phone 8.1: A Giant Leap Forward for Convergence](https://visualstudiomagazine.com/articles/2014/05/01/whats-new-for-developers-with-windows-phone-8_1.aspx)

**Apresentações**
* [The Story of Bringing Nokia Music from Windows Phone to Windows 8](https://channel9.msdn.com/Events/Build/2013/2-219)
 


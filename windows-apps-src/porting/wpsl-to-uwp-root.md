---
description: Se você for um desenvolvedor com um aplicativo Windows Phone Silverlight, poderá fazer excelente uso de seu conjunto de habilidades e seu código-fonte na mudança para o Windows 10.
title: Mudar do Windows Phone Silverlight para a UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1858600b1d21c2462404a45620584735064176e0
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730337"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>Mudar do Windows Phone Silverlight para a UWP


Se você for um desenvolvedor com um aplicativo Windows Phone Silverlight, poderá fazer excelente uso de seu conjunto de habilidades e seu código-fonte na mudança para o Windows 10. Com o Windows 10, é possível criar um aplicativo da Plataforma Universal do Windows (UWP), que é um único pacote de aplicativo que os clientes podem instalar em cada tipo de dispositivo. Para obter mais informações sobre o Windows 10, os aplicativos UWP e os conceitos de código e interface do usuário adaptável que mencionaremos neste guia de portabilidade, consulte o [Guia para aplicativos da Plataforma Universal do Windows (UWP)](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

Ao portar seu aplicativo Windows Phone Silverlight para um aplicativo do Windows 10, você poderá atualizar os recursos móveis que foram [introduzidos no Windows Phone 8.1](https://docs.microsoft.com/previous-versions/windows/apps/ff402535(v=vs.105)) e ir além deles para usar a UWP (Plataforma Universal do Windows), cujo modelo de aplicativo e estrutura da IU são universais em todos os dispositivos Windows 10. Isso possibilita o suporte a computadores, tablets, telefones e vários outros tipos de dispositivos, de uma base de código e com um pacote do aplicativo. E isso multiplicará o público-alvo potencial do seu aplicativo e criará novas possibilidades com dados compartilhados, produtos consumíveis comprados etc. Para saber mais sobre os novos recursos, consulte [Novidades para desenvolvedores no Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest).

Caso você opte por isso, as versões do Windows Phone Silverlight e do Windows 10 do aplicativo podem estar disponíveis para os clientes ao mesmo tempo.

**Observe**  que este guia foi projetado para ajudá-lo a portar seu Windows Phone aplicativo Silverlight para o Windows 10 manualmente. Além de usar as informações neste guia para fazer a portabilidade do aplicativo, você pode testar a visualização de desenvolvedor do **Mobilize.NET's Silverlight Bridge** para ajudar a automatizar o processo de portabilidade. Essa ferramenta analisa o código-fonte do aplicativo e converte referências em controles do Windows Phone Silverlight e APIs para as contrapartes UWP. Como ainda está em visualização de desenvolvedor, essa ferramenta ainda não manipula todos os cenários de conversão. No entanto, a maioria dos desenvolvedores deve ser capaz de economizar tempo e esforço começando por essa ferramenta. Para testar a visualização de desenvolvedor, visite o [site do Mobilize.NET](https://www.mobilize.net/uwp-bridge).

## <a name="xaml-and-net-or-html"></a>XAML e .NET ou HTML?

Windows Phone o Silverlight tem uma estrutura de interface do usuário XAML baseada no Silverlight 4,0, e você programa em uma versão do .NET Framework e um pequeno subconjunto de APIs de Windows Runtime. Uma vez que você usou XAML (Extensible Application Markup Language) em seu aplicativo Windows Phone Silverlight, é provável que XAML será sua opção para a versão do Windows 10, pois a maior parte do seu conhecimento e de sua experiência será transferida, assim como a maioria do seu código-fonte e os padrões de software que você usa. Até mesmo sua marcação da interface do usuário e o design podem ser prontamente portados. Você achará as APIs gerenciadas, a marcação XAML, a estrutura da IU e as ferramentas muito familiares, e poderá usar C++, C#, ou Visual Basic juntamente com XAML em um aplicativo UWP. Você pode se surpreender com a relativa facilidade do processo, mesmo se houver um desafio ou dois ao longo do caminho.

Veja [Mapa de aplicativos UWP (Plataforma Universal do Windows) usando C# ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10)).

**Observação**  o Windows 10 dá suporte a muito mais do .NET Framework do que um aplicativo da loja Windows Phone. Por exemplo, o Windows 10 tem vários System. ServiceModel. \* namespaces, bem como System.net, System .net. NetworkInformation e System .net. Sockets. Portanto, agora é um ótimo momento para portar seu Windows Phone Silverlight para que seu código .NET seja compilado e funcione na nova plataforma. Consulte [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md).
Outro ótimo motivo para recompilar o código-fonte .NET existente em um aplicativo do Windows 10 é que você aproveitará o .NET Native, uma tecnologia de compilação ahead-of-time que converte MSIL em código de máquina executável nativamente. Aplicativos .NET Native iniciam com mais rapidez, usam menos memória e bateria do que seus equivalentes MSIL.

Este guia de portador se concentrará no XAML, mas, como alternativa, você poderá criar um aplicativo funcionalmente equivalente, chamando muitas das mesmas APIs de Windows Runtime — usando JavaScript, folhas de estilos em cascata (CSS) e HTML5, juntamente com a biblioteca do Windows para JavaScript. Embora as estruturas da IU do Tempo de Execução do Windows de XAML e HTML sejam diferentes umas das outras, qualquer uma delas que você escolher funcionará universalmente em toda a gama de dispositivos Windows.

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>Direcionando a família de dispositivos móveis ou universais

Uma opção que você tem é portar seu aplicativo para um aplicativo direcionado à família de dispositivos universais. Nesse caso, o aplicativo pode ser instalado na mais ampla variedade de dispositivos. Caso o aplicativo chame APIs implementadas somente na família de dispositivos móveis, você pode proteger essas chamadas usando um código adaptável. Também é possível optar por portar o aplicativo para um aplicativo que segmenta a família de dispositivos móveis e, nesse caso, não precisa escrever código adaptável.

## <a name="adapting-your-app-to-multiple-form-factors"></a>Adaptando seu aplicativo a vários fatores forma

A opção que você escolher na seção anterior determinará a gama de dispositivos em que seu aplicativo ou aplicativos serão executados, e pode ser uma ampla variedade de dispositivos. Até mesmo limitar seu aplicativo à família de dispositivos móveis ainda exige uma ampla variedade de tamanhos de tela para as quais dar suporte. Sendo assim, uma vez que seu aplicativo será executado em fatores forma aos quais ele não dava suporte anteriormente, teste sua interface do usuário nesses fatores forma e faça qualquer alteração necessária para que sua interface do usuário se adapte adequadamente em cada um. Você pode considerar essa uma tarefa pós-portabilidade ou uma meta além da portabilidade, e há um exemplo prático disso no estudo de caso [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md).

## <a name="approaching-porting-layer-by-layer"></a>Abordagem de portabilidade camada por camada

-   **Exibição**. O modo de exibição (juntamente com o modelo de exibição) compõe a interface do usuário do seu aplicativo. Idealmente, o modo de exibição consiste em marcação associada às propriedades observáveis de um modelo de exibição. Outro padrão (comum e conveniente, mas somente a curto prazo) destina-se ao código imperativo em um arquivo code-behind para manipular elementos de interface do usuário diretamente. Em ambos os casos, grande parte da marcação da interface do usuário e do design (e até mesmo do código imperativo que manipula os elementos da interface do usuário) terá uma portabilidade simples.
-   **Modelos de exibição e modelos de dados**. Mesmo se você não adotar formalmente padrões de separação de preocupações (como MVVM), inevitavelmente haverá presente em seu aplicativo código que execute a função de modelo de exibição e de modelo de dados. O código do modelo de exibição usa tipos nos namespaces da estrutura da IU. O código dos modelos de exibição e de dados também usa APIs de sistema operacional não visual e do .NET (incluindo APIs para acesso a dados). E a maioria delas está [disponível para um aplicativo UWP](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), portanto, você pode esperar fazer a portabilidade de grande parte desse código sem alteração. Lembre-se: um modelo de exibição é um modelo, ou uma *abstração*, de um modo de exibição. Um modelo de exibição oferece o estado e o comportamento da interface do usuário, enquanto o modo de exibição propriamente dito oferece os elementos visuais. Por esse motivo, qualquer interface do usuário que você adaptar aos diferentes fatores forma em que a UWP permite execução provavelmente precisará de alterações correspondentes no modelo de exibição. Para rede e chamada de serviços de nuvem, você normalmente tem a opção de usar as APIs .NET ou Windows Runtime. Para saber os fatores envolvidos na tomada dessa decisão, consulte [Serviços de nuvem, redes e bancos de dados](wpsl-to-uwp-business-and-data.md).
-   **Serviços de nuvem**. É provável que alguma parte do aplicativo (talvez uma grande parte dele) seja executada na nuvem na forma de serviços. A parte do aplicativo em execução no dispositivo cliente se conecta a elas. Essa é a parte de um aplicativo distribuído que mais provavelmente permanecerá inalterada durante a portabilidade da parte cliente. Se você ainda não tiver, uma boa opção de serviços de nuvem para seu aplicativo UWP são os [Serviços Móveis do Microsoft Azure](https://azure.microsoft.com/services/mobile-services/) que oferecem poderosos componentes de back-end que aplicativos Universais do Windows podem chamar para serviços, desde notificações simples para atualizações de blocos dinâmicos até o tipo de escalabilidade pesada que um farm de servidores pode oferecer.

Antes ou durante a portabilidade, considere se o seu aplicativo pode ser melhorado por meio de refatoração, de forma que o código com finalidade semelhante seja agrupado em camadas e não fique espalhado arbitrariamente. A fatoração de seu aplicativo UWP em camadas como as descritas acima facilita a correção do seu aplicativo, a aplicação de testes nele e, subsequentemente, a leitura e a manutenção dele. Você pode tornar a funcionalidade mais reutilizável - e evitar alguns dos problemas de diferenças de API da interface do usuário entre as plataformas - seguindo o padrão MVVM (Model-View-ViewModel) ([MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)). Esse padrão mantém partes de dados, comercial e da interface do usuário de seu aplicativo separadas umas das outras. Mesmo na interface do usuário, ele mantém o estado e o comportamento separados, e testáveis separadamente, dos elementos visuais. Com o MVVM, você pode escrever seus dados e sua lógica de negócios uma vez e usá-los em todos os dispositivos, independentemente da interface do usuário. É provável que você também consiga reutilizar grande parte do modelo de exibição e do modo de exibição entre dispositivos.

## <a name="one-or-two-exceptions-to-the-rule"></a>Uma ou duas exceções à regra

Enquanto você lê este guia de portabilidade, consulte [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md). O mapeamento razoavelmente simples é a regra geral, e a tabela de mapeamentos de namespace e de classe descreve todas as exceções.

No nível do recurso, a boa notícia é que há poucos itens sem suporte na UWP. A maior parte do conjunto de habilidades e do código-fonte é convertida muito bem em aplicativos UWP, como você lerá no restante deste guia de portabilidade. Porém, aqui estão os poucos recursos do Windows Phone Silverlight que você pode ter usado para os quais não existe equivalente na UWP.

| Recurso para o qual não há equivalente na UWP. | Documentação do Windows Phone Silverlight sobre o recurso |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. Em geral, [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) em C++ é o substituto. Consulte [Desenvolvendo jogos](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) e [Interoperabilidade entre DirectX e XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). | [Biblioteca de classes XNA Framework](https://docs.microsoft.com/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 
|Aplicativos de fotos | [Aplicativos de fotos para o Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/jj206990(v=vs.105)) |

&nbsp;

| Tópico| Descrição|
|------|------------| 
| [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md) | Este tópico fornece um mapeamento abrangente das APIs do Windows Phone Silverlight para seus equivalentes da UWP. |
| [Portando o projeto](wpsl-to-uwp-porting-to-a-uwp-project.md) | Você começa o processo de portabilidade ao criar um novo projeto do Windows 10 no Visual Studio e ao copiar seus arquivos nele. |
| [Solução de problemas](wpsl-to-uwp-troubleshooting.md) | É altamente recomendável ler este guia de portabilidade até o final, mas também entendemos que você esteja ansioso para avançar e chegar ao estágio em que o seu projeto é compilado e executado. Para essa finalidade, você pode fazer um progresso temporário comentando ou eliminando qualquer código não essencial e, em seguida, retornando para saldar esse débito mais tarde. A tabela de solução de problemas de sintomas e soluções deste tópico pode ser útil para você neste estágio, embora não seja uma substituição para a leitura dos próximos tópicos. Você sempre pode consultar novamente a tabela conforme avança pelos próximos tópicos. |
| [Fazendo a portabilidade do XAML e da interface do usuário](wpsl-to-uwp-porting-xaml-and-ui.md) | A prática de definição da interface do usuário na forma de marcação XAML declarativa traduz extremamente bem do Windows Phone Silverlight para aplicativos UWP. Você descobrirá que grandes seções da sua marcação serão compatíveis assim que tiver atualizado as referências de chave de recurso do sistema, alterado alguns nomes de tipo de elemento e alterado "clr-namespace" para "using". |
| [Portabilidade para E/S, dispositivo e modelo de aplicativo](wpsl-to-uwp-input-and-sensors.md) | O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele. Ele também pode envolver o processamento de dados. Porém, esse código normalmente não é considerado a camada da interface do usuário ou a camada de dados. Este código inclui a integração com o controlador de vibração, o acelerômetro, o giroscópio, o microfone e o alto-falante (que interceptam o reconhecimento de fala e a síntese), a localização geográfica e as modalidades de entrada, como toque, mouse, teclado e caneta. |
| [Portando camadas de negócios e dados](wpsl-to-uwp-business-and-data.md) | Por trás de sua interface do usuário estão as camadas de negócios e dados. O código dessas camadas chama as APIs do sistema operacional e do .NET Framework (por exemplo, processamento em segundo plano, localização, a câmera, o sistema de arquivos, rede e outros acessos a dados). A maioria delas está [disponível para um aplicativo UWP](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), portanto, você pode esperar fazer a portabilidade de grande parte desse código sem alteração. |
| [Portabilidade para o fator forma e a experiência do usuário](wpsl-to-uwp-form-factors-and-ux.md) | Os aplicativos do Windows compartilham uma aparência comum em computadores, dispositivos móveis e muitos outros tipos de dispositivos. Os padrões da interface do usuário, de entrada e de interação são muito semelhantes, e um usuário que alterne entre dispositivos ficará contente com a experiência familiar.|
|[Estudo de caso: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | Este tópico apresenta um estudo de caso de portabilidade de um aplicativo Windows Phone Silverlight muito simples para um aplicativo UWP (Plataforma Universal do Windows) do Windows 10. Com o Windows 10, é possível criar um único pacote do aplicativo que os clientes podem instalar em uma ampla variedade de dispositivos, e é isso o que faremos neste estudo de caso. |
| [Estudo de caso: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | Este estudo de caso, que se baseia nas informações fornecidas no [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md), começa com um aplicativo do Windows Phone Silverlight que exibe dados agrupados em um **LongListSelector**. No modelo de exibição, cada instância da classe **Author** representa o grupo dos livros escritos por esse autor e, no **LongListSelector**, podemos exibir a lista de livros agrupados por autor ou reduzir o zoom para ver uma lista de atalhos de autores. |

## <a name="related-topics"></a>Tópicos relacionados

**Documentação**
* [Novidades para desenvolvedores no Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest)
* [Guia para aplicativos UWP (Plataforma Universal do Windows)](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [Mapa de aplicativos UWP (Plataforma Universal do Windows) usando C# ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))
* [O que mais aguarda os desenvolvedores do Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/dn655121(v=vs.105))

**Artigos de revista**
* [Visual Studio Magazine: Windows Phone 8.1: A Giant Leap Forward for Convergence](https://visualstudiomagazine.com/articles/2014/05/01/whats-new-for-developers-with-windows-phone-8_1.aspx)

**Apresentações**
* [A história de migrar o Nokia Music do Windows Phone para Windows 8](https://channel9.msdn.com/Events/Build/2013/2-219)
 


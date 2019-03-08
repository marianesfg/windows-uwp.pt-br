---
description: Se você for um desenvolvedor com um aplicativo do Windows Phone Silverlight, você pode fazer bom uso do seu conjunto de habilidades e seu código-fonte na mudança para o Windows 10.
title: Mover do Windows Phone Silverlight para UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c644e53ec0126edf418ead573e50653e8cb1cf0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649931"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>Mover do Windows Phone Silverlight para UWP


Se você for um desenvolvedor com um aplicativo do Windows Phone Silverlight, você pode fazer bom uso do seu conjunto de habilidades e seu código-fonte na mudança para o Windows 10. Com o Windows 10, você pode criar um aplicativo de plataforma Universal do Windows (UWP), que é um pacote de aplicativo único que os clientes podem instalar em todos os tipos de dispositivo. Para obter mais informações sobre o Windows 10, aplicativos da UWP e os conceitos de código adaptável e interface do usuário adaptável que mencionaremos neste guia de portabilidade, consulte o [guia para aplicativos da plataforma Universal do Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631).

Ao portar seu aplicativo do Windows Phone Silverlight para um aplicativo do Windows 10, você poderá acompanhar os recursos móveis que foram [introduzido no Windows Phone 8.1](https://msdn.microsoft.com/library/windows/apps/dn632424)e que vão muito além deles usem a Universal Windows Platform (UWP) cujo aplicativo modelo e estrutura de interface do usuário são universais em todos os dispositivos Windows 10. Isso possibilita o suporte a computadores, tablets, telefones e vários outros tipos de dispositivos, de uma base de código e com um pacote do aplicativo. E isso multiplicará o público-alvo potencial do seu aplicativo e criará novas possibilidades com dados compartilhados, produtos consumíveis comprados etc. Para obter mais informações sobre os novos recursos, consulte [quais são as novidades para desenvolvedores no Windows 10](https://dev.windows.com/getstarted/whats-new-windows-10).

Se você optar por, a versão do Windows Phone Silverlight do seu aplicativo e o Windows 10 versão dele podem ambos ser disponíveis para clientes ao mesmo tempo.

**Observação**  este guia foi projetado para ajudá-lo a portar seu aplicativo do Windows Phone Silverlight para Windows 10 manualmente. Além de usar as informações neste guia para fazer a portabilidade do aplicativo, você pode testar a visualização de desenvolvedor do **Mobilize.NET's Silverlight Bridge** para ajudar a automatizar o processo de portabilidade. Essa ferramenta analisa o código-fonte do seu aplicativo e converte referências aos controles do Windows Phone Silverlight e as APIs às suas contrapartes UWP. Como ainda está em visualização de desenvolvedor, essa ferramenta ainda não manipula todos os cenários de conversão. No entanto, a maioria dos desenvolvedores deve ser capaz de economizar tempo e esforço começando por essa ferramenta. Para testar a visualização de desenvolvedor, visite o [site do Mobilize.NET](https://go.microsoft.com/fwlink/p/?LinkId=624546).

## <a name="xaml-and-net-or-html"></a>XAML e .NET ou HTML?

Windows Phone Silverlight tem uma estrutura XAML UI com base no Silverlight 4.0 e você programar em relação uma versão do .NET Framework e um pequeno subconjunto de APIs de UWP. Uma vez que você usou de de Extensible Application Markup Language (XAML) em seu aplicativo Windows Phone Silverlight, é provável que o XAML será sua escolha para a sua versão do Windows 10 porque a maioria dos seus dados de conhecimento e experiência transferirá, assim como grande parte do seu código-fonte e os padrões de software que você usa. Até mesmo sua marcação da interface do usuário e o design podem ser prontamente portados. Você achará as APIs gerenciadas, a marcação XAML, a estrutura da IU e as ferramentas muito familiares, e poderá usar C++, C#, ou Visual Basic juntamente com XAML em um aplicativo UWP. Você pode se surpreender com a relativa facilidade do processo, mesmo se houver um desafio ou dois ao longo do caminho.

Veja [Mapa de aplicativos UWP (Plataforma Universal do Windows) usando C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583).

**Observação**  Windows 10 dá suporte a muito mais do que o .NET Framework que um aplicativo do Windows Phone Store. Por exemplo, o Windows 10 tem vários System. ServiceModel. \* namespaces, bem como Sockets, NetworkInformation e System.Net. Portanto, agora é um ótimo momento para Windows Phone Silverlight da porta e têm seu código .NET apenas compilarão e funcionarão na nova plataforma. Consulte [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md).
Outra boa razão para recompilar seu código-fonte .NET existente em um aplicativo do Windows 10 é que você se beneficiará do .NET Native, que uma tecnologia de compilação ahead of time que converte o MSIL em código executável nativo de máquina. Aplicativos .NET Native iniciam com mais rapidez, usam menos memória e bateria do que seus equivalentes MSIL.

Este guia de portabilidade se concentrará em XAML, mas, como alternativa, você poderá compilar um aplicativo funcionalmente equivalente (chamando muitas das mesmas APIs da UWP) usando JavaScript, CSS (Folhas de Estilos em Cascata) e HTML5 em conjunto com a Biblioteca do Windows para JavaScript. Embora as estruturas da IU do Tempo de Execução do Windows de XAML e HTML sejam diferentes umas das outras, qualquer uma delas que você escolher funcionará universalmente em toda a gama de dispositivos Windows.

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>Direcionando a família de dispositivos móveis ou universais

Uma opção que você tem é portar seu aplicativo para um aplicativo direcionado à família de dispositivos universais. Nesse caso, o aplicativo pode ser instalado na mais ampla variedade de dispositivos. Caso o aplicativo chame APIs implementadas somente na família de dispositivos móveis, você pode proteger essas chamadas usando um código adaptável. Também é possível optar por portar o aplicativo para um aplicativo que segmenta a família de dispositivos móveis e, nesse caso, não precisa escrever código adaptável.

## <a name="adapting-your-app-to-multiple-form-factors"></a>Adaptando seu aplicativo a vários fatores forma

A opção que você escolher na seção anterior determinará a gama de dispositivos em que seu aplicativo ou aplicativos serão executados, e pode ser uma ampla variedade de dispositivos. Até mesmo limitar seu aplicativo à família de dispositivos móveis ainda exige uma ampla variedade de tamanhos de tela para as quais dar suporte. Sendo assim, uma vez que seu aplicativo será executado em fatores forma aos quais ele não dava suporte anteriormente, teste sua interface do usuário nesses fatores forma e faça qualquer alteração necessária para que sua interface do usuário se adapte adequadamente em cada um. Você pode considerar essa uma tarefa pós-portabilidade ou uma meta além da portabilidade, e há um exemplo prático disso no estudo de caso [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md).

## <a name="approaching-porting-layer-by-layer"></a>Abordagem de portabilidade camada por camada

-   **Modo de exibição**. O modo de exibição (juntamente com o modelo de exibição) compõe a interface do usuário do seu aplicativo. Idealmente, o modo de exibição consiste em marcação associada às propriedades observáveis de um modelo de exibição. Outro padrão (comum e conveniente, mas somente a curto prazo) destina-se ao código imperativo em um arquivo code-behind para manipular elementos de interface do usuário diretamente. Em ambos os casos, grande parte da marcação da interface do usuário e do design (e até mesmo do código imperativo que manipula os elementos da interface do usuário) terá uma portabilidade simples.
-   **Exiba modelos e modelos de dados**. Mesmo se você não adotar formalmente padrões de separação de preocupações (como MVVM), inevitavelmente haverá presente em seu aplicativo código que execute a função de modelo de exibição e de modelo de dados. O código do modelo de exibição usa tipos nos namespaces da estrutura da IU. O código dos modelos de exibição e de dados também usa APIs de sistema operacional não visual e do .NET (incluindo APIs para acesso a dados). E a maioria delas está [disponível para um aplicativo UWP](https://msdn.microsoft.com/library/windows/apps/br211369), portanto, você pode esperar fazer a portabilidade de grande parte desse código sem alteração. Lembre-se: um modelo de exibição é um modelo, ou uma *abstração*, de um modo de exibição. Um modelo de exibição oferece o estado e o comportamento da interface do usuário, enquanto o modo de exibição propriamente dito oferece os elementos visuais. Por esse motivo, qualquer interface do usuário que você adaptar aos diferentes fatores forma em que a UWP permite execução provavelmente precisará de alterações correspondentes no modelo de exibição. Para redes e chamadas de serviços de nuvem, você geralmente tem a opção de usar APIs UWP ou .NET. Para saber os fatores envolvidos na tomada dessa decisão, consulte [Serviços de nuvem, redes e bancos de dados](wpsl-to-uwp-business-and-data.md).
-   **Serviços de nuvem**. É provável que alguma parte do aplicativo (talvez uma grande parte dele) seja executada na nuvem na forma de serviços. A parte do aplicativo em execução no dispositivo cliente se conecta a elas. Essa é a parte de um aplicativo distribuído que mais provavelmente permanecerá inalterada durante a portabilidade da parte cliente. Se você ainda não tiver, uma boa opção de serviços de nuvem para seu aplicativo UWP são os [Serviços Móveis do Microsoft Azure](https://azure.microsoft.com/services/mobile-services/) que oferecem poderosos componentes de back-end que aplicativos Universais do Windows podem chamar para serviços, desde notificações simples para atualizações de blocos dinâmicos até o tipo de escalabilidade pesada que um farm de servidores pode oferecer.

Antes ou durante a portabilidade, considere se o seu aplicativo pode ser melhorado por meio de refatoração, de forma que o código com finalidade semelhante seja agrupado em camadas e não fique espalhado arbitrariamente. A fatoração de seu aplicativo UWP em camadas como as descritas acima facilita a correção do seu aplicativo, a aplicação de testes nele e, subsequentemente, a leitura e a manutenção dele. Você pode tornar a funcionalidade mais reutilizável - e evitar alguns dos problemas de diferenças de API da interface do usuário entre as plataformas - seguindo o padrão MVVM (Model-View-ViewModel) ([MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)). Esse padrão mantém partes de dados, comercial e da interface do usuário de seu aplicativo separadas umas das outras. Mesmo na interface do usuário, ele mantém o estado e o comportamento separados, e testáveis separadamente, dos elementos visuais. Com o MVVM, você pode escrever seus dados e sua lógica de negócios uma vez e usá-los em todos os dispositivos, independentemente da interface do usuário. É provável que você também consiga reutilizar grande parte do modelo de exibição e do modo de exibição entre dispositivos.

## <a name="one-or-two-exceptions-to-the-rule"></a>Uma ou duas exceções à regra

Enquanto você lê este guia de portabilidade, consulte [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md). O mapeamento razoavelmente simples é a regra geral, e a tabela de mapeamentos de namespace e de classe descreve todas as exceções.

No nível do recurso, a boa notícia é que há poucos itens sem suporte na UWP. A maior parte do conjunto de habilidades e do código-fonte é convertida muito bem em aplicativos UWP, como você lerá no restante deste guia de portabilidade. Mas, aqui estão que o Windows Phone alguns recursos do Silverlight que você pode ter usado para o qual não há nenhum equivalente UWP.

| Recurso para o qual não há equivalente na UWP. | Documentação do Windows Phone Silverlight para o recurso |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. Em geral, [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) em C++ é o substituto. Consulte [Desenvolvendo jogos](https://msdn.microsoft.com/library/windows/apps/hh452744) e [Interoperabilidade entre DirectX e XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). | [Biblioteca de classes do XNA Framework](https://msdn.microsoft.com/library/bb203940.aspx) | 
|Aplicativos de fotos | [Lentes para Windows Phone 8](https://msdn.microsoft.com/library/windows/apps/jj206990.aspx) |

&nbsp;

| Tópico| Descrição|
|------|------------| 
| [Mapeamentos de Namespace e classe](wpsl-to-uwp-namespace-and-class-mappings.md) | Este tópico fornece um mapeamento abrangente de APIs do Windows Phone Silverlight para seus equivalentes UWP. |
| [Portabilidade do projeto](wpsl-to-uwp-porting-to-a-uwp-project.md) | Você pode começar o processo de portabilidade criando um novo projeto do Windows 10 no Visual Studio e copiar os arquivos nele. |
| [Solução de problemas](wpsl-to-uwp-troubleshooting.md) | É altamente recomendável ler este guia de portabilidade até o final, mas também entendemos que você esteja ansioso para avançar e chegar ao estágio em que o seu projeto é compilado e executado. Para essa finalidade, você pode fazer um progresso temporário comentando ou eliminando qualquer código não essencial e, em seguida, retornando para saldar esse débito mais tarde. A tabela de solução de problemas de sintomas e soluções deste tópico pode ser útil para você neste estágio, embora não seja uma substituição para a leitura dos próximos tópicos. Você sempre pode consultar novamente a tabela conforme avança pelos próximos tópicos. |
| [Portabilidade de XAML e a interface do usuário](wpsl-to-uwp-porting-xaml-and-ui.md) | A prática de definição de interface do usuário na forma de marcação declarativa do XAML traduz muito bem do Windows Phone Silverlight para aplicativos UWP. Você descobrirá que grandes seções da sua marcação serão compatíveis assim que tiver atualizado as referências de chave de recurso do sistema, alterado alguns nomes de tipo de elemento e alterado "clr-namespace" para "using". |
| [Portabilidade para o modelo de aplicativo, dispositivo e e/s](wpsl-to-uwp-input-and-sensors.md) | O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele. Ele também pode envolver o processamento de dados. Porém, esse código normalmente não é considerado a camada da interface do usuário ou a camada de dados. Este código inclui a integração com o controlador de vibração, o acelerômetro, o giroscópio, o microfone e o alto-falante (que interceptam o reconhecimento de fala e a síntese), a localização geográfica e as modalidades de entrada, como toque, mouse, teclado e caneta. |
| [Portabilidade de camadas de dados e de negócios](wpsl-to-uwp-business-and-data.md) | Por trás de sua interface do usuário estão as camadas de negócios e dados. O código dessas camadas chama as APIs do sistema operacional e do .NET Framework (por exemplo, processamento em segundo plano, localização, a câmera, o sistema de arquivos, rede e outros acessos a dados). A maioria delas está [disponível para um aplicativo UWP](https://msdn.microsoft.com/library/windows/apps/br211369), portanto, você pode esperar fazer a portabilidade de grande parte desse código sem alteração. |
| [Portabilidade para o formato e a experiência do usuário](wpsl-to-uwp-form-factors-and-ux.md) | Os aplicativos do Windows compartilham uma aparência comum em computadores, dispositivos móveis e muitos outros tipos de dispositivos. Os padrões da interface do usuário, de entrada e de interação são muito semelhantes, e um usuário que alterne entre dispositivos ficará contente com a experiência familiar.|
|[Estudo de caso: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | Este tópico apresenta um estudo de caso de portabilidade de um aplicativo muito simple do Windows Phone Silverlight para um aplicativo de UWP do Windows 10. Com o Windows 10, você pode criar um pacote de aplicativo único que os clientes podem instalar em uma ampla variedade de dispositivos, e que é o que vamos fazer este estudo de caso. |
| [Estudo de caso: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | Este estudo de caso — que se baseia as informações fornecidas na [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)— começa com um aplicativo que exibe dados agrupados em do Windows Phone Silverlight uma **LongListSelector**. No modelo de exibição, cada instância da classe **Author** representa o grupo dos livros escritos por esse autor e, no **LongListSelector**, podemos exibir a lista de livros agrupados por autor ou reduzir o zoom para ver uma lista de atalhos de autores. |

## <a name="related-topics"></a>Tópicos relacionados

**Documentação**
* [O que há de novo para desenvolvedores no Windows 10](https://dev.windows.com/getstarted/whats-new-windows-10)
* [Guia para aplicativos da Plataforma Universal do Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631)
* [Roteiro para aplicativos de plataforma Universal do Windows (UWP) usando o C# ou o Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583)
* [O que mais aguarda os desenvolvedores do Windows Phone 8](https://msdn.microsoft.com/library/windows/apps/xaml/dn655121.aspx)

**Artigos de revista**
* [Visual Studio Magazine: Windows Phone 8.1: Um grande salto à frente para a convergência](https://go.microsoft.com/fwlink/p/?LinkID=398541)

**Apresentações**
* [A história de levar Nokia música do Windows Phone para Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=321521)
 


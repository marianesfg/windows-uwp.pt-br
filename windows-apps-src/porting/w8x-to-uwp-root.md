---
author: stevewhims
description: Se você tiver um aplicativo Universal 8.1 & \#8212;whether para o Windows 8.1, Windows Phone 8.1 ou ambos & \#8212;then que você descobrirá que seu código-fonte e suas habilidades serão portados perfeitamente para Windows 10.
title: Mudar do Windows Runtime 8.x para a UWP
ms.assetid: ac163b57-dee0-43fa-bab9-8c37fbee3913
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eebd0467696b78458835425f7feac903ba435f42
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6855783"
---
# <a name="move-from-windows-runtime-8x-to-uwp"></a>Mudar do Windows Runtime 8.x para a UWP


Se você tiver um aplicativo Universal 8.1 — se ele está direcionando o Windows 8.1, Windows Phone 8.1 ou ambos, saiba que seu código-fonte e suas habilidades serão portados perfeitamente para Windows 10. Com o Windows 10, você pode criar um aplicativo da plataforma Universal do Windows (UWP), que é um pacote de aplicativo único que os clientes podem instalar em cada tipo de dispositivo. Para obter mais informações sobre o Windows 10, aplicativos UWP e os conceitos de código e interface do usuário adaptável que mencionaremos neste guia de portabilidade, consulte o [guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631).

Ao fazer a portabilidade, você perceberá que o Windows 10 compartilha a maioria das APIs com as plataformas anteriores, bem como marcação XAML, estrutura de interface do usuário e ferramentas, e você achará tudo isso muito familiar. Assim como antes, você ainda pode escolher entre C++, C# e Visual Basic como a linguagem de programação a ser usada com a estrutura da IU XAML. Seus primeiros passos ao planejar exatamente o que fazer com seu aplicativo atual ou aplicativos dependerá dos tipos de aplicativos e projetos que você tem. Isso é explicado nas seções a seguir.

## <a name="if-you-have-a-universal-81-app"></a>Se você já tiver um aplicativo Universal 8.1

Um aplicativo Universal 8.1 é criado a partir de um projeto de Aplicativo Universal 8.1. Digamos que o nome do projeto é AppName\_81. Ele contém estes subprojetos.

-   AppName\_81.Windows. Este é o projeto que compila o pacote do aplicativo para o Windows 8.1.
-   AppName\_81.WindowsPhone. Este é o projeto que compila o pacote do aplicativo para o Windows Phone 8.1.
-   AppName\_81.Shared. Este é o projeto que contém o código-fonte, os arquivos de marcação e outros ativos e recursos usados pelos outros dois projetos.

Geralmente, um aplicativo Universal do Windows 8.1 oferece os mesmos recursos — e faz isso usando o mesmo código e marcação — em formatos Windows 8.1 e Windows Phone 8.1. Um aplicativo como esse é um candidato ideal para a portabilidade para um único aplicativo do Windows 10 que segmenta a família de dispositivos Universal (e que você pode instalar na mais ampla variedade de dispositivos). Você portará essencialmente o conteúdo do projeto compartilhado e precisará usar pouco ou nada dos outros dois projetos porque haverá pouco ou nada neles.

Outras vezes, o Windows 8.1 e/ou o formulário do Windows Phone 8.1 do aplicativo contêm recursos únicos. Ou eles contêm os mesmos recursos, mas implementam esses recursos usando técnicas diferentes ou tecnologia distinta. Com um aplicativo assim, é possível optar por fazer a portabilidade de um único aplicativo direcionado à família de dispositivos Universal (neste caso, você vai desejar que o aplicativo se adapte a dispositivos diferentes) ou pode optar por portá-lo como mais de um aplicativo, talvez um direcionado à família de dispositivos da área de trabalho e outro à família de dispositivos móveis. A natureza do aplicativo Universal 8.1 determinará quais destas opções são melhores para o seu caso.

1.  Porte o conteúdo do projeto compartilhado para um aplicativo destinado à família de dispositivos Universal. Se aplicável, recupere qualquer outro conteúdo de projetos do Windows e do WindowsPhone, e use esse conteúdo incondicionalmente no aplicativo ou condicionalmente no dispositivo em que o seu aplicativo está sendo executado no momento (o último comportamento é conhecido como *adaptável*).
2.  Porte o conteúdo do projeto do WindowsPhone para um aplicativo destinado à família de dispositivos Universal. Se aplicável, resgate qualquer outro conteúdo do projeto do Windows, usando-o incondicionalmente ou de modo adaptável.
3.  Porte o conteúdo do projeto do Windows para um aplicativo destinado à família de dispositivos Universal. Se aplicável, resgate qualquer outro conteúdo do projeto do WindowsPhone, usando-o incondicionalmente ou de modo adaptável.
4.  Porte o conteúdo do projeto do Windows para um aplicativo direcionado à família de dispositivos Universal ou da área de trabalho, e porte também o conteúdo do projeto do WindowsPhone para um aplicativo direcionado para a família de dispositivos Universal ou móveis. É possível criar uma solução com um projeto compartilhado e continuar compartilhando código-fonte, arquivos de marcação e outros ativos e recursos entre os dois projetos. Ou é possível criar soluções diferentes e ainda compartilhar os mesmos itens usando links.

## <a name="if-you-have-a-windows-81-app"></a>Se você já tiver um aplicativo do Windows 8.1

Porte o projeto para um aplicativo direcionado à família de dispositivos Universal ou da área de trabalho. Se escolher a família de dispositivos Universal e o aplicativo chamar as APIs que são implementadas somente na família de dispositivos Desktop, você poderá proteger essas chamadas com código adaptável.

## <a name="if-you-have-a-windows-phone-81-app"></a>Se você já tiver um aplicativo do Windows Phone 8.1

Porte o projeto para um aplicativo direcionado à família de dispositivos Universal ou móveis. Se escolher a família de dispositivos Universal e o aplicativo chamar as APIs que são implementadas somente na família de dispositivos móveis, você poderá proteger essas chamadas com código adaptável.

## <a name="adapting-your-app-to-multiple-form-factors"></a>Adaptanso seu aplicativo a vários fatores forma

A opção que você escolher entre as seções anteriores determinará a gama de dispositivos em que seu aplicativo ou aplicativos serão executados, e pode ser uma ampla variedade de dispositivos. Até mesmo limitar seu aplicativo à família de dispositivos móveis ainda exige uma ampla variedade de tamanhos de tela para as quais dar suporte. Assim, se o aplicativo for executado em fatores forma a que ele não dava suporte anteriormente, teste a interface do usuário nesses fatores forma e faça qualquer alteração necessária de maneira que a interface do usuário se adapte corretamente a cada um. É possível pensar nisso como uma tarefa pós-portabilidade ou uma meta além da portabilidade, e existem alguns exemplos disso na prática nos casos de estudo [Bookstore2](w8x-to-uwp-case-study-bookstore2.md) e [QuizGame](w8x-to-uwp-case-study-quizgame.md).

## <a name="approaching-porting-layer-by-layer"></a>Abordagem de portabilidade camada por camada

Ao portar um aplicativo Universal 8.1 para o modelo de aplicativos UWP, praticamente todo o seu conhecimento e a sua experiência serão transferidos, assim como grande parte do seu código-fonte e marcação, assim como os padrões de software que você usa.

-   **Modo de Exibição**. O modo de exibição (juntamente com o modelo de exibição) compõe a interface do usuário do seu aplicativo. Idealmente, o modo de exibição consiste em marcação associada às propriedades observáveis de um modelo de exibição. Outro padrão (comum e conveniente, mas somente a curto prazo) destina-se ao código imperativo em um arquivo code-behind para manipular elementos de interface do usuário diretamente. Em ambos os casos, sua marcação da interface do usuário e o design, e até mesmo o código imperativo que manipula os elementos da interface do usuário, serão simples de portar.
-   **Modelos de exibição e modelos de dados**. Mesmo se você não adotar formalmente padrões de separação de preocupações (como MVVM), inevitavelmente haverá presente em seu aplicativo código que execute a função de modelo de exibição e de modelo de dados. O código do modelo de exibição usa tipos nos namespaces da estrutura da IU. O código dos modelos de exibição e de dados também usa APIs de sistema operacional não visual e do .NET Framework (incluindo APIs para acesso a dados). Essas APIs estão [disponíveis para aplicativos UWP, também](https://msdn.microsoft.com/library/windows/apps/br211369), portanto, a maior parte, senão todo este código será portado sem alteração.
-   **Serviços de nuvem**. É provável que alguma parte do aplicativo (talvez uma grande parte dele) seja executada na nuvem na forma de serviços. A parte do aplicativo em execução no dispositivo cliente se conecta a elas. Essa é a parte de um aplicativo distribuído que mais provavelmente permanecerá inalterada durante a portabilidade da parte cliente. Se você ainda não tiver, uma boa opção de serviços de nuvem para seu aplicativo UWP é o [Serviços Móveis do Microsoft Azure](http://azure.microsoft.com/services/mobile-services/), que oferece poderosos componentes de back-end que seu aplicativo pode chamar para serviços desde notificações simples para atualizações de blocos dinâmicos até o tipo de escalabilidade pesada que um farm de servidores pode oferecer.

Antes ou durante a portabilidade, considere se o seu aplicativo pode ser melhorado por meio de refatoração, de forma que o código com finalidade semelhante seja agrupado em camadas e não fique espalhado arbitrariamente. A fatoração de seu aplicativo em camadas como as descritas acima facilita a correção do seu aplicativo, a aplicação de testes nele e, subsequentemente, a leitura e a manutenção dele. Você pode tornar a funcionalidade mais reutilizável seguindo o padrão Model-View-ViewModel ([MVVM](http://msdn.microsoft.com/magazine/dd419663.aspx)). Esse padrão mantém partes de dados, comercial e da interface do usuário de seu aplicativo separadas umas das outras. Mesmo na interface do usuário, ele mantém o estado e o comportamento separados, e testáveis separadamente, dos elementos visuais. Com o MVVM, você pode escrever seus dados e sua lógica de negócios uma vez e usá-los em todos os dispositivos, independentemente da interface do usuário. É provável que você também consiga reutilizar grande parte do modelo de exibição e do modo de exibição entre dispositivos.

| Tópico | Descrição |
|-------|-------------|
| [Portando o projeto](w8x-to-uwp-porting-to-a-uwp-project.md) | Você tem duas opções ao começar o processo de portabilidade. Uma é editar uma cópia dos arquivos do seu projeto existente, inclusive o manifesto do pacote do aplicativo (para essa opção, consulte as informações sobre como atualizar seus arquivos de projeto em [Migrar aplicativos para a UWP (Plataforma Universal do Windows)](https://msdn.microsoft.com/library/mt148501.aspx)). A outra opção é criar um novo projeto do Windows 10 no Visual Studio e copiar seus arquivos para ele. |
| [Solução de problemas](w8x-to-uwp-troubleshooting.md) | É altamente recomendável ler este guia de portabilidade até o final, mas também entendemos que você esteja ansioso para avançar e chegar ao estágio em que o seu projeto é compilado e executado. Para essa finalidade, você pode fazer um progresso temporário comentando ou eliminando qualquer código não essencial e, em seguida, retornando para saldar esse débito mais tarde. A tabela de solução de problemas de sintomas e soluções deste tópico pode ser útil para você neste estágio, embora não seja uma substituição para a leitura dos próximos tópicos. Você sempre pode consultar novamente a tabela conforme avança pelos próximos tópicos. |
| [Portando XAML e a interface do usuário](w8x-to-uwp-porting-xaml-and-ui.md) | A prática de definição da interface do usuário na forma de marcação XAML declarativa traduz extremamente bem dos aplicativos Universal 8.1 para aplicativos UWP. Você descobrirá que a maior parte da sua marcação é compatível, embora talvez seja necessário fazer alguns ajustes nas chaves de recurso do sistema ou nos modelos personalizados que você está usando. |
| [Portabilidade para E/S, dispositivo e modelo de aplicativo](w8x-to-uwp-input-and-sensors.md) | O código que se integra ao dispositivo propriamente dito e aos sensores envolve a entrada do usuário e a saída para ele. Ele também pode envolver o processamento de dados. Porém, esse código normalmente não é considerado a camada da interface do usuário ou a camada de dados. Este código inclui a integração com o controlador de vibração, o acelerômetro, o giroscópio, o microfone e o alto-falante (que interceptam o reconhecimento de fala e a síntese), a localização geográfica e as modalidades de entrada, como toque, mouse, teclado e caneta. |
| [Estudo de caso: Bookstore1](w8x-to-uwp-case-study-bookstore1.md) | Este tópico apresenta um estudo de caso de portabilidade de um aplicativo universal 8.1 muito simples para um aplicativo UWP do Windows 10. Um aplicativo Universal 8.1 é aquele que cria um pacote do aplicativo para o Windows 8.1, e um pacote do aplicativo diferente para o Windows Phone 8.1. Com o Windows 10, é possível criar um único pacote do aplicativo que os clientes podem instalar em uma ampla variedade de dispositivos, e é isso o que faremos neste estudo de caso. Consulte [Guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631). |
| [Estudo de caso: Bookstore2](w8x-to-uwp-case-study-bookstore2.md) | Este estudo de caso, que se baseia nas informações fornecidas do controle [SemanticZoom](https://msdn.microsoft.com/library/windows/apps/hh702601). No modelo de exibição, cada instância da classe Author representa o grupo dos livros escritos por esse autor e, no SemanticZoom, podemos exibir a lista de livros agrupados por autor ou reduzir o zoom para ver uma lista de atalhos de autores. |
| [Estudo de caso: QuizGame](w8x-to-uwp-case-study-quizgame.md) | Este tópico apresenta um estudo de caso de portabilidade de uma amostra de aplicativo do WinRT 8.1 de um jogo de teste ponto a ponto em funcionamento para um aplicativo UWP do Windows 10. |

## <a name="related-topics"></a>Tópicos relacionados

**Documentação**
* [Referência do Windows Runtime](https://msdn.microsoft.com/library/windows/apps/br211377)
* [Criando aplicativos Universais do Windows para todos os dispositivos Windows](http://go.microsoft.com/fwlink/p/?LinkID=397871)
* [Criando a experiência do usuário para aplicativos](https://msdn.microsoft.com/library/windows/apps/hh767284)

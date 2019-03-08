---
Description: Aprenda técnicas de design responsivo para personalizar seu aplicativo para dispositivos específicos
label: Responsive design techniques
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9e06131da5d7dd56354e1871867f9956ad13aa2c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624811"
---
# <a name="responsive-design-techniques"></a>Técnicas de design responsivo

Aplicativos UWP usam pixels efetivos para garantir que sua interface do usuário será legível e pode ser usado em todos os dispositivos com Windows. Sendo assim, por que você desejaria personalizar a interface do usuário do seu aplicativo para uma família de dispositivos específica?

- **Para tornar o uso mais eficiente do espaço e reduzir a necessidade de navegar**

    Se você criar um aplicativo para uma boa aparência em um dispositivo que tem uma tela pequena, como um tablet, o aplicativo será usado em um computador com tela muito maior, mas provavelmente haverá desperdício de espaço. Você pode personalizar o aplicativo para exibir mais conteúdo quando a tela tiver acima de um determinado tamanho. Por exemplo, um aplicativo de compras pode exibir uma categoria de mercadorias de cada vez em um tablet, mas mostrar várias categorias e produtos simultaneamente em um computador ou laptop.

    Ao colocar mais conteúdo na tela, você reduz a quantidade de navegação que o usuário precisa executar.

- **Para tirar proveito dos recursos de dispositivos**

    Certos dispositivos têm mais probabilidade de ter determinados recursos de dispositivo. Por exemplo, laptops provavelmente têm um sensor de localização e uma câmera, enquanto uma TV não pode ter qualquer um. Seu aplicativo pode detectar quais recursos estão disponíveis e habilitar recursos que os utilizam.

- **Para otimizar a entrada**

    A biblioteca de controles universais funciona com todos os tipos de entrada (toque, caneta, teclado, mouse), mas você ainda pode otimizar para certos tipos de entrada reorganizando seus elementos de interface do usuário. Por exemplo, se você colocar elementos de navegação na parte inferior da tela, será mais fácil para os usuários de telefone acessá-los, mas a maioria dos usuários de computador espera ver os elementos de navegação na parte superior da tela.

Quando você otimiza a interface do usuário de seu aplicativo para larguras de tela específicas, dizemos que você está criando um design responsivo. Aqui estão as seis técnicas de design responsivo que você pode usar para personalizar a interface do usuário do seu aplicativo.

>[!TIP]
> Muitos controles UWP implementam automaticamente esses comportamentos responsivos. Para criar uma interface de usuário responsiva, é recomendável fazer check-out a [controles UWP](../controls-and-patterns/index.md).

## <a name="reposition"></a>Reposicionar

Você pode alterar o local e a posição dos elementos de interface do usuário para aproveitar ao máximo o tamanho da janela. Neste exemplo, a menor janela pilhas elementos verticalmente. Quando o aplicativo se traduz em uma janela maior, elementos podem tirar proveito da largura da janela mais amplo.

![Reposicionar](images/rsp-design/rspd-reposition2.gif)

Neste design de exemplo para um aplicativo de fotos, o aplicativo de fotos reposiciona seu conteúdo em telas maiores.

## <a name="resize"></a>Redimensionar

Você pode otimizar para o tamanho da janela, ajustando as margens e o tamanho dos elementos de interface do usuário. Por exemplo, isso poderia aumentar a experiência de leitura em uma tela maior, simplesmente crescendo o quadro de conteúdo.

![Redimensionando elementos de design](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Refluxo

Alterando o fluxo de elementos da interface do usuário com base no dispositivo e na orientação, seu aplicativo pode oferecer uma exibição ideal do conteúdo. Por exemplo, quando então uma tela maior, pode fazer sentido para adicionar colunas, usar contêineres maiores ou gerar itens de lista de maneira diferente.

Este exemplo mostra como uma única coluna de conteúdo em uma tela menor do que o fluxo pode ser redirecionado em uma tela maior para exibir duas colunas de texto de rolagem vertical.

![Refluindo elementos de design](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Mostrar/ocultar

Você pode mostrar ou ocultar elementos da interface do usuário com base no estado real da tela ou quando o dispositivo oferecer suporte a funcionalidades adicionais, situações específicas ou orientações de tela preferenciais.

![Ocultando elementos de design](images/rsp-design/rspd-revealhide.gif)

Por exemplo, os controles do player de mídia reduzem o conjunto de botões em telas menores e expandem em telas maiores. O player de mídia em uma janela maior pode lidar com muito mais na tela funcionalidade que ele pode em uma janela menor.

Parte da técnica de revelar ou ocultar inclui escolher quando exibir mais metadados. Com o windows menores, é melhor mostrar uma quantidade mínima de metadados. Com o windows maiores, uma quantidade significativa de metadados pode ser apresentada. Alguns exemplos de quando mostrar ou ocultar os metadados incluem:

- Em um aplicativo de email, você pode exibir o avatar do usuário.
- Em um aplicativo de música, você pode exibir mais informações sobre um álbum ou artista.
- Em um aplicativo de vídeo, você pode exibir mais informações sobre um filme ou um programa, como mostrar detalhes do elenco e da equipe.
- Em qualquer aplicativo, você pode separar colunas e revelar mais detalhes.
- Em qualquer aplicativo, você pode pegar algo que esteja empilhado verticalmente e dispor horizontalmente. Ao passar do telefone ou phablet para dispositivos maiores, os itens de lista empilhados podem mudar para revelar as linhas de itens de lista e colunas de metadados.

## <a name="replace"></a>Substituir

Essa técnica permite alternar a interface do usuário de uma interrupção específica. Neste exemplo, o painel de navegação e seu CD, interface do usuário transitório funciona bem para uma tela menor, mas em uma tela maior, guias podem ser uma opção melhor.

![Substituindo elementos de design](images/rsp-design/rspd-replace.gif)

O [NavigationView](../controls-and-patterns/navigationview.md) controle dá suporte a essa técnica responsiva, permitindo que os usuários defina a posição do painel superior ou esquerda.

## <a name="re-architect"></a>Rearquitetura

Você pode recolher ou bifurcar a arquitetura do seu aplicativo para melhor direcionar dispositivos específicos. Neste exemplo, expandindo a janela mostra o padrão mestre/detalhes inteiro.

![um exemplo de rearquitetura de uma interface do usuário](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Tópicos relacionados

- [Tamanhos de tela e pontos de interrupção](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Layouts responsivos com XAML](layouts-with-xaml.md)
- [Controles da UWP e padrões](../controls-and-patterns/index.md)

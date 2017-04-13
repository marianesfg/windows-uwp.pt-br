---
author: mcleblanc
description: "Os aplicativos do Windows compartilham uma aparência comum em computadores, dispositivos móveis e muitos outros tipos de dispositivos. Os padrões da interface do usuário, de entrada e de interação são muito semelhantes, e um usuário que alterne entre dispositivos ficará contente com a experiência familiar."
title: "Portabilidade do Windows Phone Silverlight para UWP para fator forma e experiência do usuário"
ms.assetid: 96244516-dd2c-494d-ab5a-14b7dcd2edbd
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 23afad0c6499950d13d9771a5f0286403a5fe886
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
#  <a name="porting-windows-phone-silverlight-to-uwp-for-form-factor-and-ux"></a>Portabilidade do Windows Phone Silverlight para UWP para fator forma e experiência do usuário

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O tópico anterior era [Portando camadas de negócios e de dados](wpsl-to-uwp-business-and-data.md).

Os aplicativos do Windows compartilham uma aparência comum em computadores, dispositivos móveis e muitos outros tipos de dispositivos. Os padrões da interface do usuário, de entrada e de interação são muito semelhantes, e um usuário que alterne entre dispositivos ficará contente com a experiência familiar. Diferenças entre dispositivos, como tamanho físico, orientação padrão e resolução de pixels efetivos influenciam a forma como um aplicativo UWP (Plataforma Universal do Windows) é renderizado pelo Windows 10. A boa notícia é que grande parte do trabalho pesado é feito para você pelo sistema usando conceitos inteligentes como pixels efetivos.

## <a name="different-form-factors-and-user-experience"></a>Fatores forma diferentes e a experiência do usuário

Dispositivos diferentes têm várias resoluções retrato e paisagem possíveis, em uma variedade de taxas de proporção. Como serão dimensionados os aspectos visuais da interface, do texto e dos ativos de seu aplicativo UWP? Como você pode dar suporte ao toque, assim como à entrada de mouse e de teclado? E com um aplicativo que dê suporte a toque em dispositivos de tamanhos diferentes com distâncias de exibição diferentes, como um controle poderá ter um destino de toque do tamanho correto em diferentes densidades de pixel *e* ter seu conteúdo legível a distâncias diferentes? As seções a seguir tratam das coisas que você precisa saber.

## <a name="what-is-the-size-of-a-screen-really"></a>Qual é realmente o tamanho de uma tela?

A resposta resumida é que isso é subjetivo, já que depende não só do tamanho objetivo da tela como também da distância que você está dela. A subjetividade significa que precisamos nos colocar no lugar do usuário, e isso é algo que desenvolvedores de bons aplicativos fazem em todas as ocasiões.

De maneira objetiva, uma tela é medida em unidades de polegadas e pixels (brutos) físicos. Conhecer ambas as métricas permite calcular quantos pixels cabem em uma polegada. Essa é a densidade de pixels, também conhecida como DPI (pontos por polegada) ou também PPI (pixels por polegada). E a recíproca do DPI é o tamanho físico dos pixels como uma fração de uma polegada. A densidade de pixels também é conhecida como *resolução*, embora esse termo seja frequentemente usado de forma geral para significar contagem de pixels.

À medida que a distância de exibição aumenta, todas as métricas de objeto *parecem* menores, e são resolvidos para o *tamanho efetivo* da tela e para sua *resolução efetiva*. Em geral, o telefone é o que fica mais próximo aos olhos; em seguida, vêm o tablet e o monitor do computador, e os dispositivos [Surface Hub](http://www.microsoft.com/microsoft-surface-hub) e as TVs são os que ficam mais distantes. Para compensar, os dispositivos tendem a ficar objetivamente maiores conforme a distância de exibição. Quando você define tamanhos em elementos da interface do usuário, está definindo esses tamanhos em unidades chamadas pixels efetivos (epx). E o Windows 10 levará em conta a DPI, e a distância de exibição típica de um dispositivo, para calcular o melhor tamanho dos elementos da interface do usuário em pixels físicos para oferecer a melhor experiência de exibição. Consulte [Pixels de exibição/efetivos, distância de exibição e fatores de escala](wpsl-to-uwp-porting-xaml-and-ui.md).

Mesmo assim, recomendamos que você teste seu aplicativo com muitos dispositivos diferentes para que possa confirmar cada experiência.

## <a name="touch-resolution-and-viewing-resolution"></a>Resolução de toque e resolução de exibição

As funcionalidades (widgets da interface do usuário) precisam ter o tamanho correto para o toque. Dessa forma, um destino de toque precisa manter mais ou menos o tamanho físico em dispositivos diferentes que talvez tenham densidades de pixel diferentes. Os pixels efetivos também ajudam aqui: eles são dimensionados em dispositivos diferentes, levando a densidade de pixel em conta, para obter um tamanho físico mais ou menos constante que seja ideal para destinos de toque.

O texto precisa ter o tamanho correto para leitura (texto de 12 pontos a uma distância de exibição de 20 polegadas é uma boa regra prática), e uma imagem precisa ter o tamanho correto e a resolução efetiva para a distância de exibição. Em dispositivos diferentes, esse mesmo dimensionamento de pixels efetivos mantém os elementos da interface do usuário legíveis e do tamanho correto. Texto e outros elementos gráficos vetoriais são automaticamente dimensionados, e muito bem dimensionados. Os gráficos baseados em rasterização (bitmap) também são automaticamente dimensionados caso o desenvolvedor forneça um ativo em um único tamanho grande. Porém, é preferível para o desenvolvedor fornecer cada ativo em um intervalo de tamanhos, de maneira que o tamanho apropriado para o fator de dimensionamento do dispositivo de destino seja carregado automaticamente. Para obter mais informações sobre o assunto, consulte [Pixels de exibição/efetivos, distância de exibição e fatores de escala](wpsl-to-uwp-porting-xaml-and-ui.md).

## <a name="layout-and-adaptive-visual-state-manager"></a>Layout e Gerenciador de Estado Visual adaptável

Descrevemos os fatores envolvidos em um entendimento significativo do tamanho de tela. Agora pensemos no layout do aplicativo e em como usar o estado real de tela extra quando ela estiver disponível. Considere esta página de um aplicativo muito simples que foi projetado para ser executado em um dispositivo móvel estreito. Como deve ser a aparência desta página em uma tela maior?

![o aplicativo da Loja do Windows Phone portado](images/wpsl-to-uwp-case-studies/c01-04-uni-phone-app-ported.png)

A versão do dispositivo móvel está restrita à orientação somente retrato porque essa é a melhor taxa de proporção para a lista de livros, e faremos o mesmo para uma página de texto, que funciona melhor se mantida em uma única coluna em dispositivos móveis. Porém, as telas do computador e do tablet são grandes nas duas orientações e, portanto, essa restrição do dispositivo móvel parece uma limitação desnecessária em dispositivos maiores.

Aplicar zoom óptico no aplicativo para parecer com a versão do dispositivo móvel, apenas maior, não aproveita a vantagem do dispositivo e de seu espaço adicional, além de não ser adequado ao usuário. Nós devemos pensar em mostrar mais conteúdo, em vez de mostrarmos o mesmo conteúdo em formato maior. Mesmo em um phablet, nós poderíamos mostrar mais linhas de conteúdo. Nós poderíamos usar espaço extra para exibição de conteúdo diferente, como anúncios, ou poderíamos alterar a caixa de listagem para uma exibição de listagem para encapsular itens em várias colunas, onde possível, para usar o espaço dessa maneira. Consulte [Diretrizes de controles de exibição de grade e de lista](https://msdn.microsoft.com/library/windows/apps/mt186889).

Além dos novos controles, como a exibição de lista e a exibição de grade, a maioria dos tipos de layout estabelecidos do Windows Phone Silverlight tem equivalentes na UWP (Plataforma Universal do Windows). Por exemplo, [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) e [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635). A portabilidade de grande parte da interface do usuário que usa esses tipos deve ser direta, mas sempre procure formas de aproveitar os recursos de layout dinâmico desses painéis de layout para redimensionar e gerar um novo layout em dispositivos de tamanhos diferentes automaticamente.

Além do layout dinâmico integrado aos controles do sistema e painéis de layout, podemos usar um novo recurso do Windows 10 chamado [Gerenciador de Estado Visual Adaptável](wpsl-to-uwp-porting-xaml-and-ui.md).

## <a name="input-modalities"></a>Modalidades de entrada

Uma interface do Windows Phone Silverlight é específica ao toque. E, naturalmente, a interface do seu aplicativo portado também deve dar suporte ao toque, mas você pode optar por dar suporte a outras modalidades de entrada, como mouse e teclado. Na UWP, as entradas por toque, por caneta e por mouse são unificadas como *entrada do ponteiro*. Para obter mais informações, consulte [Identificar entrada do ponteiro](https://msdn.microsoft.com/library/windows/apps/mt404610) e [Interações por teclado](https://msdn.microsoft.com/library/windows/apps/mt185607).

## <a name="maximizing-markup-and-code-re-use"></a>Maximizando a reutilização de marcação e de código

Consulte novamente a lista [maximizando a reutilização de marcação e de código](wpsl-to-uwp-porting-to-a-uwp-project.md) para obter técnicas de compartilhamento da sua interface do usuário com dispositivos de destino com uma ampla variedade de fatores forma.

## <a name="more-info-and-design-guidelines"></a>Mais informações e diretrizes de design

-   [Criar aplicativos UWP](http://dev.windows.com/design)
-   [Diretrizes de fontes](https://msdn.microsoft.com/library/windows/apps/hh700394)
-   [Plano para fatores forma diferentes](https://msdn.microsoft.com/library/windows/apps/dn958435)

## <a name="related-topics"></a>Tópicos relacionados

* [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md)


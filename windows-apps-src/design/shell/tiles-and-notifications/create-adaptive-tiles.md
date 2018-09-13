---
author: andrewleader
Description: Adaptive tile templates are a new feature in Windows 10, allowing you to design your own tile notification content using a simple and flexible markup language that adapts to different screen densities.
title: Criar blocos adaptáveis
ms.assetid: 1246B58E-D6E3-48C7-AD7F-475D113600F9
label: Create adaptive tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 761d87654ef340f4b539dbefa0950c58f627d310
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "3963312"
---
# <a name="create-adaptive-tiles"></a>Criar blocos adaptáveis

Modelos de blocos adaptáveis são um novo recurso no Windows 10, permitindo que você elabore seu próprio conteúdo de notificação de bloco usando uma linguagem de marcação simples e flexível que se adapte a densidades de tela diferentes. Este artigo explica como criar blocos dinâmicos adaptáveis para seu aplicativo da Plataforma Universal do Windows (UWP). Para obter a lista completa de elementos e atributos adaptáveis, consulte o [Esquema de blocos adaptáveis](../tiles-and-notifications/tile-schema.md).

(Se quiser, você ainda pode usar os modelos predefinidos do [Catálogo de modelos de blocos do Windows8](https://msdn.microsoft.com/library/windows/apps/hh761491) durante a criação de notificações para o Windows10.)


## <a name="getting-started"></a>Introdução

**Instale a biblioteca de notificações.** Se você quiser usar a linguagem C# em vez de XML para gerar notificações, instale o pacote NuGet denominado [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (procure por "notificações uwp"). Os exemplos em linguagem C# fornecidos neste artigo usam o pacote NuGet, versão 1.0.0.

**Instale o Visualizador de Notificações.** Esse aplicativo UWP gratuito ajuda você a criar blocos dinâmicos adaptáveis, fornecendo uma visualização instantânea de seu bloco enquanto você o edita semelhante ao modo de exibição de editor/design XAML do Visual Studio. Consulte [Visualizador de notificações](notifications-visualizer.md) para obter mais informações ou [Baixar o Visualizador de notificações da Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="how-to-send-a-tile-notification"></a>Como enviar uma notificação de bloco

Leia nosso [guia de início rápido sobre como enviar notificações de bloco local](sending-a-local-tile-notification.md). A documentação abaixo explica todas as possibilidades visuais da interface do usuário que você tem com os blocos adaptáveis.


## <a name="usage-guidance"></a>Diretriz de uso


Os modelos adaptáveis são projetados para funcionar em diferentes fatores forma e tipos de notificação. Elementos como grupo e subgrupo vinculam conteúdo e não indicam um determinado comportamento visual próprio. A aparência final de uma notificação deve ser baseada no dispositivo específico no qual ela aparecerá, seja um telefone, tablet, desktop ou outro dispositivo.

Dicas são atributos opcionais que podem ser adicionados aos elementos para obter um comportamento visual específico. As dicas podem ser específicas ao dispositivo ou à notificação.

## <a name="a-basic-example"></a>Um exemplo básico


Este exemplo demonstra o que os modelos de blocos adaptáveis podem produzir.

```xml
<tile>
  <visual>
  
    <binding template="TileMedium">
      ...
    </binding>
  
    <binding template="TileWide">
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </binding>
  
    <binding template="TileLarge">
      ...
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = ...
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Jennifer Parker",
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Photos from our trip",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Check out these awesome photos I took while in New Zealand!",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },
  
        TileLarge = ...
    }
};
```

**Resultado:**

![bloco de exemplo rápido](images/adaptive-tiles-quicksample.png)

## <a name="tile-sizes"></a>Tamanhos de bloco


O conteúdo de cada tamanho de bloco é especificado individualmente em elementos [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) separados com o conteúdo XML. Escolha o tamanho de destino definindo o atributo template com um dos seguintes valores:

-   TileSmall
-   TileMedium
-   TileWide
-   TileLarge (somente para desktop)

Para o conteúdo XML de notificação de um único bloco, forneça elementos &lt;binding&gt; para cada tamanho de bloco para o qual você gostaria de oferecer suporte, conforme mostrado neste exemplo:

```xml
<tile>
  <visual>
  
    <binding template="TileSmall">
      <text>Small</text>
    </binding>
  
    <binding template="TileMedium">
      <text>Medium</text>
    </binding>
  
    <binding template="TileWide">
      <text>Wide</text>
    </binding>
  
    <binding template="TileLarge">
      <text>Large</text>
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileSmall = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Small" }
                }
            }
        },
  
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Medium" }
                }
            }
        },
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Wide" }
                }
            }
        },
  
        TileLarge = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Large" }
                }
            }
        }
    }
};
```

**Resultado:**

![tamanhos dos blocos adaptáveis: pequeno, médio, largo e grande](images/adaptive-tiles-sizes.png)

## <a name="branding"></a>Identidade visual


Você pode controlar a identidade visual na parte inferior de um bloco dinâmico (o nome de exibição e o logotipo de canto) usando o atributo branding no conteúdo da notificação. Você pode optar por exibir "none", somente "name", somente "logo", ou ambos, com "nameAndLogo".

**Observação**  O Windows Mobile não aceita o logotipo de canto, então "logo" e "nameAndLogo" são definidos como o padrão "name" no Mobile.

 

```xml
<visual branding="logo">
  ...
</visual>
```

```csharp
new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}
```

**Resultado:**

![blocos adaptáveis, com elementos name e logo](images/adaptive-tiles-namelogo.png)

A identidade visual pode ser aplicada para tamanhos de bloco específicos de uma destas maneiras:

1. Aplicando o atributo no elemento [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding)
2. Na aplicação do atributo ao elemento [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual), que afeta a carga de notificação inteira Se você não especificar a identidade visual para uma associação, ele usará a identidade visual fornecida no elemento visual.

```xml
<tile>
  <visual branding="nameAndLogo">
 
    <binding template="TileMedium" branding="logo">
      ...
    </binding>
 
    <!--Inherits branding from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,

        TileMedium = new TileBinding()
        {
            Branding = TileBranding.Logo,
            ...
        },

        // Inherits branding from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Resultado de identidade visual padrão:**

![identidade visual padrão em blocos](images/adaptive-tiles-defaultbranding.png)

Se você não especificar a identidade visual no conteúdo da notificação, as propriedades do bloco base determinarão a identidade visual. Se o bloco base mostrar o nome de exibição, a identidade visual padrão será "name". Caso contrário, a identidade visual padrão será "none" se o nome de exibição não for mostrado.

**Observação**   Isso é uma mudança do Windows 8.x, no qual a identidade visual padrão era "logo".

 

## <a name="display-name"></a>Nome de exibição


Você pode substituir o nome de exibição de uma notificação digitando a cadeia de caracteres de texto de sua escolha com o atributo **displayName**. Assim como na identidade visual, você pode especificar isso no elemento [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual), que afeta todo o conteúdo da notificação, ou no elemento [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding), que afeta somente blocos individuais.

**Problema conhecido**  No Windows Mobile, se você especificar um ShortName para Tile, o nome de exibição fornecido na notificação não será usado (o ShortName sempre será exibido). 

```xml
<tile>
  <visual branding="nameAndLogo" displayName="Wednesday 22">
 
    <binding template="TileMedium" displayName="Wed. 22">
      ...
    </binding>
 
    <!--Inherits displayName from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
        DisplayName = "Wednesday 22",

        TileMedium = new TileBinding()
        {
            DisplayName = "Wed. 22",
            ...
        },

        // Inherits DisplayName from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Resultado:**

![nome de exibição de blocos adaptáveis](images/adaptive-tiles-displayname.png)

## <a name="text"></a>Texto


O elemento [AdaptiveText](../tiles-and-notifications/tile-schema.md#adaptivetext) é usado para exibir texto. Você pode usar dicas para modificar a aparência do texto.

```xml
<text>This is a line of text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of text"
};
```

**Resultado:**

![texto do bloco adaptável](images/adaptive-tiles-text.png)

## <a name="text-wrapping"></a>Disposição do texto


Por padrão, o texto não é encapsulado e ultrapassa a borda do bloco. Use o atributo **hint-wrap** para definir a disposição do texto em um elemento de texto. Você também pode controlar o número mínimo e máximo de linhas usando **hint-minLines** e **hint-maxLines**, que aceitam números inteiros positivos.

```xml
<text hint-wrap="true">This is a line of wrapping text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of wrapping text",
    HintWrap = true
};
```

**Resultado:**

![bloco adaptável com disposição do texto](images/adaptive-tiles-textwrapping.png)

## <a name="text-styles"></a>Estilos de texto


Os estilos controlam o tamanho, a cor e a espessura da fonte dos elementos de texto. Há uma série de estilos disponíveis, incluindo uma variação "sutil" de cada estilo que define a opacidade em 60%, que geralmente torna a cor do texto um tom de cinza-claro.

```xml
<text hint-style="base">Header content</text>
<text hint-style="captionSubtle">Subheader content</text>
```

```csharp
new AdaptiveText()
{
    Text = "Header content",
    HintStyle = AdaptiveTextStyle.Base
},

new AdaptiveText()
{
    Text = "Subheader content",
    HintStyle = AdaptiveTextStyle.CaptionSubtle
}
```

**Resultado:**

![estilos de texto de blocos adaptáveis](images/adaptive-tiles-textstyles.png)

**Observação**  O estilo padrão é caption quando hint-style é especificado.

 

**Estilos de texto básicos**

|                                |                           |             |
|--------------------------------|---------------------------|-------------|
| &lt;text hint-style="\*" /&gt; | Altura da fonte               | Espessura da fonte |
| caption                        | 12 pixels efetivos (epx) | Regular     |
| body                           | 15 epx                    | Regular     |
| base                           | 15 epx                    | Semibold    |
| subtitle                       | 20 epx                    | Regular     |
| title                          | 24 epx                    | Semilight   |
| subheader                      | 34 epx                    | Light       |
| header                         | 46 epx                    | Light       |

 

**Variações numéricas de estilo de texto**

Essas variações reduzem a altura da linha para que o conteúdo acima e abaixo se aproxime bem do texto.

|                  |
|------------------|
| titleNumeral     |
| subheaderNumeral |
| headerNumeral    |

 

**Variações sutis de estilo de texto**

Cada estilo tem uma variação sutil que oferece ao texto uma opacidade de 60%, o que geralmente torna a cor do texto um tom de cinza-claro.

|                        |
|------------------------|
| captionSubtle          |
| bodySubtle             |
| baseSubtle             |
| subtitleSubtle         |
| titleSubtle            |
| titleNumeralSubtle     |
| subheaderSubtle        |
| subheaderNumeralSubtle |
| headerSubtle           |
| headerNumeralSubtle    |

 

## <a name="text-alignment"></a>Alinhamento do texto


O texto pode ser alinhamento horizontalmente à esquerda, ao centro ou à direita. Em idiomas da esquerda para a direita como o inglês, o padrão de texto é alinhado à esquerda. Em idiomas da direita para a esquerda como o árabe, o padrão de texto é alinhado à direita. Você pode definir manualmente o alinhamento com o atributo **hint-align** nos elementos.

```xml
<text hint-align="center">Hello</text>
```


```csharp
new AdaptiveText()
{
    Text = "Hello",
    HintAlign = AdaptiveTextAlign.Center
};
```

**Resultado:**

![alinhamento de texto de blocos adaptáveis](images/adaptive-tiles-textalignment.png)

## <a name="groups-and-subgroups"></a>Grupos e subgrupos


Os grupos permitem declarar semanticamente que o conteúdo dentro do grupo está relacionado e deve ser exibido em sua totalidade para o conteúdo fazer sentido. Por exemplo, você pode ter dois elementos de texto, um cabeçalho e um subcabeçalho, e não faz sentido que apenas o cabeçalho seja mostrado. Agrupando esses elementos dentro de um subgrupo, todos os elementos serão exibidos (se couberem) ou nenhum elemento será exibido (porque não cabem).

Para proporcionar a melhor experiência em dispositivos e telas, forneça vários grupos. Ter vários grupos permite que seu bloco se adapte a telas maiores.

**Observação**  O único filho válido de um grupo é somente um subgrupo.

 

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup>
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </subgroup>
  </group>
 
  <text />
 
  <group>
    <subgroup>
      <text hint-style="subtitle">Steve Bosniak</text>
      <text hint-style="captionSubtle">Build 2015 Dinner</text>
      <text hint-style="captionSubtle">Want to go out for dinner after Build tonight?</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            CreateGroup(
                from: "Jennifer Parker",
                subject: "Photos from our trip",
                body: "Check out these awesome photos I took while in New Zealand!"),

            // For spacing
            new AdaptiveText(),

            CreateGroup(
                from: "Steve Bosniak",
                subject: "Build 2015 Dinner",
                body: "Want to go out for dinner after Build tonight?")
        }
    }
}

...

private static AdaptiveGroup CreateGroup(string from, string subject, string body)
{
    return new AdaptiveGroup()
    {
        Children =
        {
            new AdaptiveSubgroup()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    };
}
```

**Resultado:**

![subgrupos e grupos de blocos adaptáveis](images/adaptive-tiles-groups-subgroups.png)

## <a name="subgroups-columns"></a>Subgrupos (colunas)


Os subgrupos também permitem que você divida dados em seções semânticas dentro de um grupo. Para blocos dinâmicos, isso se traduz visualmente em colunas.

O atributo **hint-weight** permite que você controle as larguras das colunas. O valor de **hint-weight** é expresso como uma proporção ponderada de espaço disponível, o que é idêntico ao comportamento de **GridUnitType.Star**. Para colunas de mesma largura, atribua cada peso a 1.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Porcentagem de largura</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">Espessura total: 4</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![subgrupos, colunas uniformes](images/adaptive-tiles-subgroups01.png)

Para tornar uma coluna duas vezes maior que outra coluna, atribua à coluna menor uma espessura de 1 e à coluna maior uma espessura de 2.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Porcentagem de largura</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">33,3%</td>
</tr>
<tr class="odd">
<td align="left">2</td>
<td align="left">66,7%</td>
</tr>
<tr class="even">
<td align="left">Espessura total: 3</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![subgrupos, uma coluna duas vezes o tamanho da outra](images/adaptive-tiles-subgroups02.png)

Se quiser que sua primeira coluna ocupe 20% da largura total e sua segunda coluna ocupe 80% da largura total, atribua a espessura da primeira a 20 e a segunda espessura a 80. Se o total de espessuras for igual a 100, os valores atuarão como porcentagens.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Porcentagem de largura</td>
</tr>
<tr class="even">
<td align="left">20</td>
<td align="left">20%</td>
</tr>
<tr class="odd">
<td align="left">80</td>
<td align="left">80%</td>
</tr>
<tr class="even">
<td align="left">Espessura total: 100</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![subgrupos, com espessuras totalizando 100](images/adaptive-tiles-subgroups03.png)

**Observação**  Uma margem de 8 pixels é automaticamente adicionada entre as colunas.

 

Quando houver mais de dois subgrupos, você deverá especificar **hint-weight**, que aceita somente números inteiros positivos. Se você não especificar hint-weight para o primeiro subgrupo, ele será atribuído a uma espessura de 50. O próximo subgrupo que não tiver um atributo hint-weight especificado será atribuído a uma espessura igual a 100 menos a soma das espessuras anteriores, ou a 1 se o resultado for zero. Os subgrupos restantes que não tiverem atributos hint-weights especificados serão atribuídos a uma espessura de 1.

Este é um código de exemplo para um bloco de previsão do tempo que mostra como você pode obter um bloco com cinco colunas de mesma largura:

```xml
<binding template="TileWide" displayName="Seattle" branding="name">
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Tue</text>
      <image src="Assets\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-align="center" hint-style="captionsubtle">38°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Wed</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">59°</text>
      <text hint-align="center" hint-style="captionsubtle">43°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Thu</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">62°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Fri</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">71°</text>
      <text hint-align="center" hint-style="captionsubtle">66°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°"),
                    CreateSubgroup("Wed", "Sunny.png", "59°", "43°"),
                    CreateSubgroup("Thu", "Sunny.png", "62°", "42°"),
                    CreateSubgroup("Fri", "Sunny.png", "71°", "66°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Resultado:**

![exemplo de um bloco de previsão do tempo](images/adaptive-tiles-weathertile.png)

## <a name="images"></a>Imagens


O elemento &lt;image&gt; é usado para exibir imagens na notificação do bloco. As imagens podem ser embutidas no conteúdo do bloco (padrão), como uma imagem de plano de fundo atrás do conteúdo, ou como uma imagem de "espiar" animada na parte superior da notificação.

> [!NOTE]
> As imagens podem ser usadas do pacote do aplicativo, do armazenamento local do aplicativo ou da Web. Na Fall Creators Update, as imagens da Web podem ter até 3 MB em conexões normais e 1 MB em conexões limitadas. Em dispositivos que ainda não executam a Fall Creators Update, as imagens da Web devem ser maiores do que 200 KB.

 

Com nenhum comportamento extra especificado, as imagens se encolherão ou expandirão uniformemente para preencher a largura disponível. Esse exemplo mostra um bloco com duas colunas e imagens embutidas. As imagens embutidas se ampliam para preencher a largura da coluna.

```xml
<binding template="TileMedium" displayName="Seattle" branding="name">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Apps\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Apps\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionSubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°")
                }
            }
        }
    }
}
...
private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Resultado:**

![exemplo de imagem](images/adaptive-tiles-images01.png)

As imagens posicionadas na raiz de &lt;binding&gt;, ou no primeiro grupo, também se ampliarão para caber na altura disponível.

### <a name="image-alignment"></a>Alinhamento das imagens

As imagens podem ser definidas para alinhamento à esquerda, ao centro ou à direita com o atributo **hint-align**. Isso também fará com que as imagens sejam exibidas em sua resolução nativa, em vez de ampliadas para preencher a largura.

```xml
<binding template="TileLarge">
  <image src="Assets/fable.jpg" hint-align="center"/>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveImage()
            {
                Source = "Assets/fable.jpg",
                HintAlign = AdaptiveImageAlign.Center
            }
        }
    }
}
```

**Resultado:**

![exemplo de alinhamento de imagem (à esquerda, ao centro, à direita)](images/adaptive-tiles-imagealignment.png)

### <a name="image-margins"></a>Margens de imagem

Por padrão, as imagens embutidas têm uma margem de 8 pixels entre qualquer conteúdo acima ou abaixo da imagem. Essa margem pode ser removida usando o atributo **hint-removeMargin** na imagem. Entretanto, as imagens sempre retêm a margem de 8 pixels da borda do bloco, e os subgrupos (colunas) sempre retêm o preenchimento de 8 pixels entre colunas.

```xml
<binding template="TileMedium" branding="none">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Numbers\4.jpg" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Numbers\3.jpg" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionsubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.None,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "4.jpg", "63°", "42°"),
                    CreateSubgroup("Tue", "3.jpg", "57°", "38°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Numbers/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

![exemplo de dica de remoção de margem](images/adaptive-tiles-removemargin.png)

### <a name="image-cropping"></a>Corte de imagem

As imagens podem ser cortadas em círculo usando o atributo **hint-crop**, que atualmente aceita apenas os valores de "none" (padrão) ou "circle".

```xml
<binding template="TileLarge" hint-textStacking="center">
  <group>
    <subgroup hint-weight="1"/>
    <subgroup hint-weight="2">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-weight="1"/>
  </group>
 
  <text hint-style="title" hint-align="center">Hi,</text>
  <text hint-style="subtitleSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    new AdaptiveSubgroup() { HintWeight = 1 },
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 2,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },
                    new AdaptiveSubgroup() { HintWeight = 1 }
                }
            },
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Title,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.SubtitleSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

**Resultado:**

![exemplo de corte de imagem](images/adaptive-tiles-imagecropping.png)

### <a name="background-image"></a>Imagem de plano de fundo

Para definir uma imagem de plano de fundo, coloque um elemento de imagem na raiz de &lt;binding&gt; e defina o atributo de posicionamento como "background".

```xml
<binding template="TileWide">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    ...
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg"
        },

        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°")
                    ...
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Resultado:**

![exemplo de imagem de plano de fundo](images/adaptive-tiles-backgroundimage.png)

### <a name="peek-image"></a>Imagem de "espiar"

Você pode especificar uma imagem que "espie" a parte superior do bloco. A imagem de "espiar" usa uma animação para deslizar para baixo/para cima a partir da parte superior do bloco, espiando a visualização, e depois deslizar de volta para revelar o conteúdo principal no bloco. Para definir uma imagem de "espiar", coloque um elemento de imagem na raiz de &lt;binding&gt; e defina o atributo de posicionamento como "peek".

```xml
<binding template="TileMedium" branding="name">
  <image placement="peek" src="Assets/Apps/Hipstame/hipster.jpg"/>
  <text>New Message</text>
  <text hint-style="captionsubtle" hint-wrap="true">Hey, have you tried Windows 10 yet?</text>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Apps/Hipstame/hipster.jpg"
        },
        Children =
        {
            new AdaptiveText()
            {
                Text = "New Message"
            },
            new AdaptiveText()
            {
                Text = "Hey, have you tried Windows 10 yet?",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintWrap = true
            }
        }
    }
}
```

![exemplos de imagens de "espiar"](images/adaptive-tiles-imagepeeking.png)

**Corte em círculo para imagens de "espiar" e de plano de fundo**

Use o seguinte atributo hint-crop em imagens de "espiar" e de plano de fundo para fazer um corte em círculo:

```xml
<image placement="peek" hint-crop="circle" src="Assets/Apps/Hipstame/hipster.jpg"/>
```

```csharp
new TilePeekImage()
{
    HintCrop = TilePeekImageCrop.Circle,
    Source = "Assets/Apps/Hipstame/hipster.jpg"
}
```

O resultado ficará assim:

![corte em círculo para imagens de "espiar" e de plano de fundo](images/circlecrop-image.png)

**Usar imagem dinâmica e de plano de fundo**

Para usar uma imagem de "espiar" e de plano de fundo em uma notificação de bloco, especifique uma imagem de "espiar" e uma imagem de plano de fundo em sua carga de notificação.

O resultado ficará assim:

![imagem de "espiar" e imagem de plano de fundo usadas juntas](images/peekandbackground.png)


### <a name="peek-and-background-image-overlays"></a>As imagens de "espiar" e de plano de fundo substituem

Você pode definir uma sobreposição preta em suas imagens de "espiar" e de plano de fundo usando **hint-overlay**, que aceita inteiros de 0 a 100, sendo 0 nenhuma sobreposição e 100 uma sobreposição total preta. Você pode usar a sobreposição para assegurar que o texto em seu bloco seja legível.

**Use hint-overlay em uma imagem de plano de fundo**

Sua imagem de plano de fundo será padronizada com uma sobreposição de 20% desde que você tenha alguns elementos de texto em sua carga (caso contrário, o padrão será 0% sobreposição).

```xml
<binding template="TileWide">
  <image placement="background" hint-overlay="60" src="Assets\Mostly Cloudy-Background.jpg"/>
  ...
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg",
            HintOverlay = 60
        },

        ...
    }
}
```

**Resultado de hint-overlay:**

![exemplo de uma sobreposição de dica de imagem](images/adaptive-tiles-image-hintoverlay.png)

**Use hint-overlay em uma imagem de "espiar"**

Na versão 1511 do Windows 10, também oferecemos suporte a uma sobreposição para sua imagem de "espiar", da mesma forma que fizemos para a imagem de plano de fundo. Especifique hint-overlay no elemento de imagem de "espiar", como um número inteiro de 0 a 100. A sobreposição padrão para imagens de "espiar" é 0 (nenhuma sobreposição).

```xml
<binding template="TileMedium">
  <image hint-overlay="20" src="Assets\Map.jpg" placement="peek"/>
  ...
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Map.jpg",
            HintOverlay = 20
        },
        ...
    }
}
```

Este exemplo mostra uma imagem de "espiar" com opacidade de 20% (esquerda) e opacidade de 0% (direita):

![hint-overlay em uma imagem de "espiar"](images/hintoverlay.png)

## <a name="vertical-alignment-text-stacking"></a>Alinhamento vertical (empilhamento de texto)


Você pode controlar o alinhamento vertical do conteúdo em seu bloco usando o atributo **hint-textStacking** nos elementos [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) e [AdaptiveSubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup). Por padrão, tudo é alinhado verticalmente à parte superior, mas você também pode alinhar o conteúdo à parte inferior ou ao centro.

### <a name="text-stacking-on-binding-element"></a>Empilhamento de texto no elemento binding

Quando aplicado no nível [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding), o empilhamento de texto define o alinhamento vertical do conteúdo da notificação como um todo, alinhando no espaço vertical disponível acima da área de identidade visual/notificação.

```xml
<binding template="TileMedium" hint-textStacking="center" branding="logo">
  <text hint-style="base" hint-align="center">Hi,</text>
  <text hint-style="captionSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.Logo,
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Base,
                HintAlign = AdaptiveTextAlign.Center
            },

            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

![empilhamento de texto no elemento binding](images/adaptive-tiles-textstack-bindingelement.png)

### <a name="text-stacking-on-subgroup-element"></a>Empilhamento de texto no elemento subgroup

Quando aplicado no nível [AdaptiveSubgroup;](../tiles-and-notifications/tile-schema.md#adaptivesubgroup), o empilhamento de texto define o alinhamento vertical do conteúdo do subgrupo (coluna), alinhando no espaço vertical disponível dentro do grupo inteiro.

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup hint-weight="33">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-textStacking="center">
      <text hint-style="subtitle">Hi,</text>
      <text hint-style="bodySubtle">MasterHip</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    // Image column
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 33,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },

                    // Text column
                    new AdaptiveSubgroup()
                    {
                        // Vertical align its contents
                        TextStacking = TileTextStacking.Center,
                        Children =
                        {
                            new AdaptiveText()
                            {
                                Text = "Hi,",
                                HintStyle = AdaptiveTextStyle.Subtitle
                            },

                            new AdaptiveText()
                            {
                                Text = "MasterHip",
                                HintStyle = AdaptiveTextStyle.BodySubtle
                            }
                        }
                    }
                }
            }
        }
    }
}
```

## <a name="related-topics"></a>Tópicos relacionados
* [Esquema de conteúdo do bloco](../tiles-and-notifications/tile-schema.md)
* [Enviar uma notificação de bloco local](sending-a-local-tile-notification.md)
* [Modelos de blocos especiais](special-tile-templates-catalog.md)
* [Kit de ferramentas de comunidade UWP - Notificações](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Notificações do Windows no GitHub](https://github.com/WindowsNotifications)

 

 





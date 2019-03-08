---
description: Fornece um valor para qualquer atributo XAML avaliando uma referência a um recurso, com lógica do sistema adicional que recupera diferentes recursos, dependendo do tema ativo no momento.
title: Extensão de marcação ThemeResource
ms.assetid: 8A1C79D2-9566-44AA-B8E1-CC7ADAD1BCC5
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9466ec598fad090e31768d680b64ffea52688844
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661141"
---
# <a name="themeresource-markup-extension"></a>Extensão de marcação {ThemeResource}

Fornece um valor para qualquer atributo XAML avaliando uma referência a um recurso, com lógica do sistema adicional que recupera diferentes recursos, dependendo do tema ativo no momento. Assim como a [extensão de marcação {StaticResource}](staticresource-markup-extension.md), os recursos são definidos em um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) e o uso de **ThemeResource** faz referência à chave desse recurso no **ResourceDictionary**.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object property="{ThemeResource key}" .../>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| key | A chave para o recurso solicitado. Essa chave é inicialmente atribuída pelo [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794). Uma chave de recurso pode ser qualquer cadeia de caracteres definida na gramática de XamlName. |

## <a name="remarks"></a>Comentários

A **ThemeResource** é uma técnica para obtenção de valores referentes a um atributo XAML definidos em outro lugar em um dicionário de recursos XAML. Essa extensão de marcação tem a mesma finalidade básica que a [extensão de marcação StaticResource](staticresource-markup-extension.md). A diferença de comportamento em relação à extensão de marcação {StaticResource} é que uma referência **ThemeResource** pode usar dinamicamente diferentes dicionários como o local de pesquisa principal, dependendo do tema atualmente utilizado pelo sistema.

Quando o aplicativo é iniciado pela primeira vez, qualquer referência de recurso feita por uma referência **ThemeResource** é avaliada com base no tema em uso durante a inicialização. Mas se o usuário mudar posteriormente o tema ativo no tempo de execução, o sistema reavaliará cada referência **ThemeResource**, recuperará um recurso específico do tema que pode ser diferente e exibirá novamente o aplicativo com os novos valores de recurso em todos os locais apropriados da árvore visual. Uma **StaticResource** é determinada no tempo de carregamento do XAML/inicialização do aplicativo e não será reavaliada no tempo de execução. (Há outras técnicas como estados visuais que recarregam o XAML dinamicamente, mas essas técnicas operam em um nível mais elevado do que a avaliação de recurso básica ativada pela [extensão de marcação {StaticResource}](staticresource-markup-extension.md)).

**ThemeResource** obtém um argumento, que especifica a chave para o recurso solicitado. Uma chave de recurso sempre é uma cadeia de caracteres no XAML de Tempo de Execução do Windows. Para obter mais informações sobre como a chave de recurso é especificada inicialmente, consulte [atributo x:Key](x-key-attribute.md).

Para obter mais informações sobre como definir recursos e como usar adequadamente um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794), incluindo o exemplo de código, consulte [Referências aos recursos ResourceDictionary e XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).

**Importante** Assim como no caso de **StaticResource**, um **ThemeResource** não deve tentar fazer uma referência posterior a um recurso que é definido lexicalmente mais fundo no arquivo XAML. Não é possível fazer isso. Mesmo se a referência de encaminhamento não falhar, tentar fazê-la acarreta uma penalidade de desempenho. Para obter melhores resultados, ajuste a composição dos seus dicionários de recursos de maneira que seja possível evitar referências de encaminhamento.

Tentar especificar um **ThemeResource** para uma chave incapaz de resolver gera uma exceção de análise de XAML em tempo de execução. As ferramentas de design também podem apresentar avisos ou erros.

Na implementação do processador XAML do Windows Runtime, não há uma representação de classe de suporte para a funcionalidade **ThemeResource**. O equivalente mais próximo em código é usar a API de coleção de um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794), chamando, por exemplo, [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) ou [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139).

**ThemeResource** é uma extensão de marcação. As extensões de marcação geralmente são implementadas quando é necessário efetuar um escape de valores de atributo para que sejam diferentes de valores literais ou nomes de manipulador e o requisito é mais global do que simplesmente colocar conversores de tipo em certos tipos ou propriedades. Todas as extensões de marcação em XAML usam os caracteres "{" e "}" na sintaxe de atributo, sendo esta a convenção pela qual um processador XAML reconhece que uma extensão de marcação deve processar o atributo.

### <a name="when-and-how-to-use-themeresource-rather-than-staticresource"></a>Quando e como usar {ThemeResource} em vez de {StaticResource}

As regras pelas quais **ThemeResource** é resolvido para um item em um dicionário de recursos costumam ser as mesmas de **StaticResource**. Uma pesquisa de **ThemeResource** pode ser estendida para os arquivos do [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) que são referenciados em uma coleção de [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208807), mas um **StaticResource** também pode fazer isso. A diferença é que um **ThemeResource** pode ser reavaliado no tempo de execução e um **StaticResource** não.

O conjunto de chaves em cada dicionário de temas deve fornecer o mesmo conjunto de recursos de chave, independentemente do tema ativo. Se um determinado recurso de chave existir no dicionário de temas **HighContrast**, outro recurso com esse nome também deverá existir em **Light** e **Default**. Se isso não for verdadeiro, a pesquisa de recurso poderá falhar quando o usuário alternar entre os temas e o seu aplicativo não parecerá correto. Entretanto, é possível que um dicionário de temas contenha recursos de chave que sejam referenciados apenas no mesmo escopo para fornecer subvalores; eles não precisam ser equivalentes em todos os temas.

Em geral, você deve colocar os recursos nos mesmos dicionários de temas e fazer referências a esses recursos usando **ThemeResource** somente quando esses valores puderem mudar entre os temas ou forem compatíveis com valores que mudam. Isso é apropriado para estes tipos de recurso:

-   Pincéis, em determinadas cores para [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962). Eles compõem cerca de 80% dos usos de **ThemeResource** nos modelos de controle XAML padrão (generic.xaml).
-   Valores de pixel para bordas, deslocamentos, margens e preenchimento, etc.
-   Propriedades de fonte como **FontFamily** ou **FontSize**.
-   Modelos completos para um número limitado de controles que costumam ter o estilo do sistema e ser usados para apresentação dinâmica, como [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) e [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919).
-   Estilos de exibição de texto (geralmente para mudar a cor da fonte, a tela de fundo e possivelmente o tamanho).

O Windows Runtime fornece um conjunto de recursos especialmente concebido para ser referenciado por **ThemeResource**. Todos esses recursos estão listados como parte do arquivo XAML themeresources.xaml, que está disponível na pasta include/winrt/xaml/design integrante do Software Development Kit do Windows (SDK do Windows). Para conhecer a documentação sobre pincéis de temas e estilos adicionais que estão definidos em themeresources.xaml, consulte [Recursos de temas XAML](https://msdn.microsoft.com/library/windows/apps/mt187274). Os pincéis estão documentados em uma tabela que informa o valor de cor de cada pincel em cada um dos três temas ativos possíveis.

As definições XAML de estados visuais em um modelo de controle devem usar referências **ThemeResource** sempre que houver um recurso subjacente que possa mudar devido a uma alteração de tema. Tipicamente, uma alteração de tema do sistema não causa uma alteração de estado visual. Os recursos precisam usar referências **ThemeResource** nesse caso, para que os valores possam ser reavaliados para o estado visual ainda ativo. Por exemplo, se você tiver um estado visual que muda a cor de um pincel de uma determinada interface do usuário e uma de suas propriedades, e a cor desse pincel for diferente em cada tema, você deverá usar uma referência **ThemeResource** para fornecer o valor dessa propriedade no modelo padrão e também qualquer modificação de estado visual para esse modelo padrão.

O uso de **ThemeResource** pode ser visto em uma série de valores dependentes. Por exemplo, um valor [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) usado por um [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) que também é um recurso de chave pode usar uma referência **ThemeResource**. Mas quaisquer propriedades de interface do usuário que usem o recurso de chave **SolidColorBrush** também deverão usar uma referência **ThemeResource**, para que cada propriedade de tipo de [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) que esteja ativando um valor dinâmico mude quando o tema mudar.

**Observação**   `{ThemeResource}` e avaliação de recurso de tempo de execução na alternância de tema é aceito no Windows 8.1 XAML, mas não tem suporte no XAML para aplicativos destinados ao Windows 8.

### <a name="system-resources"></a>Recursos do sistema

Alguns recursos do sistema fazem referência a valores de recursos do sistema como um subvalor adjacente. Um recurso do sistema é um valor de recurso especial que não pode ser encontrado em nenhum dicionário de recursos XAML. Esses valores se baseiam no comportamento de suporte ao XAML do Windows Runtime para encaminhar os valores do próprio sistema e representá-los de uma forma que um recurso XAML possa fazer referência. Por exemplo, há um recurso do sistema denominado "SystemColorButtonFaceColor" que representa uma cor RGB. Essa cor é proveniente de aspectos de cores e temas do sistema que não são simplesmente específicos do Windows Runtime e de seus respectivos aplicativos.

Os recursos do sistema costumam ser valores subjacentes para um tema em alto contraste. O usuário controla as opções de cores do tema em alto contraste e o usuário faz essas escolhas usando recursos do sistema que também não são específicos dos aplicativos do Windows Runtime. Quando é feita referência aos recursos do sistema como referências **ThemeResource**, o comportamento padrão dos temas em alto contraste dos aplicativos do Windows Runtime pode usar esses valores específicos do tema que são controlados pelo usuário e apresentados pelo sistema. Além disso, as referências agora são marcadas para reavaliação quando o sistema detecta uma alteração de tema de tempo de execução.

### <a name="an-example-themeresource-usage"></a>Exemplo de uso de {ThemeResource}

Veja a seguir alguns exemplos de XAML extraídos dos arquivos padrão generic.xaml e themeresources.xaml para ilustrar como usar **ThemeResource**. Vamos analisar apenas um modelo (o [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) padrão) e como duas propriedades são declaradas ([**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) e [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414)) para serem responsivas às alterações de temas.

```xml
    <!-- Default style for Windows.UI.Xaml.Controls.Button -->
    <Style TargetType="Button">
        <Setter Property="Background" Value="{ThemeResource ButtonBackgroundThemeBrush}" />
        <Setter Property="Foreground" Value="{ThemeResource ButtonForegroundThemeBrush}"/>
...
```

Aqui, as propriedades assumem um valor [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) e a referência aos recursos [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) nomeadas `ButtonBackgroundThemeBrush` e `ButtonForegroundThemeBrush` são feitas usando **ThemeResource**.

Essas mesmas propriedades também são ajustadas por alguns dos estados visuais para um [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265). Notavelmente, a cor da tela de fundo muda quando um botão é clicado. Aqui também, as animações de [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) e [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) no storyboard de estados visuais usam objetos [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132) e referências a pincéis com **ThemeResource** como o valor chave.

```xml
<VisualState x:Name="Pressed">
  <Storyboard>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Border"
        Storyboard.TargetProperty="Background">
      <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedBackgroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter"
         Storyboard.TargetProperty="Foreground">
       <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedForegroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
  </Storyboard>
</VisualState>
```

Cada um desses pincéis é definido previamente em generic.xaml: eles precisam ser definidos antes de serem utilizados por qualquer modelo, para evitar referências posteriores de XAML. Aqui estão essas definições, para o dicionário de temas "Padrão".

```xml
    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="Transparent" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="#FFFFFFFF" />
...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="#FFFFFFFF" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="#FF000000" />
...
```

Depois, os pincéis de cada um dos outros dicionários de temas também são definidos, por exemplo:

```xml
        <ResourceDictionary x:Key="HighContrast">
            <!-- High Contrast theme resources -->
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />

...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
```

Aqui o valor [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) é outra referência **ThemeResource** a um recurso do sistema. Se você fizer referência a um recurso do sistema e quiser que ele mude em resposta a uma alteração de tema, você deverá usar **ThemeResource** para fazer a referência.

## <a name="windows8-behavior"></a>Comportamento do Windows 8

Windows 8 não oferecia suporte a **ThemeResource** extensão de marcação, está disponível a partir do Windows 8.1. Além disso, o Windows 8 não dava suporte alternar dinamicamente os recursos relacionados ao tema para um aplicativo de tempo de execução do Windows. O aplicativo tinha que ser reiniciado para selecionar a alteração de tema dos modelos e estilos XAML. Isso não é uma boa experiência do usuário, portanto, os aplicativos são altamente incentivados a recompilação e de destino do Windows 8.1 para que eles possam usar estilos com **ThemeResource** usos e dinamicamente pode trocar os temas quando o usuário faz isso. Aplicativos que foram compilados para Windows 8, mas executados no Windows 8.1, continue a usar o comportamento do Windows 8.

## <a name="design-time-tools-support-for-the-themeresource-markup-extension"></a>As ferramentas de tempo de design têm suporte para a extensão de marcação **{ThemeResource}**

Microsoft Visual Studio 2013 pode incluir possíveis valores de chave nas listas suspensas Microsoft IntelliSense quando você usa o **{ThemeResource}** extensão de marcação em uma página XAML. Por exemplo, assim que você digita "{ThemeResource", todas as chaves de recurso dos [recursos de tema XAML](https://msdn.microsoft.com/library/windows/apps/mt187274) são exibidas.

Quando uma chave de recurso existe como parte de qualquer uso **{ThemeResource}**, o recurso **Ir para Definição** (F12) pode resolver esse recurso e mostrar a você o generic.xaml do tempo de design, em que o recurso do tema é definido. Como os recursos do tema são definidos mais de uma vez (por tema), **Ir para Definição** leva você à primeira definição encontrada no arquivo, que é a definição de **Padrão**. Se você desejar obter as outras definições, poderá procurar o nome da chave no arquivo e localizar as definições dos outros temas.

## <a name="related-topics"></a>Tópicos relacionados

* [Referências de recurso de ResourceDictionary e XAML](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [Recursos de tema do XAML](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [atributo X:Key](x-key-attribute.md)
 


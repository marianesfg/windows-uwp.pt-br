---
description: Fornece um valor para qualquer atributo XAML analisando uma referência a um recurso já definido. Recursos são definidos em um ResourceDictionary, e o uso de StaticResource faz referência à chave desse recurso no ResourceDictionary.
title: Extensão de marcação StaticResource
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07d8e6c180f332e75852c6a6627004f0306e26d4
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259850"
---
# <a name="staticresource-markup-extension"></a>Extensão de marcação {StaticResource}


Fornece um valor para qualquer atributo XAML analisando uma referência a um recurso já definido. Recursos são definidos em um [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), e o uso de **StaticResource** faz referência à chave desse recurso no **ResourceDictionary**.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| chave | A chave para o recurso solicitado. Essa chave é inicialmente atribuída pelo [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary). Uma chave de recurso pode ser qualquer cadeia de caracteres definida na gramática de XamlName. |

## <a name="remarks"></a>Comentários

**StaticResource** é uma técnica para obtenção de valores referentes a um atributo XAML definidos em outro lugar em um dicionário de recursos XAML. Os valores podem ser colocados em um dicionário de recursos porque sua finalidade de uso é a de serem compartilhados por diversos valores de propriedades, ou porque um dicionário de recursos XAML é usado como uma técnica de empacotamento ou fatoramento XAML. Um exemplo de técnica de empacotamento XAML é o dicionário temático de um controle. Outro exemplo são os dicionários de recursos mesclados usados para fallback de recursos.

**StaticResource** obtém um argumento, que especifica a chave para o recurso solicitado. Uma chave de recurso sempre é uma cadeia de caracteres no XAML de Tempo de Execução do Windows. Para obter mais informações sobre como a chave de recurso é especificada inicialmente, consulte [atributo x:Key](x-key-attribute.md).

As regras pelas quais **StaticResource** é resolvido para um item em um dicionário de recursos não estão descritas neste tópico. Isso depende de a referência e o recurso existirem ou não em um modelo, de os dicionários de recursos mesclados serem usados ou não e assim por diante. Para obter mais informações sobre como definir recursos e como usar adequadamente um [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), incluindo o exemplo de código, consulte [Referências aos recursos ResourceDictionary e XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

**Importante**   um **StaticResource** não deve tentar fazer uma referência direta a um recurso definido lexicalmente mais dentro do arquivo XAML. Não é possível fazer isso. Mesmo se a referência de encaminhamento não falhar, tentar fazê-la acarreta uma penalidade de desempenho. Para obter melhores resultados, ajuste a composição dos seus dicionários de recursos de maneira que seja possível evitar referências de encaminhamento.

Tentar especificar um **StaticResource** para uma chave incapaz de resolver gera uma exceção de análise de XAML em tempo de execução. As ferramentas de design também podem apresentar avisos ou erros.

Na implementação do processador XAML do Windows Runtime XAML, não há uma representação de classe de suporte para a funcionalidade **StaticResource**. **StaticResource** é exclusivamente para ser usado em marcação XAML. O equivalente mais próximo em código é usar a API de coleção de um [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), chamando, por exemplo, [**Contains**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.contains) ou [**TryGetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.trygetvalue).

A [extensão de marcação {ThemeResource}](themeresource-markup-extension.md) é uma extensão de marcação similar que faz referência a recursos nomeados em outro local. A diferença é que a extensão de marcação {ThemeResource} tem a capacidade de retornar recursos diferentes, dependendo do tema do sistema que estiver ativo. Para obter mais informações, consulte [Extensão de marcação {ThemeResource}](themeresource-markup-extension.md).

**StaticResource** é uma extensão de marcação. As extensões de marcação geralmente são implementadas quando é necessário efetuar um escape de valores de atributo para que sejam diferentes de valores literais ou nomes de manipulador e o requisito é mais global do que simplesmente colocar conversores de tipo em certos tipos ou propriedades. Todas as extensões de marcação em XAML usam os caracteres "\{" e "\}" em sua sintaxe de atributo, que é a Convenção pela qual um processador XAML reconhece que uma extensão de marcação deve processar o atributo.

### <a name="an-example-staticresource-usage"></a>Exemplo de uso {StaticResource}

Este exemplo de XAML foi obtido do [exemplo de vinculação de dados XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind).

```xml
<StackPanel Margin="5">
    <!-- Add converter as a resource to reference it from a Binding. --> 
    <StackPanel.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </StackPanel.Resources>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Percent grade:" Margin="5" />
    <Slider x:Name="sliderValueConverter" Minimum="1" Maximum="100" Value="70" Margin="5"/>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Letter grade:" Margin="5"/>
    <TextBox x:Name="tbValueConverterDataBound"
      Text="{Binding ElementName=sliderValueConverter, Path=Value, Mode=OneWay,  
        Converter={StaticResource GradeConverter}}" Margin="5" Width="150"/> 
</StackPanel> 
```

Este exemplo específico cria um objeto com suporte em uma classe personalizada e cria-o como um recurso em [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary). Para ser um recurso válido, esse elemento `local:S2Formatter` também deve ter um valor de atributo **x:Key**. O valor do atributo está definido como "GradeConverter".

O recurso é solicitado apenas um pouco mais no XAML, em que você vê `{StaticResource GradeConverter}`.

Observe como o uso da extensão de marcação {StaticResource} está definindo uma propriedade de outra extensão de marcação [{Binding}](binding-markup-extension.md), por isso há dois usos de extensão de marcação aninhada aqui. A interna é avaliada primeiro, por isso o recurso é obtido primeiro e pode ser usado como um valor. Este mesmo exemplo também é mostrado na extensão de marcação {Binding}.

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>Suporte de ferramentas de tempo de design para a extensão de marcação **{StaticResource}**

Microsoft Visual Studio 2013 pode incluir possíveis valores de chave nas listas suspensas do Microsoft IntelliSense quando você usa a extensão de marcação **{StaticResource}** em uma página XAML. Por exemplo, assim que você digita "{StaticResource", as chaves de recurso do escopo de pesquisa são exibidas nos menus suspensos IntelliSense. Além dos recursos típicos, você estaria no nível da página ([**FrameworkElement.Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources)) e no nível do aplicativo ([**Application.Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resources)), e também veria os [recursos de temas XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources), e recursos de qualquer extensão que seu projeto esteja usando.

Como uma chave de recurso existe como parte de qualquer uso de **{StaticResource}** , o recurso **Ir para Definição** (F12) pode resolver esse recurso e mostrar a você o dicionário em que ele está definido. Para os recursos de tema, ele vai para generic.xaml do tempo de design.

## <a name="related-topics"></a>Tópicos relacionados

* [Referências de ResourceDictionary e recursos XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
* [**Dicionário**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [Atributo x:Key](x-key-attribute.md)
* [{Da mera} extensão de marcação](themeresource-markup-extension.md)


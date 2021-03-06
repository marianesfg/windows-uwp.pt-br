---
description: Vincula o valor de uma propriedade em um modelo de controle ao valor de outra propriedade exposta no controle modelo. TemplateBinding só pode ser usado dentro de uma definição ControlTemplate em XAML.
title: Extensão de marcação TemplateBinding
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e784b14c30222a28a0e10f8ba0bcf96e6c7f9afd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372312"
---
# <a name="templatebinding-markup-extension"></a>Extensão de marcação {TemplateBinding}

Vincula o valor de uma propriedade em um modelo de controle ao valor de outra propriedade exposta no controle modelo. **TemplateBinding** só pode ser usado dentro de uma definição [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) em XAML.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>Uso de atributo XAML (para a propriedade Setter em um modelo ou estilo)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| propertyName | O nome da propriedade que está sendo definida na sintaxe de setter. Deve ser uma propriedade de dependência. |
| sourceProperty | O nome de outra propriedade de dependência existente no tipo que está sendo modelado. |

## <a name="remarks"></a>Comentários

O uso de **TemplateBinding** é uma parte fundamental de como definir um modelo de controle, seja você um autor de controle personalizado ou esteja substituindo um modelo de controle para controles existentes. Para obter mais informações, consulte [guia de início rápido: Modelos de controle](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)).

É muito comum que *propertyName* e *targetProperty* usem o mesmo nome de propriedade. Nesse caso, um controle pode definir uma propriedade em si próprio e encaminhá-la para uma propriedade existente, com nome intuitivo, de uma de suas partes componentes. Por exemplo, um controle que incorpore um [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) em sua composição, usando-o para exibir a propriedade **Text** do próprio controle, pode incluir esse XAML como uma parte do modelo de controle:`<TextBlock Text="{TemplateBinding Text}" .... />`

Os tipos usados como o valor para propriedade de origem e a propriedade de destino devem corresponder. Não há oportunidade de introduzir um conversor quando estiver usando **TemplateBinding**. Caso os valores não correspondam, isso resulta em um erro ao analisar o XAML. Se um conversor for necessário você pode usar a sintaxe detalhada para uma associação de modelo, como: `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

A tentativa de usar um **TemplateBinding** fora de uma definição [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) em XAML resultará em erro do analisador.

Você pode usar **TemplateBinding** para casos em que o valor pai do modelo também está adiado como outra associação. A avaliação para **TemplateBinding** pode aguardar até que as associações do tempo de execução necessárias tenham valores.

Um **TemplateBinding** é sempre uma associação de uma via. Ambas as propriedades envolvidas devem ser propriedades dependentes.

**TemplateBinding** é uma extensão de marcação. As extensões de marcação geralmente são implementadas quando é necessário efetuar um escape de valores de atributo para que sejam diferentes de valores literais ou nomes de manipulador e o requisito é mais global do que simplesmente colocar conversores de tipo em certos tipos ou propriedades. Todas as extensões de marcação em XAML usam os caracteres "{" e "}" na sintaxe de atributo, sendo esta a convenção pela qual um processador XAML reconhece que uma extensão de marcação deve processar o atributo.

**Observação**  implementação do processador no Windows Runtime XAML, não há nenhuma representação de classe de apoio para **TemplateBinding**. **TemplateBinding** é de uso exclusivo na marcação XAML. Não existe uma maneira simples de reproduzir o comportamento em código.

### <a name="xbind-in-controltemplate"></a>X:BIND em ControlTemplate

> [!NOTE]
> Usar o X:BIND em um ControlTemplate requer o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior. Para saber mais sobre as versões de destino, consulte [Código adaptável de versão](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

Começando com o Windows 10, versão 1809, você pode usar o **X:Bind** extensão de marcação em qualquer lugar você usa **TemplateBinding** em um [ **ControlTemplate** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate). 

O [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) propriedade é necessária (não opcional) em [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) ao usar **X:Bind**.

Com o **X:Bind** dar suporte, você pode usar ambos [associações de função](../data-binding/function-bindings.md) , bem como vinculações bidirecionais em um [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate).

Neste exemplo, o **TextBlock. Text** propriedade for avaliada como **Button.Content.ToString**. TargetType em ControlTemplate atua como a fonte de dados e realiza o mesmo resultado que um TemplateBinding ao pai.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>Tópicos relacionados

* [Guia de início rápido: Modelos de controle](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))
* [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
* [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [Visão geral do XAML](xaml-overview.md)
* [Visão geral das propriedades de dependência](dependency-properties-overview.md)
 


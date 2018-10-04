---
author: jwmsft
description: Vincula o valor de uma propriedade em um modelo de controle ao valor de outra propriedade exposta no controle modelo. TemplateBinding só pode ser usado dentro de uma definição ControlTemplate em XAML.
title: Extensão de marcação TemplateBinding
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c1812adc9d5610fffd6f9d275b4e093a4fa96e6
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4354624"
---
# <a name="templatebinding-markup-extension"></a>Extensão de marcação {TemplateBinding}


Vincula o valor de uma propriedade em um modelo de controle ao valor de outra propriedade exposta no controle modelo. **TemplateBinding** só pode ser usado dentro de uma definição [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) em XAML.

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

O uso de **TemplateBinding** é uma parte fundamental de como definir um modelo de controle, seja você um autor de controle personalizado ou esteja substituindo um modelo de controle para controles existentes. Para obter mais informações, consulte [Guia de início rápido: modelos de controle](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374).

É muito comum que *propertyName* e *targetProperty* usem o mesmo nome de propriedade. Nesse caso, um controle pode definir uma propriedade em si próprio e encaminhá-la para uma propriedade existente, com nome intuitivo, de uma de suas partes componentes. Por exemplo, um controle que incorpore um [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) em sua composição, usando-o para exibir a propriedade **Text** do próprio controle, pode incluir esse XAML como uma parte do template de controle: `<TextBlock Text="{TemplateBinding Text}" .... />`

Os tipos usados como o valor para propriedade de origem e a propriedade de destino devem corresponder. Não há oportunidade de introduzir um conversor quando estiver usando **TemplateBinding**. Caso os valores não correspondam, isso resulta em um erro ao analisar o XAML. Se um conversor for necessário você pode usar a sintaxe detalhada para uma associação de modelo, como:  `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

A tentativa de usar um **TemplateBinding** fora de uma definição [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) em XAML resultará em erro do analisador.

Você pode usar **TemplateBinding** para casos em que o valor pai do modelo também está adiado como outra associação. A avaliação para **TemplateBinding** pode aguardar até que as associações do tempo de execução necessárias tenham valores.

Um **TemplateBinding** é sempre uma associação de uma via. Ambas as propriedades envolvidas devem ser propriedades dependentes.

**TemplateBinding** é uma extensão de marcação. As extensões de marcação geralmente são implementadas quando é necessário efetuar um escape de valores de atributo para que sejam diferentes de valores literais ou nomes de manipulador e o requisito é mais global do que simplesmente colocar conversores de tipo em certos tipos ou propriedades. Todas as extensões de marcação em XAML usam os caracteres "{" e "}" na sintaxe de atributo, sendo esta a convenção pela qual um processador XAML reconhece que uma extensão de marcação deve processar o atributo.

**Observação**  Na implementação do processador XAML do Windows Runtime, não há uma representação de classe de suporte para **TemplateBinding**. **TemplateBinding** é de uso exclusivo na marcação XAML. Não existe uma maneira simples de reproduzir o comportamento em código.

### <a name="xbind-in-controltemplate"></a>x: Bind em ControlTemplate

Começando com a próxima atualização principal para o Windows 10, você pode usar a extensão de marcação **X:Bind** em qualquer lugar você usou **TemplateBinding** em [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). 

A propriedade [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype#Windows_UI_Xaml_Controls_ControlTemplate_TargetType) será necessário (não opcional) em [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391) ao usar **x: Bind**.

Com o suporte de **x: Bind** , agora você pode usar ambas as [associações de função](../data-binding/function-bindings.md) como vinculações bidirecionais bem como em [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391)

No exemplo a seguir, o Text será avaliada como Button.Content.ToString(). O TargetType em ControlTemplate age como a fonte de dados e realize o mesmo resultado como um TemplateBinding para pai.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content}" />
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>Tópicos relacionados

* [Início rápido: modelos de controle](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)
* [Visão geral do XAML](xaml-overview.md)
* [Visão geral das propriedades de dependência](dependency-properties-overview.md)
 


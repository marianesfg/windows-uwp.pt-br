---
description: A extensão de marcação Binding é convertida, em um tempo de carregamento XAML, para uma instância da classe Binding.
title: Extensão de marcação Binding
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c2d54e9077429c6526168492c31a9d24a43a1be8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366526"
---
# <a name="binding-markup-extension"></a>Extensão de marcação {Binding}


**Observação**  um novo mecanismo de associação está disponível para Windows 10, que é otimizado para produtividade de desenvolvedor e de desempenho. Consulte [extensão de marcação {x: Bind}](x-bind-markup-extension.md).

**Observação**  para obter informações gerais sobre como usar a vinculação de dados em seu aplicativo com **{Binding}** (e para obter uma comparação de todo o entre **{X:Bind}** e **{Binding}** ), consulte [associação de dados em profundidade](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

O **{Binding}** extensão de marcação é usada para propriedades de associação de dados nos controles para valores provenientes de uma fonte de dados, como código. A extensão de marcação **{Binding}** é convertida, em um tempo de carregamento XAML, para uma instância da classe [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding). Esse objeto de associação obtém um valor de uma propriedade em uma fonte de dados e empurra-o para a propriedade no controle. O objeto de associação, opcionalmente, pode ser configurado para observar mudanças no valor da propriedade de origem de dados e atualizar-se com base nessas alterações. Ele também pode ser configurado opcionalmente para enviar as alterações para o valor de controle de volta para a propriedade de origem. A propriedade alvo de uma vinculação de dados deve ser uma propriedade de dependência. Para obter mais informações, consulte [Dependency properties overview](dependency-properties-overview.md).

**{Binding}** tem a mesma propriedade de dependência precedente como um valor local, e definir um valor local em código imperativo remove o efeito de qualquer **{Binding}** definido em marcações.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML


``` syntax
<object property="{Binding}" .../>
-or-
<object property="{Binding propertyPath}" .../>
-or-
<object property="{Binding bindingProperties}" .../>
-or-
<object property="{Binding propertyPath, bindingProperties}" .../>
```

| Termo | Descrição |
|------|-------------|
| *propertyPath* | Uma cadeia de caracteres que especifica o caminho de propriedade para a associação. Mais informações estão na seção [caminho de propriedade](#property-path) abaixo. |
| *bindingProperties* | *propName*=*value*\[, *propName*=*value*\]*<br/>Uma ou mais propriedades de associação que são especificadas usando uma sintaxe de par nome/valor. |
| *propName* | O nome da cadeia de caracteres da propriedade a ser definido no objeto [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding). Por exemplo, "Converter". |
| *Valor* | O valor a se definir a propriedade. A sintaxe do argumento depende da propriedade da seção [Propriedades da classe Binding que podem ser definidas com {Binding}](#properties-of-the-binding-class-that-can-be-set-with-binding) abaixo. |

## <a name="property-path"></a>Caminho de propriedade

[**Caminho** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) descreve a propriedade que você está ligando (a propriedade de origem). Caminho é um parâmetro de posição, o que significa que você pode usar o nome do parâmetro explicitamente (`{Binding Path=EmployeeID}`), ou pode especificá-lo como o primeiro parâmetro sem nome (`{Binding EmployeeID}`).

O tipo de [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) é um caminho de propriedade, que é uma cadeia de caracteres que evolui para uma propriedade ou subpropriedade do seu tipo personalizado ou de um tipo de estrutura. O tipo pode ser, mas precisa ser obrigatoriamente, um [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). Etapas de um caminho de propriedade são delimitadas por pontos (.), e você pode incluir vários delimitadores para percorrer subpropriedades sucessivas. Use o ponto delimitador independentemente da linguagem de programação usada para implementar o objeto sendo associado.

Por exemplo, para associar a interface do usuário à propriedade de primeiro nome de um objeto funcionário, o seu caminho de propriedade pode ser "Employee.FirstName". Se você estiver associando o controle de um item a uma propriedade que contém dependentes de um funcionário, o seu caminho de propriedade pode ser "Employee.Dependents", o modelo de item do controle do item exibiria os itens em "Dependents".

Se a fonte de dados for uma coleção, um caminho de propriedade pode especificar itens na coleção pela posição ou índice. Por exemplo, "equipes\[0\]. Players", em que o literal"\[\]"circunscreve o"0"que especifica o primeiro item em uma coleção.

Ao usar uma associação [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) em um [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) existente, você pode usar propriedade anexadas como parte do seu caminho de propriedade. Para desambiguar uma propriedade anexada para que o ponto intermediário no nome propriedade anexada não seja considerada uma etapa de um caminho de propriedade, coloque parênteses no nome da propriedade anexada qualificada pelo proprietário. Por exemplo: `(AutomationProperties.Name)`.

Um objeto intermediário de caminho de propriedade é armazenado como um objeto [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) em uma representação de tempo de execução, mas a maioria das situações não precisarão interagir com um objeto **PropertyPath** em código. Geralmente, você pode especificar as informações de associação de que precisa usando XAML.

Para saber mais sobre a sintaxe de cadeia de caracteres para um caminho de propriedade, caminhos de propriedade em áreas com recursos de animação e sobre construir um objeto [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath), consulte: [Property-path syntax](property-path-syntax.md).

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>Propriedades da classe Binding que podem ser definidas com {Binding}


**{Binding}** é ilustrado com a sintaxe de espaço reservado *bindingProperties* porque há várias propriedades de leitura/escrita de um [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) que podem ser definidas na extensão de marcação. As propriedades podem ser definidas em qualquer ordem com pares *propName*=*value* separados por vírgula. Algumas das propriedades exigem tipos que não têm uma conversão de tipo específica, então são necessárias extensões de marcação próprias aninhadas no **{Binding}** .

| Propriedade | Descrição |
|----------|-------------|
| [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) | Consulte a seção [Caminho de propriedade](#property-path) acima. |
| [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) | Especifica um objeto de conversor que é chamado pelo mecanismo de associação. O conversor pode ser definido na marcação usando a extensão de marcação [{StaticResource}](staticresource-markup-extension.md) para fazer referência ao objeto de um dicionário de recursos. |
| [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) | Especifica a cultura a ser usada pelo conversor. (Se você estiver definindo [ **conversor**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter).) A cultura é definida como um identificador com base em padrões. Para saber mais, veja [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) |
| [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) | Especifica um parâmetro do conversor que pode ser usado na lógica de conversão. (Se você estiver definindo [ **conversor**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter).) A maioria dos conversores usar lógica simples que obtém todas as informações de que precisam de valor transmitido para converter, e não precisa de uma **ConverterParameter** valor. O parâmetro **ConverterParameter** é para implementações de conversores mais complexos que têm mais de uma lógica condicional que controla o que for passado em **ConverterParameter**. Você pode abrir um conversor que usa valores além de cadeias de caracteres, mas não é comum. Consulte Comentários em **ConverterParameter** para saber mais. |
| [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) | Especifica uma fonte de dados referindo-se a outro elemento na mesma construção XAML que tem uma propriedade **Name** ou [x:Name attribute](x-name-attribute.md). Isso costuma ser usado para compartilhar valores relacionados ou usar subpropriedades de um elemento de interface do usuário para fornecer um valor específico a outro elemento; por exemplo, em um modelo de controle XAML. |
| [**FallbackValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.fallbackvalue) | Especifica um valor a ser exibido quando a fonte ou o caminho não podem ser resolvidos. |
| [**Modo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.mode) | Especifica o modo de ligação, como um destes valores: "OneTime", "OneWay" ou "TwoWay". Elas correspondem aos nomes de constantes da enumeração [**BindingMode**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindingMode). O padrão é "OneWay". Perceba que isso é diferente do padrão para **{x:Bind}** , que é "OneTime". | 
| [**RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) | Especifica a fonte de dados descrevendo a posição da fonte de associação relativa à posição do destino da associação. Isso geralmente é usado em associações em modelos de controle XAML. Definindo a [Extensão de marcação {RelativeSource}](relativesource-markup-extension.md). |
| [**Código-fonte**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source) | Especifica a fonte de dados do objeto. Na extensão de marcação de **Binding**, a propriedade [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source) requer uma referência de objeto, como uma referência de [extensão de marcação {StaticResource}](staticresource-markup-extension.md). Se essa propriedade não for especificada, o contexto de dados de atuação especificará a origem. É mais comum não especificar um valor Source em associações individuais e, em vez disso, usar o **DataContext** compartilhado para várias associações. Para obter mais informações, consulte [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) ou [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth). |
| [**TargetNullValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.targetnullvalue) | Especifica um valor a ser exibido quando o valor de origem é solucionado, mas é explicitamente **null**. |
| [**UpdateSourceTrigger**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.updatesourcetrigger) | Especifica o tempo das atualizações da fonte de associação. Se não estiver especificado, o padrão é **Default**. |

**Observação**  se você estiver convertendo uma marcação de **{X:Bind}** para **{Binding}** , em seguida, esteja ciente das diferenças nos valores padrão para o **modo** propriedade.

[**Conversor**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [ **ConverterLanguage** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) e **ConverterLanguage** estão relacionados ao cenário de converter um valor ou digite das origem da associação em um tipo ou um valor que é compatível com a propriedade de destino de associação. Para saber mais, veja a seção "Conversões de dados" em [Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

> [!NOTE]
> A partir do Windows 10, versão 1607, a estrutura XAML fornece um Booliano integrado para conversor de Visibilidade. O conversor mapeia **true** para o valor de enumeração **Visible** e **falso** para **Collapsed** para que você possa associar uma propriedade de Visibilidade a um Boolenao sem criar um conversor. Para usar o conversor integrado, a versão do SDK de alvo mínimo do seu aplicativo deve ser 14393 ou posterior. Você não poderá usá-lo se seu aplicativo for voltado para versões anteriores do Windows 10. Para saber mais sobre as versões de destino, consulte [Código adaptável de versão](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

[**Código-fonte**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source), [ **RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource), e [ **ElementName** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) especificar uma origem da associação, para que eles sejam mutuamente exclusivos.

**Dica**  se você precisar especificar uma única chave para um valor, como em [ **caminho** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) ou [ **ConverterParameter** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), em seguida, preceda-o com uma barra invertida: `\{`. De forma alternativa, colo a cadeia de caracteres inteira que contém a chave que precisa de escape entre apóstrofos; por exemplo, `ConverterParameter='{Mix}'`.

## <a name="examples"></a>Exemplos

```XML
<!-- binding a UI element to a view model -->    
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>

    <GridView ItemsSource="{Binding BookSkus}" SelectedItem="{Binding SelectedBookSku, Mode=TwoWay}" ... />
</Page>
```

```XML
<!-- binding a UI element to another UI element -->
<Page ... >
    <Page.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </Page.Resources>

    <Slider x:Name="sliderValueConverter" ... />
    <TextBox Text="{Binding Path=Value, ElementName=sliderValueConverter,
        Mode=OneWay,
        Converter={StaticResource GradeConverter}}"/>
</Page>
```

O segundo exemplo define quatro diferentes [ **associação** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) propriedades: [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname), [ **caminho**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path), [ **modo** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.mode) e [ **conversor** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter). **Path**, nesse caso, é mostrado nomeado explicitamente como uma propriedade **Binding**. O **Path** é avaliado para uma fonte de vinculação de dados que é outro objeto na mesma árvore de objetos de tempo de execução, um [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) nomeado como `sliderValueConverter`.

Observe como o valor da propriedade [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) usa outra extensão de marcação, [{StaticResource} markup extension](staticresource-markup-extension.md), para que haja duas extensões de marcação aninhadas. A interna é avaliada primeiro para que, quando o recurso for obtido, haja um [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) prático (uma classe personalizada exemplificada pelo elemento `local:S2Formatter` em recursos) que a associação possa usar.

## <a name="tools-support"></a>Suporte a ferramentas

O Microsoft IntelliSense no Microsoft Visual Studio exibe as propriedades de contexto dos dados enquanto cria o **{Binding}** no editor de marcações XAML. Assim que você digita "{Binding", propriedades de contexto dos dados associadas ao [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) são exibidas no menu suspenso. IntelliSense também ajuda com as outras propriedades do [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding). Para que isso funcione, você deve ter o contexto de dados ou o contexto dos dados de tempo de design definido na página de marcação. **Ir para definição** (F12) também funciona com **{Binding}** . Como alternativa, você pode usar a caixa de diálogo de vinculação de dados.

 

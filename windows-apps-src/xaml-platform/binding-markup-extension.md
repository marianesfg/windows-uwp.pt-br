---
description: A extensão de marcação Binding é convertida, em um tempo de carregamento XAML, para uma instância da classe Binding.
title: Extensão de marcação Binding
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b197ea668ec73711b7a9c63e516b4ec9a5f54d62
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8895048"
---
# <a name="binding-markup-extension"></a>Extensão de marcação {Binding}


**Observação**um novo mecanismo de associação está disponível para Windows 10, que é otimizado para a produtividade do desenvolvedor e de desempenho. Consulte [extensão de marcação {x: Bind}](x-bind-markup-extension.md).

**Observação**para obter informações gerais sobre como usar dados de associação em seu aplicativo com **{Binding}** (e para uma todas as comparações entre **{x: Bind}** e **{Binding}**), consulte a [vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).

A extensão de marcação **{Binding}** é usado para propriedades de associação de dados nos controles para valores provenientes de uma fonte de dados, como código. A extensão de marcação **{Binding}** é convertida, em um tempo de carregamento XAML, para uma instância da classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Esse objeto de associação obtém um valor de uma propriedade em uma fonte de dados e empurra-o para a propriedade no controle. O objeto de associação, opcionalmente, pode ser configurado para observar mudanças no valor da propriedade de origem de dados e atualizar-se com base nessas alterações. Ele também pode ser configurado opcionalmente para enviar as alterações para o valor de controle de volta para a propriedade de origem. A propriedade alvo de uma vinculação de dados deve ser uma propriedade de dependência. Para obter mais informações, consulte [Dependency properties overview](dependency-properties-overview.md).

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
| *propName* | O nome da cadeia de caracteres da propriedade a ser definido no objeto [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Por exemplo, "Converter". |
| *value* | O valor a se definir a propriedade. A sintaxe do argumento depende da propriedade da seção [Propriedades da classe Binding que podem ser definidas com {Binding}](#properties-of-the-binding-class-that-can-be-set-with-binding) abaixo. |

## <a name="property-path"></a>Caminho de propriedade

[**Caminho**](https://msdn.microsoft.com/library/windows/apps/br209830) descreve a propriedade à qual você está se associando (a propriedade de origem). Caminho é um parâmetro de posição, o que significa que você pode usar o nome do parâmetro explicitamente (`{Binding Path=EmployeeID}`), ou pode especificá-lo como o primeiro parâmetro sem nome (`{Binding EmployeeID}`).

O tipo de [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) é um caminho de propriedade, que é uma cadeia de caracteres que evolui para uma propriedade ou subpropriedade do seu tipo personalizado ou de um tipo de estrutura. O tipo pode ser, mas precisa ser obrigatoriamente, um [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Etapas de um caminho de propriedade são delimitadas por pontos (.), e você pode incluir vários delimitadores para percorrer subpropriedades sucessivas. Use o ponto delimitador independentemente da linguagem de programação usada para implementar o objeto sendo associado.

Por exemplo, para associar a interface do usuário à propriedade de primeiro nome de um objeto funcionário, o seu caminho de propriedade pode ser "Employee.FirstName". Se você estiver associando o controle de um item a uma propriedade que contém dependentes de um funcionário, o seu caminho de propriedade pode ser "Employee.Dependents", o modelo de item do controle do item exibiria os itens em "Dependents".

Se a fonte de dados for uma colação, um caminho de propriedade pode especificar itens na coleção pela posição ou índice. Por exemplo, em Teams\[0\].Players", onde "\[\]" contém o "0" que especifica o primeiro item em uma coleção.

Ao usar uma associação [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) em um [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) existente, você pode usar propriedade anexadas como parte do seu caminho de propriedade. Para desambiguar uma propriedade anexada para que o ponto intermediário no nome propriedade anexada não seja considerada uma etapa de um caminho de propriedade, coloque parênteses no nome da propriedade anexada qualificada pelo proprietário. Por exemplo: `(AutomationProperties.Name)`.

Um objeto intermediário de caminho de propriedade é armazenado como um objeto [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) em uma representação de tempo de execução, mas a maioria das situações não precisarão interagir com um objeto **PropertyPath** em código. Geralmente, você pode especificar as informações de associação de que precisa usando XAML.

Para saber mais sobre a sintaxe de cadeia de caracteres para um caminho de propriedade, caminhos de propriedade em áreas com recursos de animação e sobre construir um objeto [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259), consulte: [Property-path syntax](property-path-syntax.md).

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>Propriedades da classe Binding que podem ser definidas com {Binding}


**{Binding}** é ilustrado com a sintaxe de espaço reservado *bindingProperties* porque há várias propriedades de leitura/escrita de um [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) que podem ser definidas na extensão de marcação. As propriedades podem ser definidas em qualquer ordem com pares *propName*=*value* separados por vírgula. Algumas das propriedades exigem tipos que não têm uma conversão de tipo específica, então são necessárias extensões de marcação próprias aninhadas no **{Binding}**.

| Propriedade | Descrição |
|----------|-------------|
| [**Caminho**](https://msdn.microsoft.com/library/windows/apps/br209830) | Consulte a seção [Caminho de propriedade](#property-path) acima. |
| [**Conversor**](https://msdn.microsoft.com/library/windows/apps/br209826) | Especifica um objeto de conversor que é chamado pelo mecanismo de associação. O conversor pode ser definido na marcação usando a extensão de marcação [{StaticResource}](staticresource-markup-extension.md) para fazer referência ao objeto de um dicionário de recursos. |
| [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) | Especifica a cultura a ser usada pelo conversor. (Se você estiver definindo [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826).) A cultura é definida como um identificador baseado em padrões. Para saber mais, veja [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) |
| [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) | Especifica um parâmetro do conversor que pode ser usado na lógica de conversão. (Se você estiver definindo [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826).) A maioria dos conversores usam lógica simples que obtém todas as informações que precisam do valor calculado para converter, e não precisam de um valor **ConverterParameter**. O parâmetro **ConverterParameter** é para implementações de conversores mais complexos que têm mais de uma lógica condicional que controla o que for passado em **ConverterParameter**. Você pode abrir um conversor que usa valores além de cadeias de caracteres, mas não é comum. Consulte Comentários em **ConverterParameter** para saber mais. |
| [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) | Especifica uma fonte de dados referindo-se a outro elemento na mesma construção XAML que tem uma propriedade **Name** ou [x:Name attribute](x-name-attribute.md). Isso costuma ser usado para compartilhar valores relacionados ou usar subpropriedades de um elemento de interface do usuário para fornecer um valor específico a outro elemento; por exemplo, em um modelo de controle XAML. |
| [**FallbackValue**](https://msdn.microsoft.com/library/windows/apps/dn279345) | Especifica um valor a ser exibido quando a fonte ou caminho não podem ser resolvidos. |
| [**Modo**](https://msdn.microsoft.com/library/windows/apps/br209829) | Especifica o modo de associação como um desses valores: "OneTime", "OneWay" ou "TwoWay". Elas correspondem aos nomes de constantes da enumeração [**BindingMode**](https://msdn.microsoft.com/library/windows/apps/br209822). O padrão é "OneWay". Perceba que isso é diferente do padrão para **{x:Bind}**, que é "OneTime". | 
| [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) | Especifica a fonte de dados descrevendo a posição da fonte de associação relativa à posição do destino da associação. Isso geralmente é usado em associações em modelos de controle XAML. Definindo a [Extensão de marcação {RelativeSource}](relativesource-markup-extension.md). |
| [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) | Especifica a fonte de dados do objeto. Na extensão de marcação de **Binding**, a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) requer uma referência de objeto, como uma referência de [extensão de marcação {StaticResource}](staticresource-markup-extension.md). Se essa propriedade não for especificada, o contexto de dados de atuação especificará a origem. É mais comum não especificar um valor Source em associações individuais e, em vez disso, usar o **DataContext** compartilhado para várias associações. Para obter mais informações, consulte [**DataContext**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.datacontext.aspx) ou [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946). |
| [**TargetNullValue**](https://msdn.microsoft.com/library/windows/apps/dn279347) | Especifica um valor a ser exibido quando o valor de origem é solucionado, mas é explicitamente **null**. |
| [**UpdateSourceTrigger**](https://msdn.microsoft.com/library/windows/apps/dn279350) | Especifica o tempo das atualizações da fonte de associação. Se não estiver especificado, o padrão é **Default**. |

**Observação**se você estiver convertendo a marcação de **{x: Bind}** para **{Binding}**, lembre-se das diferenças em default valores para a propriedade **Mode** .

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) e **ConverterLanguage** estão todos relacionados à situação de conversão de um valor ou tipo de uma fonte de associação a um tipo ou valor que é compatível com a propriedade do destino da associação. Para saber mais, veja a seção "Conversões de dados" em [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).

> [!NOTE]
> A partir do Windows 10, versão 1607, a estrutura XAML fornece um Booleano integrado para conversor de Visibilidade. O conversor mapeia **true** para o valor de enumeração **Visible** e **falso** para **Collapsed** para que você possa associar uma propriedade de Visibilidade a um Boolenao sem criar um conversor. Para usar o conversor integrado, a versão do SDK de alvo mínimo do seu aplicativo deve ser 14393 ou posterior. Você não poderá usá-lo se seu aplicativo for voltado para versões anteriores do Windows 10. Para saber mais sobre as versões de destino, consulte [Código adaptável de versão](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

[**Source**](https://msdn.microsoft.com/library/windows/apps/br209832), [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) e [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) especificam uma fonte de associação, portanto são mutualmente exclusivas.

**Dica**se você precisar especificar uma única chave para um valor, como em um [**caminho**](https://msdn.microsoft.com/library/windows/apps/br209830) ou [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), em seguida, coloque uma barra invertida: `\{`. De forma alternativa, colo a cadeia de caracteres inteira que contém a chave que precisa de escape entre apóstrofos; por exemplo, `ConverterParameter='{Mix}'`.

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

O segundo exemplo define quatro propriedades [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) diferentes: [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830), [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) e [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826). **Path**, nesse caso, é mostrado nomeado explicitamente como uma propriedade **Binding**. O **Path** é avaliado para uma fonte de vinculação de dados que é outro objeto na mesma árvore de objetos de tempo de execução, um [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) nomeado como `sliderValueConverter`.

Observe como o valor da propriedade [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) usa outra extensão de marcação, [{StaticResource} markup extension](staticresource-markup-extension.md), para que haja duas extensões de marcação aninhadas. A interna é avaliada primeiro para que, quando o recurso for obtido, haja um [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) prático (uma classe personalizada exemplificada pelo elemento `local:S2Formatter` em recursos) que a associação possa usar.

## <a name="tools-support"></a>Suporte a ferramentas

O Microsoft IntelliSense no Microsoft Visual Studio exibe as propriedades de contexto dos dados enquanto cria o **{Binding}** no editor de marcações XAML. Assim que você digita "{Binding", propriedades de contexto dos dados associadas ao [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) são exibidas no menu suspenso. IntelliSense também ajuda com as outras propriedades do [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Para que isso funcione, você deve ter o contexto de dados ou o contexto dos dados de tempo de design definido na página de marcação. **Ir para definição** (F12) também funciona com **{Binding}**. Como alternativa, você pode usar a caixa de diálogo de vinculação de dados.

 

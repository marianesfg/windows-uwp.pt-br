---
author: jwmsft
description: "A extensão de marcação Binding é convertida, em um tempo de carregamento XAML, para uma instância da classe Binding."
title: "Extensão de marcação Binding"
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: 740110809845220d919c6ba3c90b1393dbc8ae94

---

# Extensão de marcação {Binding}

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


            **Observação**  Um novo mecanismo de associação está disponível para Windows 10, que é otimizado para o desempenho e a produtividade do desenvolvedor. Consulte [extensão de marcação {x: Bind}](x-bind-markup-extension.md).


            **Observação**  Para informações gerais sobre o uso da vinculação de dados no seu aplicativo com **{Binding}** (e para todas as comparações entre **{x:Bind}** e **{Binding}**), consulte [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).

A extensão de marcação **{Binding}** é convertida, em um tempo de carregamento XAML, para uma instância da classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Esse objeto de associação obtém um valor de uma propriedade em uma fonte de dados. O objeto de associação, opcionalmente, pode ser configurado para observar mudanças no valor da propriedade de origem de dados e atualizar-se com base nessas alterações. Ele também pode ser configurado opcionalmente para enviar as alterações em seu próprio valor de volta para a propriedade de origem. A propriedade alvo de uma vinculação de dados deve ser uma propriedade de dependência. Para obter mais informações, consulte [Dependency properties overview](dependency-properties-overview.md).


            **{Binding}** tem a mesma propriedade de dependência precedente como um valor local, e definir um valor local em código imperativo remove o efeito de qualquer **{Binding}** definido em marcações.

**Aplicativos de exemplo que demonstram {Binding}**

-   Baixe o aplicativo [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950).
-   Baixe o aplicativo [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952).

## Uso do atributo XAML


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
| *bindingProperties* | 
            *propName*
            =
            *value*\[, *propName*=*value*\]*<br/>Uma ou mais propriedades de associação que são especificadas usando uma sintaxe de par nome/valor. |
| *propName* | O nome da cadeia de caracteres da propriedade a ser definido no objeto [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Por exemplo, "Converter". | 
| *value* | O valor a se definir a propriedade. A sintaxe do argumento depende da propriedade da seção [Propriedades da classe Binding que podem ser definidas com {Binding}](#properties-of-binding) abaixo. |

## Caminho de propriedade


            *PropertyPath* define o valor do [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830), que é a propriedade a que você está fazendo a associação (a propriedade de origem). Você pode mencionar o nome da propriedade explicitamente: `{Binding Path=...}`. Ou você pode omiti-lo: `{Binding ...}`.

O tipo de [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) é um caminho de propriedade, que é uma cadeia de caracteres que evolui para uma propriedade ou subpropriedade do seu tipo personalizado ou de um tipo de estrutura. O tipo pode ser, mas precisa ser obrigatoriamente, um [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Etapas de um caminho de propriedade são delimitadas por pontos (.), e você pode incluir vários delimitadores para percorrer subpropriedades sucessivas. Use o ponto delimitador independentemente da linguagem de programação usada para implementar o objeto sendo associado.

Por exemplo, para associar a interface do usuário à propriedade de primeiro nome de um objeto funcionário, o seu caminho de propriedade pode ser "Employee.FirstName". Se você estiver associando o controle de um item a uma propriedade que contém dependentes de um funcionário, o seu caminho de propriedade pode ser "Employee.Dependents", o modelo de item do controle do item exibiria os itens em "Dependents".

Se a fonte de dados for uma colação, um caminho de propriedade pode especificar itens na coleção pela posição ou índice. Por exemplo, em Teams\[0\].Players", onde "\[\]" contém o "0" que especifica o primeiro item em uma coleção.

Ao usar uma associação [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) em um [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) existente, você pode usar propriedade anexadas como parte do seu caminho de propriedade. Para desambiguar uma propriedade anexada para que o ponto intermediário no nome propriedade anexada não seja considerada uma etapa de um caminho de propriedade, coloque parênteses no nome da propriedade anexada qualificada pelo proprietário. Por exemplo: `(AutomationProperties.Name)`.

Um objeto intermediário de caminho de propriedade é armazenado como um objeto [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) em uma representação de tempo de execução, mas a maioria das situações não precisarão interagir com um objeto **PropertyPath** em código. Geralmente, você pode especificar as informações de associação de que precisa usando XAML.

Para saber mais sobre a sintaxe de cadeia de caracteres para um caminho de propriedade, caminhos de propriedade em áreas com recursos de animação e sobre construir um objeto [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259), consulte: [Property-path syntax](property-path-syntax.md).

## Propriedades da classe Binding que podem ser definidas com {Binding}



            **{Binding}** é ilustrado com a sintaxe de espaço reservado *bindingProperties* porque há várias propriedades de leitura/escrita de um [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) que podem ser definidas na extensão de marcação. As propriedades podem ser definidas em qualquer ordem com pares *propName*=*value* separados por vírgula. Algumas das propriedades exigem tipos que não têm uma conversão de tipo específica, então são necessárias extensões de marcação próprias aninhadas no **{Binding}**.

| Propriedade | Descrição |
|----------|-------------|
| [**Caminho**](https://msdn.microsoft.com/library/windows/apps/br209830) | Consulte a seção [Caminho de propriedade](#property-path) acima. |
| [**Conversor**](https://msdn.microsoft.com/library/windows/apps/br209826) | Especifica o objeto de conversor que é chamado pelo mecanismo de associação. O conversor pode ser definido em XAML, mas somente se você se referir a uma instância de objeto atribuída em uma referência [{StaticResource} markup extension](staticresource-markup-extension.md) ao objeto no dicionário de recursos. |
| [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) | Especifica a cultura a ser usada pelo conversor. (Se você estiver definindo [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826).) A cultura é definida como um identificador baseado em padrões. Para saber mais, consulte **ConverterLanguage**. | 
| [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) | Especifica o parâmetro do conversor que pode ser usado na lógica de conversão. (Se você estiver definindo [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826).) A maioria dos conversores usam lógica simples que obtém todas as informações que precisam do valor calculado para converter, e não precisam de um valor **ConverterParameter**. O parâmetro **ConverterParameter** é para implementações moderadamente avançadas que têm mais de uma lógica que controla o que for passado em **ConverterParameter**. Você abrir um conversos que usa valores além de cadeias de caracteres, mas não é comum. Consulte Comentários em **ConverterParameter** para saber mais. |
| [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) | Especifica uma fonte de dados referindo-se a outro elemento na mesma construção XAML que tem uma propriedade **Name** ou [x:Name attribute](x-name-attribute.md). Isso costuma ser usado para compartilhar valores relacionados ou usar subpropriedades de um elemento de interface do usuário para fornecer um valor específico a outro elemento; por exemplo, em um modelo de controle XAML. |
| [**FallbackValue**](https://msdn.microsoft.com/library/windows/apps/dn279345) | Especifica um valor a ser exibido quando a fonte ou caminho não podem ser resolvidos. | 
| [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) | Especifica o mode de associação como uma dessas cadeias de caracteres: "OneTime", "OneWay" ou "TwoWay". Elas correspondem aos nomes de constantes da enumeração [**BindingMode**](https://msdn.microsoft.com/library/windows/apps/br209822). O padrão depende do destino da associação, mas, na maioria das vezes, é "OneWay". Perceba que isso é diferente do padrão para **{x:Bind}**, que é "OneTime". | 
| [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) | Especifica a fonte de dados descrevendo a posição da fonte de associação relativa à posição do destino da associação. Isso se expressa em termos do gráfico do objeto de tempo de execução; por exemplo, especificando os pais do objeto. Definindo a [Extensão de marcação {RelativeSource}](relativesource-markup-extension.md). |
| [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) | Especifica a fonte de dados do objeto. Na extensão de marcação de **Binding**, a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) requer uma referência de objeto, como uma referência de [extensão de marcação {StaticResource}](staticresource-markup-extension.md). Se essa propriedade não for especificada, o contexto de dados de atuação especificará a origem. É mais comum não especificar um valor Source em associações individuais e, em vez disso, usar o **DataContext** compartilhado para várias associações. Para obter mais informações, consulte [**DataContext**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.datacontext.aspx) ou [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946). |
| [**TargetNullValue**](https://msdn.microsoft.com/library/windows/apps/dn279347) | Especifica um valor a ser exibido quando o valor de origem é solucionado, mas é explicitamente **null**. |
| [**UpdateSourceTrigger**](https://msdn.microsoft.com/library/windows/apps/dn279350) | Especifica o tempo das atualizações da fonte de associação. Se não estiver especificado, o padrão é **Default**. |


            **Observação**  Se você estiver convertendo a marcação de **{x:Bind}** para **{Binding}**, examine as diferenças nos valores padrão da propriedade **Mode**.


            [
              **Converter**
            ](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) e **ConverterLanguage** são todos relacionados à situação de conversão de um valor ou tipo de uma fonte de associação a um tipo ou valor que é compatível com a propriedade do destino da associação. Para obter mais informações, consulte a seção de "Conversões de dados" em [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).


            [
              **Source**
            ](https://msdn.microsoft.com/library/windows/apps/br209832), [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) e [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) especificam uma fonte de associação, então são mutualmente exclusivas.


            **Dica**  Se você precisar especificar uma única chave para um valor, como em [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) ou [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), coloque uma barra invertida antes dela: `\{`. De forma alternativa, colo a cadeia de caracteres inteira que contém a chave que precisa de escape entre apóstrofos; por exemplo, `ConverterParameter='{Mix}'`.

## Exemplos

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

O segundo exemplo define quatro propriedades [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) diferentes: [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830), [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) e [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826). 
            **Path**, nesse caso, é mostrado nomeado explicitamente como uma propriedade **Binding**. O **Path** é avaliado para uma fonte de vinculação de dados que é outro objeto na mesma árvore de objetos de tempo de execução, um [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) nomeado como `sliderValueConverter`.

Observe como o valor da propriedade [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) usa outra extensão de marcação, [{StaticResource} markup extension](staticresource-markup-extension.md), para que haja duas extensões de marcação aninhadas. A interna é avaliada primeiro para que, quando o recurso for obtido, haja um [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) prático (uma classe personalizada exemplificada pelo elemento `local:S2Formatter` em recursos) que a associação possa usar.

## Suporte a ferramentas

O Microsoft IntelliSense no Microsoft Visual Studio exibe as propriedades de contexto dos dados enquanto cria o **{Binding}** no editor de marcações XAML. Assim que você digita "{Binding", propriedades de contexto dos dados associadas ao [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) são exibidas no menu suspenso. IntelliSense também ajuda com as outras propriedades do [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Para que isso funcione, você deve ter o contexto de dados ou o contexto dos dados de tempo de design definido na página de marcação. 
            **Ir para definição** (F12) também funciona com **{Binding}**. Como alternativa, você pode usar a caixa de diálogo de vinculação de dados.

 




<!--HONumber=Jun16_HO4-->



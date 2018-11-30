---
description: A marcação xbind permite funções para ser usado na marcação.
title: 'Funções em x: Bind'
ms.date: 04/26/2018
ms.topic: article
keywords: Windows 10, uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: 371b64a6161911242acd5b9abf97cfa7d2f05358
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8212606"
---
# <a name="functions-in-xbind"></a>Funções em x: Bind

**Observação**para obter informações gerais sobre como usar dados de associação em seu aplicativo com **{x: Bind}** (e para uma todas as comparações entre **{x: Bind}** e **{Binding}**), consulte a [vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946).

Desde o Windows 10, versão 1607, **{x: Bind}** dá suporte ao uso de uma função como a etapa de folha do caminho de associação. Isso permite que:

- Uma maneira mais simples de conseguir a conversão de valor
- Uma maneira para associações dependerem de mais de um parâmetro

> [!NOTE]
> Para usar funções com **{x:Bind}**, a versão do SDK de alvo mínima do aplicativo deve ser 14393 ou posterior. Você não poderá usar funções quando o aplicativo se destinar a versões anteriores do Windows 10. Para saber mais sobre as versões de destino, consulte [Código adaptável de versão](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

No exemplo a seguir, o primeiro e o segundo planos do item estão associados a funções para fazer uma conversão com base no parâmetro de cor

```xaml
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind local:ColorEntry.Brushify(Color), Mode=OneWay}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```

```csharp
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}
```

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>Caminho para a função

O caminho para a função é especificado como outros caminhos de propriedade e pode incluir pontos (.), indexadores ou conversões para localizar a função.

As funções estáticas podem ser especificadas usando-se a sintaxe XMLNamespace:ClassName.MethodName. Por exemplo, use o abaixo sintaxe para associação para funções estáticas no code-behind.

```xaml
<Page 
     xmlns:local="using:MyPage">
     ...
     <Grid x:Name="myGrid" Background="Black" >
        <TextBlock Foreground="{x:Bind local:GenerateAppropriateForeground(myGrid.Background)}" Text="Hello World!" />
    </Grid>
</Page>
```
```csharp
public class MyPage : Page
{
    public static GenerateAppropriateForeground(SolidColorBrush background)
    {
        //Implement static function
        ...
    }
}
```

Você também pode usar funções de sistema diretamente na marcação para realizar cenários simples como formatação de data, formatação de texto, concatenações de texto, etc., por exemplo:
```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyPage">
     ...
     <CalendarDatePicker Date="{x:Bind sys:DateTime.Parse(TextBlock1.Text)}" />
     <TextBlock Text="{x:Bind sys:String.Format('{0} is now available in {1}', local:MyPage.personName, local:MyPage.location)}" />
</Page>
```

Se o modo for OneWay/TwoWay, o caminho da função terá detecção de alteração realizada nele, e a associação será reavaliada se houver alterações nesses objetos.

A função associada precisa:

- Estar acessível para o código e os metadados – assim, internal / private funcionam em C#, mas C++/CX precisarão de métodos que sejam métodos WinRT públicos
- A sobrecarga se baseia no número de argumentos, e não no tipo, e ela tentará se comparar com a primeira sobrecarga com muitos argumentos
- Os tipos de argumento precisam corresponder aos dados passados – não fazemos conversões de restrição
- O tipo de retorno da função precisa corresponder ao tipo da propriedade que está usando a associação

Começando com a próxima atualização principal para o Windows 10, o mecanismo de vinculação reagir às notificações de alteração de propriedade acionadas com o nome da função e reavaliar associações conforme necessário. Por exemplo: 

```XAML
<DataTemplate x:DataType="local:Person">
   <StackPanel>
      <TextBlock Text="{x:Bind FullName}" />
      <Image Source="{x:Bind IconToBitmap(Icon, CancellationToken), Mode=OneWay}" />
   </StackPanel>
</DataTemplate>
```
```csharp
public class Person:INotifyPropertyChanged
{
    //Implementation for an Icon property and a CancellationToken property with PropertyChanged notifications
    ...

    //IconToBitmap function is essentially a multi binding converter between several options.
    public Uri IconToBitmap (Uri icon, Uri cancellationToken)
    {
        Uri foo = new Uri(...);        
        if (isCancelled)
        {
            foo = cancellationToken;
        }
        else 
        {
            if (this.fullName.Contains("Sr"))
            {
               //pass a different Uri back
               foo = new Uri(...);
            }
            else
            {
                foo = icon;
            }
        }
        return foo;
    }

    //Ensure FullName property handles change notification on itself as well as IconToBitmap since the function uses it
    public string FullName
    {
        get { return this.fullName; }
        set 
        {
            this.fullName = value;
            this.OnPropertyChanged ();
            this.OnPropertyChanged ("IconToBitmap"); 
            //this ensures Image.Source binding re-evaluates when FullName changes in addition to Icon and CancellationToken
        }
    }
}
```

> [!TIP]
> Você pode usar funções no x: Bind para alcançar os mesmos cenários como o que foi suportado por meio de conversores e MultiBinding no WPF.

## <a name="function-arguments"></a>Argumentos de função

Vários argumentos de função podem ser especificados, separados por vírgula (,)

- Caminho de associação – a mesma sintaxe como se você estivesse associando diretamente esse objeto.
  - Se o modo for OneWay/TwoWay, a detecção de alteração será realizada, e a associação será reavaliada mediante a alteração do objeto
- Cadeia de caracteres constante entre aspas – aspas são necessárias para designá-la como uma cadeia de caracteres. O acento circunflexo (^) pode ser usado no escape de citações em cadeias de caracteres
- Número da constante - por exemplo -123,456
- Booliano – especificado como "x:True" ou "x:False"

### <a name="two-way-function-bindings"></a>Associações de função bidirecional

Em um cenário de associação bidirecional, uma segunda função deve ser especificada para a direção inversa da associação. Isso é feito usando a propriedade de associação **BindBack** . No exemplo abaixo, a função deve utilizar um argumento que é o valor que precisa ser reenviado para o modelo.
```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```

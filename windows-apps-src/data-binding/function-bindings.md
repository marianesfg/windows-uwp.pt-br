---
description: A extensão de marcação xBind permite que as funções a serem usadas na marcação.
title: 'Funções em x: Bind'
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: b85777c254c36cc7bf5b156569c7cef267a6c567
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626211"
---
# <a name="functions-in-xbind"></a>Funções em x: Bind

> [!NOTE]
> Para obter informações gerais sobre como usar a vinculação de dados em seu aplicativo com **{X:Bind}** (e para obter uma comparação de todo o entre **{X:Bind}** e **{Binding}**), consulte [dados vinculação profunda](data-binding-in-depth.md).

Desde o Windows 10, versão 1607, **{x: Bind}** dá suporte ao uso de uma função como a etapa de folha do caminho de associação. Isso permite:

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

```xaml
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>Caminho para a função

O caminho para a função é especificado como outros caminhos de propriedade e pode incluir pontos (.), indexadores ou conversões para localizar a função.

As funções estáticas podem ser especificadas usando-se a sintaxe XMLNamespace:ClassName.MethodName. Por exemplo, use o abaixo de sintaxe de ligação com funções estáticas no code-behind.

```xaml
<Page 
     xmlns:local="using:MyNamespace">
     ...
    <StackPanel>
        <TextBlock x:Name="BigTextBlock" FontSize="20" Text="Big text" />
        <TextBlock FontSize="{x:Bind local:MyHelpers.Half(BigTextBlock.FontSize)}" 
                   Text="Small text" />
    </StackPanel>
</Page>
```

```csharp
namespace MyNamespace
{
    static public class MyHelpers
    {
        public static double Half(double value) => value / 2.0;
    }
}
```

Você também pode usar as funções do sistema diretamente na marcação para realizar cenários simples, como formatação de datas, formatação de texto, concatenações de cadeias de texto, etc., por exemplo:

```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyNamespace">
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

O mecanismo de associação reage a alteração da propriedade notificações acionado com o nome da função e reavaliar associações conforme necessário. Por exemplo:

```xaml
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
> Você pode usar funções em x: Bind para alcançar os mesmos cenários como o que tinha suporte por meio de conversores e MultiBinding no WPF.

## <a name="function-arguments"></a>Argumentos de função

Vários argumentos de função podem ser especificados, separados por vírgula (,)

- Caminho de associação – a mesma sintaxe como se você estivesse associando diretamente esse objeto.
  - Se o modo for OneWay/TwoWay, a detecção de alteração será realizada, e a associação será reavaliada mediante a alteração do objeto
- Cadeia de caracteres constante entre aspas – aspas são necessárias para designá-la como uma cadeia de caracteres. O acento circunflexo (^) pode ser usado no escape de citações em cadeias de caracteres
- Número da constante - por exemplo -123,456
- Booliano – especificado como "x:True" ou "x:False"

### <a name="two-way-function-bindings"></a>Associações de função bidirecional

Em um cenário de associação bidirecional, uma segunda função deve ser especificada para a direção inversa da associação. Isso é feito usando o **BindBack** propriedade de associação. No exemplo abaixo, a função deve usar um argumento que é o valor que precisa ser enviado de volta para o modelo.

```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```

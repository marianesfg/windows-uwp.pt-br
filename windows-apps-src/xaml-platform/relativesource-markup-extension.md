---
description: Fornece um meio para especificar o origem de uma associação em termos de uma relação relativa no gráfico do objeto de tempo de execução.
title: Extensão de marcação RelativeSource
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
---

# Extensão de marcação {RelativeSource}

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Fornece um meio para especificar o origem de uma associação em termos de uma relação relativa no gráfico do objeto de tempo de execução.

## Uso de atributos XAML (Modo por conta própria)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## Uso de atributos XAML (Modo TemplatedParent)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## Valores XAML

| Termo | Descrição |
| {RelativeSource Self} | Produz um valor de [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915) de <strong>Self</strong>. O elemento de destino deve ser usado como fonte para essa associação. Isso é útil para associar uma propriedade de um elemento a outra propriedade do mesmo elemento. |
| {RelativeSource TemplatedParent} | Produz um [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391) que é aplicado como a origem dessa associação. Isso é útil para aplicar informações em tempo de execução a associações em nível de modelo. | 

## Comentários

Um [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) pode definir [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) como um atributo em um elemento de objeto **Binding** ou como um componente em uma extensão de marcação [{Binding}](binding-markup-extension.md). É por isso que são exibidas duas sintaxes XAML diferentes.

**RelativeSource** é semelhante à extensão de marcação [{Binding}](binding-markup-extension.md) por ser uma extensão de marcação capaz de retornar instâncias de si, suportando uma construção baseada em cadeias de caracteres que, essencialmente, passa um argumento para o construtor. Nesse caso, o argumento passado é o valor de [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915).

O modo **Self** é útil em casos nos quais o mesmo elemento deve ser usado como objeto de origem e objeto de destino de uma associação, mas diferentes propriedades são a origem e o destino. Isso é útil para associar uma propriedade de um elemento a outra propriedade do mesmo elemento, e é uma variação da associação com [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) que não exige o nome e a autorreferência do elemento. Se você associar uma propriedade de um elemento a outra propriedade do mesmo elemento, as propriedades devem usar o mesmo tipo de propriedade, ou você também terá que usar um [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) na associação para converter os valores. Por exemplo, é possível usar [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) como origem de [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) sem conversão, mas seria necessário um conversor para usar [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) como origem de [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006).

Veja um exemplo a seguir. Esse [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) usa uma [extensão de marcação {Binding}](binding-markup-extension.md) de forma que sua [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) e [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) sejam sempre iguais e ele as renderize como um quadrado. Somente a altura é definida como um valor fixo. Para esse **Rectangle**, seu [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) padrão é **null**, e não **this**. Assim, para estabelecer a origem de contexto de dados como sendo o próprio objeto (e habilitar a associação com suas outras propriedades) usamos o argumento `RelativeSource={RelativeSource Self}` no uso da extensão de marcação {Binding}.

```XAML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Outra técnica que pode ser útil é usar `RelativeSource={RelativeSource Self}` como uma maneira de definir o [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) de um objeto para ele próprio, sendo que a classe [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) foi estendida com uma propriedade personalizada que já está fornecendo um modelo de exibição pronto para sua própria vinculação de dados. Você vê essa técnica em alguns exemplos do SDK: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Observação**  O uso de XAML para **RelativeSource** mostra apenas o uso pretendido: definir um valor para [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) em XAML como parte de uma expressão de associação. Teoricamente, outros usos são possíveis no caso da definição de uma propriedade na qual o valor é [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913).

## Tópicos relacionados

* [Visão geral da XAML](xaml-overview.md)
* [Vinculação de dados em detalhes](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Extensão de marcação {Binding}](binding-markup-extension.md)
* [**Associação**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)



<!--HONumber=Mar16_HO1-->



---
title: XAML condicional
description: Use as novas APIs na marcação XAML e mantenha a compatibilidade com versões anteriores
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c75a6c487fe4a7f7cb56deff869b36309a4b9c7
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8710586"
---
# <a name="conditional-xaml"></a>XAML condicional

A *XAML condicional* fornece uma forma de usar o método [ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent) na marcação XAML. Com isso, você pode definir propriedades e instanciar objetos na marcação com base na presença de uma API sem a necessidade de usar code-behind. Analisa seletivamente elementos ou atributos para determinar se eles estarão disponíveis no momento da execução. Instruções condicionais são avaliadas no momento da execução, e os elementos qualificados com uma marca XAML condicional são analisados se forem avaliados como **true**; caso contrário, são ignorados.

A XAML condicional estará disponível a partir da Atualização para Criadores (versão 1703, compilação 15063). Para usar a XAML condicional, a Versão Mínima do seu projeto do Visual Studio deve ser definida como compilação 15063 (Atualização para Criadores) ou posterior, e a Versão de Destino deve ser definida como uma versão mais recente que a Mínima. Consulte [Aplicativos adaptáveis de versão](version-adaptive-apps.md) para obter informações sobre como configurar seu projeto do Visual Studio.

> [!NOTE]
> Para criar um aplicativo adaptável de versão, uma Versão Mínima inferior à compilação 15063, você deve usar o [código adaptável de versão](version-adaptive-code.md), não a XAML.

Para obter informações de apoio importantes sobre ApiInformation e contratos de API, consulte [Aplicativos adaptáveis de versão](version-adaptive-apps.md).

## <a name="conditional-namespaces"></a>Namespaces condicionais

Para usar um método condicional em XAML, primeiro é preciso declarar um [Namespace XAML](../xaml-platform/xaml-namespaces-and-namespace-mapping.md) condicional na parte superior da sua página. Veja um exemplo de pseudo-código de um namespace condicional:

```xaml
xmlns:myNamespace="schema?conditionalMethod(parameter)"
```

Um namespace condicional pode ser dividido em duas partes pelo delimitador "?". 

- O conteúdo que precede o delimitador indica o namespace ou o esquema que contém a API sendo referenciada. 
- O conteúdo após o delimitador "?" representa o método condicional que determina se o namespace condicional avalia como **true** ou **false**.

Na maioria dos casos, o esquema será o namespace XAML padrão.

```xaml
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
```

A XAML condicional é compatível com os seguintes métodos condicionais:

Método | Inverso
------ | -------
IsApiContractPresent(ContractName, VersionNumber) | IsApiContractNotPresent(ContractName, VersionNumber)
IsTypePresent(ControlType) | IsTypeNotPresent(ControlType)
IsPropertyPresent(ControlType, PropertyName) | IsPropertyNotPresent(ControlType, PropertyName)

Vamos discutir esses métodos em outras seções deste artigo.

> [!NOTE]
> Recomendamos o uso de IsApiContractPresent e IsApiContractNotPresent. Outras condicionais não são compatíveis com a experiência de design do Visual Studio.

## <a name="create-a-namespace-and-set-a-property"></a>Criar um namespace e definir uma propriedade

Neste exemplo, será exibido "Olá, XAML Condicional", como o conteúdo de um bloco de texto se o aplicativo for executado no Fall Creators Update ou versões posteriores, e padrão para nenhum conteúdo se estiver em uma versão anterior.

Primeiro, defina um namespace personalizado com o prefixo 'contract5Present' e use o namespace XAML padrão (http://schemas.microsoft.com/winfx/2006/xaml/presentation) como o esquema contendo a propriedade [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.Text). Para torná-lo um namespace condicional, adicione o delimitador "?" após o esquema.

Depois, defina uma condicional que retorne **true** nos dispositivos que estão executando o Fall Creators Update ou posterior. Use o **IsApiContractPresent** do método ApiInformation para verificar a 5ª versão do UniversalApiContract. A versão 5 do UniversalApiContract foi lançada com o Fall Creators Update (SDK 16299).

```xaml
xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Após a definição do namespace, você coloca o prefixo do namespace na propriedade Text do seu TextBox para qualificá-lo como uma propriedade que deve ser definida de forma condicional no momento da execução.

```xaml
<TextBlock contract5Present:Text="Hello, Conditional XAML"/>
```

Veja a XAML completa.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock contract5Present:Text="Hello, Conditional XAML"/>
    </Grid>
</Page>
```

Ao executar esse exemplo no Fall Creators Update, o texto “Olá, XAML Condicional” é exibido; ao executá-lo na Atualização para Criadores, nenhum texto é exibido.

A XAML condicional permite realizar as verificações de API que, em vez disso, você pode fazer no código da marcação. Eis o código equivalente para essa verificação.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    textBlock.Text = "Hello, Conditional XAML";
}
```

Observe que, embora o método IsApiContractPresent use uma cadeia de caracteres para o parâmetro *contractName*, você não o coloca entre aspas ("") na declaração de namespace de XAML.

## <a name="use-ifelse-conditions"></a>Usar as condições if/else

No exemplo anterior, a propriedade Text é definida apenas quando o aplicativo é executado no Fall Creators Update. Mas e se você quiser mostrar um texto diferente quando ele é executado na Atualização para Criadores? Você poderia tentar definir a propriedade Text sem um qualificador condicional, como este.

```xaml
<!-- DO NOT USE -->
<TextBlock Text="Hello, World" contract5Present:Text="Hello, Conditional XAML"/>
```

Ele funcionará quando for executado na Atualização para Criadores, mas, quando for executado no Fall Creators Update, ocorrerá um erro informando que a propriedade Text é definida mais de uma vez.

Para definir um texto diferente quando o aplicativo é executado em versões diferentes do Windows 10, outra condição é necessária. Uma XAML condicional fornece o inverso de cada método ApiInformation compatível para permitir que você crie cenários condicionais if/else como este.

O método IsApiContractPresent retorna **true** se o dispositivo atual contiver o contrato especificado e o número de versão. Por exemplo, suponha que seu aplicativo está sendo executado na Atualização para Criadores, que tem a 4ª versão do contrato de API universal.

Várias chamadas para IsApiContractPresent apresentariam estes resultados:

- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 5) = **false**
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 4) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 3) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 2) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 1) = true.

IsApiContractNotPresent retorna o inverso de IsApiContractPresent. Várias chamadas para IsApiContractPresent apresentariam estes resultados:

- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 5) = **true**
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 4) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 3) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 2) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 1) = false

Para usar a condição inversa, crie um segundo namespace de XAML condicional que usa o condicional **IsApiContractNotPresent**. Neste exemplo, temos o prefixo "contract5NotPresent."

```xaml
xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"

xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Com os namespaces definidos, você pode definir a propriedade Text duas vezes, contanto que sejam prefixados com qualificadores que garantam que apenas uma configuração de propriedade seja usada no momento da execução, como a seguir:

```xaml
<TextBlock contract5NotPresent:Text="Hello, World"
           contract5Present:Text="Hello, Fall Creators Update"/>
```

Veja outro exemplo que define o plano de fundo de um botão. O recurso [Material acrílico](../design/style/acrylic.md) estará disponível a partir do Fall Creators Update. Assim, você usará o acrílico para o plano de fundo quando o aplicativo for executado no Fall Creators Update. Não estará disponível em versões anteriores. Por isso, nesses casos, você define o plano de fundo como vermelho.

```xaml
<Button Content="Button"
        contract5NotPresent:Background="Red"
        contract5Present:Background="{ThemeResource SystemControlAcrylicElementBrush}"/>
```

## <a name="create-controls-and-bind-properties"></a>Criar controles e propriedades de associação

Até o momento, você já viu como definir as propriedades usando a XAML condicional, mas também é possível instanciar controles de forma condicional com base no contrato de API disponível no momento da execução.

Neste exemplo, um [ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker) é instanciado quando o aplicativo é executado no Fall Creators Update em que o controle está disponível. O ColorPicker não estará disponível antes do Fall Creators Update. Portanto, quando o aplicativo é executado em versões anteriores, use um [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) para fornecer opções de cor simplificadas para o usuário.

```xaml
<contract5Present:ColorPicker x:Name="colorPicker"
                              Grid.Column="1"
                              VerticalAlignment="Center"/>

<contract5NotPresent:ComboBox x:Name="colorComboBox"
                              PlaceholderText="Pick a color"
                              Grid.Column="1"
                              VerticalAlignment="Center">
```

É possível usar qualificadores condicionais com formas diferentes de [Sintaxe da propriedade XAML](../xaml-platform/xaml-syntax-guide.md). Neste exemplo, a propriedade Fill do retângulo é definida usando-se a sintaxe de elemento de propriedade para o Fall Creators Update e usando-se uma sintaxe de atributo para versões anteriores.

```xaml
<Rectangle x:Name="colorRectangle" Width="200" Height="200"
           contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
    <contract5Present:Rectangle.Fill>
        <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
    </contract5Present:Rectangle.Fill>
</Rectangle>
```

Ao associar uma propriedade com outra que depende de um namespace condicional, é preciso usar a mesma condição em ambas as propriedades. Neste exemplo, `colorPicker.Color` depende do namespace condicional "contract5Present". Por isso, também é preciso colocar o prefixo "contract5Present" na propriedade SolidColorBrush.Color. (Ou é possível colocar o prefixo "contract5Present" na propriedade SolidColorBrush em vez da propriedade Color.) Caso isso não seja feito, ocorrerá um erro de tempo de compilação.

```xaml
<SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
```

Esta é a XAML completa que demonstra esses cenários. Este exemplo contém um retângulo e uma interface de usuário que permite definir a cor do retângulo.

Quando o aplicativo é executado no Fall Creators Update, use ColorPicker para que o usuário possa definir a cor. O ColorPicker não estará disponível antes do Fall Creators Update. Portanto, quando o aplicativo é executado em versões anteriores, use um combo box para fornecer opções de cor simplificadas para o usuário.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
    xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Rectangle x:Name="colorRectangle" Width="200" Height="200"
                   contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
            <contract5Present:Rectangle.Fill>
                <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
            </contract5Present:Rectangle.Fill>
        </Rectangle>

        <contract5Present:ColorPicker x:Name="colorPicker"
                                      Grid.Column="1"
                                      VerticalAlignment="Center"/>

        <contract5NotPresent:ComboBox x:Name="colorComboBox"
                                      PlaceholderText="Pick a color"
                                      Grid.Column="1"
                                      VerticalAlignment="Center">
            <ComboBoxItem>Red
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Red"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Blue
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Blue"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Green
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Green"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
        </contract5NotPresent:ComboBox>
    </Grid>
</Page>
```

## <a name="related-articles"></a>Artigos relacionados

- [Guia para aplicativos UWP](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Detectando dinamicamente recursos com contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [Contratos de API](https://channel9.msdn.com/Events/Build/2015/3-733) (Vídeo da Compilação 2015)
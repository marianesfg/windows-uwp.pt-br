---
Description: Diretrizes de layout para formulários em aplicativos da UWP.
title: Formulários
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluente
ms.openlocfilehash: ff071a2a98c533ad7c089b28165f026de00ba68f
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319091"
---
# <a name="forms"></a>Formulários
Um formulário é um grupo de controles que coleta e envia dados de usuários. Os formulários são usados normalmente em páginas de configurações, pesquisas, criação de contas e muito mais. 

Este artigo discute as diretrizes de design para a criação de layouts XAML para formulários.

![Exemplo de formulário](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>Quando devo usar um formulário?
Um formulário é uma página dedicada a coletar entradas de dados que estão claramente relacionadas umas com as outras. Você deve usar um formulário quando precisar coletar explicitamente os dados de um usuário. Você pode criar um formulário para um usuário para:
- Fazer logon em uma conta
- Inscrever-se em uma conta
- Alterar configurações de aplicativo, como privacidade ou opções de exibição
- Fazer uma pesquisa
- Comprar um item
- Enviar comentários

## <a name="types-of-forms"></a>Tipos de formulários

Em relação a como a entrada do usuário é enviada e exibida, há dois tipos de formulários:

### <a name="1-instantly-updating"></a>1. Atualização instantânea
![página de configurações](images/control-examples/toggle-switch-news.png)

Use um formulário de atualização instantânea quando desejar que os usuários vejam imediatamente os resultados da alteração dos valores no formulário. Por exemplo, nas páginas de configurações, as seleções atuais são exibidas e as alterações feitas às seleções são aplicadas imediatamente. Para confirmar as alterações em seu aplicativo, você precisará [adicionar um manipulador de eventos](controls-and-events-intro.md) para cada controle de entrada. Se um usuário altera um controle de entrada, seu aplicativo pode responder adequadamente.

### <a name="2-submitting-with-button"></a>2. Envio com botão
O outro tipo de formulário permite que o usuário escolha quando enviar os dados com um clique de um botão.

![página adicionar novo evento ao calendário](images/calendar-form.png)

Esse tipo de formulário fornece ao usuário flexibilidade para responder. Normalmente, esse tipo de formulário contém mais campos de entrada de formato livre e, portanto, recebe uma maior variedade de respostas. Para garantir entrada de usuário válida e dados formatados corretamente após o envio, considere as seguintes recomendações:

- Não possibilitar o envio de informações inválidas usando o controle correto (ou seja, use um CalendarDatePicker em vez de um TextBox para datas do calendário). Veja mais sobre como selecionar os controles de entrada apropriados no seu formulário na seção de Controles de Entrada.
- Ao usar controles de TextBox, forneça aos usuários uma dica do formato de entrada desejado com a propriedade [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText).
- Forneça aos usuários o teclado virtual adequado informando a entrada esperada de um controle com a propriedade [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope).
- Marque a entrada necessária com um asterisco * no rótulo.
- Desabilite o botão de envio até que todas as informações necessárias sejam preenchidas.
- Se houver dados inválidos após o envio, marque os controles com entrada inválida com campos ou bordas realçadas e solicite que o usuário envie o formulário novamente.
- Para outros erros, como conexão de rede com falha, certifique-se exibir uma mensagem de erro apropriada para o usuário. 


## <a name="layout"></a>Layout

Para facilitar a experiência do usuário e certificar-se de que os usuários sejam capazes de inserir a entrada correta, considere as seguintes recomendações para a criação de layouts de formulários. 

### <a name="labels"></a>Rótulos
Os [rótulos](labels.md) devem estar alinhados à esquerda e ser colocados acima do controle de entrada. Muitos controles têm uma propriedade Header interna para exibir o rótulo. Para controles que não tenham uma propriedade Header ou para rotular grupos de controles, use um [TextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

Para [design com acessibilidade](../accessibility/accessibility.md), rotule todos os indivíduos e grupos de controles para maior clareza tanto para leitores de tela quanto para pessoas. 

Para estilos de fonte, use o padrão [rampa de tipo UWP](../style/typography.md). Use `TitleTextBlockStyle` para títulos de página, `SubtitleTextBlockStyle` para cabeçalhos de grupo e `BodyTextBlockStyle` para rótulos de controle.

<div class="mx-responsive-img">
<table>
<tr><th>Você deve</th><th>Você não deve</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-shortform1col.png" alt="form with top labels"></td>
<td><img src="../controls-and-patterns/images/forms-leftlabel-donot1.png" alt="form with left labels don't"></td>
</tr>
</table>
</div>

### <a name="spacing"></a>Espaçamento
Para separar visualmente os grupos de controles entre si, use [alinhamento, margens e preenchimento](../layout/alignment-margin-padding.md). Os controles de entrada individuais têm 80px de altura e devem ter um espaçamento de 24px. Os grupos de controles de entrada devem ter um espaçamento de 48px.

![grupos de formulários](images/forms-groups.png)

### <a name="columns"></a>Colunas
A criação de colunas pode reduzir o espaço em branco desnecessário em formulários, especialmente os com telas maiores. No entanto, se você quiser criar um formulário de várias colunas, o número de colunas deverá depender do número de controles de entrada na página e do tamanho de tela da janela do aplicativo. Em vez de sobrecarregar a tela com vários controles de entrada, considere a criação de várias páginas para o seu formulário.  

<div class="mx-responsive-img">
<table>
<tr><th>Você deve</th><th>Você não deve</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-2cols.png" alt="form with 2 columns"></td>
<td><img src="../controls-and-patterns/images/forms-2cols-bad.png" alt="form with 2 bad columns"></td>
</tr>
<tr><td><img src="../controls-and-patterns/images/forms-3cols.png" alt="form with 3 columns"></td></tr>
</table>

</div>

### <a name="responsive-layout"></a>Layouts dinâmicos
Os formulários devem ser redimensionados conforme o tamanho da janela ou da tela for alterado para que os usuários não se esqueçam de algum campo de entrada. Para obter mais informações, confira [técnicas de design dinâmico](../layout/responsive-design.md). Por exemplo, você talvez queira manter regiões específicas do formulário sempre em exibição, independentemente do tamanho da tela.

![foco dos formulários](images/forms-focus2.png)

### <a name="tab-stops"></a>Paradas de tabulação
Os usuários podem usar o teclado para navegar pelos controles com as [paradas de tabulação](../input/keyboard-interactions.md#tab-stops). Por padrão, a ordem de tabulação dos controles reflete a ordem na qual eles são criados em XAML. Para substituir o comportamento padrão, altere as propriedades **IsTabStop** ou **TabIndex** do controle. 

![foco de tabulação no controle no formulário](images/forms-focus1.png)

## <a name="input-controls"></a>Controles de entrada
Os controles de entrada são os elementos de interface do usuário que permitem aos usuários inserir informações em formulários. Alguns controles comuns que podem ser adicionados aos formulários estão listados abaixo, bem como informações sobre quando usá-los.

### <a name="text-input"></a>Entrada de texto
Controle | Uso | Exemplo
 - | - | -
[TextBox](text-box.md) | Capturar uma ou várias linhas de texto | Nomes, comentários ou respostas em formato livre
[PasswordBox](password-box.md) | Colete dados particulares ao ocultar caracteres | Senhas, SSN (Número do Seguro Social), PINs, informações de cartão de crédito 
[AutoSuggestBox](auto-suggest-box.md) | Mostre aos usuários uma lista de sugestões de um conjunto correspondente de dados à medida que eles digitam | Pesquisa de banco de dados, enviar email para: linha, consultas anteriores
[RichEditBox](rich-edit-box.md) | Edite arquivos de texto com texto formatado, hiperlinks e imagens | Faça upload do arquivo, visualização e edição no aplicativo

### <a name="selection"></a>Seleção
Controle | Uso | Exemplo
- | - | - 
| [CheckBox](checkbox.md) | Selecione ou anule a seleção de itens de ação | Concorde com os termos e condições, adicione itens opcionais, selecione todas as alternativas aplicáveis
[RadioButton](radio-button.md) | Selecione uma opção entre duas ou mais escolhas | Escolha tipo, método de envio, etc.
[ToggleSwitch](toggles.md) | Escolha uma de duas opções mutuamente exclusivas | Ativar/desativar

> **Observação**: Se houver cinco ou mais itens de seleção, use um controle de lista.

### <a name="lists"></a>Listas
Controle | Uso | Exemplo
- | - | -
[ComboBox](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | Inicie no estado compacto e expanda para mostrar a lista de itens selecionáveis | Selecione de uma longa lista de itens, como estados ou países
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | Categorize itens e atribua cabeçalhos de grupo, arraste e solte itens, corrija conteúdo e reordene os itens | Opções de classificação
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | Organize e procure coleções baseadas em imagem | Escolha uma foto, cor, tema de exibição

### <a name="numeric-input"></a>Entrada numérica
Controle | Uso | Exemplo
- | - | -
[Slider](slider.md) | Selecione um número de um intervalo de valores numéricos contíguos | Porcentagens, volume e velocidade de reprodução
[Classificação](rating.md) | Classificação por estrelas | Comentários do cliente

### <a name="date-and-time"></a>Data e Hora

Controle | Uso 
- | - 
[CalendarView](calendar-view.md) | Selecione uma única data ou um intervalo de datas de um calendário sempre visível 
[CalendarDatePicker](calendar-date-picker.md) | Selecione uma única data de um calendário contextual 
[DatePicker](date-picker.md) | Selecione uma única data localizada quando informações contextuais não forem importantes
[TimePicker](time-picker.md) | Selecione um valor temporal único

### <a name="additional-controls"></a>Controles adicionais 
Para obter uma lista completa de controles da UWP, confira [índice de controles por função](controls-by-function.md).

Para controles de interface do usuário mais complexos e personalizados, confira os recursos da UWP disponíveis de empresas como [Telerik](https://www.telerik.com/), [SyncFusion](https://www.syncfusion.com/uwp-ui-controls), [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [Infragistics](https://www.infragistics.com/products/universal-windows-platform), [ComponentOne](https://www.componentone.com/Studio/Platform/UWP) e [ActiPro](https://www.actiprosoftware.com/products/controls/universal).

## <a name="one-column-form-example"></a>Exemplo de formulário de uma coluna
Este exemplo usa uma [exibição de lista](master-details.md) [mestre/detalhada](lists.md) acrílica e o controle [NavigationView](navigationview.md).
![Captura de tela de outro exemplo de formulário](images/FormExample2.png)
```xaml
<StackPanel>
    <TextBlock Text="New Customer" Style="{StaticResource TitleTextBlockStyle}"/>
    <Button Height="100" Width="100" Background="LightGray" Content="Add photo" Margin="0,24" Click="AddPhotoButton_Click"/>
    <TextBox x:Name="Name" Header= "Name" Margin="0,24,0,0" MaxLength="32" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
    <TextBox x:Name="PhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="15" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
    <TextBox x:Name="Email" Header="Email" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <RelativePanel>
        <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
        <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
             <x:String>WA</x:String>
        </ComboBox>
    </RelativePanel>
    <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Margin="0,24" Click="SaveButton_Click"/>
        <Button Content="Cancel" Margin="24" Click="CancelButton_Click"/>
    </StackPanel>  
</StackPanel>
```

## <a name="two-column-form-example"></a>Exemplo de formulário de duas colunas
Este exemplo usa o controle [Pivô](pivot.md), a tela de fundo [Acrílica](../style/acrylic.md) e o [CommandBar](app-bars.md), além dos controles de entrada.
![Captura de tela de exemplo de formulário](images/FormExample.png)
```xaml
<Grid>
    <Pivot Background="{ThemeResource SystemControlAccentAcrylicWindowAccentMediumHighBrush}" >
        <Pivot.TitleTemplate>
            <DataTemplate>
                <Grid>
                    <TextBlock Text="Company Name" Style="{ThemeResource HeaderTextBlockStyle}"/>
                </Grid>
            </DataTemplate>
        </Pivot.TitleTemplate>
        <PivotItem Header="Orders" Margin="0"/>    
        <PivotItem Header="Customers" Margin="0">
            <!--Form Example-->
            <Grid Background="White">
                <RelativePanel>
                    <StackPanel x:Name="Customer" Margin="20">
                        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="CustomerPhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <RelativePanel>
                            <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="Text" />
                            <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
                                <x:String>WA</x:String>
                            </ComboBox>
                        </RelativePanel>
                        <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
                    </StackPanel>
                    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
                        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="AssociatePhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
                        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
                    </StackPanel>
                </RelativePanel>
            </Grid>
        </PivotItem>
        <PivotItem Header="Invoices"/>
        <PivotItem Header="Stock"/>
        <Pivot.RightHeader>
            <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit" />
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
            </CommandBar>
        </Pivot.RightHeader>
    </Pivot>
</Grid>
```

## <a name="customer-orders-database-sample"></a>Amostra de banco de dados de pedidos de clientes
![captura de tela do banco de dados de pedidos de clientes](images/customerorderform.png) Para saber como conectar entradas de formulário a um banco de dados do **Azure** e ver um formulário totalmente implementado, confira a amostra de aplicativo do [Banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database).

## <a name="related-topics"></a>Tópicos relacionados
- [Controles de entrada](controls-and-events-intro.md)
- [Tipografia](../style/typography.md)

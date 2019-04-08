---
Description: Diretrizes de layout para formulários em aplicativos UWP.
title: Formulários
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 8a57f13e168a248569bca1beeceed7b4f6c89f69
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658151"
---
# <a name="forms"></a>Formulários
Um formulário é um grupo de controles que coletam e enviar dados de usuários. Formulários são usados normalmente para páginas de configurações, pesquisas, criação de contas e muito mais. 

Este artigo discute as diretrizes de design para a criação de layouts XAML de formulários.

![Exemplo de formulário](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>Quando você usar um formulário?
Um formulário é uma página dedicada para coletar entradas de dados que estão claramente relacionadas uns aos outros. Você deve usar um formulário quando você precisar coletar explicitamente os dados de um usuário. Você pode criar um formulário para um usuário:
- Faça logon em uma conta
- Inscreva-se para uma conta
- Alterar configurações de aplicativo, como a privacidade ou opções de exibição
- Faça uma pesquisa
- Comprar um item
- Enviar comentários

## <a name="types-of-forms"></a>Tipos de formulários

Ao pensar sobre como a entrada do usuário é enviada e exibida, há dois tipos de formulários:

### <a name="1-instantly-updating"></a>1. Atualizando instantaneamente
![Página de configurações](images/control-examples/toggle-switch-news.png)

Use um formulário de atualização instantaneamente quando você deseja que os usuários para ver imediatamente os resultados de alteração dos valores no formulário. Por exemplo, nas páginas de configurações, as seleções atuais são exibidas e as alterações feitas às seleções são aplicadas imediatamente. Para confirmar as alterações em seu aplicativo, você precisará [adicionar um manipulador de eventos](controls-and-events-intro.md) para cada controle de entrada. Se um usuário altera um controle de entrada, seu aplicativo pode responder adequadamente.

### <a name="2-submitting-with-button"></a>2. Enviando com o botão
O outro tipo de formulário permite que o usuário escolha quando enviar os dados com um clique de um botão.

![Calendário adicionar nova página de eventos](images/calendar-form.png)

Esse tipo de formulário fornece a flexibilidade de usuário para responder. Normalmente, esse tipo de formulário contém mais campos de entrada de forma livre e, portanto, recebe uma maior variedade de respostas. Para garantir que a entrada de usuário válido e dados formatados corretamente após o envio, considere as seguintes recomendações:

- Tornar impossível enviar informações inválidas, usando o controle correto (ou seja, use um CalendarDatePicker em vez de uma caixa de texto para datas do calendário). Veja mais sobre como selecionar os controles de entrada apropriados no seu formulário na seção de controles de entrada.
- Ao usar controles de caixa de texto, fornecer aos usuários uma dica do formato de entrada desejado com o [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText) propriedade.
- Fornecer aos usuários com os devidos teclado na tela informando a entrada esperada de um controle com o [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope) propriedade.
- Marca necessária entrada com um asterisco * no rótulo.
- Desabilite o botão de envio até que todas as informações necessárias são preenchidas.
- Se não houver dados inválidos após o envio, marcar os controles com uma entrada inválida com campos realçados ou bordas e que o usuário envie o formulário novamente.
- Para outros erros, como conexão de rede com falha, certifique-se exibir uma mensagem de erro apropriado para o usuário. 


## <a name="layout"></a>Layout

Para facilitar a experiência do usuário e certifique-se de que os usuários sejam capazes de inserir a entrada correta, considere as seguintes recomendações para a criação de layouts de formulários. 

### <a name="labels"></a>Rótulos
[Rótulos](labels.md) deve ser alinhado à esquerda e colocado acima do controle de entrada. Muitos controles têm uma propriedade de cabeçalho interna para exibir o rótulo. Para os controles sem uma propriedade Header ou para rotular grupos de controles, você pode utilizar um [TextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

Para [design de acessibilidade](../accessibility/accessibility.md), todos os individuais e grupos de controles para maior clareza para ambos os humanos e leitores de tela de rótulo. 

Para estilos de fonte, use o padrão [rampa de tipo UWP](../style/typography.md). Use `TitleTextBlockStyle` títulos de página, `SubtitleTextBlockStyle` de cabeçalhos de grupo, e `BodyTextBlockStyle` para rótulos de controle.

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
Para separar visualmente os grupos de controles entre si, use [alinhamento, margens e preenchimento](../layout/alignment-margin-padding.md). Controles de entrada individuais são 80px de altura e devem ser espaçados 24px distância. Grupos de controles de entrada devem ser espaçados 48px de distância.

![grupos de formulários](images/forms-groups.png)

### <a name="columns"></a>Colunas
Criação de colunas pode reduzir o espaço em branco desnecessário em formulários, especialmente com os maiores tamanhos de tela. No entanto, se você quiser criar um formulário de várias coluna, o número de colunas deve depender o número de controles na página de entrada e o tamanho de tela da janela do aplicativo. Em vez de sobrecarregar a tela com vários controles de entrada, considere a criação de várias páginas para o seu formulário.  

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

### <a name="responsive-layout"></a>Layout dinâmico
Formulários devem ser redimensionado como as alterações de tamanho de tela ou janela, para que os usuários não se esqueçam quaisquer campos de entrada. Para obter mais informações, consulte [técnicas de design dinâmico](../layout/responsive-design.md). Por exemplo, você talvez queira manter regiões específicas do formulário sempre no modo de exibição, independentemente do tamanho da tela.

![foco de formulários](images/forms-focus2.png)

### <a name="tab-stops"></a>Paradas de tabulação
Os usuários podem usar o teclado para navegar com os controles [paradas de tabulação](../input/keyboard-interactions.md#tab-stops). Por padrão, a ordem de tabulação dos controles reflete a ordem na qual eles são criados em XAML. Para substituir o comportamento padrão, altere o **IsTabStop** ou **TabIndex** propriedades do controle. 

![foco de tabulação no controle no formulário](images/forms-focus1.png)

## <a name="input-controls"></a>Controles de entrada
Controles de entrada são os elementos de interface do usuário que permitem aos usuários inserir informações em formulários. Alguns controles comuns que podem ser adicionados a formulários estão listadas abaixo, bem como informações sobre quando usá-los.

### <a name="text-input"></a>Entrada de texto
Controle | Uso | Exemplo
 - | - | -
[TextBox](text-box.md) | Capturar uma ou várias linhas de texto | Nomes, as respostas de forma livre ou comentários
[PasswordBox](password-box.md) | Coletar dados particulares, concedendo os caracteres | Pinos de senhas, números do seguro Social (SSN), informações de cartão de crédito 
[AutoSuggestBox](auto-suggest-box.md) | Mostrar aos usuários uma lista de sugestões de um conjunto correspondente de dados enquanto digitam | Pesquisa de banco de dados, de email para: de linha, consultas anteriores
[RichEditBox](rich-edit-box.md) | Editar arquivos de texto com texto formatado, hiperlinks e imagens | Carregar arquivo, visualização e edição no aplicativo

### <a name="selection"></a>Seleção
Controle | Uso | Exemplo
- | - | - 
| [Caixa de seleção](checkbox.md) | Marque ou desmarque uma ou mais itens de ação | Concordar com os termos e condições, adicionar itens opcionais, selecione todas as alternativas aplicáveis
[Botão de opção](radio-button.md) | Selecione uma opção de duas ou mais opções | Escolha tipo de envio de método, etc.
[ToggleSwitch](toggles.md) | Escolha uma das duas opções mutuamente exclusivas | Ativar/desativar

> **Observação**: Se houver cinco ou mais itens de seleção, use um controle de lista.

### <a name="lists"></a>Listas
Controle | Uso | Exemplo
- | - | -
[ComboBox](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | Iniciar em estado compact e expanda para mostrar a lista de itens selecionáveis | Selecione de uma longa lista de itens, como estados ou países
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | Categorizar itens e atribuir os cabeçalhos de grupo, arraste e solte itens de, organizar o conteúdo e reordenar os itens | Opções de classificação
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | Organizar e procurar coleções baseadas em imagem | Escolher uma foto, cor, a exibição de tema

### <a name="numeric-input"></a>Entrada numérica
Controle | Uso | Exemplo
- | - | -
[Slider](slider.md) | Selecione um número em um intervalo de valores contíguos de numérico | Porcentagens, o volume e velocidade de reprodução
[Classificação](rating.md) | Taxa de estrelas | Comentários do cliente

### <a name="date-and-time"></a>Data e Hora

Controle | Uso 
- | - 
[Exibição de calendário](calendar-view.md) | Escolher uma única data ou um intervalo de datas a partir de um calendário sempre visível 
[CalendarDatePicker](calendar-date-picker.md) | Escolha uma data única de um calendário contextual 
[DatePicker](date-picker.md) | Escolher uma única data localizada quando informações contextuais não não importante
[TimePicker](time-picker.md) | Selecione um valor único de tempo

### <a name="additional-controls"></a>Controles adicionais 
Para obter uma lista completa de controles UWP, consulte [índice de controles por função](controls-by-function.md).

Para controles de interface do usuário mais complexos e personalizados, examinar os UWP recursos disponíveis de empresas, como [Telerik](https://www.telerik.com/), [SyncFusion](https://www.syncfusion.com/products/uwp), [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [ A Infragistics](https://www.infragistics.com/products/universal-windows-platform), [ComponentOne](https://www.componentone.com/Studio/Platform/UWP), e [ActiPro](https://www.actiprosoftware.com/products/controls/universal).

## <a name="one-column-form-example"></a>Exemplo de formulário de uma coluna
Este exemplo usa uma tinta Acrílica [mestre/detalhes](master-details.md) [exibição de lista](lists.md) e [NavigationView](navigationview.md) controle.
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
Este exemplo usa o [Pivot](pivot.md) controle, [tinta Acrílica](../style/acrylic.md) em segundo plano, e [CommandBar](app-bars.md) além dos controles de entrada.
![Captura de tela do exemplo de formulário](images/FormExample.png)
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

## <a name="customer-orders-database-sample"></a>Exemplo de banco de dados de pedidos do cliente
![captura de tela do banco de dados de pedidos de clientes](images/customerorderform.png) para aprender a conectar-se a entrada de formulário para um **Azure** de banco de dados e ver um formulário totalmente implementado, consulte o [banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) aplicativo de exemplo.

## <a name="related-topics"></a>Tópicos relacionados
- [Controles de entrada](controls-and-events-intro.md)
- [Tipografia](../style/typography.md)

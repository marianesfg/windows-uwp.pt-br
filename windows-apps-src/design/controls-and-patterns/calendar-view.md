---
Description: Uma exibição de calendário permite que um usuário visualize e interaja com um calendário em que ele pode navegar por mês, ano ou década.
title: Exibição de calendário
ms.assetid: d8ec5ba8-7a9d-405d-a1a5-5a1b502b9e64
label: Calendar view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 08fac0fedaa3f01ad362a57a6db3abf428771258
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081260"
---
# <a name="calendar-view"></a>Exibição de calendário

Uma exibição de calendário permite que um usuário visualize e interaja com um calendário em que ele pode navegar por mês, ano ou década. Um usuário pode selecionar uma única data ou um intervalo de datas. Ele não tem uma superfície de seletor e o calendário está sempre visível.

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | A Biblioteca de interface do usuário do Windows 2.2 ou posterior inclui um novo modelo para esse controle que usa cantos arredondados. Para obter mais informações, confira [Raio de canto](/windows/uwp/design/style/rounded-corner). WinUI é um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, confira [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs de plataforma:**  [classe CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [evento SelectedDatesChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?
Use um modo de exibição de calendário para permitir que um usuário selecione uma única data ou um intervalo de datas de um calendário sempre visível.

Se precisar permitir que um usuário selecione várias datas ao mesmo tempo, você deve usar um modo de exibição de calendário. Se você precisar permitir que um usuário selecione apenas uma única data e não precisar de um calendário sempre visível, considere usar um [seletor de data do calendário](calendar-date-picker.md) ou um controle [seletor de data](date-picker.md).

Para obter mais informações sobre como escolher o controle correto, consulte o artigo [Controles de data e hora](date-and-time.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/CalendarView">abrir o aplicativo e ver o CalendarView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

A exibição de calendário é composta de três visões separadas: a visão de mês, a visão de ano e a visão de década. Por padrão, ele abre com o modo de exibição de mês. Você pode especificar um modo de exibição de inicialização definindo a propriedade [DisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.displaymode).

![Os três modos de exibição de um modo de exibição de calendário](images/calendar-view-3-views.png)

Os usuários clicam no cabeçalho na exibição de mês para abrir o modo de exibição de ano, e clicam no cabeçalho na exibição de ano para abrir o modo de exibição de década. Os usuários escolhem um ano na exibição de década para retornar à exibição de ano, e escolhem um mês na exibição de ano para retornar ao modo de exibição de mês. As duas setas ao lado do cabeçalho navegam para frente ou para trás por mês, ano ou década.

## <a name="create-a-calendar-view"></a>Criar um modo de exibição de calendário

Este exemplo mostra como criar um modo de exibição de calendário simples.

```xaml
<CalendarView/>
```

O modo de exibição de calendário resultante fica assim:

![Exemplo de exibição de calendário](images/controls_calendar_monthview.png)

### <a name="selecting-dates"></a>Selecionando datas

Por padrão, a propriedade [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode) é definida como **Single**. Isso permite que um usuário escolha uma única data no calendário. Defina SelectionMode como **Nenhum** para desabilitar a seleção de data.

Defina SelectionMode como **Múltiplo** para permitir que um usuário selecione várias datas. Você pode selecionar várias datas programaticamente, adicionando objetos [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime)/[DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset) à coleção [SelectedDates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates), conforme mostrado aqui:

```csharp
calendarView1.SelectedDates.Add(DateTimeOffset.Now);
calendarView1.SelectedDates.Add(new DateTime(1977, 1, 5));
```

Um usuário poderá desmarcar uma data selecionada, clicando ou tocando nela na grade do calendário.

Você pode manipular o evento [SelectedDatesChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged) para ser notificado quando a coleção [SelectedDates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates) for alterada.

> [!NOTE]
> Para obter informações importantes sobre valores de data, consulte [Valores DateTime e Calendar](date-and-time.md#datetime-and-calendar-values) no artigo Controles de data e hora.

### <a name="customizing-the-calendar-views-appearance"></a>Personalizando a aparência do modo de exibição de calendário

O modo de exibição de calendário é composto de elementos XAML definidos em ControlTemplate e elementos visuais renderizados diretamente pelo controle.
- Os elementos XAML definidos no modelo de controle incluem a borda que envolve o controle, o cabeçalho, os botões Anterior e Próximo e elementos DayOfWeek. Você pode definir o estilo e o novo modelo desses elementos como qualquer controle XAML.
- A grade de calendário é composta de objetos [CalendarViewDayItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem). Não é possível definir o estilo ou o novo modelo desses elementos, mas são fornecidas várias propriedades para permitir que você personalize a aparência deles.

Este diagrama mostra os elementos que compõem o modo de exibição de mês do calendário. Para obter mais informações, confira os Comentários sobre a classe [CalendarViewDayItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem).

![Os elementos de um modo de exibição de mês de calendário](images/calendar-view-month-elements.png)

Esta tabela lista as propriedades que você pode alterar para modificar a aparência de elementos de calendário.

Elemento | Propriedades
--------|-----------
DayOfWeek | [DayOfWeekFormat](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayofweekformat)
CalendarItem | [CalendarItemBackground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendaritembackground), [CalendarItemBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderbrush), [CalendarItemBorderThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderthickness), [CalendarItemForeground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemforeground)
DayItem | [DayItemFontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontfamily), [DayItemFontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontsize), [DayItemFontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontstyle), [DayItemFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontweight), [HorizontalDayItemAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.horizontaldayitemalignment), [VerticalDayItemAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.verticaldayitemalignment), [CalendarViewDayItemStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemstyle)
MonthYearItem (nos modos de exibição de ano e de década, equivalentes a DayItem) | [MonthYearItemFontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontfamily), [MonthYearItemFontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontsize), [MonthYearItemFontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontstyle), [MonthYearItemFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontweight)
FirstOfMonthLabel | [FirstOfMonthLabelFontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontfamily), [FirstOfMonthLabelFontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontsize), [FirstOfMonthLabelFontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontstyle), [FirstOfMonthLabelFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontweight), [HorizontalFirstOfMonthLabelAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.horizontalfirstofmonthlabelalignment), [VerticalFirstOfMonthLabelAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.verticalfirstofmonthlabelalignment), [IsGroupLabelVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.isgrouplabelvisible)
FirstofYearDecadeLabel (nos modos de exibição de ano e de década, equivalentes a FirstOfMonthLabel) | [FirstOfYearDecadeLabelFontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontfamily), [FirstOfYearDecadeLabelFontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontsize), [FirstOfYearDecadeLabelFontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontstyle), [FirstOfYearDecadeLabelFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontweight)
Bordas de Estado Visual | [FocusBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.focusborderbrush), [HoverBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.hoverborderbrush), [PressedBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.pressedborderbrush), [SelectedBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectedborderbrush), [SelectedForeground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectedforeground), [SelectedHoverBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectedhoverborderbrush), [SelectedPressedBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectedpressedborderbrush)
OutofScope | [IsOutOfScopeEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.isoutofscopeenabled), [OutOfScopeBackground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.outofscopebackground), [OutOfScopeForeground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.outofscopeforeground)
Hoje | [IsTodayHighlighted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.istodayhighlighted), [TodayFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.todayfontweight), [TodayForeground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.todayforeground)

 Por padrão, o modo de exibição de mês mostra 6 semanas de cada vez. Você pode alterar o número de semanas mostrado, definindo a propriedade [NumberOfWeeksInView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.numberofweeksinview). O número mínimo de semanas para mostrar é 2; o máximo, 8.

Por padrão, os modos de exibição de ano e de década aparecem em uma grade de 4 x 4. Para alterar o número de linhas ou colunas, chame [SetYearDecadeDisplayDimensions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.setyeardecadedisplaydimensions) com o número desejado de linhas e colunas. Isso irá alterar a grade para os modos de exibição de ano e de década.

Aqui, os modos de exibição de ano e de década são definidos para aparecer em uma grade de 3 x 4.

```csharp
calendarView1.SetYearDecadeDisplayDimensions(3, 4);
```

Por padrão, a data mínima mostrada na exibição de calendário é 100 anos antes da data atual, e a data máxima mostrada é 100 anos após a data atual. Você pode alterar as datas mínima e máxima que o calendário mostra definindo as propriedades [MinDate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.mindate) e [MaxDate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.maxdate).

```csharp
calendarView1.MinDate = new DateTime(2000, 1, 1);
calendarView1.MaxDate = new DateTime(2099, 12, 31);
```

### <a name="updating-calendar-day-items"></a>Atualizando itens de dia do calendário

Cada dia no calendário é representado por um objeto [CalendarViewDayItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem). Para acessar um item individual de dia e usar seus métodos e propriedades, manipule o evento [CalendarViewDayItemChanging](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemchanging) e use a propriedade Item de argumentos de evento para acessar o CalendarViewDayItem.

Você pode fazer um dia não selecionável na exibição de calendário definindo sua propriedade [CalendarViewDayItem.IsBlackout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.isblackout) como **true**.

Você pode mostrar informações contextuais sobre a densidade de eventos em um dia chamando o método [CalendarViewDayItem.SetDensityColors](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.setdensitycolors). Você pode mostrar de 0 a 10 barras de densidade para cada dia, e definir a cor de cada barra.

Veja alguns itens de dia em um calendário. Dias 1 e 2 são escurecidos. Dias 2, 3 e 4 têm várias definições de barras de densidade.

![Dias de calendário com barras de densidade](images/calendar-view-density-bars.png)

### <a name="phased-rendering"></a>Renderização em fases

Um modo de exibição de calendário pode conter um grande número de objetos CalendarViewDayItem. Para manter a interface do usuário rápida e habilitar uma navegação suave pelo calendário, o modo de exibição de calendário suporta renderização em fases. Essa função permite que você separe o processamento de um item de dia em fases. Se um dia for movido para fora da exibição antes que todas as fases sejam concluídas, não será usado mais tempo para processar e renderizar aquele item.

Este exemplo mostra a renderização em fases de uma exibição de calendário para agendar compromissos.
- Na fase 0, o item dia padrão é renderizado.
- Na fase 1, você escurece datas que não podem ser registradas. Isso inclui datas passadas, domingos e datas que já estão totalmente reservadas.
- Na fase 2, você verifica cada compromisso que está reservado para o dia. Você exibe uma barra de densidade verde para cada compromisso confirmado e uma barra de densidade azul para cada compromisso provisório.

A classe `Bookings` neste exemplo é de um aplicativo de reserva de compromisso fictício, e não é exibida.

```xaml
<CalendarView CalendarViewDayItemChanging="CalendarView_CalendarViewDayItemChanging"/>
```

```csharp
private void CalendarView_CalendarViewDayItemChanging(CalendarView sender,
                                   CalendarViewDayItemChangingEventArgs args)
{
    // Render basic day items.
    if (args.Phase == 0)
    {
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set blackout dates.
    else if (args.Phase == 1)
    {
        // Blackout dates in the past, Sundays, and dates that are fully booked.
        if (args.Item.Date < DateTimeOffset.Now ||
            args.Item.Date.DayOfWeek == DayOfWeek.Sunday ||
            Bookings.HasOpenings(args.Item.Date) == false)
        {
            args.Item.IsBlackout = true;
        }
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set density bars.
    else if (args.Phase == 2)
    {
        // Avoid unnecessary processing.
        // You don't need to set bars on past dates or Sundays.
        if (args.Item.Date > DateTimeOffset.Now &&
            args.Item.Date.DayOfWeek != DayOfWeek.Sunday)
        {
            // Get bookings for the date being rendered.
            var currentBookings = Bookings.GetBookings(args.Item.Date);

            List<Color> densityColors = new List<Color>();
            // Set a density bar color for each of the days bookings.
            // It's assumed that there can't be more than 10 bookings in a day. Otherwise,
            // further processing is needed to fit within the max of 10 density bars.
            foreach (booking in currentBookings)
            {
                if (booking.IsConfirmed == true)
                {
                    densityColors.Add(Colors.Green);
                }
                else
                {
                    densityColors.Add(Colors.Blue);
                }
            }
            args.Item.SetDensityColors(densityColors);
        }
    }
}
```

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Controles de data e hora](date-and-time.md)
- [Seletor de data do calendário](calendar-date-picker.md)
- [Seletor de data](date-picker.md)
- [Seletor de hora](time-picker.md)

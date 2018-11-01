---
author: muhsinking
Description: The calendar date picker is a drop down control that’s optimized for picking a single date from a calendar view where contextual information like the day of the week or fullness of the calendar is important.
title: Seletor de data do calendário
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6756d8c64f33de8d16d6aa455c3fad694a6306d5
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5875693"
---
# <a name="calendar-date-picker"></a>Seletor de data do calendário

 

O seletor de data do calendário é um controle suspenso otimizado para escolher uma única data em uma exibição de calendário onde informações contextuais como dia da semana ou integridade do calendário são importantes. Você pode modificar o calendário para fornecer contexto adicional ou limitar as datas disponíveis.

> **APIs importantes**: [classe CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx), [Propriedade de dados](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx), [evento DateChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx)


## <a name="is-this-the-right-control"></a>Este é o controle correto?
Use um **seletor de data do calendário** para permitir que um usuário selecione uma única data de um modo de exibição de calendário contextual. Use-o para coisas como escolher a data de um compromisso ou de partida.

Para permitir que um usuário selecione uma data conhecida, como uma data de nascimento, onde o contexto do calendário não é importante, considere usar um [seletor de data](date-picker.md).

Para obter mais informações sobre como escolher o controle correto, consulte o artigo [Controles de data e hora](date-and-time.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/CalendarDatePicker">abrir o aplicativo e ver o CalendarDatePicker em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Baixe o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

O ponto de entrada exibirá o texto de espaço reservado se uma data não tiver sido definida; caso contrário, ele exibirá a data escolhida. Quando o usuário seleciona o ponto de entrada, uma exibição de calendário se expande para que o usuário faça uma seleção de data. A exibição de calendário se sobrepõe à outra interface do usuário; ela não remove a outra interface do usuário.

![Exemplo de seletor de data do calendário](images/calendar-date-picker-2-views.png)

## <a name="create-a-date-picker"></a>Criar um seletor de data

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

O seletor de data do calendário resultante tem esta aparência:

![Exemplo de seletor de data do calendário](images/calendar-date-picker-closed.png)

O seletor de data do calendário tem um [CalendarView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) interno para selecionar uma data. Há um subconjunto das propriedades CalendarView, como [IsTodayHighlighted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted.aspx) e [FirstDayOfWeek](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.firstdayofweek.aspx), no CalendarDatePicker e elas são encaminhadas para CalendarView interno para permitir que você o modifique. 

No entanto, você não pode alterar o [SelectionMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx) do CalendarView interno para permitir seleção múltipla. Se você precisa permitir que o usuário selecione várias datas ou de um calendário que esteja sempre visível, considere usar um modo de exibição de calendário em vez de um seletor de data do calendário. Consulte o artigo [Calendar view](calendar-view.md) para saber mais sobre como você pode modificar a exibição de calendário.

### <a name="selecting-dates"></a>Selecionando datas

Use a propriedade [Data](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx) para obter ou definir a data selecionada. Por padrão, a propriedade Date é **null**. Quando um usuário seleciona uma data na exibição de calendário, essa propriedade é atualizada. Um usuário pode limpar a data clicando na data selecionada na exibição de calendário para desmarcá-la. 

Você pode definir a data no seu código assim.

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

Quando você define a data em código, o valor é restringido pelas propriedades [MinDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.mindate.aspx) e [MaxDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.maxdate.aspx).
- Se **Date** é menor que **MinDate**, o valor é definido como **MinDate**.
- Se **Date** é maior que **MaxDate**, o valor é definido como **MaxDate**.

Você pode manipular o evento [DateChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx) para ser notificado quando o valor Date for alterado.

> [!NOTE]
Para obter informações importantes sobre valores de data, consulte [Valores DateTime e Calendar](date-and-time.md#datetime-and-calendar-values) no artigo Controles de data e hora.

### <a name="setting-a-header-and-placeholder-text"></a>Definindo um texto de cabeçalho e o espaço reservado

Você pode adicionar um [Header](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.header.aspx) (ou rótulo) e um [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.placeholdertext.aspx) (ou marca d'água) para o seletor de data do calendário fornecer ao usuário uma indicação de para que ele é usado. Para personalizar a aparência do cabeçalho, você pode definir a propriedade [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.headertemplate.aspx) em vez de Header.

O texto de espaço reservado padrão é "selecionar uma data". Você pode remover isso definindo a propriedade PlaceholderText como uma cadeia de caracteres vazia ou pode fornecer texto personalizado conforme mostrado aqui.

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" 
                    PlaceholderText="Choose your arrival date"/>
```

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Controles de data e hora](date-and-time.md)
- [Exibição de Calendário](calendar-view.md)
- [Seletor de data](date-picker.md)
- [Seletor de hora](time-picker.md)

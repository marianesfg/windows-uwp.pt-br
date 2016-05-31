---
author: Jwmsft
Description: O seletor de data do calendário é um controle suspenso otimizado para escolher uma única data em uma exibição de calendário onde informações contextuais como dia da semana ou integridade do calendário são importantes.
title: Seletor de data do calendário
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
---

# Seletor de data do calendário

O seletor de data do calendário é um controle suspenso otimizado para escolher uma única data em uma exibição de calendário onde informações contextuais como dia da semana ou integridade do calendário são importantes. Você pode modificar o calendário para fornecer contexto adicional ou limitar as datas disponíveis.

<span class="sidebar_heading" style="font-weight: bold;">APIs importantes</span>

-   [**Classe TimePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)
-   [**Propriedade Time**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)

## Este é o controle correto?
Use um **seletor de data do calendário** para permitir que um usuário selecione uma única data de um modo de exibição de calendário contextual. Use-o para coisas como escolher a data de um compromisso ou de partida.

Para permitir que um usuário selecione uma data conhecida, como uma data de nascimento, onde o contexto do calendário não é importante, considere usar um [**seletor de data**](date-picker.md).

Para obter mais informações sobre como escolher o controle correto, consulte o artigo [Controles de data e hora](date-and-time.md).

## Exemplos

O ponto de entrada exibe o texto de espaço reservado se uma data não tiver sido definida. Caso contrário, ele exibe a data escolhida. Quando o usuário seleciona o ponto de entrada, uma visão de calendário se expande para que o usuário faça uma seleção de data. A exibição de calendário substitui outras interfaces do usuário; ela não as tira do caminho.

![Exemplo de seletor de data do calendário](images/calendar-date-picker-2-views.png)

## Criar um seletor de data

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

O seletor de data do calendário resultante tem esta aparência:

![Exemplo de seletor de data do calendário](images/calendar-date-picker-closed.png)

O seletor de data do calendário tem um [**CalendarView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) interno para selecionar uma data. Há um subconjunto das propriedades CalendarView, como [**IsTodayHighlighted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted.aspx) e [**FirstDayOfWeek**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.firstdayofweek.aspx), no CalendarDatePicker e elas são encaminhadas para CalendarView interno para permitir que você o modifique. 

No entanto, você não pode alterar o [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx) do CalendarView interno para permitir seleção múltipla. Se você precisa permitir que o usuário selecione várias datas ou de um calendário que esteja sempre visível, considere usar um modo de exibição de calendário em vez de um seletor de data do calendário. Consulte o artigo [Calendar view](calendar-view.md) para saber mais sobre como você pode modificar a exibição de calendário.

### Selecionando datas

Use a propriedade [**Data**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx) para obter ou definir a data selecionada. Por padrão, a propriedade Date é **null**. Quando um usuário seleciona uma data na exibição de calendário, essa propriedade é atualizada. Um usuário pode limpar a data clicando na data selecionada na exibição de calendário para desmarcá-la. 

Você pode definir a data no seu código assim.

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

Quando você define a data em código, o valor é restringido pelas propriedades [**MinDate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.mindate.aspx) e [**MaxDate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.maxdate.aspx).
- Se **Date** é menor que **MinDate**, o valor é definido como **MinDate**.
- Se **Date** é maior que **MaxDate**, o valor é definido como **MaxDate**.

Você pode manipular o evento [**DateChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx) para ser notificado quando o valor Date for alterado.

> **Observação**
            &nbsp;&nbsp;Para obter informações importantes sobre valores de data, consulte [Valores DateTime e Calendar](date-and-time.md#datetime-and-calendar-values) no artigo sobre os controles de Data e Hora.

### Definindo um texto de cabeçalho e o espaço reservado

Você pode adicionar um [**Header**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.header.aspx) (ou rótulo) e um [**PlaceholderText**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.placeholdertext.aspx) (ou marca d'água) para o seletor de data do calendário fornecer ao usuário uma indicação de para que ele é usado. Para personalizar a aparência do cabeçalho, você pode definir a propriedade [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.headertemplate.aspx) em vez de Header.

O texto de espaço reservado padrão é "selecionar uma data". Você pode remover isso definindo a propriedade PlaceholderText como uma cadeia de caracteres vazia ou pode fornecer texto personalizado conforme mostrado aqui.

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" PlaceholderText="Choose your arrival date"/>
```


## Artigos relacionados

- [Controles de data e hora](date-and-time.md)
- [Exibição de Calendário](calendar-view.md)
- [Seletor de data](date-picker.md)
- [Seletor de hora](time-picker.md)


<!--HONumber=May16_HO2-->



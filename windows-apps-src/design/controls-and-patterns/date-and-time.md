---
author: muhsinking
Description: Date and time controls let you view and set the date and time. This article provides design guidelines and helps you pick the right control.
title: Diretrizes para os controles de data e hora
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
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
ms.openlocfilehash: 363c60a9aeaaa98d808e015f96e4096cf889a501
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5977966"
---
# <a name="calendar-date-and-time-controls"></a>Controles de calendário, data e hora

 

Os controles de data e hora oferecem formas padrão e localizadas para permitir que um usuário exiba e defina valores de data e hora em seu aplicativo. Este artigo fornece diretrizes de design e ajuda você a escolher o controle correto.

> **APIs importantes**: [CalendarView classe](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx), [classe CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx), [classe DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx), [classe TimePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/category/DataInput">abrir o aplicativo e ver estes controles em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="which-date-or-time-control-should-you-use"></a>Qual controle de data ou hora você deve usar?

Há quatro controles de data e hora para você escolher; o controle que você usa depende do cenário. Use essas informações para selecionar o controle correto para usar em seu aplicativo.

&nbsp;|&nbsp;|&nbsp;                                                                                                                      
--------------------|-------|-------------------------------------------------------------------------------------------------------------------------------
Exibição de calendário       |![Exemplo de exibição de calendário](images/controls_calendar_monthview_small.png)|Use-a para selecionar uma única data ou um intervalo de datas de um calendário sempre visível.                   
Seletor de data do calendário|![Exemplo de seletor de data do calendário](images/calendar-date-picker-closed.png)|Use-o para selecionar uma única data de um calendário contextual. 
Seletor de data         |![Exemplo de seletor de data](images/date-picker-closed.png)|Use-o para selecionar uma única data conhecida quando informações contextuais não forem importantes.
Seletor de hora         |![Exemplo de seletor de hora](images/time-picker-closed.png)|Use-o para selecionar um valor de hora único.                                        

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>Exibição de calendário

**CalendarView** permite que um usuário visualize e interaja com um calendário em que ele pode navegar por mês, ano ou década. Um usuário pode selecionar uma única data ou um intervalo de datas. Ele não tem uma superfície de seletor e o calendário está sempre visível.

A exibição de calendário é composta de três visões separadas: a visão de mês, a visão de ano e a visão de década. Por padrão, ele é iniciado com a visão de mês aberta, mas você pode especificar qualquer visão como a visão de inicialização.

![Exemplo de seletor de data do calendário](images/calendar-view-3-views.png)

- Se precisar permitir que o usuário selecione várias datas, você deverá usar um **CalendarView**.
- Se você precisar permitir que o usuário selecione apenas uma única data e não precisar de um calendário sempre visível, considere usar um controle **CalendarDatePicker** ou **DatePicker**.

### <a name="calendar-date-picker"></a>Seletor de data do calendário

**CalendarDatePicker** é um controle suspenso que é otimizado para selecionar uma única data em um modo de exibição de calendário, onde informações contextuais como o dia da semana ou o preenchimento do calendário são importantes. Você pode modificar o calendário para fornecer contexto adicional ou limitar as datas disponíveis.

O ponto de entrada exibirá o texto de espaço reservado se uma data não tiver sido definida; caso contrário, ele exibirá a data escolhida. Quando o usuário seleciona o ponto de entrada, uma exibição de calendário se expande para que o usuário faça uma seleção de data. A exibição de calendário se sobrepõe à outra interface do usuário; ela não remove a outra interface do usuário.

![Exemplo de seletor de data do calendário](images/calendar-date-picker-2-views.png)

- Use um seletor de data do calendário para itens como escolher um compromisso ou a data de partida. 

### <a name="date-picker"></a>Seletor de data

O controle **DatePicker** oferece uma maneira padronizada para escolher uma data específica. 

O ponto de entrada mostra a data escolhida e, quando o usuário seleciona o ponto de entrada, uma superfície de seletor se expande verticalmente do meio para que o usuário faça uma seleção. O seletor de data se sobrepõe à outra interface do usuário; ele não remove a outra interface do usuário.

![Exemplo da expansão do seletor de data](images/controls_datepicker_expand.png)

- Use um seletor de data para permitir que um usuário selecione uma data conhecida, como uma data de nascimento, em que o contexto do calendário não é importante.

### <a name="time-picker"></a>Seletor de hora

O **TimePicker** é usado para selecionar um valor de hora único para itens como compromissos ou uma hora de partida. É uma exibição estática que é definida pelo usuário ou no código, mas não é atualizada para exibir a hora atual. 

O ponto de entrada mostra a hora escolhida e, quando o usuário seleciona o ponto de entrada, uma superfície de seletor se expande verticalmente do meio para que o usuário faça uma seleção. O seletor de hora se sobrepõe a outra interface do usuário; ele não remove a outra interface do usuário.

![Exemplo da expansão do seletor de hora](images/controls_timepicker_expand.png)

- Use um seletor de hora para permitir que um usuário selecione um valor de hora único.

## <a name="create-a-date-or-time-control"></a>Criar um controle de data ou hora

Consulte estes artigos para obter informações e exemplos específicos para cada controle de data e hora.

- [Exibição de calendário](calendar-view.md)
- [Seletor de data do calendário](calendar-date-picker.md)
- [Seletor de data](date-picker.md)
- [Seletor de hora](time-picker.md)

### <a name="globalization"></a>Globalização

Os controles de data de XAML são compatíveis com todos os sistemas de calendário com suporte no Windows. Esses calendários são especificados na classe [Windows.Globalization.CalendarIdentifiers](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendaridentifiers.aspx). Cada controle usa o calendário correto para o idioma padrão de seu aplicativo, ou você pode definir a propriedade **CalendarIdentifier** para usar um sistema de calendário específico.

O controle seletor de hora oferece suporte a todos os sistemas de relógio especificados na classe [Windows.Globalization.ClockIdentifiers](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.clockidentifiers.aspx). Você pode definir a propriedade [ClockIdentifier](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.clockidentifier.aspx) para usar um relógio de 12 horas ou 24 horas. O tipo da propriedade é String, mas você deve usar valores que correspondam às propriedades de cadeia de caracteres estática da classe ClockIdentifiers. Eles são: TwelveHour (a cadeia de caracteres "12HourClock") e TwentyFourHour (a cadeia de caracteres "24HourClock"). O valor padrão é "12HourClock".


### <a name="datetime-and-calendar-values"></a>Valores DateTime e Calendar

Os objetos de data usados nos controles de data e hora de XAML têm uma representação diferente, dependendo de sua linguagem de programação. 
- O C# e o Visual Basic usam a estrutura [System.DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx) que faz parte do .NET. 
- O C++/CX usa a estrutura [Windows::Foundation::DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/br205770.aspx). 

Um conceito relacionado é a classe Calendar, que influencia como as datas são interpretadas em contexto. Todos os aplicativos do Windows Runtime podem usar a classe [Windows.Globalization.Calendar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendar.aspx). Os aplicativos em C# e Visual Basic também podem usar a classe [System.Globalization.Calendar](https://msdn.microsoft.com/library/windows/apps/xaml/system.globalization.calendar.aspx), que tem funcionalidade muito semelhante. (Os aplicativos Windows Runtime podem usar a classe base do calendário do .NET, mas não as implementações específicas; por exemplo, GregorianCalendar.)

O .NET também oferece suporte a um tipo chamado [DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx), que pode ser convertido implicitamente em [DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx). Portanto, você pode ver um tipo de "DateTime"usado em código .NET que é usado para definir valores que são realmente DateTimeOffset. Para saber mais sobre a diferença entre DateTime e DateTimeOffset, consulte os Comentários na classe [DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx).

> **Observação**&nbsp;&nbsp;Propriedades que usam objetos de data não podem ser definidas como uma cadeia de caracteres de atributo XAML, pois o analisador de XAML do Windows Runtime não tem uma lógica de conversão para converter cadeias de caracteres em datas como objetos DateTime/DateTimeOffset. Você normalmente define esses valores no código. Outra técnica possível é definir uma data que está disponível como um objeto de dados ou no contexto de dados e, em seguida, definir a propriedade como um atributo XAML que faz referência a uma expressão da [extensão de marcação \{Binding\}](../../xaml-platform/binding-markup-extension.md) que pode acessar a data como dados.

## <a name="get-the-sample-code"></a>Obter o código de exemplo
* [Amostra de noções básicas de interface do usuário XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)


## <a name="related-topics"></a>Tópicos relacionados

**Para desenvolvedores (XAML)**
- [Classe CalendarView](https://msdn.microsoft.com/library/windows/apps/dn890052)
- [Classe CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/dn950083)
- [Classe DatePicker](https://msdn.microsoft.com/library/windows/apps/dn298584)
- [Classe TimePicker](https://msdn.microsoft.com/library/windows/apps/dn299280)

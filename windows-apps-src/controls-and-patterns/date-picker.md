---
author: Jwmsft
Description: O seletor de data oferece uma maneira padronizada de permitir que os usuários escolham um valor de data localizado usando entrada por toque, mouse ou teclado. 
title: Seletor de data
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
---

# Seletor de data

O seletor de data oferece uma maneira padronizada de permitir que os usuários escolham um valor de data localizado usando entrada por toque, mouse ou teclado. 

<span class="sidebar_heading" style="font-weight: bold;">APIs importantes</span>

-   [**Classe DatePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx)
-   [**Propriedade Date**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)

## Este é o controle correto?
Use um seletor de data para permitir que um usuário selecione uma data conhecida, como uma data de nascimento, em que o contexto do calendário não é importante.

Para saber mais sobre como escolher o controle de data correto, veja o artigo [Controles de data e hora](date-and-time.md).

## Exemplos

O ponto de entrada mostra a data escolhida e, quando o usuário seleciona o ponto de entrada, uma superfície de seletor se expande verticalmente do meio para que o usuário faça uma seleção. O seletor de data se sobrepõe à outra interface do usuário; ele não remove a outra interface do usuário.

![Exemplo da expansão do seletor de data](images/controls_datepicker_expand.png)

## Criar um seletor de data

Este exemplo mostra como criar um seletor de data simples com um cabeçalho

```xaml
<DatePicker x:Name=birthDatePicker Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

O seletor de data resultante tem esta aparência:

![Exemplo de seletor de data](images/date-picker-closed.png)

> **Observação**
            &nbsp;&nbsp;Para obter informações importantes sobre valores de data, consulte [Valores DateTime e Calendar](date-and-time.md#datetime-and-calendar-values) no artigo sobre os controles de Data e Hora.



## Artigos relacionados

- [Controles de data e hora](date-and-time.md)
- [Seletor de data do calendário](calendar-date-picker.md)
- [Visão de calendário](calendar-view.md)
- [Seletor de hora](time-picker.md)


<!--HONumber=May16_HO2-->



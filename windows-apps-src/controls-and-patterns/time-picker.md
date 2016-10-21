---
author: Jwmsft
Description: "O seletor de hora oferece uma maneira padronizada de permitir que os usuários escolham um valor usando entrada por toque, mouse ou teclado."
title: Seletor de hora
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 69f682b0edddbcf88515af537c33b3d8297f91f0

---
# Seletor de hora
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

O seletor de hora oferece uma maneira padronizada de permitir que os usuários escolham um valor usando entrada por toque, mouse ou teclado. 

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx"><strong>Classe TimePicker</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx"><strong>Propriedade Time</strong></a></li>
</ul>

</div>
</div>




## Esse é o controle correto?
Use um seletor de hora para permitir que um usuário selecione um valor de hora único.

Para obter mais informações sobre como escolher o controle correto, consulte o artigo [Controles de data e hora](date-and-time.md).

## Exemplos

O ponto de entrada mostra a hora escolhida e, quando o usuário seleciona o ponto de entrada, uma superfície de seletor se expande verticalmente do meio para que o usuário faça uma seleção. O seletor de hora se sobrepõe a outra interface do usuário; ele não remove a outra interface do usuário.

![Exemplo da expansão do seletor de hora](images/controls_timepicker_expand.png)

## Criar um seletor de hora

Este exemplo mostra como criar um seletor de hora simples com um cabeçalho.

```xaml
<TimePicker x:Name=arrivalTimePicker Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

O seletor de hora resultante tem esta aparência:

![Exemplo de seletor de hora](images/time-picker-closed.png)

> **Observação**&nbsp;&nbsp;Para obter informações importantes sobre valores de data e hora, consulte [Valores DateTime e Calendar](date-and-time.md#datetime-and-calendar-values) no artigo sobre os *Controles de data e hora*.



## Tópicos relacionados

- [Controles de data e hora](date-and-time.md)
- [Seletor de data do calendário](calendar-date-picker.md)
- [Visão de calendário](calendar-view.md)
- [Seletor de data](date-picker.md)



<!--HONumber=Aug16_HO3-->



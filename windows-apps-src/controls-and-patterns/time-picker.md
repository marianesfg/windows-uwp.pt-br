---
author: Jwmsft
Description: "O seletor de hora oferece uma maneira padronizada de permitir que os usuários escolham um valor usando entrada por toque, mouse ou teclado."
title: Seletor de hora
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 83ef423ce4e6fd57726879dd83a704c47cc43744
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="time-picker"></a>Seletor de hora
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

O seletor de hora oferece uma maneira padronizada de permitir que os usuários escolham um valor usando entrada por toque, mouse ou teclado. 

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Classe TimePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)</li>
<li>[**Propriedade Time**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)</li>
</ul>
</div>

## <a name="is-this-the-right-control"></a>Esse é o controle correto?
Use um seletor de hora para permitir que um usuário selecione um valor de hora único.

Para obter mais informações sobre como escolher o controle correto, consulte o artigo [Controles de data e hora](date-and-time.md).

## <a name="examples"></a>Exemplos

O ponto de entrada mostra a hora escolhida e, quando o usuário seleciona o ponto de entrada, uma superfície de seletor se expande verticalmente do meio para que o usuário faça uma seleção. O seletor de hora se sobrepõe a outra interface do usuário; ele não remove a outra interface do usuário.

![Exemplo da expansão do seletor de hora](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>Criar um seletor de hora

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

> [!NOTE]
> Para obter informações importantes sobre valores de data e hora, consulte [Valores DateTime e Calendar](date-and-time.md#datetime-and-calendar-values) no artigo *Controles de data e hora*.



## <a name="related-topics"></a>Tópicos relacionados

- [Controles de data e hora](date-and-time.md)
- [Seletor de data do calendário](calendar-date-picker.md)
- [Exibição de calendário](calendar-view.md)
- [Seletor de data](date-picker.md)

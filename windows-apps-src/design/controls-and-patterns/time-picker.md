---
author: muhsinking
Description: The time picker gives you a standardized way to let users pick a time value using touch, mouse, or keyboard input.
title: Seletor de hora
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.author: mukin
ms.date: 05/08/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c30b310deb509b9abfcd49531f85ea109cb12864
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5830026"
---
# <a name="time-picker"></a>Seletor de hora
 

O seletor de hora oferece uma maneira padronizada de permitir que os usuários escolham um valor usando entrada por toque, mouse ou teclado. 

> **APIs importantes**: [classe TimePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx), [propriedade Time](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)


## <a name="is-this-the-right-control"></a>Este é o controle correto?
Use um seletor de hora para permitir que um usuário selecione um valor de hora único.

Para obter mais informações sobre como escolher o controle correto, consulte o artigo [Controles de data e hora](date-and-time.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/TimePicker">abrir o aplicativo e ver o TimePicker em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

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

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.

## <a name="related-topics"></a>Tópicos relacionados

- [Controles de data e hora](date-and-time.md)
- [Seletor de data do calendário](calendar-date-picker.md)
- [Exibição de calendário](calendar-view.md)
- [Seletor de data](date-picker.md)

---
author: muhsinking
Description: The date picker gives you a standardized way to let users pick a localized date value using touch, mouse, or keyboard input.
title: Seletor de data
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
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
ms.openlocfilehash: 1f258ff63d2cf9badfc1066f46c97ecc14b90f5f
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5993022"
---
# <a name="date-picker"></a>Seletor de data

 

O seletor de data oferece uma maneira padronizada de permitir que os usuários escolham um valor de data localizado usando entrada por toque, mouse ou teclado. 

> **APIs importantes**: [classe DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx), [propriedade Date](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)


## <a name="is-this-the-right-control"></a>Este é o controle correto?
Use um seletor de data para permitir que um usuário selecione uma data conhecida, como uma data de nascimento, em que o contexto do calendário não é importante.

Para saber mais sobre como escolher o controle de data correto, veja o artigo [Controles de data e hora](date-and-time.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/DatePicker">abrir o aplicativo e ver o DatePicker em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

O ponto de entrada mostra a data escolhida e, quando o usuário seleciona o ponto de entrada, uma superfície de seletor se expande verticalmente do meio para que o usuário faça uma seleção. O seletor de data se sobrepõe à outra interface do usuário; ele não remove a outra interface do usuário.

![Exemplo da expansão do seletor de data](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>Criar um seletor de data

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

> **Observação**&nbsp;&nbsp;Para obter informações importantes sobre valores de data, consulte [Valores DateTime e Calendar](date-and-time.md#datetime-and-calendar-values) no artigo sobre os controles de Data e Hora.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Controles de data e hora](date-and-time.md)
- [Seletor de data do calendário](calendar-date-picker.md)
- [Exibição de calendário](calendar-view.md)
- [Seletor de hora](time-picker.md)

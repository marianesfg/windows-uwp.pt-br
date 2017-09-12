---
author: Jwmsft
Description: "O seletor de data oferece uma maneira padronizada de permitir que os usuários escolham um valor de data localizado usando entrada por toque, mouse ou teclado."
title: Seletor de data
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: True
label: Date picker
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.openlocfilehash: 25c49681085946e145d4feb4c8f93d2908f7bcbd
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="date-picker"></a>Seletor de data

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

O seletor de data oferece uma maneira padronizada de permitir que os usuários escolham um valor de data localizado usando entrada por toque, mouse ou teclado. 

> **APIs importantes**: [classe DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx), [propriedade Date](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)


## <a name="is-this-the-right-control"></a>Este é o controle correto?
Use um seletor de data para permitir que um usuário selecione uma data conhecida, como uma data de nascimento, em que o contexto do calendário não é importante.

Para saber mais sobre como escolher o controle de data correto, veja o artigo [Controles de data e hora](date-and-time.md).

## <a name="examples"></a>Exemplos

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



## <a name="related-articles"></a>Artigos relacionados

- [Controles de data e hora](date-and-time.md)
- [Seletor de data do calendário](calendar-date-picker.md)
- [Exibição de calendário](calendar-view.md)
- [Seletor de hora](time-picker.md)

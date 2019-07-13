---
Description: Utilize um rótulo para indicar ao usuário o que ele deve inserir em um controle adjacente. Você também pode rotular um grupo de controles relacionados ou exibir texto de instrução próximo a um grupo de controles relacionados.
title: Rótulos
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2b642f1d6c3f2a04bacdf293858492ea095af1a8
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319526"
---
# <a name="labels"></a>Rótulos

 

Rótulo é o nome ou o título de um controle ou de um grupo de controles relacionados.

> **APIs importantes**: Propriedade Header, [Classe TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

No XAML, muitos controles têm uma propriedade Header interna que é usada para especificar o rótulo. Para controles que não tenham uma propriedade Header ou para rotular grupos de controles, utilize um [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

![uma captura de tela que ilustra o controle de rótulo padrão](images/label-standard.png)

## <a name="recommendations"></a>Recomendações


-   Utilize um rótulo para indicar ao usuário o que ele deve inserir em um controle adjacente. Você também pode rotular um grupo de controles relacionados ou exibir texto de instrução próximo a um grupo de controles relacionados.
-   Ao rotular os controles, grave o rótulo como um substantivo ou uma frase nominal concisa, não como uma oração nem como texto de instrução. Evite usar dois-pontos ou outros sinais de pontuação.
-   Quando tiver texto de instrução em um rótulo, você poderá ser mais generoso com o comprimento da cadeia de texto e também usar pontuação.


## <a name="get-the-sample-code"></a>Obter o código de exemplo
* [Amostra de noções básicas de interface do usuário XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Tópicos relacionados
* [Controles de texto](text-controls.md)
* [Propriedade TextBox.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.header)
* [Propriedade PasswordBox.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.header)
* [Propriedade ToggleSwitch.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.header)
* [Propriedade DatePicker.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepicker.header)
* [Propriedade TimePicker.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.header)
* [Propriedade Slider.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.slider.header)
* [Propriedade ComboBox.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.header)
* [Propriedade RichEditBox.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.header)
* [Classe TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

 

 





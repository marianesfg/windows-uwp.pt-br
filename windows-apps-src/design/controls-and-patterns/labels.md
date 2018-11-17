---
author: Jwmsft
Description: Use a label to indicate to the user what they should enter into an adjacent control. You can also label a group of related controls, or display instructional text near a group of related controls.
title: Rótulos
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c17b2a539a01572bed984b86f72f5439896fac87
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7171176"
---
# <a name="labels"></a>Rótulos

 

Rótulo é o nome ou o título de um controle ou de um grupo de controles relacionados.

> **APIs importantes**: propriedade Header, [classe TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

No XAML, muitos controles têm uma propriedade Header interna que é usada para especificar o rótulo. Para os controles sem uma propriedade Header ou para rotular grupos de controles, você pode utilizar um [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652).

![uma captura de tela que ilustra o controle de rótulo padrão](images/label-standard.png)

## <a name="recommendations"></a>Recomendações


-   Utilize um rótulo para indicar ao usuário o que ele deve inserir em um controle adjacente. Você também pode rotular um grupo de controles relacionados, ou exibir texto de instrução próximo a um grupo de controles relacionados.
-   Ao rotular os controles, grave o rótulo como um substantivo ou uma frase nominal concisa, não como uma oração nem como texto de instrução. Evite usar dois-pontos ou outros sinais de pontuação.
-   Quando tiver texto de instrução em um rótulo, você poderá ser mais generoso com o comprimento da cadeia de texto e também usar pontuação.


## <a name="get-the-sample-code"></a>Obter o código de exemplo
* [Amostra de noções básicas de interface do usuário XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Tópicos relacionados
* [Controles de texto](text-controls.md)
* [Propriedade TextBox.Header](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [Propriedade PasswordBox.Header](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [Propriedade ToggleSwitch.Header](https://msdn.microsoft.com/library/windows/apps/br209713)
* [Propriedade DatePicker.Header](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [Propriedade TimePicker.Header](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [Propriedade Slider.Header](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [Propriedade ComboBox.Header](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [Propriedade RichEditBox.Header](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [Classe TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 





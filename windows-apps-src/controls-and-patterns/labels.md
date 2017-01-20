---
author: Jwmsft
Description: "Utilize um rótulo para indicar ao usuário o que ele deve inserir em um controle adjacente. Você também pode rotular um grupo de controles relacionados ou exibir texto de instrução próximo a um grupo de controles relacionados."
title: "Rótulos"
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 6757e612d5a04db52222cfb73c855a0a4f615bf6

---
# <a name="labels"></a>Rótulos

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Rótulo é o nome ou o título de um controle ou de um grupo de controles relacionados.

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>Propriedade Header</li>
<li>[**Classe TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)</li>
</ul>
</div>


No XAML, muitos controles têm uma propriedade Header interna que é usada para especificar o rótulo. Para controles que não tenham uma propriedade Header, ou para rotular grupos de controles, você pode utilizar um [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) em vez disso.


## <a name="example"></a>Exemplo


![uma captura de tela que ilustra o controle de rótulo padrão](images/label-standard.png)

## <a name="recommendations"></a>Recomendações


-   Utilize um rótulo para indicar ao usuário o que ele deve inserir em um controle adjacente. Você também pode rotular um grupo de controles relacionados, ou exibir texto de instrução próximo a um grupo de controles relacionados.
-   Ao rotular os controles, grave o rótulo como um substantivo ou uma frase nominal concisa, não como uma oração nem como texto de instrução. Evite usar dois-pontos ou outros sinais de pontuação.
-   Quando tiver texto de instrução em um rótulo, você poderá ser mais generoso com o comprimento da cadeia de texto e também usar pontuação.


## <a name="get-the-sample-code"></a>Obter o código de exemplo
* [Exemplo de noções básicas da interface do usuário XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Tópicos relacionados
* [Controles de texto](text-controls.md)

**Para desenvolvedores**
* [**Propriedade TextBox.Header**](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [**Propriedade PasswordBox.Header**](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [**Propriedade ToggleSwitch.Header**](https://msdn.microsoft.com/library/windows/apps/br209713)
* [**Propriedade DatePicker.Header**](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [**Propriedade TimePicker.Header**](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [**Propriedade Slider.Header**](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [**Propriedade ComboBox.Header**](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [**Propriedade RichEditBox.Header**](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [**Classe TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 







<!--HONumber=Dec16_HO2-->



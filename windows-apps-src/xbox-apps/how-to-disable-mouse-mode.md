---
title: Como desabilitar o modo do mouse
description: Instruções para desabilitar o modo de mouse padrão.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e57ee4e6-7807-4943-a933-c2b4dc80fc01
ms.localizationpriority: medium
ms.openlocfilehash: 1e4b8868f416494daf978d65d4a4ccde02d6ccf5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656621"
---
# <a name="how-to-disable-mouse-mode"></a>Como desabilitar o modo do mouse
O modo de mouse está ativado por padrão para todos os aplicativos, o que significa que todos os aplicativos que não o desativaram receberão um ponteiro de mouse (semelhante ao do navegador Microsoft Edge no console). É altamente recomendável desativar essa opção e otimizar para navegação direcional de controlador.   
   
## <a name="html"></a>HTML   
Para ativar a navegação de controlador direcional em um aplicativo da Plataforma Universal do Windows (UWP) JavaScript, use a biblioteca JavaScript de [navegação direcional TVHelpers](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Inclua o arquivo JavaScript de navegação direcional no pacote do aplicativo e adicione uma referência a ele em todas as páginas HTML que exigem navegação de controlador direcional:

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Para obter mais detalhes, consulte o [wiki de navegação direcional](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation).

Se você desejar desativar o modo de mouse e usar as APIs de gamepad DOM ou WinRT diretamente, execute o seguinte para cada página que precisar delas: 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   Essa propriedade assume `mouse` como padrão, o que habilita o modo de mouse. Definir como `keyboard` desativa o modo de mouse e, em vez disso, a entrada de gamepad gera eventos de teclado do DOM. Definir como `gamepad` desativa o modo de mouse e não gera eventos de teclado do DOM e permite que você use apenas as APIs de gamepad DOM ou WinRT.

## <a name="xaml"></a>XAML    
Para desativar o modo de mouse, adicione o seguinte ao construtor do seu aplicativo:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## <a name="cdirectx"></a>C++/DirectX   
Se você estiver escrevendo um aplicativo em C++/DirectX, não há nada a fazer. O modo do mouse só se aplica a aplicativos HTML e XAML.

## <a name="see-also"></a>Consulte também
- [Práticas recomendadas para Xbox](tailoring-for-xbox.md)
- [UWP no Xbox One](index.md)


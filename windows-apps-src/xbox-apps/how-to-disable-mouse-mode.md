---
author: payzer
title: Como desativar o modo do mouse
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 6f4719c98d490cdcac8c799c4c68af55b217cbc5
ms.openlocfilehash: d1ee946693b9f9714b8d570b8ae3718469d2c10d

---

# Como desativar o modo do mouse
O modo do mouse é ativado por padrão para todos os aplicativos. Isso significa que todos os aplicativos que não recusaram a opção receberão um ponteiro de mouse (semelhante ao do navegador Edge no console). É altamente recomendável desativar essa opção e otimizar para navegação direcional de controlador.   
   
## HTML   
Para ativar a navegação de controlador direcional em um aplicativo UWP JavaScript, use a biblioteca JavaScript de [navegação direcional TVHelpers](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Inclua o arquivo JavaScript de navegação direcional no pacote do aplicativo e adicione uma referência a ele em todas as páginas HTML que exigem navegação de controlador direcional:
```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Consulte o [wiki de navegação direcional](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) para obter mais detalhes.

Se você desejar desativar o modo de mouse e usar as APIs de gamepad DOM ou WinRT diretamente, execute o seguinte para cada página que precisar delas: 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

Essa propriedade assume ```'mouse'``` como padrão, o que habilita o modo de mouse. Definir como ```'keyboard'``` desativa o modo de mouse e, em vez disso, a entrada de gamepad gera eventos de teclado do DOM. Definir como ```'gamepad'``` desativa o modo de mouse e não gera eventos de teclado do DOM e permite que você use apenas as APIs de gamepad DOM ou WinRT.

## XAML    
Para desativar o modo de mouse, adicione o seguinte ao construtor do seu aplicativo:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## C++/DirectX   
Se você estiver escrevendo um aplicativo em C++/DirectX, não há nada a fazer. O modo do mouse só se aplica a aplicativos HTML e XAML.



<!--HONumber=Jul16_HO1-->



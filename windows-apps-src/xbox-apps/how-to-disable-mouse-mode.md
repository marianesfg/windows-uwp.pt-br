---
author: payzer
title: Como desativar o modo do mouse
description: 
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: bd7780f105f86d7d292c5db2535ad40af07d6e4f

---

# Como desativar o modo do mouse
O modo do mouse é ativado por padrão para todos os aplicativos. Isso significa que todos os aplicativos que não recusaram a opção receberão um ponteiro de mouse (semelhante ao do navegador Edge no console). É altamente recomendável desativar essa opção e otimizar para navegação direcional de controlador.   
   
## HTML   
Para desativar o modo do mouse, adicione o seguinte em um arquivo JavaScript em seu aplicativo:   
   
```code
navigator.gamepadInputEmulation = "keyboard";
```   

## XAML    
Para desativar o modo do mouse, adicione o seguinte em um arquivo JavaScript em seu aplicativo:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
        }
```

## C++/DirectX   
Se você estiver escrevendo um aplicativo em C++/DirectX, não há nada a fazer. O modo do mouse só se aplica a aplicativos HTML e XAML.



<!--HONumber=Jun16_HO5-->



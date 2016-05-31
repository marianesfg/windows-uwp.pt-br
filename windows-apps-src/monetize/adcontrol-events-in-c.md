---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: Saiba como manipular os eventos da classe AdControl.
title: Eventos de AdControl em C#
---

# Eventos de AdControl em C\# #  


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Os exemplos a seguir demonstram como manipular os eventos da classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx). Esses exemplos pressupõem que você atribuiu anteriormente os manipuladores de eventos aos eventos **AdControl** em XAML. Para obter mais informações sobre como fazer isso, consulte [Exemplo das propriedades XAML](xaml-properties-example.md).

Para obter mais informações sobre como manipular eventos em C#, consulte [Visão geral de eventos e eventos roteados (aplicativos universais do Windows em C#/VB/C++ e XAML)](http://msdn.microsoft.com/library/windows/apps/hh758286).

## Exemplos


``` syntax
private void OnAdError(object sender, AdErrorEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}

private void OnAdRefresh(object sender, RoutedEventArgs e) {
  // place code here that you wish to execute when the ad refreshes.
 return;
}

private void OnAdEngagedChanged(object sender, RoutedEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}
```

## Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
* [Tratamento de erros de AdControl](adcontrol-error-handling.md)
* [Classe RoutedEventArgs](http://msdn.microsoft.com/en-us/library/system.windows.routedeventargs.aspx)

 

 


<!--HONumber=May16_HO2-->



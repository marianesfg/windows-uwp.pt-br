---
author: msatranjr
Description: "O controle de mapa pode exibir mapas de rodovias e vistas aéreas, trajeto, resultados de pesquisa e informações sobre trânsito."
title: Diretrizes para mapas
ms.assetid: 7B5B6BC9-D1EC-4978-8876-20B78EF44797
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: 3c1d37a119eca88ee443772292f8cfe97cb3c1ac

---

# Controle de mapa


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


O controle de mapa pode exibir mapas de rodovias, visualizações aéreas, 3D, trajetos, resultados de busca e informações sobre trânsito. Em um mapa, você pode exibir a localização, o trajeto e pontos de interesse para o usuário. Um mapa também pode exibir vistas aéreas em 3D, modos de exibição Streetside, tráfego, trânsito e empresas locais.

![exemplo de um mapa, modo de exibição básico](./images/win10fa/controls-maps-basic.jpg)

## Este é o controle correto?


Use um controle de mapa quando quiser que um mapa dentro de seu aplicativo permita aos usuários ver informações geográficas gerais ou específicas do aplicativo. Ter um controle de mapa no seu aplicativo significa que os usuários não têm que sair do seu aplicativo para obter informações.

**Observação**  Se você não se importar que os usuários saiam do seu aplicativo para obter essas informações, considere usar o aplicativo Mapas do Windows para fornecer essas informações. Seu aplicativo pode iniciar o aplicativo Mapas do Windows para exibir mapas específicos, trajetos e resultados de pesquisa. Para obter mais informações, consulte [Iniciar o aplicativo Mapas do Windows](https://msdn.microsoft.com/library/windows/apps/mt228341).

## Exemplos


Este exemplo mostra um mapa com um modo de exibição Streetside:

![exemplo de modo de exibição Streetside do controle de mapa](./images/win10fa/controls-maps-streetside.jpg)

 

Este exemplo mostra um mapa com uma vista aérea 3D:

![exemplo de modo de exibição 3D do controle de mapa](./images/win10fa/controls-maps-3dview.jpg)

 

Este exemplo mostra um aplicativo com uma vista aérea 3D e um modo de exibição do Streetside:

![exemplo de vista de mapa em 3D com modo de exibição Streetside](./images/win10fa/controls-maps-3dstreetview.png)


## Recomendações


-   Use espaço amplo na tela (ou a tela inteira) para exibir o mapa para que os usuários não precisem aplicar Panorâmica e zoom excessivamente para ver informações geográficas.

-   Se o mapa só é usado para apresentar uma exibição estática e informativa, convém usar um mapa menor. Se você optar por um mapa menor e estático, baseie suas dimensões na usabilidade - pequeno o suficiente para conservar espaço suficiente na tela, mas grande o suficiente para permanecer legível.

-   Insira os pontos de interesse na cena do mapa cena usando [**elementos de mapa**](https://msdn.microsoft.com/library/windows/apps/dn637034); informações adicionais podem ser exibidas como uma interface do usuário temporária que se sobreponha à a cena do mapa.

## Tópicos relacionados


* [Exibir mapas com modos de exibição 2D, 3D e Streetside](https://msdn.microsoft.com/library/windows/apps/mt219695)
* [Exibir pontos de interesse (POI) em um mapa](https://msdn.microsoft.com/library/windows/apps/mt219696)
* [Central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [//Vídeo do build 2015: Aproveitando mapas e localização em telefones, tablets e computadores em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Iniciar o aplicativo Mapas do Windows](https://msdn.microsoft.com/library/windows/apps/mt228341)
 

 







<!--HONumber=Jun16_HO5-->



---
author: msatranjr
title: "Anúncios de Bluetooth"
description: "Esta seção contém artigos sobre como integrar anúncios de baixa energia (LE) Bluetooth em aplicativos da Plataforma Universal do Windows (UWP) por meio do usuário das APIs AdvertisementWatcher e AdvertisementPublisher."
ms.sourcegitcommit: 62e97bdb8feb78981244c54c76a00910a8442532
ms.openlocfilehash: a419ad04fe4f21867f2f1bd1664fbce39a7da792

---

# Anúncios de Bluetooth

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** APIs importantes ** 

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

Este artigo fornece uma visão geral dos anúncios de Bluetooth (Beacons) para aplicativos da Plataforma Universal do Windows (UWP).  

## Visão geral

Há duas funções principais que um desenvolvedor pode executar usando as APIs de anúncio:

-   [Inspetor de anúncio](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx): ouve beacons nas proximidades e filtra com base na carga ou proximidade.  
-   [Fornecedor de anúncio](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx): define uma carga para o Windows anunciar em nome de um desenvolvedor.  

O código de exemplo completo é encontrado no [Exemplo de anúncio de Bluetooth](http://go.microsoft.com/fwlink/p/?LinkId=619990) no Github



<!--HONumber=Jun16_HO5-->



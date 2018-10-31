---
author: Xansky
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Consulte as notas de versão das bibliotecas do Microsoft Advertising.
title: Notas de versão para as bibliotecas de anúncio
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, notas de versão
ms.localizationpriority: medium
ms.openlocfilehash: dbe932eb9391a4de0304b4be42944b2bced3287a
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5888745"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Notas de versão para as bibliotecas de anúncio




Esta seção fornece notas de versão para a versão atual das bibliotecas do Microsoft Advertising. Essas bibliotecas dão suporte a aplicativos XAML e JavaScript/HTML para Windows 10, Windows 8.1, Windows Phone 8.1 e WindowsPhone8.

## <a name="installation"></a>Instalação


As bibliotecas de publicidade da Microsoft estão disponíveis como parte do [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp). Para obter mais informações sobre como instalar o SDK do Windows, consulte [Instalar o SDK do Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

## <a name="uninstall-previous-versions"></a>Desinstalar as versões anteriores

Antes de instalar o SDK do Microsoft Advertising mais recente, é altamente recomendável que você desinstale todas as instâncias anteriores do SDK. Para obter mais informações, consulte [Instalar o SDK do Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

## <a name="target-architecture-specific-build-outputs"></a>Direcionar saídas de compilação específicas da arquitetura

Ao usar as bibliotecas do Microsoft Advertising, você não pode direcionar **nenhuma CPU** em seu projeto. Se o seu projeto for direcionado para **qualquer plataforma de CPU**, você poderá ver um aviso em seu projeto depois de adicionar uma referência às bibliotecas do Microsoft Advertising. Para remover esse aviso, atualize seu projeto para usar uma saída de compilação específica da arquitetura (por exemplo, **x86**). Para saber mais, consulte [Problemas conhecidos](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>Suporte a C++

As bibliotecas do Microsoft Advertising (que incluem as classes **AdControl** e **InterstitialAd**) dão suporte a aplicativos escritos em C++ e DirectX usando a interoperabilidade do Windows Runtime (*interoperabilidade*). Para obter mais informações e exemplos sobre a programação em XAML e C++, consulte [Sistema de tipos](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

## <a name="no-toolbox-control"></a>Nenhum controle de caixa de ferramentas

A versão atual das bibliotecas do Microsoft Advertising no [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp), não há nenhum controle de caixa de ferramentas para arrastar um **AdControl** ou **InterstitialAd** para um Design Surface em seu aplicativo. Para obter instruções sobre como adicionar esses controles ao seu código e marcação, consulte os [guias passo a passo do desenvolvedor](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Propriedades de latitude e longitude não estão mais disponíveis

A classe **AdControl** não tem as propriedades **Latitude** e **Longitude** para aplicativos UWP. Em vez disso, o código integrado no controle de anúncios detectará e enviará esses valores aos servidores de anúncios em nome do aplicativo.


 

 

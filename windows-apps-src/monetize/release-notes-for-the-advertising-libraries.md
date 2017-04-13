---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "Revise as notas de versão para as bibliotecas do Microsoft Advertising no Microsoft Store Services SDK."
title: "Notas de versão para as bibliotecas de anúncio"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, notas de versão"
ms.openlocfilehash: f3d07df6e64c96e9070cb82bd7ac6e96b9cad1ee
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="release-notes-for-the-advertising-libraries"></a>Notas de versão para as bibliotecas de anúncio




Esta seção fornece notas de versão para a versão atual das bibliotecas do Microsoft Advertising no Microsoft Store Services SDK (para aplicativos UWP) e o Microsoft Advertising SDK para Windows e Windows Phone 8.x (para aplicativos do Windows 8.1 e Windows Phone 8.x). Essas bibliotecas dão suporte a aplicativos XAML e JavaScript/HTML para Windows 10, Windows 8.1, Windows Phone 8.1 e Windows Phone 8.

## <a name="installation"></a>Instalação


As bibliotecas da Microsoft Advertising estão disponíveis como parte do [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) (para aplicativos UWP) e do [SDK da Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk) (para aplicativos Windows 8.1 e Windows Phone 8.x). Para obter mais informações sobre como instalar os SDKs e as bibliotecas que estão incluídas neles, consulte [Instalar as bibliotecas do Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

Para obter os assemblies do Microsoft Advertising para projetos do Windows Phone 8.x Silverlight, instale o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk), abra o projeto no Visual Studio e, em seguida, vá para **Projeto** > **Adicionar Serviço Conectado** > **Ad Mediator** para baixar automaticamente os assemblies. Depois de fazer isso, você pode remover as referências do Ad Mediator do seu projeto se não quiser usar a mediação de anúncios. Para saber mais, consulte [AdControl no Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).


## <a name="uninstall-previous-versions"></a>Desinstalar as versões anteriores

Antes de instalar o Microsoft Store Services SDK (para aplicativos UWP) ou o SDK da Microsoft Advertising para Windows e Windows Phone 8.x (para aplicativos Windows 8.1 e Windows Phone 8.x), é altamente recomendável desinstalar todas as instâncias anteriores do SDK do Microsoft Universal Ad Client ou do SDK da Microsoft Advertising.

## <a name="target-architecture-specific-build-outputs"></a>Direcionar saídas de compilação específicas da arquitetura

Ao usar as bibliotecas do Microsoft Advertising, você não pode direcionar **nenhuma CPU** em seu projeto. Se o seu projeto for direcionado para **qualquer plataforma de CPU**, você poderá ver um aviso em seu projeto depois de adicionar uma referência às bibliotecas do Microsoft Advertising. Para remover esse aviso, atualize seu projeto para usar uma saída de compilação específica da arquitetura (por exemplo, **x86**). Para saber mais, consulte [Problemas conhecidos](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>Suporte a C++

As bibliotecas do Microsoft Advertising (que incluem as classes **AdControl** e **InterstitialAd**) dão suporte a aplicativos escritos em C++ e DirectX usando a interoperabilidade do Windows Runtime (*interoperabilidade*). Para obter mais informações e exemplos sobre a programação em XAML e C++, consulte [Sistema de tipos](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx).

## <a name="no-toolbox-control"></a>Nenhum controle de caixa de ferramentas

Na versão atual das bibliotecas da Microsoft Advertising no Microsoft Store Services SDK ou no SDK da Microsoft Advertising para Windows e Windows Phone 8.x, não há controle de caixa de ferramentas para arrastar um **AdControl** ou **InterstitialAd** até uma área de design no aplicativo. Para obter instruções sobre como adicionar esses controles ao seu código e marcação, consulte os [guias passo a passo do desenvolvedor](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Propriedades de latitude e longitude não estão mais disponíveis

A classe **AdControl** não tem as propriedades **Latitude** e **Longitude** para aplicativos UWP. Em vez disso, o código integrado no controle de anúncios detectará e enviará esses valores aos servidores de anúncios em nome do aplicativo.


 

 

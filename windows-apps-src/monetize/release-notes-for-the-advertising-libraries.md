---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Revise as notas de versão para as bibliotecas do Microsoft Advertising no SDK de Microsoft Store Engagement and Monetization.
title: Notas de versão para as bibliotecas do Microsoft Advertising
---

# Notas de versão para as bibliotecas do Microsoft Advertising


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção fornece notas de versão para a versão atual das bibliotecas do Microsoft Advertising no SDK de Microsoft Store Engagement and Monetization. Essas bibliotecas dão suporte a aplicativos XAML e JavaScript/HTML para Windows 10, Windows 8.1, Windows Phone 8.1 e Windows Phone 8.

## Instalação


As bibliotecas do Microsoft Advertising estão disponíveis como parte do [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Para todos os tipos de projeto que não sejam Windows Phone 8.x Silverlight, os assemblies do Microsoft Advertising que eram distribuídos nas versões autônomas anteriores do SDK do Microsoft Universal Ad Client e do Microsoft Advertising agora são instalados com o SDK de Microsoft Store Engagement and Monetization. Para obter mais informações sobre como instalar o SDK e as bibliotecas que estão incluídas nele, consulte [Instalar as bibliotecas do Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

Para obter os assemblies do Microsoft Advertising para projetos do Windows Phone 8.x Silverlight, instale o [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk), abra o projeto no Visual Studio e, em seguida, vá para **Projeto** > **Adicionar Serviço Conectado** > **Ad Mediator** para baixar automaticamente os assemblies. Depois de fazer isso, você pode remover as referências do Ad Mediator do seu projeto se não quiser usar a mediação de anúncios. Para saber mais, consulte [AdControl no Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).

## Noções básicas sobre a diferença entre bibliotecas do Microsoft Advertising e a mediação de anúncios

Embora as bibliotecas do Microsoft Advertising e as bibliotecas de mediação de anúncio sejam fornecidas pelo SDK de Microsoft Store Engagement and Monetization, essas bibliotecas têm finalidades diferentes. Use as classes [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) nas bibliotecas do Microsoft Advertising se quiser exibir anúncios em banner ou intersticiais em vídeo da Microsoft em um aplicativo XAML ou JavaScript. Use a classe **AdMediatorControl** nas bibliotecas de mediação de anúncio, se quiser exibir anúncios em banner de várias redes de publicidade em um aplicativo XAML (a mediação de anúncios não tem suporte em aplicativos JavaScript/HTML). Para obter mais informações, consulte [Qual é a diferença: AdMediatorControl ou AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

## Desinstalar as versões anteriores

Antes de instalar o SDK de Microsoft Store Engagement and Monetization, é altamente recomendável que você desinstale todas as instâncias anteriores do SDK do Microsoft Universal Ad Client ou do SDK do Microsoft Advertising.

## Direcionar saídas de compilação específicas da arquitetura

Ao usar as bibliotecas do Microsoft Advertising, você não pode direcionar **nenhuma CPU** em seu projeto. Se o seu projeto for direcionado para **qualquer plataforma de CPU**, você poderá ver um aviso em seu projeto depois de adicionar uma referência às bibliotecas do Microsoft Advertising. Para remover esse aviso, atualize seu projeto para usar uma saída de compilação específica da arquitetura (por exemplo, **x86**). Para saber mais, consulte [Problemas conhecidos](known-issues-for-the-advertising-libraries.md).

## Suporte a C++

As bibliotecas do Microsoft Advertising (que incluem as classes **AdControl** e **InterstitialAd**) dão suporte a aplicativos escritos em C++ e DirectX usando a interoperabilidade do Windows Runtime (*interoperabilidade*). Para obter mais informações e exemplos sobre a programação em XAML e C++, consulte [Sistema de tipos](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx).

## Nenhum controle de caixa de ferramentas

A versão atual das bibliotecas do Microsoft Advertising no SDK de Microsoft Store Engagement and Monetization, não há nenhum controle de caixa de ferramentas para arrastar um **AdControl** ou **InterstitialAd** para uma superfície de design em seu aplicativo. Para obter instruções sobre como adicionar esses controles ao seu código e marcação, consulte os [guias passo a passo do desenvolvedor](developer-walkthroughs.md).

## Propriedades de latitude e longitude não estão mais disponíveis

A classe **AdControl** não tem as propriedades **Latitude** e **Longitude** para aplicativos UWP. Em vez disso, o código integrado no controle de anúncios detectará e enviará esses valores aos servidores de anúncios em nome do aplicativo.

## Aviso importante

Certifique-se de ler o contrato de licença do usuário final (EULA) em sua totalidade. Consulte o tópico [Aviso importante - EULA](important-notice-eula.md).

 

 


<!--HONumber=May16_HO2-->



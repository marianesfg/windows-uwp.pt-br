---
Description: Estas são algumas soluções para vários problemas comuns de desenvolvimento relacionados ao controle de anúncios.
title: Solucionar problemas de controle de anúncios
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
---

# Solucionar problemas de controle de anúncios


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, veja o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Estas são algumas soluções para vários problemas comuns de desenvolvimento relacionados ao controle de anúncios.

**Não é possível adicionar AdMediatorControl à superfície de design**  
Quando você arrasta o controle **AdMediatorControl** para o designer pela primeira vez em uma Plataforma Universal do Windows (UWP), no Windows 8.1 ou no projeto do Windows Phone 8.1 usando a linguagem C# ou o Visual Basic com XAML, o Visual Studio adiciona a referência de assembly do controle de anúncios necessários ao seu projeto, mas o controle ainda não é adicionado ao designer. Para adicionar o controle, clique em OK na mensagem exibida pelo Visual Studio, aguarde alguns segundos pela atualização do designer e arraste o controle de volta para o designer.

Se você ainda não conseguir adicionar o controle ao designer, verifique se o projeto se destina à arquitetura do processador pertinente a seu aplicativo (por exemplo, **x86**) em vez de **Qualquer CPU**. O controle não pode ser adicionado ao designer se o projeto se destinar a **Qualquer CPU** da plataforma de compilação.

**O controle AdMediatorControl mostra o erro “&lt;*width*&gt; x &lt;*height*&gt; Sem suporte" em tempo de execução durante a veiculação de anúncios da Microsoft**  
O Microsoft Advertising é compatível [apenas com alguns tamanhos de anúncio recomendados pelo Interactive Advertising Bureau (IAB).](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising). Em alguns casos, mesmo se você definir a altura e a largura do controle no controle de anúncios do designer ou de seu XAML para um desses tamanhos anúncios compatíveis, problemas de dimensionamento e arredondamento poderão impedir que a estrutura do controle de anúncios veicule um anúncio. Para evitar esse problema, atribua os parâmetros opcionais **Width** e **Height** ao Microsoft Advertising em seu código a um dos tamanhos de anúncios compatíveis.

O exemplo de código a seguir demonstra como atribuir os parâmetros opcionais **Width** e **Height** ao Microsoft Advertising para uma largura e altura de 728 x 90, respectivamente.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**O controle de anúncios não inclui a localização (latitude/longitude) da rede de publicidade**  
Se você habilitar a funcionalidade de localização em seu aplicativo, o controle de anúncios buscará automaticamente as coordenadas de latitude/longitude e as fornecerá às redes de publicidade que as aceitam.

**O controle de anúncios Smaato não se alinha corretamente**  
Tente usar os parâmetros opcionais para definir os valores nos controles do SDK:

```
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Margin”] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Width”] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Height”] = 320d;
```

**O controle de anúncio AdDuplex não aparece com o tamanho correto (aparece como 250 × 250)**  
O controle de anúncios não define qualquer valor para o tamanho, portanto você deve alterá-lo usando o parâmetro opcional Size. Por exemplo:

```
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex][“Size”] = “160×600″;
```

**Você recebe o erro “Something is covering the ad control”**  
O AD Duplex sempre mostrará um erro se o anúncio estiver coberto de alguma forma em seu aplicativo. [Leia a solução](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl) para esse erro.

**Você recebe o erro "There was a conflict between two files"**  
Você referenciou os assemblies do Microsoft Advertising em outro lugar de seu aplicativo. O controle de anúncios foi desenvolvido para funcionar exclusivamente em seu aplicativo. Ele não funcionará se outras referências aos assemblies do Microsoft Advertising forem usadas. Remova as referências do Microsoft Advertising manualmente e reinstale o SDK do Microsoft Store Engagement and Monetization para apagar o erro.

## Tópicos relacionados

* [Selecionar e gerenciar suas redes de publicidade](select-and-manage-your-ad-networks.md)
* [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md)
* [Testar sua implementação de controle de anúncios](test-your-ad-mediation-implementation.md)
* [Enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md)
 

 


<!--HONumber=Mar16_HO5-->



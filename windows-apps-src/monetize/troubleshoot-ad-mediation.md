---
author: mcleanbyron
Description: "Estas são algumas soluções para vários problemas comuns de desenvolvimento relacionados ao controle de anúncios."
title: "Solucionar problemas de controle de anúncios"
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
translationtype: Human Translation
ms.sourcegitcommit: 10dcf3c2b8ea530b94e9c17ada80aaa98e9418fe
ms.openlocfilehash: f32dc28c9b199c11a1932639f49ab4c29d3e1e8f

---

# Solucionar problemas de controle de anúncios


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Estas são algumas soluções para vários problemas comuns de desenvolvimento relacionados ao controle de anúncios.

**Não é possível adicionar AdMediatorControl à superfície de design**  
Quando você arrasta o controle **AdMediatorControl** para o designer pela primeira vez em uma Plataforma Universal do Windows (UWP), no Windows 8.1 ou no projeto do Windows Phone 8.1 usando a linguagem C# ou o Visual Basic com XAML, o Visual Studio adiciona a referência de assembly do controle de anúncios necessários ao seu projeto, mas o controle ainda não é adicionado ao designer. Para adicionar o controle, clique em OK na mensagem exibida pelo Visual Studio, aguarde alguns segundos pela atualização do designer e arraste o controle de volta para o designer.

Se você ainda não conseguir adicionar o controle ao designer, verifique se o projeto se destina à arquitetura do processador pertinente a seu aplicativo (por exemplo, **x86**) em vez de **Qualquer CPU**. O controle não poderá ser adicionado ao designer se o projeto se destinar a **Qualquer CPU** da plataforma de compilação.

*
              *O AdMediatorControl mostra o erro "&lt;*width*
            &gt; x &lt;*height*&gt; Not supported" no tempo de execução ao servir anúncios da Microsoft**  O Microsoft Advertising só dá suporte a [determinados tamanhos de anúncios recomendados pelo Interactive Advertising Bureau (IAB)](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising). Em alguns casos, mesmo se você definir a altura e a largura do controle de mediador de anúncios no designer ou em seu XAML como um desses tamanhos de anúncio com suporte, problemas de dimensionamento e arredondamento podem impedir que a estrutura de mediação de anúncios sirva um anúncio. Para evitar esse problema, atribua os parâmetros opcionais **Width** e **Height** para o Microsoft Advertising em seu código a um dos tamanhos de anúncio com suporte.

O exemplo de código a seguir demonstra como atribuir os parâmetros opcionais **Width** e **Height** ao Microsoft Advertising para uma largura e altura de 728 x 90, respectivamente.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**O controle de anúncios não inclui a localização (latitude/longitude) da rede de publicidade**  
Se você habilitar a funcionalidade de localização em seu aplicativo, o controle de anúncios buscará automaticamente as coordenadas de latitude/longitude e as fornecerá às redes de publicidade que as aceitam.

**O controle de anúncios Smaato não se alinha corretamente**  
Tente usar os parâmetros opcionais para definir os valores nos controles do SDK:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Margin"] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 320d;
```

**O controle de anúncios AdDuplex não aparece com o tamanho correto (aparece como 250 × 250)**  
O controle de anúncios não define nenhum valor para o tamanho, portanto, você deve alterá-lo usando o parâmetro opcional **Size**. Por exemplo:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex]["Size"] = "160x600";
```

**Você recebe o erro “Something is covering the ad control”**  
O AD Duplex sempre mostrará um erro se o anúncio estiver coberto de alguma forma em seu aplicativo. [Leia a solução](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl) para esse erro.

**Você recebe o erro "There was a conflict between two files"**  
Você referenciou os assemblies do Microsoft Advertising em outro lugar de seu aplicativo. O controle de anúncios foi desenvolvido para funcionar exclusivamente em seu aplicativo. Ele não funcionará se outras referências aos assemblies do Microsoft Advertising forem usadas. Remova as referências do Microsoft Advertising manualmente e reinstale o SDK do Microsoft Store Engagement and Monetization para apagar o erro.

**Você se depara com um comportamento inesperado depois de alterar o valor RefreshRate no arquivo AdMediator.config**

Depois de configurar suas redes de publicidade executando o componente **Ad Mediator** na caixa de diálogo **Adicionar Serviço Conectado** no Visual Studio, as informações de configuração padrão são salvas no arquivo AdMediator.config em seu projeto. Você não deve modificar esse arquivo diretamente. Em vez disso, modifique essas informações (incluindo a taxa de atualização para novos anúncios) ao [definir as configurações de mediação de anúncios para seu aplicativo](submit-your-app-and-configure-ad-mediation.md) depois de carregar o pacote do aplicativo para o painel do Centro de Desenvolvimento do Windows.

Se você alterar o valor **RefreshRate** no arquivo AdMediator.config, lembre-se de que esse valor deve conter um número inteiro entre 30 e 120 que represente a taxa de atualização em segundos. Se você definir isso como um número inteiro menor do que 30 ou maior do que 120, a estrutura de mediação de anúncio usará automaticamente uma taxa de atualização de 60 segundos.

## Tópicos relacionados

* [Selecionar e gerenciar suas redes de publicidade](select-and-manage-your-ad-networks.md)
* [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md)
* [Testar sua implementação de controle de anúncios](test-your-ad-mediation-implementation.md)
* [Enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->



---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Saiba como iniciar um anúncio intersticial usando c#.
title: Código de exemplo de anúncio intersticial em C#

---

# Código de exemplo de anúncio intersticial em C\# #  


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico mostra como iniciar um anúncio intersticial usando C#. Para obter instruções passo a passo que ilustram como configurar seu projeto para usar esse código, consulte [anúncios intersticiais](interstitial-ads.md). Para um projeto de exemplo completo que demonstra como adicionar anúncios intersticiais de vídeo a um aplicativo XAML em C#, consulte os [Exemplos de publicidade no GitHub](http://aka.ms/githubads).


## Exemplo de código

Esse código de exemplo mostra um arquivo de código MainPage.xaml.cs que implementa um anúncio intersticial. Esse código pressupõe que o arquivo MainPage.xaml tenha um botão funcional com um evento de **clique** que aciona e é manipulado por um método chamado **button_Click**. Esse código inicia o anúncio intersticial quando o evento de **clique** no botão é acionado.

Substitua o texto nas variáveis **AppID** e **AdUnitId** por valores dinâmicos antes de enviar seu aplicativo para a Loja. Para obter mais informações, consulte [Configurar unidades de anúncios em seu aplicativo](set-up-ad-units-in-your-app.md).

``` syntax
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Microsoft.Advertising.WinRT.UI;


namespace BasicCSharpInterstitialUWP
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        InterstitialAd MyVideoAd = new InterstitialAd();

        public MainPage()
        {
            this.InitializeComponent();
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Define ApplicationId and AdUnitId.
            // Test values are shown here. Replace the test values with live values before submitting the app to the Store.
            var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
            var MyAdUnitId = "11389925";

            MyVideoAd.AdReady += MyVideoAd_AdReady;
            MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
            MyVideoAd.Completed += MyVideoAd_Completed;
            MyVideoAd.Cancelled += MyVideoAd_Cancelled;

            // Pre-fetch an ad 30-60 seconds before you need it.
            MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
        }

        void MyVideoAd_AdReady(object sender, object e)
        {
            if ((InterstitialAdState.Ready) == (MyVideoAd.State))
            {
                MyVideoAd.Show();
            }
        }

        void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
        {
            // Add your code here.
        }

        void MyVideoAd_Completed(object sender, object e)
        {
            // Add your code here.
        }

        void MyVideoAd_Cancelled(object sender, object e)
        {
            // Add your code here.  
        }
    }
}
```

 
## Tópicos relacionados

* [Exemplos de publicidade no GitHub](http://aka.ms/githubads)
 


<!--HONumber=May16_HO2-->



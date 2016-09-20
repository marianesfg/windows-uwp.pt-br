---
author: mcleanbyron
ms.assetid: 9621641A-7462-425D-84CC-101877A738DA
description: Saiba mais sobre como migrar do AdMediatorControl para o AdControl em seus aplicativos UWP.
title: Migrar do AdMediatorControl para o AdControl para aplicativos UWP
translationtype: Human Translation
ms.sourcegitcommit: 07baa54990ec31dc0cb9c289f9f0222754da9d7c
ms.openlocfilehash: 3abef943530cc756de117edccc5ab16e5f178604

---

# Migrar do AdMediatorControl para o AdControl para aplicativos UWP

As versões anteriores do SDK de publicidade da Microsoft permitiam que os aplicativos da Plataforma Universal do Windows (UWP) exibissem anúncios em faixa usando a classe **AdMediatorControl**, o que permitia que os desenvolvedores otimizassem sua receita de anúncios exibindo anúncios em faixa de nossas redes de parceiros (AOL e AppNexus) e do AdDuplex. O [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) não dá mais suporte à classe **AdMediatorControl**. Se você tem um aplicativo existente que usa a classe **AdMediatorControl** de um SDK anterior e quer migrá-lo para um aplicativo UWP que usa o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk), siga as instruções deste artigo para atualizar seu código para usar a classe **AdControl**, em vez da classe **AdMediatorControl**. Opcionalmente, você pode configurar seu aplicativo para mediar anúncios com AdDuplex, usando uma abordagem ponderada ou classificada.

>**Observação**&nbsp;&nbsp;Os exemplos de código neste artigo são fornecidos somente para fins de ilustração. Talvez seja necessário fazer ajustes nos exemplos de código para que funcionem em seu aplicativo.

## Pré-requisitos

* Um aplicativo UWP que está usando o AdMediatorControl e está publicado na Windows Store.
* Um computador de desenvolvimento com o Visual Studio 2015 e o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) instalados.
* Se você quiser mediar anúncios com AdDuplex, deverá ter também o [AdDuplex Windows 10 SDK](https://visualstudiogallery.msdn.microsoft.com/6930860a-e64b-4b46-9d72-62d7fddda077) instalado no computador de desenvolvimento.

  >**Observação**&nbsp;&nbsp;Como alternativa para a execução do instalador do AdDuplex SDK do link acima, você pode instalar as bibliotecas AdDuplex para seu projeto de aplicativo UWP no Visual Studio 2015. Com seu projeto de aplicativo UWP aberto no Visual Studio 2015, clique em **Projeto** > **Gerenciar Pacotes NuGet**, procure o pacote NuGet denominado **AdDuplexWin10** e instale o pacote.

## Etapa 1: recuperar as IDs de seus aplicativos e unidades de anúncio

Quando você migrar seu código para usar a classe **AdControl**, deverá saber as IDs de seus aplicativos e unidades de anúncio. A melhor maneira de obter as IDs mais recentes é recuperá-las de seu arquivo de configuração de controle.

1. Entre no painel do Centro de Desenvolvimento do Windows e clique no aplicativo que está usando **AdMediatorControl** atualmente.
2. Clique em **Monetização** e em **Monetizar com anúncios**.
3. Na seção **Controle de anúncios do Windows**, clique no link **Baixar configuração de controle** e abra o arquivo AdMediator.config em um editor de texto, como o Bloco de Notas.
4. No arquivo, localize o elemento ```<AdAdapterInfo>``` com o elemento filho ```<Name>MicrosoftAdvertising</Name>```. Esta seção contém a configuração para anúncios pagos da Microsoft.
5. Neste elemento ```<AdAdapterInfo>```, localize os elementos ```<Property>``` que contêm elementos ```<Key>``` com os valores **WApplicationId** e **WAdUnitId**. No exemplo a seguir, os valores dos elementos ```<Value>``` são exemplos.

  ```xml
  <Metadata>
      <Property>
          <Key>WApplicationId</Key>
          <Value>364d4938-c0f5-4c3d-8aae-090206211dc9</Value>
      </Property>
      <Property>
          <Key>WAdUnitId</Key>
          <Value>301568</Value>
      </Property>
  </Metadata>
  ```
6. Copie ambos os valores nesses elementos ```<Value>``` para uso posterior. Esses valores contêm a ID do aplicativo e a ID da unidade de anúncio não móvel para anúncios pagos da Microsoft.
5. No mesmo elemento ```<AdAdapterInfo>```, localize os elementos ```<Property>``` que contêm elementos ```<Key>``` com os valores **MApplicationId** e **MAdUnitId**. No exemplo a seguir, os valores dos elementos ```<Value>``` são exemplos.

  ```xml
  <Metadata>
      <Property>
          <Key>MApplicationId</Key>
          <Value>e2cf8388-7018-4a11-8ab0de90f2a7a401</Value>
      </Property>
      <Property>
          <Key>MAdUnitId</Key>
          <Value>301056</Value>
      </Property>
  </Metadata>
  ```
6. Copie ambos os valores nos elementos ```<Value>``` para uso posterior. Esses valores contêm a ID do aplicativo e a ID da unidade de anúncio móvel para anúncios pagos da Microsoft.
7. Se você usar [anúncios domésticos](../publish/about-house-ads.md), localize o elemento ```<AdAdapterInfo>``` com o elemento filho ```<Name>MicrosoftAdvertisingHouse</Name>```. Nesse elemento, localize os elementos ```<Key>``` com os valores **MAdUnitId** e **WAdUnitId** e salve os valores dos elementos ```<Value>``` correspondentes para uso posterior. Esses são as IDs das unidades de anúncio móveis e não móveis para anúncios domésticos da Microsoft, respectivamente.
8. Se você usar AdDuplex, localize o elemento ```<AdAdapterInfo>``` com o elemento filho ```<Name>AdDuplex</Name>```. Nesse elemento, localize os elementos ```<Key>``` com os valores **AppKey** e **AdUnitId** e salve os valores dos elementos ```<Value>``` correspondentes para uso posterior. Esses valores são sua chave de aplicativo AdDuplex e a ID da unidade de anúncio, respectivamente.

## Etapa 2: atualizar o código de seu aplicativo

Agora que você tem as IDs de seus aplicativos e unidades de anúncio, já pode atualizar o código de seu aplicativo para usar **AdControl** em vez de **AdMediatorControl**.

### Apenas anúncios pagos da Microsoft

Se você usa apenas anúncios pagos da Microsoft em sua configuração de controle de anúncios, siga estas etapas.

  >**Observação**&nbsp;&nbsp;Estas etapas presumem que a página do aplicativo em que você quer exibir anúncios contenha uma grade vazia chamada **myAdGrid**, por exemplo: ```<Grid x:Name="myAdGrid"/>```. Nestas etapas, você vai criar e configurar os controles de anúncios inteiramente em código, em vez de XAML.

1. No Visual Studio, abra o projeto de seu aplicativo UWP.
2.  Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**.
No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).
3. No **Gerenciador de Referências**, clique em OK.
4. Remova a declaração **AdMediatorControl** do XAML e remova qualquer código que use esse objeto **AdMediatorControl**, inclusive os manipuladores de eventos relacionados.
5. Abra o arquivo de código para a **página** do aplicativo na qual você deseja exibir anúncios.
6. Adicione a instrução a seguir à parte superior do arquivo de código, caso ainda não exista.

  ```csharp
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. Adicione as declarações de constantes a seguir à classe **Page**.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID = "<tbd>";
  ```
7. Para cada uma destas declarações de constantes, substitua os valores ```<tbd>```:
  * **AD_WIDTH** e **AD_HEIGHT**: atribua a um dos [tamanhos de anúncio compatíveis com anúncios em faixa]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **WAPPLICATIONID** e **WADUNITID**: atribua aos valores **WApplicationId** e **WAdUnitId** para anúncios pagos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esses valores são para a unidade de anúncio não móvel para anúncios pagos).
  * **MAPPLICATIONID** e **MADUNITID**: atribua aos valores **MApplicationId** e **MAdUnitId** para anúncios pagos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esses valores são para a unidade de anúncio não móvel para anúncios pagos).

8. Adicione as declarações de variáveis a seguir à classe **Page**.

  ```csharp
  // Declare an AdControl.
  private AdControl myAdControl = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myAppId = WAPPLICATIONID;
  private string myAdUnitId = WADUNITID;
  ```
5. Adicione o código a seguir ao construtor da classe **Page**, após a chamada para o método **InitializeComponent()**.

  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myAppId = MAPPLICATIONID;
      myAdUnitId = MADUNITID;
  }

  // Initialize the AdControl.
  myAdControl = new AdControl();
  myAdControl.ApplicationId = myAppId;
  myAdControl.AdUnitId = myAdUnitId;
  myAdControl.Width = AD_WIDTH;
  myAdControl.Height = AD_HEIGHT;
  myAdControl.IsAutoRefreshEnabled = true;

  myAdGrid.Children.Add(myAdControl);
  ```  

### Anúncios pagos da Microsoft, anúncios domésticos e AdDuplex

Se você usa anúncios domésticos da Microsoft ou AdDuplex, bem como anúncios pagos da Microsoft, e quer continuar a controlar anúncios com o AdDuplex, siga as etapas desta seção. Os exemplos de código dão suporte ao AdDuplex e a anúncios domésticos da Microsoft. Se você usa o AdDuplex, mas não anúncios domésticos da Microsoft ou vice-versa, remova o código que não se aplica ao seu cenário.

  >**Observação**&nbsp;&nbsp;Estas etapas presumem que a página do aplicativo em que você quer exibir anúncios contenha uma grade vazia chamada **myAdGrid**, por exemplo: ```<Grid x:Name="myAdGrid"/>```. Nestas etapas, você vai criar e configurar os controles de anúncios inteiramente em código, em vez de XAML.

1. No Visual Studio, abra o projeto de seu aplicativo UWP.
2.  Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**.
No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).
3. No **Gerenciador de Referências**, clique em OK.
4. Remova a declaração **AdMediatorControl** do XAML e remova qualquer código que use esse objeto **AdMediatorControl**, inclusive os manipuladores de eventos relacionados.
5. Abra o arquivo de código para a **página** do aplicativo na qual você deseja exibir anúncios.
6. Adicione as instruções a seguir à parte superior do arquivo de código, caso ainda não existam.

  ```csharp
  using Windows.System.UserProfile;
  using Microsoft.Advertising.WinRT.UI;
  using Windows.System.Profile;
  ```
7. Adicione as declarações de constantes a seguir à classe **Page**.

  ```csharp
  private const int AD_WIDTH = <tbd>;
  private const int AD_HEIGHT = <tbd>;
  private const int HOUSE_AD_WEIGHT = <tbd>;
  private const int AD_REFRESH_SECONDS = 35;
  private const int MAX_ERRORS_PER_REFRESH = 3;
  private const string WAPPLICATIONID = "<tbd>";
  private const string WADUNITID_PAID = "<tbd>";
  private const string WADUNITID_HOUSE = "<tbd>";
  private const string MAPPLICATIONID = "<tbd>";
  private const string MADUNITID_PAID = "<tbd>";
  private const string MADUNITID_HOUSE = "<tbd>";
  private const string ADDUPLEX_APPKEY = "<tbd>";
  private const string ADDUPLEX_ADUNIT = "<tbd>";
  ```
4. Para cada uma das declarações de constantes que é atribuída a ```<tbd>```, substitua ```<tbd>``` pelo valor real para seu cenário.
  * **AD_WIDTH** e **AD_HEIGHT**: atribua a um dos [tamanhos de anúncio compatíveis com anúncios em faixa]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **HOUSE_AD_WEIGHT**: atribua a um inteiro de 0 a 100 que especifique o valor de peso que você deseja aplicar aos anúncios domésticos da Microsoft em comparação aos anúncios pagos da Microsoft (onde 0 é para nunca mostrar anúncios domésticos e 100 é para sempre mostrar anúncios domésticos).
  * **WAPPLICATIONID** e **WADUNITID_PAID**: atribua aos valores **WApplicationId** e **WAdUnitId** para anúncios pagos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esses valores são para a unidade de anúncio não móvel para anúncios pagos).
  * **WADUNITID_HOUSE**: atribua a **WAdUnitId** para anúncios domésticos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esse valor é para a unidade de anúncio não móvel para anúncios domésticos).
  * **MAPPLICATIONID** e **MADUNITID_PAID**: atribua aos valores **MApplicationId** e **MAdUnitId** para anúncios pagos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esses valores são para a unidade de anúncio não móvel para anúncios pagos).
  * **MADUNITID_HOUSE**: atribua a **MAdUnitId** para anúncios domésticos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esse valor é para a unidade de anúncio móvel para anúncios domésticos).
  * **ADDUPLEX_APPKEY** e **ADDUPLEX_ADUNIT**: atribua aos valores de chave de aplicativo e ID da unidade de anúncio AdDuplex que você recuperou anteriormente do arquivo de configuração de controle.
5. Adicione as declarações de variáveis a seguir à classe **Page**.
  ```csharp
  // Dispatch timer to fire at each ad refresh interval.
  private DispatcherTimer myAdRefreshTimer = new DispatcherTimer();

  // Global variables used for mediation decisions.
  private Random randomGenerator = new Random();
  private int errorCountCurrentRefresh = 0;  // Prevents infinite redirects.
  private int adDuplexWeight = 0;            // Will be set by GetAdDuplexWeight().

  // Microsoft and AdDuplex controls for banner ads.
  private AdControl myMicrosoftBanner = null;
  private AdDuplex.AdControl myAdDuplexBanner = null;

  // Application ID and ad unit ID values for Microsoft advertising. By default,
  // assign these to non-mobile ad unit info.
  private string myMicrosoftAppId = WAPPLICATIONID;
  private string myMicrosoftPaidUnitId = WADUNITID_PAID;
  private string myMicrosoftHouseUnitId = WADUNITID_HOUSE;
  ```
5. Adicione o código a seguir ao construtor da classe **Page**, após a chamada para o método **InitializeComponent()**.
  ```csharp
  myAdGrid.Width = AD_WIDTH;
  myAdGrid.Height = AD_HEIGHT;
  adDuplexWeight = GetAdDuplexWeight();
  RefreshBanner();

  // Start the timer to refresh the banner at the desired interval.
  myAdRefreshTimer.Interval = new TimeSpan(0, 0, AD_REFRESH_SECONDS);
  myAdRefreshTimer.Tick += myAdRefreshTimer_Tick;
  myAdRefreshTimer.Start();

  // For mobile device families, use the mobile ad unit info.
  if ("Windows.Mobile" == AnalyticsInfo.VersionInfo.DeviceFamily)
  {
      myMicrosoftAppId = MAPPLICATIONID;
      myMicrosoftPaidUnitId = MADUNITID_PAID;
      myMicrosoftHouseUnitId = MADUNITID_HOUSE;
  }
  ```
6. Por fim, adicione os métodos a seguir à classe **Page**. Esses métodos instanciam os objetos Microsoft **AdControl** e AdDuplex **AdControl**, e eles usam um gerador de números aleatórios em conjunto com os valores de peso fornecidos para atualizar os anúncios em faixa nesses controles em intervalos regulares.
  ```csharp
  private int GetAdDuplexWeight()
  {
      // TODO: Change this logic to fit your needs.
      // This example uses Microsoft ads first in Canada and Mexico, then
      // AdDuplex as fallback. In France, AdDuplex is first. In other regions,
      // this example uses a weighted average approach, with 50% to AdDuplex.

      int returnValue = 0;
      switch (GlobalizationPreferences.HomeGeographicRegion)
      {
          case "CA":
          case "MX":
              returnValue = 0;
              break;
          case "FR":
              returnValue = 100;
              break;
          default:
              returnValue = 50;
              break;
      }
      return returnValue;
  }

  private void ActivateMicrosoftBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Use random number generator and house ads weight to determine whether
      // to use paid ads or house ads. Paid is the default. You could alternatively
      // write a method similar to GetAdDuplexWeight and override by region.
      string myAdUnit = myMicrosoftPaidUnitId;
      int houseWeight = HOUSE_AD_WEIGHT;
      int randomInt = randomGenerator.Next(0, 100);
      if (randomInt < houseWeight)
      {
          myAdUnit = myMicrosoftHouseUnitId;
      }

      // Hide the AdDuplex control if it is showing.
      if (null != myAdDuplexBanner)
      {
          myAdDuplexBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the Microsoft control.
      if (null == myMicrosoftBanner)
      {
          myMicrosoftBanner = new AdControl();
          myMicrosoftBanner.ApplicationId = myMicrosoftAppId;
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Width = AD_WIDTH;
          myMicrosoftBanner.Height = AD_HEIGHT;
          myMicrosoftBanner.IsAutoRefreshEnabled = false;

          myMicrosoftBanner.AdRefreshed += myMicrosoftBanner_AdRefreshed;
          myMicrosoftBanner.ErrorOccurred += myMicrosoftBanner_ErrorOccurred;

          myAdGrid.Children.Add(myMicrosoftBanner);
      }
      else
      {
          myMicrosoftBanner.AdUnitId = myAdUnit;
          myMicrosoftBanner.Visibility = Visibility.Visible;
          myMicrosoftBanner.Refresh();
      }
  }

  private void ActivateAdDuplexBanner()
  {
      // Return if you hit the error limit for this refresh interval.
      if (errorCountCurrentRefresh >= MAX_ERRORS_PER_REFRESH)
      {
          myAdGrid.Visibility = Visibility.Collapsed;
          return;
      }

      // Hide the Microsoft control if it is showing.
      if (null != myMicrosoftBanner)
      {
          myMicrosoftBanner.Visibility = Visibility.Collapsed;
      }

      // Initialize or display the AdDuplex control.
      if (null == myAdDuplexBanner)
      {
          myAdDuplexBanner = new AdDuplex.AdControl();
          myAdDuplexBanner.AppKey = ADDUPLEX_APPKEY;
          myAdDuplexBanner.AdUnitId = ADDUPLEX_ADUNIT;
          myAdDuplexBanner.Width = AD_WIDTH;
          myAdDuplexBanner.Height = AD_HEIGHT;
          myAdDuplexBanner.RefreshInterval = AD_REFRESH_SECONDS;

          myAdDuplexBanner.AdLoaded += myAdDuplexBanner_AdLoaded;
          myAdDuplexBanner.AdCovered += myAdDuplexBanner_AdCovered;
          myAdDuplexBanner.AdLoadingError += myAdDuplexBanner_AdLoadingError;
          myAdDuplexBanner.NoAd += myAdDuplexBanner_NoAd;

          myAdGrid.Children.Add(myAdDuplexBanner);
      }
      myAdDuplexBanner.Visibility = Visibility.Visible;
  }

  private void myAdRefreshTimer_Tick(object sender, object e)
  {
      RefreshBanner();
  }

  private void RefreshBanner()
  {
      // Reset the error counter for this refresh interval and
      // make sure the ad grid is visible.
      errorCountCurrentRefresh = 0;
      myAdGrid.Visibility = Visibility.Visible;

      // Display ad from AdDuplex.
      if (100 == adDuplexWeight)
      {
          ActivateAdDuplexBanner();
      }
      // Display Microsoft ad.
      else if (0 == adDuplexWeight)
      {
          ActivateMicrosoftBanner();
      }
      // Use weighted approach.
      else
      {
          int randomInt = randomGenerator.Next(0, 100);
          if (randomInt < adDuplexWeight) ActivateAdDuplexBanner();
          else ActivateMicrosoftBanner();
      }
  }

  private void myMicrosoftBanner_AdRefreshed(object sender, RoutedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myMicrosoftBanner_ErrorOccurred(object sender, AdErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateAdDuplexBanner();
  }

  private void myAdDuplexBanner_AdLoaded(object sender, AdDuplex.Banners.Models.BannerAdLoadedEventArgs e)
  {
      // Add your code here as necessary.
  }

  private void myAdDuplexBanner_NoAd(object sender, AdDuplex.Common.Models.NoAdEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdLoadingError(object sender, AdDuplex.Common.Models.AdLoadingErrorEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }

  private void myAdDuplexBanner_AdCovered(object sender,   AdDuplex.Banners.Core.AdCoveredEventArgs e)
  {
      errorCountCurrentRefresh++;
      ActivateMicrosoftBanner();
  }
  ```



<!--HONumber=Sep16_HO1-->



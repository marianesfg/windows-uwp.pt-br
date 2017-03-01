---
author: mcleanbyron
ms.assetid: 9621641A-7462-425D-84CC-101877A738DA
description: Saiba mais sobre como migrar do AdMediatorControl para o AdControl em seus apps UWP.
title: Migrar do AdMediatorControl para o AdControl para apps UWP
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, propaganda, AdMediatorControl, AdControl, migrar"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 83235595a6a7f9e9b0b5d8de154d6e5d8a8db8ef
ms.lasthandoff: 02/07/2017

---

# <a name="migrate-from-admediatorcontrol-to-adcontrol-for-uwp-apps"></a>Migrar do AdMediatorControl para o AdControl para apps UWP

As versões anteriores do SDK de publicidade da Microsoft permitiam que os apps da Plataforma Universal do Windows (UWP) exibissem anúncios em faixa usando a classe **AdMediatorControl**, o que permitia que os desenvolvedores otimizassem sua receita de anúncios exibindo anúncios em faixa de nossas redes de parceiros (AOL e AppNexus) e do AdDuplex. O [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) não dá mais suporte à classe **AdMediatorControl**. Se você tem um app existente que usa a classe **AdMediatorControl** de um SDK anterior e quer migrá-lo para um app UWP que usa o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk), siga as instruções deste artigo para atualizar seu código para usar a classe **AdControl**, em vez da classe **AdMediatorControl**. Opcionalmente, você pode configurar seu app para mediar anúncios com AdDuplex, usando uma abordagem ponderada ou classificada.

>**Observação**&nbsp;&nbsp;Os exemplos de código neste artigo são fornecidos somente para fins de ilustração. Talvez seja necessário fazer ajustes nos exemplos de código para que funcionem em seu app.

## <a name="prerequisites"></a>Pré-requisitos

* Um app UWP que está usando o AdMediatorControl e está publicado na Windows Store.
* Um computador de desenvolvimento com o Visual Studio 2015 e o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) instalados.
* Se você quiser mediar anúncios com AdDuplex, deverá ter também o [AdDuplex Windows 10 SDK](https://visualstudiogallery.msdn.microsoft.com/6930860a-e64b-4b46-9d72-62d7fddda077) instalado no computador de desenvolvimento.

  >**Observação**&nbsp;&nbsp;Como alternativa para a execução do instalador do AdDuplex SDK do link acima, você pode instalar as bibliotecas AdDuplex para seu projeto de app UWP no Visual Studio 2015. Com seu projeto de app UWP aberto no Visual Studio 2015, clique em **Projeto** > **Gerenciar Pacotes NuGet**, procure o pacote NuGet denominado **AdDuplexWin10** e instale o pacote.

## <a name="step-1-retrieve-your-application-ids-and-ad-unit-ids"></a>Etapa 1: recuperar as IDs de seus apps e unidades de anúncio

Quando você migrar seu código para usar a classe **AdControl**, deverá saber as IDs de seus apps e unidades de anúncio. A melhor maneira de obter as IDs mais recentes é recuperá-las de seu arquivo de configuração de controle.

1. Entre no painel do Centro de Desenvolvimento do Windows e clique no app que está usando **AdMediatorControl** atualmente.
2. Clique em **Monetização** e em **Monetizar com anúncios**.
3. Na seção **Controle de anúncios do Windows**, clique no link **Baixar configuração de controle** e abra o arquivo AdMediator.config em um editor de texto, como o Bloco de Notas.
4. No arquivo, localize o elemento ```<AdAdapterInfo>``` com o elemento filho ```<Name>MicrosoftAdvertising</Name>```. Esta seção contém a configuração para anúncios pagos da Microsoft.
5. Neste elemento ```<AdAdapterInfo>```, localize os elementos ```<Property>``` que contêm elementos ```<Key>``` com os valores **WApplicationId** e **WAdUnitId**. No exemplo a seguir, os valores dos elementos ```<Value>``` são exemplos.

  > [!div class="tabbedCodeSnippets"]
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

6. Copie ambos os valores nesses elementos ```<Value>``` para uso posterior. Esses valores contêm a ID do app e a ID da unidade de anúncio não móvel para anúncios pagos da Microsoft.
5. No mesmo elemento ```<AdAdapterInfo>```, localize os elementos ```<Property>``` que contêm elementos ```<Key>``` com os valores **MApplicationId** e **MAdUnitId**. No exemplo a seguir, os valores dos elementos ```<Value>``` são exemplos.

  > [!div class="tabbedCodeSnippets"]
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

6. Copie ambos os valores nos elementos ```<Value>``` para uso posterior. Esses valores contêm a ID do app e a ID da unidade de anúncio móvel para anúncios pagos da Microsoft.
7. Se você usar [anúncios domésticos](../publish/about-house-ads.md), localize o elemento ```<AdAdapterInfo>``` com o elemento filho ```<Name>MicrosoftAdvertisingHouse</Name>```. Nesse elemento, localize os elementos ```<Key>``` com os valores **MAdUnitId** e **WAdUnitId** e salve os valores dos elementos ```<Value>``` correspondentes para uso posterior. Esses são as IDs das unidades de anúncio móveis e não móveis para anúncios domésticos da Microsoft, respectivamente.
8. Se você usar AdDuplex, localize o elemento ```<AdAdapterInfo>``` com o elemento filho ```<Name>AdDuplex</Name>```. Nesse elemento, localize os elementos ```<Key>``` com os valores **AppKey** e **AdUnitId** e salve os valores dos elementos ```<Value>``` correspondentes para uso posterior. Esses valores são sua chave de app AdDuplex e a ID da unidade de anúncio, respectivamente.

## <a name="step-2-update-your-app-code"></a>Etapa 2: atualizar o código de seu app

Agora que você tem as IDs de seus apps e unidades de anúncio, já pode atualizar o código de seu app para usar **AdControl** em vez de **AdMediatorControl**.

### <a name="microsoft-paid-ads-only"></a>Apenas anúncios pagos da Microsoft

Se você usa apenas anúncios pagos da Microsoft em sua configuração de controle de anúncios, siga estas etapas.

  >**Observação**&nbsp;&nbsp;Estas etapas presumem que a página do app em que você quer exibir anúncios contenha uma grade vazia chamada **myAdGrid**, por exemplo: ```<Grid x:Name="myAdGrid"/>```. Nestas etapas, você vai criar e configurar os controles de anúncios inteiramente em código, em vez de XAML.

1. No Visual Studio, abra o projeto de seu app UWP.
2.  Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**.
No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).
3. No **Gerenciador de Referências**, clique em OK.
4. Remova a declaração **AdMediatorControl** do XAML e remova qualquer código que use esse objeto **AdMediatorControl**, inclusive os manipuladores de eventos relacionados.
5. Abra o arquivo de código para a **página** do app na qual você deseja exibir anúncios.
6. Adicione a instrução a seguir à parte superior do arquivo de código, caso ainda não exista.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[TrialVersion](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet1)]

7. Adicione as declarações de constantes a seguir à classe **Page**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[TrialVersion](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet2)]

7. Para cada uma destas declarações de constantes, substitua os valores conforme descrito abaixo:

  * **AD_WIDTH** e **AD_HEIGHT**: atribua a um dos [tamanhos de anúncio compatíveis com anúncios em faixa]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **WAPPLICATIONID** e **WADUNITID**: atribua aos valores **WApplicationId** e **WAdUnitId** para anúncios pagos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esses valores são para a unidade de anúncio não móvel para anúncios pagos).
  * **MAPPLICATIONID** e **MADUNITID**: atribua aos valores **MApplicationId** e **MAdUnitId** para anúncios pagos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esses valores são para a unidade de anúncio não móvel para anúncios pagos).

8. Adicione as declarações de variáveis a seguir à classe **Page**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet3)]

5. Adicione o código a seguir ao construtor da classe **Page**, após a chamada para o método **InitializeComponent()**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/MainPage.xaml.cs#Snippet4)]

### <a name="microsoft-paid-ads-house-ads-and-adduplex"></a>Anúncios pagos da Microsoft, anúncios domésticos e AdDuplex

Se você usa anúncios domésticos da Microsoft ou AdDuplex, bem como anúncios pagos da Microsoft, e quer continuar a controlar anúncios com o AdDuplex, siga as etapas desta seção. Os exemplos de código dão suporte ao AdDuplex e a anúncios domésticos da Microsoft. Se você usa o AdDuplex, mas não anúncios domésticos da Microsoft ou vice-versa, remova o código que não se aplica ao seu cenário.

  >**Observação**&nbsp;&nbsp;Estas etapas presumem que a página do app em que você quer exibir anúncios contenha uma grade vazia chamada **myAdGrid**, por exemplo: ```<Grid x:Name="myAdGrid"/>```. Nestas etapas, você vai criar e configurar os controles de anúncios inteiramente em código, em vez de XAML.

1. No Visual Studio, abra o projeto de seu app UWP.
2.  Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**.
No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).
3. No **Gerenciador de Referências**, clique em OK.
4. Remova a declaração **AdMediatorControl** do XAML e remova qualquer código que use esse objeto **AdMediatorControl**, inclusive os manipuladores de eventos relacionados.
5. Abra o arquivo de código para a **página** do app na qual você deseja exibir anúncios.
6. Adicione as instruções a seguir à parte superior do arquivo de código, caso ainda não existam.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet1)]

7. Adicione as declarações de constantes a seguir à classe **Page**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet2)]

4. Para essas declarações de constantes, substitua os valores conforme descrito abaixo:

  * **AD_WIDTH** e **AD_HEIGHT**: atribua a um dos [tamanhos de anúncio compatíveis com anúncios em faixa]( https://msdn.microsoft.com/windows/uwp/monetize/supported-ad-sizes-for-banner-ads).
  * **HOUSE_AD_WEIGHT**: atribua a um inteiro de 0 a 100 que especifique o valor de peso que você deseja aplicar aos anúncios domésticos da Microsoft em comparação aos anúncios pagos da Microsoft (onde 0 é para nunca mostrar anúncios domésticos e 100 é para sempre mostrar anúncios domésticos).
  * **WAPPLICATIONID** e **WADUNITID_PAID**: atribua aos valores **WApplicationId** e **WAdUnitId** para anúncios pagos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esses valores são para a unidade de anúncio não móvel para anúncios pagos).
  * **WADUNITID_HOUSE**: atribua a **WAdUnitId** para anúncios domésticos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esse valor é para a unidade de anúncio não móvel para anúncios domésticos).
  * **MAPPLICATIONID** e **MADUNITID_PAID**: atribua aos valores **MApplicationId** e **MAdUnitId** para anúncios pagos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esses valores são para a unidade de anúncio não móvel para anúncios pagos).
  * **MADUNITID_HOUSE**: atribua a **MAdUnitId** para anúncios domésticos da Microsoft que você recuperou anteriormente do arquivo de configuração de controle (esse valor é para a unidade de anúncio móvel para anúncios domésticos).
  * **ADDUPLEX_APPKEY** e **ADDUPLEX_ADUNIT**: atribua aos valores de chave de app e ID da unidade de anúncio AdDuplex que você recuperou anteriormente do arquivo de configuração de controle.

  >**Observação**&nbsp;&nbsp;Não altere os valores **AD_REFRESH_SECONDS** e **MAX_ERRORS_PER_REFRESH** mostrados no exemplo anterior.

5. Adicione as declarações de variáveis a seguir à classe **Page**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet3)]

5. Adicione o código a seguir ao construtor da classe **Page**, após a chamada para o método **InitializeComponent()**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet4)]

6. Por fim, adicione os métodos a seguir à classe **Page**. Esses métodos instanciam os objetos Microsoft **AdControl** e AdDuplex **AdControl**, e eles usam um gerador de números aleatórios em conjunto com os valores de peso fornecidos para atualizar os anúncios em faixa nesses controles em intervalos regulares.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[AdControl](./code/AdvertisingSamples/MigrateToAdControl/cs/ExamplePage1.xaml.cs#Snippet5)]


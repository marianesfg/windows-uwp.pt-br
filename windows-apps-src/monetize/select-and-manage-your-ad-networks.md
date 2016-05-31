---
author: mcleanbyron
ms.assetid: 86D9D3CF-8FDC-4B67-881B-DF33A1BEE8BF
description: Antes de usar o controle de anúncios, você precisará configurar as contas com cada rede de publicidade que você gostaria de usar em seus aplicativos.
title: Selecionar e gerenciar suas redes de publicidade
---

# Selecionar e gerenciar suas redes de publicidade


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Antes de usar o controle de anúncios, você precisará configurar as contas com cada rede de publicidade que você gostaria de usar em seus aplicativos. É recomendável fazer isso antes de adicionar o controle de anúncios ao seu projeto do Microsoft Visual Studio. Também recomendamos que você configure tantas contas com redes de publicidade quanto possível, para que você tenha o máximo de flexibilidade a fim de otimizar o desempenho do controle de anúncios.

## Redes de publicidade e tipos de projeto compatíveis

Atualmente, o controle de anúncios é compatível com as redes de publicidade e os tipos de projetos a seguir.

|  Rede de publicidade    | Aplicativo UWP (Plataforma Universal do Windows) usando o C# ou o Visual Basic com XAML | Aplicativo Windows 8.1 usando o C# ou o Visual Basic com XAML | Aplicativo Windows Phone 8.1 usando o C# ou o Visual Basic com XAML | Aplicativo Silverlight para Windows Phone 8.x |               
|-------------------------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------|-----------------------------------|---------------|
| [Microsoft](#microsoft)                         | **Com suporte**                                      | **Com suporte**                                            | **Com suporte**                     | **Com suporte** |
| [AdDuplex](#adduplex)                                                   | **Com suporte**                                      | **Com suporte**                                            | **Com suporte**                     | **Com suporte** |
| [Smaato](#smaato)                                                       | Sem suporte                                      | **Com suporte**                                            | **Com suporte**                     | **Com suporte** |
| [AdMob (Google)](#admob)                                                | Sem suporte                                      | Sem suporte                                            | Sem suporte                     | **Com suporte** |
| [Inneractive](#inneractive)                                             | Sem suporte                                      | Sem suporte                                            | Sem suporte                     | **Com suporte** |
| [Vserv VMAX](#vserv)                                                    | Sem suporte                                      | Sem suporte                                            | **Com suporte**                     | Sem suporte | 

Alguns tipos de projeto, como C++ com XAML e JavaScript com HTML, não são compatíveis atualmente com o controle de anúncios. Para esses tipos de projeto, você pode mostrar anúncios da Microsoft sem usar o controle de anúncios. Para saber mais, consulte [AdControl em XAML e .NET](https://msdn.microsoft.com/library/mt313186.aspx) e [AdControl em HTML 5 e JavaScript](https://msdn.microsoft.com/library/mt313130.aspx).

## Parâmetros específicos e os detalhes de cada rede


Aqui estão os detalhes específicos de que você vai precisar em cada rede de publicidade, incluindo a forma de configurar sua conta e como carregar um aplicativo. Também listamos os parâmetros obrigatórios que você precisa fornecer ao [enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md) (e/ou nos Connected Services para [testar sua implementação do controle de anúncios](test-your-ad-mediation-implementation.md)). Para obter mais detalhes sobre o uso de uma rede de publicidade específica, visite o site.

Além dos parâmetros necessários, cada rede de publicidade também tem parâmetros opcionais que você pode definir através do código em seu aplicativo. Para obter exemplos, consulte [Parâmetros de rede de publicidade opcionais](#optionalparameters) posteriormente neste tópico. Para obter a lista completa dos parâmetros opcionais, consulte a documentação fornecida por cada rede de publicidade. Alguns desses parâmetros serão ignorados ou substituídos pelo controle de anúncios, e esses recursos estão listados nas seções a seguir. Por exemplo, os parâmetros relacionados à localização geralmente são substituídos por informações sobre a localização do cliente que é determinada pela funcionalidade de localização dentro do próprio aplicativo.

Observe que, quando você [adiciona o controle de mediador de anúncios](add-and-use-the-ad-mediator-control.md) e especifica quais redes de publicidade usar, o Visual Studio tenta obter os assemblies necessários por meio de programação para algumas redes de publicidade. Se houver assemblies que não possam ser recuperados automaticamente, você poderá instalá-los usando os links mostrados a seguir para cada rede de publicidade.

### Microsoft

| Site                        | Para configurar os parâmetros da rede de publicidade, use a página [Monetizar com anúncios](https://msdn.microsoft.com/library/windows/apps/mt170658) no [painel do Centro de Desenvolvimento do Windows](https://dev.windows.com/overview).   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Localização do SDK                   | [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk).                                                                                                                                                                                                                         |
| Carregando um aplicativo              | Adicione o controle de anúncios ao seu aplicativo e envie-o para o painel do Centro de Desenvolvimento do Windows.                                                                                                                                                                                                            |
| Parâmetros necessários            | ApplicationId e AdUnitId: esses parâmetros são preenchidos automaticamente para você quando você envia o pacote do aplicativo, com base no conteúdo do seu aplicativo. No entanto, você também pode editar esses parâmetros ao [enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md). <br> <br> Altura e Largura (obrigatórias apenas para Windows Phone 8 Silverlight e Windows Phone 8.1 Silverlight).                                                                                                                                                                                                           |
| Parâmetros substituídos/ignorados | Latitude (substituído)  <br><br> Longitude (substituído) <br><br> AutoRefreshIntervalInSeconds (ignorado) <br><br> IsAutoRefreshEnabled (ignorado) <br><br> IsAutoCollapsedEnabled (ignorado) <br><br> IsEngaged (ignorado) <br><br> IsSuspended (ignorado) |

 

### AdDuplex

| Site                        | [http://adduplex.com](http://go.microsoft.com/fwlink/p/?LinkId=518028)      |
|--------------------------------|-----------------------------------------------------------------------------|
| localização do SDK                   | Primeiro, tente permitir que o controle de anúncios recupere os assemblies por meio dos Connected Services conforme descrito em [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md). Se você precisar baixá-los manualmente, vá para a seção [Ponto de Partida](http://go.microsoft.com/fwlink/p/?LinkId=518029) no site do AdDuplex. |
| Carregando um aplicativo              | No portal do AdDuplex, crie um novo aplicativo.                                                                                                                                                                                                                                                                                                        |
| Parâmetros necessários            | AppKey <br><br> AdUnitId <br><br> Size (necessário somente para Windows 8.1 XAML)  |
| Parâmetros substituídos/ignorados | Latitude (substituído) <br><br> Longitude (substituído) <br><br> AutoRefreshIntervalInSeconds (ignorado) <br><br> IsAutoRefreshEnabled (ignorado) <br><br> IsAutoCollapsedEnabled (ignorado) <br><br> IsEngaged (ignorado) <br><br> IsSuspended (ignorado) |
 

### Smaato

| Site                        | [http://www.smaato.com/](http://go.microsoft.com/fwlink/p/?LinkId=518030)                                                                                                                                                                                                        |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| localização do SDK                   | Primeiro, tente permitir que o controle de anúncios recupere os assemblies por meio dos Connected Services conforme descrito em [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md). Se você precisar baixá-los manualmente, vá para a guia Downloads no portal do Smaato. |
| Carregando um aplicativo              | No portal do Smaato, vá para Meus Espaços e gere um novo Adspace.                                                                                                                                                                                                                |
| Parâmetros necessários            | Pub <br> <br> Adspace <br> <br> Height e Width (necessários somente para XAML do Windows 8.1)  |
| Parâmetros substituídos/ignorados | Gps (substituído)                                                                                                                                                                                                                                                                |

 

### AdMob (Google)

| Site                        | [http://apps.admob.com](http://go.microsoft.com/fwlink/p/?LinkId=518031)                                               |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------|
| localização do SDK                   | Obtenha o SDK do Windows Phone 8 no [site do SDK de anúncios do Google Mobile](http://go.microsoft.com/fwlink/p/?LinkId=518032). |
| Carregando um aplicativo              | No portal do AdMob, selecione Monetizar novo aplicativo.                                                                          |
| Parâmetros necessários            | AdUnitID <br> <br> Formato                                                                                              |
| Parâmetros substituídos/ignorados | Location (substituído)  <br><br> ForceTesting (ignorado) <br><br> Refresh Rate (ignorado)                                |
 

### Inneractive

| Site             | [http://inner-active.com](http://go.microsoft.com/fwlink/p/?LinkId=518035)                                                                                                                                                                                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| localização do SDK        | Primeiro, tente permitir que o controle de anúncios recupere os assemblies por meio dos Connected Services conforme descrito em [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md). Se você precisar baixá-los manualmente, entre em sua conta e vá para a guia SDK no Painel para baixar o SDK do Windows Phone 8. |
| Carregando um aplicativo   | No portal do Inneractive, crie um novo aplicativo.                                                                                                                                                                                                                                                                                           |
| Parâmetros necessários | AppID <br> <br> AdType (IaAdType_Banner ou IaAdType_Text)                                                                               |
 

### Vserv VMAX

| Site             | [http://www.vmax.com](http://www.vmax.com)                                                                                                                                                                                                                                                                         |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Localização do SDK        | Primeiro, tente permitir que o controle de anúncios recupere os assemblies por meio dos Connected Services conforme descrito em [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md). Se você precisar baixá-los manualmente, acesse a seção [SDK](http://go.microsoft.com/fwlink/?LinkId=627078) no site VMAX. |
| Carregando um aplicativo   | Obtenha a ID do Adspot para seu aplicativo no site VMAX (a ID do Adspot é chamada ZoneId nas APIs VMAX).                                                                                                                                                                                                                     |
| Parâmetros necessários | ZoneId                                                                                                                                                                                                                                                                                                                         |12345

 

## Parâmetros de rede de publicidade opcionais


Além dos parâmetros necessários, cada rede de publicidade também tem parâmetros opcionais que você pode definir através do código em seu aplicativo. Para obter a lista completa dos parâmetros opcionais, consulte a documentação fornecida por cada rede de publicidade. Para definir esses parâmetros opcionais em seu código, use a propriedade **AdSdkOptionalParameters** do seu objeto **AdMediatorControl**.

O exemplo a seguir demonstra como configurar a propriedade [CountryOrRegion](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.countryorregion.aspx) do Microsoft Advertising para código de país ou região de duas letras do usuário.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["CountryOrRegion"] = "IN";
```

O exemplo a seguir demonstra como configurar os parâmetros **Width** e **Height** para o Smaato.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 250;
```

## Tópicos relacionados

* [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md)
* [Testar sua implementação de controle de anúncios](test-your-ad-mediation-implementation.md)
* [Enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md)
* [Solucionar problemas de controle de anúncios](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->



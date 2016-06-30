---
author: mcleanbyron
ms.assetid: c0450f7b-5c81-4d8c-92ef-2b1190d18af7
description: "Saiba como usar a classe AdControl para exibir anúncios em faixa em um aplicativo Silverlight para Windows Phone 8.1 ou Windows Phone 8.0."
title: AdControl no Windows Phone Silverlight
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5a12badfb11cfd43c0833522d996da7df73b3d55


---

# AdControl no Windows Phone Silverlight


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este guia passo a passo mostra como usar a classe [AdControl](https://msdn.microsoft.com/library/windows/apps/hh524191.aspx) para exibir anúncios em faixa em um aplicativo Silverlight para Windows Phone 8.1 ou Windows Phone 8.0.

## Pré-requisitos

*  Instale o [SDK do Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk) com o Visual Studio 2015 ou o Visual Studio 2013.


## Adicionar as referências de assembly de publicidade

Os assemblies do Microsoft Advertising para projetos do Windows Phone Silverlight não são instalados localmente com o SDK do Microsoft Store Engagement and Monetization. Antes de começar a atualizar seu código, você deve primeiro usar os **Serviços Conectados** com o suporte à mediação de anúncios no SDK do Microsoft Store Engagement and Monetization para baixar esses assemblies e fazer referência a eles em seu projeto.

1.  No Visual Studio, clique em **Projeto** e **Adicionar Serviço Conectado**.

2.  Na caixa de diálogo **Adicionar Serviço Conectado**, clique em **Ad Mediator** e, em seguida, em **Configurar**.

3.  Clique em **Selecionar redes de anúncios**, selecione **Microsoft Advertising** somente.

    Neste ponto, todos os assemblies necessários do Microsoft Advertising para o Silverlight são baixados para o seu projeto local por meio de pacotes NuGet, e as referências a esses assemblies são adicionadas automaticamente ao seu projeto. Uma referência ao assembly de mediação de anúncios também é adicionada ao seu projeto. Você removerá a referência ao assembly de mediação de anúncios em uma etapa posterior, pois ele não é necessário neste cenário.

4.  Na caixa de diálogo **Selecionar redes de anúncios**, clique em **OK**. Clique em **OK** novamente na página de confirmação **Obtendo status** seguinte e, finalmente, clique em **Adicionar** para fechar a caixa de diálogo **Ad Mediator**.

5.  No **Gerenciador de Soluções**, expanda o nó **Referências**. Clique com o botão direito do mouse em **Microsoft.AdMediator.WindowsPhone81SL.MicrosoftAdvertising** e clique em **Remover**. Esta referência ao assembly não é necessária neste cenário.

## Codificar seu aplicativo


1.  Adicione os seguintes recursos ao nó **Recursos** no arquivo WMAppManifest.xml.

    ``` syntax
    <Capability Name="ID_CAP_IDENTITY_USER"/>
    <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
    <Capability Name="ID_CAP_PHONEDIALER"/>
    ```

    Neste exemplo, seu nó **Recursos** se parece com:

    ``` syntax
        <Capabilities>
          <Capability Name="ID_CAP_NETWORKING"/>
          <Capability Name="ID_CAP_MEDIALIB_AUDIO"/>
          <Capability Name="ID_CAP_MEDIALIB_PLAYBACK"/>
          <Capability Name="ID_CAP_SENSORS"/>
          <Capability Name="ID_CAP_WEBBROWSERCOMPONENT"/>
          <Capability Name="ID_CAP_IDENTITY_USER"/>
          <Capability Name="ID_CAP_MEDIALIB_PHOTO"/>
          <Capability Name="ID_CAP_PHONEDIALER"/>
        </Capabilities>
    ```

2.  (Opcional) Salve seu projeto. Clique no ícone **Salvar Tudo** ou no menu **Arquivo**, clique em **Salvar Tudo**.

3.  Adicione a funcionalidade **Internet (Cliente e Servidor)** ao arquivo Package.appxmanifest em seu projeto. No **Gerenciador de Soluções**, clique duas vezes em **Package.appxmanifest**.

    ![wp81silverlightmarkup\-solutionexplorer\-packageappxmanifest](images/13-b98c2a1a-69c3-4018-be0a-6ce010e703e7.jpg)

    No **Editor**, clique na guia **Recursos**. Marque a caixa **Internet (Cliente e Servidor)**.

4.  (Opcional) Salve seu projeto. Clique no ícone **Salvar Tudo** ou no menu **Arquivo**, clique em **Salvar Tudo**.

5.  Modifique a marcação Silverlight no arquivo MainPage.xaml para incluir o namespace **Microsoft.Advertising.Mobile.UI**.

    ``` syntax
    xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
    ```

    O cabeçalho da sua página terá o código a seguir:

    ``` syntax
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="clr-namespace:Microsoft.Advertising.Mobile.UI;assembly=Microsoft.Advertising.Mobile.UI"
        x:Class="PhoneApp1.MainPage"
    ```

6.  Na marca **Grade**, adicione o código a seguir do **AdControl**. Atribua as propriedades **ApplicationId** e **AdUnitId** aos valores de teste fornecidos em [Valores de modo de teste](test-mode-values.md), e ajuste as propriedades **Height** e **Width** para um dos [tamanhos de anúncio compatíveis com anúncios em faixa](supported-ad-sizes-for-banner-ads.md).

    > **Observação**  
    Você substituirá os valores de teste **ApplicationId** e **AdUnitId** por valores dinâmicos antes de enviar seu aplicativo.

    ``` syntax
    <Grid x:Name="ContentPanel" Grid.Row="1">

      <UI:AdControl
             ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
             AdUnitId="10865270"
             HorizontalAlignment="Left"
             Height="80"
             VerticalAlignment="Top"
             Width="480"/>

    </Grid>
    ```

7.  Compile e execute o seu projeto. Confirme que seu aplicativo exibe um anúncio, semelhante ao seguinte:

    ![wp81silverlight\-emulatorwithad](images/13-8db1492f-ae1d-439b-9b78-bed8e22fe996.jpg)

## Lançar seu aplicativo com anúncios dinâmicos usando o Centro de Desenvolvimento


1.  No painel do Centro de Desenvolvimento, vá para a página **Monetização**&gt;**Monetizar com anúncios** para seu aplicativo e [crie uma unidade autônoma do Microsoft Advertising](../publish/monetize-with-ads.md). Para obter o tipo de unidade de anúncio, especifique **Banner**. Anote o ID da unidade de anúncio e o ID do aplicativo.

2.  Em seu código, substitua os valores da unidade de anúncio de teste (**applicationId** e **adUnitId**) pelos valores dinâmicos gerados no Centro de Desenvolvimento.

3.  [Envie seu aplicativo](../publish/app-submissions.md) para a Loja usando o painel do Centro de Desenvolvimento.

4.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.


 



<!--HONumber=Jun16_HO4-->



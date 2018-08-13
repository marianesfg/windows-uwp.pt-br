---
author: mcleanbyron
description: Saiba como adicionar anúncios nativos ao seu aplicativo UWP.
title: Anúncios nativos
ms.author: mcleans
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anúncios, publicidade, controle de anúncio, anúncio nativo
ms.localizationpriority: medium
ms.openlocfilehash: 5479efef22d31c5a23086b7e596553542e6e9e51
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
ms.locfileid: "1881107"
---
# <a name="native-ads"></a>Anúncios nativos

Um anúncio nativo é um formato de anúncio com base em componente em que cada parte do criativo do anúncio (como título, imagem, descrição e texto do chamada para ação) é entregue ao aplicativo como um elemento individual. Você pode integrar esses elementos ao seu app usando suas próprias fontes, cores, animações e outros componentes de IU para compor uma experiência de usuário discreto que se ajuste à aparência do seu app enquanto também obtém alto rendimento dos anúncios.

Para os anunciantes, os anúncios nativos fornecem posicionamentos de alto desempenho, porque a experiência de anúncios está totalmente integrada ao aplicativo e os usuários, portanto, tendem a interagir mais com esses tipos de anúncios.

> [!NOTE]
> No momento, os anúncios nativos têm suporte apenas para aplicativos UWP baseados em XAML para Windows 10. O suporte para aplicativos UWP escrito usando HTML e JavaScript está planejado para uma versão futura do SDK do Microsoft Advertising.

## <a name="prerequisites"></a>Pré-requisitos

* Instale o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp) com o Visual Studio 2015 ou uma versão posterior do Visual Studio. Para obter instruções de instalação, consulte [este artigo](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-native-ad-into-your-app"></a>Integrar um anúncio nativo ao seu app

Siga estas instruções para integrar um anúncio nativo ao seu app e confirme que sua implementação de anúncios nativos mostra um anúncio de teste.

1. No Visual Studio, abra o projeto ou crie um novo projeto.
    > [!NOTE]
    > Se você estiver usando um projeto existente, abra o arquivo Package. appxmanifest em seu projeto e certifique-se de que o recurso da **Internet (cliente)** está selecionado. Seu aplicativo precisa dessa funcionalidade para receber anúncios de teste e anúncios ativos.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência ao SDK do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Adicione uma referência ao SDK do Microsoft Advertising em seu projeto:

    1. Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**
    2.  No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).
    3.  No **Gerenciador de Referências**, clique em OK.

4. No arquivo de código apropriado no aplicativo (por exemplo, em MainPage.xaml.cs ou um arquivo de código para alguma outra página), adicione as seguintes referências de namespace.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

5.  Em um local indicado no aplicativo (por exemplo, em ```MainPage``` ou em alguma outra página), declare um objeto **NativeAdsManagerV2** e diversos campos da cadeia de caracteres que representam a ID do aplicativo e a ID da unidade de anúncio para o anúncio nativo. O exemplo de código a seguir atribui os campos `myAppId` e `myAdUnitId` aos [valores de teste](set-up-ad-units-in-your-app.md#test-ad-units) para anúncios nativos.
    > [!NOTE]
    > Cada **NativeAdsManagerV2** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle de anúncio nativo, e cada unidade de anúncio consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu app na Store, [substitua os valores de teste por valores reais](#release) do Centro de Desenvolvimento do Windows.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

6.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **NativeAdsManagerV2** e conecte os manipuladores de eventos **AdReady** e **ErrorOccurred** do objeto.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

7.  Quando você estiver pronto para mostrar um anúncio nativo, chame o método **RequestAd** para buscar um anúncio.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

8.  Quando um anúncio nativo estiver pronto para seu app, seu manipulador de eventos **AdReady** será chamado e um objeto **NativeAdV2** que representa o anúncio nativo é passado para o parâmetro *e*. Use as propriedades de **NativeAdV2** para cada elemento do anúncio nativo e exiba esses elementos em sua página. Chame também o método **RegisterAdContainer** para registrar o elemento de interface do usuário que age como um contêiner para o anúncio nativo; isso é necessário para rastrear corretamente impressões e cliques de anúncio.
    > [!NOTE]
    > Alguns elementos do anúncio nativo são necessários e sempre devem ser mostrados em seu app. Para obter mais informações, consulte [diretrizes de anúncios nativos](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

    Por exemplo, suponha que seu app contenha uma ```MainPage``` (ou alguma outra página) com o seguinte **StackPanel**. Esse **StackPanel** contém uma série de controles que exibem elementos diferentes de um anúncio nativo, incluindo o título, a descrição, as imagens, o texto *patrocinado por* e um botão que mostrará o texto do *chamada para ação*.

    ``` xml
    <StackPanel x:Name="NativeAdContainer" Background="#555555" Width="Auto" Height="Auto"
                Orientation="Vertical">
        <Image x:Name="AdIconImage" HorizontalAlignment="Left" VerticalAlignment="Center"
               Margin="20,20,20,20"/>
        <TextBlock x:Name="TitleTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
               Text="The ad title will go here" FontSize="24" Foreground="White" Margin="20,0,0,10"/>
        <TextBlock x:Name="DescriptionTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
                   Foreground="White" TextWrapping="Wrap" Text="The ad description will go here"
                   Margin="20,0,0,0" Visibility="Collapsed"/>
        <Image x:Name="MainImageImage" HorizontalAlignment="Left"
               VerticalAlignment="Center" Margin="20,20,20,20" Visibility="Collapsed"/>
        <Button x:Name="CallToActionButton" Background="Gray" Foreground="White"
                HorizontalAlignment="Left" VerticalAlignment="Center" Width="Auto" Height="Auto"
                Content="The call to action text will go here" Margin="20,20,20,20"
                Visibility="Collapsed"/>
        <StackPanel x:Name="SponsoredByStackPanel" Orientation="Horizontal" Margin="20,20,20,20">
            <TextBlock x:Name="SponsoredByTextBlock" Text="The ad sponsored by text will go here"
                       FontSize="24" Foreground="White" Margin="20,0,0,0" HorizontalAlignment="Left"
                       VerticalAlignment="Center" Visibility="Collapsed"/>
            <Image x:Name="IconImageImage" Margin="40,20,20,20" HorizontalAlignment="Left"
                   VerticalAlignment="Center" Visibility="Collapsed"/>
        </StackPanel>
    </StackPanel>
    ```

    O exemplo de código a seguir demonstra um manipulador de eventos **AdReady** que exibe cada elemento do anúncio nativo nos controles no **StackPanel** e então chama o método **RegisterAdContainer** para registrar o **StackPanel**. Esse código pressupõe que ele é executado do arquivo code-behind para a página que contém o **StackPanel**.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#AdReady)]

9.  Defina um manipulador de eventos para o evento **ErrorOccurred** para manipular erros relacionados ao anúncio nativo. O exemplo a seguir grava as informações de erro para a janela **Saída** do Visual Studio durante o teste.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

10.  Compile e execute o app para vê-lo com um anúncio de teste.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Liberar seu app com anúncios ativos

Depois de confirmar que a implementação do seu anúncio nativo mostra com êxito um anúncio de teste, siga estas instruções para configurar seu aplicativo para mostrar anúncios reais e enviar seu aplicativo atualizado para a Store.

1.  Certifique-se de que sua implementação de anúncios nativo segue as [diretrizes para anúncios nativos](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

2.  No painel do Centro de Desenvolvimento, vá para a página [Anúncios no app](../publish/in-app-ads.md) e [crie uma unidade publicitária](set-up-ad-units-in-your-app.md#live-ad-units). No tipo de unidade publicitária, especifique **Nativa**. Anote a ID da unidade publicitária e a ID do aplicativo.
    > [!NOTE]
    > Os valores da ID de aplicativo para unidades publicitárias de teste e unidades publicitárias dinâmicas UWP têm formatos diferentes. Valores de ID de aplicativo de teste são GUIDs. Ao criar uma unidade publicitária dinâmica UWP no painel, o valor da ID de aplicativo da unidade publicitária sempre corresponde à ID da Store do seu aplicativo (um valor de valor da ID da Microsoft Store é semelhante a 9NBLGGH4R315).

3. Como alternativa, você pode habilitar o controle de anúncios para o anúncio nativo ao definir as configurações na seção [Configurações de controle](../publish/in-app-ads.md#mediation) na página [Anúncios no app](../publish/in-app-ads.md). O controle de anúncio permite que você maximize a receita do anúncio e os recursos de promoção do app ao exibir anúncios de várias redes de anúncios conhecidas.

4.  Em seu código, substitua os valores da unidade de teste de anúncio (ou seja, os parâmetros  *applicationId* e *adUnitId* do construtor **NativeAdsManagerV2**) pelos valores ativos gerados no Centro de Desenvolvimento.

5.  [Envie seu aplicativo](../publish/app-submissions.md) para a Store usando o painel do Centro de Desenvolvimento.

6.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>Gerenciar unidades publicitárias para vários anúncios nativos no aplicativo

Você pode usar vários posicionamentos de anúncio nativo em um único aplicativo. Nesse cenário, recomendamos que você atribua uma unidade publicitária diferente para posicionamento de anúncio nativo. O uso de unidades de anúncio diferentes para anúncios nativos a fim de permitir que você [defina as configurações de mediação](../publish/in-app-ads.md#mediation) separadamente e obtenha [dados de relatório](../publish/advertising-performance-report.md) discretos para cada controle. Isso também permite que nossos serviços melhorem a otimização dos anúncios veiculados em seu app.

> [!IMPORTANT]
> Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes para anúncios nativos](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [Anúncios no aplicativo](../publish/in-app-ads.md)
* [Configurar unidades publicitárias para seu app](set-up-ad-units-in-your-app.md)
---
author: mcleanbyron
description: "Saiba como adicionar anúncios nativos ao seu aplicativo UWP."
title: "Anúncios nativos"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, publicidade, controle de anúncio, anúncio nativo"
ms.openlocfilehash: 47a69e48f04c670a462c34083af1117d2c7908d8
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2017
---
# <a name="native-ads"></a>Anúncios nativos

Um anúncio nativo é um formato de anúncio com base em componente em que cada parte do criativo do anúncio (como título, imagem, descrição e texto do chamada para ação) é entregue ao aplicativo como um elemento individual. Você pode integrar esses elementos ao seu app usando suas próprias fontes, cores, animações e outros componentes de IU para compor uma experiência de usuário discreto que se ajuste à aparência do seu app enquanto também obtém alto rendimento dos anúncios.

Para os anunciantes, os anúncios nativos fornecem posicionamentos de alto desempenho, porque a experiência de anúncios está totalmente integrada ao aplicativo e os usuários, portanto, tendem a interagir mais com esses tipos de anúncios.

> [!NOTE]
> Para veicular anúncios nativos para a versão pública do seu app na Loja, você deve criar uma unidade publicitária **Nativa** desde a página **Monetizar com anúncios** no painel do Centro de Desenvolvimento. A capacidade de criar unidades publicitárias **Nativas** só está disponível para selecionar os desenvolvedores que estejam participando de um programa piloto, mas nós pretendemos disponibilizar esse recurso para todos os desenvolvedores em breve. Se você estiver interessado em participar do nosso programa piloto, entre em contato conosco pelo email aiacare@microsoft.com.

> [!NOTE]
> No momento, os anúncios nativos têm suporte apenas para aplicativos UWP baseados em XAML para Windows 10. O suporte para aplicativos UWP escrito usando HTML e JavaScript está planejado para uma versão futura do SDK do Microsoft Advertising.

## <a name="prerequisites"></a>Pré-requisitos

* Instale o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp) (versão 10.0.4 ou posterior) com o Visual Studio 2015 ou uma versão posterior do Visual Studio. Para obter instruções de instalação, consulte [este artigo](install-the-microsoft-advertising-libraries.md). Você pode instalar o SDK no computador de desenvolvimento via o instalador MSI ou também pode instalar o SDK para uso em um projeto específico por meio do pacote NuGet.

## <a name="integrate-a-native-ad-into-your-app"></a>Integrar um anúncio nativo ao seu app

Siga estas instruções para integrar um anúncio nativo ao seu app e confirme que sua implementação de anúncios nativos mostra um anúncio de teste.

1. No Visual Studio, abra o projeto ou crie um novo projeto.

2. Se o seu projeto tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**). Se o seu projeto tem direcionamento **Any CPU**, você não conseguirá adicionar uma referência ao SDK do Microsoft Advertising nas etapas a seguir. Para obter mais informações, consulte [Erros de referência causados pelo direcionamento Any CPU em seu projeto](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Na janela **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**

4.  No **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** (versão 10.0).

5.  No **Gerenciador de Referências**, clique em OK.

6. No arquivo de código apropriado no aplicativo (por exemplo, em MainPage.xaml.cs ou um arquivo de código para alguma outra página), adicione as seguintes referências de namespace.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

7.  Em um local indicado no aplicativo (por exemplo, em ```MainPage``` ou em alguma outra página), declare um objeto [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.aspx) e diversos campos da cadeia de caracteres que representam a ID do aplicativo e a ID da unidade de anúncio para o anúncio nativo. O exemplo de código a seguir atribui os campos `myAppId` e `myAdUnitId` aos [valores de teste](test-mode-values.md) para anúncios nativos.

    > [!NOTE]
    > Cada **NativeAdsManager** tem uma *unidade publicitária* correspondente que é usado por nossos serviços para veicular anúncios para o controle de anúncio nativo, e cada unidade de anúncio consiste em uma *ID da unidade publicitária* e *ID do aplicativo*. Nestas etapas, você atribui os valores da ID da unidade publicitária de teste e da ID do aplicativo para seu controle. Esses valores de teste só podem ser usados em uma versão de teste do seu app. Antes de publicar seu app na Loja, [substitua os valores de teste por valores reais](#release) do Centro de Desenvolvimento do Windows.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

8.  No código executado na inicialização (por exemplo, no construtor da página), instancie o objeto **NativeAdsManager** e conecte os manipuladores de eventos [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.adready.aspx) e [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.erroroccurred.aspx) do objeto.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

9.  Quando você estiver pronto para mostrar um anúncio nativo, chame o método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.requestad.aspx) para buscar um anúncio.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

10.  Quando um anúncio nativo estiver pronto para seu app, seu manipulador de eventos **AdReady** será chamado e um objeto [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) que representa o anúncio nativo é passado para o parâmetro *e*. Use as propriedades de **NativeAd** para cada elemento do anúncio nativo e exiba esses elementos em sua página. Chame também o método [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) para registrar o elemento de interface do usuário que age como um contêiner para o anúncio nativo; isso é necessário para rastrear corretamente impressões e cliques de anúncio.
  > [!NOTE]
  > Alguns elementos do anúncio nativo são necessários e sempre devem ser mostrados em seu app. Para obter mais informações, veja os [requisitos e as diretrizes](#requirements-and-guidelines) .

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

11.  Defina um manipulador de eventos para o evento **ErrorOccurred** para manipular erros relacionados ao anúncio nativo. O exemplo a seguir grava as informações de erro para a janela **Saída** do Visual Studio durante o teste.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

12.  Compile e execute o app para vê-lo com um anúncio de teste.

<span id="release" />
## <a name="release-your-app-with-live-ads"></a>Liberar seu app com anúncios ativos

Depois de confirmar que a implementação do seu anúncio nativo mostra com êxito um anúncio de teste, siga estas instruções para configurar seu aplicativo para mostrar anúncios reais e enviar seu aplicativo atualizado para a Loja.

1.  Certifique-se de que sua implementação de anúncios nativo segue os [requisitos e as diretrizes](#requirements-and-guidelines) para anúncios nativos.

2.  No painel do Centro de Desenvolvimento, vá até a página [Monetizar com anúncios](../publish/monetize-with-ads.md) do app e [crie uma unidade publicitária](../monetize/set-up-ad-units-in-your-app.md). No tipo de unidade publicitária, especifique **Nativa**. Anote a ID da unidade publicitária e a ID do aplicativo.
    > [!IMPORTANT]
    > Você pode usar cada unidade publicitária em apenas um app. Se você usar uma unidade publicitária em mais de um app, os anúncios não serão veiculados para essa unidade publicitária.

3. Opcionalmente, você pode habilitar o controle de anúncios para o anúncio nativo ao definir as configurações na seção [Correção de anúncio](../publish/monetize-with-ads.md#mediation) na página [Monetizar com anúncios](../publish/monetize-with-ads.md). O controle de anúncio permite que você maximize a receita do anúncio e os recursos de promoção do app ao exibir anúncios de várias redes de anúncios conhecidas.

4.  Em seu código, substitua os valores da unidade de teste de anúncio (ou seja, os parâmetros  *applicationId* e *adUnitId* do construtor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx)) pelos valores ativos gerados no Centro de Desenvolvimento.

5.  [Envie seu aplicativo](../publish/app-submissions.md) para a Loja usando o painel do Centro de Desenvolvimento.

6.  Analise seus [relatórios de desempenho de publicidade](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento.

<span id="requirements-and-guidelines" />
## <a name="requirements-and-guidelines"></a>Requisitos e diretrizes

Os anúncios nativos oferecem muito controle sobre como você apresenta o conteúdo publicitário para seus usuários. Siga estes requisitos e diretrizes para ajudar a garantir que a mensagem do anunciante chegue aos usuários enquanto também ajuda a evitar o fornecimento de uma experiência de anúncios nativos confusa para os usuários.

### <a name="register-the-container-for-your-native-ad"></a>Registrar o contêiner para seu anúncio nativo

Em seu código, você deve chamar o método [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) do objeto **NativeAd** para registrar o elemento de interface do usuário que age como um contêiner para o anúncio nativo e, opcionalmente, quaisquer controles específicos que você deseja registrar como destinos clicáveis para o anúncio. Isso é necessário para controlar corretamente as impressões e os cliques de anúncios.

Há duas sobrecargas para o método **RegisterAdContainer** que você pode usar:

* Se você quiser que o contêiner inteiro para todos os elementos de anúncio nativo individuais seja clicável, chame o método [RegisterAdContainer(FrameworkElement)](https://msdn.microsoft.com/library/windows/apps/mt809188.aspx) e passe o controle de contêiner para o método. Por exemplo, se você exibir todos os elementos de anúncio nativo em controles separados hospedados em um **StackPanel** e se quiser que todo o **StackPanel** seja clicável, passe o **StackPanel** para esse método.

* Se você quiser que somente determinados elementos de anúncio nativo sejam clicáveis, chave o método [RegisterAdContainer (FrameworkElement, IVector(FrameworkElement))](https://msdn.microsoft.com/library/windows/apps/mt809189.aspx). Somente os controles que você passa para o segundo parâmetro serão clicáveis.

### <a name="required-native-ad-elements"></a>Elementos de anúncio nativo necessários

No mínimo, você sempre deverá mostrar os seguintes elementos de anúncio nativo para o usuário em seu design de anúncio nativo. Se você falhar ao incluir esses elementos, talvez veja um desempenho ruim e resultados baixos para sua unidade publicitária.

1. Sempre exiba o título do anúncio nativo (disponível na propriedade [Title](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.title.aspx) do objeto **NativeAd**). Forneça espaço suficiente para exibir pelo menos 25 caracteres. Se o título for mais longo, substitua o texto adicional por reticências.
2. Sempre exiba pelo menos um dos elementos a seguir para ajudar a diferenciar a experiência do anúncio nativo do resto do seu app e mostre claramente que o conteúdo é fornecido por um anunciante:
  * O ícone *ad* distinguível (disponível na propriedade [AdIcon](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.adicon.aspx) do objeto **NativeAd**). Esse ícone é fornecido pela Microsoft.
  * O texto *patrocinado por* (disponível na propriedade [SponsoredBy](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.sponsoredby.aspx) do objeto **NativeAd**). Esse texto é fornecido pelo anunciante.
  * Como uma alternativa para o texto *patrocinado por*, você pode optar por exibir algum outro texto que ajude a diferenciar a experiência do anúncio nativo do resto do seu app, como "Conteúdo patrocinado", "Conteúdo promocional", "Conteúdo recomendado" etc.

### <a name="user-experience"></a>Experiência do usuário

Seu anúncio nativo deve ser claramente delineado do resto do seu aplicativo e ter espaço ao redor dele para impedir cliques acidentais. Use bordas, telas de fundo diferentes ou alguma outra interface do usuário para separar o conteúdo do anúncio do resto do seu aplicativo. Tenha em mente que os cliques acidentais em anúncios não são benéficos para sua receita baseada em anúncio ou para a experiência do seu usuário final a longo prazo.

### <a name="description"></a>Descrição

Se você optar por mostrar a descrição para o anúncio (disponível na opção [Description](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.description.aspx) do objeto **NativeAd**), forneça espaço suficiente para exibir pelo menos 75 caracteres. Recomendamos que você use uma animação para mostrar todo o conteúdo da descrição do anúncio.

### <a name="call-to-action"></a>Plano de ação

O texto *chamada para ação* (disponível na propriedade [CallToAction](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.calltoaction.aspx) do objeto **NativeAd**) é um componente essencial do anúncio. Se você optar por mostrar esse texto, siga estas diretrizes:

* Sempre exiba o texto *chamada para ação* para o usuário em um controle clicável, como um botão ou um hiperlink.
* Sempre exiba o texto *chamada para ação* em sua totalidade.
* Verifique se o texto *chamada para ação* está separado do resto do texto promocional do anunciante.

### <a name="learn-and-optimize"></a>Aprender e otimizar

Recomendamos que você crie e use unidades publicitárias diferentes para cada posicionamento de anúncios nativo diferentes em seu aplicativo. Isso permite que você obtenha dados de relatórios separados para cada posicionamento de anúncio nativo, e você pode usar esses dados para fazer alterações que otimizam o desempenho de cada posicionamento de anúncios nativo.

## <a name="related-topics"></a>Tópicos relacionados

* [Monetizar com anúncios](../publish/monetize-with-ads.md)
* [Configurar unidades publicitárias para seu app](../monetize/set-up-ad-units-in-your-app.md)

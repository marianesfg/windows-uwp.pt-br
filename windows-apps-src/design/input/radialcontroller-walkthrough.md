---
author: Karl-Bridge-Microsoft
ms.assetid: ''
title: Suporte ao Surface Dial (e a outros dispositivos de roda de rolagem) no aplicativo UWP
description: Um tutorial passo a passo para adicionar suporte ao Surface Dial (e a outros dispositivos de roda de rolagem) no aplicativo UWP.
keywords: dial, radial, tutorial
ms.author: kbridge
ms.date: 01/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3df0f37fda62a7b673e28a6198758365886b6783
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6035628"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-uwp-app"></a>Tutorial: Suporte ao Surface Dial (e a outros dispositivos de roda de rolagem) no aplicativo UWP

![Imagem do Surface Dial com Surface Studio](images/radialcontroller/dial-pen-studio-600px.png)  
*Surface Dial com Surface Studio e Caneta Surface* (disponível para compra na [Microsoft Store](https://aka.ms/purchasesurfacedial)).

Este tutorial descreve como personalizar as experiências de interação do usuário compatíveis com dispositivos de roda de rolagem, como o Surface Dial. Usamos trechos de um aplicativo de exemplo, que você pode baixar no GitHub (consulte [Código de exemplo](#sample-code)), para demonstrar os diversos recursos e APIs [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) associadas abordados em cada etapa.

Centraremos a atenção no seguinte:
* Especificando quais ferramentas internas são exibidas no menu [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)
* Adicionando uma ferramenta personalizada ao menu
* Controlando os comentários hápticos
* Personalizando as interações de clique
* Personalizando as interações de rotação

Para obter mais informações sobre como implementar esses e outros recursos, consulte [Interações do Surface Dial nos aplicativos UWP](windows-wheel-interactions.md).

## <a name="introduction"></a>Introdução

O Surface Dial é um dispositivo de entrada secundário que ajuda os usuários a ser mais produtivo quando usado junto com um dispositivo de entrada principal, como caneta, toque ou mouse. Como dispositivo de entrada secundário, o Dial é normalmente usado com a mão não dominante para fornecer acesso aos comandos do sistema e a outras ferramentas e funcionalidade mais contextuais. 

O Dial oferece suporte a três gestos básicos: 
- Manter pressionado para exibir o menu de comandos interno.
- Girar para realçar um item de menu (se o menu estiver ativo) ou para modificar a ação atual no aplicativo (se o menu não estiver ativo).
- Clicar para selecionar o item de menu realçado (se o menu estiver ativo) ou para invocar um comando no aplicativo (se o menu não estiver ativo).

## <a name="prerequisites"></a>Pré-requisitos

* Um computador (ou uma máquina virtual) executando a Atualização do Windows 10 para Criadores ou mais recente
* [Visual Studio 2017 (10.0.15063.0)](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Um dispositivo de roda de rolagem (somente o [Surface Dial](https://aka.ms/purchasesurfacedial) no momento)
* Se você for novato no desenvolvimento de aplicativos UWP (Plataforma Universal do Windows) com o Visual Studio, dê uma olhada nestes tópicos antes de iniciar este tutorial:  
    * [Prepare-se para começar](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [Criar um aplicativo "Olá, Mundo" (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)

## <a name="set-up-your-devices"></a>Configure seus dispositivos

1. Assegure que o dispositivo Windows está ligado.
2. Vá para **Iniciar**, selecione **Configurações** > **Dispositivos** > **Bluetooth e outros dispositivos** e ative **Bluetooth**.
3. Remova a parte inferior do Surface Dial para abrir o compartimento de pilha e verifique se há duas pilhas AAA.
4. Se a guia de pilha estiver presente na parte inferior do Dial, remova-a.
5. Mantenha pressionado o pequeno botão embutido ao lado das pilhas até que a luz do Bluetooth pisque.
6. Retorne ao dispositivo Windows e selecione **Adicionar Bluetooth ou outro dispositivo**.
7. Na caixa de diálogo **Adicionar um Dispositivo**, selecione **Bluetooth** > **Surface Dial**. O Surface Dial se conectará e será adicionado à lista de dispositivos em **Mouse, teclado e caneta** na página de configurações **Bluetooth e outros dispositivos**.
8. Teste o Dial mantendo-o pressionado por alguns segundos para exibir o menu interno.
9. Se o menu não for exibido na tela (o Dial também vibrará), vá para configurações de Bluetooth, remova o dispositivo e tente conectar o dispositivo novamente.

> [!NOTE]
> Os dispositivos de roda de rolagem podem ser configurados por meio das configurações de **Roda**:
> 1. No menu **Iniciar**, selecione **Configurações**.
> 2. Selecione **Dispositivos** > **Roda**.    
> ![Tela de configurações de roda](images/radialcontroller/wheel-settings.png)

Agora você está pronto para iniciar este tutorial. 

## <a name="sample-code"></a>Código de exemplo
Neste tutorial, usamos um aplicativo de exemplo para demonstrar os conceitos e a funcionalidade abordados.

Baixe este código-fonte e de exemplo do Visual Studio no [GitHub](https://github.com/) em [windows-appsample-get-started-radialcontroller sample](https://aka.ms/appsample-radialcontroller):

1. Selecione o botão verde **Clone or download**.  
![Clonando o repositório](images/radialcontroller/wheel-clone.png)
2. Se você tiver uma conta do GitHub, poderá clonar o repositório no computador local escolhendo **Open in Visual Studio**. 
3. Se você não tiver uma conta do GitHub ou apenas quiser uma cópia local do projeto, escolha **Download ZIP** (você precisará fazer verificações regulares para baixar as atualizações mais recentes).

> [!IMPORTANT]
> Grande parte do código no exemplo é comentado. À medida que percorrermos cada etapa deste tópico, você será solicitado a remover os comentários das várias seções do código. No Visual Studio, basta realçar as linhas de código, pressionar CTRL-K e, em seguida, CTRL-U.

## <a name="components-that-support-wheel-functionality"></a>Componentes que oferecem suporte à funcionalidade de roda de rolagem

Esses objetos proporcionam a maior parte da experiência com dispositivos de roda de rolagem nos aplicativos UWP.

| Componente | Descrição |
| --- | --- |
| [**Classe RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) e componentes relacionados | Representa um acessório ou dispositivo de entrada de roda de rolagem, como o Surface Dial. |
| [**IRadialControllerConfigurationInterop**](https://msdn.microsoft.com/library/windows/desktop/mt790709) / [**IRadialControllerInterop**](https://msdn.microsoft.com/library/windows/desktop/mt790711)<br/>Não abordaremos essa funcionalidade aqui; para obter mais informações, consulte o [Exemplo de área de trabalho clássica do Windows](https://aka.ms/radialcontrollerclassicsample). | Permite a interoperabilidade com um aplicativo UWP. |

## <a name="step-1-run-the-sample"></a>Etapa 1: Executar o exemplo

Após baixar o aplicativo de exemplo RadialController, verifique se ele é executado:
1. Abra o projeto de exemplo no Visual Studio.
2. Defina a lista suspensa **Solution Platforms** para uma seleção não ARM.
3. Pressione F5 para compilar, implantar e executar. 

> [!NOTE]
> Se desejar, você pode selecionar o item de menu **Debug** > **Start debugging** ou selecione o botão **Local Machine** Run mostrado aqui: ![botão de projeto Visual Studio Build](images/radialcontroller/wheel-vsrun.png)

A janela do aplicativo será aberta e, depois que uma tela inicial aparecer por alguns segundos, você verá esta tela inicial.

![Aplicativo vazio](images/radialcontroller/wheel-app-step1-empty.png)

Agora temos o aplicativo UWP básico que usaremos durante todo o restante deste tutorial. Nas etapas a seguir, adicionamos nossa funcionalidade **RadialController**.

## <a name="step-2-basic-radialcontroller-functionality"></a>Etapa 2: Funcionalidade básica RadialController

Com o aplicativo em execução e em primeiro plano, mantenha pressionado o Surface Dial para exibir o menu **RadialController**.

Ainda não fizemos nenhuma personalização no aplicativo, portanto, o menu contém um conjunto padrão de ferramentas contextuais. 

Essas imagens mostram duas variações do menu padrão. (Há muitas outras, incluindo apenas as ferramentas básicas do sistema quando a área de trabalho do Windows está ativa e não há aplicativos em primeiro plano, ferramentas adicionais de escrita à tinta quando um InkToolbar está presente e ferramentas de mapeamento quando você está usando o aplicativo Mapas.)

| Menu RadialController (padrão)  | Menu RadialController (padrão com reprodução de mídia)  |
|---|---|
| ![Menu RadialController padrão](images/radialcontroller/wheel-app-step2-basic-default.png) | ![Menu RadialController padrão com música](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

Agora, começaremos com uma personalização básica.

## <a name="step-3-add-controls-for-wheel-input"></a>Etapa 3: Adicionar controles de entrada de roda de rolagem

Primeiro, vamos adicionar a interface do usuário do aplicativo:

1. Abra o arquivo MainPage_Basic.xaml.
2. Localize o código marcado com o título desta etapa ("\<!-- Etapa 3: Adicionar controles de entrada de roda de rolagem -->").
3. Remova o comentário das linhas a seguir.

    ```xaml
    <Button x:Name="InitializeSampleButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Initialize sample" />
    <ToggleButton x:Name="AddRemoveToggleButton"
                    HorizontalAlignment="Center" 
                    Margin="10" 
                    Content="Remove Item"
                    IsChecked="True" 
                    IsEnabled="False"/>
    <Button x:Name="ResetControllerButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Reset RadialController menu" 
            IsEnabled="False"/>
    <Slider x:Name="RotationSlider" Minimum="0" Maximum="10"
            Width="300"
            HorizontalAlignment="Center"/>
    <TextBlock Text="{Binding ElementName=RotationSlider, Mode=OneWay, Path=Value}"
                Margin="0,0,0,20"
                HorizontalAlignment="Center"/>
    <ToggleSwitch x:Name="ClickToggle"
                    MinWidth="0" 
                    Margin="0,0,0,20"
                    HorizontalAlignment="center"/>
    ```
Neste ponto, somente o botão **Initialize sample**, o controle deslizante e o botão de alternância estão habilitados. Os outros botões são usados em etapas posteriores para adicionar e remover itens do menu **RadialController** que fornecem acesso ao controle deslizante e ao botão de alternância.

![Interface do usuário básica do aplicativo de exemplo](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>Etapa 4: Personalizar o menu básico RadialController

Agora vamos adicionar o código necessário para habilitar o acesso do **RadialController** aos controles.

1. Abra o arquivo MainPage_Basic.xaml.cs.
2. Localize o código marcado com o título desta etapa ("// Etapa 4: Personalização do menu básico RadialController").
3. Remova o comentário das linhas a seguir:
    - As referências de tipo [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input) and [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) são usadas na funcionalidade em etapas subsequentes:  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - Esses objetos globais ([RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller), [RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration), [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)) são usados em todo o aplicativo.
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - Aqui, especificamos o manipulador **Click** do botão que habilita os controles e inicializa o item de menu personalizado **RadialController**.

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - Em seguida, inicializamos o objeto [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) e configuramos os manipuladores dos eventos [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) e [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked).

        ```csharp
        // Set up the app UI and RadialController.
        private void InitializeSample(object sender, RoutedEventArgs e)
        {
            ResetControllerButton.IsEnabled = true;
            AddRemoveToggleButton.IsEnabled = true;

            ResetControllerButton.Click += (resetsender, args) =>
            { ResetController(resetsender, args); };
            AddRemoveToggleButton.Click += (togglesender, args) =>
            { AddRemoveItem(togglesender, args); };

            InitializeController(sender, e);
        }
        ```

    - Aqui, inicializamos o item de menu personalizado RadialController. Usamos [CreateForCurrentView](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView) para obter uma referência ao objeto [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller), definimos a sensibilidade de rotação como "1" usando a propriedade [RotationResolutionInDegrees](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees), criamos o [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem) usando [CreateFromFontGlyph](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph), adicionamos o item de menu à coleção de itens de menu **RadialController** e, por fim, usamos [SetDefaultMenuItems](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems) para limpar os itens de menu padrão e deixar apenas nossa ferramenta personalizada. 

        ```csharp
        // Configure RadialController menu and custom tool.
        private void InitializeController(object sender, RoutedEventArgs args)
        {
            // Create a reference to the RadialController.
            radialController = RadialController.CreateForCurrentView();
            // Set rotation resolution to 1 degree of sensitivity.
            radialController.RotationResolutionInDegrees = 1;

            // Create the custom menu items.
            // Here, we use a font glyph for our custom tool.
            radialControllerMenuItem =
                RadialControllerMenuItem.CreateFromFontGlyph("SampleTool", "\xE1E3", "Segoe MDL2 Assets");

            // Add the item to the RadialController menu.
            radialController.Menu.Items.Add(radialControllerMenuItem);

            // Remove built-in tools to declutter the menu.
            // NOTE: The Surface Dial menu must have at least one menu item. 
            // If all built-in tools are removed before you add a custom 
            // tool, the default tools are restored and your tool is appended 
            // to the default collection.
            radialControllerConfig =
                RadialControllerConfiguration.GetForCurrentView();
            radialControllerConfig.SetDefaultMenuItems(
                new RadialControllerSystemMenuItemKind[] { });

            // Declare input handlers for the RadialController.
            // NOTE: These events are only fired when a custom tool is active.
            radialController.ButtonClicked += (clicksender, clickargs) =>
            { RadialController_ButtonClicked(clicksender, clickargs); };
            radialController.RotationChanged += (rotationsender, rotationargs) =>
            { RadialController_RotationChanged(rotationsender, rotationargs); };
        }

        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees >= RotationSlider.Maximum)
            {
                RotationSlider.Value = RotationSlider.Maximum;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < RotationSlider.Minimum)
            {
                RotationSlider.Value = RotationSlider.Minimum;
            }
            else
            {
                RotationSlider.Value += args.RotationDeltaInDegrees;
            }
        }

        // Connect wheel device click to toggle switch control.
        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ClickToggle.IsOn = !ClickToggle.IsOn;
        }
        ```
4. Agora execute o aplicativo novamente.
5. Selecione o botão **Initialize radial controller**.  
6. Com o aplicativo em primeiro plano, mantenha pressionado o Surface Dial para exibir o menu. Observe que todas as ferramentas padrão foram removidas (usando o método **RadialControllerConfiguration.SetDefaultMenuItems**), deixando apenas a ferramenta personalizada. Este é o menu com a ferramenta personalizada. 

| Menu RadialController (personalizado)  | 
|---|
| ![Menu personalizado RadialController](images/radialcontroller/wheel-app-step3-custom.png) |

7. Selecione a ferramenta personalizada e experimente as interações compatíveis com o Surface Dial:
    * Uma ação de rotação move o controle deslizante. 
    * Um clique define a alternância entre ativar e desativar.

Vamos conectar esses botões.

## <a name="step-5-configure-menu-at-runtime"></a>Etapa 5: Configurar o menu em tempo de execução

Nesta etapa, conectamos os botões do **item Adicionar/Remover** e do **menu Reset RadialController** para mostrar como personalizar o menu dinamicamente.

1. Abra o arquivo MainPage_Basic.xaml.cs.
2. Localize o código marcado com o título desta etapa ("// Etapa 5: Configurar o menu em tempo de execução").
3. Remova o comentário do código nos métodos a seguir e execute o aplicativo novamente, mas não selecione nenhum botão (salve isso para a próxima etapa).  

    ``` csharp
    // Add or remove the custom tool.
    private void AddRemoveItem(object sender, RoutedEventArgs args)
    {
        if (AddRemoveToggleButton?.IsChecked == true)
        {
            AddRemoveToggleButton.Content = "Remove item";
            if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Add(radialControllerMenuItem);
            }
        }
        else if (AddRemoveToggleButton?.IsChecked == false)
        {
            AddRemoveToggleButton.Content = "Add item";
            if (radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Remove(radialControllerMenuItem);
                // Attempts to select and activate the previously selected tool.
                // NOTE: Does not differentiate between built-in and custom tools.
                radialController.Menu.TrySelectPreviouslySelectedMenuItem();
            }
        }
    }

    // Reset the RadialController to initial state.
    private void ResetController(object sender, RoutedEventArgs arg)
    {
        if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
        {
            radialController.Menu.Items.Add(radialControllerMenuItem);
        }
        AddRemoveToggleButton.Content = "Remove item";
        AddRemoveToggleButton.IsChecked = true;
        radialControllerConfig.SetDefaultMenuItems(
            new RadialControllerSystemMenuItemKind[] { });
    }
    ```
4. Selecione o botão **Remover item** e mantenha pressionado o Dial para exibir o menu novamente.

    Observe que, agora, o menu contém o conjunto padrão de ferramentas. Lembre-se de que, na Etapa 3, ao configurar o menu personalizado, removemos todas as ferramentas de padrão e adicionamos apenas a ferramenta personalizada. Observamos também que, quando o menu é definido como uma coleção vazia, os itens padrão do contexto atual são restabelecidos. (Adicionamos a ferramenta personalizada antes de remover as ferramentas padrão.)

5. Selecione o botão **Adicionar item** e mantenha pressionado o Dial.

    Observe que, agora, o menu contém a coleção padrão de ferramentas e a ferramenta personalizada.

6. Selecione o botão do **menu Reset RadialController** e mantenha pressionado o Dial.

    Observe que o menu retorna ao estado original.

## <a name="step-6-customize-the-device-haptics"></a>Etapa 6: Personalizar a háptica do dispositivo
O Surface Dial e outros dispositivos de roda de rolagem podem fornecer aos usuários comentários hápticos correspondentes à interação atual (com base no clique ou na rotação).

Nesta etapa, vamos mostrar como personalizar comentários hápticos associando os controles deslizantes e de botão de alternância e usando-os para especificar dinamicamente o comportamento desses comentários. Neste exemplo, o botão de alternância deve ser ativado para que os comentários sejam habilitados enquanto o valor do controle deslizante especifica a frequência em que os comentários de clique são repetidos. 

> [!NOTE]
> Os comentários hápticos podem ser desabilitados pelo usuário na página **Configurações** >  **Dispositivos** > **Roda**.

1. Abra o arquivo App.xaml.cs.
2. Localize o código marcado com o título desta etapa ("Etapa 6: Personalizar a háptica do dispositivo").
3. Comente a primeira e terceira linhas ("MainPage_Basic" e "MainPage") e remova o comentário do segundo ("MainPage_Haptics").  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. Abra o arquivo MainPage_Haptics.xaml.
5. Localize o código marcado com o título desta etapa ("\<!-- Etapa 6: Personalizar a háptica do dispositivo -->").
6. Remova o comentário das linhas a seguir. (Este código de interface do usuário simplesmente indica quais recursos hápticos são compatíveis com o dispositivo atual.)    

    ```xaml
    <StackPanel x:Name="HapticsStack" 
                Orientation="Vertical" 
                HorizontalAlignment="Center" 
                BorderBrush="Gray" 
                BorderThickness="1">
        <TextBlock Padding="10" 
                    Text="Supported haptics properties:" />
        <CheckBox x:Name="CBDefault" 
                    Content="Default" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsChecked="True" />
        <CheckBox x:Name="CBIntensity" 
                    Content="Intensity" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayCount" 
                    Content="Play count" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayDuration" 
                    Content="Play duration" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBReplayPauseInterval" 
                    Content="Replay/pause interval" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBBuzzContinuous" 
                    Content="Buzz continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBClick" 
                    Content="Click" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPress" 
                    Content="Press" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRelease" 
                    Content="Release" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRumbleContinuous" 
                    Content="Rumble continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
    </StackPanel>
    ```
7. Abra o arquivo MainPage_Haptics.xaml.cs.
8. Localize o código marcado com o título desta etapa ("Etapa 6: Personalização da háptica")
9. Remova o comentário das linhas a seguir:  

    - A referência de tipo [Windows.Devices.Haptics](https://docs.microsoft.com/uwp/api/windows.devices.haptics) é usada para a funcionalidade nas etapas subsequentes.  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - Aqui especificamos o manipulador do evento [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired), que é disparado quando o item de menu personalizado **RadialController** é selecionado.

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - Em seguida, definimos o manipulador [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired), no qual podemos desabilitar comentários hápticos padrão e inicializar a interface do usuário háptica.

        ```csharp
        private void RadialController_ControlAcquired(
            RadialController rc_sender,
            RadialControllerControlAcquiredEventArgs args)
        {
            // Turn off default haptic feedback.
            radialController.UseAutomaticHapticFeedback = false;

            SimpleHapticsController hapticsController =
                args.SimpleHapticsController;

            // Enumerate haptic support.
            IReadOnlyCollection<SimpleHapticsControllerFeedback> supportedFeedback =
                hapticsController.SupportedFeedback;

            foreach (SimpleHapticsControllerFeedback feedback in supportedFeedback)
            {
                if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.BuzzContinuous)
                {
                    CBBuzzContinuous.IsEnabled = true;
                    CBBuzzContinuous.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Click)
                {
                    CBClick.IsEnabled = true;
                    CBClick.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Press)
                {
                    CBPress.IsEnabled = true;
                    CBPress.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Release)
                {
                    CBRelease.IsEnabled = true;
                    CBRelease.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.RumbleContinuous)
                {
                    CBRumbleContinuous.IsEnabled = true;
                    CBRumbleContinuous.IsChecked = true;
                }
            }

            if (hapticsController?.IsIntensitySupported == true)
            {
                CBIntensity.IsEnabled = true;
                CBIntensity.IsChecked = true;
            }
            if (hapticsController?.IsPlayCountSupported == true)
            {
                CBPlayCount.IsEnabled = true;
                CBPlayCount.IsChecked = true;
            }
            if (hapticsController?.IsPlayDurationSupported == true)
            {
                CBPlayDuration.IsEnabled = true;
                CBPlayDuration.IsChecked = true;
            }
            if (hapticsController?.IsReplayPauseIntervalSupported == true)
            {
                CBReplayPauseInterval.IsEnabled = true;
                CBReplayPauseInterval.IsChecked = true;
            }
        }
        ```

    - Nos manipuladores de evento [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) e [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked), conectamos os controles deslizantes e de botão de alternância correspondentes à háptica personalizada. 

        ```csharp
        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            ...
            if (ClickToggle.IsOn && 
                (RotationSlider.Value > RotationSlider.Minimum) && 
                (RotationSlider.Value < RotationSlider.Maximum))
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.BuzzContinuous);
                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedback(waveform);
                }
            }
        }

        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ...

            if (RotationSlider?.Value > 0)
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.Click);

                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedbackForPlayCount(
                        waveform, 1.0, 
                        (int)RotationSlider.Value, 
                        TimeSpan.Parse("1"));
                }
            }
        }
        ```
    - Por fim, obtemos a **[Waveform](https://docs.microsoft.com/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)** solicitada (se compatível) para os comentários hápticos. 

        ```csharp
        // Get the requested waveform.
        private SimpleHapticsControllerFeedback FindWaveform(
            SimpleHapticsController hapticsController,
            ushort waveform)
        {
            foreach (var hapticInfo in hapticsController.SupportedFeedback)
            {
                if (hapticInfo.Waveform == waveform)
                {
                    return hapticInfo;
                }
            }
            return null;
        }
        ```

Agora execute o aplicativo novamente para testar a háptica personalizada, alterando o valor do controle deslizante e o estado do botão de alternância.

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>Etapa 7: Definir interações na tela para dispositivos Surface Studio e similares
Emparelhado com o Surface Studio, o Surface Dial pode fornecer uma experiência de usuário ainda mais distinta. 

Além da experiência do menu padrão de pressionar e segurar descrita, o Surface Dial também pode ser colocado diretamente na tela do Surface Studio. Isso ativa um menu "na tela" especial. 

Detectando o local do contato e os limites do Surface Dial, o sistema trata a oclusão pelo dispositivo e exibe uma versão posterior do menu que é disposta na parte externa do Dial. Essas mesmas informações também podem ser usadas pelo seu aplicativo para adaptar a interface do usuário para a presença do dispositivo e seu uso previsto, como o posicionamento da mão e do braço do usuário. 

O exemplo que acompanha este tutorial inclui um exemplo um pouco mais complexo que demonstra algumas dessas funcionalidades.

Para ver isso em ação (você precisará do Surface Studio):

1. Baixe o exemplo em um dispositivo Surface Studio (com o Visual Studio instalado)
2. Abra o exemplo no Visual Studio
3. Abra o arquivo App.xaml.cs
4. Localize o código marcado com o título desta etapa ("Etapa 7: Definir interações na tela para dispositivos Surface Studio e similares")
5. Comente a primeira e segunda linhas ("MainPage_Basic" e "MainPage_Haptics") e remova o comentário do terceiro ("MainPage").  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. Execute o aplicativo e coloque o Surface Dial em cada uma das duas regiões de controle, alternando entre elas.    
![RadialController na tela](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    Este é um vídeo deste exemplo em ação:  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>Resumo

Parabéns, você concluiu o *Tutorial de Introdução: Suporte ao Surface Dial (e outros dispositivos de roda de rolagem) no aplicativo UWP*! Mostramos a você o código básico necessário para oferecer suporte a um dispositivo de roda de rolagem nos aplicativos UWP e proporcionar algumas experiências de usuário mais sofisticadas compatíveis com as APIs **RadialController**.

---
Description: The media player has customizable XAML transport controls to manage control of audio and video content.
title: Como criar controles personalizados de transporte de mídia
ms.assetid: 6643A108-A6EB-42BC-B800-22EABD7B731B
label: Create custom media transport controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4878ce99d449674243c8a3f7360a9e9b0dd6db19
ms.sourcegitcommit: 1cf04b0b1bd7623cd7f6067b8392dce4372f2c69
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2018
ms.locfileid: "8970989"
---
# <a name="create-custom-transport-controls"></a>Criar controles personalizados de transporte



MediaPlayerElement tem controles de transporte XAML personalizáveis para gerenciar o controle de conteúdo de áudio e vídeo em um aplicativo da UWP (Plataforma Universal do Windows). Aqui, demonstramos como personalizar o modelo MediaTransportControls. Mostraremos como trabalhar com o menu de estouro, adicionar um botão personalizado e modificar o controle deslizante.

> **APIs importantes**: [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx), [MediaPlayerElement.AreTransportControlsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aretransportcontrolsenabled.aspx), [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677)

Antes de começar, você deve estar familiarizado com as classes MediaPlayerElement e MediaTransportControls. Para obter mais informações, consulte o Guia do controle MediaPlayerElement.

> [!TIP]
> Os exemplos neste tópico se baseiam na [Amostra de controles de transporte de mídia](http://go.microsoft.com/fwlink/p/?LinkId=620023). Você pode baixar a amostra para exibir e executar o código completo.

> [!NOTE]
> O **MediaPlayerElement** só está disponível no Windows 10, versão 1607, e posterior. Se estiver desenvolvendo um aplicativo para uma versão anterior do Windows 10, você precisará usar [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926). Todos os exemplos nesta página também funcionam com o **MediaElement**.

## <a name="when-should-you-customize-the-template"></a>Quando você deve personalizar o modelo?

**MediaPlayerElement** tem controles de transporte internos que foram projetados para funcionar bem sem modificação na maioria dos aplicativos de reprodução de áudio e vídeo. Eles são fornecidos pela classe [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) e incluem botões para executar, parar e navegar na mídia, além de ajustar volume, alternar tela inteira, converter em um segundo dispositivo, habilitar legendas, alternar faixas de áudio e ajustar a taxa de reprodução. MediaTransportControls tem propriedades que permitem controlar se cada botão é mostrado e habilitado. Você também pode definir a propriedade [**IsCompact**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.iscompact.aspx) para especificar se os controles são mostrados em uma linha ou duas.

No entanto, talvez haja cenários em que você precise personalizar ainda mais a aparência do controle ou alterar seu comportamento. Aqui estão alguns exemplos:
- Altere os ícones, o comportamento do controle deslizante e as cores.
- Mover botões de comando menos usados para um menu de estouro.
- Altere a ordem na qual os comandos saem quando o controle é redimensionado.
- Forneça um botão de comando que não esteja no padrão definido.

> [!NOTE]
> Os botões visíveis na tela sairão dos controles de transporte integrados em uma ordem predefinida se não houver espaço na tela. Para alterar essa ordem ou colocar os comandos que não cabem em um menu de estouro, você precisa personalizar os controles.

Você pode personalizar a aparência do controle modificando o modelo padrão. Para modificar o comportamento do controle ou adicionar novos comandos, você pode criar um controle personalizado derivado de MediaTransportControls.

> [!TIP]
> Os modelos de controle personalizáveis são um recurso avançado da plataforma XAML, mas também há consequências que você deve levar em consideração. Quando você personaliza um modelo, ele se torna uma parte estática de seu aplicativo e, portanto, não receberá as atualizações da plataforma feitas no modelo pela Microsoft. Se atualizações de modelo forem feitas pela Microsoft, você deverá pegar o novo modelo e modificá-lo novamente para obter os benefícios do modelo atualizado.

## <a name="template-structure"></a>Estrutura do modelo

O [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx) faz parte do estilo padrão. Você pode copiar esse estilo padrão para o seu projeto a fim de modificá-lo. O ControlTemplate é dividido em seções semelhantes a de outros modelos de controle XAML.
- A primeira seção do modelo contém as definições de estilo para os diversos componentes dos [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) dos MediaTransportControls.
- A segunda seção define os diversos estados visuais que são usados pelos MediaTransportControls.
- A terceira seção contém o [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) que contém vários elementos MediaTransportControls juntos e define o layout dos componentes.

> [!NOTE]
> Para obter mais informações sobre como modificar modelos, consulte [Modelos de controle](). Você pode usar um editor de texto ou editores semelhantes no IDE para abrir os arquivos XAML em \(*Program Files*)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\\(*SDK version*)\Generic. O modelo e o estilo padrão para cada controle são definidos no arquivo **generic.xaml**. Você pode encontrar o modelo MediaTransportControls em generic.xaml procurando por "MediaTransportControls".

Nas seguintes seções, você aprende a personalizar diversos dos principais elementos dos controles de transporte:
- [**Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx): permite que um usuário navegue em sua mídia e também exibe o progresso
- [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx): contém todos os botões.
Para obter mais informações, consulte a seção Anatomia do tópico de referência MediaTransportControls.

## <a name="customize-the-transport-controls"></a>Personalizar os controles de transporte

Se você quiser modificar apenas a aparência dos MediaTransportControls, crie uma cópia do estilo de controle e modelo padrão e modifique-a. No entanto, se também quiser adicionar ou modificar a funcionalidade do controle, você deverá criar uma nova classe derivada de MediaTransportControls.

### <a name="re-template-the-control"></a>Novo modelo do controle

**Para personalizar o modelo e o estilo padrão MediaTransportControls**
1. Copie o estilo padrão de Estilos e modelos MediaTransportControls para um ResourceDictionary em seu projeto.
2. Atribua a Style um valor x:Key para identificá-lo, assim.

```xaml
<Style TargetType="MediaTransportControls" x:Key="myTransportControlsStyle">
    <!-- Style content ... -->
</Style>
```

3. Adicione um MediaPlayerElement com MediaTransportControls à sua interface do usuário.
4. Defina a propriedade Style do elemento MediaTransportControls como seu recurso personalizado Style, conforme mostrado aqui.

```xaml
<MediaPlayerElement AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls Style="{StaticResource myTransportControlsStyle}"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

Para obter mais informações sobre como modificar estilos e modelos, consulte [Aplicando estilos a controles]() e [Modelos de controle]().

### <a name="create-a-derived-control"></a>Criar um controle derivado

Para adicionar ou modificar a funcionalidade dos controles de transporte, você deve criar uma nova classe que seja derivada de MediaTransportControls. Uma classe derivada chamada `CustomMediaTransportControls` é mostrada na [Amostra de controles de transporte de mídia](http://go.microsoft.com/fwlink/p/?LinkId=620023) e os demais exemplos nesta página.

**Para criar uma nova classe derivada de MediaTransportControls**
1. Adicione um novo arquivo de classe ao projeto.
    - No Visual Studio, selecione Projeto > Adicionar Classe. A caixa de diálogo Adicionar Novo Item é aberta.
    - Na caixa de diálogo Adicionar Novo Item, insira um nome para o arquivo de classe e clique em Adicionar. (Na amostra de Controles de Transporte de Mídia, a classe é chamada `CustomMediaTransportControls`.)
2. Modifique o código de classe a ser derivado da classe MediaTransportControls.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
}
```

3. Copie o estilo padrão de [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) em um [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx) em seu projeto. Este é o estilo e o modelo que você modifica.
(Na Amostra de controles de transporte de mídia, uma nova pasta chamada "Themes" é criada, e um arquivo ResourceDictionary chamado generic.xaml é adicionado a ela.)
4. Altere o [**TargetType**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.targettype.aspx) do estilo para o novo tipo de controle personalizado. (Na amostra, o TargetType é alterado para `local:CustomMediaTransportControls`.)

```xaml
xmlns:local="using:CustomMediaTransportControls">
...
<Style TargetType="local:CustomMediaTransportControls">
```

5. Defina o [**DefaultStyleKey**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.defaultstylekey.aspx) de sua classe personalizada. Isso informa sua classe personalizada para usar um Style com um TargetType de `local:CustomMediaTransportControls`.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }
}
```

6. Adicione um [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) à sua marcação XAML e os controles personalizados de transporte a ela. Uma coisa a observar é que as APIs para ocultar, mostrar, desabilitar e habilitar os botões padrão ainda funcionam com um modelo personalizado.

```xaml
<MediaPlayerElement Name="MediaPlayerElement1" AreTransportControlsEnabled="True" Source="video.mp4">
    <MediaPlayerElement.TransportControls>
        <local:CustomMediaTransportControls x:Name="customMTC"
                                            IsFastForwardButtonVisible="True"
                                            IsFastForwardEnabled="True"
                                            IsFastRewindButtonVisible="True"
                                            IsFastRewindEnabled="True"
                                            IsPlaybackRateButtonVisible="True"
                                            IsPlaybackRateEnabled="True"
                                            IsCompact="False">
        </local:CustomMediaTransportControls>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

Agora você pode modificar o estilo de controle e o modelo para atualizar a aparência de seu controle personalizado e o código de controle para atualizar seu comportamento.

### <a name="working-with-the-overflow-menu"></a>Trabalhando com o menu de estouro

Você pode mover botões de comando MediaTransportControls para um menu de estouro, de maneira que os comandos menos usados permaneçam ocultos até que o usuário precise deles.

No modelo MediaTransportControls, os botões de comando estão contidos em um elemento [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx). A barra de comandos tem o conceito de comandos principais e secundários. Os comandos principais são os botões exibidos no controle por padrão e que estão sempre visíveis (a menos que você desabilite o botão, oculte-o ou que não haja espaço suficiente). Os comandos secundários são mostrados em um menu de estouro exibido quando um usuário clica no botão de reticências (...) Para obter mais informações, consulte o artigo [Barras de comandos e barras de aplicativos](app-bars.md).

Para mover um elemento dos comandos principais da barra de comandos para o menu de estouro, você precisa editar o modelo de controle XAML.

**Para mover um comando para o menu de estouro:**
1. No modelo de controle, encontre o elemento CommandBar chamado `MediaControlsCommandBar`.
2. Adicione uma seção [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) ao XAML da CommandBar. Coloque-a após a marca de fechamento do [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx).

```xaml
<CommandBar x:Name="MediaControlsCommandBar" ... >  
  <CommandBar.PrimaryCommands>
...
    <AppBarButton x:Name='PlaybackRateButton'
                    Style='{StaticResource AppBarButtonStyle}'
                    MediaTransportControlsHelper.DropoutOrder='4'
                    Visibility='Collapsed'>
      <AppBarButton.Icon>
        <FontIcon Glyph="&#xEC57;"/>
      </AppBarButton.Icon>
    </AppBarButton>
...
  </CommandBar.PrimaryCommands>
<!-- Add secondary commands (overflow menu) here -->
  <CommandBar.SecondaryCommands>
    ...
  </CommandBar.SecondaryCommands>
</CommandBar>
```

3. Para popular o menu com comandos, recorte e cole o XAML dos objetos [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) desejados no PrimaryCommands no SecondaryCommands. Neste exemplo, movemos o `PlaybackRateButton` para o menu de estouro.

4. Adicione um rótulo ao botão e remova as informações de estilo, conforme mostrado aqui.
Como o menu de estouro é composto de botões de texto, você deve adicionar um rótulo de texto ao botão e também remover o estilo que define a altura e a largura do botão. Do contrário, ele não será exibido corretamente no menu de estouro.

```xaml
<CommandBar.SecondaryCommands>
    <AppBarButton x:Name='PlaybackRateButton'
                  Label='Playback Rate'>
    </AppBarButton>
</CommandBar.SecondaryCommands>
```

> [!IMPORTANT]
> Você ainda precisa deixar o botão visível e habilitá-lo para usá-lo no menu de estouro. Neste exemplo, o elemento PlaybackRateButton não permanece visível no menu de estouro, a menos que a propriedade IsPlaybackRateButtonVisible seja true. Ele não é habilitado, a menos que a propriedade IsPlaybackRateEnabled seja true. A definição dessas propriedades é mostrada na seção anterior.

### <a name="adding-a-custom-button"></a>Adicionando um botão personalizado

Uma razão pela qual você talvez queira personalizar MediaTransportControls é adicionar um comando personalizado ao controle. Se você adicioná-lo como um comando principal ou um comando secundário, o procedimento para criar o botão de comando e modificar seu comportamento será o mesmo. Na [Amostra de controles de transporte de mídia](http://go.microsoft.com/fwlink/p/?LinkId=620023), um botão "rating" é adicionado aos comandos principais.

**Para adicionar um botão de comando personalizado**
1. Crie um objeto AppBarButton e o adicione ao CommandBar no modelo de controle.

```xaml
<AppBarButton x:Name="LikeButton"
              Icon="Like"
              Style="{StaticResource AppBarButtonStyle}"
              MediaTransportControlsHelper.DropoutOrder="3"
              VerticalAlignment="Center" />
```

    You must add it to the CommandBar in the appropriate location. (For more info, see the Working with the overflow menu section.) How it's positioned in the UI is determined by where the button is in the markup. For example, if you want this button to appear as the last element in the primary commands, add it at the very end of the primary commands list.

    You can also customize the icon for the button. For more info, see the [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) reference.

2. Na substituição [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.onapplytemplate.aspx), obtenha o botão do modelo e registre um manipulador para seu evento [**Click**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Esse código entra na classe `CustomMediaTransportControls`.

```csharp
public sealed class CustomMediaTransportControls :  MediaTransportControls
{
    // ...

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    //...
}
```

3. Adicione código ao manipulador de eventos Click para executar a ação que ocorre quando o botão é clicado.
Este é o código completo da classe.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public event EventHandler< EventArgs> Liked;

    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    private void LikeButton_Click(object sender, RoutedEventArgs e)
    {
        // Raise an event on the custom control when 'like' is clicked.
        var handler = Liked;
        if (handler != null)
        {
            handler(this, EventArgs.Empty);
        }
    }
}
```

**Controles personalizados de transporte de mídia com o botão "Curtir" adicionado**
![Controle personalizado de transporte de mídia com botão Curtir adicional](images/controls/mtc_double_custom_inprod.png)

### <a name="modifying-the-slider"></a>Modificando o controle deslizante

O controle "seek" do MediaTransportControls é fornecido por um elemento [**Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx). Uma maneira como você pode personalizá-lo é alterar a granularidade do comportamento de busca.

Como o controle deslizante de busca padrão é dividido em 100 partes, o comportamento de busca é limitado a essa quantidade de seções. Você pode alterar a granularidade do controle deslizante de busca obtendo o Slider da árvore visual XAML no manipulador de eventos [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx) no [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx). Este exemplo mostra como usar [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.visualtreehelper.aspx) para obter uma referência para o Slider e, em seguida, alterar a frequência de etapa padrão do controle deslizante de 1% para 0,1% (1.000 etapas) caso a mídia tenha mais de 120 minutos. O MediaPlayerElement é chamado `MediaPlayerElement1`.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
  MediaPlayerElement1.MediaPlayer.MediaOpened += MediaPlayerElement_MediaPlayer_MediaOpened;
  base.OnNavigatedTo(e);
}

private void MediaPlayerElement_MediaPlayer_MediaOpened(object sender, RoutedEventArgs e)
{
  FrameworkElement transportControlsTemplateRoot = (FrameworkElement)VisualTreeHelper.GetChild(MediaPlayerElement1.TransportControls, 0);
  Slider sliderControl = (Slider)transportControlsTemplateRoot.FindName("ProgressSlider");
  if (sliderControl != null && MediaPlayerElement1.NaturalDuration.TimeSpan.TotalMinutes > 120)
  {
    // Default is 1%. Change to 0.1% for more granular seeking.
    sliderControl.StepFrequency = 0.1;
  }
}
```
## <a name="related-articles"></a>Artigos relacionados

- [Reprodução de mídia](media-playback.md)

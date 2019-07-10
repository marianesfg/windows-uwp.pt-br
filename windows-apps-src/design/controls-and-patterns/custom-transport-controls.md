---
Description: O media player tem controles de transporte XAML personalizáveis para gerenciar o controle de conteúdo de áudio e vídeo.
title: Como criar controles personalizados de transporte de mídia
ms.assetid: 6643A108-A6EB-42BC-B800-22EABD7B731B
label: Create custom media transport controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 41c42a058398539701cc1df003717eec99d1b2cd
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66362862"
---
# <a name="create-custom-transport-controls"></a>Criar controles personalizados de transporte



MediaPlayerElement tem controles de transporte XAML personalizáveis para gerenciar o controle de conteúdo de áudio e vídeo em um aplicativo da UWP (Plataforma Universal do Windows). Aqui, demonstramos como personalizar o modelo MediaTransportControls. Mostraremos como trabalhar com o menu de estouro, adicionar um botão personalizado e modificar o controle deslizante.

> **APIs importantes**: [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement), [MediaPlayerElement.AreTransportControlsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.aretransportcontrolsenabled), [MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls)

Antes de começar, você deve estar familiarizado com as classes MediaPlayerElement e MediaTransportControls. Para obter mais informações, consulte o Guia do controle MediaPlayerElement.

> [!TIP]
> Os exemplos neste tópico se baseiam na [Amostra de controles de transporte de mídia](https://go.microsoft.com/fwlink/p/?LinkId=620023). Você pode baixar a amostra para exibir e executar o código completo.

> [!NOTE]
> O **MediaPlayerElement** só está disponível no Windows 10, versão 1607, e posterior. Se estiver desenvolvendo um aplicativo para uma versão anterior do Windows 10, você precisará usar [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement). Todos os exemplos nesta página também funcionam com o **MediaElement**.

## <a name="when-should-you-customize-the-template"></a>Quando você deve personalizar o modelo?

**MediaPlayerElement** tem controles de transporte internos que foram projetados para funcionar bem sem modificação na maioria dos aplicativos de reprodução de áudio e vídeo. Eles são fornecidos pela classe [**MediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) e incluem botões para executar, parar e navegar na mídia, além de ajustar volume, alternar tela inteira, converter em um segundo dispositivo, habilitar legendas, alternar faixas de áudio e ajustar a taxa de reprodução. MediaTransportControls tem propriedades que permitem controlar se cada botão é mostrado e habilitado. Você também pode definir a propriedade [**IsCompact**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediatransportcontrols.iscompact) para especificar se os controles são mostrados em uma linha ou duas.

No entanto, talvez haja cenários em que você precise personalizar ainda mais a aparência do controle ou alterar seu comportamento. Veja alguns exemplos:
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

O [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) faz parte do estilo padrão. Você pode copiar esse estilo padrão para o seu projeto a fim de modificá-lo. O ControlTemplate é dividido em seções semelhantes a de outros modelos de controle XAML.
- A primeira seção do modelo contém as definições de estilo para os diversos componentes dos [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) dos MediaTransportControls.
- A segunda seção define os diversos estados visuais que são usados pelos MediaTransportControls.
- A terceira seção contém o [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) que contém vários elementos MediaTransportControls juntos e define o layout dos componentes.

> [!NOTE]
> Para obter mais informações sobre como modificar modelos, consulte [Modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates). É possível usar um editor de texto ou editores semelhantes no IDE para abrir os arquivos XAML em \(*Arquivos de Programas*)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\\(*versão do SDK*)\Generic. O modelo e o estilo padrão para cada controle são definidos no arquivo **generic.xaml**. Você pode encontrar o modelo MediaTransportControls em generic.xaml procurando por "MediaTransportControls".

Nas seguintes seções, você aprende a personalizar diversos dos principais elementos dos controles de transporte:
- [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider): permite que um usuário navegue em sua mídia e também exibe o progresso
- [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar): contém todos os botões.
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

Para obter mais informações sobre como modificar estilos e modelos, consulte [Aplicando estilos a controles](/windows/uwp/design/controls-and-patterns/xaml-styles) e [Modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates).

### <a name="create-a-derived-control"></a>Criar um controle derivado

Para adicionar ou modificar a funcionalidade dos controles de transporte, você deve criar uma nova classe que seja derivada de MediaTransportControls. Uma classe derivada chamada `CustomMediaTransportControls` é mostrada na [Amostra de controles de transporte de mídia](https://go.microsoft.com/fwlink/p/?LinkId=620023) e os demais exemplos nesta página.

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

3. Copie o estilo padrão de [**MediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) em um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) em seu projeto. Este é o estilo e o modelo que você modifica.
(Na Amostra de controles de transporte de mídia, uma nova pasta chamada "Themes" é criada, e um arquivo ResourceDictionary chamado generic.xaml é adicionado a ela.)
4. Altere o [**TargetType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.targettype) do estilo para o novo tipo de controle personalizado. (Na amostra, o TargetType é alterado para `local:CustomMediaTransportControls`.)

```xaml
xmlns:local="using:CustomMediaTransportControls">
...
<Style TargetType="local:CustomMediaTransportControls">
```

5. Defina o [**DefaultStyleKey**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.defaultstylekey) de sua classe personalizada. Isso informa sua classe personalizada para usar um Style com um TargetType de `local:CustomMediaTransportControls`.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }
}
```

6. Adicione um [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) à sua marcação XAML e os controles personalizados de transporte a ela. Uma coisa a observar é que as APIs para ocultar, mostrar, desabilitar e habilitar os botões padrão ainda funcionam com um modelo personalizado.

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

No modelo MediaTransportControls, os botões de comando estão contidos em um elemento [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar). A barra de comandos tem o conceito de comandos principais e secundários. Os comandos principais são os botões exibidos no controle por padrão e que estão sempre visíveis (a menos que você desabilite o botão, oculte-o ou que não haja espaço suficiente). Os comandos secundários são mostrados em um menu de estouro exibido quando um usuário clica no botão de reticências (...) Para obter mais informações, consulte o artigo [Barras de comandos e barras de aplicativos](app-bars.md).

Para mover um elemento dos comandos principais da barra de comandos para o menu de estouro, você precisa editar o modelo de controle XAML.

**Para mover um comando para o menu de estouro:**
1. No modelo de controle, encontre o elemento CommandBar chamado `MediaControlsCommandBar`.
2. Adicione uma seção [**SecondaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) ao XAML da CommandBar. Coloque-a após a marca de fechamento do [**PrimaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands).

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

3. Para popular o menu com comandos, recorte e cole o XAML dos objetos [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) desejados no PrimaryCommands no SecondaryCommands. Neste exemplo, movemos o `PlaybackRateButton` para o menu de estouro.

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

Uma razão pela qual você talvez queira personalizar MediaTransportControls é adicionar um comando personalizado ao controle. Se você adicioná-lo como um comando principal ou um comando secundário, o procedimento para criar o botão de comando e modificar seu comportamento será o mesmo. Na [Amostra de controles de transporte de mídia](https://go.microsoft.com/fwlink/p/?LinkId=620023), um botão "rating" é adicionado aos comandos principais.

**Para adicionar um botão de comando personalizado**
1. Crie um objeto AppBarButton e o adicione ao CommandBar no modelo de controle.

```xaml
<AppBarButton x:Name="LikeButton"
              Icon="Like"
              Style="{StaticResource AppBarButtonStyle}"
              MediaTransportControlsHelper.DropoutOrder="3"
              VerticalAlignment="Center" />
```

É necessário adicioná-lo à CommandBar na localização correta. Para saber mais, confira a seção "Trabalhar com o menu de estouro". A maneira como ele está posicionado na interface do usuário é determinada pela posição do botão na marcação. Por exemplo, se você quiser que o botão apareça como o último elemento nos comandos principais, adicione-o no fim da lista de comandos principais.

Também é possível personalizar o ícone do botão. Para saber mais, confira a referência <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton"><b>AppBarButton</b></a>.
    

2. Na substituição [**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate), obtenha o botão do modelo e registre um manipulador para seu evento [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Esse código entra na classe `CustomMediaTransportControls`.

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

O controle "seek" do MediaTransportControls é fornecido por um elemento [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider). Uma maneira como você pode personalizá-lo é alterar a granularidade do comportamento de busca.

Como o controle deslizante de busca padrão é dividido em 100 partes, o comportamento de busca é limitado a essa quantidade de seções. Você pode alterar a granularidade do controle deslizante de busca obtendo o Slider da árvore visual XAML no manipulador de eventos [**MediaOpened**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.mediaopened) no [**MediaPlayerElement.MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement). Este exemplo mostra como usar [**VisualTreeHelper**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.VisualTreeHelper) para obter uma referência para o Slider e, em seguida, alterar a frequência de etapa padrão do controle deslizante de 1% para 0,1% (1.000 etapas) caso a mídia tenha mais de 120 minutos. O MediaPlayerElement é chamado `MediaPlayerElement1`.

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

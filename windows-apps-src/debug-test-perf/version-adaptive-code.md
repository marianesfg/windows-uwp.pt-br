---
author: jwmsft
title: "Código adaptável de versão"
description: "Saiba como tirar proveito das novas APIs e manter a compatibilidade com versões anteriores"
translationtype: Human Translation
ms.sourcegitcommit: 3f81d80cef0fef6d24cad1b42ce9726b03857b5a
ms.openlocfilehash: db6b9c83d36ac876661197dce81e5724e44bb640

---

# Código adaptável de versão: use as novas APIs e mantenha a compatibilidade com versões anteriores

Cada versão do SDK do Windows 10 adiciona novas funcionalidades interessantes que você vai querer aproveitar. No entanto, nem todos os seus clientes atualizarão seus dispositivos para a versão mais recente do Windows 10 ao mesmo tempo, e você quer garantir que seu aplicativo funcione na maior variedade possível de dispositivos. Aqui, mostraremos como projetar seu aplicativo de modo que ele seja executado em versões anteriores do Windows 10, mas também tire proveito dos novos recursos, sempre que o aplicativo for executado em um dispositivo com a atualização mais recente instalada.

Há duas etapas que devem ser seguidas a fim de garantir que o aplicativo seja compatível com a mais ampla gama de dispositivos Windows 10. Primeiro, configure seu projeto do Visual Studio para acessar as APIs mais recentes. Isso afeta o que acontece quando você compila o aplicativo. Segundo, execute verificações de tempo de execução para garantir que você só chama APIs que estão presentes no dispositivo em que seu aplicativo esteja sendo executado.

> [!NOTE] 
> Este artigo usa exemplos do SDK do Windows Insider Preview para Windows 10, versão 1607 (atualização de aniversário). O SDK do Preview é uma versão de pré-lançamento e não pode ser usado em um ambiente de produção. Instale o SDK somente em sua máquina de teste. O SDK do Preview contém correções de erros e mudanças em desenvolvimento para a área de superfície da API. Se estiver trabalhando em um aplicativo que precisa enviar à loja, você não deve instalar o Preview.

## Configurar seu projeto no Visual Studio

A primeira etapa para dar suporte a várias versões do Windows 10 é especificar as versões de *Destino* e *Mínima* com suporte do SO/SDK em seu projeto do Visual Studio.
- *Destino*: a versão do SDK na qual o Visual Studio compila o código do aplicativo e executa todas as ferramentas. Todas as APIs e recursos nesta versão do SDK estão disponíveis no código do aplicativo no momento da compilação.
- *Mínima*: versão do SDK que dá suporte à versão mais antiga do sistema operacional na qual seu aplicativo possa ser executado (e que será implantado pela loja) e a versão que o Visual Studio compila o código de marcação do aplicativo. 

Durante o tempo de execução, o aplicativo será executado com a versão do sistema operacional que for implantado, portanto o aplicativo gerará exceções se você usar recursos ou chamar as APIs que não estejam disponíveis nessa versão. Mostramos a você como usar verificações de tempo de execução para chamar as APIs corretas mais adiante neste artigo.

As configurações de Destino e Mínima especificam os términos de um intervalo das versões do SO/SDK. No entanto, se você testar o aplicativo na versão mínima, pode ter certeza de que ele será executado em qualquer versão entre a Mínima e a de Destino.

> [!TIP]
> O Visual Studio não avisa você sobre compatibilidade de API. É sua responsabilidade testar e garantir que o aplicativo tenha o desempenho esperado entre todas as versões de sistema operacional, incluindo a Mínima e a de Destino.

Quando você cria um novo projeto no Visual Studio 2015, Atualização 2 ou posterior, você será solicitado a definir as versões de Destino e Mínima que são compatíveis com seu aplicativo. Por padrão, a versão de Destino é a maior versão instalada do SDK e a versão Mínima é a menor versão instalada do SDK. Você só poderá escolher as versões de Destino e Mínima de acordo com as versões do SDK que estão instaladas em seu computador. 

![Definir o SDK de destino no Visual Studio](images/vs-target-sdk-1.png)

Normalmente, recomendamos que você deixe os padrões. Entretanto, se você tiver uma versão Preview do SDK instalada e estiver escrevendo um código para produção, altere a versão de Destino do SDK do Preview para a última versão oficial do SDK. 

Para alterar a versão Mínima e de Destino de um projeto que já tenha sido criado no Visual Studio, vá para Projeto -> Propriedades -> guia Aplicativo -> Direcionamento.

![Alterar o SDK de destino no Visual Studio](images/vs-target-sdk-2.png) 

Para referência, estes são os números de compilação para cada SDK:
- Windows 10, versão 1506: SDK versão 10240
- Windows 10, versão 1511 (atualização de novembro): SDK versão 10586
- Windows 10, versão 1607 Insider Preview (atualização de aniversário): na ocasião em que este documento foi redigido, [a versão mais recente do SDK do Insider Preview é 14332](https://blogs.windows.com/buildingapps/2016/04/28/windows-10-anniversary-sdk-preview-build-14332-released/).

Você pode baixar qualquer versão do SDK do [arquivo de emulador e do SDK do Windows](https://developer.microsoft.com/downloads/sdk-archive). Você pode baixar o SDK mais recente do Windows Insider Preview na seção de desenvolvedor do site [Windows Insider](https://insider.windows.com/).

## Escrever código adaptável

Você pode pensar em escrever código adaptável da mesma forma que pensa em [criar uma interface do usuário adaptável](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml). Você pode projetar a interface do usuário básica para ser executada na tela menor e então mover ou adicionar elementos ao detectar que o aplicativo está sendo executado em uma tela maior. Com código adaptável, você escreve o código base para ser executado na versão mais baixa do sistema operacional e pode adicionar recursos selecionados a mão quando você detecta que o aplicativo está sendo executado em uma versão mais recente em que o novo recurso está disponível.

### Verificações de API de tempo de execução

Você usa a classe [Windows.Foundation.Metadata.ApiInformation](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx) em uma condição no código para testar a presença da API que você deseja chamar. Essa condição será avaliada onde quer que seu aplicativo seja executado, mas só será avaliada como **true** em dispositivos em que a API esteja presente e, portanto, disponível para ser chamada. Isso permite que você escreva código adaptável de versão para criar aplicativos que usam APIs que estão disponíveis somente em determinadas versões do sistema operacional.

Neste artigo, analisaremos exemplos específicos para o direcionamento de novos recursos do Windows Insider Preview. Para obter uma visão geral do uso de **ApiInformation**, consulte [Guia para aplicativos UWP](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) e a postagem do blog [Detectando dinamicamente recursos com contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/) (em inglês).

> [!TIP]
> Várias verificações de API do tempo de execução podem afetar o desempenho do seu aplicativo. Mostramos as verificações em linha nesses exemplos. No código de produção, você deve executar a verificação de uma vez e armazenar o resultado em cache. Depois, usar o resultado em cache em todo o aplicativo. 

### Cenários sem suporte

Na maioria dos casos, você pode manter a versão mínima do aplicativo configurada para a versão 10240 do SDK e usar verificações de tempo de execução para habilitar quaisquer novas APIs quando o aplicativo for executado em uma versão mais recente. No entanto, existem alguns casos em que você deve aumentar a versão Mínima do aplicativo para usar os novos recursos.

Você deve aumentar a versão Mínima do seu aplicativo se você usar:
- uma nova API que exija uma funcionalidade que não esteja disponível em uma versão anterior. Você deve aumentar a versão mínima com suporte para uma que inclua essa funcionalidade. Para saber mais, consulte [Declarações de funcionalidades do aplicativo](../packaging/app-capability-declarations.md).
- chaves de recurso nova adicionadas a generic.xaml e não disponíveis em uma versão anterior. A versão do generic.xaml usada no tempo de execução será determinada pela versão do sistema operacional que dispositivo estiver executando. Não é possível usar verificações de API do tempo de execução para determinar a presença de recursos XAML. Portanto, você deve usar somente as chaves de recurso que estão disponíveis na versão mínima à qual o aplicativo dá suporte ou [XAMLParseException](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.markup.xamlparseexception.aspx) causará uma falha do aplicativo no tempo de execução.

### Opções de código adaptável

Há duas maneiras para criar código adaptável. Na maioria dos casos, escreva a marcação do aplicativo para ser executada na versão mínima e use seu código do aplicativo para utilizar os recursos mais recentes do sistema operacional quando presentes. No entanto, se precisar atualizar uma propriedade em um estado visual e houver apenas uma alteração de propriedade ou de valor de enumeração entre versões do sistema operacional, você pode criar um gatilho de estado extensível que será ativado com base na presença de uma API.

Aqui, comparamos essas opções.

**Código do aplicativo**

Quando usar:
- Recomendado para todos os cenários de código adaptável com exceção de casos específicos de gatilhos extensíveis definidos abaixo.

Benefícios:
- Evita sobrecarga/complexidade do desenvolvedor para vincular as diferenças da API na marcação.

Desvantagens:
- Não há suporte do Designer.

**Gatilhos de estado**

Quando usar:
- Use quando houver somente uma alteração de propriedade ou de enumeração entre versões do sistema operacional que não necessite de alterações de lógica e esteja conectada a um estado visual.

Benefícios:
- Permite que você crie estados visuais específicos que são acionados com base na presença de uma API.
- Há algum suporte do designer disponível.

Desvantagens:
- O uso de gatilhos personalizados é restrito aos estados visuais, o que não combina com layouts adaptáveis complicados.
- Deve-se usar Setters para especificar mudanças de valor, portanto somente alterações simples são possíveis.
- Gatilhos de estado personalizado são bastante detalhados para configurar e usar.

## Exemplos de código adaptável

Nesta seção, mostraremos vários exemplos de código adaptável que usam APIs que são novas no Windows 10, versão 1607 (Windows Insider Preview).

### Exemplo 1: novo valor de enumeração

O Windows 10, versão 1607 adiciona um novo valor à enumeração [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.inputscopenamevalue.aspx): **ChatWithoutEmoji**. Esse novo escopo de entrada tem o mesmo comportamento de entrada que o escopo de entrada **Chat** (verificação ortográfica, preenchimento automático, uso de maiúsculas automático), mas ele é mapeado para um teclado virtual sem um botão emoji. Isso é útil se você criar seu próprio seletor de emoji e quiser desabilitar o botão emoji interno no teclado virtual. 

Este exemplo mostra como verificar se o valor de enumeração **ChatWithoutEmoji** está presente e define a propriedade [InputScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.inputscope.aspx) de um **TextBox** se estiver presente. Se não estiver presente no sistema em que o aplicativo for executado, a propriedade **InputScope** será definida como **Chat** em vez disso. O código mostrado poderia ser colocado em um construtor de página ou em um manipulador de eventos Page.Loaded.

> [!TIP]
> Quando você verificar uma API, use cadeias estáticas em vez de usar recursos de linguagem .NET. Caso contrário, o aplicativo pode tentar acessar um tipo que não esteja definido e falhar no tempo de execução.

**C#**
```csharp
// Create a TextBox control for sending messages 
// and initialize an InputScope object.
TextBox messageBox = new TextBox();
messageBox.AcceptsReturn = true;
messageBox.TextWrapping = TextWrapping.Wrap;
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();

// Check that the ChatWithEmoji value is present.
// (It's present starting with Windows 10, version 1607,
//  the Target version for the app. This check returns false on earlier versions.)         
if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
{
    // Set new ChatWithoutEmoji InputScope if present.
    scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
}
else
{
    // Fall back to Chat InputScope.
    scopeName.NameValue = InputScopeNameValue.Chat;
}

// Set InputScope on messaging TextBox.
scope.Names.Add(scopeName);
messageBox.InputScope = scope;

// For this example, set the TextBox text to show the selected InputScope.
messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();

// Add the TextBox to the XAML visual tree (rootGrid is defined in XAML).
rootGrid.Children.Add(messageBox);
```

No exemplo anterior, um controle TextBox é criada e todas as propriedades são definidas no código. No entanto, se você tiver XAML existente e só precisar alterar a propriedade InputScope nos sistemas em que o novo valor tenha suporte, isso poderá ser feito sem alterar seu XAML, conforme mostrado aqui. Você define o valor padrão para **Chat** em XAML, mas o substitui no código, se o valor **ChatWithoutEmoji** estiver presente.

**XAML**
```xaml
<TextBox x:Name="messageBox"
         AcceptsReturn="True" TextWrapping="Wrap"
         InputScope="Chat"
         Loaded="messageBox_Loaded"/>
```

**C#**
```csharp
private void messageBox_Loaded(object sender, RoutedEventArgs e)
{
    if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
    {
        // Check that the ChatWithEmoji value is present.
        // (It's present starting with Windows 10, version 1607,
        //  the Target version for the app. This code is skipped on earlier versions.)
        InputScope scope = new InputScope();
        InputScopeName scopeName = new InputScopeName();
        scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
        // Set InputScope on messaging TextBox.
        scope.Names.Add(scopeName);
        messageBox.InputScope = scope;
    }

    // For this example, set the TextBox text to show the selected InputScope.
    // This is outside of the API check, so it will happen on all OS versions.
    messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();
}
```

Agora que temos um exemplo concreto, vamos ver como as configurações de versão de Destino e Mínima se aplicam a ele.

Nesses exemplos, você pode usar o valor de enumeração Chat em XAML ou no código sem uma verificação, porque ele está presente na versão mínima do sistema operacional com suporte. 

Se você usar o valor de ChatWithoutEmoji em XAML ou no código sem uma verificação, ele será compilado sem erros porque está presente na versão de Destino do sistema operacional. Ele também será executado sem erros em um sistema com a versão de Destino do sistema operacional. No entanto, quando o aplicativo for executado em um sistema que use a versão Mínima, haverá falha no tempo de execução porque o valor de enumeração ChatWithoutEmoji não está presente. Portanto, você deve usar esse valor somente no código e encapsulá-lo em uma verificação de API do tempo de execução para que seja chamado somente se for compatível com o sistema atual.

### Exemplo 2: novo controle

Uma nova versão do Windows normalmente traz novos controles para a superfície de API da UWP que traz novas funcionalidades à plataforma. Para aproveitar a presença de um novo controle, use o método [ApiInformation.IsTypePresent](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx).

O Windows 10, versão 1607 introduz um novo controle de mídia chamado [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx). Esse controle complementa a classe [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx), para que ele ofereça recursos como a capacidade de enquadrar-se facilmente ao áudio em segundo plano, e faz uso de melhorias arquiteturais na pilha de mídia.

No entanto, se o aplicativo for executado em um dispositivo que estiver executando uma versão mais antiga do que a versão 1607 do Windows 10, você deve usar o controle [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx) em vez do novo controle **MediaPlayerElement**. Você pode usar o método [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx) para verificar a presença do controle MediaPlayerElement no tempo de execução e carregar qualquer controle que seja adequado ao sistema em que o aplicativo esteja sendo executado.

Este exemplo mostra como criar um aplicativo que usa o novo MediaPlayerElement ou o antigo MediaElement dependendo se o tipo de MediaPlayerElement estiver presente. Nesse código, você usa a classe [UserControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol.aspx) para separar em componentes os controles, a interface do usuário e o código relacionados a eles para que você possa alterná-los com base na versão do sistema operacional. Como alternativa, você pode usar um controle personalizado que fornece mais funcionalidades e comportamento personalizado do que é necessário para este exemplo simples.
 
**MediaPlayerUserControl** 

O `MediaPlayerUserControl` encapsula um **MediaPlayerElement** e vários botões que são usados para examinar a mídia quadro a quadro. O UserControl permite tratar esses controles como uma única entidade e facilita a alternância com um MediaElement em sistemas mais antigos. Esse controle de usuário deve ser usado somente em sistemas em que o MediaPlayerElement estiver presente, portanto você não usa verificações de ApiInformation no código dentro desse controle de usuário.

> [!NOTE]
> Para manter este exemplo simples e específico, os botões de avanço de quadros estão colocados fora do media player. Para uma melhor experiência de usuário, você deve personalizar o MediaTransportControls para incluir seus botões personalizados. Consulte [Criar controles personalizados de transporte](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls) para saber mais. 

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaPlayerUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid x:Name="MPE_grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Orientation="Horizontal" 
                    HorizontalAlignment="Center" Grid.Row="1">
            <RepeatButton Click="StepBack_Click" Content="Step Back"/>
            <RepeatButton Click="StepForward_Click" Content="Step Forward"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**C#**
```csharp
using System;
using Windows.Media.Core;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace MediaApp
{
    public sealed partial class MediaPlayerUserControl : UserControl
    {
        public MediaPlayerUserControl()
        {
            this.InitializeComponent();
            
            // The markup code compiler runs against the Minimum OS version so MediaPlayerElement must be created in app code
            MPE = new MediaPlayerElement();
            Uri videoSource = new Uri("ms-appx:///Assets/UWPDesign.mp4");
            MPE.Source = MediaSource.CreateFromUri(videoSource);
            MPE.AreTransportControlsEnabled = true;
            MPE.MediaPlayer.AutoPlay = true;

            // Add MediaPlayerElement to the Grid
            MPE_grid.Children.Add(MPE);

        }

        private void StepForward_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }

        private void StepBack_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }
    }
}
```

**MediaElementUserControl**
 
O `MediaElementUserControl` encapsula um controle **MediaElement**.

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaElementUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid>
        <MediaElement AreTransportControlsEnabled="True" 
                      Source="Assets/UWPDesign.mp4"/>
    </Grid>
</UserControl>
```

> [!NOTE]
> A página de código para `MediaElementUserControl` só contém código gerado, portanto ela não é mostrada.

**Inicializar um controle baseado em IsTypePresent**

No tempo de execução, você chama **ApiInformation.IsTypePresent** para verificar se há o MediaPlayerElement. Se ele estiver presente, você carrega `MediaPlayerUserControl`. Se não estiver, você carrega `MediaElementUserControl`.

**C#**
```csharp
public MainPage()
{
    this.InitializeComponent();

    UserControl mediaControl;

    // Check for presence of type MediaPlayerElement.
    if (ApiInformation.IsTypePresent("Windows.UI.Xaml.Controls.MediaPlayerElement"))
    {
        mediaControl = new MediaPlayerUserControl();
    }
    else
    {
        mediaControl = new MediaElementUserControl();
    }

    // Add mediaControl to XAML visual tree (rootGrid is defined in XAML).
    rootGrid.Children.Add(mediaControl);
}
```

> [!IMPORTANT]
> Lembre-se de que essa verificação somente define o objeto `mediaControl` para `MediaPlayerUserControl` ou `MediaElementUserControl`. Você precisa executar essas verificações condicionais em qualquer outro lugar no código em que é preciso determinar se o MediaPlayerElement ou as APIs MediaElement devem ser usadas. Você deve executar a verificação de uma vez e armazenar o resultado em cache. Depois, usar o resultado em cache em todo o aplicativo.

## Exemplos de gatilho de estado

Gatilhos de estado extensíveis permitem que você use o código e a marcação juntos para acionar as alterações de estado visual com base em uma condição que você verifica no código. Nesse caso, a presença de uma API específica. Não recomendamos os gatilhos de estado para cenários comuns de código adaptável devido à sobrecarga envolvida e a restrição somente aos estados visuais. 

Você deve usar gatilhos de estado para código adaptável somente quando tiver pequenas alterações de interface do usuário entre diferentes versões do sistema operacional que não afetem o restante da interface do usuário, como uma alteração de propriedade ou de valor enumeração em um controle.

### Exemplo 1: nova propriedade

A primeira etapa na configuração de um gatilho de estado extensível é subclassificar a classe [StateTriggerBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.statetriggerbase.aspx) para criar um gatilho personalizado que será acionado com base na presença de uma API. Este exemplo mostra um gatilho que é acionado se a presença da propriedade corresponder à variável `_isPresent` definida em XAML.

**C#**
```csharp
class IsPropertyPresentTrigger : StateTriggerBase
{
    public string TypeName { get; set; }
    public string PropertyName { get; set; }

    private Boolean _isPresent;
    private bool? _isPropertyPresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;
            if (_isPropertyPresent == null)
            {
                // Call into ApiInformation method to determine if property is present.
                _isPropertyPresent =
                ApiInformation.IsPropertyPresent(TypeName, PropertyName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isPropertyPresent);
        }
    }
}
```

A próxima etapa é configurar o gatilho de estado visual em XAML para que resultem dois diferentes estados visuais com base na presença da API. 

O Windows 10, versão 1607 apresenta uma nova propriedade na classe [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) chamada [AllowFocusOnInteraction](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.allowfocusoninteraction.aspx), que determina se um controle recebe o foco quando um usuário interage com ele. Isso é útil se você quiser manter o foco em uma caixa de texto para entrada de dados (e manter o teclado virtual habilitado) enquanto o usuário clica em um botão.

O gatilho neste exemplo verifica se a propriedade está presente. Se a propriedade estiver presente, ele define a propriedade **AllowFocusOnInteraction** em um botão para **false**. Se a propriedade não estiver presente, o botão manterá seu estado original. A caixa de texto é incluída para facilitar a visualização dessa propriedade quando você executa o código.

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Width="300" Height="36"/>
        <!-- Button to set the new property on. -->
        <Button x:Name="testButton" Content="Test" Margin="12"/>
    </StackPanel>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="propertyPresentStateGroup">
            <VisualState>
                <VisualState.StateTriggers>
                    <!--Trigger will activate if the AllowFocusOnInteraction property is present-->
                    <local:IsPropertyPresentTrigger 
                        TypeName="Windows.UI.Xaml.FrameworkElement" 
                        PropertyName="AllowFocusOnInteraction" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="testButton.AllowFocusOnInteraction" 
                            Value="False"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### Exemplo 2: novo valor de enumeração

Este exemplo mostra como definir valores de enumeração diferentes com base em se um valor estiver presente. Ele usa um gatilho de estado personalizado para obter o mesmo resultado conforme o exemplo anterior de chat. Neste exemplo, você usa o novo escopo de entrada ChatWithoutEmoji se o dispositivo estiver executando o Windows 10, versão 1607. Caso contrário, o escopo de entrada **Chat** é usado. Os estados visuais que usam esse gatilho são configurados em estilo *if-else* em que o escopo de entrada é escolhido com base na presença do novo valor de enumeração.

**C#**
```csharp
class IsEnumPresentTrigger : StateTriggerBase
{
    public string EnumTypeName { get; set; }
    public string EnumValueName { get; set; }

    private Boolean _isPresent;
    private bool? _isEnumValuePresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;

            if (_isEnumValuePresent == null)
            {
                // Call into ApiInformation method to determine if value is present.
                _isEnumValuePresent =
                ApiInformation.IsEnumNamedValuePresent(EnumTypeName, EnumValueName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isEnumValuePresent);
        }
    }
}
```

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <TextBox x:Name="messageBox"
     AcceptsReturn="True" TextWrapping="Wrap"/>


    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="EnumPresentStates">
            <!--if-->
            <VisualState x:Name="isPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="ChatWithoutEmoji"/>
                </VisualState.Setters>
            </VisualState>
            <!--else-->
            <VisualState x:Name="isNotPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="False"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="Chat"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```
## Artigos relacionados

- [Guia para aplicativos UWP](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Detectando dinamicamente recursos com contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)



<!--HONumber=Jun16_HO4-->


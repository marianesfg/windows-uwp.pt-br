---
author: Karl-Bridge-Microsoft
Description: Além de usar comandos de voz na Cortana para acessar recursos do sistema, você também pode usar comandos de voz por meio da Cortana para iniciar um aplicativo em primeiro plano e especificar uma ação ou um comando para ser executado dentro do aplicativo.
title: Iniciar um aplicativo em primeiro plano com comandos de voz na Cortana
ms.assetid: 8D3D1F66-7D17-4DD1-B426-DCCBD534EF00
label: Cortana-Launch a foreground app
template: detail.hbs
---

# Ativar um aplicativo em primeiro plano com comandos de voz por meio da Cortana

Além de usar comandos de voz na **Cortana** para acessar os recursos do sistema, você também pode estender a **Cortana** com recursos e funcionalidades de seu aplicativo. Usando comandos de voz, seu aplicativo pode ser ativado para o primeiro plano e uma ação ou comando executado dentro do aplicativo. 

**APIs importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Elementos e atributos VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Quando um aplicativo manipula um comando de voz em primeiro plano, ele tem o foco e a Cortana é ignorada. Se preferir, você pode ativar o aplicativo e executar um comando como uma tarefa em segundo plano. Nesse caso, a Cortana retém o foco e seu aplicativo retorna todos os comentários e os resultados por meio da tela da **Cortana** e da voz da **Cortana**.

Os comandos de voz que exigem contexto adicional ou entrada do usuário (como enviar uma mensagem a um contato específico) são manipulados mais facilmente em um aplicativo em primeiro plano, enquanto comandos básicos (como listagem de futuras viagens) podem ser manipulados na **Cortana** por meio de um aplicativo em segundo plano.

Se você quiser ativar um aplicativo no primeiro plano com comandos de voz, consulte [Ativar um aplicativo em segundo plano com comandos de voz por meio da Cortana](launch-a-background-app-with-voice-commands-in-cortana.md).

> **Observação**  
> Comando de voz é uma expressão única com um propósito específico, definido em um arquivo VCD (Definição de Comando de Voz), direcionado para um aplicativo instalado por meio da **Cortana**.

> Um arquivo VCD define um ou mais comandos de voz, cada um com um propósito exclusivo.

> A definição de um comando de voz pode variar em complexidade. Pode dar suporte a tudo, desde uma única expressão restrita até uma coleção de expressões mais flexíveis e de linguagem natural, todas indicando o mesmo propósito.

Para demonstrar recursos de aplicativos de primeiro plano, usaremos um aplicativo de planejamento e gerenciamento de viagens chamado **Adventure Works** do [Amostra de comando de voz da Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899).

Para criar uma nova viagem do **Adventure Works** sem a **Cortana**, o usuário pode iniciar o aplicativo e navegar até a página **Nova viagem**. Para exibir uma viagem existente, o usuário pode iniciar o aplicativo, navegar até a página **Viagens futuras** e selecionar a viagem.

Em vez disso, usando os comandos de voz da **Cortana**, o usuário pode simplesmente dizer "Adventure Works, adicione uma viagem" ou "Adicione uma viagem no Adventure Works" para iniciar o aplicativo e navegar até a página **Nova viagem**. Por sua vez, dizer "Adventure Works, mostre minha viagem para Londres" iniciará o aplicativo e navegará até a página de detalhes da **Viagem**, mostrada aqui.

![cortana iniciando aplicativo em primeiro plano](images/cortana-foreground-with-adventureworks.png)

Essas são as etapas básicas para adicionar a funcionalidade de comando por voz e integrar a Cortana ao seu aplicativo usando a controle por voz ou entrada do teclado:

1.  Crie um arquivo VCD. Esse é um documento XML que define todos os comandos falados que o usuário pode dizer para iniciar ações ou invocar comandos ao ativar seu aplicativo. Consulte [**Elementos e atributos VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).
2.  Registre os conjuntos de comandos no arquivo VCD quando o aplicativo for iniciado.
3.  Manipule a ativação por comando de voz, a navegação no aplicativo e a execução do comando.

**Pré-requisitos:  **

Se você for iniciante no desenvolvimento de aplicativos da Plataforma Universal do Windows (UWP), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

-   [Criar seu primeiro aplicativo](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Diretrizes de experiência do usuário:  **

Consulte as [Diretrizes de design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233), para obter informações sobre como integrar seu aplicativo à **Cortana** e as [Diretrizes de design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obter dicas úteis para criar um aplicativo interessante e prático, habilitado para controle por voz.

## <span id="Create_a_new_solution_with_project_in_Visual_Studio"></span><span id="create_a_new_solution_with__project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH__PROJECT_IN_VISUAL_STUDIO"></span>Criar uma nova solução com um projeto principal no Visual Studio


1.  Inicie o Microsoft Visual Studio 2015.

    A página inicial do Visual Studio 2015 é exibida.

2.  No menu **Arquivo**, selecione **Novo** > **Projeto**

    A caixa de diálogo **Novo Projeto** será exibida. O painel esquerdo da caixa de diálogo permite que você selecione o tipo de modelos a exibir.

3.  No painel esquerdo, expanda **Instalado > Modelos > Visual C\# > Windows** e escolha o grupo de modelos **Universal**. O painel central da caixa de diálogo exibe uma lista de modelos de projetos para aplicativos UWP (Plataforma Universal do Windows).
4.  No painel central, selecione o modelo **Aplicativo em Branco (Universal do Windows)**.

    O modelo **Aplicativo em Branco** cria um aplicativo UWP básico que é compilado e executado, mas não contém controles de interface do usuário ou dados. Você adicionará controles ao aplicativo durante este tutorial.

5.  Na caixa de texto **Nome**, digite o nome do seu projeto. Para este exemplo, usamos "AdventureWorks".
6.  Clique em **OK** para criar o projeto.

    O Microsoft Visual Studio criará seu projeto e o exibirá no **Gerenciador de Soluções**.

## <span id="Add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="ADD_IMAGE_ASSETS_TO_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"></span>Adicionar ativos de imagem ao projeto e especificá-los no manifesto do aplicativo
      
Os aplicativos UWP (Plataforma Universal do Windows) podem selecionar automaticamente imagens adequadas com base em configurações específicas e recursos do dispositivo (alto contraste, pixels efetivos, localidade etc.) Tudo o que você precisa fazer é fornecer as imagens e se certificar de usar a convenção de nomenclatura apropriada e a organização de pasta dentro do projeto do aplicativo para as versões de recursos diferentes. Se você não fornecer as versões de recurso recomendadas, a acessibilidade, a localização e a qualidade da imagem poderão sofrer, dependendo das preferências do usuário, habilidades, tipo de dispositivo e local.

Para obter mais detalhes sobre os recursos de imagem dos fatores de escala e de alto contraste, consulte [Diretrizes de ativos de bloco e ícone](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets).

Você nomeia recursos usando qualificadores. Os qualificadores de recursos são modificadores de pasta e nome de arquivo que identificam o contexto em que uma determinada versão de um recurso deve ser usada.

A convenção de nomenclatura padrão é `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`. Por exemplo, `images/en-US/logo.scale-100_contrast-white.png`, que pode ser referenciado no código usando apenas a pasta raiz e o nome do arquivo: `images/logo.png`. Consulte [Como nomear recursos usando qualificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx)

Recomendamos marcar o idioma padrão nos arquivos de recursos de cadeia de caracteres (como `en-US\resources.resw`) e o fator de escala padrão em imagens (como `logo.scale-100.png`), mesmo que você não pretenda fornecer recursos localizados ou de várias resoluções no momento. No entanto, é recomendável, no mínimo, que você forneça ativos para os fatores de escala 100, 200 e 400.

> *Importante

> O ícone do aplicativo usado na área de título da tela da **Cortana** é o ícone Square44x44Logo especificado no arquivo "Package.appxmanifest". 
    
## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>Criar um arquivo VCD

1. No Visual Studio, clique com o botão direito do mouse no nome do projeto principal e selecione **Adicionar > Novo Item**. Adicione um **Arquivo XML**
2. Digite um nome para o arquivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) (por exemplo, "AdventureWorksCommands.xml") e clique em Adicionar. 
3. No **Gerenciador de Soluções**, selecione o arquivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593).
4.  Na janela **Propriedades**, defina **Ação de compilação** como **Conteúdo** e defina **Copiar para diretório de saída** como **Copiar se mais recente**

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>Editar arquivo VCD


Adicione um **VoiceCommands** elemento com um **xmlns** atributo apontando para

2. Para cada idioma suportado por seu aplicativo, crie um elemento [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) que contenha os comandos de voz suportados por seu aplicativo.

  Você pode declarar vários elementos [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331), cada um com um atributo [**xml:lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) diferente para que seu aplicativo seja usado em diferentes mercados. Por exemplo, um aplicativo para os Estados Unidos poderá ter um [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) para inglês e um [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) para espanhol.

  >  **Cuidado**  
  Para ativar um aplicativo e iniciar uma ação usando um comando de voz, o aplicativo deve registrar um arquivo VCD que contenha um [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) com um idioma correspondente ao idioma do controle por voz selecionado pelo usuário para seu dispositivo. O idioma de controle por voz está localizado em **Configurações > Sistema > Controle por Voz >Idioma do Controle por Voz**

3. Adicione um elemento **Command** para cada comando ao qual você deseja dar suporte.

  Cada **Command** declarado em um arquivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) deve incluir estas informações:

  - Um atributo **Name** que seu aplicativo usa para identificar o comando de voz em tempo de execução. 
  - Um elemento **Example** que contém uma frase que descreve como um usuário pode invocar o comando. **A Cortana** mostra este exemplo quando o usuário diz "O que posso dizer?", "Ajuda" ou quando toca em **Ver mais**    
  -   Um elemento **ListenFor** que contém as palavras ou frases que o aplicativo reconhece como um comando. Cada elemento **ListenFor** pode conter referências a um ou mais elementos **PhraseList** que contenham palavras específicas relevantes para o comando.
  > **Observação**  
Os elementos  **ListenFor** não podem ser modificados programaticamente. No entanto, os elementos **PhraseList** que estão associados aos elementos **ListenFor** podem ser modificados programaticamente. Os aplicativos devem modificar o conteúdo de **PhraseList** no tempo de execução com base no conjunto de dados gerado conforme o usuário usa o aplicativo. Consulte [Modificar dinamicamente listas de frases de VCD (Definição de Comando de Voz)](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)

  -   Um elemento **Feedback** que contém o texto para a **Cortana** exibir e falar quando o aplicativo é iniciado.

Um elemento **Navigate** indica que o comando de voz ativa o aplicativo no primeiro plano. Neste exemplo, o comando ```showTripToDestination``` é uma tarefa em primeiro plano.

O elemento **VoiceCommandService** indica que o comando de voz ativa o aplicativo em segundo plano. O valor do atributo **Target** deste elemento deve corresponder ao valor do atributo **Name** do elemento [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) no arquivo package.appxmanifest. Neste exemplo, os comandos ```whenIsTripToDestination``` e ```cancelTripToDestination``` são tarefas em segundo plano que especificam o nome do serviço de aplicativo, como "AdventureWorksVoiceCommandService".

Para obter mais detalhes, consulte a referência [**Elementos e atributos VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).

Esta é uma parte do arquivo [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) que define os comandos de voz em en-us para o aplicativo **Adventure Works**.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <span id="Install_the_VCD_commands"></span><span id="install_the_vcd_commands"></span><span id="INSTALL_THE_VCD_COMMANDS"></span>Instalar os comandos VCD


Seu aplicativo deve ser executado uma vez para instalar o VCD. 

>  **Observação**  
Dados de comandos de voz não são preservados entre instalações do aplicativo. Para garantir que os dados de comandos de voz de seu aplicativo permaneçam intactos, considere inicializar o arquivo VCD a cada vez que seu aplicativo for iniciado ou ativado ou manter uma configuração que indique se o VCD está instalado no momento.

No arquivo "app.xaml.cs":

1. Adicione a seguinte diretiva de uso:  
```csharp
using Windows.Storage;
```
2. Marque o método "OnLaunched" com o modificador async.  
```csharp
protected async override void OnLaunched(LaunchActivatedEventArgs e)
```
3. Chame [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205) no manipulador [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) para registrar os comandos de voz que o sistema deve reconhecer.

  Na amostra do Adventure Works, primeiro definimos um objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). 

  Em seguida, chamamos [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) para inicializá-lo com nosso arquivo "AdventureWorksCommands.xml".

  Esse objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) é então passado para [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205)    
```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
    await Package.Current.InstalledLocation.GetFileAsync(
      @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
    InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}
```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Manipulação de ativação e execução de comandos por voz

Especifique como seu aplicativo responderá às ativações por comando de voz subsequentes (após ele ser iniciado e os conjuntos de comandos de voz serem instalados).

1.  Confirmar que o aplicativo foi ativado por comando por voz.

    Substitua o evento [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) e verifique se [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) é [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693)

2.  Determine o nome do comando e o que foi falado.

    Obtenha uma referência a um objeto [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) de [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) e consulte a propriedade [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) de um objeto [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432).

    Para determinar o que o usuário disse, verifique o valor de [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) ou das propriedades semânticas da frase reconhecida no dicionário [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443).

3.  Execute a ação apropriada no aplicativo, por exemplo, navegando até a página desejada.

Para este exemplo, nos referimos ao VCD na Etapa 3: editar o arquivo VCD.

Ao termos o resultado do reconhecimento por voz para o comando por voz, temos o nome do comando do primeiro valor na matriz [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438). Como o arquivo VCD definiu mais de um comando de voz possível, precisamos comparar o valor em relação os nomes dos comando no VCD e executar a ação apropriada.

A ação mais comum do aplicativo é navegar até uma página com conteúdo relevante ao contexto do comando de voz. Neste exemplo, navegamos para uma página **TripPage** e transmitimos o valor do comando de voz, como o comando foi inserido e a frase de "destino" reconhecida (se aplicável). Como opção, o aplicativo pode enviar um parâmetro de navegação para [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) ao navegar até a página.

Você pode descobrir se o comando de voz inicializado pelo aplicativo foi realmente falado ou digitado como texto, a partir do dicionário [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) usando a chave **commandMode**. O valor dessa tecla será "voz" ou "texto". Se o valor da chave for "voz", considere o uso de sintetização de voz ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) em seu aplicativo para dar feedback falado ao usuário.

Use [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) para descobrir o conteúdo falado nas restrições **PhraseList** ou **PhraseTopic** de um elemento **ListenFor**. A chave de dicionário é o valor do atributo **Label** do elemento **PhraseList** ou **PhraseTopic**. Aqui, mostramos como acessar o valor da frase **{destination}**.

``` csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <span id="related_topics"></span>Artigos relacionados


**Desenvolvedores**
* [Interações da Cortana](cortana-interactions.md)
* [Definir restrições de reconhecimento personalizadas](define-custom-recognition-constraints.md)
* [**Elementos e atributos VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designers**
* [Diretrizes para design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemplos**
* [Amostra de comando de voz da Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->



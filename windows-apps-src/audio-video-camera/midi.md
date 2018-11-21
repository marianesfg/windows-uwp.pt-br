---
author: drewbatgit
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: Este artigo mostra como enumerar dispositivos MIDI (Interface Digital de Instrumento Musical) e enviar e receber mensagens MIDI de um aplicativo Universal do Windows.
title: MIDI
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36d1e4afd620b871d4273699aea5c02cc9faec80
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7443325"
---
# <a name="midi"></a>MIDI



Este artigo mostra como enumerar dispositivos MIDI (Interface Digital de Instrumento Musical) e enviar e receber mensagens MIDI de um aplicativo Universal do Windows. Windows 10 dá suporte a MIDI por USB (drivers compatíveis com a classe e mais proprietários), MIDI sobre Bluetooth LE (edição de aniversário do Windows 10 e posterior) e por meio de produtos de terceiros disponíveis gratuitamente, MIDI pela Ethernet e roteado MIDI.

## <a name="enumerate-midi-devices"></a>Enumerar dispositivos MIDI

Antes de enumerar e usar dispositivos MIDI, adicione os seguintes namespaces ao seu projeto.

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

Adicione um controle [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) à página XAML que irá permitir que o usuário selecione um dos dispositivos de entrada MIDI conectados ao sistema. Adicione outro para listar os dispositivos de saída MIDI.

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

O método [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) da classe [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) é usado para enumerar os vários tipos diferentes de dispositivos que são reconhecidos pelo Windows. Para especificar que você deseja apenas o método para localizar dispositivos de entrada MIDI, use a cadeia de caracteres do seletor retornada por [**MidiInPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894779). **FindAllAsync** retorna um [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) que contém um **DeviceInformation** para cada dispositivo de entrada MIDI registrado no sistema. Se a coleção retornada não contém itens, então não há nenhum dispositivo de entrada MIDI disponível. Se houver itens na coleção, percorra os objetos **DeviceInformation** e adicione o nome de cada dispositivo para o dispositivo de entrada MIDI **ListBox**.

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

Enumerar os dispositivos de saída MIDI funciona exatamente da mesma maneira como enumerar dispositivos de entrada, exceto por ter que especificar a cadeia de caracteres do seletor retornado por [**MidiOutPort.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894845) ao chamar **FindAllAsync**.

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]



## <a name="create-a-device-watcher-helper-class"></a>Criar uma classe auxiliar de inspetor de dispositivo

O namespace [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) fornece o [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) que pode notificar seu aplicativo quando dispositivos forem adicionados ou removidos do sistema, ou se as informações para um dispositivo forem atualizadas. Desde que os aplicativos habilitados para MIDI normalmente estão interessados em dispositivos de entrada e saída, este exemplo cria uma classe auxiliar que implementa o padrão **DeviceWatcher**, de modo que o mesmo código pode ser usado para ambos os MIDI de entrada e saída, sem a necessidade de duplicação.

Adicione uma nova classe ao seu projeto para servir como o inspetor de dispositivos. Neste exemplo,a classe é denominada **MyMidiDeviceWatcher**. O restante do código desta seção é usado para implementar a classe auxiliar.

Adicione algumas variáveis de membro à classe:

-   Um objeto [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) que monitorará trocas de dispositivos.
-   Uma cadeia caracteres do seletor de dispositivo que conterá o MIDI de entrada para uma instância e a cadeia de caracteres do seletor para outra instância de MIDI de saída.
-   Um controle [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) que será preenchido com os nomes dos dispositivos disponíveis.
-   Um [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) que é necessário para atualizar a interface do usuário de um thread diferente do thread da interface do usuário.

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

Adicione uma propriedade [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) que é usada para acessar a lista atual de dispositivos fora da classe auxiliar.

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

No construtor de classe, o chamador passa à cadeia de caracteres do seletor de dispositivos MIDI, o **ListBox** para listar os dispositivos e o **Dispatcher** necessário para atualizar a interface do usuário.

Chame [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) para criar uma nova instância da classe **DeviceWatcher**, passando a cadeia de caracteres do seletor de dispositivo MIDI.

Registre manipuladores para os manipuladores de eventos do inspetor.

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

O **DeviceWatcher** tem os eventos a seguir:

-   [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450) - Gerado quando um novo dispositivo é adicionado ao sistema.
-   [**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453) - Gerado quando um dispositivo é removido do sistema.
-   [**Updated**](https://msdn.microsoft.com/library/windows/apps/br225458) - Gerado quando as informações associadas a um dispositivo existente são atualizadas.
-   [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) - Gerado quando o inspetor concluiu sua enumeração do tipo de dispositivo solicitado.

No manipulador para cada um desses eventos, um método auxiliar, **UpdateDevices**, é chamado para atualizar o **ListBox** com a lista atual de dispositivos. Por **UpdateDevices** atualizar elementos de interface do usuário e esses manipuladores de eventos não serem chamados no thread da interface do usuário, cada chamada deve ser encapsulada em uma chamada para [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), que faz com que o código especificado seja executado no thread da interface do usuário.

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

O método auxiliar **UpdateDevices** chama [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) e atualiza o **ListBox** com os nomes dos dispositivos retornados como descrito anteriormente neste artigo.

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

Adicione métodos para iniciar o inspetor, usando o método [**Start**](https://msdn.microsoft.com/library/windows/apps/br225454) do objeto **DeviceWatcher** e para parar o inspetor, use o método [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456).

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

Forneça um destruidor para cancelar o registro de manipuladores de eventos do inspetor e defina o inspetor de dispositivos como nulo.

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## <a name="create-midi-ports-to-send-and-receive-messages"></a>Criar portas MIDI para enviar e receber mensagens

No code-behind da página, declare variáveis membros para armazenar duas instâncias da classe auxiliar **MyMidiDeviceWatcher**, um para dispositivos de entrada e outro para dispositivos de saída.

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

Crie uma nova instância do inspetor de classes auxiliares, passando a cadeia de caracteres do seletor de dispositivo, o **ListBox** para que sejam preenchidos e o objeto **CoreDispatcher** que pode ser acessado por meio da propriedade **Dispatcher** da página. Em seguida, chame o método para iniciar cada **DeviceWatcher** do objeto.

Logo após cada **DeviceWatcher** ser iniciado, será concluído o processo de enumerar os dispositivos atuais conectados ao sistema e aumentar seu evento [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451), o que fará com que cada **ListBox** possa ser atualizado com os dispositivos MIDI atuais.

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

Quando o usuário seleciona um item no **ListBox** de entrada MIDI, o evento [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) é gerado. No manipulador para este evento, acesse a propriedade **DeviceInformationCollection** da classe auxiliar para obter a lista atual de dispositivos. Se houver entradas na lista, selecione o objeto **DeviceInformation** com o índice correspondente para o **ListBox** do controle [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/br209768).

Crie o objeto [**MidiInPort**](https://msdn.microsoft.com/library/windows/apps/dn894770) que representa o dispositivo de entrada selecionado chamando [**MidiInPort.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894776) e passando a propriedade [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) do dispositivo selecionado.

Registre um manipulador para o evento [**MessageReceived**](https://msdn.microsoft.com/library/windows/apps/dn894781), que é acionado sempre que uma mensagem de MIDI é recebida por meio do dispositivo especificado.

[!code-cs[DeclareMidiPorts](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareMidiPorts)]

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

Quando o manipulador **MessageReceived** é chamado, a mensagem estará contida na propriedade [**Message**](https://msdn.microsoft.com/library/windows/apps/dn894783) do **MidiMessageReceivedEventArgs**. O [**Type**](https://msdn.microsoft.com/library/windows/apps/dn894726) do objeto de mensagem é um valor da enumeração [**MidiMessageType**](https://msdn.microsoft.com/library/windows/apps/dn894786) indicando o tipo de mensagem que foi recebida. Os dados da mensagem dependem do tipo da mensagem. Este exemplo verifica se a mensagem é uma observação sobre a mensagem e, em caso afirmativo, produz o canal midi, observação e a velocidade da mensagem.

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

O manipulador [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) para o dispositivo de saída **ListBox** funciona da mesma forma que o manipulador para dispositivos de entrada, exceto em casos em que nenhum manipulador de evento é registrado.

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

Depois que o dispositivo de saída é criado, você pode enviar uma mensagem, criando um novo [**IMidiMessage**](https://msdn.microsoft.com/library/windows/apps/dn911508) para o tipo de mensagem que deseja enviar. Neste exemplo, a mensagem é um [**NoteOnMessage**](https://msdn.microsoft.com/library/windows/apps/dn894817). O método [**SendMessage**](https://msdn.microsoft.com/library/windows/apps/dn894730) do objeto [**IMidiOutPort**](https://msdn.microsoft.com/library/windows/apps/dn894727) é chamado para enviar a mensagem.

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

Quando seu aplicativo é desativado, certifique-se de limpar os recursos de aplicativos. Cancele o registro de manipuladores de eventos e defina o MIDI na porta e os objetos de porta de saída para nulo. Pare os inspetores de dispositivo e defina-os como null.

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## <a name="using-the-built-in-windows-general-midi-synth"></a>Usando o sintetizador Windows General MIDI interno

Quando você enumerar dispositivos MIDI de saída usando a técnica descrita acima, seu aplicativo descobre um dispositivo MIDI chamado "Microsoft GS Wavetable Synth". Este é um sintetizador General MIDI interno que você pode jogar de seu aplicativo. No entanto, a tentativa de criar um outport MIDI para este dispositivo falhará, a menos que você tenha incluído a extensão do SDK para o sintetizador interno em seu projeto.

**Para incluir a extensão do SDK General MIDI Synth no seu projeto de aplicativo**

1.  No **Gerenciador de Soluções**, no seu projeto, clique com o botão direito do mouse em **Referências** e selecione **Adicionar referência...**
2.  Expanda o nó **Universal do Windows**.
3.  Selecione **Extensões**.
4.  Na lista de extensões, selecione **Microsoft General MIDI DLS para aplicativos universais do Windows**.
    > [!NOTE] 
    > Se houver várias versões da extensão, certifique-se de selecionar a versão que corresponde ao seu aplicativo. Você pode ver qual versão do SDK destina-se ao seu aplicativo na guia **Aplicativo** das propriedades do projeto.

 

 





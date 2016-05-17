---
author: drewbatgit
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: Este artigo mostra como usar as APIs no namespace Windows.Media.Audio para criar gráficos de áudio para cenários de roteamento, mixagem e processamento de áudio.
title: Gráficos de áudio
---

# Gráficos de áudio

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo mostra como usar as APIs no namespace [**Windows.Media.Audio**](https://msdn.microsoft.com/library/windows/apps/dn914341) para criar gráficos de áudio para cenários de roteamento, mixagem e processamento de áudio.

Um gráfico de áudio é um conjunto de nós de áudio interconectados, por meio dos quais fluem os dados de áudio. Os nós de entrada de áudio fornecem dados de áudio para o gráfico a partir de dispositivos de entrada de áudio, arquivos de áudio ou de código personalizado. Os nós de saída de áudio são o destino do áudio processado pelo gráfico. O áudio pode ser roteado para fora do gráfico para dispositivos de saída de áudio, arquivos de áudio ou código personalizado. O último tipo de nó é um nó de submixagem que aceita áudio de um ou mais nós e os combina em uma única saída que pode ser roteada para outros nós no gráfico. Depois que todos os nós são criados e as conexões entre eles são configuradas, basta iniciar o gráfico de áudio para que os dados de áudio fluam dos nós de entrada, por meio de todos os nós de submixagem, para os nós de saída. Esse modelo torna cenários, como gravação do microfone de um dispositivo para um arquivo de áudio, reprodução de áudio de um arquivo do alto-falante do dispositivo ou mixagem de áudio de várias fontes rápida e fácil de implementar.

Cenários adicionais são habilitados com a adição de efeitos de áudio ao gráfico de áudio. Cada nó em um gráfico de áudio pode ser preenchido com zeros ou mais efeitos de áudio que executam o processamento de áudio no áudio que passa pelo nó. Há vários efeitos internos como eco, equalizador, limitação e reverberação que podem ser anexados a um nó de áudio com apenas algumas linhas de código. Você também pode criar seus próprios efeitos de áudio personalizados que funcionam exatamente como os efeitos internos.

**Observação**  
O [exemplo AudioGraph UWP](http://go.microsoft.com/fwlink/?LinkId=619481) implementa o código abordado nesta visão geral. Você pode baixar a amostra para ver o código usado no contexto ou usá-lo como ponto de partida para seu próprio aplicativo.

## Escolhendo AudioGraph ou XAudio2 do Windows Runtime

As APIs de gráfico de áudio do Windows Runtime oferecem a funcionalidade que também pode ser implementada com as [APIs XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) baseadas em COM. A seguir estão os recursos da estrutura de gráfico de áudio do Windows Runtime que diferem de XAudio2.

-   As APIs de gráfico de áudio do Windows Runtime são significativamente mais fáceis de usar que XAudio2.
-   As APIs de gráfico de áudio do Windows Runtime podem ser usadas em C#, além de terem suporte para C++.
-   As APIs de gráfico de áudio do Windows Runtime podem usar arquivos de áudio, incluindo formatos de arquivo compactado, diretamente. XAudio2 opera somente em buffers de áudio e não fornece nenhuma funcionalidade de E/S.
-   As APIS de gráfico de áudio do Windows Runtime podem usar o pipeline de áudio de baixa latência no Windows 10.
-   As APIs de gráfico de áudio do Windows Runtime dão suporte à alternância automática de pontos de extremidade quando são usados parâmetros de ponto de extremidade padrão. Por exemplo, se o usuário alterna do alto-falante do dispositivo para um fone de ouvido, o áudio é automaticamente redirecionado para a nova entrada.

## Classe AudioGraph

A classe [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/dn914176) é pai de todos os nós que compõem o gráfico. Use esse objeto para criar instâncias de todos os tipos de nós de áudio. Crie uma instância da classe **AudioGraph** inicializando um objeto [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185), contendo configurações do gráfico, e chame [**AudioGraph.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn914216). O [**CreateAudioGraphResult**](https://msdn.microsoft.com/library/windows/apps/dn914273) retornado dá acesso ao gráfico de áudio criado ou fornece um valor de erro em caso de falha na criação do gráfico de áudio.

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   Todos os tipos de nó áudio são criados usando os métodos Create\* da classe **AudioGraph**.
-   O método [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244) faz com que o gráfico de áudio comece a processar dados de áudio. O método [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245) para o processamento do áudio. Cada nó no gráfico pode ser iniciado e interrompido independentemente enquanto o gráfico está sendo executado, mas nenhum nó está ativo quando o gráfico é interrompido. [
              **ResetAllNodes**
            ](https://msdn.microsoft.com/library/windows/apps/dn914242) faz com que todos os nós no gráfico descartem todos os dados em seus buffers de áudio no momento.
-   O evento [**QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn914241) ocorre quando o gráfico está iniciando o processamento de um novo quantum de dados de áudio. O evento [**QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240) ocorre quando o processamento de um quantum é concluído.

-   A única propriedade [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185) necessária é [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724). A especificação desse valor permite que o sistema otimize o pipeline de áudio para a categoria especificada.
-   O tamanho de quantum do gráfico de áudio determina o número de amostras que são processadas de uma só vez. Por padrão, o tamanho do quantum é de 10 ms com base na taxa de amostra padrão. Se você especificar um tamanho de quantum definido a propriedade [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205), também deve definir a propriedade [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208) como **ClosestToDesired**; caso contrário, o valor fornecido será ignorado. Se esse valor for usado, o sistema escolherá o tamanho de quantum mais próximo possível daquele que você especificou. Para determinar o tamanho real quantum, verifique o [**SamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914243) de **AudioGraph** depois que ele for criado.
-   Se você pretende usar o gráfico de áudio com arquivos apenas e não pretende enviar saída para um dispositivo de áudio, é recomendável que você use o tamanho de quantum padrão não definindo a propriedade [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205).
-   A propriedade [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522) determina a quantidade de processamento que o dispositivo de renderização principal realiza na saída do gráfico de áudio. A configuração **Default** permite que o sistema use o processamento de áudio padrão na categoria de renderização de áudio especificada. Esse processamento pode melhorar significativamente o som do áudio em alguns dispositivos, especialmente dispositivos móveis com alto-falantes pequenos. A configuração **Raw** pode melhorar o desempenho, minimizando a quantidade de processamento de sinal realizada, mas pode resultar em som de qualidade inferior em alguns dispositivos.
-   Se o [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208) é definido como **LowestLatency**, o gráfico de áudio usará automaticamente **Raw** para [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522)
-   [
            **EncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn958523) determina o formato de áudio usado pelo gráfico. Há suporte para formatos float de 32 bits somente.
-   O [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524) define o dispositivo de renderização principal para o gráfico de áudio. Se você não definir isso, o dispositivo padrão do sistema será usado. O dispositivo de renderização principal é usado para calcular os tamanhos de quantum para outros nós do gráfico. Se não houver dispositivos de renderização de áudio presentes no sistema, a criação de gráfico de áudio falhará.

Você pode permitir que o gráfico de áudio use o dispositivo de renderização de áudio padrão ou usar a classe [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) para obter uma lista dos dispositivos de renderização de áudio disponíveis no sistema chamando [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) e passando o seletor de dispositivo de renderização de áudio retornado por [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817). É possível escolher um dos objetos **DeviceInformation** retornados programaticamente ou mostrar a interface do usuário para permitir que o usuário selecione um dispositivo e, em seguida, usá-lo para definir a propriedade [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524).

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  Nó de entrada de dispositivo

Um nó de entrada de dispositivo passa áudio para o gráfico a partir de um dispositivo de captura de áudio conectado ao sistema, como um microfone. Crie um objeto [**DeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) que usa o dispositivo de captura de áudio padrão do sistema chamando [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218). Forneça uma [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724) para permitir que o sistema otimize o pipeline de áudio para a categoria especificada.

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

Se você quiser especificar um dispositivo de captura de áudio específico para o nó de entrada do dispositivo, pode usar a classe [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) para obter uma lista dos dispositivos de captura de áudio disponíveis no sistema chamando [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) e passando o seletor de dispositivo de renderização de áudio retornado por [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817). É possível escolher um dos objetos **DeviceInformation** retornados programaticamente ou mostrar a interface do usuário para permitir que o usuário selecione um dispositivo e, em seguida, passá-lo para [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218)

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  Nó de saída do dispositivo

Um nó de saída do dispositivo empurra áudio do gráfico para um dispositivo de renderização de áudio, como alto-falantes ou um fone de ouvido. Crie um [**DeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098) chamando [**CreateDeviceOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn958525). O nó de saída usa o [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524) do gráfico de áudio.

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  Nó de entrada do arquivo

Um nó de entrada de arquivo permite que você alimente dados de um arquivo de áudio no gráfico. Crie um [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) chamando [**CreateFileInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914226)

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   Nós de entrada do arquivo dão suporte aos seguintes formatos de arquivo: mp3, wav, wma, m4a
-   Defina a propriedade [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) para especificar o deslocamento de tempo para o arquivo no qual a reprodução deve começar. Se essa propriedade for null, o início do arquivo será usado. Defina a propriedade [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118) para especificar o deslocamento de tempo para o arquivo no qual a reprodução deve terminar. Se essa propriedade for nula, será usado o fim do arquivo. O valor de hora inicial deve ser menor que o valor de hora final, e o valor de hora final deve ser menor ou igual à duração do arquivo de áudio, que pode ser determinada verificando o valor da propriedade [**Duration**](https://msdn.microsoft.com/library/windows/apps/dn914116).
-   Busque uma posição no arquivo de áudio chamando [**Seek**](https://msdn.microsoft.com/library/windows/apps/dn914127) e especificando o deslocamento de tempo para o arquivo no qual a posição de reprodução deve ser movida. O valor especificado deve estar dentro do intervalo de [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) e [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118). Obtenha a posição de reprodução atual do nó com a propriedade somente leitura [**Position**](https://msdn.microsoft.com/library/windows/apps/dn914124).
-   Habilite o loop do arquivo de áudio definindo a propriedade [**LoopCount**](https://msdn.microsoft.com/library/windows/apps/dn914120). Quando não é nulo, esse valor indica o número de vezes em que o arquivo será reproduzido após a reprodução inicial. Assim, por exemplo, a definição de **LoopCount** como 1 fará com que o arquivo seja reproduzido 2 vezes no total e defini-lo como 5 fará com que o arquivo seja reproduzido 6 vezes no total. A definição de **LoopCount** como nulo fará com que o arquivo entre em um loop infinito. Para interromper o loop, defina o valor como 0.
-   Ajuste a velocidade em que o arquivo de áudio é reproduzido definindo [**PlaybackSpeedFactor**](https://msdn.microsoft.com/library/windows/apps/dn914123). Um valor 1 indica a velocidade original do arquivo, 0,5 indica metade da velocidade e 2 é velocidade dupla.

##  Nó de saída de arquivo

Um nó de saída de arquivo permite que você direcione dados de áudio do gráfico para um arquivo de áudio. Crie um [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133) chamando [**CreateFileOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914227)

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   Nós de saída do arquivo dão suporte aos seguintes formatos de arquivo: mp3, wav, wma, m4a
-   Você deve chamar [**AudioFileOutputNode.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914144) para parar o processamento do nó antes de chamar [**AudioFileOutputNode.FinalizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914140); caso contrário, uma exceção será lançada.

##  Nó de entrada de quadro de áudio

Um nó de entrada de quadro de áudio permite que você envie dados de áudio que você gera em seu próprio código para o gráfico de áudio. Isso habilita cenários como a criação de um sintetizador de software personalizado. Crie um [**AudioFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914147) chamando [**CreateFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914230)

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

O evento [**FrameInputNode.QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn958507) é disparado quando o gráfico de áudio está pronto para começar o processamento do próximo quantum de dados de áudio. Forneça dados de áudio personalizados gerados de dentro do manipulador para esse evento.

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   O objeto [**FrameInputNodeQuantumStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958533) passado para o manipulador de evento **QuantumStarted** expõe a propriedade [**RequiredSamples**](https://msdn.microsoft.com/library/windows/apps/dn958534) que indica quantas amostras são necessárias para que o gráfico de áudio preencha o quantum a ser processado.
-   Chame [**AudioFrameInputNode.AddFrame**](https://msdn.microsoft.com/library/windows/apps/dn914148) para passar um objeto [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) preenchido com dados de áudio para o gráfico.
-   Veja a seguir um exemplo de implementação do método auxiliar **GenerateAudioData**.

Para preencher um [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) com dados de áudio, você deve obter acesso ao buffer de memória subjacente do quadro de áudio. Para fazer isso, você deve inicializar a interface COM **IMemoryBufferByteAccess** adicionando o seguinte código dentro de seu namespace.

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

O código a seguir mostra um exemplo de implementação de um método auxiliar **GenerateAudioData** que cria um [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) e o preenche com dados de áudio.

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   Como esse método acessa o buffer bruto subjacente aos tipos do Windows Runtime, ele deve ser declarado usando a palavra-chave **unsafe**. Você também deve configurar seu projeto no Microsoft Visual Studio para permitir a compilação de código não seguro. Para fazer isso, abra a página **Propriedades** do projeto, clique na página de propriedade **Build** e selecione a caixa de seleção **Permitir Código Não Seguro**.
-   Inicialize uma nova instância de [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871), no namespace **Windows.Media**, passando o tamanho de buffer desejado para o construtor. O tamanho do buffer é o número de amostras multiplicado pelo tamanho de cada exemplo.
-   Obtenha o [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454) do quadro de áudio chamando [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878)
-   Obtenha uma instância da interface COM [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505) do buffer de áudio chamando [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457)
-   Obtenha um ponteiro para dados de buffer de áudio bruto chamando [**IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506) e transmita-o para o tipo de dados de amostra dos dados de áudio.
-   Preencha o buffer com dados e retorne o [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) para envio para o gráfico de áudio.

##  Nó de saída de quadro de áudio

Um nó de saída de quadro de áudio permite receber e processar a saída de dados de áudio do gráfico de áudio com o código personalizado que você criar. Um cenário de exemplo para isso é a análise do sinal na saída de áudio. Crie um [**AudioFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914166) chamando [**CreateFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914233)

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

O evento [**AudioGraph.QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240) é disparado quando o gráfico de áudio conclui o processamento de um quantum de dados de áudio. Você pode acessar os dados de áudio de dentro do manipulador para esse evento.

[!code-cs[QuantumProcessed](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumProcessed)]

-   Chame [**GetFrame**](https://msdn.microsoft.com/library/windows/apps/dn914171) para obter um objeto [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) preenchido com dados de áudio do gráfico.
-   Veja a seguir um exemplo de implementação do método auxiliar **ProcessFrameOutput**.

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   Como o exemplo de nó de entrada de quadro de áudio acima, você precisa declarar a interface COM **IMemoryBufferByteAccess** e configurar seu projeto para permitir código não seguro a fim de acessar o buffer de áudio subjacente.
-   Obtenha o [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454) do quadro de áudio chamando [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878)
-   Obtenha uma instância da interface COM **IMemoryBufferByteAccess** do buffer de áudio chamando [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457)
-   Obtenha um ponteiro para dados de buffer de áudio bruto chamando **IMemoryBufferByteAccess.GetBuffer** e transmita-o para o tipo de dados de amostra dos dados de áudio.

## Conexões de nós e nós de submixagem

Todos os nós a exposição de tipos de entrada do método **AddOutgoingConnection** que encaminha o áudio produzido pelo nó para o nó que é passado para o método. O exemplo a seguir conecta um [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) a um [**AudioDeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098), que é uma configuração simples para reproduzir um arquivo de áudio no alto-falante do dispositivo.

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

Você pode criar mais de uma conexão de um nó de entrada para outros nós. O exemplo a seguir adiciona outra conexão do [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) para um [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133). Agora, o áudio do arquivo de áudio é reproduzido no alto-falante do dispositivo e também é gravado em um arquivo de áudio.

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

Nós de saída também podem receber mais de uma conexão de outros nós. No exemplo a seguir, uma conexão é feita de um [**AudioDeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) para o nó [**AudioDeviceOutput**](https://msdn.microsoft.com/library/windows/apps/dn914098). Como o nó de saída tem conexões do nó de entrada do arquivo e do nó de entrada do dispositivo, a saída conterá uma combinação das duas fontes de áudio. **AddOutgoingConnection** fornece uma sobrecarga que permite a especificação de um valor de ganho para o sinal que passa pela conexão.

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

Embora nós de saída possam aceitar conexões de vários nós, convém criar uma combinação intermediária de sinais de um ou mais nós antes de passar a combinação para uma saída. Por exemplo, pode ser desejável definir o nível ou aplicar efeitos em um subconjunto de sinais de áudio em um gráfico. Para fazer isso, use o [**AudioSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914247). Você pode se conectar a um nó de submixagem a partir de um ou mais nós ou outros nós de submixagem de entrada. No exemplo a seguir, um novo nó de submixagem é criado com [**AudioGraph.CreateSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914236). Em seguida, são adicionadas conexões de um nó de entrada de arquivo e um nó de saída de quadro para o nó de submixagem. Por fim, o nó de submixagem é conectado a um nó de saída do arquivo.

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## Iniciando e interrompendo nós de gráfico de áudio

Quando [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244) é chamado, o gráfico de áudio começa a processar dados de áudio. Cada tipo de nó fornece métodos **Start** e **Stop** que fazem com que o nó individual inicie ou pare o processamento de dados. Quando [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245) é chamado, todo o processamento de áudio em todos os nós é parado independentemente do estado dos nós individuais, mas o estado de cada nó pode ser definido enquanto o gráfico de áudio é interrompido. Por exemplo, você pode chamar **Stop** em um nó individual enquanto o gráfico é parado e, em seguida, chamar **AudioGraph.Start** para que o nó individual permaneça no estado parado.

Todos os tipos de nó expõem a propriedade **ConsumeInput** que, quando definida como false, permite que o nó continue o processamento de áudio mas interrompe seu consumo de quaisquer dados de áudio que estejam sendo recebidos de outros nós.

Todos os tipos de nó expõem o método **Reset** que faz o nó descartar todos os dados de áudio no buffer no momento.

## Adicionando efeitos de áudio

A API do gráfico de áudio permite que você adicione efeitos de áudio a cada tipo de nó em um gráfico. Nós de saída, nós de entrada e nós de submixagem podem ter um número ilimitado de efeitos de áudio, limitado somente pelos recursos do hardware. O exemplo a seguir demonstra a adição do efeito de eco interno a um nó de submixagem.

[!code-cs[AddEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddEffect)]

-   Todos os efeitos de áudio implementam [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044). Cada nó expõe uma propriedade **EffectDefinitions** que representa a lista de efeitos aplicados ao nó. Adicione um efeito adicionando seu objeto de definição à lista.
-   Várias classes de definição de efeito são fornecidas no namespace **Windows.Media.Audio**. Por exemplo:
    -   [**EchoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914276)
    -   [**EqualizerEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914287)
    -   [**LimiterEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914306)
    -   [**ReverbEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914313)
-   Você pode criar seus próprios efeitos de áudio que implementam [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044) e aplicá-los a qualquer nó em um gráfico de áudio.
-   Cada nó expõe um método **DisableEffectsByDefinition** que desabilita todos os efeitos na lista **EffectDefinitions** do nó que foram adicionados usando a definição especificada. **EnableEffectsByDefinition** habilita os efeitos com a definição especificada.

 

 






<!--HONumber=May16_HO2-->



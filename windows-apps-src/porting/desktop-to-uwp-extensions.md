---
author: awkoren
Description: "Além das APIs normais disponíveis para todos os aplicativos UWP, há algumas extensões e APIs disponíveis somente para aplicativos de área de trabalho convertidos. Este artigo descreve essas extensões e como usá-las."
Search.Product: eADQiWindows 10XVcnh
title: "Extensões de aplicativos da área de trabalho convertidos"
translationtype: Human Translation
ms.sourcegitcommit: 09ddc8cad403a568a43e08f32abeaf0bbd40d59a
ms.openlocfilehash: 2aa55797ed3a6588b3a27158282a02827fbd2109

---

# Extensões de aplicativos da área de trabalho convertidos

Você pode aprimorar seu aplicativo de área de trabalho convertido com uma ampla gama de APIs da Plataforma Universal do Windows (UWP). No entanto, além das APIs normais disponíveis para todos os aplicativos UWP, há algumas extensões e APIs disponíveis somente para aplicativos de área de trabalho convertidos. Esses recursos se concentram em cenários como iniciar um processo quando o usuário faz logon e integração com o Explorador de Arquivos e são projetados para suavizar a transição entre o aplicativo de área de trabalho original e o pacote do aplicativo convertido.

Este artigo descreve essas extensões e como usá-las. A maioria exige modificação manual do arquivo de manifesto do seu aplicativo convertido, que contém declarações sobre as extensões usadas por seu aplicativo. Para editar o manifesto, clique com o botão direito no arquivo **Package.appxmanifest** em sua solução do Visual Studio e selecione *Exibir código*. 

## Tarefas de inicialização

As tarefas de inicialização permitem que seu aplicativo execute um executável automaticamente sempre que um usuário faz logon. 

Para declarar uma tarefa de inicialização, adicione o seguinte ao manifesto do aplicativo: 

```XML
<desktop:Extension Category="windows.startupTask" Executable="bin\MyStartupTask.exe" EntryPoint="Windows.FullTrustApplication">
    <desktop:StartupTask TaskId="MyStartupTask" Enabled="true" DisplayName="My App Service" />
</desktop:Extension>
```
- *Categoria de extensão* sempre deve ter o valor "windows.startupTask".
- *Executável da extensão* é o caminho relativo para iniciar o .exe.
- *EntryPoint da extensão* sempre deve ter o valor "Windows.FullTrustApplication".
- *StartupTask TaskId* é um identificador exclusivo para a sua tarefa. Usando esse identificador, seu aplicativo pode chamar as APIs na classe **Windows.ApplicationModel.StartupTask** para habilitar ou desabilitar uma tarefa de inicialização de forma programática.
- *StartupTask habilitado* indica se a primeira inicialização da tarefa está ativada ou desativada. As tarefas habilitadas serão executadas na próxima vez em que o usuário fizer logon (a menos que o usuário as tenha desabilitado). 
- *StartupTask DisplayName* é o nome da tarefa que aparece no Gerenciador de Tarefas. Essa sequência é localizável usando ```ms-resource```. 

Os aplicativos podem declarar várias tarefas de inicialização; cada uma será acionada e executada de forma independente. Todas as tarefas de inicialização serão exibidas no Gerenciador de Tarefas na guia **Inicialização** com o nome especificado no manifesto do aplicativo e no ícone do aplicativo. O Gerenciador de Tarefas analisará automaticamente o impacto de inicialização de suas tarefas. Os usuários podem optar por desativar manualmente as tarefas de inicialização do seu aplicativo usando o Gerenciador de Tarefas. Se um usuário desabilita uma tarefa, você não pode reativá-la programaticamente.

## Alias de execução do aplicativo

Um alias de execução do aplicativo permite que você especifique um nome de palavra-chave para o seu aplicativo. Os usuários ou outros processos podem usar essa palavra-chave para facilmente iniciar seu aplicativo como se estivesse na variável PATH, por exemplo, de Run ou um prompt de comando, sem fornecer o caminho completo. Por exemplo, se você declarar o alias "Foo", um usuário poderá digitar "Foo Bar.txt" em cmd.exe e seu aplicativo será ativado com o caminho para "Bar.txt" como parte dos argumentos do evento de ativação.

Para especificar um alias de execução do aplicativo, adicione o seguinte ao manifesto do aplicativo: 

```XML 
<uap3:Extension Category="windows.appExecutionAlias" Executable="exes\launcher.exe" EntryPoint="Windows.FullTrustApplication">
    <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="Foo.exe" />
    </uap3:AppExecutionAlias>
</uap3:Extension>
```

- *Categoria de extensão* sempre deve ter o valor "windows.appExecutionAlias".
- *Executável da extensão* é o caminho relativo para o executável a ser iniciado quando o alias é invocado.
- *EntryPoint da extensão* sempre deve ter o valor "Windows.FullTrustApplication".
- *ExecutionAlias Alias* é o nome curto do seu aplicativo. Sempre deve terminar com a extensão ".exe". 

Você só pode especificar um único alias de execução de aplicativo para cada aplicativo no pacote. Se vários aplicativos se registrarem para o mesmo alias, o sistema irá chamar o último que foi registrado. Portanto, escolha um alias exclusivo que outros aplicativos provavelmente não substituirão.

## Associações de protocolo 

Associações de protocolo habilitam cenários de interoperabilidade entre o seu aplicativo convertido e outros programas ou componentes do sistema. Quando seu aplicativo convertido é iniciado usando um protocolo, você pode especificar parâmetros específicos a serem transmitidos para seus argumentos de evento de ativação para que ele possa se comportar adequadamente. Observe que só há suporte dos parâmetros para aplicativos convertidos e confiáveis; aplicativos UWP não podem usar parâmetros.  

Para declarar uma associação de protocolo, adicione o seguinte ao manifesto do aplicativo:

```XML
<uap3:Extension Category="windows.protocol">
    <uap3:Protocol Name="myapp-cmd" Parameters="/p &quot;%1&quot;" />
</uap3:Extension>
```

- *Categoria de extensão* sempre deve ter o valor "windows.protocol". 
- *Nome do protocolo* é o nome do protocolo. 
- *Parâmetros de protocolo* é a lista de parâmetros e valores a serem transmitidos para o seu aplicativo como argumentos do evento quando ele for ativado. Observe que, se uma variável puder conter um caminho de arquivo, você deverá encapsular o valor entre aspas para que ele não seja interrompido se for transmitido um caminho que inclui espaços.

## Arquivos e integração com o Explorador de Arquivos

Os aplicativos convertidos têm uma variedade de opções de registro para lidar com determinados tipos de arquivo e integrar com o Explorador de Arquivos. Isso permite aos usuários acessar facilmente seu aplicativo como parte de seu fluxo de trabalho normal.

Para começar, primeiro adicione o seguinte ao manifesto do aplicativo: 

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="MyApp">
        ... additional elements here ...
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

- *Categoria de extensão* sempre deve ter o valor "windows.fileTypeAssociation". 
- *Nome de FileTypeAssociation* é uma ID exclusiva. Essa Id é usada internamente para gerar um ProgID com hash associado a sua associação de tipo de arquivo. Você pode usar essa Id para gerenciar as alterações em versões futuras do seu aplicativo. Por exemplo, se você quiser alterar o ícone de uma extensão de arquivo, poderá movê-lo para uma nova FileTypeAssociation com um nome diferente.  

Em seguida, adicione outros elementos filhos a essa entrada com base nos recursos específicos de que você precisa. As opções disponíveis são descritas a seguir.

### Tipos de arquivos com suporte

Seu aplicativo pode especificar que oferece suporte à abertura de tipos específicos de arquivos. Se um usuário clicar com o botão direito em um arquivo e selecionar "Abrir com", o aplicativo aparecerá na lista de sugestões.

Exemplo:

```XML
<uap:SupportedFileTypes>
    <uap:FileType>.txt</uap:FileType>
    <uap:FileType>.avi</uap:FileType>
</uap:SupportedFileTypes>
```

- *FileType* é a extensão compatível com seu aplicativo.

### Verbos de menu de contexto 

Os usuários costumam abrir arquivos simplesmente clicando duas vezes neles. No entanto, quando um usuário clica com o botão direito em um arquivo, o menu de contexto o apresenta com várias opções (conhecidas como "Verbos") que fornecem detalhes adicionais sobre como desejam interagir com o arquivo, como "Abrir", "Editar", "Visualizar" ou "Imprimir". 

Especificar um tipo de arquivo com suporte adiciona automaticamente o verbo "Abrir". No entanto, os aplicativos também podem adicionar verbos personalizados adicionais ao menu de contexto do Explorador de Arquivos. Eles permitem que o aplicativo inicie uma determinada maneira com base na seleção do usuário ao abrir um arquivo.

Exemplo: 

```XML
<uap2:SupportedVerbs>
    <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot">Edit</uap:Verb>
    <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot">Print</uap:Verb>
</uap2:SupportedVerbs>
```

- *Id do Verbo* é uma Id exclusiva do verbo. Se seu aplicativo for um aplicativo UWP, isso será transmitido para seu aplicativo como parte de seus argumentos do evento de ativação para que ele possa manipular a seleção do usuário de forma adequada. Se seu aplicativo for um aplicativo convertido confiável, receberá parâmetros (veja o próximo item). 
- *Parâmetros de Verbo* é a lista de parâmetros de argumento e valores associados ao verbo. Se seu aplicativo for um aplicativo convertido confiável, eles serão transmitidos como argumentos de evento quando ele for ativado para que você possa personalizar seu comportamento para verbos de ativação diferentes. Se uma variável puder conter um caminho de arquivo, você deverá encapsular o valor entre aspas para que ele não seja interrompido se for transmitido um caminho que inclui espaços. Observe que, se seu aplicativo for um aplicativo UWP, você não poderá transmitir parâmetros; ele receberá a Id (veja o item anterior). 
- *Verbo Estendido* especifica que o verbo deve aparecer somente se o usuário mantiver a tecla **Shift** pressionada antes de clicar com o botão direito no arquivo para mostrar o menu de contexto. Esse atributo é opcional e o padrão é *False* (por exemplo, mostrar sempre o verbo) se não listado. Você especifica esse comportamento individualmente para cada verbo (com exceção de "Abrir", que é sempre *False*). 
- *Verbo* é o nome para exibição no menu de contexto do Explorador de Arquivos. Essa sequência é localizável usando ```ms-resource```.

### Modelo de seleção múltipla

Várias seleções permitem que você especifique como seu aplicativo lida com um usuário que abre vários arquivos simultaneamente (por exemplo, selecionando 10 arquivos no Explorador de Arquivos e tocando em "Abrir").

Aplicativos da área de trabalho convertidos têm as mesmas três opções dos aplicativos de área de trabalho normais. 
- *Player*: seu aplicativo é ativado uma vez com todos os arquivos selecionados transmitidos como parâmetros de argumento.
- *Único*: o aplicativo é ativado uma vez para o primeiro arquivo selecionado. Outros arquivos são ignorados. 
- *Documento*: uma nova instância separada do seu aplicativo é ativada para cada arquivo selecionado.

Você pode definir preferências diferentes para diferentes tipos de arquivo e ações. Por exemplo, você pode abrir *Documentos* no modo *Documento* e *Imagens* no modo *Player*.

Para definir o comportamento do aplicativo, adicione o atributo *MultiSelectModel* aos elementos em seu manifesto que estão relacionados a tipos de arquivo e inicialização de arquivo. 

Definindo um modelo para um tipo de arquivo com suporte: 

```XML
<uap:FileType MultiSelectModel="Document">.txt</uap:FileType>
```

Definindo um modelo para verbos:

```XML
<uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap:Verb>
<uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap:Verb>
```

Se seu aplicativo não especificar uma opção para seleção múltipla, o padrão será *Player* se o usuário estiver abrindo 15 arquivos ou menos. Caso contrário, se seu aplicativo for um aplicativo convertido, o padrão será *Documento*. Os aplicativos UWP sempre são iniciados como *Player*. 

### Exemplo completo

Veja a seguir um exemplo completo que integra muitos elementos relacionados a arquivos e Explorador de Arquivos descritos acima: 

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="MyApp">
        <uap:SupportedFileTypes>
            <uap:FileType MultiSelectModel="Document">.txt</uap:FileType>
            <uap:FileType>.foo</uap:FileType>
    </uap:SupportedFileTypes>
    <uap2:SupportedVerbs>
            <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot">Edit</uap:Verb>
            <uap3:Verb Id="Print" Parameters="/p &quot;%1&quot">Print</uap:Verb>
    </uap2:SupportedVerbs>
    <uap:Logo>Assets\MyExtensionLogo.png</uap:Logo>
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

## Consulte também

- [Manifesto do pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)


<!--HONumber=Aug16_HO3-->



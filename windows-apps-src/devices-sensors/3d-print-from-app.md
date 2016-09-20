---
author: PatrickFarley
title: "Impressão 3D do seu aplicativo"
description: "Saiba como adicionar a funcionalidade de impressão 3D ao seu aplicativo Universal do Windows. Esse tópico aborda como iniciar a caixa de diálogo de impressão 3D após verificar se o seu modelo 3D é imprimível e está no formato correto."
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.sourcegitcommit: 61d9f5c1fca1ad2e26f052b901361813975ae357
ms.openlocfilehash: e68a9c681974152bc0d4dfa58e824f80e77dc51f

---

# Impressão 3D do seu aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

Saiba como adicionar a funcionalidade de impressão 3D ao seu aplicativo Universal do Windows. Este tópico aborda como carregar dados de geometria 3D em seu aplicativo e iniciar a caixa de diálogo de impressão 3D após assegurar que seu modelo 3D é imprimível e está no formato correto. Para obter um exemplo de trabalho desses procedimentos em ação, consulte o [Exemplo UWP de impressão 3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

## Configuração de classe


Na classe que terá a funcionalidade de impressão 3D, insira o namespace [Windows.Graphics.Printing3D](https://msdn.microsoft.com/library/windows/apps/dn998169).

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

Os seguintes namespaces adicionais serão usados nesta guia específica:

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Em seguida, dê à sua classe alguns campos de membro úteis. Declare um objeto [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) para servir como referência para a tarefa de impressão que deve ser passada para o driver de impressão. Declare um objeto [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) para armazenar o arquivo de dados 3D original. Por fim, declare um objeto [Printing3D3MFPackage](https://msdn.microsoft.com/library/windows/apps/dn998063) que representa um modelo 3D pronto para imprimir com todos os metadados necessários.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## Crie uma interface do usuário simples


Este exemplo usa três controles de usuário: um botão de carregamento, que levará um arquivo para a memória de programa, um botão de correção, que modificará o arquivo conforme necessário e um botão de impressão, que iniciará o trabalho de impressão. O código a seguir gera esses botões (com seus manipuladores de eventos de clique) no arquivo XAML da sua classe:

[!code-xml[Botões](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Insira um **TextBlock** para feedback de interface do usuário.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]

## Obtenha os dados 3D


O método pelo qual o seu aplicativo obtém dados de geometria 3D para imprimir varia. Seu aplicativo pode recuperar dados de uma varredura 3D, puxar dados de modelo de um recurso da web ou gerar uma malha 3D de forma programática usando equações. Por questões de simplicidade, este guia irá carregar um arquivo de dados 3D (de qualquer um dos vários tipos de arquivo comuns) em memória de programa do Explorador de Arquivos.

Em seu método `OnLoadClick`, use a classe [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) para carregar um único arquivo na memória do seu aplicativo.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## Use o 3D Builder para converter em Formato de manufatura 3D (.3mf)

Neste momento, você pode carregar um arquivo de dados 3D na memória do seu aplicativo. Entretanto, dados de geometria 3D vêm em muitos formatos diferentes, e nem todos são eficientes para impressão 3D. O Windows 10 usa o tipo de arquivo no Formato de manufatura 3D (.3mf) para todas as tarefas de impressão 3D.

> 
            **Observação**  O tipo de arquivo 3MF oferece uma grande quantidade de funcionalidades não abordadas neste tutorial. Para saber mais sobre 3MF e os recursos que ele fornece produtores e consumidores de produtos 3D, consulte o [3MF especificação](http://3mf.io/what-is-3mf/3mf-specification/). Para saber como aproveitar esses recursos usando as APIs do Windows 10, consulte o tutorial [Gerar um pacote 3MF](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf).

Felizmente, o aplicativo [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) pode abrir arquivos nos formatos 3D mais populares e salvá-los como arquivos .3mf. Neste exemplo, onde o tipo de arquivo pode variar, uma solução muito simples é abrir o 3D Builder e solicitar que o usuário salve os dados importados como um arquivo .3mf e recarregue-o.

> 
            **Observação**  Além de converter formatos de arquivo, o **3D Builder** oferece ferramentas simples para editar seus modelos, adicionar dados de cor e realizar outras operações específicas de impressão; portanto, costuma valer a pena integrá-lo a um aplicativo que lida com impressão 3D.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## Repare os dados do modelo de impressão 3D

Nem todos os dados de modelo 3D podem ser impressos, mesmo no formato .3mf. Para a impressora determinar corretamente o espaço de preenchimento e o que deixar em branco, o modelo a ser impresso deve ser um único padrão consistente, ter normais de superfície voltados para o exterior e ter uma geometria variada. Problemas nessas áreas podem surgir de várias formas diferentes e ser difíceis de se perceber em formatos complexos. Felizmente, soluções de software atuais costumam ser adequadas para converter geometria crua em formatos 3D imprimíveis. Isso será feito no método `OnFixClick`.

O arquivo de dados 3D deve ser convertido para implementar o [IRandomAccessStream](https://msdn.microsoft.com/library/windows/apps/br241731), que pode ser usado para gerar um objeto [Printing3DModel](https://msdn.microsoft.com/library/windows/apps/mt203679).

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

O objeto **Printing3DModel** agora está reparado e imprimível. Use **SaveModelToPackageAsync** para atribuir o modelo para o objeto Printing3D3MFPackage que você declarou ao criar a classe.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## Executar a tarefa de impressão: crie um manipulador de TaskRequested


Depois, quando a caixa de diálogo de impressão 3D for exibida para o usuário e o usuário optar por iniciar a impressão, o seu aplicativo precisará passar os parâmetros desejados para o pipeline de impressão 3D. A API de impressão 3D acionará o evento **TaskRequested**. Você deve escrever um método para manipular esse evento adequadamente. Como sempre, ele deve ser do mesmo tipo do evento: o evento **TaskRequested** tem parâmetros [Print3DManager](https://msdn.microsoft.com/library/windows/apps/dn998029) (seu objeto remetente) e um objeto [Print3DTaskRequestedEventArgs](https://msdn.microsoft.com/library/windows/apps/dn998051), que contém a maior parte das informações relevantes. Seu tipo de retorno é **void**.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

O objetivo principal desse método é usar o objeto *args* para enviar um **Printing3D3MFPackage** para o pipeline. O tipo **Print3DTaskRequestedEventArgs** tem uma propriedade: **Request**. Ela é do tipo [Print3DTaskRequest](https://msdn.microsoft.com/library/windows/apps/dn998050) e representa uma solicitação de trabalho de impressão. Seu método **CreateTask** permite que o programa envie as informações certas para o trabalho de impressão, e retorna uma referência para o objeto [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) que é enviado para o pipeline de impressão 3D.


            O **CreateTask** tem os seguintes parâmetros de entrada: uma **cadeia de caracteres** para o nome do trabalho de impressão, uma **cadeia de caracteres** para a ID da impressora usar e uma delegação **Print3DTaskSourceRequestedHandler**. A delegação é automaticamente invocada quando o evento **3DTaskSourceRequested** é acionado (isso é feito pela API em si). O importante a observar é que tal delegação é invocada quando um trabalho de impressão é iniciado, e é responsável por fornecer o pacote de impressão 3D certo.


            O **Print3DTaskSourceRequestedHandler** usa um parâmetro, um objeto [Print3DTaskSourceRequestedArgs](https://msdn.microsoft.com/library/windows/apps/dn998056) que fornece os dados a serem enviados. O único método público dessa classe, **SetSource**, aceita o pacote a ser impresso. Implemente uma delegação **Print3DTaskSourceRequestedHandler** da seguinte maneira:

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

Em seguida, chame o **CreateTask**, usando a delegação definida recentemente, `sourceHandler`:

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

O **Print3DTask** devolvido é atribuído à variável de classe declarada no início. Agora você pode usar (opcionalmente) essa referência para lidar com determinados eventos lançados pela tarefa:

[!code-cs[Opcional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> 
            **Observação**  Você deve implementar um método `Task_Submitting` e `Task_Completed` se quiser registrá-los nesses eventos.

## Execute a tarefa de impressão: abrir a caixa de diálogo de impressão 3D


A parte final do código necessária para imprimir a partir do seu aplicativo é a que inicia a caixa de diálogo de impressão 3D. Como uma janela de caixa de diálogo de impressão convencional, a caixa de diálogo de impressão 3D fornece um número de especificações de impressão de última hora e permite que o usuário escolha qual impressora usar (se conectado via USB ou rede).

Primeiro, registre seu método `MyTaskRequested` com o evento **TaskRequested**.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

Depois de registrar seu manipulador de eventos **TaskRequested**, você pode chamar o método **ShowPrintUIAsync**, que abre a caixa de diálogo de impressão 3D na janela do aplicativo atual.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Por fim, é uma boa prática registrar os manipuladores de eventos quando seu aplicativo retoma o controle: [!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## Tópicos relacionados

[Gerar um pacote 3MF](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)

[Exemplo UWP de impressão 3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 







<!--HONumber=Jun16_HO4-->



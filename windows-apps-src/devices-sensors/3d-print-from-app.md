---
title: Impressão 3D a partir de seu aplicativo
description: Saiba como adicionar a funcionalidade de impressão 3D ao seu aplicativo Universal do Windows. Esse tópico aborda como iniciar a caixa de diálogo de impressão 3D após verificar se o seu modelo 3D é imprimível e está no formato correto.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 3dprinting, impressão 3d
ms.localizationpriority: medium
ms.openlocfilehash: e2ed99720afdccef297d46853d4a2445b497195e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321693"
---
# <a name="3d-printing-from-your-app"></a>Impressão 3D a partir de seu aplicativo

**APIs importantes**

-   [**Windows.Graphics.Printing3D**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D)

Saiba como adicionar a funcionalidade de impressão 3D ao seu aplicativo Universal do Windows. Este tópico aborda como carregar dados de geometria 3D em seu aplicativo e iniciar a caixa de diálogo de impressão 3D após assegurar que seu modelo 3D é imprimível e está no formato correto. Para obter um exemplo de trabalho desses procedimentos, consulte o [Exemplo UWP de impressão 3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

> [!NOTE]
> No código de exemplo deste guia, os relatórios e o tratamento de erros estão bastante simplificados por questões de simplicidade.

## <a name="setup"></a>Configuração


Na classe de aplicativo que terá a funcionalidade de impressão 3D, insira o namespace [**Windows.Graphics.Printing3D**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D).

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

Os seguintes namespaces adicionais serão usados nesta guia:

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Em seguida, dê à sua classe campos de membro úteis. Declare um objeto [**Print3DTask**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTask) para representar a tarefa de impressão que deve ser passada para o driver de impressão. Declare um objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) para armazenar o arquivo de dados 3D original que será carregado no aplicativo. Declare um objeto [**Printing3D3MFPackage**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Printing3D3MFPackage) que representa um modelo 3D pronto para imprimir com todos os metadados necessários.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>Crie uma interface do usuário simples

Este exemplo apresenta três controles de usuário: um botão de carregamento, que levará um arquivo para a memória de programa, um botão de correção, que modificará o arquivo conforme necessário e um botão de impressão, que iniciará o trabalho de impressão. O código a seguir cria esses botões (com seus manipuladores de eventos ao clicar) no arquivo XAML correspondente da sua classe .cs:

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Insira um **TextBlock** para feedback de interface do usuário.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>Obtenha os dados 3D


O método pelo qual o seu aplicativo obtém dados de geometria 3D varia. Seu aplicativo pode recuperar dados de uma varredura 3D, baixar dados de modelo de um recurso da web ou gerar uma malha 3D de forma programática usando fórmulas matemáticas ou entrada do usuário. Por questões de simplicidade, este guia mostrará como carregar um arquivo de dados 3D (de qualquer um dos vários tipos de arquivo comuns) em memória de programa do armazenamento do dispositivo. A [biblioteca de modelos 3D Builder](https://developer.microsoft.com/windows/hardware/3d-print/windows-3d-printing) fornece uma variedade de modelos que você pode baixar facilmente em seu dispositivo.

Em seu método `OnLoadClick`, use a classe [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para carregar um único arquivo na memória do seu aplicativo.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>Use o 3D Builder para converter em Formato de manufatura 3D (.3mf)

Neste momento, você pode carregar um arquivo de dados 3D na memória do seu aplicativo. Entretanto, dados de geometria 3D pode vir em muitos formatos diferentes, e nem todos são eficientes para impressão 3D. O Windows 10 usa o tipo de arquivo no Formato de manufatura 3D (.3mf) para todas as tarefas de impressão 3D.

> [!NOTE]  
> O tipo de arquivo .3mf oferece mais funcionalidades que são abordadas neste tutorial. Para saber mais sobre 3MF e os recursos que ele fornece produtores e consumidores de produtos 3D, consulte o [3MF especificação](https://3mf.io/what-is-3mf/3mf-specification/). Para saber como utilizar esses recursos com as APIs do Windows 10, consulte o tutorial [Gerar um pacote 3MF](https://docs.microsoft.com/windows/uwp/devices-sensors/generate-3mf).

O aplicativo [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) pode abrir arquivos nos formatos 3D mais populares e salvá-los como arquivos .3mf. Neste exemplo, onde o tipo de arquivo pode variar, uma solução muito simples é abrir o aplicativo 3D Builder e solicitar que o usuário salve os dados importados como um arquivo .3mf e recarregue-o.

> [!NOTE]  
> Além de converter formatos de arquivo, o 3D Builder oferece ferramentas simples para editar seus modelos, adicionar dados de cor e realizar outras operações específicas de impressão; portanto, costuma valer a pena integrá-lo a um aplicativo que lida com impressão 3D.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>Repare os dados do modelo de impressão 3D

Nem todos os dados de modelo 3D podem ser impressos, mesmo no tipo de .3mf. Para a impressora determinar corretamente o espaço de preenchimento e o que deixar em branco, o modelo a ser impresso deve ser uma única malha consistente, ter normais de superfície voltados para o exterior e ter uma geometria variada. Problemas nessas áreas podem surgir de várias formas diferentes e ser difíceis de se perceber em formatos complexos. No entanto, soluções de software modernas costumam ser adequadas para converter geometria crua em formatos 3D imprimíveis. Isso é conhecido como *reparar* o modelo e será feito no método `OnFixClick`.

O arquivo de dados 3D deve ser convertido para implementar o [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream), que pode ser usado para gerar um objeto [**Printing3DModel**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Printing3DModel).

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

O objeto **Printing3DModel** agora está reparado e imprimível. Use [**SaveModelToPackageAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) para atribuir o modelo para o objeto **Printing3D3MFPackage** que você declarou ao criar a classe.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>Executar a tarefa de impressão: crie um manipulador de TaskRequested


Depois, quando a caixa de diálogo de impressão 3D for exibida para o usuário e o usuário optar por iniciar a impressão, o seu aplicativo precisará passar os parâmetros desejados para o pipeline de impressão 3D. A API de impressão 3D acionará o evento **[TaskRequested](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** . Você deve escrever um método para manipular esse evento adequadamente. Como sempre, o método do manipulador deve ser do mesmo tipo que o evento: O **TaskRequested** evento tem parâmetros [ **Print3DManager** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager) (uma referência para seu objeto de remetente) e um [  **Print3DTaskRequestedEventArgs** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequestedEventArgs) objeto, que contém a maioria das informações relevantes.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

O objetivo principal desse método é usar o parâmetro *args* para enviar um **Printing3D3MFPackage** para o pipeline. O **Print3DTaskRequestedEventArgs** tipo tem uma propriedade: [**Solicitar**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtaskrequestedeventargs.request). Ela é do tipo [**Print3DTaskRequest**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequest) e representa uma solicitação de trabalho de impressão. Seu método [**CreateTask**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtaskrequest.createtask) permite que o programa envie as informações corretas para o trabalho de impressão, e retorna uma referência para o objeto **Print3DTask** que foi enviado para o pipeline de impressão 3D.

O **CreateTask** tem os seguintes parâmetros de entrada: uma cadeia de caracteres para o nome do trabalho de impressão, uma cadeia de caracteres para a ID da impressora usar e uma delegação [**Print3DTaskSourceRequestedHandler**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedhandler). A delegação é automaticamente invocada quando o evento **3DTaskSourceRequested** é acionado (isso é feito pela API em si). O importante a observar é que tal delegação é invocada quando um trabalho de impressão é iniciado, e é responsável por fornecer o pacote de impressão 3D certo.

O **Print3DTaskSourceRequestedHandler** usa um parâmetro, um objeto [**Print3DTaskSourceRequestedArgs**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskSourceRequestedArgs) que fornece os dados a serem enviados. O único método público dessa classe, [**SetSource**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource), aceita o pacote a ser impresso. Implemente uma delegação **Print3DTaskSourceRequestedHandler** da seguinte maneira.

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

Em seguida, chame o **CreateTask**, usando a delegação definida recentemente, `sourceHandler`:

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

O **Print3DTask** devolvido é atribuído à variável de classe declarada no início. Agora você pode usar (opcionalmente) essa referência para lidar com determinados eventos lançados pela tarefa.

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> Você deve implementar um método `Task_Submitting` e `Task_Completed` se quiser registrá-los nesses eventos.

## <a name="execute-printing-task-open-3d-print-dialog"></a>Execute a tarefa de impressão: abrir a caixa de diálogo de impressão 3D


A parte final do código necessário é a que faz com que a caixa de diálogo de impressão 3D seja exibida. Como uma janela de caixa de diálogo de impressão convencional, a caixa de diálogo de impressão 3D fornece uma série de opções de impressão de última hora e permite que o usuário escolha qual impressora usar (se conectado por USB ou rede).

Registre seu método `MyTaskRequested` com o evento **TaskRequested**.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

Depois de registrar seu manipulador de eventos **TaskRequested**, você pode chamar o método [**ShowPrintUIAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dmanager.showprintuiasync), que abre a caixa de diálogo de impressão 3D na janela do aplicativo atual.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Por fim, é uma boa prática registrar os manipuladores de eventos quando seu aplicativo retoma o controle.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>Tópicos relacionados

[Gerar um pacote 3MF](https://docs.microsoft.com/windows/uwp/devices-sensors/generate-3mf)  
[Exemplo UWP de impressão 3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 

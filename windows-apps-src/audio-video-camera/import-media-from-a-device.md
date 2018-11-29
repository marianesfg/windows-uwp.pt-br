---
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: Este artigo descreve como importar mídia de um dispositivo, inclusive procurar fontes de mídia disponíveis, importar arquivos, como fotos e arquivos secundários, e excluir os arquivos importados do dispositivo de origem.
title: Importar mídia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c08612e48eec7989f3b56fba41a17e1c149b2058
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8196852"
---
# <a name="import-media-from-a-device"></a>Importar mídia de um dispositivo

Este artigo descreve como importar mídia de um dispositivo, inclusive procurar fontes de mídia disponíveis, importar arquivos, como vídeos, fotos e arquivos secundários, e excluir os arquivos importados do dispositivo de origem.

> [!NOTE] 
> O código neste artigo foi adaptado da [**amostra de aplicativo UWP MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport). Você pode clonar ou baixar essa amostra do [**repositório Git de amostras de aplicativo Universal do Windows**](https://github.com/Microsoft/Windows-universal-samples) para ver o código no contexto ou usá-la como ponto de partida para seu próprio aplicativo.

## <a name="create-a-simple-media-import-ui"></a>Criar uma interface do usuário de importação de mídia simples
O exemplo neste artigo usa uma interface do usuário mínima para habilitar os principais cenários de importação de mídia. Para ver como criar uma interface do usuário mais robusta para um aplicativo de importação de mídia, veja a amostra [**amostra MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport). O XAML a seguir cria um painel de pilha com os seguintes controles:
* Um controle [**Button**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Button) para começar a procurar fontes de onde a mídia possa ser importada.
* Um controle [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) para listar e selecionar as fontes de importação de mídia encontradas.
* Um controle [**ListView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListView) para exibir e selecionar os itens de mídia da fonte de importação selecionada.
* Um controle **Button** para começar a importar itens de mídia da fonte selecionada.
* Um controle **Button** para começar a excluir os itens que foram importados da fonte selecionada.
* Um controle **Button** para cancelar a operação de importação de mídia assíncrona.

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## <a name="set-up-your-code-behind-file"></a>Configurar seu arquivo code-behind
Adicione diretivas *using* para incluir os namespaces usados por este exemplo que ainda não estão incluídos no modelo de projeto padrão.

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="set-up-task-cancellation-for-media-import-operations"></a>Configurar o cancelamento de tarefas para operações de importação de mídia

Como as operações de importação de mídia podem levar muito tempo, elas são executadas assincronamente usando [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx). Declare uma variável de membro de classe do tipo [**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource) que será usada para cancelar uma operação em andamento se o usuário clicar no botão Cancelar.

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

Implemente um manipulador para o botão Cancelar. Os exemplos mostrados mais adiante neste artigo inicializam o **CancellationTokenSource** quando uma operação começa e os definem como nulos quando a operação é concluída. No manipulador do botão Cancelar, verifique se o token é nulo e, se não for, chame [**Cancel**](https://msdn.microsoft.com/library/dd321955) para cancelar a operação.

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## <a name="data-binding-helper-classes"></a>Classes auxiliares de vinculação de dados

Em um cenário de importação de mídia comum, você mostre ao usuário uma lista de itens de mídia disponíveis para importação, pode haver um grande número de arquivos de mídia para escolher e, em geral, convém mostrar uma miniatura de cada item de mídia. Por esse motivo, este exemplo usa três classes auxiliares para carregar entradas incrementalmente no controle ListView conforme o usuário rola a lista para baixo.

* Classe **IncrementalLoadingBase** – Implementa [**IList**](https://msdn.microsoft.com/library/system.collections.ilist), [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading) e [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged(v=vs.105).aspx) para fornecer o comportamento base de carregamento incremental.
* Classe **GeneratorIncrementalLoadingClass** – Fornece uma implementação da classe base de carregamento incremental.
* Classe **ImportableItemWrapper** – Um wrapper fino ao redor da classe [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) para adicionar uma propriedade [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.BitmapImage) associável à imagem de miniatura para cada item importado.

Essas classes são fornecidas na [**amostra MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) e podem ser adicionadas ao seu projeto sem modificações. Depois de adicionar as classes auxiliares ao seu projeto, declare uma variável de membro de classe do tipo **GeneratorIncrementalLoadingClass** que será usada mais tarde neste exemplo.

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


## <a name="find-available-sources-from-which-media-can-be-imported"></a>Localizar fontes disponíveis das quais mídias podem ser importadas

No manipulador de cliques do botão Localizar fontes, chame o método estático [**PhotoImportManager.FindAllSourcesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportManager.FindAllSourcesAsync) para começar a procurar no sistema por dispositivos dos quais mídias possam ser importadas. Depois de aguardar a conclusão da operação, percorra cada objeto [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) na lista retornada de objeto e adicione uma entrada a **ComboBox**, definindo a propriedade **Tag** ao objeto de origem em si, para que ela possa ser facilmente recuperado quando o usuário fizer uma seleção.

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

Declare uma variável de membro de classe para armazenar a fonte de importação selecionada do usuário.

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

No manipulador [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) para **ComboBox** da fonte de importação, defina a variável de membro de classe para a fonte selecionada e, em seguida, chame o método auxiliar **FindItems** que será mostrado neste artigo. 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

## <a name="find-items-to-import"></a>Localizar itens para importação

Adicione as variáveis de membro de classe do tipo [**PhotoImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSession) e [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) que serão usadas nas próximas etapas.

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

No método **FindItems**, inicialize a variável **CancellationTokenSource** para que ela possa ser usada para cancelar a operação de localização, se necessário. Em um bloco **try**, crie uma nova sessão de importação chamando [**CreateImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource.CreateImportSession) no objeto [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) selecionado pelo usuário. Crie um novo objeto [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) para fornecer um retorno de chamada para exibir o progresso da operação de localização. Em seguida, chame **[FindItemsAsync](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportsession.finditemsasync)** para iniciar a operação de localização. Forneça um valor [**PhotoImportContentTypeFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportContentTypeFilter) para especificar se fotos, vídeos ou ambos devem ser retornados. Forneça um valor [**PhotoImportItemSelectionMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItemSelectionMode) para especificar se todos, nenhum ou apenas os novos itens de mídia são retornados com a propriedade [**IsSelected**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem.IsSelected) definida como true. Essa propriedade está associada a uma caixa de seleção de cada item de mídia em nosso modelo de item ListBox.

**FindItemsAsync** retorna [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx). O método de extensão [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) é usado para criar uma tarefa que pode ser aguardada, pode ser cancelada com o token de cancelamento e que relata o progresso usando o objeto **Progress** fornecido.

Depois, a classe auxiliar de vinculação de dados **GeneratorIncrementalLoadingClass** é inicializada. **FindItemsAsync**, quando retorna da espera, retorna um objeto [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult). Esse objeto contém informações de status sobre a operação de localização, incluindo o sucesso da operação e o número de diferentes tipos de itens de mídia que foram encontrados. A propriedade [**FoundItems**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.FoundItems) contém uma lista de objetos [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) que representam os itens de mídia encontrados. O construtor **GeneratorIncrementalLoadingClass** assume como argumentos o número total de itens que serão carregados de forma incremental e uma função que gera novos itens a serem carregados conforme necessário. Nesse caso, a expressão lambda fornecida cria uma nova instância de **ImportableItemWrapper** que ajusta **PhotoImportItem** e inclui uma miniatura para cada item. Depois que a classe de carregamento incremental for inicializada, defina-a como a propriedade [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl.ItemsSource) do controle **ListView** da interface do usuário. Agora, os itens de mídia encontrados serão carregados de forma incremental e exibidos na lista.

Em seguida, as informações de status da operação de localização serão geradas. Um aplicativo comum exibirá essas informações na interface do usuário, mas este exemplo simplesmente gera as informações no console de depuração. Por fim, defina o token de cancelamento como null porque a operação foi concluída.

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## <a name="import-media-items"></a>Importar itens de mídia

Antes de implementar a operação de importação, declare um objeto [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) para armazenar os resultados da operação de importação. Isso será usado mais tarde para excluir itens de mídia que foram importados com sucesso da fonte.

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

Antes de iniciar a operação de importação de mídia, inicialize a variável **CancellationTokenSource** e defina o valor do controle [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ProgressBar) como 0.

Se não houver itens selecionados no controle **ListView**, não há nada para importar. Caso contrário, inicialize um objeto [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) para fornecer um retorno de chamada de progresso que atualiza o valor do controle da barra de progresso. Registre um manipulador para o evento [**ItemImported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ItemImported) do [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) retornado pela operação de localização. Esse evento é gerado sempre que um item é importado e, neste exemplo, gera o nome de cada arquivo importado no console de depuração.

Chame [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) para iniciar a operação de importação. Assim como na operação de localização, o método de extensão [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) é usado para converter a operação retornada em uma tarefa que possa ser aguardada, informe o progresso e possa ser cancelada.

Depois que a operação de importação for concluída, o status da operação poderá ser obtido do objeto [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) retornado por [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync). Este exemplo gera as informações de status no console de depuração e, por fim, define o token de cancelamento como null.

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## <a name="delete-imported-items"></a>Excluir itens importados
Para excluir os itens importados com sucesso da fonte de onde foram importados, primeiro inicialize o token de cancelamento para que a operação de exclusão possa ser cancelada e defina o valor da barra de progresso como 0. Certifique-se de que o [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) retornado de [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) não seja null. Se não for, crie mais uma vez um objeto [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) para fornecer um retorno de chamada de progresso para a operação de exclusão. Chame [**DeleteImportedItemsFromSourceAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult.DeleteImportedItemsFromSourceAsync) para iniciar a exclusão dos itens importados. Use **AsTask** para converter o resultado em uma tarefa que possa ser aguardada com recursos de progresso e cancelamento. Depois de aguardar, o objeto [**PhotoImportDeleteImportedItemsFromSourceResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) retornado pode ser usado para obter e exibir informações de status sobre a operação de exclusão.

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








 



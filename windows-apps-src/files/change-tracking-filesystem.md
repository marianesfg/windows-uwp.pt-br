---
title: Controlar alterações no sistema de arquivos em segundo plano
description: Descreve como controlar alterações em arquivos e pastas em segundo plano conforme os usuários os movimentam pelo sistema.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1cef2fb660681d3e382eb8ca7dcb92456756f627
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685231"
---
# <a name="track-file-system-changes-in-the-background"></a>Controlar alterações no sistema de arquivos em segundo plano

**APIs importantes**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

A classe [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) permite que os aplicativos controlem alterações em arquivos e pastas conforme os usuários os movimentam pelo sistema. Usando a classe **StorageLibraryChangeTracker**, um aplicativo pode acompanhar:

- Operações de arquivo, inclusive adicionar, excluir e modificar.
- Operações de pasta, como renomeações e exclusões.
- Movimentação de arquivos e pastas na unidade.

Use este guia para conhecer o modelo de programação para trabalhar com o controlador de alterações, ver alguns exemplos de código e entender os diferentes tipos de operações de arquivo que são controladas pelo **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** funciona para bibliotecas do usuário ou para qualquer pasta no computador local. Isso inclui unidades secundárias ou unidades removíveis, mas não inclui unidades NAS ou unidades de rede.

## <a name="using-the-change-tracker"></a>Usando o controlador de alterações

O controlador de alterações é implementado no sistema como um buffer circular armazenando as últimas *N* operações de sistema de arquivos. Os aplicativos são capazes de ler as alterações do buffer e, em seguida, processá-los em suas próprias experiências. Após terminar as alterações, o aplicativo as marca como processadas e nunca mais as verá.

Para usar o controlador de alterações em uma pasta, siga estas etapas:

1. Habilitar o controle de alterações para a pasta.
2. Aguardar as alterações.
3. Ler as alterações.
4. Aceitar as alterações.

As seções a seguir percorrem cada uma das etapas com alguns exemplos de código. O exemplo de código completo é fornecido no final do artigo.

### <a name="enable-the-change-tracker"></a>Habilitar o controlador de alterações

A primeira coisa que o aplicativo precisa fazer é informar ao sistema que ele interessado em uma determinada biblioteca de controle de alterações. Ele faz isso chamando o método [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) no controlador de alterações para a biblioteca de interesse.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Algumas observações importantes:

- Verifique se seu aplicativo tem permissão para a biblioteca correta no manifesto antes de criar o objeto [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary). Para obter mais detalhes, consulte [Permissões de acesso a arquivo](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).
- [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) é thread safe, não redefine seu ponteiro e pode ser chamado quantas vezes você quiser (haverá mais informações sobre isso posteriormente).

![Habilitando um controlador de alterações vazio](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Aguardar as alterações

Após o controlador de alterações ser inicializado, ele começará a registrar todas as operações que ocorrerem dentro de uma biblioteca, mesmo enquanto o aplicativo não estiver em execução. Os aplicativos podem ser registrados para ser ativados a qualquer momento em que houver uma alteração fazendo o registro no evento [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger).

![Alterações sendo adicionadas ao controlador de alterações sem o aplicativo lê-los](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Ler as alterações

O aplicativo pode, então, sondar alterações do controlador de alterações e receber uma lista das alterações ocorridas desde a última vez que ele fez a verificação. O código abaixo mostra como obter uma lista de alterações do controlador de alterações.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

O aplicativo, então, é responsável por processar as alterações em sua própria experiência ou banco de dados, conforme necessário.

![Lendo as alterações do controlador de alterações em um banco de dados do aplicativo](images/changetracker-reading.png)

> [!TIP]
> A segunda chamada para Enable tem a finalidade de proteger contra uma condição de corrida se o usuário adicionar outra pasta à biblioteca enquanto seu aplicativo estiver lendo as alterações. Sem a chamada extra para **Enable**, o código falhará com ecSearchFolderScopeViolation (0x80070490) se o usuário estiver mudando as pastas em sua biblioteca

### <a name="accept-the-changes"></a>Aceitar as alterações

Após o aplicativo terminar de processar as alterações, ele deverá instruir o sistema a nunca mostrar essas alterações novamente chamando o método [**AcceptChangesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync).

```csharp
await changeReader.AcceptChangesAsync();
```

![Marcar alterações como lidas para que elas não sejam mostradas novamente](images/changetracker-accepting.png)

Agora, o aplicativo receberá apenas novas alterações ao ler o controlador de alterações no futuro.

- Se as alterações tiverem ocorrido entre as chamadas para [**ReadBatchAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) e [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), o ponteiro será adiantado apenas até a alteração mais recente que o aplicativo viu. Essas outras alterações ainda estarão disponíveis na próxima vez em que ele chamar **ReadBatchAsync**.
- Não aceitar as alterações fará com que o sistema retorne o mesmo conjunto de alterações na próxima vez que o aplicativo chamar **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Itens importantes a lembrar

Ao usar o controlador de alterações, há algumas coisas que você deve ter em mente para certificar-se de que tudo está funcionando corretamente.

### <a name="buffer-overruns"></a>Saturações de buffer

Embora tentemos reservar espaço suficiente no controlador de alterações para manter todas as operações acontecendo no sistema até que seu aplicativo possa lê-los, é muito fácil imaginar um cenário em que o aplicativo não lê as alterações antes que o buffer circular substitua a si mesmo. Especialmente se o usuário estiver restaurando dados de um backup ou sincronizando uma grande coleção de fotos com a câmera do celular.

Nesse caso, **ReadBatchAsync** retornará o código de erro [**StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Se seu aplicativo receber esse código de erro, isso significará duas coisas:

* O buffer substituiu a si mesmo desde a última vez em que você observou. O melhor curso de ação é rastrear novamente a biblioteca, porque as informações no controlador estarão incompletas.
* O controlador de alterações não retornará mais alterações até que você chame [**Reset**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Após a redefinição das chamadas do aplicativo, o ponteiro será movido para a alteração mais recente e o controle será retomado normalmente.

Deve ser raro se deparar com esses casos, mas em cenários em que o usuário está movendo um grande número de arquivos em seu disco, não queremos que o controlador de alterações expanda e ocupe muito espaço de armazenamento. Isso deve permitir que aplicativos reajam a grandes operações de sistema de arquivos ao mesmo tempo em que não prejudicam a experiência do cliente no Windows.

### <a name="changes-to-a-storagelibrary"></a>Alterações em uma StorageLibrary

A classe [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) existe como um grupo virtual de pastas raiz que contêm outras pastas. Para conciliar isso com um controlador de alterações do sistema de arquivos, fizemos as seguintes opções:

- Qualquer alteração em descendentes das pastas raiz da biblioteca será representada no controlador de alterações. As pastas de biblioteca de raiz podem ser encontradas usando a propriedade [**Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders).
- Adicionar ou remover pastas raiz de uma **StorageLibrary** (por meio de [**RequestAddFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) e [**RequestRemoveFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) não criará uma entrada no controlador de alterações. Essas alterações podem ser controladas por meio do evento [**DefinitionChanged**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) ou enumerando as pastas raiz na biblioteca usando a propriedade [**Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders).
- Se uma pasta com conteúdo for adicionada à biblioteca, não será gerada uma notificação de alteração ou entrada no controlador de alterações. Todas as alterações posteriores dos descendentes dessa pasta gerarão notificações e entradas do controlador de alterações.

### <a name="calling-the-enable-method"></a>Chamando o método Enable

Os aplicativos devem chamar [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) assim que iniciarem o acompanhamento do sistema de arquivos e antes de cada enumeração das alterações. Isso garantirá que todas as alterações serão capturadas pelo controlador de alterações.  

## <a name="putting-it-together"></a>Juntando tudo

Este é todo o código usado para registrar para receber as alterações da biblioteca de vídeos começar a extrair as alterações do controlador de alterações.

```csharp
private async void EnableChangeTracker()
{
    StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
    videoTracker.Enable();
}

private async void GetChanges()
{
    StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    videosLibrary.ChangeTracker.Enable();
    StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
    IReadOnlyList changeSet = await changeReader.ReadBatchAsync();


    //Below this line is for the blog post. Above the line is for the magazine
    foreach (StorageLibraryChange change in changeSet)
    {
        if (change.ChangeType == StorageLibraryChangeType.ChangeTrackingLost)
        {
            //We are in trouble. Nothing else is going to be valid.
            log("Resetting the change tracker");
            videosLibrary.ChangeTracker.Reset();
            return;
        }
        if (change.IsOfType(StorageItemTypes.Folder))
        {
            await HandleFileChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.File))
        {
            await HandleFolderChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.None))
        {
            if (change.ChangeType == StorageLibraryChangeType.Deleted)
            {
                RemoveItemFromDB(change.Path);
            }
        }
    }
    await changeReader.AcceptChangesAsync();
}
```

---
title: Controlar alterações no sistema de arquivos em segundo plano
description: Descreve como controlar alterações em arquivos e pastas em segundo plano como usuários movem-los ao redor do sistema.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621571"
---
# <a name="track-file-system-changes-in-the-background"></a>Controlar alterações no sistema de arquivos em segundo plano

**APIs importantes**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

O [ **StorageLibraryChangeTracker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) classe permite que os aplicativos rastrear alterações em arquivos e pastas como usuários movem-los ao redor do sistema. Usando o **StorageLibraryChangeTracker** classe, um aplicativo pode acompanhar:

- As operações, inclusive adicionar de arquivo, excluir, modificar.
- Operações de pasta como renomeações e exclusões.
- Arquivos e pastas, mover na unidade.

Use este guia para aprender o modelo de programação para trabalhar com o rastreador de alteração, exibir alguns exemplos de código e entender os diferentes tipos de operações de arquivo que são controladas pelo **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** funciona para bibliotecas do usuário, ou para qualquer pasta no computador local. Isso inclui unidades secundárias ou em unidades removíveis, mas não inclui NAS unidades ou unidades de rede.

## <a name="using-the-change-tracker"></a>Usando o rastreador de alteração

O rastreador de alteração é implementado no sistema como um buffer circular armazenando a última *N* operações de sistema de arquivos. Aplicativos são capazes de ler as alterações do buffer e, em seguida, processá-los em suas próprias experiências. Depois que o aplicativo foi concluído com as alterações, ele marca as alterações como processado e nunca vê-los novamente.

Para usar o rastreador de alteração em uma pasta, siga estas etapas:

1. Habilite o controle de alterações para a pasta.
2. Aguarde até que as alterações.
3. Ler as alterações.
4. Aceite as alterações.

As seções a seguir percorrer cada uma das etapas com alguns exemplos de código. O exemplo de código completo é fornecido no final do artigo.

### <a name="enable-the-change-tracker"></a>Habilitar o rastreador de alteração

A primeira coisa que o aplicativo precisa fazer é informar ao sistema que ele está interessado em uma determinada biblioteca de controle de alterações. Ele faz isso chamando o [ **habilitar** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) método no controlador de alterações para a biblioteca de interesse.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Algumas observações importantes:

- Verifique se seu aplicativo tem permissão para a biblioteca correta no manifesto do antes de criar o [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) objeto. Ver [permissões de acesso de arquivo](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions) para obter mais detalhes.
- [**Habilitar** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) é thread-safe, não redefinirão o ponteiro do mouse e podem ser chamados quantas vezes desejar (mais sobre isso posteriormente).

![Habilitando um rastreador de alterações vazio](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Aguarde até que as alterações

Depois que o rastreador de alteração é inicializado, ele começará a registrar todas as operações que ocorrem dentro de uma biblioteca, mesmo enquanto o aplicativo não está em execução. Aplicativos podem se registrar para ser ativada a qualquer momento, há uma alteração ao se registrar para o [ **StorageLibraryChangedTrigger** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) eventos.

![Alterações que está sendo adicionadas ao controlador de alterações sem o aplicativo lê-los](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Leitura das alterações

O aplicativo pode sondar alterações do rastreador de alterações e receber uma lista das alterações desde a última vez que ele check-in. O código a seguir mostra como obter uma lista das alterações de rastreador de alterações.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

O aplicativo, em seguida, é responsável por processar as alterações em sua própria experiência ou um banco de dados conforme necessário.

![Ler as alterações no Rastreador de alterações em um banco de dados do aplicativo](images/changetracker-reading.png)

> [!TIP]
> É a segunda chamada para habilitar para proteção contra uma condição de corrida se o usuário adiciona à biblioteca de outra pasta enquanto seu aplicativo está lendo as alterações. Sem a chamada extra **habilitar** o código falhará com ecSearchFolderScopeViolation (0x80070490) se o usuário está mudando as pastas em sua biblioteca

### <a name="accept-the-changes"></a>Aceitar as alterações

Depois que o aplicativo é feito as alterações de processamento, ela deve informar o sistema nunca mostrará essas alterações novamente chamando o [ **AcceptChangesAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) método.

```csharp
await changeReader.AcceptChangesAsync();
```

![Marcar as alterações como leitura, portanto, eles nunca serão mostrados novamente](images/changetracker-accepting.png)

O aplicativo agora receberá apenas novas alterações ao ler o rastreador de alteração no futuro.

- Se as alterações ocorreram entre chamar [ **ReadBatchAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) e [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), o ponteiro será apenas ser adiantada até a alteração mais recente que o aplicativo tenha visto. Essas outras alterações ainda estará disponíveis na próxima vez que ele chama **ReadBatchAsync**.
- Não aceitar as alterações fará com que o sistema retornar o mesmo conjunto de alterações a próxima hora de chamadas do aplicativo **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Coisas importantes para lembrar

Ao usar o rastreador de alteração, há algumas coisas que você deve ter em mente para certificar-se de que tudo está funcionando corretamente.

### <a name="buffer-overruns"></a>Saturações de buffer

Embora tentemos reservar espaço suficiente no Rastreador de alterações para manter todas as operações acontecendo no sistema até que seu aplicativo pode lê-los, é muito fácil imaginar um cenário em que o aplicativo não lê as alterações antes que o buffer circular substitui em si. Especialmente se o usuário está restaurando os dados de um backup ou sincronizar uma grande coleção de fotos com a câmera do celular.

Nesse caso, **ReadBatchAsync** retornará o código de erro [ **StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Se seu aplicativo recebe esse código de erro, isso significará duas coisas:

* O buffer tem substituídos em si desde a última vez em que você observou. O melhor curso de ação é rastreie a biblioteca, porque todas as informações do rastreador será incompletas.
* O rastreador de alteração não retornará mais alterações até que você chame [ **redefinir**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Depois que o aplicativo chama redefinição, o ponteiro será movido para a alteração mais recente e o rastreamento será retomado normalmente.

Ele deve ser raro para obter esses casos, mas em cenários em que o usuário está movendo um grande número de arquivos em torno de seu disco não queremos que o rastreador de alteração para aumentar e ocupar muito espaço de armazenamento. Isso deve permitir que aplicativos reagir a operações de sistema de arquivos grandes, ao mesmo tempo em que não danifiquem a experiência do cliente no Windows.

### <a name="changes-to-a-storagelibrary"></a>Alterações para um StorageLibrary

O [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) classe existe como um grupo virtual das pastas raiz que contêm outras pastas. Para reconciliar isso com um controlador de alterações do sistema de arquivo, fizemos as seguintes opções:

- Qualquer alteração descendente das pastas raiz de biblioteca será representada no Rastreador de alteração. As pastas de biblioteca de raiz podem ser encontradas usando o [ **pastas** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) propriedade.
- Adicionar ou remover pastas raiz de um **StorageLibrary** (por meio de [ **RequestAddFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) e [ **RequestRemoveFolderAsync**  ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) não criará uma entrada no Rastreador de alteração. Essas alterações podem ser controladas por meio de [ **DefinitionChanged** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) eventos ou enumerando as pastas raiz na biblioteca usando o [ **pastas** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) propriedade.
- Se uma pasta com o conteúdo já está em que ele é adicionada à biblioteca, não haverá uma alteração de notificação ou alterar o controlador entradas geradas. Todas as alterações subsequentes para os descendentes de nessa pasta serão gerar notificações e entradas de rastreador de alteração.

### <a name="calling-the-enable-method"></a>Chamar o método Enable

Os aplicativos devem chamar [ **habilitar** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) assim que iniciarem o sistema de arquivos de rastreamento e antes de cada enumeração das alterações. Isso garantirá que todas as alterações serão capturadas pelo controlador de alterações.  

## <a name="putting-it-together"></a>Juntando as peças

Aqui está todo o código que é usado para registrar as alterações da biblioteca de vídeos e iniciar a extrair as alterações do rastreador de alterações.

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

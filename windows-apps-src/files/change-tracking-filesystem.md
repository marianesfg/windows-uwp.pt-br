---
title: Controlar alterações de sistema de arquivo em segundo plano
description: Descreve como controlar as alterações em arquivos e pastas em segundo plano à medida que os usuários movem-los ao redor do sistema.
ms.date: 11/20/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e90692753924572a932767b9c188ed6d24f94593
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8459749"
---
# <a name="track-file-system-changes-in-the-background"></a>Controlar alterações de sistema de arquivo em segundo plano

A classe [StorageLibraryChangeTracker](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) permite que os aplicativos rastrear alterações em arquivos e pastas que os usuários movem-los ao redor do sistema. Usando a classe **StorageLibraryChangeTracker** , um aplicativo pode controlar:

- Incluindo adicionar as operações de arquivo, excluir, modificar.
- Operações de pasta como renomeia e exclusões.
- Arquivos e pastas movendo na unidade.

Use este guia para saber o modelo de programação para trabalhar com o controlador de alteração, exibir alguns códigos de exemplo e entender os diferentes tipos de operações de arquivo que são controlados por **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** funciona para bibliotecas do usuário ou para qualquer pasta no computador local. Isso inclui unidades secundárias ou em unidades removíveis, mas não inclui NAS unidades ou unidades de rede.

## <a name="using-the-change-tracker"></a>Usando o rastreador de alteração

O rastreador de alteração é implementado no sistema como um buffer circular armazenando as operações de sistema de arquivo *N* últimos. Os aplicativos são capazes de ler as alterações fora o buffer e, em seguida, processá-los em seus próprios experiências. Depois que o aplicativo for concluído com as alterações, ela marca as alterações como processado e nunca vê-los novamente.

Para usar o rastreador de alteração em uma pasta, siga estas etapas:

1. Habilite o rastreamento de alterações para a pasta.
2. Aguarde até que as alterações.
3. Leia as alterações.
4. Aceite alterações.

As próximas seções percorrer cada uma das etapas com alguns exemplos de código. O exemplo de código completo é fornecido no final do artigo.

### <a name="enable-the-change-tracker"></a>Habilitar o rastreador de alteração

A primeira coisa que o aplicativo precisa fazer é informar ao sistema que ele está interessado em uma determinada biblioteca de controle de alterações. Ele faz isso chamando o método [Habilitar](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) sobre o rastreador de alteração para a biblioteca de interesse.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Algumas observações importantes:

- Verifique se que seu aplicativo tem permissão para a biblioteca correta no manifesto do antes de criar o objeto [StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) . Consulte [Permissões de acesso de arquivo](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions) para obter mais detalhes.
- [Habilitar](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) é thread-safe, não vai restaurar o ponteiro e podem ser chamados quantas vezes desejar (mais sobre isso mais tarde).

![Habilitar um rastreador de alteração vazia](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Aguarde até que as alterações

Depois que o rastreador de alteração é inicializado, ele começará a gravar todas as operações que ocorrem dentro de uma biblioteca, mesmo enquanto o aplicativo não estiver em execução. Aplicativos podem se registrar para ser ativada sempre que houver uma alteração ao registrar o evento [StorageLibraryChangedTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) .

![Alterações que está sendo adicionadas à alteração rastreador sem que o aplicativo lê-los](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Leia as alterações

O aplicativo pode pesquisar alterações do controlador alteração e receber uma lista das alterações desde a última vez em que ele verificado. O código a seguir mostra como obter uma lista das alterações do controlador alteração.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

O aplicativo, em seguida, é responsável por processar as alterações em sua própria experiência ou banco de dados conforme necessário.

![As alterações de leitura do controlador alteração em um banco de dados de aplicativo](images/changetracker-reading.png)

> [!TIP]
> A segunda chamada para habilitar é a proteger contra uma condição de corrida se o usuário adiciona outra pasta para a biblioteca enquanto seu aplicativo está lendo as alterações. Sem a chamada extra para **Habilitar** o código falhará com ecSearchFolderScopeViolation (0x80070490) se o usuário está mudando as pastas em sua biblioteca

### <a name="accept-the-changes"></a>Aceitar as alterações

Depois que o aplicativo é feito processar as alterações, ele deve dizer ao sistema para nunca mostrar essas alterações novamente, chamando o método [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) .

```csharp
await changeReader.AcceptChangesAsync();
```

![Marcando alterações como lidas para que eles nunca serão mostrados novamente](images/changetracker-accepting.png)

O aplicativo agora receberá apenas novas alterações ao ler o rastreador de alteração no futuro.

- Se alterações tem acontecido entre chamada [ReadBatchAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) e [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), o ponteiro será apenas ser avançados para a alteração mais recente que o aplicativo tenha visto. Essas outras alterações ainda estará disponíveis na próxima vez que ele chama **ReadBatchAsync**.
- As alterações não está aceitando fará com que o sistema retornar o mesmo conjunto de alterações na próxima vez em que o aplicativo chama **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Itens importantes a serem lembrados

Ao usar o rastreador de alteração, há algumas coisas que você deve ter em mente para certificar-se de que tudo está funcionando corretamente.

### <a name="buffer-overruns"></a>Saturações de buffer

Embora tentamos reservar espaço suficiente no controlador alteração para manter todas as operações acontecendo no sistema até que seu aplicativo pode lê-los, é muito fácil imaginar um cenário em que o aplicativo não lê as alterações antes que o buffer circular substitui em si. Principalmente se o usuário está restaurando dados de um backup ou sincronizar uma grande coleção de imagens do telefone da câmera.

Nesse caso, **ReadBatchAsync** retornará o código de erro [StorageLibraryChangeType.ChangeTrackingLost](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Se seu aplicativo recebe esse código de erro, isso significa que algumas coisas:

* O buffer substituiu em si desde a última vez em que você viu. O melhor curso de ação é rastreie a biblioteca, porque qualquer informação do controlador será incompleta.
* O controlador de alteração não retornará quaisquer alterações mais até que você chame [Redefinir](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Depois que o aplicativo chama redefinição, o ponteiro será movido para a alteração mais recente e rastreamento será retomado normalmente.

Ele deve ser raro para obter nesses casos, mas em cenários em que o usuário está se movendo um grande número de arquivos em torno de seu disco não queremos rastreador alteração dimensionados e ocupam muito armazenamento. Isso deve permitir que aplicativos reagir a operações de sistema de arquivos enorme enquanto não danifiquem a experiência do cliente no Windows.

### <a name="changes-to-a-storagelibrary"></a>Alterações em um StorageLibrary

A classe [StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) existe como um grupo virtual de pastas de raiz que contêm outras pastas. Para reconciliar isso com um controlador de alteração de sistema de arquivo, fizemos as seguintes opções:

- As alterações para descendente das pastas de biblioteca de raiz serão representadas no controlador alteração. As pastas de biblioteca de raiz podem ser encontradas usando a propriedade de [pastas](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) .
- Adicionar ou remover pastas raiz de um **StorageLibrary** (por meio de [RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) e [RequestRemoveFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) não criará uma entrada no controlador alteração. Essas alterações podem ser rastreadas por meio do evento [DefinitionChanged](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) ou enumerando as pastas raiz na biblioteca usando a propriedade de [pastas](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) .
- Se uma pasta com o conteúdo já nele é adicionada à biblioteca, não haverá uma alteração de notificação ou alterar entradas de rastreador geradas. As alterações subsequentes para os descendentes dessa pasta irá gerar notificações e alterar as entradas do controlador.

### <a name="calling-the-enable-method"></a>Chamar o método de ativação

Aplicativos devem chamar [Habilitar](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) assim que iniciarem o sistema de arquivos de rastreamento e antes de cada enumeração das alterações. Isso garante que todas as alterações serão capturadas pelo controlador de alteração.  

## <a name="putting-it-together"></a>Juntando

Aqui está todo o código que é usado para se registrar para as alterações da biblioteca de vídeo e iniciar extrair as alterações do controlador alteração.

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

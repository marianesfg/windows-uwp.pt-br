---
Description: Saiba como armazenar e recuperar dados de aplicativos locais, móveis e temporários.
title: Armazenar e recuperar configurações e outros dados de aplicativo
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0eb7ef49d0ce1876635dc36e84f43432c13e1791
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690364"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>Armazenar e recuperar configurações e outros dados de aplicativo

*Os dados de aplicativo* são dados mutáveis que são criados e gerenciados por um aplicativo específico. Ele inclui o estado do tempo de execução, as configurações do aplicativo, as preferências do usuário, o conteúdo de referência (como as definições de dicionário em um aplicativo de dicionário) e outras configurações. Os dados do aplicativo são diferentes dos *dados do usuário*, os dados que o usuário cria e gerencia ao usar um aplicativo. Os dados do usuário incluem arquivos de documento ou mídia, transcrições de email ou de comunicação ou registros de banco de dados que contêm conteúdo criado pelo usuário. Os dados do usuário podem ser úteis ou significativos para mais de um aplicativo. Geralmente, esses são dados que o usuário deseja manipular ou transmitir como uma entidade independente do aplicativo em si, como um documento.

**Observação importante sobre os dados do aplicativo:** O tempo de vida dos dados do aplicativo está vinculado ao tempo de vida do aplicativo. Se o aplicativo for removido, todos os dados do aplicativo serão perdidos como consequência. Não use dados de aplicativo para armazenar dados de usuário ou qualquer coisa que os usuários possam perceber como valiosos e insubstituíveis. Recomendamos que as bibliotecas do usuário e o Microsoft OneDrive sejam usados para armazenar esse tipo de informação. Os dados do aplicativo são ideais para armazenar preferências de usuário específicas do aplicativo, configurações e favoritos.

## <a name="types-of-app-data"></a>Tipos de dados de aplicativo

Há dois tipos de dados de aplicativo: configurações e arquivos.

### <a name="settings"></a>Configurações

Use as configurações para armazenar as informações de preferências do usuário e estado do aplicativo. A API de dados de aplicativo permite que você crie e recupere configurações facilmente (mostraremos alguns exemplos posteriormente neste artigo).

Aqui estão os tipos de dados que você pode usar para as configurações do aplicativo:

- **Uint8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **single**e **Double**
- **Booliano**
- **Char16**, **cadeia de caracteres**
- [**DateTime**](/uwp/api/Windows.Foundation.DateTime), [ **TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan)
    - Para C#/.net, use: [**System. DateTimeOffset**](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0), [**System. TimeSpan**](/dotnet/api/system.timespan?view=dotnet-uwp-10.0)
- **GUID**, [**ponto**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**tamanho**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size), [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect)
- [**ApplicationDataCompositeValue**](/uwp/api/Windows.Storage.ApplicationDataCompositeValue): um conjunto de configurações de aplicativo relacionadas que devem ser serializadas e desserializadas atomicamente. Use as configurações de composição para manipular facilmente as atualizações atômicas de configurações interdependentes. O sistema garante a integridade das configurações de composição durante o acesso e o roaming simultâneos. As configurações de composição são otimizadas para pequenas quantidades de dados e o desempenho pode ser ruim se você usá-las para grandes conjuntos de dados.

### <a name="files"></a>Arquivos

Use arquivos para armazenar dados binários ou para habilitar seus próprios tipos serializados personalizados.

## <a name="storing-app-data-in-the-app-data-stores"></a>Armazenando dados de aplicativo nos armazenamentos de dados de aplicativo


Quando um aplicativo é instalado, o sistema fornece a ele seus armazenamentos de dados por usuário para configurações e arquivos. Você não precisa saber onde ou como esses dados existem, pois o sistema é responsável por gerenciar o armazenamento físico, garantindo que os dados sejam mantidos isolados de outros aplicativos e outros usuários. O sistema também preserva o conteúdo desses armazenamentos de dados quando o usuário instala uma atualização em seu aplicativo e remove o conteúdo desses armazenamentos de dados de forma completa e limpa quando seu aplicativo é desinstalado.

Em seu armazenamento de dados de aplicativo, cada aplicativo tem diretórios raiz definidos pelo sistema: um para arquivos locais, um para arquivos de roaming e outro para arquivos temporários. Seu aplicativo pode adicionar novos arquivos e novos contêineres a cada um desses diretórios raiz.

## <a name="local-app-data"></a>Dados do aplicativo local


Os dados do aplicativo local devem ser usados para qualquer informação que precise ser preservada entre as sessões do aplicativo e não seja adequada para dados de aplicativo de roaming. Os dados que não são aplicáveis em outros dispositivos também devem ser armazenados aqui. Não há nenhuma restrição de tamanho geral nos dados locais armazenados. Use o armazenamento de dados do aplicativo local para dados que não faz sentido para o roaming e para grandes conjuntos de dados.

### <a name="retrieve-the-local-app-data-store"></a>Recuperar o armazenamento de dados do aplicativo local

Antes de ler ou gravar dados do aplicativo local, você deve recuperar o armazenamento de dados do aplicativo local. Para recuperar o armazenamento de dados do aplicativo local, use a propriedade [**applicationData. LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) para obter as configurações locais do aplicativo como um objeto [**ApplicationDataContainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer) . Use a propriedade [**applicationData. LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) para obter os arquivos em um objeto [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) . Use a propriedade [**applicationData. LocalCacheFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder) para obter a pasta no repositório de dados do aplicativo local, onde você pode salvar os arquivos que não estão incluídos no backup e na restauração.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>Criar e recuperar uma configuração local simples

Para criar ou gravar uma configuração, use a propriedade [**ApplicationDataContainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) para acessar as configurações no contêiner de `localSettings` que obtemos na etapa anterior. Este exemplo cria uma configuração chamada `exampleSetting`.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

Para recuperar a configuração, use a mesma propriedade [**ApplicationDataContainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) usada para criar a configuração. Este exemplo mostra como recuperar a configuração que acabamos de criar.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>Criar e recuperar um valor composto local

Para criar ou gravar um valor composto, crie um objeto [**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) . Este exemplo cria uma configuração composta chamada `exampleCompositeSetting` e a adiciona ao contêiner `localSettings`.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

Este exemplo mostra como recuperar o valor composto que acabamos de criar.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)localSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-read-a-local-file"></a>Criar e ler um arquivo local

Para criar e atualizar um arquivo no armazenamento de dados do aplicativo local, use as APIs de arquivo, como [**Windows. Storage. StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) e [**Windows. Storage. FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). Este exemplo cria um arquivo chamado `dataFile.txt` no contêiner de `localFolder` e grava a data e hora atuais no arquivo. O valor de **ReplaceExisting** da enumeração [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) indica substituir o arquivo, caso ele já exista.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Para abrir e ler um arquivo no armazenamento de dados do aplicativo local, use as APIs de arquivo, como [**Windows. Storage. StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)e [**Windows. Storage. FileIO. ReadTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). Este exemplo abre o arquivo `dataFile.txt` criado na etapa anterior e lê a data do arquivo. Para obter detalhes sobre como carregar recursos de arquivo de vários locais, consulte [como carregar recursos de arquivo](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await localFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="roaming-data"></a>Dados de roaming


Se você usa dados de roaming em seu aplicativo, os usuários podem facilmente manter os dados de aplicativo de seu aplicativo sincronizados entre vários dispositivos. Se um usuário instalar seu aplicativo em vários dispositivos, o sistema operacional manterá os dados do aplicativo em sincronia, reduzindo a quantidade de trabalho de configuração que o usuário precisa fazer para seu aplicativo em seu segundo dispositivo. O roaming também permite que os usuários continuem em uma tarefa, como compor uma lista, certo onde foram deixados de fora em um dispositivo diferente. O sistema operacional replica dados de roaming para a nuvem quando ele é atualizado e sincroniza os dados para os outros dispositivos nos quais o aplicativo está instalado.

O sistema operacional limita o tamanho dos dados de aplicativo que cada aplicativo pode fazer roaming. Consulte [**applicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota). Se o aplicativo atingir esse limite, nenhum dos dados do aplicativo do aplicativo será replicado para a nuvem até que os dados do aplicativo móvel total do aplicativo sejam menores que o limite novamente. Por esse motivo, é uma prática recomendada usar dados de roaming somente para preferências do usuário, links e arquivos de dados pequenos.

Os dados de roaming de um aplicativo estão disponíveis na nuvem, desde que sejam acessados pelo usuário de algum dispositivo dentro do intervalo de tempo necessário. Se o usuário não executar um aplicativo por mais tempo do que esse intervalo de tempo, seus dados de roaming serão removidos da nuvem. Se um usuário desinstalar um aplicativo, seus dados de roaming não serão removidos automaticamente da nuvem, ele será preservado. Se o usuário reinstalar o aplicativo dentro do intervalo de tempo, os dados de roaming serão sincronizados da nuvem.

### <a name="roaming-data-dos-and-donts"></a>Dados de roaming são e não

- Use roaming para preferências do usuário e personalizações, links e arquivos de dados pequenos. Por exemplo, use roaming para preservar a preferência de cor de plano de fundo de um usuário em todos os dispositivos.
- Use o roaming para permitir que os usuários continuem uma tarefa entre os dispositivos. Por exemplo, dados de aplicativo de roaming como o conteúdo de um email rascunho ou a página exibida mais recentemente em um aplicativo leitor.
- Manipule o evento [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged) atualizando os dados do aplicativo. Esse evento ocorre quando os dados do aplicativo acabaram de sincronizar a partir da nuvem.
- Referências de roaming para conteúdo em vez de dados brutos. Por exemplo, faça roaming de uma URL em vez do conteúdo de um artigo online.
- Para configurações importantes, de tempo crítico, use a configuração *HighPriority* associada a [**RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings).
- Não faça roam de dados de aplicativo que são específicos para um dispositivo. Algumas informações são pertinentes apenas localmente, como um nome de caminho para um recurso de arquivo local. Se você decidir usar o roaming de informações locais, verifique se o aplicativo pode ser recuperado se as informações não forem válidas no dispositivo secundário.
- Não faça roam de grandes conjuntos de dados do aplicativo. Há um limite para a quantidade de dados de aplicativo que um aplicativo pode fazer roaming; Use a propriedade [**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) para obter esse máximo. Se um aplicativo atingir esse limite, nenhum dado poderá ser movido até que o tamanho do armazenamento de dados do aplicativo não exceda mais o limite. Ao projetar seu aplicativo, considere como colocar um limite em dados maiores para não exceder o limite. Por exemplo, se salvar um estado de jogo exigir 10 KB cada, o aplicativo só poderá permitir que o usuário armazene até 10 jogos.
- Não use roaming para dados que dependam da sincronização instantânea. O Windows não garante uma sincronização instantânea; o roaming pode ser significativamente retardado se um usuário estiver offline ou em uma rede de alta latência. Verifique se a interface do usuário não depende da sincronização instantânea.
- Não use roaming para dados que mudam frequentemente. Por exemplo, se o aplicativo acompanhar informações que mudam frequentemente, como a posição em uma música por segundo, não armazene isso como dados de aplicativo de roaming. Em vez disso, escolha uma representação menos frequente que ainda forneça uma boa experiência do usuário, como a música em execução no momento.

### <a name="roaming-pre-requisites"></a>Pré-requisitos de roaming

Qualquer usuário poderá se beneficiar de dados de aplicativos móveis se usarem um conta Microsoft para fazer logon em seu dispositivo. No entanto, os usuários e os administradores de diretiva de grupo podem desativar dados de aplicativos móveis em um dispositivo a qualquer momento. Se um usuário optar por não usar um conta Microsoft ou desabilitar os recursos de dados de roaming, ele ainda poderá usar seu aplicativo, mas os dados do aplicativo serão locais para cada dispositivo.

Os dados armazenados no [**PasswordVault**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.PasswordVault) só serão transferidos se um usuário tiver tornado um dispositivo "confiável". Se um dispositivo não for confiável, os dados protegidos nesse cofre não serão movidos.

### <a name="conflict-resolution"></a>Resolução de conflitos

Os dados de aplicativo de roaming não se destinam ao uso simultâneo em mais de um dispositivo por vez. Se ocorrer um conflito durante a sincronização porque uma unidade de dados específica foi alterada em dois dispositivos, o sistema sempre favorecerá o valor que foi gravado por último. Isso garante que o aplicativo utilize as informações mais atualizadas. Se a unidade de dados for uma configuração composta, a resolução de conflitos ainda ocorrerá no nível da unidade de configuração, o que significa que a composição com a alteração mais recente será sincronizada.

### <a name="when-to-write-data"></a>Quando gravar dados

Dependendo do tempo de vida esperado da configuração, os dados devem ser gravados em momentos diferentes. Dados de aplicativo de alteração frequente ou lenta devem ser gravados imediatamente. No entanto, os dados do aplicativo que são alterados com frequência só devem ser gravados periodicamente em intervalos regulares (como uma vez a cada 5 minutos), bem como quando o aplicativo é suspenso. Por exemplo, um aplicativo de música pode gravar as configurações de "música atual" sempre que uma nova música começar a reproduzir, no entanto, a posição real na música só deve ser gravada na suspensão.

### <a name="excessive-usage-protection"></a>Proteção de uso excessivo

O sistema tem vários mecanismos de proteção em vigor para evitar o uso inadequado de recursos. Se os dados do aplicativo não fizerem a transição conforme o esperado, é provável que o dispositivo tenha sido temporariamente restrito. Esperar por algum tempo geralmente resolverá essa situação automaticamente e nenhuma ação será necessária.

### <a name="versioning"></a>Controle de versão

Os dados do aplicativo podem utilizar o controle de versão para atualizar de uma estrutura de dados para outra. O número de versão é diferente da versão do aplicativo e pode ser definido em vai. Embora não seja imposta, é altamente recomendável que você use números de versão crescentes, já que complicações indesejáveis (incluindo perda de dados) podem ocorrer se você tentar fazer a transição para um número de versão de dados mais baixo que represente dados mais recentes.

Os dados do aplicativo só são movimentados entre os aplicativos instalados com o mesmo número de versão. Por exemplo, os dispositivos na versão 2 farão a transição dos dados entre si e os dispositivos na versão 3 farão o mesmo, mas nenhum roaming ocorrerá entre um dispositivo executando a versão 2 e um dispositivo executando a versão 3. Se você instalar um novo aplicativo que utilize vários números de versão em outros dispositivos, o aplicativo recém-instalado sincronizará os dados de aplicativo associados ao número de versão mais alto.

### <a name="testing-and-tools"></a>Testes e ferramentas

Os desenvolvedores podem bloquear seus dispositivos a fim de disparar uma sincronização de dados de aplicativo de roaming. Se parecer que os dados do aplicativo não fazem a transição em um determinado intervalo de tempo, verifique os seguintes itens e certifique-se de que:

- Os dados de roaming não excedem o tamanho máximo (consulte [**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) para obter detalhes).
- Seus arquivos são fechados e liberados corretamente.
- Há pelo menos dois dispositivos executando a mesma versão do aplicativo.


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>Registre-se para receber notificações quando os dados de roaming forem alterados

Para usar dados de aplicativo de roaming, você precisa se registrar para alterações de dados de roaming e recuperar os contêineres de dados de roaming para que você possa ler e gravar configurações.

1.  Registre-se para receber notificações quando os dados em roaming forem alterados.

    O evento [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged) notifica você quando os dados em roaming são alterados. Este exemplo define `DataChangeHandler` como o manipulador para alterações de dados de roaming.

```csharp
void InitHandlers()
    {
       Windows.Storage.ApplicationData.Current.DataChanged += 
          new TypedEventHandler<ApplicationData, object>(DataChangeHandler);
    }

    void DataChangeHandler(Windows.Storage.ApplicationData appData, object o)
    {
       // TODO: Refresh your data
    }
```

2.  Obtenha os contêineres para as configurações e os arquivos do aplicativo.

    Use a propriedade [**applicationData. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) para obter as configurações e a propriedade [**applicationData. RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder) para obter os arquivos.

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>Criar e recuperar configurações de roaming

Use a propriedade [**ApplicationDataContainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) para acessar as configurações no contêiner `roamingSettings` que obtemos na seção anterior. Este exemplo cria uma configuração simples chamada `exampleSetting` e um valor composto chamado `composite`.

```csharp
// Simple setting

roamingSettings.Values["exampleSetting"] = "Hello World";
// High Priority setting, for example, last page position in book reader app
roamingSettings.values["HighPriority"] = "65";

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

roamingSettings.Values["exampleCompositeSetting"] = composite;

```

Este exemplo recupera as configurações que acabamos de criar.

```csharp
// Simple setting

Object value = roamingSettings.Values["exampleSetting"];

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)roamingSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-retrieve-roaming-files"></a>Criar e recuperar arquivos móveis

Para criar e atualizar um arquivo no armazenamento de dados de aplicativo de roaming, use as APIs de arquivo, como [**Windows. Storage. StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) e [**Windows. Storage. FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). Este exemplo cria um arquivo chamado `dataFile.txt` no contêiner de `roamingFolder` e grava a data e hora atuais no arquivo. O valor de **ReplaceExisting** da enumeração [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) indica substituir o arquivo, caso ele já exista.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Para abrir e ler um arquivo no armazenamento de dados de aplicativo de roaming, use as APIs de arquivo, como [**Windows. Storage. StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)e [ **Windows. Storage. FileIO. ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). Este exemplo abre o arquivo `dataFile.txt` criado na seção anterior e lê a data do arquivo. Para obter detalhes sobre como carregar recursos de arquivo de vários locais, consulte [como carregar recursos de arquivo](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await roamingFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```


## <a name="temporary-app-data"></a>Dados temporários do aplicativo


O armazenamento de dados de aplicativo temporário funciona como um cache. Seus arquivos não são movidos e podem ser removidos a qualquer momento. A tarefa de manutenção do sistema pode excluir automaticamente os dados armazenados neste local a qualquer momento. O usuário também pode limpar arquivos do armazenamento de dados temporário usando a limpeza de disco. Os dados temporários do aplicativo podem ser usados para armazenar informações temporárias durante uma sessão de aplicativo. Não há nenhuma garantia de que esses dados persistirão além do final da sessão do aplicativo, pois o sistema pode recuperar o espaço usado, se necessário. O local está disponível por meio da propriedade [**temporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) .

### <a name="retrieve-the-temporary-data-container"></a>Recuperar o contêiner de dados temporários

Use a propriedade [**applicationData. TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) para obter os arquivos. As próximas etapas usam a variável `temporaryFolder` desta etapa.

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>Criar e ler arquivos temporários

Para criar e atualizar um arquivo no armazenamento de dados de aplicativo temporário, use as APIs de arquivo, como [**Windows. Storage. StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) e [**Windows. Storage. FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). Este exemplo cria um arquivo chamado `dataFile.txt` no contêiner de `temporaryFolder` e grava a data e hora atuais no arquivo. O valor de **ReplaceExisting** da enumeração [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) indica substituir o arquivo, caso ele já exista.


```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Para abrir e ler um arquivo no armazenamento de dados de aplicativo temporário, use as APIs de arquivo, como [**Windows. Storage. StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)e [ **Windows. Storage. FileIO. ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). Este exemplo abre o arquivo `dataFile.txt` criado na etapa anterior e lê a data do arquivo. Para obter detalhes sobre como carregar recursos de arquivo de vários locais, consulte [como carregar recursos de arquivo](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await temporaryFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="organize-app-data-with-containers"></a>Organizar dados de aplicativo com contêineres


Para ajudá-lo a organizar suas configurações de dados de aplicativo e arquivos, crie contêineres (representados por objetos [**ApplicationDataContainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer) ) em vez de trabalhar diretamente com diretórios. Você pode adicionar contêineres aos armazenamentos de dados de aplicativos locais, móveis e temporários. Os contêineres podem ser aninhados até 32 níveis de profundidade.

Para criar um contêiner de configurações, chame o método [**ApplicationDataContainer. CreateContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.createcontainer) . Este exemplo cria um contêiner de configurações local chamado `exampleContainer` e adiciona uma configuração chamada `exampleSetting`. O valor **sempre** da enumeração [**ApplicationDataCreateDisposition**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCreateDisposition) indica que o contêiner será criado se ele ainda não existir.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Setting in a container
Windows.Storage.ApplicationDataContainer container = 
   localSettings.CreateContainer("exampleContainer", Windows.Storage.ApplicationDataCreateDisposition.Always);

if (localSettings.Containers.ContainsKey("exampleContainer"))
{
   localSettings.Containers["exampleContainer"].Values["exampleSetting"] = "Hello Windows";
}
```

## <a name="delete-app-settings-and-containers"></a>Excluir configurações e contêineres do aplicativo


Para excluir uma configuração simples que seu aplicativo não precisa mais, use o método [**ApplicationDataContainerSettings. Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainersettings.remove) . Este exemplo deletesthe `exampleSetting` configuração local que criamos anteriormente.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

Para excluir uma configuração composta, use o método [**ApplicationDataCompositeValue. Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacompositevalue.remove) . Este exemplo exclui a configuração de composição de `exampleCompositeSetting` local que criamos em um exemplo anterior.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

Para excluir um contêiner, chame o método [**ApplicationDataContainer. DeleteContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.deletecontainer) . Este exemplo exclui o contêiner de configurações de `exampleContainer` local que criamos anteriormente.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>Controle de versão dos dados do seu aplicativo


Você pode, opcionalmente, obter a versão dos dados do aplicativo para seu aplicativo. Isso permitirá que você crie uma versão futura do seu aplicativo que altere o formato de seus dados de aplicativo sem causar problemas de compatibilidade com a versão anterior do seu aplicativo. O aplicativo verifica a versão dos dados do aplicativo no armazenamento de dados e, se a versão for menor que a versão esperada pelo aplicativo, o aplicativo deverá atualizar os dados do aplicativo para o novo formato e atualizar a versão. Para obter mais informações, consulte a propriedade[**Application. Version**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.version) e o método [**applicationData. SetVersionAsync**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.setversionasync) .

## <a name="related-articles"></a>Artigos relacionados

* [**Windows. Storage. ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData)
* [**Windows. Storage. ApplicationData. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)
* [**Windows. Storage. ApplicationData. RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)
* [**Windows. Storage. ApplicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)
* [**Windows. Storage. ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)

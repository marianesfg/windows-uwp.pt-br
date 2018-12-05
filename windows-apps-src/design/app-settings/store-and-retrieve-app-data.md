---
Description: Learn how to store and retrieve local, roaming, and temporary app data.
title: Armazenar e recuperar configurações e outros dados de app
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a5e3a29a252b091b1e52dbea5fa7af5058488ed5
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8689382"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>Armazenar e recuperar configurações e outros dados de aplicativo

*Dados de aplicativo* são dados mutáveis específicos de determinado aplicativo. Eles incluem estado de tempo de execução, preferências de usuário e outras configurações. Dados de aplicativo são diferentes de *dados do usuário*, dados que o usuário cria e gerencia ao usar um aplicativo. Os dados do usuário incluem arquivos de documentos ou mídia, emails, transcrições de comunicações ou registros de bancos de dados com conteúdo criado pelo usuário. Os dados do usuário podem ser úteis ou significativos para mais de um aplicativo. Geralmente, trata-se de dados que o usuário quer manipular ou transmitir como uma entidade independente do próprio aplicativo, como um documento.

**Observação importante sobre os dados de aplicativo:** O tempo de vida dos dados de aplicativo está vinculado ao tempo de vida do aplicativo. Se o aplicativo for removido, como consequência todos os dados do aplicativo serão perdidos. Não use dados do aplicativo para armazenar dados do usuário ou qualquer coisa que os usuários possam perceber como valioso e insubstituível. Recomendamos que as bibliotecas do usuário e o Microsoft OneDrive sejam usados para armazenar esse tipo de informações. Os dados do Aplicativo são ideais para armazenar as preferências, as configurações e os favoritos do usuário específicos ao aplicativo.

## <a name="types-of-app-data"></a>Tipos de dados de aplicativo


Existem dois tipos de dados de aplicativo: configurações e arquivos.

-   **Configurações**

    Use configurações para armazenar as preferências do usuário e informações de estado do aplicativo. A API de dados de aplicativo permite que você crie e recupere facilmente configurações (mostraremos alguns exemplos neste artigo).

    Aqui estão os tipos de dados que você pode usar para as configurações do aplicativo:

    -   **UInt8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **Single**, **Double**
    -   **Booliano**
    -   **Char16**, **String**
    -   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576), [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996)
    -   **GUID**, [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995), [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)
    -   [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588): conjunto de configurações de aplicativo relacionados que devem ser serializados e desserializados atomicamente. Use configurações compostas para lidar com facilidade com atualizações atômicas ou configurações interdependentes. O sistema assegura a integridade das configurações compostas durante o acesso e o roaming. As configurações compostas são otimizadas para pequenas quantidades de dados e o desempenho pode ser ruim se você as utilizar para conjuntos grandes de dados.
-   **Arquivos**

    Use arquivos para armazenar dados binários ou para permitir seus tipos serializado próprios e personalizados.

## <a name="storing-app-data-in-the-app-data-stores"></a>Armazenando dados de aplicativo nos repositórios de dados do aplicativo


Quando um aplicativo é instalado, o sistema concede a ele seus próprios repositórios de dados por usuário para configurações e arquivos. Você não precisa saber onde ou como esses dados existem, porque o sistema é responsável por gerenciar o armazenamento físico, garantindo que os dados sejam mantidos isolados de outros aplicativos e outros usuários. O sistema também preserva o conteúdo desses repositórios de dados quando o usuário instala uma atualização de seu aplicativo e remove totalmente o conteúdo desses repositórios de dados quando o aplicativo é desinstalado.

Em seu repositório de dados de aplicativo, cada aplicativo possui diretórios-raiz definidos pelo sistema: um para arquivos locais, um para arquivos roaming e um para arquivos temporários. Seu aplicativo pode adicionar novos arquivos e novos contêineres a cada um desses diretórios raiz.

## <a name="local-app-data"></a>Dados de aplicativo local


Dados de aplicativo local devem ser utilizados para informações que precisam ser preservadas entre sessões de aplicativo e não são adequados para dados de aplicativo de roaming. Dados que não são aplicáveis a outros dispositivos devem ser armazenados aqui também. Não há restrição de tamanho geral para dados locais armazenados. Use o repositório de dados locais de aplicativo para dados que não precisam usar perfil móvel e para conjuntos de dados maiores.

### <a name="retrieve-the-local-app-data-store"></a>Recuperar o repositório de dados de aplicativo local

Antes de ler ou gravar dados de aplicativo local, você deve recuperar o repositório de dados de aplicativo local. Para recuperar o repositório de dados de aplicativo local, use a propriedade [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) para obter as configurações locais do aplicativo como um objeto [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599). Use a propriedade [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) para obter os arquivos de dentro de um objeto [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230). Use a propriedade [**ApplicationData.LocalCacheFolder**](https://msdn.microsoft.com/library/windows/apps/dn633825) para obter a pasta no repositório de dados do aplicativo local onde você pode salvar arquivos que não estão incluídos no backup e restauração.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>Criar e recuperar uma configuração de local simples

Para criar ou gravar uma configuração, use a propriedade [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) para acessar as configurações no contêiner `localSettings` que obtivemos na etapa anterior. Este exemplo cria uma configuração chamada `exampleSetting`.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

Para recuperar a configuração, você usa a mesma propriedade [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) que usou para criar a configuração. Este exemplo mostra como recuperar a configuração que acabamos de criar.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>Criar e recuperar um valor de composição local

Para criar ou gravar um valor composto, crie um objeto [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588). Este exemplo cria uma definição composta nomeada `exampleCompositeSetting` e a adiciona ao contêiner `localSettings`.

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

Para criar e atualizar um arquivo no repositório de dados de aplicativo local, use as APIs de arquivo, como [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) e [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505). Este exemplo cria um arquivo chamado `dataFile.txt` no contêiner `localFolder` e escreve a data e hora atual no arquivo. O valor **ReplaceExisting** da enumeração [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) indica para substituir o arquivo se ele já existir.

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

Para abrir e ler um arquivo no repositório local de dados de aplicativo, use as APIs de arquivo, como [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) e [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482). Este exemplo abre o arquivo `dataFile.txt` criado na etapa anterior e lê a data do arquivo. Para saber detalhes de como carregar recursos de arquivos de vários locais, veja [Como carregar recursos de arquivos](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

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

## <a name="roaming-data"></a>Roaming de dados


Se você usa dados roaming em seu aplicativo, seus usuários poderão manter com facilidade os dados de aplicativo do aplicativo em sincronia com vários dispositivos. Se um usuário instalar seu aplicativo em vários dispositivos, o SO mantém os dados do aplicativo sincronizados, reduzindo a quantidade de configurações que o usuário precisa realizar no aplicativo no segundo dispositivo. O roaming também permite que os usuários continuem uma tarefa, como a composição de uma lista, exatamente do ponto onde pararam, mesmo que em um dispositivo diferente. O SO replica os dados roaming para a nuvem quando eles são atualizados e sincroniza os dados com os outros dispositivos nos quais o aplicativo está instalado.

O SO limita o tamanho dos dados de aplicativo para roaming em cada aplicativo. Consulte [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625). Se o aplicativo chegar ao limite, nenhum de seus dados de aplicativo serão replicados para a nuvem até que todos os dados de aplicativo em roaming estejam abaixo do limite novamente. Por essa razão, a prática recomendada é usar dados roaming apenas para preferências do usuário, links e pequenos arquivos de dados.

Os dados de roaming de um aplicativo estarão disponíveis na nuvem desde que sejam acessados pelo usuário por meio de algum dispositivo dentro do intervalo de tempo necessário. Se o usuário não executar um aplicativo por mais tempo do que esse intervalo de tempo, os dados de roaming serão removidos da nuvem. Se um usuário desinstala um aplicativo, os dados de roaming não são removidos automaticamente da nuvem, eles são preservados. Se o usuário reinstalar o aplicativo dentro do intervalo de tempo, os dados de roaming serão sincronizados pela nuvem.

### <a name="roaming-data-dos-and-donts"></a>O que fazer e não fazer no roaming de dados

-   Use o roaming para preferências de usuário e personalizações, links e pequenos arquivos de dados. Por exemplo, use o roaming para preservar a preferência de cor de fundo de um usuário em todos os seus dispositivos.
-   Use o roaming para permitir que os usuários continuem uma tarefa entre dispositivos. Por exemplo, use roaming de dados do aplicativo como o conteúdo de um rascunho de email ou a página mais recentemente exibida em um aplicativo de leitura.
-   Manipule o evento [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) atualizando os dados do aplicativo. Esse evento ocorrerá quando os dados de aplicativos tiverem terminado de ser sincronizados da nuvem.
-   Use um perfil móvel para o conteúdo em vez de dados brutos. Por exemplo, faça roaming de uma URL em vez do conteúdo de um artigo online.
-   Para configurações importantes e urgentes, use a configuração *HighPriority* associada a [**RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624).
-   Não use roaming em dados de aplicativo específicos de um dispositivo. Algumas informações são pertinentes apenas localmente, como um nome de caminho para um recurso de arquivo local. Se você decidir usar um perfil móvel nas informações locais, certifique-se de que o aplicativo possa se recuperar se as informações não forem válidas no dispositivo secundário.
-   Não use perfis móveis em grandes conjuntos de dados de aplicativos. Há um limite na quantidade de dados à qual um aplicativo pode aplicar roaming; use a propriedade [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) para atingir essa quantidade máxima. Se um aplicativo atingir esse limite, o roaming não poderá ser usado em nenhum dado até que o tamanho do armazenamento de dados do aplicativo deixe de exceder o limite. Ao projetar seu aplicativo, considere como definir um limite para dados maiores, para que eles não ultrapassem o limite. Por exemplo, se para salvar o estado de um jogo for preciso 10 KB, o aplicativo deverá apenas permitir o armazenamento de até 10 jogos.
-   Não use o perfil móvel para dados que dependerem da sincronização instantânea. O Windows não garante uma sincronização instantânea; o uso de roaming poderá sofrer um atraso significativo se um usuário estiver offline em uma rede de alta latência. Assegure-se de que sua interface do usuário não dependa da sincronização instantânea.
-   Não use roaming para dados alterados com frequência. Por exemplo: se o aplicativo acompanhar informações que mudarem frequentemente, como a posição em um música por segundos, não armazene essas informações como dados de uso do perfil móvel do aplicativo. Em vez disso, escolha uma representação menos frequente que ainda represente uma boa experiência do usuário, como uma música que estiver tocando no momento.

### <a name="roaming-pre-requisites"></a>Pré-requisitos do uso do roaming

Qualquer usuário pode se beneficiar de roaming, desde que esteja usando uma conta da Microsoft para fazer logon no dispositivo. No entanto, usuários e administradores de política de grupo podem desligar o dados de uso do perfil móvel do aplicativo a qualquer momento. Se uma usuária escolher não usar uma conta da Microsoft ou desabilitar os recursos de roaming, ela ainda poderá usar seu aplicativo, mas os dados de aplicativo serão locais em cada dispositivo.

Os dados armazenados no [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) serão transferidos somente se o usuário tiver definido o dispositivo como "confiável". Se um dispositivo não for confiável, os dados armazenados nesse cofre não estarão disponíveis para roaming.

### <a name="conflict-resolution"></a>Resolução de conflitos

Dados de uso do perfil móvel do aplicativo não devem estar ativos simultaneamente em mais de um dispositivo por vez. Se houver um conflito durante a sincronização por causa de uma unidade de dados específica ter sido alterada em dois dispositivos, o sistema sempre favorecerá o valor que tiver sido gravado por último. Isso garantirá que o aplicativo utilize as informações mais atualizadas. Se a unidade de dados for uma composição de configuração, a resolução do conflito ainda ocorrerá no nível da unidade de configuração, o que significa que a composição com a última alteração será sincronizada.

### <a name="when-to-write-data"></a>Quando gravar dados

Dependendo do tempo de vida esperado da configuração, os dados deverão ser gravados em momentos diferentes. Os dados de aplicativo que mudarem com pouca frequência ou lentamente deverão ser gravados imediatamente. No entanto, os dados de aplicativo que forem modificados com frequência devem ser gravados apenas periodicamente, a intervalos regulares (como a cada 5 minutos), e também quando o aplicativo for suspenso. Por exemplo, um aplicativo de música poderá gravar as configurações da “música atual” sempre que uma nova música começar a tocar, mas a posição real na música deverá ser gravada apenas em suspensão.

### <a name="excessive-usage-protection"></a>Proteção excessiva da utilização

O sistema conta com vários mecanismos ativos de proteção para evitar o uso inapropriado de recursos. Se os dados de aplicativo não forem transmitidos como esperado, é provável que o dispositivo esteja temporariamente restrito. Normalmente, basta esperar alguns instantes para que a situação seja solucionada automaticamente, sem a necessidade de nenhuma ação do usuário.

### <a name="versioning"></a>Controle de versão

Os dados de aplicativo podem utilizar o controle de versão para atualizar de uma estrutura para outra. O número da versão é diferente da versão do aplicativo e pode ser definido conforme desejado. Ainda que não seja obrigatório, é altamente recomendável usar somente números de versão crescentes, pois a transferência para números de verão inferiores que representarem novos dados poderá resultar em complicações indesejadas (inclusive perda de dados).

Os dados de aplicativo só estarão disponíveis ao perfil móvel de aplicativos instalados com o mesmo número de versão. Por exemplo, os dispositivos com a versão 2 fazem a transferência de dados entre si, e os dispositivos com a versão 3 também, mas não há atividade do perfil móvel entre os dispositivos das versões 2 e 3. Se você instalar um novo aplicativo que tiver utilizado diversos números de versão em outros dispositivos, o aplicativo recém-instalado sincronizará os dados de aplicativo associados ao número de versão maior.

### <a name="testing-and-tools"></a>Testes e ferramentas

Os desenvolvedores podem bloquear o dispositivo para disparar uma sincronização de dados de aplicativo de roaming. Se não houver transferência de dados de aplicativo em determinado intervalo de tempo, verifique os itens a seguir:

-   Os dados de roaming não podem ultrapassar o limite de tamanho máximo (consulte [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) para obter detalhes).
-   Os arquivos foram fechados e desbloqueados corretamente.
-   Existem pelo menos dois dispositivos com a mesma versão do aplicativo.


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>Registre-se para receber uma notificação quando os dados de roaming mudarem

Para usar dados de aplicativo em roaming, você precisa se registrar para alterações de dados em roaming e recuperar os contêineres de dados em roaming para que você possa ler e gravar configurações.

1.  Registre-se para receber uma notificação quando os dados de roaming mudarem.

    O evento [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) avisa quando há alterações dos dados em roaming. Este exemplo define `DataChangeHandler` como o manipulador para alterações de dados em roaming.

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

2.  Obtenha os contêineres das configurações e dos arquivos do aplicativo.

    Use a propriedade [**ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624) para obter as configurações e a propriedade [**ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) para obter os arquivos.

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>Criar e recuperar configurações em roaming

Use a propriedade [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) para acessar as configurações no contêiner `roamingSettings` que obtivemos na seção anterior. Este exemplo cria uma configuração simples nomeada `exampleSetting` e um valor composto nomeado `composite`.

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

### <a name="create-and-retrieve-roaming-files"></a>Criar e recuperar arquivos em roaming

Para criar e atualizar um arquivo no repositório de dados locais de aplicativo, use as APIs de arquivo, como [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) e [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505). Este exemplo cria um arquivo chamado `dataFile.txt` no contêiner `roamingFolder` e escreve a data e hora atual no arquivo. O valor **ReplaceExisting** da enumeração [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) indica para substituir o arquivo se ele já existir.

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

Para abrir e ler um arquivo no repositório de dados de aplicativo em roaming, use as APIs de arquivo, como [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) e [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482). Este exemplo abre o arquivo `dataFile.txt` criado na etapa anterior e lê a data do arquivo. Para saber detalhes de como carregar recursos de arquivos de vários locais, veja [Como carregar recursos de arquivos](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

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


## <a name="temporary-app-data"></a>Dados temporários de aplicativo


O repositório de dados temporários de aplicativo funciona como um cache. Os arquivos não se movimentam e podem ser removidos a qualquer momento. A tarefa Manutenção do Sistema pode excluir automaticamente dados armazenados neste local a qualquer momento. O usuário também pode limpar arquivos do repositório de dados temporários usando a Limpeza de Disco. Dados temporários de aplicativo podem ser usados para armazenar informações temporárias durante uma sessão de aplicativo. Não há garantia de que esses dados persistirão além do fim da sessão do aplicativo, pois o sistema pode recuperar o espaço usado, se necessário. O local está disponível por meio da propriedade [**temporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629).

### <a name="retrieve-the-temporary-data-container"></a>Recuperar o contêiner de dados temporários

Use a propriedade [**ApplicationData.TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) para obter os arquivos. As próximas etapas usam a variável `temporaryFolder` desta etapa.

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>Criar e ler arquivos temporários

Para criar e atualizar um arquivo no repositório de dados de aplicativo temporário, use as APIs de arquivo, como [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) e [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505). Este exemplo cria um arquivo chamado `dataFile.txt` no contêiner `temporaryFolder` e escreve a data e hora atual no arquivo. O valor **ReplaceExisting** da enumeração [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) indica para substituir o arquivo se ele já existir.


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

Para abrir e ler um arquivo no repositório temporário de dados de aplicativo, use as APIs de arquivo, como [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) e [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482). Este exemplo abre o arquivo `dataFile.txt` criado na etapa anterior e lê a data do arquivo. Para saber detalhes de como carregar recursos de arquivos de vários locais, veja [Como carregar recursos de arquivos](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

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

## <a name="organize-app-data-with-containers"></a>Organizar os dados de aplicativo com contêineres


Para ajudá-lo a organizar seus arquivos e configurações de dados de aplicativo, você pode criar contêineres (representado por objetos [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599)) em vez de trabalhar diretamente com diretórios. Você pode adicionar contêineres aos repositórios de dados de aplicativo local, em roaming e temporário. Contêineres podem ser aninhados em até 32 níveis.

Para criar um contêiner de configurações, chame o método [**ApplicationDataContainer.CreateContainer**](https://msdn.microsoft.com/library/windows/apps/br241611). Este exemplo cria um contêiner de configurações local nomeado `exampleContainer` e adiciona uma configuração nomeada `exampleSetting`. O valor **Always** da enumeração de [**ApplicationDataCreateDisposition**](https://msdn.microsoft.com/library/windows/apps/br241616) indica que o contêiner deve ser criado se ele ainda não existir.

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

## <a name="delete-app-settings-and-containers"></a>Excluir configurações de aplicativo e contêineres


Para excluir um configuração simples de que seu aplicativo não precisa, use o método [**ApplicationDataContainerSettings.Remove**](https://msdn.microsoft.com/library/windows/apps/br241608). Este exemplo exclui a configuração local `exampleSetting` que criamos anteriormente.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

Para excluir uma definição composta, use o método [**ApplicationDataCompositeValue.Remove**](https://msdn.microsoft.com/library/windows/apps/br241597). Este exemplo exclui a configuração composta `exampleCompositeSetting` local que criamos em um exemplo anterior.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

Para excluir um contêiner, chame o método [**ApplicationDataContainer.DeleteContainer**](https://msdn.microsoft.com/library/windows/apps/br241612). Este exemplo exclui o contêiner de configurações `exampleContainer` local que criamos anteriormente.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>Controlar a versão de seus dados de aplicativo


Como opção, você pode converter os dados do aplicativo para seu aplicativo. Isso permite que você crie uma versão futura de seu aplicativo que altere o formato de seus dados de aplicativo sem causar problemas de compatibilidade com a versão anterior do aplicativo. O aplicativo verifica a versão dos dados de aplicativo no armazenamento de dados e, caso a versão seja inferior à versão esperada, o aplicativo deverá atualizar os dados do aplicativo para o novo formato e atualizar a versão. Para obter mais informações, consulte a propriedade [** Application.Version**](https://msdn.microsoft.com/library/windows/apps/br241630) e o método [**ApplicationData.SetVersionAsync**](https://msdn.microsoft.com/library/windows/apps/hh701429).

## <a name="related-articles"></a>Artigos relacionados

* [**Windows.Storage.ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587)
* [**Windows.Storage.ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624)
* [**Windows.Storage.ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)
* [**Windows.Storage.ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)
* [**Windows.Storage.ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)

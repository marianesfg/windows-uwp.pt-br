---
title: Usando propriedades complementares
description: Introdução ao uso de propriedades complementares e detalhes sobre como eles foram implementados no Windows
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, uwp, API do WinRT, indexador, pesquisa
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369260"
---
# <a name="using-supplemental-properties"></a>Usando propriedades complementares  

## <a name="summary"></a>Resumo  
- Propriedades complementares permitir que os aplicativos em arquivos de marca com propriedades sem alterar o arquivo 
- Útil para casos em que você tem as propriedades que são difíceis de calcular ou o arquivo não pode ser modificado 
- Usando propriedades complementares é o mesmo que usar qualquer outra propriedade no sistema de propriedade do Windows  

## <a name="introduction"></a>Introdução 
Muitos dos novos aplicativos interessantes nos últimos anos exigem a execução de operações com uso intensivo da CPU nos arquivos de usuário para extrair propriedades úteis de arquivos além do básicos coisas como data de criação. Esses aplicativos de reconhecimento de objeto em imagens, extração intencional em emails e análise de texto a documentos do grupo juntos. Isso está sendo orientado pelo como computação poderosos agora está disponível na maioria dos consumidores de PCs.   

Tornando esses metadados pesquisáveis instantaneamente permite que os usuários sejam exponencialmente mais produtivos. Basta saber que sua filha está em uma imagem é interessante, mas que está sendo capaz de pesquisar a imagem com sua avó é muito mais útil. Ele torna a experiência de usar uma noção de computador mais pessoal e mais ativa. Como alguém na máquina está contatando para ajudá-lo a encontrar suas preciosas memórias. 

Há décadas, a solução para a pesquisa rápida no Windows tem sido o indexador e na atualização para criadores ele foi atualizado para dar suporte a esses novos cenários. Aplicativos agora são capazes de arquivos de marca com propriedades adicionais além daqueles que são extraídas pelo sistema. Essas propriedades são tratadas como cidadãos de primeira classe  

## <a name="windows-properties"></a>Propriedades do Windows 
O [sistema de propriedades do Windows](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system) tem sido uma parte fundamental de interagir com arquivos por anos. Ele permite que os aplicativos ler as propriedades de arquivos sem precisar compreender as partes internas de todos os formatos de arquivo diferentes ou linguagens de que um arquivo pode ser no. Tudo o que é abstraído para você como desenvolvedor, tudo o que você precisa fazer é perguntar para obter uma lista e especifique em ordem crescente ou decrescente.  

O sistema de propriedades está entremeado com o indexador do Windows – ele lê todas as propriedades de arquivos dentro de seu escopo e armazena-os. Mais tarde quando um aplicativo solicita uma lista de todos os. docx em uma pasta a ser classificada por data de modificação, exceto aquelas criadas por John Smith, o indexador pode retornar a lista instantaneamente.  

A desvantagem de como esses sistemas funcionam juntos é que o indexador usado para solicitar todas as propriedades ele seria armazenar sobre um arquivo esteja disponível imediatamente. Isso limitou-lo de saber sobre as propriedades mais interessantes que levam mais tempo para calcular uma vez que há um requisito de tempo de disco rígido.  

No entanto usando propriedades é fácil, o aplicativo pode solicitar um conjunto ordenado de propriedades de um arquivo, assim como trabalhar com um banco de dados, ou ele pode passar uma consulta como usando um mecanismo de pesquisa. O indexador irá processar a consulta e retornar os resultados. Isso dará aos desenvolvedores a flexibilidade para combinar seus filtros (por exemplo somente pesquisa arquivos jpg) com consulta do usuário (nome do arquivo a partir de "bird"). 

## <a name="supplemental-properties"></a>Propriedades complementares 

Propriedades complementares têm comportamento idêntico às propriedades do Windows regulares com uma diferença muito importante – eles não serão gravados quando o arquivo é adicionado ao indexador. Uma propriedade complementar deve ser adicionada por outro aplicativo no sistema mais tarde. É possível dois minutos mais tarde, uma vez conclui o reconhecimento de objeto ou pode ser dias mais tarde. 

Depois que a propriedade é gravada ele pode ser pesquisado, filtrado, classificado ou agrupado como qualquer outra propriedade no sistema. Bem-pode ser usado em consultas combinadas com outras propriedades no sistema, ou complementares ou não. Isso lhe dá a flexibilidade para combinar propriedades complementares facilmente com o código de sistema de arquivo existente sem ter que fazer uma reescrita.  

### <a name="example-scenarios"></a>Cenários de exemplo 

Existem milhares de propriedades diferentes, que você poderia escrever a uma propriedade complementar, mas há alguns cenários importantes que este tutorial será continua a voltar para:  

#### <a name="tagging-pictures-with-extracted-properties"></a>Marcar imagens com propriedades extraídas 
Esses aplicativos podem usar um modelo do AM treinado para extrair recursos de uma imagem que o sistema não sabe sobre como o objeto na imagem. Em seguida, tirar os objetos que ele identifica a imagem e adicioná-los para o sistema de propriedade para pesquisar mais tarde ou agrupamento.  

#### <a name="tagging-files-with-an-app-specific-id"></a>Arquivos de marcação com uma ID específica do aplicativo 
Muitos aplicativos de sincronização de arquivo usam sua própria identificação exclusiva para acompanhar arquivos quando eles passam entre o servidor e vários dispositivos de cliente. O cliente de sincronização pode escrever essa ID para o sistema de propriedades sem afetar o arquivo. Essa ID agora está disponível para o aplicativo mais tarde para acesso rápido e disponível para qualquer outro aplicativo no sistema para ler ao se comunicar com o provedor de sincronização. 

Há muitas outras opções para usar as propriedades complementares, mas ambos fazer bons exemplos porque eles requerem uma pesquisa rápida ou pesquisa, são informações que o sistema não conhece e não pode ser adicionado ao próprio arquivo.  

### <a name="using-supplemental-properties"></a>Usando propriedades complementares 
Usando as propriedades complementares é o mesmo que escrever uma propriedade normal para o sistema de arquivos. Se você estiver familiarizado com o uso StorageFiles e propriedades, você pode ignorar isso. Caso contrário, vamos percorrer um exemplo rápido de escrever uma única propriedade em um arquivo e, em seguida, ler a mesma propriedade novamente mais tarde.  

### <a name="writing-supplemental-properties"></a>Gravar propriedades complementares  
O exemplo será modificar apenas o primeiro arquivo que encontra para manter a simplicidade, mas geralmente um aplicativo adicionará a propriedade a todos os arquivos que ele encontra.  

```csharp
// Only indexed jpg files are going to be used 
QueryOptions option = new QueryOptions(CommonFileQuery.DefaultQuery, new string[] { ".jpg" }); 
option.IndexerOption = IndexerOption.OnlyUseIndexer; 
// Typically an app would loop over all the files in the library, updating them all with the new 
// value. To make the sample easier to understand however, this app is only going to update the  
// first file it finds 
var query = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(option); 
StorageFile file = (await query.GetFilesAsync()).FirstOrDefault(); 
if (file == null) 
{ 
    log("No jpg file found in the library. Stopping"); 
    return; 
} 
log("Found file: " + file.Path); 
// Selecting the property to modify and writing it out 
List<KeyValuePair<string, object>> props = new List<KeyValuePair<string, object>>();             
props.Add(new KeyValuePair<string, object>("System.Supplemental.ResourceId", fileId)); 
await file.Properties.SavePropertiesAsync(props); 
```

Há uma verificação importante se o local é indexado antes da gravação de uma propriedade. Neste exemplo estamos usando as opções de consulta para filtrar apenas indexados locais. Se isso não for viável, você pode verificar o estado indexado da pasta pai (arquivo. GetParentAsync(). GetIndexedStateAsync()). De qualquer forma produzirá os mesmos resultados 

### <a name="reading-supplemental-properties"></a>Ler propriedades complementares 
Novamente, uma propriedade suplementar de leitura é o mesmo que ler qualquer outra propriedade do sistema de arquivos. Neste exemplo o aplicativo apenas ler uma propriedade de um arquivo já tem um StorageFile, mas ele também pode ler as outras propriedades ao mesmo tempo.  

```csharp
// An object to hold the result from the indexer, and a string to store  
// the value in once we have confirmed it is valid. 
object uncheckedResourceId; 
string resourceId = ""; 
// Fetching the key value pair from the indexer 
IDictionary<string,object> returnedProps =  
    await file.Properties.RetrievePropertiesAsync(new string[] { "System.Supplemental.ResourceId" });             
if (returnedProps.TryGetValue("System.Supplemental.ResourceId", out uncheckedResourceId)) 
{ 
    if (uncheckedResourceId != null && !String.IsNullOrEmpty(uncheckedResourceId.ToString())) 
    { 
        resourceId = uncheckedResourceId.ToString(); 
    } 
} 
```
Há uma verificação para certificar-se de que o valor que está voltando do sistema de propriedades é o esperado. Embora seja improvável, é possível que o valor foi limpo, pois seu aplicativo o escreveu. Isso será abordado em detalhes abaixo.  

### <a name="implementation-notes"></a>Notas de implementação 
Há algumas opções sutis que foram feitas no design das propriedades complementares. Para ajudar com sua implementação, as seções a seguir foram copiadas da especificação de design de engenharia para o recurso. Eles fornecem uma visão rápida em como o recurso foi criado e por que algumas das limitações existem. 

### <a name="supplemental-properties-available"></a>Propriedades complementares disponíveis 
Há apenas duas propriedades disponíveis para uso para aplicativos inicialmente: System.Supplemental.ResourceId e System.Supplemental.AlbumID. Se houver uma necessidade para obter mais informações, que eles podem ser adicionados. A ID de álbum é uma cadeia de caracteres com vários valores que pode ser usada para muitos aplicativos diferentes e o ResourceId é usado como uma ID exclusiva para provedores de sincronização de nuvem. 

#### <a name="file-system-support"></a>Suporte de sistema de arquivos 
Como o FAT formatado mídia removível é um cenário importante, propriedades complementares dará suporte a unidades FAT e NTFS. Isso garantirá que as propriedades complementares estão estará disponível para todos os usuários, independentemente de seu tipo de dispositivo.   

### <a name="non-indexed-locations"></a>Locais não indexados  
Na área de trabalho, há um número de pastas que não são indexadas. Nesses casos, aplicativos talvez ainda queira tem acesso às propriedades complementares. No entanto, as propriedades complementares não estão disponíveis fora de locais indexados. Essa compensação foi feita por vários motivos:  

- Todas as bibliotecas e os locais de armazenamento de nuvem são indexados por padrão.   
  Estes são os locais que aplicativos UWP usando principalmente. Há outros locais que não são indexadas (unidades de rede ou sistema), mas com menos frequência são usados para armazenar dados de usuário. 

- O design de superfície de API do WinRT pressupõe que o indexador é quase sempre disponível.  
  Assim, o indexador já está disponível na maioria dos locais de aplicativos estão interessados. Se forem encontrados usuários para armazenar dados em locais não indexados, a solução mais fácil será adicionar esse local para o índice. Trabalho de propriedades, em seguida, complementar, enumeração será mais rápida e aplicativos poderão alterar controla o local.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>Leitura ou gravação de propriedades complementares de um arquivo em um local de Non-Indexed 
No caso em que um aplicativo tenta gravar uma propriedade complementar em um local que não é indexado no momento, em seguida, a chamada à API lançará uma exceção. Essa será a mesma exceção que é gerada como quando alguém tenta atualizar o System.Music.AlbumArtist em um arquivo. docx (Args inválido).  
 
### <a name="change-notifications"></a>As notificações de alteração:  
As notificações de alteração de UWP e controle de alterações continuem a funcionar para as propriedades adicionais que funcionam para as propriedades padrão. Isso permitirá que os aplicativos que estão fornecendo dados para acompanhar todas as alterações ocorrendo para um dos seus aplicativos 
  
### <a name="invalidating-properties"></a>Invalidar propriedades:  
As propriedades complementares em um arquivo podem se tornar desatualizadas sempre que um arquivo é modificado ou movido no sistema. Enviar por push os dados de aplicativos serão aqueles com as informações sobre se os dados são válidos ou precisam ser atualizado para que o sistema apenas fornecerá as ferramentas para que eles possam consultá-la em si.  
 
No caso de um arquivo é modificado, mas não movidos ou renomeados, todas as propriedades de complementares o arquivo permanecerá inalteradas. Os aplicativos poderão se registrar para notificações de alteração por meio da superfície de API existente e atualizar as propriedades conforme necessário. 
 
Se o arquivo é movido, as propriedades serão invalidados. O aplicativo irá receber as notificações de alteração de qualquer exclusão, criar, renomeado ou movido, dependendo de como exatamente a conclusão da operação. Depois que o aplicativo recebeu a notificação de alteração será capaz de inspecionar o arquivo e atualize as propriedades adicionais no arquivo conforme necessário. 
 
### <a name="indexer-rebuilds"></a>Recompilações do indexador  
Ocasionalmente, o índice do sistema tem ser recompilados para uma das várias razões – o esquema de propriedade foi alterado, EDP pode permitir que o usuário ou simplesmente o arquivo de banco de dados pode estar corrompido. Nesses casos, as propriedades complementares não serão preservadas. Consideramos que fazem o trabalho para tentar preservar as propriedades adicionais quando o índice estiver sendo recriado, mas houve alguns dos principais bloqueadores:  

### <a name="protecting-the-data"></a>Protegendo os dados 
No caso em que o arquivo de banco de dados está corrompido, erros de disco ou software não autorizado, será impossível proteger os dados que foram armazenados nesse arquivo. Ele precisa ser armazenado em outro lugar no sistema ou de alguma forma isolado do restante do banco de dados. 

Uma vez que já estamos fazendo muito trabalho para tornar o índice menor probabilidade de ser corrompidos, isso reduzirá a taxa de incidência de neste caso, mesmo assim.  
Manter o mapeamento entre os arquivos e seus metadados durante as recriações 

Mesmo se o índice pode proteger os dados entre uma recompilação, é impossível saber se o arquivo foi alterado enquanto o índice estava sendo reconstruído. Os dados que o índice é proteger contra o arquivo podem ser não é mais válidos se o arquivo for modificado ou movido.  
Comportamento 

No caso de uma recompilação de indexador, todos os dados complementares serão perdidos. Aplicativos será responsáveis por colocar os dados de volta em indexador que foi perdido durante a recriação. Colocar uma carga extra de aplicativos, mas é considerada razoável, pois eles sempre manterá o estado mestre para todos os seus dados.  

### <a name="recovering"></a>Recuperando 
Depois que os aplicativos tem observado que o índice estiver sendo recriado, será responsáveis por atualizar as propriedades adicionais com a sua conveniência.  
### <a name="privacy"></a>Privacidade 
Algumas das propriedades que podem ser escritas para os arquivos serão, de modo que os usuários não poderão querer que eles sejam compartilhados com outros aplicativos. Aplicativos devem ser capazes de indicar que as informações que estão escrevendo propriedades vai ser privada como seus aplicativos, compartilhados com apenas alguns outros aplicativos ou públicos para todos os aplicativos no sistema.  

Embora, potencialmente, isso é um recurso interessante para algumas das primeiras a adotar o recurso, eles acham que obter as propriedades do públicas ainda será adicionar muito valor para o design. Assim, isso está marcado como um bom ter, e podemos deve continuar a criar o recurso sem suporte para ocultar os valores se necessário. Adicioná-lo mais tarde será aberta mais cenários, portanto, será importante levar em consideração em qualquer designs.  

## <a name="conclusions"></a>Conclusões 
Isso é tudo, propriedades complementares são uma maneira fácil de armazenar mais propriedades de arquivo no sistema. Usá-los logicamente é opcional, mas ela pode fornecer uma borda de seu aplicativo em outros aplicativos que não é possível classificar e pesquisar seus dados mais rápido. 

Estamos ansiosos para ver os aplicativos a começar a usar essas propriedades. Se você tiver alguma dúvida sobre como usar o cabeçalho informe-nos comentários abaixo 

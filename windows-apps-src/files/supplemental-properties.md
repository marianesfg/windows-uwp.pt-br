---
title: Usar propriedades complementares
description: Introdução ao uso de propriedades complementares e detalhes sobre como elas foram implementadas no Windows
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10, uwp, WinRT API, Indexador, Pesquisar
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369260"
---
# <a name="using-supplemental-properties"></a>Usar propriedades complementares  

## <a name="summary"></a>Resumo  
- As propriedades complementares permitem que os aplicativos marquem arquivos com propriedades sem alterar o arquivo 
- Isso é útil quando você tem propriedades que são difíceis de calcular ou quando o arquivo não pode ser modificado 
- Usar propriedades complementares é o mesmo que usar qualquer outra propriedade no sistema de propriedades do Windows  

## <a name="introduction"></a>Introdução 
Muitos dos novos aplicativos interessantes nos últimos anos exigem a execução de operações intensivas da CPU em arquivos do usuário para extrair propriedades úteis dos arquivos além de itens básicos, como a data de criação. Esses aplicativos fazem reconhecimento de objetos em imagens, extração de intenção em e-mails e análise de texto para agrupar documentos. Isso está sendo impulsionado pela capacidade de computação disponível na maioria dos PCs de consumo.   

Tornar esses metadados pesquisáveis ​​instantaneamente permite que os usuários sejam mais produtivos de forma exponencial. Simplesmente saber que sua filha está em uma foto é interessante, mas ser capaz de procurar a foto dela com sua avó é muito mais útil. Isso faz com que a experiência de usar um computador pareça mais pessoal e mais ativa. Como se alguém na máquina estivesse tentando ajudar você a encontrar suas preciosas memórias. 

Por décadas, a solução para pesquisa rápida no Windows foi o indexador e, na Atualização para Criadores, foi atualizada para oferecer suporte a esses novos cenários. Os aplicativos agora podem marcar arquivos com propriedades adicionais além daquelas extraídas pelo sistema. Essas propriedades são tratadas como cidadãos de primeira classe  

## <a name="windows-properties"></a>Propriedades do Windows 
Há anos, o [sistema de propriedades do Windows](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system) tem sido uma parte importante da interação com arquivos. Ele permite que os aplicativos leiam propriedades de arquivos sem ter que compreender os componentes internos de todos os diferentes formatos de arquivo ou idiomas nos quais um arquivo pode estar. Tudo isso é abstraído para você como desenvolvedor, só é preciso pedir uma lista e especificar a ordem ascendente ou descendente.  

O sistema de propriedades está entremeado com o indexador do Windows, ele lê todas as propriedades dos arquivos dentro de seu escopo e as armazena. Posteriormente, quando um aplicativo solicitar uma lista de todos os .docx em uma pasta a ser classificada por data de modificação, excluindo os itens criados por John Smith, o indexador poderá retornar a lista instantaneamente.  

A desvantagem de como esses sistemas funcionavam juntos é que o indexador exigia que todas as propriedades que ele armazenaria sobre um arquivo estivessem disponíveis instantaneamente. Isso impedia o conhecimento de propriedades mais interessantes que demoram mais para serem calculadas, pois há um requisito de tempo difícil.  

Usar propriedades, porém, é fácil, o aplicativo pode solicitar um conjunto classificado de propriedades sobre um arquivo, muito parecido com o trabalho com um banco de dados, ou pode passar uma consulta como o uso de um mecanismo de busca. O indexador processará a consulta e retornará os resultados. Isso dá aos desenvolvedores a flexibilidade de combinar seus filtros (por exemplo, pesquisar somente arquivos jpg) com a consulta do usuário (nome do arquivo que começa com “pássaro”). 

## <a name="supplemental-properties"></a>Propriedades complementares 

As propriedades complementares comportam-se de forma idêntica às propriedades normais do Windows com uma diferença muito importante – elas não serão gravadas quando o arquivo for adicionado ao indexador. Uma propriedade complementar deve ser adicionada por outro aplicativo no sistema posteriormente. Pode ser dois minutos mais tarde, quando o reconhecimento de objeto for concluído, ou dias depois. 

Depois que a propriedade é gravada, ela pode ser pesquisada, filtrada, classificada ou agrupada como qualquer outra propriedade no sistema. Também pode ser usada em consultas combinadas com outras propriedades no sistema, seja complementar ou não. Isso dá a você a flexibilidade de combinar facilmente as propriedades complementares com o código do sistema de arquivos existente sem precisar reescrever.  

### <a name="example-scenarios"></a>Cenários de exemplo 

Existem milhares de propriedades diferentes que você pode escrever para uma propriedade complementar, mas há alguns cenários importantes que este tutorial continuará a retomar:  

#### <a name="tagging-pictures-with-extracted-properties"></a>Marcar imagens com propriedades extraídas 
Esses aplicativos podem usar um modelo treinado em ML para extrair recursos de uma imagem que o sistema não conhece, como um objeto na imagem. Ele pode, então, pegar os objetos identificados na imagem e adicioná-los ao sistema de propriedades para pesquisa ou agrupamento posterior.  

#### <a name="tagging-files-with-an-app-specific-id"></a>Marcar arquivos com uma ID específica do aplicativo 
Muitos aplicativos de sincronização de arquivos usam sua própria ID exclusiva para rastrear arquivos à medida que eles se movem entre o servidor e vários dispositivos clientes. O cliente de sincronização pode gravar essa ID no sistema de propriedades sem afetar o arquivo. Essa ID agora está disponível para o aplicativo posteriormente para um acesso rápido e para qualquer outro aplicativo no sistema para leitura na comunicação com o provedor de sincronização. 

Há muitas outras opções para usar propriedades complementares, mas ambas são bons exemplos porque exigem consulta ou pesquisa rápida, são informações que o sistema não conhece e não podem ser adicionadas ao próprio arquivo.  

### <a name="using-supplemental-properties"></a>Usar propriedades complementares 
Usar as propriedades complementares é o mesmo que gravar uma propriedade normal no sistema de arquivos. Se você estiver familiarizado com o uso de StorageFiles e propriedades, poderá ignorar isso. Caso contrário, vamos examinar rapidamente uma amostra de uma única propriedade em um arquivo e depois ler a mesma propriedade novamente mais tarde.  

### <a name="writing-supplemental-properties"></a>Gravar propriedades complementares  
O exemplo só modificará o primeiro arquivo que encontrar para simplificar, mas geralmente um aplicativo adicionará a propriedade a todos os arquivos que encontrar.  

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

Há uma verificação importante se o local é indexado antes de escrever uma propriedade. Neste exemplo, estamos usando as opções de consulta para filtrar somente locais indexados. Se isso não for viável, você poderá verificar o estado indexado da pasta pai (file.GetParentAsync().GetIndexedStateAsync()). De qualquer maneira, produzirá os mesmos resultados 

### <a name="reading-supplemental-properties"></a>Ler propriedades complementares 
Novamente, ler uma propriedade complementar é o mesmo que ler qualquer outra propriedade do sistema de arquivos. Neste exemplo, o aplicativo lerá apenas uma propriedade de um arquivo para o qual já possui um StorageFile, mas também poderá ler outras propriedades ao mesmo tempo.  

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
Há uma verificação para certificar-se de que o valor retornado do sistema de propriedades seja o esperado. Embora seja improvável, é possível que o valor tenha sido limpo desde que o momento em que o aplicativo o gravou. Isso será abordado em detalhes abaixo.  

### <a name="implementation-notes"></a>Notas de implementação 
Há algumas escolhas sutis que foram feitas no design das propriedades complementares. Para ajudar com sua implementação, as seções a seguir foram copiadas da especificação de design de engenharia do recurso. Elas fornecem uma visão rápida de como o recurso foi projetado e por que algumas das limitações existem. 

### <a name="supplemental-properties-available"></a>Propriedades complementares disponíveis 
Há apenas duas propriedades disponíveis para uso nos aplicativos inicialmente: System.Supplemental.ResourceId e System.Supplemental.AlbumID. Se houver necessidade de mais, elas podem ser adicionadas. A ID do álbum é uma cadeia de vários valores que pode ser usada para muitos aplicativos diferentes e o ResourceId é usado como uma ID exclusiva para provedores de sincronização na nuvem. 

#### <a name="file-system-support"></a>Suporte ao sistema de arquivos 
Como a mídia removível formatada em FAT é um cenário importante, as propriedades complementares oferecerão suporte a unidades FAT e NTFS. Isso garantirá que as propriedades complementares estejam disponíveis para todos os usuários, independentemente do tipo de dispositivo.   

### <a name="non-indexed-locations"></a>Locais não indexados  
Na área de trabalho, há várias pastas que não são indexadas. Nesses casos, os aplicativos ainda podem querer ter acesso a propriedades complementares. No entanto, as propriedades complementares não estão disponíveis fora dos locais indexados. Essa compensação foi empregada por algumas razões:  

- Todas as bibliotecas e locais de armazenamento em nuvem são indexados por padrão.   
  Esses são os locais que os aplicativos UWP usam principalmente. Há outros locais que não são indexados (unidades de sistema ou de rede), mas são menos usados ​​para armazenar dados do usuário. 

- O design de superfície da API do WinRT pressupõe que o indexador esteja quase sempre disponível.  
  Assim, o indexador já está disponível na maioria dos locais em que os aplicativos estão interessados. Caso os usuários estejam armazenando dados em locais não indexados, a solução mais fácil será adicionar esse local ao índice. Assim, as propriedades complementares funcionam, a enumeração será mais rápida e os aplicativos poderão alterar o rastreamento do local.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>Ler ou gravar propriedades complementares de um arquivo em um local não indexado 
No caso de um aplicativo tentar gravar uma propriedade complementar em um local que não esteja indexado no momento, a chamada da API lançará uma exceção. Esta será a mesma exceção que é lançada como quando alguém tenta atualizar o System.Music.AlbumArtist em um arquivo .docx (Args inválido).  
 
### <a name="change-notifications"></a>Alterar notificações:  
As notificações de alteração de UWP e o acompanhamento de alterações continuarão a funcionar para as propriedades complementares, assim como para as propriedades padrão. Isso permitirá que os aplicativos que fornecem dados acompanhem todas as alterações que estão ocorrendo em um de seus aplicativos 
  
### <a name="invalidating-properties"></a>Invalidar propriedades:  
As propriedades complementares em um arquivo podem ficar desatualizadas sempre que um arquivo é modificado ou movido no sistema. Os aplicativos que enviam dados serão aqueles com as informações sobre se os dados são válidos ou precisam ser atualizados para que o sistema forneça apenas as ferramentas para que eles possam descobrir isso por si mesmos.  
 
No caso de um arquivo ser modificado, mas não movido ou renomeado, todas as propriedades complementares no arquivo permanecerão inalteradas. Os aplicativos poderão se registrar para notificações de alteração por meio da superfície da API existente e atualizar as propriedades conforme necessário. 
 
Se o arquivo for movido, as propriedades serão invalidadas. O aplicativo receberá as notificações de alteração de excluir, criar, renomear ou mover, dependendo de como exatamente a operação for feita. Depois que o aplicativo receber a notificação de alteração, ele poderá inspecionar o arquivo e atualizar as propriedades complementares no arquivo, conforme necessário. 
 
### <a name="indexer-rebuilds"></a>Recompilações do indexador  
Ocasionalmente, o índice do sistema deve ser recompilado por uma de várias razões – o esquema da propriedade pode mudar, o usuário pode habilitar o EDP ou simplesmente o arquivo do banco de dados pode estar corrompido. Nestes casos, as propriedades complementares não serão preservadas. Consideramos fazer o trabalho para tentar preservar as propriedades complementares quando o índice está sendo recompilado, mas havia alguns grandes bloqueadores:  

### <a name="protecting-the-data"></a>Proteger os dados 
No caso em que o arquivo de banco de dados está corrompido, seja por erros de disco ou software invasor, será impossível proteger os dados que foram armazenados nesse arquivo. Ele teria que ser armazenado em algum outro lugar do sistema ou, de alguma forma, isolado do restante do banco de dados. 

Como já estamos empregando muito trabalho para tornar o índice menos propenso a ser corrompido, isso reduzirá a taxa de incidência deste caso de qualquer maneira.  
Manter o mapeamento entre arquivos e seus metadados durante as recompilações 

Mesmo se o índice puder proteger os dados em uma recompilação, é impossível saber se o arquivo foi alterado enquanto o índice estava sendo recompilado. Os dados que o índice está protegendo do arquivo podem não ser mais válidos se o arquivo for modificado ou movido.  
Comportamento 

No caso de uma recompilação do indexador, todos os dados complementares serão perdidos. Os aplicativos serão responsáveis ​​por colocar os dados de volta no indexador que foi perdido durante a recompilação. Isso sobrecarrega os aplicativos, mas é considerado razoável, pois sempre manterá o estado mestre para todos os dados.  

### <a name="recovering"></a>Recuperação 
Depois que os aplicativos notarem que o índice está sendo recompilado, eles serão responsáveis ​​por atualizar as propriedades complementares conforme sua conveniência.  
### <a name="privacy"></a>Privacidade 
Os usuários podem não querer que algumas das propriedades gravadas nos arquivos sejam compartilhadas com outros aplicativos. Os aplicativos devem indicar que as informações que estão gravando nas propriedades serão privadas para seus aplicativos, compartilhadas com apenas alguns outros aplicativos ou públicas para todos os aplicativos no sistema.  

Embora esse seja um recurso potencialmente interessante para alguns dos primeiros usuários do recurso, eles acham que a obtenção de propriedades públicas ainda adicionará muito valor ao design. Assim, isso é marcado como um ponto positivo, e devemos continuar a compilar o recurso sem suporte para ocultar os valores, se necessário. Adicioná-lo mais tarde abrirá mais cenários, por isso será importante considerá-lo em qualquer projeto.  

## <a name="conclusions"></a>Conclusões 
É isso, as propriedades complementares são uma maneira fácil de armazenar mais propriedades de arquivos no sistema. É claro que usá-las é opcional, mas pode dar ao seu aplicativo uma vantagem sobre outros aplicativos que não podem classificar e pesquisar seus dados com a mesma rapidez. 

Estamos ansiosos para ver os aplicativos começarem a usar essas propriedades. Se você tiver alguma dúvida sobre como usar o cabeçalho, entre em contato conosco nos comentários abaixo 

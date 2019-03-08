---
title: Práticas recomendadas para gravar em arquivos
description: Aprenda as práticas recomendadas para usar vários arquivos escrever métodos das classes FileIO e PathIO.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8bed97e060015f92ff95c9f7d797bbcb83db431
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605831"
---
# <a name="best-practices-for-writing-to-files"></a>Práticas recomendadas para gravar em arquivos

**APIs importantes**

* [**Classe FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**Classe PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Os desenvolvedores, às vezes, são executados em um conjunto de problemas comuns ao usar o **escrever** métodos das [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) classes para executar operações de e/s do sistema de arquivos. Por exemplo, problemas comuns incluem:

• Um arquivo parcialmente é gravado • o aplicativo recebe uma exceção ao chamar um dos métodos. • As operações de deixam para trás. Arquivos TMP com um nome de arquivo semelhante ao nome do arquivo de destino.

O **escrever** métodos das [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) classes incluem o seguinte:

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Este artigo fornece detalhes sobre como esses métodos funcionam para que os desenvolvedores melhor entendem quando e como usá-los. Este artigo fornece diretrizes e não tenta fornecer uma solução para todos os problemas de e/s de arquivo possíveis. 

> [!NOTE]
> Este artigo enfoca os **FileIO** métodos em exemplos e discussões. No entanto, o **PathIO** métodos seguem um padrão semelhante e a maioria das diretrizes neste artigo aplica-se também a esses métodos. 

## <a name="conveience-vs-control"></a>Conveience x controle

Um [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) objeto não é um identificador de arquivo, como o modelo de programação nativo do Win32. Em vez disso, uma [ **StorageFile** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) é uma representação de um arquivo com métodos para manipular seu conteúdo.

Noções básicas sobre esse conceito é útil ao executar e/s com uma **StorageFile**. Por exemplo, o [gravando em um arquivo](quickstart-reading-and-writing-files.md#writing-to-a-file) seção apresenta três maneiras de gravar em um arquivo:

* Usando o [ **FileIO.WriteTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) método.
* Criando um buffer e, em seguida, chamando o [ **FileIO.WriteBufferAsync** ](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) método.
* O modelo de quatro etapas usando um fluxo:
  1. [Abra](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) o arquivo para obter um fluxo.
  2. [Obter](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) um fluxo de saída.
  3. Criar uma [ **DataWriter** ](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) do objeto e chame correspondente **gravar** método.
  4. [Confirmar](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) os dados no gravador de dados e [flush](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) no fluxo de saída.

Os dois primeiros cenários são os mais comumente usados por aplicativos. Gravação no arquivo em uma única operação é mais fácil de codificar e manter, e também remove a responsabilidade do aplicativo de lidar com muitas das complexidades de e/s de arquivo. No entanto, essa conveniência gera um custo: a perda de controle sobre toda a operação e a capacidade de detectar erros em pontos específicos.

## <a name="the-transactional-model"></a>O modelo transacional

O **escrever** métodos das [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) classes envolvem as etapas de gravação de terceiro modelo descrito acima, com uma camada adicional. Essa camada é encapsulada em uma transação de armazenamento.

Para proteger a integridade do arquivo original no caso de algo der errado durante a gravação de dados, o **escrever** métodos usam um modelo transacional, abrindo o arquivo usando [ **OpenTransactedWriteAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Esse processo cria uma [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) objeto. Depois que esse objeto de transação é criado, as APIs de gravar os dados a seguir de modo semelhante de [acesso a arquivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) amostra ou o exemplo de código a [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) artigo.

O diagrama a seguir ilustra as subjacentes tarefas executadas pelo a **WriteTextAsync** método em uma operação de gravação com êxito. Esta ilustração fornece uma exibição simplificada da operação. Por exemplo, ele ignora etapas, como preenchimento automático de codificação e async do texto em threads diferentes.

![Diagrama de sequência de chamada de API de UWP para gravar em um arquivo](images/file-write-call-sequence.svg)

As vantagens de usar o **escrever** métodos das [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) classes em vez disso o modelo de quatro etapas mais complexo usando um fluxo são:

* Uma chamada à API para lidar com todas as etapas intermediárias, incluindo erros.
* O arquivo original é mantido se algo der errado.
* O estado do sistema tentará ser mantidas tão limpo quanto possível.

No entanto, com muitos pontos intermediários de possíveis de falha, há uma maior possibilidade de falha. Quando ocorre um erro pode ser difícil de entender em que o processo falhou. As seções a seguir apresentam algumas das falhas, você pode encontrar ao usar o **gravar** métodos e fornecem soluções possíveis.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Códigos de erro comuns para os métodos de gravação das classes FileIO e PathIO

Esta tabela apresenta os códigos de erro comuns que os desenvolvedores de aplicativos encontram ao usar o **gravar** métodos. As etapas na tabela correspondem às etapas no diagrama anterior.

|  Erro de nome (valor)  |  Etapas  |  Causas  |  Soluções  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  O arquivo original pode ser marcado para exclusão, possivelmente a partir de uma operação anterior.  |  Repita a operação.</br>Certifique-se de que o acesso para o arquivo é sincronizado.  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  O arquivo original é aberto por outro gravação exclusivo.   |  Repita a operação.</br>Certifique-se de que o acesso para o arquivo é sincronizado.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  Não foi possível substituir o arquivo original (arquivo. txt), porque ele está em uso. Outro processo ou operação obteve acesso ao arquivo antes que ele pode ser substituído.  |  Repita a operação.</br>Certifique-se de que o acesso para o arquivo é sincronizado.  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  O modelo transacionado cria um arquivo extra e isso consome armazenamento extra.  |    |
|  ERROR_OUTOFMEMORY (0X8007000E)  |  14, 16  |  Isso pode acontecer devido a várias operações de e/s pendentes ou tamanhos de arquivos grandes.  |  Uma abordagem mais granular, controlando o fluxo pode resolver o erro.  |
|  E_FAIL (0x80004005) |  Qualquer  |  Diversos  |  Repita a operação. Se ele ainda falhar, ele pode ser um erro de plataforma e o aplicativo deve ser interrompido porque ele está em um estado inconsistente. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Outras considerações para estados de arquivo que podem levar a erros

Além dos erros retornados pelo **gravar** métodos, aqui estão algumas diretrizes sobre o que um aplicativo pode esperar ao gravar em um arquivo.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Dados foram gravados para o arquivo somente se a operação foi concluída

Seu aplicativo não deve fazer qualquer suposição sobre os dados no arquivo enquanto uma operação de gravação está em andamento. Tentativa de acessar o arquivo antes de uma operação é concluída pode levar a dados inconsistentes. Seu aplicativo deve ser responsável de acompanhamento e/SS pendentes.

### <a name="readers"></a>Leitores

Se o arquivo que também está sendo gravada no que está sendo usado por um leitor cortês (ou seja, é aberto com [ **FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode), as leituras subsequentes falhará com um erro ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). Às vezes, aplicativos repetir a abertura do arquivo para leitura novamente enquanto o **gravar** operação está em andamento. Isso pode resultar em uma condição de corrida em que o **gravar** finalmente falha ao tentar substituir o arquivo original, porque ele não pode ser substituído.

### <a name="files-from-knownfolders"></a>Arquivos de KnownFolders

Seu aplicativo pode não ser o único aplicativo que está tentando acessar um arquivo que reside em qualquer um dos [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). Não há nenhuma garantia de que, se a operação for bem-sucedida, o conteúdo de que um aplicativo escrito para o arquivo permanecerá constante na próxima vez que ele tenta ler o arquivo. Além disso, compartilhamento ou acesso negado erros se tornar mais comuns nessa situação.

### <a name="conflicting-io"></a>Conflito de e/s

As chances de erros de simultaneidade podem ser reduzidas se nosso aplicativo usa o **gravar** métodos para arquivos em seus dados locais, mas algum cuidado ainda é necessária. Se vários **gravar** operações estão sendo enviados simultaneamente para o arquivo, não há nenhuma garantia sobre quais dados acabam no arquivo. Para atenuar isso, é recomendável que seu aplicativo serializa **gravar** operações para o arquivo.

### <a name="tmp-files"></a>~ Arquivos TMP

Ocasionalmente, se a operação de modo forçado foi cancelada (por exemplo, se o aplicativo foi suspenso ou encerrado pelo sistema operacional), a transação não está confirmada ou fechada adequadamente. Isso pode deixar para trás arquivos com um (. ~ TMP) extensão. Considere a exclusão desses arquivos temporários (se existirem os dados do aplicativo local) ao lidar com a ativação do aplicativo.

## <a name="considerations-based-on-file-types"></a>Considerações com base nos tipos de arquivo

Alguns erros podem ser mais prevalentes dependendo do tipo de arquivos, a frequência na qual são acessados e tamanho do arquivo. Em geral, há três categorias de arquivos de que seu aplicativo pode acessar:

* Arquivos criados e editados pelo usuário na pasta de dados local do seu aplicativo. Eles são criados e editados somente durante o uso de seu aplicativo, e existem somente dentro do aplicativo.
* Metadados do aplicativo. Seu aplicativo usa esses arquivos para controlar seu próprio estado.
* Outros arquivos em locais do sistema de arquivos em que seu aplicativo declarou recursos para acessar. Geralmente, eles estão localizados em um dos [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

Seu aplicativo tem controle total sobre as duas primeiras categorias de arquivos, pois elas são parte dos arquivos de pacote do seu aplicativo e são acessadas exclusivamente pelo seu aplicativo. Para arquivos em que a última categoria, seu aplicativo deve estar ciente de que outros aplicativos e serviços do sistema operacional podem estar acessando os arquivos simultaneamente.

Dependendo do aplicativo, o acesso aos arquivos pode variar na frequência:

* Muito baixa. Geralmente, esses são arquivos que são abertos depois que quando o aplicativo é iniciado e são salvos quando o aplicativo está suspenso.
* Baixa. Esses são arquivos que o usuário é especificamente assumir uma ação (como salvar ou carregar).
* Média ou alta. Esses são arquivos em que o aplicativo deve atualizar constantemente dados (por exemplo, recursos de salvamento automático ou constante metadados de controle).

Para o tamanho do arquivo, considere os dados de desempenho no gráfico a seguir para o **WriteBytesAsync** método. Este gráfico compara o tempo para concluir um tamanho de arquivo do vs de operação, ao longo de um desempenho médio de operações de 10000 por tamanho do arquivo em um ambiente controlado.

![Desempenho WriteBytesAsync](images/writebytesasync-performance.png)

Os valores de tempo no eixo y são omitidos intencionalmente nesse gráfico porque as configurações e um hardware diferente produzirá valores diferentes de tempo absoluto. No entanto, temos consistentemente observado essas tendências em nossos testes:

* Para arquivos muito pequenos (< = 1 MB): O tempo para concluir as operações é consistentemente rápido.
* Para arquivos maiores (> 1 MB): O tempo para concluir as operações começa a aumentar exponencialmente.

## <a name="io-during-app-suspension"></a>E/s durante a suspensão de aplicativos

Seu aplicativo devem ser criados para lidar com a suspensão se você quiser manter informações de estado ou de metadados para uso em sessões posteriores. Para obter informações sobre a suspensão de aplicativos, consulte [ciclo de vida do aplicativo](../launch-resume/app-lifecycle.md) e [nesta postagem de blog](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

A menos que o sistema operacional concede execução estendida ao seu aplicativo, quando seu aplicativo é suspenso, ele tem cinco segundos para liberar todos os seus recursos e salvar seus dados. Para a confiabilidade e o usuário a melhor experiência, sempre supor que a hora em que você precisa lidar com tarefas de suspensão é limitada. Tenha em mente as seguintes diretrizes durante os período de tempo para lidar com tarefas de suspensão de 5 segundos:

* Tente manter a e/s mínima para evitar condições de corrida causadas por operações de liberação e versão.
* Evite gravar arquivos que exigem a centenas de milissegundos ou mais para escrever.
* Se seu aplicativo usa o **gravar** métodos, tenha em mente que todas as etapas intermediárias que esses métodos exigem.

Se seu aplicativo opera em uma pequena quantidade de dados de estado durante a suspensão, a maioria dos casos você pode usar o **gravar** métodos para liberar os dados. No entanto, se seu aplicativo usa uma grande quantidade de dados de estado, considere usar fluxos diretamente armazenar seus dados. Isso pode ajudar a reduzir o atraso introduzido pelo modelo transacional do **gravar** métodos. 

Por exemplo, consulte o [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) exemplo.

## <a name="other-examples-and-resources"></a>Outros exemplos e recursos

Aqui estão vários exemplos e outros recursos para cenários específicos.

### <a name="code-example-for-retrying-file-io-example"></a>Exemplo de código para repetir exemplo de arquivo e/s

A seguir está um exemplo de pseudocódigo sobre como repetir uma gravação (C#), supondo que a gravação deve ser feito depois que o usuário seleciona um arquivo para gravação:

```csharp
Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();

Int32 retryAttempts = 5;

const Int32 ERROR_ACCESS_DENIED = unchecked((Int32)0x80070005);
const Int32 ERROR_SHARING_VIOLATION = unchecked((Int32)0x80070020);

if (file != null)
{
    // Application now has read/write access to the picked file.
    while (retryAttempts > 0)
    {
        try
        {
            retryAttempts--;
            await Windows.Storage.FileIO.WriteTextAsync(file, "Text to write to file");
            break;
        }
        catch (Exception ex) when ((ex.HResult == ERROR_ACCESS_DENIED) ||
                                   (ex.HResult == ERROR_SHARING_VIOLATION))
        {
            // This might be recovered by retrying, otherwise let the exception be raised.
            // The app can decide to wait before retrying.
        }
    }
}
else
{
    // The operation was cancelled in the picker dialog.
}
```

### <a name="synchronize-access-to-the-file"></a>Sincronizar o acesso ao arquivo

O [.NET blog Parallel Programming with](https://blogs.msdn.microsoft.com/pfxteam/) é um excelente recurso para obter orientações sobre programação paralela. Em particular, o [postagens sobre AsyncReaderWriterLock](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) descreve como manter o acesso exclusivo a um arquivo para gravações, permitindo que o acesso simultâneo de leitura. Tenha em mente serializando que e/s terá impacto sobre desempenho.

## <a name="see-also"></a>Consulte também

* [Criar, gravar e ler um arquivo](quickstart-reading-and-writing-files.md)

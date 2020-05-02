---
title: Práticas recomendadas para gravar em arquivos
description: Conheça as melhores práticas para usar os vários métodos de gravação de arquivos das classes FileIO e PathIO.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dcbeffc7e3db8f3df9c197e8c388f30faf7ad03d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75685246"
---
# <a name="best-practices-for-writing-to-files"></a>Práticas recomendadas para gravar em arquivos

**APIs importantes**

* [**Classe FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**Classe PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Às vezes, os desenvolvedores enfrentam um conjunto de problemas comuns ao usar os métodos **Write** das classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) para executar operações de E/S do sistema de arquivos. Por exemplo, problemas comuns incluem:

* Um arquivo parcialmente gravado.
* O aplicativo recebe uma exceção ao chamar um dos métodos.
* As operações deixam para trás arquivos .TMP com um nome de arquivo semelhante ao nome do arquivo de destino.

Os métodos **Write** das classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) incluem o seguinte:

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Este artigo fornece detalhes de como esses métodos funcionam para que os desenvolvedores entendam quando e como usá-los. Este artigo fornece diretrizes e não tem objetivo de fornecer uma solução para todos os problemas de E/S de arquivo possíveis. 

> [!NOTE]
> Este artigo concentra-se nos métodos da **FileIO** nos exemplos e discussões. No entanto, os métodos da **PathIO** seguem um padrão semelhante e a maioria das diretrizes deste artigo aplica-se também a esses métodos. 

## <a name="convenience-vs-control"></a>Conveniência versus controle

Um objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) não é um identificador de arquivo, como o modelo de programação nativo do Win32. Em vez disso, um [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) é uma representação de um arquivo com métodos para manipular seu conteúdo.

Entender esse conceito é útil ao realizar E/S com um **StorageFile**. Por exemplo, a seção [Gravando em um arquivo](quickstart-reading-and-writing-files.md#writing-to-a-file) apresenta três maneiras de gravar em um arquivo:

* Usando o método [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync).
* Criando um buffer e, em seguida, chamando o método [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writebufferasync).
* O modelo de quatro etapas usando um fluxo:
  1. [Abra](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) o arquivo para obter um fluxo.
  2. [Obtenha](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) um fluxo de saída.
  3. Crie um objeto [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) e chame o método **Write** correspondente.
  4. [Faça commit](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) dos dados no gravador de dados e [libere](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) o fluxo de saída.

Os dois primeiros cenários são os mais comumente usados por aplicativos. Gravar no arquivo em uma única operação é mais fácil para escrever código e dar manutenção, além de remover a responsabilidade do aplicativo de lidar com muitas das complexidades de E/S de arquivo. No entanto, essa conveniência gera um custo: a perda de controle sobre toda a operação e da capacidade de detectar erros em pontos específicos.

## <a name="the-transactional-model"></a>O modelo transacional

Os métodos **Write** das classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) encapsulam as etapas no terceiro modelo de gravação descrito acima, com uma camada adicional. Essa camada é encapsulada em uma transação de armazenamento.

Para proteger a integridade do arquivo original no caso de algo dar errado durante a gravação dos dados, os métodos **Write** usam um modelo transacional, abrindo o arquivo usando [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Esse processo cria um objeto [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction). Depois que esse objeto de transação é criado, as APIs gravam os dados seguindo um modo semelhante ao exemplo [Acesso a Arquivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) ou o exemplo de código no artigo [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction).

O diagrama a seguir ilustra as tarefas subjacentes executadas pelo método **WriteTextAsync** em uma operação de gravação com êxito. Esta ilustração oferece uma exibição simplificada da operação. Por exemplo, ela ignora etapas como codificação e de texto e conclusão assíncrona em threads diferentes.

![Diagrama de sequência de chamada à API da UWP para gravar em um arquivo](images/file-write-call-sequence.svg)

As vantagens de usar os métodos **Write** das classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) em vez do modelo de quatro etapas mais complexo que usa um fluxo são:

* Uma chamada à API para manipular todas as etapas intermediárias, incluindo erros.
* O arquivo original será mantido se algo der errado.
* O estado do sistema tentará ser mantido tão limpo quanto possível.

No entanto, com tantos pontos intermediários possíveis de falha, há uma maior possibilidade de falha. Quando ocorrer um erro poderá ser difícil de entender o local em que o processo falhou. As seções a seguir apresentam algumas das falhas que você poderá encontrar ao usar os métodos **Write** e oferecem as soluções possíveis.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Códigos de erro comuns para os métodos Write das classes FileIO e PathIO

Esta tabela apresenta códigos de erro comuns que os desenvolvedores de aplicativos encontram ao usar os métodos **Write**. As etapas na tabela correspondem às etapas no diagrama anterior.

|  Nome do erro (valor)  |  Etapas  |  Causas  |  Soluções  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  O arquivo original pode estar marcado para exclusão, possivelmente devido a uma operação anterior.  |  Repita a operação.</br>Certifique-se de que o acesso ao arquivo seja sincronizado.  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  O arquivo original é aberto por outra gravação exclusiva.   |  Repita a operação.</br>Certifique-se de que o acesso ao arquivo seja sincronizado.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  Não foi possível substituir o arquivo original (arquivo.txt), porque ele está em uso. Outro processo ou operação obteve acesso ao arquivo antes que ele fosse substituído.  |  Repita a operação.</br>Certifique-se de que o acesso ao arquivo seja sincronizado.  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  O modelo transacionado cria um arquivo extra e isso consome armazenamento adicional.  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14, 16  |  Isso pode acontecer devido a várias operações de E/S pendentes ou tamanhos de arquivos grandes.  |  Uma abordagem mais granular, controlando o fluxo, pode resolver o erro.  |
|  E_FAIL (0x80004005) |  qualquer  |  Diversos  |  Repita a operação. Se ainda falhar, poderá ser devido a um erro de plataforma e o aplicativo deverá ser interrompido porque está em um estado inconsistente. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Outras considerações de estados de arquivo que podem levar a erros

Além dos erros retornados pelos métodos **Write**, aqui estão algumas diretrizes sobre o que um aplicativo pode esperar ao gravar em um arquivo.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Dados foram gravados no arquivo se e somente se a operação foi concluída

Seu aplicativo não deverá fazer nenhuma suposição sobre os dados no arquivo enquanto uma operação de gravação estiver em andamento. Tentar acessar o arquivo antes da conclusão de uma operação poderá gerar dados inconsistentes. Seu aplicativo deve ser responsável por acompanhar operações de E/S pendentes.

### <a name="readers"></a>Leitores

Se o arquivo que está sendo gravado também estiver sendo usado por um leitor cortês (ou seja, aberto com [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode)), as leituras subsequentes falharão com um erro ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). Às vezes, os aplicativos repetem a abertura do arquivo para leitura enquanto a operação **Write** está em andamento. Isso pode resultar em uma condição de corrida em que o **Write** acaba falhando ao tentar substituir o arquivo original, porque ele não pode ser substituído.

### <a name="files-from-knownfolders"></a>Arquivos de KnownFolders

Seu aplicativo pode não ser o único aplicativo que está tentando acessar um arquivo que reside em qualquer uma das [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). Não há nenhuma garantia de que, se a operação for bem-sucedida, o conteúdo que um aplicativo gravar no arquivo permanecerá constante na próxima vez que ele tentar ler o arquivo. Além disso, erros de compartilhamento ou de acesso negado se tornam mais comuns nessa situação.

### <a name="conflicting-io"></a>Conflito de E/S

As chances de erros de simultaneidade podem ser reduzidas se o aplicativo usa os métodos **Write** para arquivos em seus dados locais, mas ainda é necessário algum cuidado. Se várias operações **Write** estiverem sendo enviadas simultaneamente para o arquivo, não haverá nenhuma garantia de quais dados acabarão no arquivo. Para atenuar isso, é recomendável que seu aplicativo serialize as operações **Write** no arquivo.

### <a name="tmp-files"></a>Arquivos ~TMP

Ocasionalmente, se a operação for cancelada de modo forçado (por exemplo, se o aplicativo for suspenso ou encerrado pelo sistema operacional), a transação não será confirmada nem fechada adequadamente. Isso poderá deixar para trás arquivos com uma extensão (.~TMP). Considere a exclusão desses arquivos temporários (se existirem nos dados locais do aplicativo) ao lidar com a ativação do aplicativo.

## <a name="considerations-based-on-file-types"></a>Considerações com base nos tipos de arquivo

Alguns erros podem ser mais predominantes dependendo do tipo dos arquivos, da frequência com que são acessados e do tamanho do arquivo. Em geral, há três categorias de arquivos que seu aplicativo pode acessar:

* Arquivos criados e editados pelo usuário na pasta de dados local do seu aplicativo. Eles são criados e editados somente durante o uso de seu aplicativo e existem somente dentro do aplicativo.
* Metadados do aplicativo. Seu aplicativo usa esses arquivos para controlar seu próprio estado.
* Outros arquivos em locais do sistema de arquivos em que seu aplicativo declarou recursos para acessar. Geralmente, eles estão localizados em uma das [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

Seu aplicativo tem controle total sobre as duas primeiras categorias de arquivos, pois elas fazem parte dos arquivos do pacote do aplicativo e são acessadas exclusivamente pelo aplicativo. Quanto aos arquivos da última categoria, seu aplicativo deve estar ciente de que outros aplicativos e serviços do sistema operacional podem acessar os arquivos simultaneamente.

Dependendo do aplicativo, o acesso aos arquivos pode variar na frequência:

* Muito baixa. Geralmente, esses são arquivos que são abertos uma vez quando o aplicativo é iniciado e são salvos quando o aplicativo é suspenso.
* Baixa. Esses são arquivos em que o usuário está especificamente realizando uma ação (como salvar ou carregar).
* Média ou alta. Esses são arquivos em que o aplicativo deve atualizar dados constantemente (por exemplo, recursos de salvamento automático ou acompanhamento constante de metadados).

Em relação ao tamanho do arquivo, considere os dados de desempenho no gráfico a seguir para o método **WriteBytesAsync**. Este gráfico compara o tempo para concluir uma operação em relação ao tamanho do arquivo, considerando o desempenho médio de 10 mil operações por tamanho de arquivo em um ambiente controlado.

![Desempenho do WriteBytesAsync](images/writebytesasync-performance.png)

Os valores de tempo no eixo y foram omitidos intencionalmente nesse gráfico porque as configurações e hardware diferentes produzirão valores diferentes de tempo absoluto. No entanto, temos observado consistentemente essas tendências em nossos testes:

* Para arquivos muito pequenos (<= 1 MB): O tempo para concluir as operações é consistentemente rápido.
* Para arquivos maiores (> 1 MB): O tempo para concluir as operações começa a aumentar exponencialmente.

## <a name="io-during-app-suspension"></a>E/S durante a suspensão do aplicativo

Seu aplicativo deverá ser projetado para lidar com a suspensão se você quiser manter informações de estado ou metadados para uso em sessões posteriores. Para obter informações básicas sobre a suspensão de aplicativos, consulte [Ciclo de vida do aplicativo](../launch-resume/app-lifecycle.md) e [esta postagem no blog](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

A menos que o sistema operacional conceda execução estendida ao aplicativo, quando seu aplicativo é suspenso, ele tem cinco segundos para liberar todos os recursos e salvar os dados. Para uma melhor confiabilidade e experiência do usuário, sempre presuma que o tempo que você tem para lidar com as tarefas de suspensão é limitado. Tenha em mente as seguintes diretrizes durante o período de tempo de 5 segundos para lidar com tarefas de suspensão:

* Tente manter a E/S ao mínimo para evitar condições de corrida causadas por operações de liberação.
* Evite gravação arquivos que exigem centenas de milissegundos ou mais para gravar.
* Se seu aplicativo usa os métodos **Write**, tenha em mente todas as etapas intermediárias que esses métodos exigem.

Se seu aplicativo opera em uma pequena quantidade de dados de estado durante a suspensão, na maioria dos casos você pode usar os métodos **Write** para liberar os dados. No entanto, se seu aplicativo usa uma grande quantidade de dados de estado, considere usar fluxos para armazenar diretamente seus dados. Isso pode ajudar a reduzir o atraso introduzido pelo modelo transacional dos métodos **Write**. 

Por exemplo, consulte a amostra [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension).

## <a name="other-examples-and-resources"></a>Outros exemplos e recursos

Aqui estão vários exemplos e outros recursos para cenários específicos.

### <a name="code-example-for-retrying-file-io-example"></a>Exemplo de código para repetir a E/S de arquivo

A seguir está um exemplo de pseudocódigo de como repetir uma gravação (C#), supondo que a gravação deve ser feita depois que o usuário seleciona um arquivo para salvar:

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

### <a name="synchronize-access-to-the-file"></a>Sincronizar acesso ao arquivo

O [blog Programação Paralela com .NET](https://devblogs.microsoft.com/pfxteam/) é um excelente recurso para obter diretrizes sobre programação paralela. Em particular, a [postagens sobre AsyncReaderWriterLock](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) descreve como manter o acesso exclusivo a um arquivo para gravações, permitindo o acesso simultâneo de leitura. Tenha em mente que serializar a E/S afetará o desempenho.

## <a name="see-also"></a>Veja também

* [Criar, gravar e ler um arquivo](quickstart-reading-and-writing-files.md)

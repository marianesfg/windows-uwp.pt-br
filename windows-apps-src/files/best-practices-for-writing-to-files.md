---
title: Práticas recomendadas para escrever a arquivos
description: Saiba mais práticas recomendadas para usar o arquivo diversos métodos das classes FileIO e PathIO de gravação.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8bed97e060015f92ff95c9f7d797bbcb83db431
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9115700"
---
# <a name="best-practices-for-writing-to-files"></a>Práticas recomendadas para escrever a arquivos

**APIs importantes**

* [**Classe FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**Classe PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Os desenvolvedores, às vezes, executar em um conjunto de problemas comuns ao usar os métodos de **gravação** das classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) para realizar operações de e/s do sistema de arquivos. Por exemplo, problemas comuns incluem:

• Um arquivo é gravado parcialmente • o aplicativo recebe uma exceção quando chamar um dos métodos. • As operações de deixar para trás. Arquivos TMP com um nome de arquivo semelhante ao nome do arquivo de destino.

Os métodos de **gravação** das classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) incluem o seguinte:

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Este artigo fornece detalhes sobre como esses métodos funcionam para que os desenvolvedores compreendem melhor quando e como usá-los. Este artigo fornece diretrizes e não tenta fornecer uma solução para todos os problemas de e/s de arquivo possíveis. 

> [!NOTE]
> Este artigo se concentra em métodos **FileIO** na exemplos e discussões. No entanto, os métodos **PathIO** seguem um padrão semelhante e a maioria das diretrizes neste artigo se aplica também a esses métodos. 

## <a name="conveience-vs-control"></a>Conveience versus controle

Um objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) não é um identificador de arquivo como o modelo de programação Win32 nativo. Em vez disso, um [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) é uma representação de um arquivo com métodos para manipular o conteúdo.

Noções básicas sobre esse conceito é útil ao executar a e/s com um **StorageFile**. Por exemplo, a seção de [escrita em um arquivo](quickstart-reading-and-writing-files.md#writing-to-a-file) apresenta três maneiras de gravar em um arquivo:

* Usando o método [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) .
* Criando um buffer e, em seguida, chamando o método [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) .
* O modelo de quatro etapas usando um fluxo:
  1. [Abra](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) o arquivo para obter um fluxo.
  2. [Obter](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) um fluxo de saída.
  3. Criar um objeto [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) e chamar o método correspondente de **escrever** .
  4. [Confirmar](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) os dados no gravador de dados e [liberar](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) o fluxo de saída.

Os dois primeiros cenários são os mais comumente usados por aplicativos. A gravação para o arquivo em uma única operação é mais fácil de código e manter e também elimina a responsabilidade do aplicativo do lidar com muitas das complexidades de e/s de arquivo. No entanto, essa conveniência tem um custo: a perda de controle sobre a operação inteira e a capacidade de detectar erros em pontos específicos.

## <a name="the-transactional-model"></a>O modelo transacional

Os métodos de **gravação** das classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) encapsulam as etapas no terceiro modelo gravação descrito acima, com uma camada adicional. Essa camada é encapsulada em uma transação de armazenamento.

Para proteger a integridade do arquivo original, caso algo dê errado ao gravar os dados, os métodos da **gravação** usam um modelo transacional abrindo o arquivo usando [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Esse processo cria um objeto [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) . Depois que esse objeto de transação é criado, as APIs gravam os dados de maneira semelhante a seguir para o exemplo de [Acesso a arquivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) ou o exemplo de código no artigo [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) .

O diagrama a seguir ilustra as tarefas subjacentes realizadas pela o método **WriteTextAsync** em uma operação de gravação bem-sucedida. Esta ilustração fornece uma exibição simplificada da operação. Por exemplo, ele ignora etapas como conclusão de codificação e assíncrona de texto em threads diferentes.

![Diagrama de sequência de chamada de API da UWP para gravar um arquivo](images/file-write-call-sequence.svg)

As vantagens de usar os métodos de **gravação** das classes [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) e [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) em vez do modelo de quatro etapas mais complexo usando um fluxo são:

* Uma chamada de API para manipular todas as etapas intermediárias, incluindo erros.
* O arquivo original é mantido se algo der errado.
* O estado do sistema tentará mantidos mais simples possível.

No entanto, com muitos pontos intermediários de possíveis de falha, há uma probabilidade maior de falha. Quando ocorre um erro pode ser difícil entender onde o processo falhou. As seções a seguir apresentam alguns das falhas, você pode encontrar ao usar os métodos de **escrever** e fornecer as possíveis soluções.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Códigos de erro comuns para os métodos de gravação das classes FileIO e PathIO

Esta tabela apresenta os códigos de erro comuns que os desenvolvedores de aplicativos encontrar ao usar os métodos de **gravação** . As etapas na tabela correspondem às etapas no diagrama anterior.

|  Erro de nome (valor)  |  Etapas  |  Causas  |  Soluções  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  O arquivo original pode ser marcado para exclusão, possivelmente de uma operação anterior.  |  Repita a operação.</br>Certifique-se de que o acesso ao arquivo é sincronizado.  |
|  ERROR_SHARING_VIOLATION (0X80070020)  |  5  |  O arquivo original seja aberto por outra gravação exclusiva.   |  Repita a operação.</br>Certifique-se de que o acesso ao arquivo é sincronizado.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0X80070497)  |  19-20  |  O arquivo original (arquivo. txt) não pode ser substituído porque ele está em uso. Outro processo ou operação obteve acesso ao arquivo antes que ele pode ser substituído.  |  Repita a operação.</br>Certifique-se de que o acesso ao arquivo é sincronizado.  |
|  ERROR_DISK_FULL (0X80070070)  |  7, 14, 16, 20  |  O modelo realizado cria um arquivo extra, e isso consome armazenamento extra.  |    |
|  ERROR_OUTOFMEMORY (0X8007000E)  |  14, 16  |  Isso pode acontecer devido a várias operações de e/s pendentes ou arquivos grandes.  |  Uma abordagem mais detalhada, controlando o fluxo pode resolver o erro.  |
|  E_FAIL (0X80004005) |  Qualquer  |  Diversos  |  Repita a operação. Se ele ainda falhar, talvez seja um erro de plataforma e o aplicativo deve encerrar porque ela está em um estado inconsistente. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Outras considerações para estados de arquivo que podem levar a erros

Além de erros retornados pelos métodos **gravar** , aqui estão algumas diretrizes em que um aplicativo pode esperar ao gravar um arquivo.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Dados foi escritos para o arquivo se e somente se operação concluída

Seu aplicativo não deve fazer qualquer suposição sobre os dados no arquivo enquanto uma operação de gravação está em andamento. Ao tentar acessar o arquivo antes de concluir uma operação pode levar a dados inconsistentes. Seu aplicativo deve ser responsável de rastreamento e/SS pendentes.

### <a name="readers"></a>Leitores

Se o arquivo que está sendo gravados também está sendo usado por um leitor educado (ou seja, aberto com [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode), leituras subsequentes falhará com um erro ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). Às vezes, aplicativos repetir abrir o arquivo para leitura novamente enquanto a operação de **gravação** está em andamento. Isso pode resultar em uma condição de corrida em que, por fim, a **gravação** Falha ao tentar substituir o arquivo original, porque ele não pode ser substituído.

### <a name="files-from-knownfolders"></a>Arquivos de KnownFolders

Seu aplicativo pode não estar somente aplicativo que está tentando acessar um arquivo que reside em qualquer um do [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). Não há nenhuma garantia de que, se a operação for bem-sucedida, o conteúdo de que um aplicativo escrito para o arquivo permanecerá constante na próxima vez que ele tentar ler o arquivo. Além disso, o compartilhamento ou acesso negado erros se tornam mais comuns sob esse cenário.

### <a name="conflicting-io"></a>Conflitantes de e/s

A probabilidade de erros de simultaneidade pode ser reduzida se nosso aplicativo usa os métodos de **gravação** de arquivos em seus dados locais, mas alguns cuidado ainda é necessário. Se várias operações de **gravação** estão sendo enviadas ao arquivo simultaneamente, não há nenhuma garantia sobre quais dados acabam no arquivo. Para atenuar isso, recomendamos que seu aplicativo serializa operações de **gravação** para o arquivo.

### <a name="tmp-files"></a>~ Arquivos de TMP

Ocasionalmente, se a operação será cancelada forçada (por exemplo, se o aplicativo foi suspenso ou encerrado pelo sistema operacional), a transação não for confirmada ou fechada adequadamente. Isso pode deixar para trás arquivos com um (. ~ TMP) extensão. Considere a possibilidade de excluir esses arquivos temporários (se existirem nos dados locais do aplicativo) ao manipular a ativação do aplicativo.

## <a name="considerations-based-on-file-types"></a>Considerações com base nos tipos de arquivo

Alguns erros podem se tornar mais predominantes dependendo do tipo de arquivos, a frequência em que eles são acessados e o tamanho do arquivo. Em geral, há três categorias de arquivos que seu aplicativo pode acessar:

* Arquivos criados e editados pelo usuário na pasta de dados local do seu aplicativo. Eles são criados e editados somente enquanto estiver usando seu aplicativo, e existem apenas dentro do aplicativo.
* Metadados do aplicativo. Seu aplicativo usa esses arquivos para acompanhar seu próprio estado.
* Outros arquivos em locais do sistema de arquivos onde o aplicativo declarou recursos para acessar. Mais comumente, elas estão localizadas em um do [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

Seu aplicativo tem controle total sobre as duas primeiras categorias de arquivos, porque eles são parte dos arquivos do pacote do aplicativo e são acessados pelo seu aplicativo exclusivamente. Para arquivos na última categoria, seu aplicativo deve estar ciente de que outros aplicativos e serviços do sistema operacional podem ser acessando os arquivos simultaneamente.

Dependendo do aplicativo, o acesso aos arquivos pode variar frequência:

* Muito baixa. Geralmente, esses são os arquivos abertos depois quando as inicializações de aplicativo e são salvas quando o aplicativo está suspenso.
* Baixa. Esses são os arquivos que o usuário é especificamente realizando uma ação (como salvar ou carregar).
* Médio ou alto. Esses são os arquivos em que o aplicativo deve atualizar constantemente dados (por exemplo, os recursos de gravação automática ou constante metadados de controle).

Tamanho do arquivo, considere os dados de desempenho no gráfico a seguir para o método **WriteBytesAsync** . Este gráfico compara o tempo para concluir um tamanho de arquivo do vs operação, ao longo de um desempenho médio de 10000 operações por tamanho de arquivo em um ambiente controlado.

![WriteBytesAsync desempenho](images/writebytesasync-performance.png)

Os valores de tempo no eixo y são omitidos intencionalmente este gráfico porque configurações e um hardware diferente produzirá valores absolutos de tempo diferente. No entanto, temos consistentemente observado essas tendências em nossos testes:

* Para arquivos muito pequenos (< = 1 MB): O tempo para concluir as operações é consistentemente rápido.
* Para arquivos maiores ( gt _ 1 MB): O tempo para concluir as operações começa a aumentar exponencialmente.

## <a name="io-during-app-suspension"></a>E/s durante a suspensão do aplicativo

Seu aplicativo deve projetado para tratar a suspensão se você deseja manter informações de estado ou metadados para uso em sessões posteriores. Para obter informações detalhadas sobre a suspensão do aplicativo, consulte o [ciclo de vida do aplicativo](../launch-resume/app-lifecycle.md) e [esta postagem no blog](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

A menos que o sistema operacional concede a execução estendida para seu aplicativo, quando seu aplicativo é suspenso, ele tem cinco segundos para liberar todos os seus recursos e salvar seus dados. Para a confiabilidade e o usuário a melhor experiência, sempre presumir que o tempo que você precisa manipular tarefas de suspensão é limitado. Tenha em mente as seguintes diretrizes durante os período de tempo para lidar com tarefas de suspensão de 5 segundos:

* Tente manter e/s em um mínimo para evitar condições de corrida causadas por operações de liberação e liberação.
* Evite a gravação de arquivos que exigem centenas de milissegundos ou mais escrever.
* Se seu aplicativo usa os métodos de **escrever** , lembre-se todas as etapas intermediárias que exigem esses métodos.

Se seu aplicativo opera em uma pequena quantidade de dados de estado durante a suspensão, na maioria dos casos você pode usar os métodos de **gravação** para liberar os dados. No entanto, se seu aplicativo usa uma grande quantidade de dados de estado, considere usar fluxos diretamente armazenar os dados. Isso pode ajudar a reduzir o atraso introduzido pelo modelo de transacional dos métodos **de gravação** . 

Por exemplo, veja a amostra de [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) .

## <a name="other-examples-and-resources"></a>Outros exemplos e recursos

Aqui estão vários exemplos e outros recursos para cenários específicos.

### <a name="code-example-for-retrying-file-io-example"></a>Exemplo de código para repetindo o exemplo de arquivo e/s

Este é um exemplo de código pseudo sobre como repetir uma gravação (c#), supondo que a gravação seja ser feito após o usuário seleciona um arquivo para salvar:

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

### <a name="synchronize-access-to-the-file"></a>Sincronizar o acesso ao arquivo.

A [Programação paralela com .NET blog](https://blogs.msdn.microsoft.com/pfxteam/) é um ótimo recurso para obter orientações sobre a programação paralela. Em particular, a [postagem sobre AsyncReaderWriterLock](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) descreve como manter acesso exclusivo a um arquivo para gravações enquanto permite acesso de leitura simultâneo. Tenha em mente serializar que e/s afetará desempenho.

## <a name="see-also"></a>Consulte também

* [Criar, gravar e ler um arquivo](quickstart-reading-and-writing-files.md)

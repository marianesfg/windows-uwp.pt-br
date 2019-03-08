---
title: Gerenciar o armazenamento local conectado
description: Saiba como gerenciar dados locais de armazenamento conectado em um ambiente de desenvolvimento.
ms.assetid: 630cb5fc-5d48-4026-8d6c-3aa617d75b2e
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, armazenamento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 09fce637c50b0a03230d0808e51a9e5cfadc7179
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642051"
---
# <a name="managing-local-connected-storage"></a>Gerenciar o armazenamento local conectado
Enquanto conectado de armazenamento é usado para armazenar seus dados de jogos na nuvem, também há um componente de armazenamento local para o serviço de armazenamento conectados. Se você estiver usando um PC ou console há um cache local dos dados do armazenamento conectado que contém os dados sincronizados na nuvem. Se você estiver criando um título do XDK ou UWP há uma ferramenta para permitir que você gerencie seus dados locais de armazenamento conectados.

Consulte a tabela a seguir para localizar a ferramenta apropriada para gerenciar seus dados de armazenamento conectado localmente em cache:

|Classificação de título  |Dispositivo  |Ferramenta de armazenamento local  |
|---------|---------|---------|
|XDK     |Console do Xbox One     |*xbstorage*         |
|UWP     |Computador         |*gamesaveutil*         |
|UWP     |Console do Xbox One     |Portal de dispositivo do Xbox (XDP |) simples

- *Xbstorage* é uma ferramenta de linha de comando, executar do prompt de comando XDK, para gerenciar armazenada em cache localmente conectado de armazenamento em um Console Xbox. O *xbstorage* ferramenta pode ser encontrada no Xbox XDK um sob o caminho do arquivo: **/Program arquivos (x86)/Microsoft Durango XDK/bin/xbstorage.exe**
- *Gamesaveutil* é uma ferramenta de linha de comando para gerenciar UWP localmente em cache armazenamento conectado no PC. O *gamesaveutil* ferramenta vem com o [SDK do Windows 10](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) para Fall Creators Update e posterior (build 10.0.16299.15 e posterior). Depois que você instalou a versão apropriada do SDK do Windows 10 *gamesaveutil* podem ser encontrados na pasta: **ProgramFiles(x86)/Windows Kits/10/bin/[SDK Version]/x64/gamesaveutil.exe**.
- *Xbox dispositivo Portal(XDP)* é um portal online que permite que você gerencie os dados de UWP de armazenamento conectado localmente em cache em seu Xbox, um único Console. Este artigo não aborda o uso XDP. Para aprender a usar XDP leia [Portal do dispositivo para Xbox](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-xbox).

## <a name="xbstorage"></a>Xbstorage

*Xbstorage* permite que a limpeza de dados armazenados em cache localmente conectado de armazenamento de dados do disco rígido, bem como importação e exportação de dados para usuários ou computadores de espaços de armazenamento conectados por meio de arquivos XML.

Quando uma operação é executada nos dados locais usando o *xbstorage* ferramenta, o sistema se comportará como se essa operação tinha sido realizada pelo próprio aplicativo, portanto, fará com que o ato de lendo os dados de um espaço de armazenamento conectados em um arquivo local sincronização com a nuvem antes de copiar.

Da mesma forma, uma cópia de dados de um arquivo XML em que o computador de desenvolvimento para um contêiner de armazenamento conectados no kit de desenvolvimento de Xbox One fará com que o console iniciar o upload de dados para a nuvem. No entanto, existem condições em que isso não ocorrerá: se o kit de desenvolvimento não é possível adquirir o bloqueio ou se houver um conflito de dados entre os contêineres no console e aqueles na nuvem, o console irá se comportar como se o usuário tenha decidido não resolver o conflito por Escolher uma versão do contêiner serão mantidos e o console irá se comportar como se o usuário está reproduzindo off-line até a próxima vez em que o título é iniciado.

É a única exceção a esses comandos **redefina** **/Force** que limpa o armazenamento local de dados salvos para todos os usuários e SCIDs, mas não altera os dados armazenados na nuvem. Isso é útil para colocar um console para o estado que estaria em se um usuário foi roaming para um console e baixar dados de nuvem após a execução de um título.

### <a name="xbstorage-commands"></a>Comandos de Xbstorage

Xbstorage tem as seguintes desenvolvedores seis comandos podem usar com o prompt de comando do XDK para gerenciar dados locais em seu Xbox um Kit de desenvolvimento:

<a id="xbstorage_reset"></a>

|Comando  |Descrição  |
|---------|---------|
|Redefinir    |Executa uma redefinição de fábrica no armazenamento conectado.         |
|importar   |Importa dados do arquivo XML especificado para um espaço de armazenamento conectados.         |
|Exportar   |Exporta dados de um espaço de armazenamento conectados ao arquivo XML especificado.         |
|excluir   |Exclui dados de um espaço de armazenamento conectados.         |
|Gerar |Gera dados fictícios e salva o arquivo XML especificado.         |
|Simular |Simula condições de espaço de armazenamento insuficiente.         |

### <a name="xbstorage-reset"></a>Redefinição de Xbstorage

`xbstorage reset [/force]`

Apaga todos os dados locais no armazenamento conectado a partir do console local, restaurá-lo para as configurações de fábrica. Dados que tem sido persistidos para a nuvem não são modificados e serão baixados novamente conforme necessário.

|Opção  |Descrição  |
|---------|---------|
|/force   |Confirma que o armazenamento conectado deve ser redefinido. Executando o comando reset sem especificar **/Force** faz com que a seguinte mensagem a ser exibida:   Como alocador de armazenamento conectado a redefinição é uma operação potencialmente destrutiva esse comando não executa a redefinição, a menos que o **/Force** sinalizador estiver presente. |

<a id="xbstorage_import"></a>

### <a name="xbstorage-import"></a>Importação de Xbstorage

`xbstorage import *file-name* [/scid:*SCID*] [/machine] [/msa:*account*] [/replace] [/verbose]`

Importa os dados especificados na *filename* para um espaço de armazenamento conectados.
O arquivo é um arquivo XML que contém os dados. Por exemplo, consulte [xbstorage gerar](#xbstorage_generate). Para obter mais informações sobre o formato XML do arquivo, consulte [formato de arquivo de importação e exportação](#xbstorage_fileformat), mais adiante neste tópico.
Há duas maneiras para especificar o espaço de armazenamento conectado:

- Se o arquivo de entrada tem um **ContextDescription** seção que é preenchida corretamente, isso será usado para especificar o espaço de armazenamento conectados.
- O espaço de armazenamento também pode ser parcialmente ou totalmente especificado por meio de parâmetros de linha de comando, que têm precedência sobre os respectivos elementos do espaço de armazenamento especificado no arquivo de entrada.

Exemplos de uso:

```cmd
  xbstorage import mydata.xml
  xbstorage import mydata.xml /replace
  xbstorage import mydata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /verbose 
```

> [!NOTE]
> Antes de importar para o espaço de armazenamento conectados especificado, o sistema tentará sincronizar com a nuvem usando a mesma lógica que é executado quando um espaço de armazenamento conectado é adquirido por um aplicativo em execução.
>
> Se um aplicativo com o mesmo SCID primário estiver em execução, essa operação pode causar uma condição de corrida e o conteúdo do espaço de armazenamento conectados poderia ser em um estado indeterminado.
>
> Se **/substituir** não for especificado, os contêineres especificados no arquivo de entrada serão apagados antes de gravar os blobs especificados no arquivo de entrada. Contêineres no espaço de armazenamento conectados não especificado no arquivo de entrada permanecerá inalterados.

|Opção  |Descrição  |
|---------|---------|
|*file-name*     |Especifica um arquivo XML que contém os dados a serem importados.         |
|/scid:*SCID*    |Especifica o identificador de configuração de serviço (SCID).         |
|/machine        |Especifica um espaço de armazenamento conectados por máquina.  Essa opção não pode ser usada simultaneamente com o **/msa** opção.         |
|/msa:*account*  |Especifica uma conta a ser usado para armazenamento de cada usuário conectado. O usuário deve ser conectado ao console para o espaço a ser usado.  Essa opção não pode ser usada simultaneamente com o **/máquina** opção.         |
|/replace        |Exclui todos os contêineres no espaço de armazenamento conectados especificado antes de importar.         |
|/verbose        |Exibe o status de importação.         |


 <a id="xbstorage_export"></a>

### <a name="xbstorage-export"></a>Exportação de Xbstorage

`xbstorage export *outfile* [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

Exporte dados de um espaço de armazenamento conectado para o arquivo especificado por **outfile**.    O arquivo é um arquivo XML que contém os dados. Ver [xbstorage gerar](#xbstorage_generate) para saber como gerar um exemplo. Para obter mais informações sobre o formato XML do arquivo, consulte [formato de arquivo de importação e exportação](#xbstorage_fileformat), mais adiante neste tópico. Há duas maneiras para especificar o espaço de armazenamento conectado:

- Se o **/context** parâmetro é usado e o nome do arquivo especificado por \<infile > tem um **ContextDescription** seção que é preenchida corretamente, esse arquivo será usado para especificar o Espaço de armazenamento conectado.
- O espaço de armazenamento também pode ser parcialmente ou totalmente especificado por meio de parâmetros de linha de comando, que têm precedência sobre os respectivos elementos do espaço de armazenamento especificada na **/context** arquivo.

Exemplos de uso:

```cmd
  xbstorage export exporteddata.xml /context:space.xml
  xbstorage export exporteddata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /context:space.xml /verbose
```

> [!NOTE]
> Antes de exportar o espaço de armazenamento conectados especificado, o sistema tentará sincronizar com a nuvem usando a mesma lógica que é executado quando um espaço de armazenamento conectado é adquirido por um aplicativo em execução.
>
> Se um aplicativo com o mesmo SCID primário estiver em execução, essa operação pode causar uma condição de corrida e o conteúdo do espaço de armazenamento conectados poderia ser em um estado indeterminado.

|Opção  |Descrição  |
|---------|---------|
|*outfile*             |Arquivo XML os dados será exportado para.         |
|/context:*input-file* |Especifica um arquivo de entrada do qual ler as informações de espaço.         |
|/scid:*SCID*          |Especifica o identificador de configuração de serviço (SCID).         |
|/machine              |Especifica um espaço de armazenamento conectados por máquina.  Essa opção não pode ser usada simultaneamente com o **/msa** opção.         |
|/msa:*account*        |Especifica uma conta a ser usado para armazenamento de cada usuário conectado. O usuário deve ser conectado ao console para o espaço a ser usado.  Essa opção não pode ser usada simultaneamente com o **/máquina** opção.         |
|/verbose              |Exibe o status da operação de exportação.         |

<a id="xbstorage_delete"></a>

### <a name="xbstorage-delete"></a>Excluir Xbstorage

`xbstorage delete [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

Exclui todos os dados de um espaço de armazenamento conectados.
Há duas maneiras para especificar o espaço de armazenamento conectado:

- Se o **/context** parâmetro é usado e o nome do arquivo especificado por \<infile > tem um **ContextDescription** seção que é preenchida corretamente, esse arquivo será usado para especificar o Espaço de armazenamento conectado.
- O espaço de armazenamento também pode ser parcialmente ou totalmente especificado por meio de parâmetros de linha de comando, que têm precedência sobre os respectivos elementos do espaço de armazenamento especificada na **/context** arquivo.

Exemplos de uso:

```cmd
  xbstorage delete /context:space.xml
  xbstorage delete /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /context:space.xml /verbose
```

> [!NOTE]
> Antes de excluir o espaço de armazenamento conectados especificado, o sistema tentará sincronizar com a nuvem usando a mesma lógica que é executado quando um espaço de armazenamento conectado é adquirido por um aplicativo em execução.
>> Se um aplicativo com o mesmo SCID primário estiver em execução, essa operação pode causar uma condição de corrida e o conteúdo do espaço de armazenamento conectados poderia ser em um estado indeterminado.

|Opção  |Descrição |
|---------|---------|
|/context:*input-file*     |Especifica um arquivo de entrada do qual ler as informações de espaço.         |
|/scid:*SCID*              |Especifica o identificador de configuração de serviço (SCID).         |
|/machine                  |Especifica um espaço de armazenamento conectados por máquina.  Essa opção não pode ser usada simultaneamente com o **/msa** opção.         |
|/msa:*account*            |Especifica uma conta a ser usado para armazenamento de cada usuário conectado. O usuário deve ser conectado ao console para o espaço a ser usado.  Essa opção não pode ser usada simultaneamente com o **/máquina** opção.         |
|/verbose                  |Exibe o status da operação de exclusão.         |

 <a id="xbstorage_generate"></a>

### <a name="xbstorage-generate"></a>Gerar Xbstorage

`xbstorage generate *file-name* [/containers:*n*] [/blobs:*n*] [/blobsize:*n*]`

Gera dados fictícios e salva em um arquivo especificado por \<filename >. Para obter mais informações sobre o formato XML do arquivo, consulte [formato de arquivo de importação e exportação](#xbstorage_fileformat), mais adiante neste tópico.    O identificador de configuração de serviço (SCID) será definido como 00000000-0000-0000-0000-000000000000 e a conta será definida para um espaço de armazenamento conectados por máquina. Se você quiser alterar esses valores, você pode editar o arquivo diretamente após ele ser gerado.

Exemplos de uso:

```cmd
  xbstorage generate dummydata.xml
  xbstorage generate dummydata.xml /containers:4
  xbstorage generate dummydata.xml /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```

> [!NOTE]
> Os dados de bytes são uma sequência crescente simple; Por exemplo, um blob de cinco bytes teria os bytes a seguir: 00 01 02 03 04. >> Se você quiser especificar um espaço de armazenamento conectados por usuário, alterar o **conta** nó no arquivo XML para algo semelhante ao seguinte:
>  ```
>    <Account msa="user@domain.com"/>
>  ```

|Opção  |Descrição  |
|---------|---------|
|*file-name*     | Arquivo XML de dados será gravado. |
|/containers:*n* | Especifica o número *n*, de contêineres para gerar. A contagem padrão é 2.  |
|/blobs:*n*      | Especifica o número *n*, de blobs para gerar. A contagem padrão é 3.  |
|/blobsize:*n*   | Especifica o número *n*, de bytes por blob. O tamanho padrão é 1024 bytes.  |

 <a id="xbstorage_simulate"></a>

### <a name="xbstorage-simulate"></a>Simular Xbstorage

`xbstorage simulate [/reserveremainingspace] [/forceoutoflocalstorage] [/stop] [/verbose]`

Simula fora de condições de armazenamento local para o serviço de armazenamento conectados.

|Opção  |Descrição  |
|---------|---------|
|/reserveremainingspace | Reserva todo o espaço restante no armazenamento conectado. Excluindo algo de ConnectedStorage abrirá o espaço que você pode usar. |

| / forceoutoflocalstorage | Simula o serviço de armazenamento conectados não ter nenhum espaço disponível para a esquerda. Excluindo algo de armazenamento conectados não alterará o serviço de armazenamento conectados do relatório de memória insuficiente. |

| / parar | Interrompe todas as simulações. |

| /verbose | Exibe o status da operação simulado. |

 <a id="ID4E4MAC"></a>

### <a name="common-options"></a>Opções comuns

`xbstorage [/?] [/X*:address* [*+accesskey*] ]`

|Opção  |Descrição  |
|---------|---------|
| /?                           |  Exibe a Ajuda para xbstorage.exe |
| /X *:address* [*+accesskey*]  | Especifica o nome do host ou endereço (mostrado como **IP ferramentas** no console) de um console de destino, mas não altera o padrão de console. Se você não usar essa opção, o console padrão será usado. *Accesskey* é uma cadeia de caracteres que você pode usar para restringir o acesso a um console somente às pessoas que conhecem a chave de acesso. Defina a chave de acesso usando o comando **xbconfig** **accesskey = * * * sua chave*; em seguida, reinicie o console para efetivar a chave de acesso. Para acessar um console que está configurado com uma chave de acesso, você deve incluir um sinal de adição (+) e a chave de acesso após o nome de host ou endereço IP do console.
> [!NOTE]
> Se uma chave de acesso for fornecida quando o console padrão é definido por xbconnect, em seguida, a chave de acesso é armazenada como parte do endereço do console do padrão.
|

## <a name="gamesaveutil"></a>Gamesaveutil

*Gamesaveutil* permite que você gerencie o armazenamento em cache localmente conectado para funções de seu aplicativo com todos os mesmos *xbstorage* fornece. Assim como a ferramenta de xbstorage *gamesaveutil* oferece os mesmos dados de seis funções, com algumas diferenças no comportamento de gerenciamento. A importação, exportação, excluir e comandos de redefinição exigem um usuário do Xbox Live entrar. Você pode usar o App Xbox no Windows 10 para exibir e alterar o usuário atual.

### <a name="commands"></a>Comandos

|Comando  |Descrição  |
|---------|---------|
|importar   |Importa dados do arquivo XML especificado         |
|Exportar   |Exporta dados para o arquivo xml especificado         |
|excluir   |Exclui os dados do aplicativo especificado        |
|Redefinir    |Exclui os dados locais somente para o aplicativo especificado         |
|Gerar |gera dados fictícios e salva no arquivo de xml especificado.         |
|Simular |simula condições especiais que são difíceis de testar         |

### <a name="gamesaveutil-import"></a>Importação de Gamesaveutil

`gamesaveutil import <filename> [/pfn:<PFN>] [/scid:<SCID>] [/replace]`

Importa os dados especificados na \<filename >

O arquivo é um arquivo XML que contém os dados. Tipo `gamesaveutil help generate` para saber como gerar um exemplo.

Há duas maneiras para especificar o aplicativo PFN e SCID onde os dados são salvos:

Se o arquivo de entrada tem uma seção de ContextDescription é preenchida corretamente, isso será usado para especificar o aplicativo de destino PFN e SCID.

O PFN e SCID podem ser parcialmente ou totalmente especificado por meio de parâmetros de linha de comando, que têm precedência sobre os respectivos elementos do PFN especificado e SCID do arquivo de entrada.

|Opção  |Descrição  |
|---------|---------|
|/pfn:\<PFN>       |Especifica a família de pacote Name(PFN) para o aplicativo para executar a importação para. O aplicativo deve ser instalado.         |
|/scid:\<SCID>     |Especifica o identificador de configuração de serviço (SCID). Isso é de sua configuração do Xbox Live.         |
|/replace         |Exclua todos os contêineres antes de executar a importação.         |

Usos de exemplo:

```cmd
gamesaveutil import mydata.xml
gamesaveutil import mydata.xml /replace
gamesaveutil import mydata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> O aplicativo deve ser instalado e sincronização de dados já para importar.
> 
> Se /replace não for especificado, os contêineres existentes não são alterados, a menos que elas são especificadas no arquivo de entrada.

### <a name="gamesaveutil-export"></a>Exportação de Gamesaveutil

`gamesaveutil export <outfile> [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Exporta dados para o arquivo especificado por \<outfile >.

O arquivo é um arquivo XML que contém os dados. Digite help gamesaveutil gerar para saber como gerar um exemplo.

Há duas maneiras para especificar o local dos dados para exportar:

Se o parâmetro /context é usado e o nome do arquivo especificado por \<infile > tem uma seção de ContextDescription que é preenchida corretamente, esse arquivo será usado para especificar o local da fonte de dados.

O local também pode ser especificado por meio de parâmetros de linha de comando, que têm precedência sobre os respectivos elementos especificados pelo arquivo /context.

|Opção  |Descrição  |
|---------|---------|
|/context:\<infile>     |Use o arquivo especificado para especificar o aplicativo PFN e SCID.         |
|/pfn:\<PFN>            |Especifica a família de pacote Name(PFN) para o aplicativo para realizar a exportação de. O aplicativo deve ser instalado.       |
|/scid:\<SCID>          |Especifica o identificador de configuração de serviço (SCID). Isso é de sua configuração do Xbox Live.        |

Usos de exemplo:

```cmd
gamesaveutil export exporteddata.xml /context:target.xml
gamesaveutil export exporteddata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> O aplicativo deve ser instalado e sincronização de dados já para exportar.

### <a name="gamesaveutil-delete"></a>Excluir Gamesaveutil

`gamesaveutil delete [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Exclui todos os dados para o PFN e SCID especificados.

Há duas maneiras para especificar o local dos dados para excluir:

Se o parâmetro /context é usado e o nome do arquivo especificado por \<infile > tem uma seção de ContextDescription que é preenchida corretamente, esse arquivo será usado para especificar o local da fonte de dados.

O local também pode ser especificado por meio de parâmetros de linha de comando, que têm precedência sobre os respectivos elementos especificados pelo arquivo /context.

|Opção  |Descrição  |
|---------|---------|
|/context:\<infile>     |Use o arquivo especificado para especificar o aplicativo PFN e SCID.         |
|/pfn:\<PFN>            |Especifica a família de pacote Name(PFN) para o aplicativo para excluir os dados do. O aplicativo deve ser instalado.       |
|/scid:\<SCID>          |Especifica o identificador de configuração de serviço (SCID). Isso é de sua configuração do Xbox Live.        |

Usos de exemplo:

```cmd
gamesaveutil delete /context:target.xml
gamesaveutil delete /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> O aplicativo deve ser instalado para excluir os contêineres.

### <a name="gamesaveutil-reset"></a>Redefinição de Gamesaveutil

`gamesaveutil reset [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Exclui todos os dados locais para o PFN e SCID especificados.

Há duas maneiras para especificar o local dos dados para excluir:

Se o parâmetro /context é usado e o nome do arquivo especificado por \<infile > tem uma seção de ContextDescription que é preenchida corretamente, esse arquivo será usado para especificar o local da fonte de dados.

O local também pode ser especificado por meio de parâmetros de linha de comando, que têm precedência sobre os respectivos elementos especificados pelo arquivo /context.

|Opção  |Descrição  |
|---------|---------|
|/context:\<infile>     |Use o arquivo especificado para especificar o aplicativo PFN e SCID.         |
|/pfn:\<PFN>            |Especifica a família de pacote Name(PFN) para o aplicativo para excluir os dados do. O aplicativo deve ser instalado.       |
|/scid:\<SCID>          |Especifica o identificador de configuração de serviço (SCID). Isso é de sua configuração do Xbox Live.        |

Usos de exemplo:

```cmd
gamesaveutil reset /context:target.xml
gamesaveutil reset /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> O aplicativo deve ser instalado para excluir dados locais.

### <a name="gamesaveutil-generate"></a>Gerar Gamesaveutil

`gamesaveutil generate <filename> [/containers:<n>] [/blobs:<n>] [/blobsize:<n>]`

Gera dados fictícios e salva em um arquivo especificado por \<filename >.
O identificador de configuração de serviço (SCID) será definido como 00000000-0000-0000-0000-000000000000. Edite o arquivo manualmente após a geração de alterar esses valores se desejar.

|Opção  |Descrição  |
|---------|---------|
|/containers:\<n>     |Especifica quantos contêineres para gerar. O padrão é 2.         |
|/blobs:\<n>          |Especifica quantos blobs por contêiner para gerar. O padrão é 3.        |
|/blobsize:\<n>       |Especifica o número de bytes por blob. O padrão é 1024.         |

Usos de exemplo:

```cmd
gamesaveutil generate dummydata.xml
gamesaveutil generate dummydata.xml /containers:4
gamesaveutil generate dummydata.xml /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```


> [!NOTE]
> Os dados de bytes são uma simple sequência em ordem crescente, ou seja, um blob de byte cinco teria os bytes 00 01 02 03 04.

### <a name="gamesaveutil-simulate"></a>Simular Gamesaveutil

`gamesaveutil simulate [/markcontainerschanged] [/stop]`

Simula condições especiais para certos cenários de teste.

|Opção  |Descrição  |
|---------|---------|
|/markcontainerschanged     |Força todos os contêineres se pareçam com eles foram alterados quando um aplicativo sai do modo de suspensão e obtém um novo provedor. Afeta todos os aplicativos até limpo com /stop.      |
|/stop                      |Interrompe todas as simulações.         |


 <a id="xbstorage_fileformat"></a>

## <a name="import-and-export-file-format"></a>Formato de arquivo de importação e exportação

O arquivo XML usado com o **importação**, **exportar**, e **gerar** comandos com o *xbstorage* ferramenta tem o formato mostrado no exemplo a seguir.

```xml
<?xml version="1.0" encoding="UTF-8"?>
  <XbConnectedStorageSpace>
    <ContextDescription>
      <Account msa="user@domain.com" />
      <Title scid="00000000-0000-0000-0000-000000000000" />
    </ContextDescription>
    <Data>
      <Containers>
        <Container name="Container1" displayName="Optional Display Name">
          <Blobs>
            <Blob name="Blob1">
              <![CDATA[... ] ]>
            </Blob>
            ...
            <Blob name="BlobN">
              <![CDATA[... ] ]>
            </Blob>
          </Blobs>
        </Container>
        ...
        <Container name="ContainerN">
        ...
        </Container>
      </Containers>
    </Data>
  </XbConnectedStorageSpace>
```

A única alteração necessária para formatar esse xml para **importação**, **exportar**, e **gerar** na *gamesaveutil* é substituir o \<Conta do > nó de membro do \<ContextDescription > nó com um \<PackageFamilyName > nó.
Isso alterará o \<ContextDescription > nó desta:

```xml
<ContextDescription>
    <Account msa="user@domain.com" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

Para isso:

```xml
<ContextDescription>
    <PackageFamilyName pfn="MyGame_xxxxxx" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

> [!NOTE]
> O formato dos dados nesses arquivos XML não é idêntico ao que está na plataforma, ele é para importar e exportar apenas a fins. O formato de dados para esses arquivos XML pode mudar no futuro, portanto, eles devem ser tratados como um formato intermediário ou o utilitário, não um formato de arquivamento.

O **ContextDescription** nó é opcional. Se você estiver fazendo um espaço de armazenamento conectado a uma máquina, você pode usar `<Account machine="true"/>` em vez de `<Account msa="user@domain.com"/>`. Caso contrário, o contexto pode ser especificado na linha de comando para importação.
BLOBs e contêineres devem ter os nomes correspondentes concedidos a eles no jogo ou aplicativo para o qual o arquivo está sendo gerado.
O conteúdo de cada blob deve ser uma cadeia de caracteres encapsulada em um **CDATA** marca, que é gerada chamando **CryptBinaryToStringW** com o sinalizador **CRYPT_STRING_BASE64** fornece os dados desse blob como uma matriz de bytes brutos. Por outro lado, os dados de blob podem ser convertidos chamando **CryptStringToBinary** e fornecendo a cadeia de caracteres anteriormente criptografada. Um exemplo de como usar essas duas funções é mostrado na [CryptBinaryToString retorna bytes inválidos](https://social.msdn.microsoft.com/Forums/vstudio/en-US/9acac904-c226-4ae0-9b7f-261993b9fda2/cryptbinarytostring-returns-invalid-bytes?forum=vcgeneral) nos fóruns do MSDN para Visual Studio.

<a id="ID4EWBAE"></a>
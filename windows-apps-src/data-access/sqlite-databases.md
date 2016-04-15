---
ms.assetid: 5A47301A-2291-4FC8-8BA7-55DB2A5C653F
title: Bancos de dados SQLite
description: SQLite é um mecanismo de banco de dados sem servidor inserido. Este artigo explica como usar a biblioteca SQLite incluída no SDK, empacotar a própria biblioteca SQLite em um aplicativo Universal do Windows ou compilá-lo desde a origem.
---
# Bancos de dados SQLite

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


SQLite é um mecanismo de banco de dados sem servidor inserido. Este artigo explica como usar a biblioteca SQLite incluída no SDK, empacotar a própria biblioteca SQLite em um aplicativo Universal do Windows ou compilá-lo desde a origem.

## O que é SQLite e quando usá-lo

SQLite é um banco de dados sem servidor, inserido e de software livre. Ao longo dos anos, ele surgiu como a tecnologia no lado do dispositivo dominante para o armazenamento de dados em vários dispositivos e plataformas. A Plataforma Universal do Windows (UWP) dá suporte a e recomenda SQLite para armazenamento local em todas as famílias de dispositivos com o Windows 10.

SQLite é mais indicado para aplicativos de telefone, aplicativos inseridos para Windows 10 IoT Core (IoT Core) e como um cache para dados de servidor de banco de dados de relações corporativas (RDBS). Ele atenderá à maior parte das necessidades de acesso a dados locais, a menos que eles acarretem gravações simultâneas pesadas ou cenários de escala de Big Data - improváveis para a maioria dos aplicativos.

Em aplicativos de jogos e reprodução de mídia, o SQLite também pode ser usado como um formato de arquivo para armazenar catálogos ou outros ativos, como níveis de um jogo, que podem ser baixados como são em um servidor Web.

## Adicionando SQLite a um projeto de aplicativo UWP

Existem três maneiras de adicionar SQLite a um projeto UWP.

1.  [Como usar o SDK SQLite](#using-the-sdk-sqlite)
2.  [Incluindo SQLite no pacote do aplicativo](#including-sqlite-in-the-app-package)
3.  [Compilando SQLite pela fonte no Visual Studio](#building-sqlite-from-source-in-visual-studio)

### Como usar o SDK SQLite

Convém usar a biblioteca SQLite incluída no SDK da UWP para reduzir o tamanho do pacote do aplicativo e contar com a plataforma para atualizar a biblioteca periodicamente. Usar o SDK do SQLite também pode levar a vantagens em termos de desempenho, como tempos de inicialização mais rápidos porque a biblioteca SQLite já deve estar carregada na memória a ser usada pelos componentes do sistema.

Para referenciar o SDK do SQLite, inclua o cabeçalho a seguir no projeto. O cabeçalho também contém a versão do SQLite compatível com a plataforma.

`#include <winsqlite/winsqlite3.h>`

Configure o projeto para vincular a winsqlite3.lib. Em **Gerenciador de Soluções**, clique com botão direito em seu projeto e selecione **Propriedades** &gt; **Vinculador** &gt; **Entrada**, em seguida, adicione winsqlite3.lib a **Dependências Adicionais**.

### 2. Inclusão de SQLite no pacote do aplicativo

Às vezes, você talvez queira a própria biblioteca, em vez de usar a versão do SDK, por exemplo, talvez queira usar uma determinada versão dele nos clientes de plataforma cruzada diferente da versão do SQLite incluída no SDK.

Instale a biblioteca SQLite na extensão do Visual Studio da Plataforma Universal do Windows disponível em SQLite.org ou por meio da ferramenta Extensões e Atualizações.

![Tela de Extensões e Atualizações](./images/extensions-and-updates.png)

Depois que a extensão é instalada, faça referência ao arquivo de cabeçalho a seguir no código.

`#include <sqlite3.h>`

### 3. Compilação de SQLite pela fonte no Visual Studio

Às vezes, você talvez queira compilar o próprio binário de SQLite para usar [diversas opções de compilador](http://www.sqlite.org/compile.html) a fim de reduzir o tamanho do arquivo, melhorar o desempenho da biblioteca ou personalizar o recurso definido para o aplicativo. SQLite oferece opções de configuração da plataforma, definindo valores de parâmetro padrão, definindo limites de tamanho, controlando características operacionais, habilitando recursos normalmente desativados, desativando recursos normalmente ativados, omitindo recursos, habilitando a análise e depuração, além de gerenciar o comportamento da alocação de memória no Windows.

*Adição de origem a um projeto do Visual Studio*

O código-fonte de SQLite está disponível para download na [Página de download SQLite.org](https://www.sqlite.org/download.html). Adicione esse arquivo ao projeto do Visual Studio do aplicativo em que você deseja usar o SQLite.

*Configurar pré-processadores*

Sempre use SQLITE\_OS\_WINRT and SQLITE\_API=\_\_declspec(dllexport), além de qualquer outra [opção de tempo de compilação](http://www.sqlite.org/compile.html).

![Tela Páginas de Propriedades do SQLite](./images/property-pages.png)

## Gerenciamento de um banco de dados SQLite

Os bancos de dados SQLite podem ser criados, atualizados e excluídos com as APIs de C SQLite. Detalhes da API do SQLite C podem ser encontrados na página de SQLite.org [Introdução à interface SQLite C/C++](http://www.sqlite.org/cintro.html).

Para obter uma compreensão sólida de como funciona o SQLite, comece pela tarefa principal do banco de dados SQL, que é avaliar instruções SQL. Existem dois objetos a serem lembrados:

-   [O manipulador de conexão de banco de dados](https://www.sqlite.org/c3ref/sqlite3.html)
-   [O objeto de instrução preparado](https://www.sqlite.org/c3ref/stmt.html)

Existem seis interfaces para realizar operações de banco de dados nesses objetos:

-   [sqlite3\_open()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/open.html)
-   [sqlite3\_prepare()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/prepare.html)
-   [sqlite3\_step()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/step.html)
-   [sqlite3\_column()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/column_blob.html)
-   [sqlite3\_finalize()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/finalize.html)
-   [sqlite3\_close()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/close.html)

 

 






<!--HONumber=Mar16_HO1-->



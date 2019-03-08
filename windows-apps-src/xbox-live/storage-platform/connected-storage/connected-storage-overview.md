---
title: Visão geral de armazenamento conectada
description: Saiba mais sobre como usar o armazenamento conectado para salvar e carregar dados de jogos em todos os dispositivos.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, armazenamento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 40ad13e46e074154d72d7aad236747c3374110ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639051"
---
# <a name="connected-storage"></a>Armazenamento Conectado
Armazenamento conectado foi projetado para permitir que o título salvar dados de jogos e outros dados de estado relevante que devem ser usado entre os dispositivos. A API de armazenamento conectado permite que os títulos no Xbox One e Platform(UWP) Universal do Windows para salvar, carregar e excluir dados de título que são armazenados localmente e também são sincronizados com a nuvem sempre que o título do Xbox One ou UWP é conectado à internet. Os dados salvos estará disponíveis em qualquer dispositivo que executa seu título, após a sincronização. Os desenvolvedores são incentivados a salvar estado de título de forma mais precisa possível oferecer que o melhor em casa reproduzir experiência. Armazenamento conectado é o que permite que você progride em um jogo em casa, em seguida, pegue seu direito de jogo onde você parou em qualquer outro dispositivo que suporta o mesmo jogo.

## <a name="connected-storage-features"></a>Recursos de armazenamento conectado

A API de armazenamento conectado fornece os seguintes recursos:

- Aplicativos rapidamente podem salvar até 16 MB de dados por vez em um buffer de memória, que, em seguida, é armazenado em cache localmente em HDD pelo sistema e carregado para a nuvem.
- Para os parceiros gerenciados e ID@Xbox desenvolvedores:
    - 256 MB por usuário/aplicativo de armazenamento em nuvem.
- Para desenvolvedores do programa de criadores do Xbox Live:
    - 64 MB por usuário/aplicativo de armazenamento em nuvem.
- Resposta robusta para falhas de energia — aplicativos não precisam lidar com dados parciais que está sendo salvos.
- Dados são carregados automaticamente para a nuvem, mesmo quando o aplicativo não está em execução.
- Dados estão disponíveis em todos os dispositivos Xbox One ou UWP que estão conectados ao Xbox Live.
- Xbox Live lida com gerenciamento de sincronização e a conflitos entre dispositivos sem a necessidade de envolvimento pelo aplicativo.

O sistema de armazenamento conectado certifica-se de que todos os salvamentos são feitos em sua totalidade ou não. Isso significa que como desenvolvedor você nunca terá que se preocupar sobre dados parcialmente salvos que afetam o estado do jogo no caso de falha de energia ou o usuário fechar o aplicativo de repente, manualmente ou abrindo o outro aplicativo e de jogos no console. Isso também garante um jogo mais suave reproduzir a experiência dos usuários de seu título, como o armazenamento conectado é uma parte importante do que o que permite que os usuários alternem entre os jogos e aplicativos rapidamente e livremente, mas ainda retornam para o jogo original no estado em que eles à esquerda. Para implementar esses recursos em seu próprio título você precisará ter uma compreensão das APIs de armazenamento conectados.

## <a name="connected-storage-structure"></a>Estrutura de armazenamento conectada

O sistema de armazenamento conectado permite que os aplicativos armazenam dados como um ou mais blobs em contêineres. Em um alto nível, todos os dados no sistema de armazenamento conectado está associado um usuário ou um usuário ou o computador no caso do Kit de desenvolvimento do Xbox desenvolvidos títulos. Todos os dados salvos por um aplicativo para um determinado usuário ou computador é armazenado em um espaço de armazenamento conectados. Cada usuário de seu aplicativo obtém um espaço de armazenamento conectados com um limite de 64 ou 256 MB de armazenamento total. É importante observar que esse armazenamento é dedicado ao seu aplicativo sozinho, ele não é compartilhado com outros aplicativos.

### <a name="containers-and-blobs"></a>Contêineres e blobs

O contêiner de armazenamento conectados ou contêiner para abreviar, é a unidade básica de armazenamento. Cada espaço de armazenamento conectados pode conter vários contêineres, conforme mostrado no diagrama a seguir.

Espaço de armazenamento conectado (por título/máquina ou por título/usuário)

![connected_storage_space_containers.png](../../images/connected_storage/connected_storage_space_containers.png)

 Dados são armazenados em contêineres como um ou mais buffers chamados blobs. O diagrama a seguir ilustra a representação interna do sistema de contêineres no disco. Para cada contêiner, há um arquivo de contêiner que contém referências ao arquivo de dados para cada blob no contêiner.

Diagrama de um contêiner

![container_storage_blobs.png](../../images/connected_storage/container_storage_blobs.png)

Para armazenar dados em um contêiner, chame o método de contêiner apropriado do APIs SubmitUpdatesAsync, fornecendo um mapa de nomes e blobs (objetos de Buffer). Todas as alterações descritas em uma chamada de SubmitUpdatesAsync sejam aplicadas automaticamente, ou seja, o todos os blobs são atualizados conforme solicitado, ou toda a operação será encerrada e o contêiner permanecerá em seu estado antes da chamada.

Indivíduo de operações que usam SubmitUpdatesAsync de salvamento são limitados a 16 MB de dados por vez.

## <a name="connected-storage-api"></a>API de armazenamento conectada

Armazenamento conectado tem APIs separadas para os aplicativos do XDK e UWP. Felizmente, essas APIs são semelhantes entre si de forma bastante aproximada. As duas APIs diferem principalmente em seus nomes de classe e namespace. As funções necessárias para [salve](connected-storage-saving.md), [carregar](connected-storage-loading.md), e [excluir](connected-storage-deleting.md) dados com a API têm nomes idênticos.

Ainda mais diferenças entre as duas APIs de armazenamento conectados são detalhadas na seção de armazenamento conectados [portabilidade Xbox Live o código do XDK para UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

Você pode encontrar que as APIs de armazenamento conectados XDK documentados no arquivo. chm XDK sob o caminho: **Xbox um XDK >> Referência da API >> Referência de API da plataforma >> Referência de API do sistema >> Windows.Xbox.Storage**.
As APIs do XDK também estão documentadas na [developer.microsoft.com site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
O link para as APIs do XDK requer que você tenha um Account(MSA) Microsoft que foi habilitada para acesso de Kit(XDK) de desenvolvedor do Xbox.
Windows.Xbox.Storage é o nome do namespace de armazenamento conectados para consoles do Xbox One.

Você pode encontrar as APIs UWP conectado armazenamento documentados no arquivo. chm Xbox Live SDK sob o caminho: **APIs do Xbox Live >> Xbox Live a referência de API do SDK de extensões de plataforma >> Windows.Gaming.XboxLive.Storage**.
As APIs de armazenamento conectados UWP também estão documentadas online sob o [Windows.Gaming.XboxLive.Storage referência ao namespace](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Windows.Gaming.XboxLive.Storage é o nome do namespace de armazenamento conectados para aplicativos UWP.

Para começar a usar o armazenamento conectado, você precisará adquirir um armazenamento conectado *espaço*. Um espaço de armazenamento conectado está associado um usuário ou computador e mantém todos os dados de armazenamento conectado associados a esse usuário ou computador na forma de *recipientes* e *blobs*. Aquisição de um espaço de armazenamento conectados para um computador ou usuário lhe dará acesso para ler e gravar esses dados de entidades armazenadas. Para adquirir um espaço de armazenamento conectados, títulos XDK e UWP chamará o `GetForUserAsync` método, os títulos XDK também podem chamar o `GetForMachineAsync` método, os títulos UWP será possível chamar `GetForMachineAsync`. `GetForUserAsync` e `GetForMachineAsync` estão contidos no `ConnectedStorageSpace` classe no XDK. `GetForUserAsync` está contida no `GameSaveProvider` classe na API de UWP. Esses métodos são operações potencialmente de longa execução, especialmente se o usuário tem dados salvos em um dispositivo e está retomando o jogo pela primeira vez em outro dispositivo. `GetForUserAsync` recupera um espaço de armazenamento conectados para um usuário que você pode usar para criar, acessar e excluir os contêineres.

Para criar um contêiner, ou acessar um contêiner criado anteriormente, chamar o `CreateContainer` função de seu `ConnectedStorageSpace` ou `GameSaveProvider` classe, isso lhe dará acesso a um contêiner nomeado para o usuário ou computador associado a `ConnectedStorageSpace` ou `GameSaveProvider`usado para criar o contêiner. O `ConnectedStorageSpace` e `GameSaveProvider` classes também incluem o `DeleteContainerAsync` função que permite que você excluir um contêiner de fornecida a você fornecer o nome do contêiner a ser excluído.

Para atualizar blobs em sua chamada de contêiner `SubmitUpdatesAsync` no `ConnectedStorageContainer` classe o XDK ou na `GameSaveContainer` classe da API UWP. `SubmitUpdatesAsync` permite que você forneça uma lista de nomes e buffers de dados a serem gravados no blob, uma lista de nomes de blobs a ser excluído e um nome de exibição para a chamada salvam contêiner. Essa é a função que, por fim, você precisará chamar para atualizar dados no seu espaço de armazenamento conectados.

Para ver exemplos das APIs de armazenamento conectado em uso, leia os seguintes artigos de armazenamento conectado: [Salvar dados](connected-storage-saving.md)
[carregar dados](connected-storage-loading.md)
[excluir dados](connected-storage-deleting.md)

> [!NOTE]
> Uma observação sobre segurança:
>
> Aplicativos universais do Windows Platform (UWP) em execução em PCs salvar dados locais em um local que está acessível para o usuário local e não é inerentemente protegido contra violação de usuário.
>Você deve considerar a aplicação da sua própria criptografia e verificações de validade para jogos salvar dados para ajudar a impedir que os usuários modifiquem o jogo salva fora o jogo.
>Por esse mesmo motivo, você deve decidir se deseja permitir que o PC e Xbox versões do seu jogo para compartilhar salva ou mantê-los separados.

## <a name="managing-local-storage"></a>Gerenciar o armazenamento local

Ao testar o armazenamento conectado em seu aplicativo do Kit de desenvolvimento do Xbox ou UWP, você precisa manipular os dados armazenados localmente em seu dispositivo de desenvolvimento.

A ferramenta xbstorage é fornecido com o XDK e é uma ferramenta de linha de comando que permitem que você manipule o armazenamento local no seu console de desenvolvimento.
Para os desenvolvedores da UWP, há uma ferramenta idêntica para PC chamado gamesaveutil.exe que é fornecido com o SDK do Windows 10 Fall Creators Update(10.0.16299.91) e posteriores.

Ambas as ferramentas permitem que você manipule o armazenamento local em seu dispositivo com estes comandos:

|Comando  |Descrição  |
|---------|---------|
|Redefinir    | Executa uma redefinição de fábrica no armazenamento conectado. |
|importar   | Importa dados do arquivo XML especificado para um espaço de armazenamento conectados. |
|Exportar   | Exporta dados de um espaço de armazenamento conectados ao arquivo XML especificado. |
|excluir   | Exclui dados de um espaço de armazenamento conectados. |
|Gerar | Gera dados fictícios e salva o arquivo XML especificado. |
|Simular | Simula condições de espaço de armazenamento insuficiente. |

Para saber mais sobre as funções disponíveis na ferramenta xbstorage e gamesaveutils.exe leia [gerenciar o armazenamento local conectado](connected-storage-xb-storage.md).

## <a name="technical-overview"></a>Visão geral técnica

Para tirar uma mais profundidade examinar como funciona o armazenamento conectado no XDK ler o [conectados armazenamento visão geral técnica](connected-storage-technical-overview.md). A visão geral técnica foi escrita especificamente para os desenvolvedores do XDK, mas os conceitos contidos se aplicam ao armazenamento conectado em geral.
---
title: Armazenamento do Xbox Live título
description: Saiba como usar o armazenamento de título do Xbox Live para armazenar informações sobre o jogo para um título na nuvem.
ms.assetid: a4182bc8-d232-4e77-93ae-97fe17ac71b1
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 472a2131c113cdde5d7383e169e4fbe638e4e555
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623921"
---
# <a name="xbox-live-title-storage"></a>Armazenamento do Xbox Live título

O serviço de armazenamento de título do Xbox Live fornece uma maneira de armazenar informações sobre o jogo para um título na nuvem. Jogos em execução em todas as plataformas podem usar esse serviço.

<a name="ID4EW"></a>

## <a name="features-of-xbox-live-title-storage"></a>Recursos do armazenamento de título Xbox Live

Alguns dos recursos de alto nível de armazenamento de título Xbox Live incluem, mas não estão limitados a:

-   Podem ser compartilhados entre usuários, títulos e várias plataformas
-   Dá suporte a JSON, binário e arquivos de configuração

Os principais recursos do armazenamento de título Xbox Live são explicados mais detalhadamente nas seções a seguir:

-   [Tipos de armazenamento](#ID4ETB)
-   [Tipos de dados](#ID4ECF)
-   [URIs de armazenamento do título](#ID4EBEAC)
-   [Limite de limitação](#ID4ETEAC)

<a name="ID4ETB"></a>

Para os parceiros gerenciados e ID@Xbox membros:

| Tipo de Armazenamento       | Cota (gerenciados Partners/ID@Xbox) | Cota (programa de criadores ao vivo do Xbox) |  Finalidade                                                                                                                                                      | Plataformas                                                                                           | Usuários                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| Trusted Platform   | 256 MB por usuário | 64 MB por usuário    | Jogos de dados salvos, como por usuário ou o estado do jogo para Reproduzir/Pausar/retomar. Mais segura, mas com restrições de plataforma. | Qualquer plataforma pode ler, mas pode gravar somente Xbox One, Xbox 360 ou Windows Phone.  | Configurável para public ou apenas o proprietário.       |
| Plataforma universal | 64 MB por usuário | 64 MB por usuário    | Jogos de dados salvos, como por usuário ou o estado do jogo para Reproduzir/Pausar/retomar. | Qualquer plataforma pode gravar, mas apenas a plataformas diferentes do Xbox One, Xbox 360 ou o Windows Phone podem ler. | Configurável para public ou apenas o proprietário.       |
| Global             | 256 MB | 256 MB            | Dados que todos podem ler, como listas, mapas, desafios ou recursos de arte. | Somente gravável por meio do Portal do desenvolvedor do Xbox ou Partner Center, qualquer plataforma pode ler.                                | Todos os usuários podem ler.

### <a name="deprecated-storage-types"></a>Tipos de armazenamento preterido

Os seguintes tipos de armazenamento foram preteridos. Eles têm suporte apenas para títulos que estão usando-os. Eles não estão disponíveis para novos títulos.

| Tipo de Armazenamento       | Cota  |   Finalidade                                                                                                                                                      | Plataformas                                                                                           | Usuários                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| JSON               | 64 MB por usuário     | Jogos de dados salvos, como por usuário ou o estado do jogo para Reproduzir/Pausar/retomar. Mais segura, sem restrições de plataforma, mas com dados de formato restrições (JSON). | Qualquer plataforma pode ler ou gravar.                                                                     | Configurável para public ou apenas o proprietário.       |
| Dispositivo             | 64 MB por dispositivo   | Dados específicos de um dispositivo, como configurações ou preferências de dispositivo.                                                                                            | Pode gravar somente Xbox One, Xbox 360 ou Windows Phone. Somente o dispositivo que gravou os dados pode ser lidos.  | Todos os usuários podem ler.                         |
| Armazenamento de sessão    | 256 MB por sessão | Dados para qualquer pessoa que é associado a uma determinada sessão de jogo com vários participantes.                                                                                             | Todas as plataformas que podem ingressar na sessão.                                                             | Todos os usuários na sessão podem ler ou gravar. |


<a name="ID4ECF"></a>

## <a name="types-of-data"></a>Tipos de dados

Jogos de especificam o tipo de dados a serem usados na **{type}** parâmetro de um método GET ou PUT. A seção a seguir descreve os três tipos com suporte:

-   [Informações binárias](#ID4ENF)
-   [Informações do JSON](#ID4EUF)
-   [informações de configuração](#ID4ECAAC)

<a name="ID4ENF"></a>

#### <a name="binary-information"></a>Informações binárias

Imagens, sons e dados personalizados usam o tipo binário. Porque os dados devem ser transmitidos por HTTP, os dados binários devem ser codificados em caracteres aceitos pelo HTTP. Por exemplo, você pode converter os dados em cadeias de caracteres hexadecimais ou usar a codificação base64. O sistema de armazenamento do título não processa os dados codificados, portanto, seu jogo deve usar o mesmo esquema para codificar e decodificar dados ao ler e gravar no armazenamento do título.

<a name="ID4EUF"></a>

#### <a name="json-information"></a>Informações do JSON

Dados estruturados podem usar o tipo JSON. Objetos JSON podem ser usados diretamente em linguagens, como JavaScript, que dão suporte a eles. Ao recuperar dados de arquivos JSON, o jogo pode fornecer um *selecionar* parâmetro para retornar itens específicos dentro da estrutura. Por exemplo, use um arquivo formatado JSON que contém as seguintes informações:

    {
    "difficulty" : 1,
    "level" :
        [
            { "number" : "1", "quest" : "swords" },
            { "number" : "2", "quest" : "iron" },
            { "number" : "3", "quest" : "gold" },
            { "number" : "4", "quest" : "queen" }
         ],
    "weapon" :
        {
             "name" : "poison",
             "timeleft" : "2mins"
        }
    }


| Observação                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Para fins de segurança, o primeiro elemento de dados JSON não deve ser uma matriz. Dados JSON enviados com uma matriz na raiz serão rejeitados pelo serviço. |

Jogos podem selecionar partes dessa estrutura com uma consulta como esta:

             GET https://titlestorage.xboxlive.com/users/xuid(1234)/storage/titlestorage/titlegroups/
             faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=weapon.name
             Content-Type: application/octet-stream
             x-xbl-contract-version: 1
             Authorization: XBL3.0 x=<userHash>;<STSTokenString>
             Connection: Keep-Alive

O corpo da resposta para esta consulta é:

    {
        "name" : "poison"
    }

A matriz pode ser acessada com uma consulta como esta:

      GET https://titlestorage.xboxlive.com//users/xuid(1234)/storage/titlestorage/titlegroups/
      faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=levels[3].quest
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

O corpo da resposta para esta consulta é:

    {
        "quest" : "queen"
    }

As seguintes restrições de comprimento são impostas para dados JSON:

-   Valor numérico, o comprimento máximo = 32
-   Valor, o comprimento máximo da cadeia de caracteres = 1024
-   Nome da propriedade, o comprimento máximo = 64
-   Hierarquia, profundidade máxima = 16
-   Matriz, o tamanho máximo = 1024
-   Propriedades filho máxima em um objeto = 1024

<a name="ID4ECAAC"></a>

#### <a name="configuration-information"></a>informações de configuração

O **{type}** pode ser **config** para indicar que os dados são um blob de configuração. Blobs de configuração são estruturas de dados que são armazenadas no armazenamento de título global. O formato do blob é semelhante a um objeto JSON.

Blobs de configuração podem incluir os nós virtuais que retornam uma configuração de uma lista de possibilidades. Nós virtuais são úteis para fornecer configurações para situações específicas, como para um título ou uma localidade. O nó virtual inclui várias configurações possíveis, junto com valores e as condições para a seleção dos valores. No exemplo a seguir, o **defaultCardDesign** configuração pode ter um dos valores no nó virtual.

    {
      "defaultCardDesign":
      {
        "_virtualNode":
       {
          "_selectBy":"titleId",
          "_sourceNodes":
          [
            {"_selector":"123456799", "_data":"RobotUnicornCard.png,binary"},
            {"_selector":"default", "_data":"StandardCard.png,binary"}
          ]
        }
      },
    }

Quando um jogo lê esse arquivo, o sistema seleciona um dos valores a partir de  **\_sourceNodes** matriz. Nesse caso, o item é selecionado com base na ID do título do jogo. Os usuários jogando **12456799** consulte:

    {
      "defaultCardDesign":"RobotUnicornCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:1234567899"]
    }

Consulte o restante dos usuários:

    {
      "defaultCardDesign":"StandardCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:default"]
    }

Jogos podem definir seletores personalizados que correspondem a um parâmetro na solicitação. Por exemplo, no blob configuração:

    {
        "defaultCardDesign":
        {
            "_virtualNode":
            {
                "_selectBy":"custom:gameMode",
                "_sourceNodes":
                [
                    {"_selector":"silly", "_data":"RobotUnicornCard.png,binary"},
                    {"_selector":"serious", "_data":"SeriousCard.png,binary"},
                    {"_selector":"default", "_data":"StandardCard.png,binary"}
                 ]
            }
        },
        "backgroundColor":"green",
        "dealerHitsOnSoft17":true
    }

Jogos passam uma cadeia de caracteres a **customSelector** parâmetro para selecionar qual item a retornar. Por exemplo, para que a segunda opção, um jogo de solicitações:

      GET https://titlestorage.xboxlive.com/media/titlegroups/faa29d21-2b49-4908-96bf-b953157ac4fe
      /storage/data/config.json,config?customSelector=gameMode.serious
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

O  **\_selectBy** valor indica o tipo de seleção fazer e o  **\_seletor** valor indica que os dados a serem usados na seleção. Os valores possíveis são:

<table>
<thead>
<tr>
<th>_selectBy</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td >titleId</td>
<td ><p>O <strong>_selector</strong> corresponde à ID do título da declaração fornecido.</p></td>
</tr>
<tr>
<td >locale</td>
<td ><p>O <strong>_selector</strong> corresponde a cadeia de caracteres de localidade do cabeçalho Accept-Language.</p></td>
</tr>
<tr>
<td >Personalizado</td>
<td ><p>O <strong>_selector</strong> corresponde a uma cadeia de caracteres personalizada passada a <strong>customSelector</strong> parâmetro de consulta. O <strong>customSelector</strong> contém uma ou mais consultas separadas por vírgulas. Cada consulta é o nome da <strong>selectBy</strong> elemento e o valor da <strong>_selector</strong> elemento.</p></td>
</tr>
</tbody>
</table>

<a name="ID4EBEAC"></a>

## <a name="title-storage-uris"></a>URIs de armazenamento do título

URIs de armazenamento de título são formatados da seguinte maneira:

    https://titlestorage.xboxlive.com/{path}

O **{path}** parte do URI é o tipo de solicitação que está sendo feita e deve ser 245 caracteres ou menos.

<a name="ID4ETEAC"></a>

## <a name="throttle-limit"></a>Limite de limitação

Há sem limites fixos em quantas leituras ou gravações um título pode fazer por minuto, mas ela geralmente não é possível fazer mais de uma por minuto em média em uma sessão de uma hora. Por exemplo, um título pode fazer 60 leituras ou gravações no início de uma sessão, mas não há mais para o restante da hora. Títulos devem ser protegidos contra mais chamadas mais tarde, no caso de serviços do Xbox LIVE precisar limitar as solicitações.

Se o título tem requisitos especiais de particionamento, como extras leituras ou gravações, entre em contato com o Microsoft.

<a name="ID4E5EAC"></a>

## <a name="using-title-storage"></a>Usando o armazenamento de título

Para começar com o armazenamento de título, primeiro determine que tipo de dados que você deseja armazenar. Alguns exemplos incluem jogos salvos, estado do jogo, superar os desafios diários, mapas de jogos e recursos de arte.

Em seguida, determine quais títulos e plataformas precisarão acessar os dados. Título armazenamento dá suporte à nuvem acesso a dados de cada título em uma plataforma única e de vários títulos em várias plataformas.

Por fim, use os tópicos nesta seção para configurar o armazenamento, carregar seus dados e definir permissões de acesso adequadamente com base em suas opções.

<a name="ID4EJFAC"></a>

## <a name="in-this-section"></a>Nesta seção

[Ler um Blob de configuração no armazenamento do Xbox Live título](reading-configuration-blobs.md)  
Demonstra a leitura armazenamento de blobs de configuração do Xbox Live do título.

[Armazenando um Blob binário no armazenamento do Xbox Live título](storing-binary-blobs.md)  
Demonstra como armazenar blobs binários no armazenamento de título Xbox Live.

[Ler um Blob binário no armazenamento do Xbox Live título](reading-binary-blobs.md)  
Demonstra a leitura de blobs binários do armazenamento de título de Xbox Live.

[Armazenando um Blob JSON no armazenamento do Xbox Live título](storing-jsonblobs.md)  
Demonstra a armazenar blobs JSON no armazenamento de título Xbox Live.

[Ler um Blob JSON no armazenamento do Xbox Live título](reading-jsonblobs.md)  
Demonstra a leitura armazenamento de blobs do JSON do Xbox Live do título.

<a name="ID4E4FAC"></a>

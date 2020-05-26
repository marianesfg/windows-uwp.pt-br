---
title: Comando source
description: Gerencia os repositórios acessados pelo Gerenciador de Pacotes do Windows.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: b44f20021a0fa33da862e2361be60b730b041b49
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824967"
---
# <a name="source-command-winget"></a>Comando source (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

O comando **source** da ferramenta [winget](index.md) gerencia os repositórios acessados pelo Gerenciador de Pacotes do Windows. Com o comando **source**, é possível **adicionar**, **remover**, **listar** e **atualizar** os repositórios.

Uma fonte fornece os dados para você descobrir e instalar aplicativos. Somente adicione uma nova fonte se você confiar nela como um local seguro.

## <a name="usage"></a>Uso

`winget source \<sub command> \<options>`

![Imagem de origem](images\source.png)

## <a name="arguments"></a>Argumentos

Os argumentos a seguir estão disponíveis.

| Argumento  | Descrição |
|--------------|-------------|
| **-?, --help** |  Obtém ajuda adicional sobre esse comando. |

## <a name="sub-commands"></a>Subcomandos

A fonte dá suporte aos seguintes subcomandos para manipular as fontes.

| Subcomando  | Descrição |
|--------------|-------------|
|  **add** |  Adiciona uma nova fonte. |
|  **list** | Enumera a lista de fontes habilitadas. |
|  **update** | Atualiza uma fonte. |
|  **remove** | Remove uma fonte. |
|  **reset** | Redefine **winget** para a configuração inicial.  |

## <a name="options"></a>Opções

O comando **source** dá suporte às opções a seguir.

| Opção  | Descrição |
|--------------|-------------|
|  **-n,--name** | O nome pelo qual identificar a fonte. |
|  **-a,--arg** | A URL ou o UNC da fonte. |
|  **-t,--type** | O tipo de fonte. |
| **-?, --help** |  Obtém ajuda adicional sobre esse comando. |

## <a name="add"></a>adicionar

O subcomando **add** adiciona uma nova fonte. Esse subcomando requer a opção **--name** e o argumento **name**.

Uso: `winget source add [-n, --name] \<name> [-a] \<url> [[-t] \<type>]`

Exemplo: `winget source add --name Contoso  https://www.contoso.com/cache`

O subcomando **add** também dá suporte ao parâmetro **type** opcional. O parâmetro **type** comunica ao cliente a qual tipo de repositório ele está se conectando. Há suporte para os tipos a seguir.

| Tipo  | Descrição |
|--------------|-------------|
| **Microsoft.PreIndexed.Package** | O tipo de fonte \<default>. |

## <a name="list"></a>lista

o subcomando **list** enumera as fontes habilitadas no momento. Esse subcomando também fornece detalhes sobre uma fonte específica.

Uso: `winget list [-n, --name] \<name>`

### <a name="list-all"></a>list all

O subcomando **list** por si só revelará a lista completa de fontes com suporte. Por exemplo:

```CMD
> C:\winget list
> Current sources:
>     Contoso ->  https://www.contoso.com/cache
```

### <a name="list-source-details"></a>list source details

Para obter detalhes completos sobre a fonte, passe o nome usado para identificá-la. Por exemplo:

```CMD
> C:\winget source list --name contoso  
> Name   : contoso  
> Type   : Microsoft.PreIndexed.Package  
> Arg    : https://pkgmgr-int.azureedge.net/cache  
> Data   : AppInstallerSQLiteIndex-int_g4ype1skzj3jy  
> Updated: 2020-4-14 17:45:32.000
```

**Name** exibe o nome pelo qual identificar a fonte.
**Type** exibe o tipo de repositório.
**Arg** exibe a URL ou o caminho usado pela fonte.
**Data** exibe o nome do pacote opcional usado, se apropriado.
**Updated** exibe a data e hora da última atualização da fonte.

## <a name="update"></a>atualizar

O subcomando **update** força uma atualização para uma fonte individual ou para todas.

uso: `winget update [-n, --name] \<name>`

### <a name="update-all"></a>update all

O subcomando **update** por si só solicitará e atualizará cada repositório. Por exemplo: `C:\winget update`

### <a name="update-source"></a>atualizar origem

O subcomando **update** combinado com a opção **--name** pode direcionar e atualizar para uma fonte individual. Por exemplo: `C:\winget update --name contoso`

## <a name="remove"></a>remover

O subcomando **remove** remove uma fonte. Esse subcomando requer a opção **--name** e **name argument** a fim de identificar a fonte.

Uso: `winget source add [-n, --name] \<name>`

Por exemplo: `winget source remove --name Contoso`

## <a name="reset"></a>reset

O subcomando **reset** redefine o cliente para a configuração original dele. O subcomando **redefinir** remove todas as fontes e define a fonte como o padrão. Esse subcomando só deve ser usado em casos raros.

Uso: `winget source reset`

Por exemplo: `winget source reset`

## <a name="default-repository"></a>Repositório padrão

O Gerenciador de Pacotes do Windows especifica um repositório padrão. É possível identificar o repositório usando o comando **list**. Por exemplo: `winget source list`

## <a name="related-topics"></a>Tópicos relacionados

* [Usar a ferramenta winget para instalar e gerenciar aplicativos](index.md)

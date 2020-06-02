---
title: Comando search
description: Consulta as fontes para ver se há aplicativos disponíveis que possam ser instalados
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5a176c1138ebfe3f3a9eb2cbef02dad745cfe170
ms.sourcegitcommit: 8193aef04deb3514eb2d34bfe5cb9424ba12cd76
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83865013"
---
# <a name="search-command-winget"></a>Comando search (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

O comando **search** da ferramenta [winget](index.md) consulta as fontes para ver se há aplicativos disponíveis que possam ser instalados.  

O comando **search** pode mostrar todos os aplicativos disponíveis ou pode ser filtrado para um aplicativo disponível. O comando **search** é usado normalmente para identificar a cadeia de caracteres a ser usada para instalar um aplicativo específico.

## <a name="usage"></a>Uso

`winget search [[-q] \<query>] [\<options>]`

![pesquisar](images\search.png)

## <a name="arguments"></a>Argumentos

Os argumentos a seguir estão disponíveis.

| Argumento  | Descrição |
 --------------|-------------|
| **-q,--query** |  A consulta usada para pesquisar um aplicativo. |
| **-?, --help** |  Obtém ajuda adicional sobre esse comando. |

## <a name="show-all"></a>Mostrar tudo

Se o comando search não incluir filtros nem opções, ele exibirá todos os aplicativos disponíveis na origem padrão. Também será possível pesquisar todos os aplicativos em outra fonte se passar apenas a opção de **origem**.

## <a name="search-strings"></a>Cadeias de caracteres de pesquisa

As cadeias de caracteres de pesquisa podem ser filtradas com as opções a seguir.

| Opção  | Descrição |
 --------------|-------------|
| **--id**        |   Limita a pesquisa à ID do aplicativo. A ID inclui o editor e o nome do aplicativo. |
| **--name**      |  Limita a pesquisa ao nome do aplicativo. |
| **--moniker**  |    Limita a pesquisa ao moniker especificado. |
| **--tag**    |  Limita a pesquisa às tags listadas para o aplicativo. |
| **--command**   |   Limita a pesquisa ao nome do aplicativo. |

A cadeia de caracteres será tratada como uma substring. A pesquisa, por padrão, também não diferencia maiúsculas de minúsculas. Por exemplo, `winget search micro` poderia retornar o seguinte:

* Microsoft
* microscope
* MyMicro

## <a name="search-options"></a>Opções de pesquisa

Os comandos de pesquisa dão suporte a várias opções ou filtros para ajudar a limitar os resultados.

| Opção  | Descrição |
 --------------|-------------|
| **-e, --exact**  |     Usa a cadeia de caracteres exata na consulta, incluindo a verificação da diferenciação de maiúsculas e minúsculas. Ele não usará o comportamento padrão de uma substring.  |  
| **-n, --count**      |  Restringe a saída da exibição à contagem especificada. |
| **-s, --source**     |  Restringe a pesquisa ao nome de [origem](source.md) especificado.  |

## <a name="related-topics"></a>Tópicos relacionados

* [Usar a ferramenta winget para instalar e gerenciar aplicativos](index.md)

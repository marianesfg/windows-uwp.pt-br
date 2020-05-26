---
title: Comando show
description: Exibe detalhes para o aplicativo especificado, incluindo detalhes sobre a fonte do aplicativo, bem como os metadados associados a ele.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 1df5a5287b6c7a1321025182f7b3f24ed896e76d
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824977"
---
# <a name="show-command-winget"></a>Comando show (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

O comando **show** da ferramenta [winget](index.md) exibe detalhes para o aplicativo especificado, incluindo detalhes sobre a fonte do aplicativo, além dos metadados associados a ele.

O comando **show** mostra apenas os metadados que foram enviados com o aplicativo. Se o aplicativo enviado excluir alguns metadados, os dados não serão exibidos.

## <a name="usage"></a>Uso

`winget show [[-q] \<query>] [\<options>]`

![Comando show](images\show.png)

## <a name="arguments"></a>Argumentos

Os argumentos a seguir estão disponíveis.

| Argumento  | Descrição |
|--------------|-------------|
| **-q,--query** |  A consulta usada para pesquisar um aplicativo. |
| **-?, --help** |  Obtém ajuda adicional sobre esse comando. |

## <a name="options"></a>Opções

As opções a seguir estão disponíveis.

| Opção  | Descrição |
|--------------|-------------|
| **-m,--manifest** | O caminho para o manifesto do aplicativo a ser instalado. |
| **--id**         |  Filtrar resultados por ID. |
| **--name**   |      Filtrar resultados por nome. |
| **--moniker**   |  Filtrar resultados por moniker do aplicativo. |
| **-v,--version** |  Usar a versão especificada. O padrão é a última versão. |
| **-s,--source** |   Localizar o aplicativo usando a [fonte](source.md) especificada. |
| **-e,--exact**     | Localizar o aplicativo usando a correspondência exata. |
| **--versions**    | Mostrar as versões disponíveis do aplicativo. |

## <a name="multiple-selections"></a>Seleções múltiplas

Se a consulta fornecida para **winget** não resultar em um único aplicativo, **winget** exibirá os resultados da pesquisa. Isso fornecerá os dados adicionais necessários para refinar a pesquisa.

## <a name="results-of-show"></a>Resultados de mostrar

Se um único aplicativo for detectado, os dados a seguir serão exibidos.

### <a name="metadata"></a>Metadados

| Valor  | Descrição |
|--------------|-------------|
| **Id**   | Id do aplicativo. |
| **Nome**  | Nome do aplicativo. |
| **Publisher** | Editor do aplicativo. |
| **Version** | Versão do aplicativo. |
| **Author**  | Autor do aplicativo. |
| **AppMoniker** | AppMoniker do aplicativo. |
| **Descrição** | Descrição do aplicativo. |
| **Licença**  | Licença do aplicativo. |
| **LicenseUrl** | A URL para o arquivo de licença do aplicativo. |
| **Homepage**  | Página inicial do aplicativo. |
| **Tags** | As tags fornecidas para auxiliar na pesquisa.  |
| **Comando** | Os comandos compatíveis com o aplicativo. |
| **Channel**  | Os detalhes sobre se o aplicativo é versão prévia ou lançamento.  |
| **Minimum OS Version** | A versão mínima do sistema operacional compatível com o aplicativo. |

### <a name="installer-details"></a>Detalhes do instalador

| Valor  | Descrição |
|--------------|-------------|
| **Arch**   | A arquitetura da instância do instalador. |
| **Idioma**  | O idioma do instalador. |
| **Installer Type**  | O tipo de instalador. |
| **Download Url** | A URL do instalador. |
| **Hash** | O Sha-256 do instalador.  |
| **Escopo** | Exibe se o instalador é por computador ou por usuário. |

## <a name="related-topics"></a>Tópicos relacionados

* [Usar a ferramenta winget para instalar e gerenciar aplicativos](index.md)

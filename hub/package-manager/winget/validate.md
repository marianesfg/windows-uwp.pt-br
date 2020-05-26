---
title: Comando winget validate
description: Valida um arquivo de manifesto para enviar o software para o Repositório do Manifesto do Pacote da Microsoft Community no GitHub.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5772e144a4226be9fbb4a4949aaac1e3d4408e6
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824927"
---
# <a name="validate-command-winget"></a>Comando validate (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

O comando **validate** da ferramenta [winget](index.md) valida um [arquivo de manifesto](../package/manifest.md) para enviar software para o **Repositório do Manifesto do Pacote da Microsoft Community** no GitHub. O manifesto deve ser um arquivo YAML que siga a [especificação](https://github.com/microsoft/winget-pkgs/YamlSpec.md).

## <a name="usage"></a>Uso

`winget validate [--manifest] \<manifest>`

## <a name="arguments"></a>Argumentos

Os argumentos a seguir estão disponíveis.

| Argumento  | Descrição |
|--------------|-------------|
| **--manifest** |  o caminho para o manifesto a ser validado. |
| **-?, --help** |  obter ajuda adicional sobre esse comando |

## <a name="related-topics"></a>Tópicos relacionados

* [Usar a ferramenta winget para instalar e gerenciar aplicativos](index.md)
* [Enviar pacotes para o Gerenciador de Pacotes do Windows](../package/index.md)

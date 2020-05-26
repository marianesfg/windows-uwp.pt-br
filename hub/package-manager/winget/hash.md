---
title: Comando winget hash
description: Gera o hash SHA256 para um aplicativo.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2f7b2074a9d6f2a01fe5a74f7f081ceeedcccec9
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824947"
---
# <a name="hash-command-winget"></a>Comando hash (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

O comando **hash** da ferramenta [winget](index.md) gera o hash SHA256 para um instalador. Esse comando será usado se for necessário criar um [arquivo de manifesto](../package/manifest.md) para enviar o software para o **Repositório do Manifesto do Pacote da Microsoft Community** no GitHub. Além disso, o comando **hash** também dá suporte à geração de um hash de certificado SHA256 para arquivos MSIX.

## <a name="usage"></a>Uso

`winget hash [-f] \<file> [\<options>]`

O subcomando **hash** só pode ser executado em um arquivo local. Para usar o subcomando **hash**, baixe o instalador para um local conhecido. Em seguida, passe o caminho do arquivo como um argumento para o subcomando **hash**.

## <a name="arguments"></a>Argumentos

Os seguintes argumentos estão disponíveis:

| Argumento  | Descrição |
|--------------|-------------|
| **-f,--file** |  O caminho para o arquivo a receber o hash. |
| **-m,--msix**  | Especifica que o comando hash também criará o SHA 256 SignatureSha256 para uso com instaladores MSIX. |
| **-?, --help** |  Obtém ajuda adicional sobre esse comando. |

## <a name="related-topics"></a>Tópicos relacionados

* [Usar a ferramenta winget para instalar e gerenciar aplicativos](index.md)
* [Enviar pacotes para o Gerenciador de Pacotes do Windows](../package/index.md)

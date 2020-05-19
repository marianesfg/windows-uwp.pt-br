---
title: Enviar pacotes para o Gerenciador de Pacotes do Windows
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: e83088c5a6b2755a8ce7f08e513d09f877580db8
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580083"
---
# <a name="submit-packages-to-windows-package-manager"></a>Enviar pacotes para o Gerenciador de Pacotes do Windows

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Se você for um ISV (Fornecedor Independente de Software), poderá usar o Gerenciador de Pacotes do Windows como um canal de distribuição para pacotes de software que contenham seus aplicativos. No momento, o Gerenciador de Pacotes do Windows dá suporte a instaladores nos seguintes formatos: MSIX, MSI e EXE.

Para enviar pacotes de software para o Gerenciador de Pacotes do Windows, siga estas etapas:

1. [Crie um manifesto do pacote que forneça informações sobre seu aplicativo](manifest.md). Os manifestos são arquivos YAML que seguem o esquema do Gerenciador de Pacotes do Windows.
2. [Envie seu manifesto para o repositório do Gerenciador de Pacotes do Windows](repository.md). Esse é um repositório de software livre no GitHub que contém uma coleção de manifestos que a ferramenta **winget** pode acessar.

## <a name="related-topics"></a>Tópicos relacionados

* [Usar a ferramenta winget](../winget/index.md)
* [Criar o manifesto do pacote](manifest.md)
* [Enviar seu manifesto para o repositório](repository.md)
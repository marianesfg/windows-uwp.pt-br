---
title: Gerenciador de Pacotes do Windows
description: ''
author: denelon
ms.author: denelon
ms.date: 05/03/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 4f7a6533d3dea9c304e9be7d8e689ab537a46449
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580093"
---
# <a name="windows-package-manager-preview"></a>Gerenciador de Pacotes do Windows (versão prévia)

[!INCLUDE [preview-note](../includes/package-manager-preview.md)]

O Gerenciador de Pacotes do Windows é uma [solução de gerenciador de pacotes](#understanding-package-managers) abrangente composta por uma ferramenta de linha de comando e um conjunto de serviços para instalar aplicativos no Windows 10.

## <a name="windows-package-manager-for-developers"></a>Gerenciador de Pacotes do Windows para desenvolvedores

Os desenvolvedores usam a ferramenta de linha de comando **winget** para descobrir, instalar, atualizar, remover e configurar um conjunto selecionado de aplicativos. Após a instalação, os desenvolvedores poderão acessar o **winget** por meio do Terminal do Windows, do PowerShell ou do Prompt de Comando.

Para obter mais informações, confira [Usar a ferramenta winget para instalar e gerenciar aplicativos](winget/index.md).

## <a name="windows-package-manager-for-isvs"></a>Gerenciador de Pacotes do Windows para ISVs

ISVs (Fornecedores Independentes de Software) podem usar o Gerenciador de Pacotes do Windows como um canal de distribuição para pacotes de software que contenham as ferramentas e aplicativos deles. Para enviar pacotes de software (contendo instaladores .msix, .msi ou .exe) para o Gerenciador de Pacotes do Windows, fornecemos o **Repositório do Manifesto do Pacote da Microsoft Community** de software livre no GitHub, em que os ISVs podem carregar [manifestos do pacote](package/manifest.md) para que os pacotes de software deles sejam considerados para inclusão no Gerenciador de Pacotes do Windows. Os manifestos são validados automaticamente e também podem ser examinados manualmente.

Para obter mais informações, confira [Enviar pacotes para o Gerenciador de Pacotes do Windows](package/repository.md).

## <a name="understanding-package-managers"></a>Noções básicas sobre gerenciadores de pacotes

Um gerenciador de pacotes é um sistema ou conjunto de ferramentas usado para automatizar a instalação, a atualização, a configuração e o uso do software. A maioria dos gerenciadores de pacotes foi projetada para descobrir e instalar ferramentas para desenvolvedores.

O ideal é que os desenvolvedores usem um gerenciador de pacotes para especificar os pré-requisitos das ferramentas de que eles precisam para desenvolver soluções para um determinado projeto. O gerenciador de pacotes segue, então, as instruções declarativas para instalar e configurar as ferramentas. O gerenciador de pacotes reduz o tempo gasto para preparar um ambiente e ajuda a verificar se as mesmas versões dos pacotes estão instaladas no computador.

Os gerenciadores de pacotes de terceiros podem utilizar o [Repositório do Manifesto do Pacote da Microsoft Community](package/repository.md) para aumentar o tamanho do catálogo de software deles.

## <a name="related-topics"></a>Tópicos relacionados

* [Usar a ferramenta winget para instalar e gerenciar pacotes de software](winget/index.md)
* [Enviar pacotes para o Gerenciador de Pacotes do Windows](package/index.md)
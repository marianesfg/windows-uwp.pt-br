---
title: Comando install
description: Instala o aplicativo especificado.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 8c460ccd18bb1bb12e5322e0e08a17edbd9692f7
ms.sourcegitcommit: 5a145eda92b5915393e58006867cdd8b98e922f5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84166235"
---
# <a name="install-command-winget"></a>Comando install (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

O comando **install** da ferramenta [winget](index.md) instala o aplicativo especificado. Use o comando [**search**](search.md) para identificar o aplicativo que você deseja instalar.  

O comando **install** requer que você especifique a cadeia de caracteres exata a ser instalada. Se houver alguma ambiguidade, você deverá filtrar ainda mais o comando **install** para um aplicativo exato.

## <a name="usage"></a>Uso

`winget install [[-q] \<query>] [\<options>]`

![Comando search](images\install.png)

## <a name="arguments"></a>Argumentos

Os argumentos a seguir estão disponíveis.

| Argumento      | Descrição |
|-------------|-------------|  
| **-q,--query**  |  A consulta usada para pesquisar um aplicativo. |
| **-?, --help** |  Obtêm ajuda adicional sobre esse comando. |

## <a name="options"></a>Opções

As opções permitem que você personalize a experiência de instalação para atender às suas necessidades.

| Opção      | Descrição |
|-------------|-------------|  
| **-m, --manifest** |   Deve ser seguido pelo caminho para o arquivo de manifesto (YAML). É possível usar o manifesto para executar a experiência de instalação de um [arquivo YAML local](#local-install). |
| **--id**    |  Limita a instalação à ID do aplicativo.   |  
| **--name**   |  Limita a pesquisa ao nome do aplicativo. |  
| **--moniker**   | Limita a pesquisa ao moniker listado para o aplicativo. |  
| **-v, --version**  |  Permite que você especifique uma versão exata a ser instalada. Se ela não for especificada, a mais recente instalará o aplicativo com versão mais alta. |  
| **-s, --source**   |  Restringe a pesquisa ao nome de origem fornecido. Deve ser seguido pelo nome de origem. |  
| **-e, --exact**   |   Usa a cadeia de caracteres exata na consulta, incluindo a verificação da diferenciação de maiúsculas e minúsculas. Ele não usará o comportamento padrão de uma substring. |  
| **-i, --interactive** |  Executa o instalador no modo interativo. A experiência padrão mostra o progresso do instalador. |  
| **-h, --silent** |  Executa o instalador no modo sem confirmação. Isso suprime todas as interfaces do usuário. A experiência padrão mostra o progresso do instalador. |  
| **-o, --log**  |  Direciona o log a um arquivo de log. É necessário fornecer um caminho para um arquivo ao qual você tem direitos de gravação. |
| **--override** | Uma cadeia de caracteres que será passada diretamente para o instalador.    |
| **-l, --location** |    Local da instalação (se houver suporte). |

### <a name="example-queries"></a>Exemplos de consulta

O exemplo a seguir instala uma versão específica de um aplicativo.

```CMD
winget install powertoys --version 0.15.2
```

O exemplo a seguir instala um aplicativo por meio da ID dele.

```CMD
winget install --id Microsoft.PowerToys
```

O exemplo a seguir instala um aplicativo por versão e ID.

```CMD
winget install --id Microsoft.PowerToys --version 0.15.2
```

## <a name="multiple-selections"></a>Seleções múltiplas

Se a consulta fornecida para **winget** não resultar em um único aplicativo, **winget** exibirá os resultados da pesquisa. Isso fornecerá os dados adicionais necessários para refinar a pesquisa para uma instalação correta.

## <a name="local-install"></a>Instalação local

A opção **manifesto** permite que você instale um aplicativo passando um arquivo YAML diretamente para o cliente. A opção **manifesto** tem o uso a seguir.

Uso: `winget install --manifest \<file>`

| Opção  | Descrição |
|-------------|-------------|  
|  **-m, --manifest** | O caminho para o manifesto do aplicativo a ser instalado. |

### <a name="log-files"></a>Arquivos de log

Os arquivos de log do winget, a menos que sejam redirecionados, estão localizados na seguinte pasta:  **\%temp%\\AICLI\\*.log**

## <a name="related-topics"></a>Tópicos relacionados

* [Usar a ferramenta winget para instalar e gerenciar aplicativos](index.md)

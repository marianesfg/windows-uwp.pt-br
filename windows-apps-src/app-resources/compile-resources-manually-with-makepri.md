---
author: stevewhims
Description: MakePri.exe is a command line tool that you can use to create and dump PRI files. It is integrated as part of MSBuild within Microsoft Visual Studio, but it could be useful to you for creating packages manually or with a custom build system.
title: Compilar recursos manualmente com o MakePri.exe
template: detail.hbs
ms.author: stwhi
ms.date: 10/23/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 3eb26a8e4939ade3b12e4527b7452a9c6b535a77
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6453909"
---
# <a name="compile-resources-manually-with-makepriexe"></a>Compilar recursos manualmente com o MakePri.exe

O MakePri.exe é uma ferramenta de linha de comando que você pode usar para criar e despejar arquivos PRI. Ele é integrado como parte do MSBuild no Microsoft Visual Studio, mas pode ser útil para criar pacotes manualmente ou com um sistema de compilação personalizado.

> [!NOTE]
> MakePri.exe é instalado ao verificar a opção de **SDK do Windows para aplicativos gerenciados do UWP** ao instalar o Software Development Kit do Windows. Ele é instalado no caminho `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (bem como nas pastas nomeadas para as outras arquiteturas). Por exemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

O limite de tamanho em um arquivo PRI é 64 KB.

## <a name="in-this-section"></a>Nesta seção
|Tópico|Descrição|
|-|-|
| [Opções de linha de comando do MakePri.exe](makepri-exe-command-options.md) | O MakePri.exe tem o conjunto de comandos `createconfig`, `dump`, `new`, `resourcepack` e `versioned`. Este tópico descreve detalhadamente as opções de linha de comando para seu uso. |
| [Arquivo de configuração MakePri.exe](makepri-exe-configuration.md) | Este tópico descreve o esquema do arquivo de configuração XML MakePri.exe. |
| [Indexadores específicos de formato do MakePri.exe](makepri-exe-format-specific-indexers.md) | Este tópico descreve os indexadores específicos de formato usados pela ferramenta MakePri.exe para gerar seu índice de recursos. |

## <a name="makepriexe-command-line-options"></a>Opções de linha de comando do MakePri.exe

O MakePri.exe tem o conjunto de comandos `createconfig`, `dump`, `new`, `resourcepack` e `versioned`. Para ver os detalhes sobre seu uso, consulte [Opções de linha de comando do MakePri.exe](makepri-exe-command-options.md).

## <a name="makepriexe-configuration"></a>Configuração do MakePri.exe

O arquivo de configuração XML PRI determina como e quais recursos são indexados. O esquema do XML de configuração é descrito em [Configuração do MakePri.exe](makepri-exe-configuration.md).

## <a name="format-specific-indexers"></a>Indexadores específicos do formato

O MakePri.exe é normalmente usado com as opções `new`, `versioned` e `resourcepack`. Nesses casos, ele indexa os arquivos de origem para gerar um índice de recursos. O MakePri.exe usa vários indexadores individuais para ler diferentes arquivos de recurso de origem ou contêineres de recursos. O indexador mais simples é o indexador de pasta, que indexa o conteúdo de uma pasta para recursos como imagens `.jpg` ou `.png`. Para obter mais informações, consulte [Indexadores específicos de formato do MakePri.exe](makepri-exe-format-specific-indexers.md).

## <a name="makepriexe-warnings-and-error-messages"></a>Mensagens de erro e de aviso do MakePri.exe

```
Resources found for language(s) '<language(s)>' but no resources found for default language(s): '<language(s)>'. Change the default language or qualify resources with the default language.
```

O aviso acima é exibido quando o MakePri.exe ou o MSBuild descobre arquivos ou recursos de cadeia de caracteres para um determinado recurso nomeado que parece estar marcado com qualificadores de idioma, mas não existem candidatos para um idioma padrão. O processo para usar qualificadores nos nomes de arquivo e de pasta é descrito em [Personalizar os recursos para idioma, escala e outros qualificadores](tailor-resources-lang-scale-contrast.md). Um arquivo ou uma pasta pode conter um nome de idioma, mas não é descoberto nenhum recurso qualificado para o idioma padrão exato. Por exemplo, se um projeto usa "en-US" como idioma padrão e tem um arquivo chamado "de/logo.png", mas não tem nenhum arquivo marcado com o idioma padrão "en-US", esse aviso será exibido. Para remover esse aviso, algum arquivo ou recurso de cadeia de caracteres deve estar qualificado com o idioma padrão ou o idioma padrão deve ser alterado. Para alterar o idioma padrão, com sua solução aberta no Visual Studio, abra `Package.appxmanifest`. Na guia Aplicativo, confirme se o idioma padrão está definido corretamente (por exemplo, "en" ou "en-US").

```
No default or neutral resource given for '<resource identifier>'. The application may throw an exception for certain user configurations when retrieving the resources.
```

O aviso acima é exibido quando o MakePri.exe ou o MSBuild descobre arquivos ou recursos que parecem estar marcados com qualificadores de idioma cujos recursos não são claros. Há qualificadores, mas não há garantia de que um candidato a recurso específico possa ser retornado para esse identificador de recurso no tempo de execução. Se não for possível encontrar nenhum candidato a recurso de um determinado idioma, região ou outro qualificador que seja um padrão ou que sempre corresponda ao contexto de um usuário, esse aviso será exibido. Em tempo de execução, para configurações de usuário específicas, como preferências de idioma ou local de residência de um usuário (**Configurações** > **Hora e idioma** > **Região e idioma**), as APIs usadas para recuperar o recurso podem gerar uma exceção inesperada. Para remover esse aviso, devem ser fornecidos recursos padrão, como um recurso no idioma padrão ou na região de residência global (homeregion-001) do projeto.

## <a name="using-makepriexe-in-a-build-system"></a>Usando o MakePri.exe em um sistema de compilação

Os sistemas de compilação devem usar o comando `new`, `versioned` ou `resourcepack` do MakePri.exe, dependendo do tipo de projeto que está sendo compilado. Os sistemas de compilação que criam um arquivo PRI novo devem usar o comando `new`. Os sistemas de compilação que devem garantir a compatibilidade de deslocamentos internos por meio de iterações podem usar o comando `versioned`. Os sistemas de compilação que devem criar um arquivo PRI com variantes adicionais de recursos, com validação para garantir que nenhum novo recurso seja adicionado para essa variante, devem usar o comando `resourcepack`.

Os sistemas de compilação que exigem controle explícito sobre arquivos de origem indexados podem usar o indexador ResFiles, em vez de indexar uma pasta. Os sistemas de compilação também podem usar várias passagens de índice com diferentes [indexadores específicos de formato](makepri-exe-format-specific-indexers.md) para gerar um único arquivo PRI.

Os sistemas de compilação também podem usar o indexador específico de formato PRI para adicionar arquivos PRI pré-compilados ao PRI do pacote de outros componentes, como bibliotecas de classes, assemblies, SDKs e DLLs.

Quando forem criados arquivos PRI para outros componentes, bibliotecas de classes, assemblies, DLLs e SDKs, a configuração **initialPath** deverá ser usada para garantir que os recursos de componente tenham seus próprios mapas de sub-recursos que não gerem conflitos com o app no qual estão incluídos.

## <a name="related-topics"></a>Tópicos relacionados
* [Opções de linha de comando do MakePri.exe](makepri-exe-command-options.md)
* [Configuração do MakePri.exe](makepri-exe-configuration.md)
* [Indexadores específicos de formato do MakePri.exe](makepri-exe-format-specific-indexers.md)
* [Personalizar os recursos para idioma, escala e outros qualificadores](tailor-resources-lang-scale-contrast.md)

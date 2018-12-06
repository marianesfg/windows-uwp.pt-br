---
title: Distribuição de um componente do Tempo de Execução do Windows gerenciado
description: É possível distribuir o componente do Tempo de Execução do Windows por cópia de arquivo.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef51e2235d8ac5c46af6093809d241d5c137d57d
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8752327"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Distribuição de um componente do Tempo de Execução do Windows gerenciado



É possível distribuir o componente do Tempo de Execução do Windows por cópia de arquivo. No entanto, caso o componente consista em muitos arquivos, a instalação pode ser entediante para os usuários. Além disso, erros na colocação de arquivos ou falha na definição de referências podem causar problemas para eles. É possível empacotar um componente complexo como um SDK de extensão do Visual Studio para facilitar a instalação e o uso. Os usuários só precisam definir uma referência para todo o pacote. Eles podem localizar e instalar facilmente o componente usando a caixa de diálogo **Extensões e Atualizações**, conforme descrito em [Como encontrar e usar extensões do Visual Studio](https://msdn.microsoft.com/library/vstudio/dd293638.aspx) na Biblioteca MSDN.

## <a name="planning-a-distributable-windows-runtime-component"></a>Planejamento de um componente do Tempo de Execução do Windows distribuível

Escolha nomes exclusivos para arquivos binários, como os arquivos .winmd. Recomendamos o seguinte formato para garantir a exclusividade:

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Os arquivos binários serão instalados em pacotes dos aplicativos, possivelmente com arquivos binários de outros desenvolvedores. Consulte "SDKs de extensão" em [Como criar um Software Development Kit do](https://msdn.microsoft.com/library/hh768146.aspx) na Biblioteca MSDN.

Para decidir como distribuir o componente, leve em consideração como ele é complexo. Um SDK de extensão ou gerenciador de pacotes semelhante é recomendado quando:

-   O componente consiste em vários arquivos.
-   Você fornece versões do componente para várias plataformas (x86 e ARM, por exemplo).
-   Você fornece versões de depuração e versão do componente.
-   O componente tem arquivos e assemblies usados somente em tempo de design.

Um SDK de extensão é especialmente útil caso mais de uma das opções acima seja verdadeira.

> **Observação**para componentes complexos, o sistema de gerenciamento de pacotes NuGet oferece uma alternativa livre para SDKs de extensão. Assim como os SDKs de extensão, NuGet permite criar pacotes que simplificam a instalação de componentes complexos. Para obter uma comparação de pacotes NuGet e SDKs de extensão do Visual Studio, consulte [Adição de referências usando-se NuGet em comparação com um SDK de extensão](https://msdn.microsoft.com/library/jj161096.aspx) na Biblioteca MSDN.

## <a name="distribution-by-file-copy"></a>Distribuição por cópia do arquivo

Caso o componente consista em um único arquivo .winmd ou em um arquivo .winmd e um arquivo de índice de recurso (.pri), basta disponibilizar o arquivo .winmd para os usuários copiarem. Os usuários podem colocar o arquivo onde quiserem em um projeto, usar a caixa de diálogo **Adicionar Item Existente** para adicionar o arquivo .winmd ao projeto e usar a caixa de diálogo do Gerenciador de Referências para criar uma referência. Caso você inclua um arquivo .pri ou um arquivo .xml, instrua os usuários a colocarem esses arquivos com o arquivo .winmd.

> **Observação**Visual Studio sempre produz um arquivo. PRI quando você compila o componente de tempo de execução do Windows, mesmo se seu projeto não inclua recursos. Caso tenha um aplicativo de teste para o componente, você pode determinar se o arquivo .pri é usado examinando o conteúdo do pacote do aplicativo na pasta bin\\debug\\AppX. Caso o arquivo .pri no componente não seja exibido, você não precisa distribuí-lo. Também é possível usar a ferramenta [MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx) para despejar o arquivo de recurso do projeto do componente do Tempo de Execução do Windows. Por exemplo, na janela Prompt de Comando do Visual Studio, digite: makepri dump /if MyComponent.pri /of MyComponent.pri.xml É possível saber mais sobre arquivos .pri em [Sistema de Gerenciamento de Recursos (Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx).

## <a name="distribution-by-extension-sdk"></a>Distribuição pelo SDK de extensão

Um componente complexo normalmente inclui recursos do Windows, mas consulte a observação sobre como detectar arquivos .pri vazios na seção anterior.

**Para criar um SDK de extensão**

1.  Assegure-se de que você tenha o SDK do Visual Studio instalado. É possível baixar o SDK do Visual Studio na página [Downloads do Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs).
2.  Crie um novo projeto usando o modelo de projeto VSIX. É possível encontrar o modelo em Visual C# ou Visual Basic, na categoria de extensibilidade. Esse modelo é instalado como parte do SDK do Visual Studio. ([Procedimento passo a passo: criação de um SDK em C# ou Visual Basic](https://msdn.microsoft.com/library/jj127119.aspx) ou [Procedimento passo a passo: criação de um SDK usando C++](https://msdn.microsoft.com/library/jj127117.aspx) demonstra o uso desse modelo em um cenário muito simples. )
3.  Determine a estrutura de pastas do SDK. A estrutura de pastas começa no nível raiz do projeto VSIX, com as pastas **References**, **Redist** e **DesignTime**.

    -   **References** é o local para arquivos binários que os usuários podem programar. O SDK de extensão cria referências para esses arquivos em projetos do Visual Studio dos usuários.
    -   **Redist** é o local para outros arquivos que devem ser distribuídos com os arquivos binários, em aplicativos criados usando o componente.
    -   **DesignTime** é o local para os arquivos que só são usados quando os desenvolvedores estão criando aplicativos que usam o componente.

    Em cada uma dessas pastas, é possível criar pastas de configuração. Os nomes permitidos são debug, retail e CommonConfiguration. A pasta CommonConfiguration se destina a arquivos que sejam iguais, independentemente de serem usados por compilações de varejo ou depuração. Caso só esteja distribuindo compilações de varejo do componente, você pode colocar tudo em CommonConfiguration e omitir as outras duas pastas.

    Em cada pasta de configuração, é possível fornecer pastas de arquitetura para arquivos específicos da plataforma. Caso use os mesmos arquivos para todas as plataformas, você pode fornecer uma única pasta chamada neutral. É possível encontrar detalhes da estrutura de pastas, inclusive outros nomes de pasta de arquitetura, em [Como criar um Software Development Kit](https://msdn.microsoft.com/library/hh768146.aspx), na Biblioteca MSDN. (Este artigo aborda SDKs de plataforma e SDKs de extensão. Talvez seja útil recolher a seção sobre SDKs de plataforma, para evitar confusão. )

4.  Crie um arquivo de manifesto do SDK. O manifesto especifica informações de nome e versão, as arquiteturas compatíveis com o SDK, versões do .NET Framework e outras informações sobre a maneira como o Visual Studio usa o SDK. É possível encontrar detalhes e um exemplo em [Como criar um Software Development Kit](https://msdn.microsoft.com/library/hh768146.aspx).
5.  Compile e distribua o SDK de extensão. Para obter informações aprofundadas, inclusive a localização e a assinatura do pacote VSIX, consulte a Implantação VSIX na Biblioteca MSDN.

## <a name="related-topics"></a>Tópicos relacionados

* [Criando um Software Development Kit](https://msdn.microsoft.com/library/hh768146.aspx)
* [Sistema de gerenciamento de pacotes NuGet](https://github.com/NuGet/Home)
* [Sistema de Gerenciamento de Recursos (Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx)
* [Como encontrar e usar extensões do Visual Studio](https://msdn.microsoft.com/library/dd293638.aspx)
* [Opções do comando MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx)

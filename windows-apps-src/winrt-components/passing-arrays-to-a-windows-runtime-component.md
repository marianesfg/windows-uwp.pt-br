---
title: Passagem de matrizes para um componente do Tempo de Execução do Windows
description: Na Plataforma Universal do Windows (UWP), os parâmetros são de entrada ou de saída, jamais ambos. Isso significa que o conteúdo de uma matriz passada para um método, bem como a matriz propriamente dita, é de entrada ou de saída.
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 60c2e2221cd174ffd75a45d6fe8e2f66744d67a0
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8191911"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>Passagem de matrizes para um componente do Tempo de Execução do Windows




Na Plataforma Universal do Windows (UWP), os parâmetros são de entrada ou de saída, jamais ambos. Isso significa que o conteúdo de uma matriz passada para um método, bem como a matriz propriamente dita, é de entrada ou de saída. Caso o conteúdo da matriz seja de entrada, o método lê da matriz, mas não grava nela. Caso o conteúdo da matriz seja de saída, o método grava na matriz, mas não lê dela. Isso representa um problema para os parâmetros de matrizes, pois as matrizes na estrutura .NET são tipos de referencia e o conteúdo de uma matriz é mutável, mesmo quando a referência de matriz é passada por valor (**ByVal** no Visual Basic). A [Ferramenta de Exportação de Metadados do Windows Runtime (Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx) exige que você especifique o uso pretendido da matriz, se não for claro pelo contexto, aplicando o atributo ReadOnlyArrayAttribute ou WriteOnlyArrayAttribute para o parâmetro. O uso da matriz é determinado conforme o seguinte:

-   Para o valor de retorno ou para um parâmetro out (um parâmetro **ByRef** com o atributo [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx) no Visual Basic), a matriz é sempre apenas de saída. Não aplique o atributo ReadOnlyArrayAttribute. O atributo WriteOnlyArrayAttribute é permitido em parâmetros de saída, mas é redundante.

    > **Cuidado**compilador do Visual Basic não impõe regras de apenas saída. Nunca se deve ler a partir de um parâmetro de saída; ele pode conter **Nada**. Sempre atribua uma nova matriz.
 
-   Os parâmetros que tiverem o modificador **ref** (**ByRef** no Visual Basic) não são permitidos. Winmdexp.exe gera um erro.
-   Para um parâmetro passado por valor, você deve especificar se o conteúdo da matriz é de entrada ou saída aplicando o atributo [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx) ou [WriteOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx). Especificar ambos os atributos é um erro.

Caso um método deva aceitar uma matriz de entrada, modifique o conteúdo da matriz e devolva a matriz ao chamador, use um parâmetro somente leitura para a entrada e um parâmetro somente gravação (ou o valor de retorno) para a saída. O seguinte código mostra uma maneira de implementar esse padrão:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public int[] ChangeArray([ReadOnlyArray()] int[] input)
> {
>     int[] output = input.Clone();
>     // Manipulate the copy.
>     //   ...
>     return output;
> }
> ```
> ```vb
> Public Function ChangeArray(<ReadOnlyArray> input() As Integer) As Integer()
>     Dim output() As Integer = input.Clone()
>     ' Manipulate the copy.
>     '   ...
>     Return output
> End Function
> ```

Recomendamos criar uma cópia da matriz de entrada imediatamente e manipular a cópia. Isso ajuda a garantir que o método se comporte da mesma maneira, independentemente do componente ser chamado pelo código do .NET Framework.

## <a name="using-components-from-managed-and-unmanaged-code"></a>Como usar componentes de código gerenciado e não gerenciado


Parâmetros que tenham o atributo ReadOnlyArrayAttribute ou o atributo WriteOnlyArrayAttribute se comportam de maneira diferente dependendo do chamador estar gravado em código gerenciado ou nativo. Caso o chamador seja um código nativo (extensões de componente Visual C++ ou JavaScript), o conteúdo da matriz é tratado da seguinte maneira:

-   ReadOnlyArrayAttribute: a matriz é copiada quando a chamada atravessa o limite da interface binária do aplicativo (ABI). Os elementos são convertidos caso necessário. Portanto, qualquer alteração acidental feita pelo método em uma matriz somente de entrada não permanece visível para o chamador.
-   WriteOnlyArrayAttribute: o método chamado não pode fazer pressuposições sobre o conteúdo da matriz original. Por exemplo, a matriz que o método recebe não pode ser inicializada, ou pode conter valores padrão. O método é esperado para definir os valores de todos os elementos na matriz.

Caso o chamador seja um código gerenciado, a matriz original permanece disponível para o método chamado, como estaria em qualquer chamada de método no .NET Framework. O conteúdo da matriz é mutável no código do .NET Framework, logo, qualquer alteração que o método faça na matriz permanece visível para o chamador. É importante lembrar isso porque ele afeta testes de unidade escritos para um componente do Tempo de Execução do Windows. Se os testes forem escritos em código gerenciado, o conteúdo de uma matriz parecerá ser mutável durante o teste.

## <a name="related-topics"></a>Tópicos relacionados

* [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx)
* [WriteOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx)
* [Criando componentes do Tempo de Execução do Windows em C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)

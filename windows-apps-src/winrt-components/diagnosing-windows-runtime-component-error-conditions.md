---
title: Diagnóstico das condições de erro do componente do Windows Runtime
description: Este artigo fornece informações adicionais sobre restrições em Windows Runtime componentes escritos com código gerenciado.
ms.assetid: CD0D0E11-E68A-411D-B92E-E9DECFDC9599
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 55bf6360f09ba4ab6c7878543ecfa0c80c4558e3
ms.sourcegitcommit: 74c674c70b86bafeac7c8c749b1662fae838c428
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72252314"
---
# <a name="diagnosing-windows-runtime-component-error-conditions"></a>Diagnóstico das condições de erro do componente do Windows Runtime

Este artigo fornece informações adicionais sobre restrições em Windows Runtime componentes escritos com código gerenciado. Ele expande as informações fornecidas em mensagens de erro de [Winmdexp. exe (Windows Runtime ferramenta de exportação de metadados)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)e complementa as informações sobre as restrições fornecidas em [Windows Runtime componentes com C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

Este artigo não aborda todos os erros. Os erros debatidos aqui são agrupados por categoria geral, e cada categoria inclui uma tabela de mensagens de erro associadas. Procure o texto da mensagem (omitindo valores específicos de espaços reservados) ou o número da mensagem. Caso você não encontre as informações de que precisa aqui, ajude-nos a melhorar a documentação usando o botão de feedback ao final deste artigo. Inclua a mensagem de erro. Também é possível registrar um bug no site Microsoft Connect.

## <a name="error-message-for-implementing-async-interface-provides-incorrect-type"></a>A mensagem de erro de implementação da interface assíncrona apresenta um tipo incorreto

Os componentes do Windows Runtime gerenciado não podem implementar as interfaces Plataforma Universal do Windows (UWP) que representam ações ou operações assíncronas ([IAsyncAction](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction), [IAsyncActionWithProgress&lt;TProgress&gt;](https://docs.microsoft.com/previous-versions/br205784(v=vs.85)), [IAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)ou [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)). Em vez disso, o .NET fornece a classe [AsyncInfo](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime) para gerar operações assíncronas em componentes Windows Runtime. A mensagem de erro que Winmdexp.exe exibe quando você tenta implementar uma interface async se refere incorretamente a essa classe pelo nome anterior, AsyncInfoFactory. O .NET não inclui mais a classe AsyncInfoFactory.

| Número do erro | Texto da mensagem|       
|--------------|-------------|
| WME1084      | O tipo '{0}' implementa Windows Runtime interface assíncrona '{1}'. Os tipos de Windows Runtime não podem implementar interfaces assíncronas. Use a classe System.Runtime.InteropServices.WindowsRuntime.AsyncInfoFactory para gerar operações assíncronas a serem exportadas para o Tempo de Execução do Windows. |

> **Observe** as mensagens de erro que se referem ao Windows Runtime usar uma terminologia antiga. Ele agora é conhecido como a Plataforma Universal do Windows (UWP). Por exemplo, agora os tipos de Windows Runtime são chamados de tipos UWP.

## <a name="missing-references-to-mscorlibdll-or-systemruntimedll"></a>Referências não encontradas a mscorlib. dll ou System.Runtime.dll

Esse problema só ocorre quando você usa Winmdexp.exe na linha de comando. Recomendamos que você use a opção/reference para incluir referências a mscorlib. dll e System. Runtime. dll dos assemblies de referência do .NET Framework Core, que estão localizados em assemblies de referência do "% ProgramFiles (x86)%\\\\Microsoft\\Framework\\. NetCore\\v 4.5 "("% ProgramFiles%\\... " em um computador de 32 bits).

| Número do erro | Texto da mensagem                                                                                                                                     |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1009      | Nenhuma referência foi feita a mscorlib. Uma referência a esses metadados é necessária exportar corretamente.                               |
| WME1090      | Não foi possível determinar o assembly de referência básico. Assegure-se de que mscorlib.dll e System.Runtime.dll sejam referenciados usando a opção /reference. |

## <a name="operator-overloading-is-not-allowed"></a>Não é permitida a sobrecarga do operador

Em um componente do Windows Runtime escrito em código gerenciado, não é possível expor operadores sobrecarregados em tipos públicos.

> **Observação** na mensagem de erro, o operador é identificado por seu nome de metadados, como op\_adição, op\_multiplique, op\_exclusiver, op\_implícito (conversão implícita) e assim por diante.

| Número do erro | Texto da mensagem                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------|
| WME1087      | '{0}' é uma sobrecarga de operador. Os tipos gerenciados não podem expor sobrecargas do operador no Windows Runtime. |

## <a name="constructors-on-a-class-have-the-same-number-of-parameters"></a>Construtores em uma classe têm o mesmo número de parâmetros

No UWP, uma classe pode ter apenas um construtor com um determinado número de parâmetros; por exemplo, não é possível ter um construtor com um único parâmetro de tipo **String** e outro com um parâmetro único de tipo **int** (**Integer** no Visual Basic). A única solução alternativa é usar um número diferente de parâmetros para cada construtor.

| Número do erro | Texto da mensagem                                                                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1099      | O tipo '{0}' tem vários construtores com argumento '{1}'. Os tipos de Windows Runtime não podem ter vários construtores com o mesmo número de argumentos. |

## <a name="must-specify-a-default-for-overloads-that-have-the-same-number-of-parameters"></a>É preciso especificar um padrão para sobrecargas com o mesmo número de parâmetros

Na UWP, os métodos sobrecarregados só podem ter o mesmo número de parâmetros caso uma sobrecarga seja especificada como a sobrecarga padrão. Consulte "métodos sobrecarregados" em [Windows Runtime componentes com C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

| Número do erro | Texto da mensagem                                                                                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1059      | Várias sobrecargas de {0}parâmetros de '{1}.{2}' são decorados com Windows. Foundation. Metadata. defaultoverloadattribute.                                                            |
| WME1085      | As sobrecargas de {0}parâmetro de {1}.{2} deve ter exatamente um método especificado como a sobrecarga padrão decorando-o com Windows. Foundation. Metadata. defaultoverloadattribute. |

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>Erros de namespace e nomes inválidos para o arquivo de saída

Na Plataforma Universal do Windows, todos os tipos públicos em um arquivo de metadados do Windows (. winmd) devem estar em um namespace que compartilha o nome do arquivo .winmd ou em subnamespaces do nome do arquivo. Por exemplo, caso o projeto do Visual Studio se chame A.B (ou seja, o componente do Windows Runtime é A.B.winmd), ele pode conter classes públicas A.B.Class1 e A.B.C.Class2, mas não A.Class3 (WME0006) ou D.Class4 (WME1044).

> **Observação**  Essas restrições só se aplicam a tipos públicos, e não a tipos privados usados na implementação.

No caso de A.Class3, é possível mover Class3 para outro namespace ou alterar o nome do componente do Windows Runtime para A.winmd. Embora WME0006 seja um aviso, você deve tratá-lo como um erro. No exemplo anterior, o código que chama A.B.winmd não conseguirá localizar A.Class3.

No caso de D.Class4, nenhum nome de arquivo pode conter D.Class4 e as classes no namespace A.B, logo, alterar o nome do componente do Windows Runtime não é uma opção. Você pode mover D.Class4 para outro namespace ou colocá-lo em outro componente do Windows Runtime.

O sistema de arquivos não consegue diferenciar maiúsculas e minúsculas, logo, namespaces com o uso de maiúsculas diferente não são permitidas (WME1067).

O componente deve conter pelo menos um tipo **public sealed** (**Public NotInheritable** no Visual Basic). Do contrário, você obterá WME1042 ou WME1043, se o componente contiver tipos privados.

Um tipo em um componente do Windows Runtime não pode ter um nome que seja igual ao de um namespace (WME1068).

> **Cuidado**  Caso você chame Winmdexp.exe diretamente e não use a opção /out para especificar um nome para o componente do Windows Runtime, Winmdexp.exe tenta gerar um nome que inclua todos os namespaces no componente. Renomear namespaces pode alterar o nome do componente.

 

| Número do erro | Texto da mensagem                                                                                                                                                                                                                                                                                                                                             |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME0006      | '{0}' não é um nome de arquivo winmd válido para este assembly. Todos os tipos dentro de um arquivo de metadados do Windows devem estar em um subnamespace do namespace implícito do nome do arquivo. Os tipos que não existem nesse subnamespace não podem ser localizados no runtime. Neste assembly, o menor namespace comum que pode servir como um nome de arquivo é '{1}'. |
| WME1042      | O módulo de entrada deve conter pelo menos um tipo público localizado dentro de um namespace.                                                                                                                                                                                                                                                                   |
| WME1043      | O módulo de entrada deve conter pelo menos um tipo público localizado dentro de um namespace. Os únicos tipos encontrados dentro de namespaces são privados.                                                                                                                                                                                                               |
| WME1044      | Um tipo público tem um namespace ('{1}') que não compartilha nenhum prefixo comum com outros namespaces ('{0}'). Todos os tipos dentro de um arquivo de metadados do Windows devem estar em um subnamespace do namespace implícito do nome do arquivo.                                                                                                                              |
| WME1067      | Os nomes de namespace não podem diferir somente por maiúsculas/minúsculas: '{0}', '{1}'.                                                                                                                                                                                                                                                                                                |
| WME1068      | O tipo '{0}' não pode ter o mesmo nome que o namespace '{1}'.                                                                                                                                                                                                                                                                                                 |

## <a name="exporting-types-that-arent-valid-universal-windows-platform-types"></a>Exportação de tipos que não sejam tipos da Plataforma Universal do Windows válidos

A interface pública do componente deve expor somente tipos UWP. No entanto, o .NET fornece mapeamentos para vários tipos comumente usados que são ligeiramente diferentes no .NET e no UWP. Isso permite que o desenvolvedor do .NET trabalhe com tipos conhecidos em vez de aprender novos. Você pode usar esses tipos .NET mapeados na interface pública do seu componente. Consulte "declarando tipos em componentes Windows Runtime" e "passando tipos de Plataforma Universal do Windows para código gerenciado" em [componentes de Windows Runtime com C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)e [mapeamentos .net de tipos de Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

Muitos desses mapeamentos são interfaces. Por exemplo, [IList&lt;T&gt;](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1) é mapeado para a interface UWP [IVector&lt;T&gt;](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IVector_T_). Caso você use List&lt;string&gt; (`List(Of String)` em Visual Basic) em vez de IList&lt;string&gt; como tipo de parâmetro, Winmdexp.exe oferece uma lista de alternativas que inclui todas as interfaces mapeadas implementadas por List&lt;T&gt;. Caso você use tipos genéricos aninhados, como List&lt;Dictionary&lt;int, string&gt;&gt; (List(Of Dictionary(Of Integer, String)) em Visual Basic), Winmdexp.exe oferece opções para cada nível de aninhamento. Essas listas podem ficar muito longas.

Em geral, a melhor opção é a interface mais próxima do tipo. Por exemplo, para Dictionary&lt;int, string&gt;, a melhor opção é mais provavelmente IDictionary&lt;int, string&gt;.

> **Importante**  O JavaScript usa a primeira interface exibida na lista de interfaces implementadas por um tipo gerenciado. Por exemplo, se você retornar Dictionary&lt;int, string&gt; ao código JavaScript, ele será exibido como IDictionary&lt;int, string&gt;, independentemente de qual interface você especificar como o tipo de retorno. Isso significa que, se a primeira interface não incluir um membro exibido em interfaces posteriores, esse membro não permanecerá visível para JavaScript.

> **Cuidado**  Evite usar as interfaces [IList](https://docs.microsoft.com/dotnet/api/system.collections.ilist) e [IEnumerable](https://docs.microsoft.com/dotnet/api/system.collections.ienumerable) não genéricas se o componente for usado pelo JavaScript. Essas interfaces são mapeadas para [IBindableVector](https://docs.microsoft.com/uwp/api/windows.ui.xaml.interop.ibindablevector) e [IBindableIterator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.interop.ibindableiterator), respectivamente. Elas dão suporte à associação de controles XAML e permanecem invisíveis para JavaScript. O JavaScript emite o erro de tempo de execução "A função 'X' tem uma assinatura inválida e não pode ser chamada".

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Número do erro</th>
<th align="left">Texto da mensagem</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">WME1033</td>
<td align="left">O método '{0}' tem o parâmetro '{1}' do tipo '{2}'. '{2}' não é um tipo de parâmetro de Windows Runtime válido.</td>
</tr>
<tr class="even">
<td align="left">WME1038</td>
<td align="left">O método '{0}' tem um parâmetro do tipo '{1}' em sua assinatura. Embora esse tipo não seja um tipo de Windows Runtime válido, ele implementa interfaces que são tipos de Windows Runtime válidos. Leve em consideração alterar a assinatura do método para usar um dos seguintes tipos: '{2}'.</td>
</tr>
<tr class="odd">
<td align="left">WME1039</td>
<td align="left"><p>O método '{0}' tem um parâmetro do tipo '{1}' em sua assinatura. Embora esse tipo genérico não seja um tipo de Windows Runtime válido, o tipo ou os parâmetros genéricos implementam interfaces que são tipos de Windows Runtime válidos. {2}</p>
> **Observação**  Por {2}, Winmdexp. exe acrescenta uma lista de alternativas, como "considere alterar o tipo ' System. Collections. Generic. List&lt;T&gt;' na assinatura do método para um dos seguintes tipos em vez disso: ' System. Collections. Generic. IList&lt;T&gt;, System. Collections. Generic. IReadOnlyList&lt;T&gt;, System. Collections. Generic. IEnumerable&lt;T&gt;'."
</td>
</tr>
<tr class="even">
<td align="left">WME1040</td>
<td align="left">O método '{0}' tem um parâmetro do tipo '{1}' em sua assinatura. Em vez de usar um tipo de tarefa gerenciado, use Windows.Foundation.IAsyncAction, Windows.Foundation.IAsyncOperation ou uma das outras interfaces assíncronas de Windows Runtime. O padrão de espera .NET também se aplica a essas interfaces. Por favor, consulte System.Runtime.InteropServices.WindowsRuntime.AsyncInfo para obter mais informações sobre como converter objetos de tarefa gerenciada em interfaces assíncronas de Tempo de Execução do Windows.</td>
</tr>
</tbody>
</table>

 

## <a name="structures-that-contain-fields-of-disallowed-types"></a>Estruturas que contêm campos de tipos não permitidos


Na UWP, uma estrutura só pode conter campos, e apenas estruturas podem conter campos. Esses campos devem ser públicos. Entre os tipos de campo válidos estão enumerações, estruturas e tipos primitivos.

| Número do erro | Texto da mensagem                                                                                                                                                                                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1060      | A estrutura '{0}' tem o campo '{1}' do tipo '{2}'. '{2}' não é um tipo de campo de Windows Runtime válido. Cada campo em uma estrutura de Windows Runtime só pode ser UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String, Enum, ou a própria estrutura. |

 

## <a name="restrictions-on-arrays-in-member-signatures"></a>Restrições de matrizes em assinaturas de membro


Na UWP, as matrizes em assinaturas de membro devem ser unidimensionais com um limite inferior de 0 (zero). Tipos de matrizes aninhados como `myArray[][]` (`myArray()()` em Visual Basic) não são permitidos.

> **Observação** essa restrição não se aplica a matrizes que você usa internamente em sua implementação.

 

| Número do erro | Texto da mensagem                                                                                                                                                     |
|--------------|--------------------|
| WME1034      | O método '{0}' tem uma matriz do tipo '{1}' com limite inferior diferente de zero em sua assinatura. As matrizes de assinaturas do método de Windows Runtime devem ter um limite mínimo de zero. |
| WME1035      | O método '{0}' tem uma matriz multidimensional do tipo '{1}' em sua assinatura. As matrizes em assinaturas do método de Windows Runtime devem ser unidimensionais.                  |
| WME1036      | O método '{0}' tem uma matriz aninhada do tipo '{1}' em sua assinatura. As matrizes em assinaturas do Windows Runtime não podem ser aninhadas.                                    |

 

## <a name="array-parameters-must-specify-whether-array-contents-are-readable-or-writable"></a>Os parâmetros de matriz devem especificar se o conteúdo da matriz é legível ou gravável


Na UWP, os parâmetros devem ser somente leitura ou somente gravação. Os parâmetros não podem ser marcados **ref** (**ByRef** sem o atributo [OutAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.outattribute) em Visual Basic). Isso se aplica ao conteúdo das matrizes, logo, os parâmetros da matriz devem indicar se o conteúdo da matriz é somente leitura ou somente gravação. A direção é clara para parâmetros **out** (parâmetro **ByRef** com o atributo OutAttribute no Visual Basic), mas parâmetros de matriz passados por valor (ByVal no Visual Basic) devem ser marcados. Consulte [Passagem de matrizes para um componente do Windows Runtime](passing-arrays-to-a-windows-runtime-component.md).

| Número do erro | Texto da mensagem         |
|--------------|----------------------|
| WME1101      | O método '{0}' tem o parâmetro '{1}', que é uma matriz e que tem {2} e {3}. No Windows Runtime, os parâmetros de matriz de conteúdo devem ser legíveis ou graváveis. Remova um dos atributos de '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| WME1102      | O método '{0}' tem um parâmetro de saída '{1}', que é uma matriz, mas que tem {2}. No Windows Runtime, o conteúdo das matrizes de saída é gravável. Remova o atributo de '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| WME1103      | O método '{0}' tem o parâmetro '{1}', que é uma matriz e que tem um System. Runtime. InteropServices. InAttribute ou um System. Runtime. InteropServices. OutAttribute. No Windows Runtime, os parâmetros de matriz devem ter {3} ou {3}. Remova esses atributos ou os substitua pelo atributo de Windows Runtime apropriado, se necessário.                                                                                                                                                                                                                                                                                                                                                                                          |
| WME1104      | O método '{0}' tem o parâmetro '{1}', que não é uma matriz e que tem um {2} ou um {3}. O Windows Runtime não dá suporte à marcação de parâmetros não matriz com {3} ou {3}.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| WME1105      | O método '{0}' tem o parâmetro '{1}' com um System. Runtime. InteropServices. InAttribute ou System. Runtime. InteropServices. OutAttribute. O Tempo de Execução do Windows não dá suporte à marcação de parâmetros com System.Runtime.InteropServices.InAttribute ou System.Runtime.InteropServices.OutAttribute. Leve em consideração a remoção de System.Runtime.InteropServices.InAttribute e substitua System.Runtime.InteropServices.OutAttribute pelo modificador 'out' em vez disso. O método '{0}' tem o parâmetro '{1}' com um System. Runtime. InteropServices. InAttribute ou System. Runtime. InteropServices. OutAttribute. O Tempo de Execução do Windows só dá suporte à marcação de parâmetros ByRef com System.Runtime.InteropServices.OutAttribute e não a outros usos desses atributos. |
| WME1106      | O método '{0}' tem o parâmetro '{1}', que é uma matriz. No Windows Runtime, o conteúdo dos parâmetros de matriz deve ser legível ou gravável. Aplique {1} ou {1} a '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


## <a name="member-with-a-parameter-named-value"></a>Membro com um parâmetro chamado "value"


Na UWP, os valores de retorno são considerados parâmetros de saída e os nomes dos parâmetros devem ser exclusivos. Por padrão, Winmdexp.exe dá ao valor de retorno o nome "value". Se o método tiver um parâmetro chamado "value", você receberá o erro WME1092. Existem duas maneiras de corrigir isso:

-   Dê ao parâmetro um nome diferente de "value" (em acessadores de propriedade, um nome diferente de "returnValue").
-   Use o atributo ReturnValueNameAttribute para alterar o nome do valor de retorno, conforme mostrado aqui:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > using System.Runtime.InteropServices;
    > using System.Runtime.InteropServices.WindowsRuntime;
    >
    > [return: ReturnValueName("average")]
    > public int GetAverage(out int lowValue, out int highValue)
    > ```
    > ```vb
    > Imports System.Runtime.InteropServices
    > Imports System.Runtime.InteropServices.WindowsRuntime
    >
    > Public Function GetAverage(<Out> ByRef lowValue As Integer, _
    > <Out> ByRef highValue As Integer) As <ReturnValueName("average")> String
    > ```

> **Observação**  Se alterar o nome do valor de retorno e o novo nome colidir com o nome de outro parâmetro, você obterá o erro WME1091.

O código JavaScript pode acessar os parâmetros de saída de um método por nome, inclusive o valor de retorno. Por exemplo, consulte o atributo [ReturnValueNameAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.returnvaluenameattribute).

| Número do erro | Texto da mensagem |
|--------------|--------------|
| WME1091 | O método '\{0} ' tem o valor de retorno denominado '\{1} ', que é o mesmo que um nome de parâmetro. Os parâmetros de método de Windows Runtime e o valor de retorno devem ter nomes exclusivos. |
| WME1092 | O método '\{0} ' tem um parâmetro denominado '\{1} ', que é o mesmo que o nome do valor de retorno padrão. Leve em consideração usar outro nome para o parâmetro ou usar o System.Runtime.InteropServices.WindowsRuntime.ReturnValueNameAttribute para especificar explicitamente o nome do valor de retorno. |

**Observação**  O nome padrão é "returnValue" para acessadores de propriedade e "value" para todos os outros métodos.

## <a name="related-topics"></a>Tópicos relacionados

* [Componentes do Windows Runtime com C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Winmdexp. exe (Windows Runtime ferramenta de exportação de metadados)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)

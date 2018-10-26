---
author: msatranjr
title: Diagnóstico das condições de erro do componente do Tempo de Execução do Windows
description: Este artigo fornece informações adicionais sobre restrições em componentes do Tempo de Execução do Windows escritos com código gerenciado.
ms.assetid: CD0D0E11-E68A-411D-B92E-E9DECFDC9599
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 833dd0a6447e9d0bb49c21a18d17bd7b0dc3455d
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5640422"
---
# <a name="diagnosing-windows-runtime-component-error-conditions"></a>Diagnóstico das condições de erro do componente do Tempo de Execução do Windows




Este artigo fornece informações adicionais sobre restrições em componentes do Tempo de Execução do Windows escritos com código gerenciado. Ele expande as informações fornecidas em mensagens de erro da [Winmdexp.exe (Ferramenta de Exportação de Metadados do Tempo de Execução do Windows)](https://msdn.microsoft.com/library/hh925576.aspx) e complementa as informações sobre as restrições fornecidas em [Criação de componentes de Tempo de Execução do Windows em C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

Este artigo não aborda todos os erros. Os erros debatidos aqui são agrupados por categoria geral, e cada categoria inclui uma tabela de mensagens de erro associadas. Procure o texto da mensagem (omitindo valores específicos de espaços reservados) ou o número da mensagem. Caso você não encontre as informações de que precisa aqui, ajude-nos a melhorar a documentação usando o botão de feedback ao final deste artigo. Inclua a mensagem de erro. Também é possível registrar um bug no site Microsoft Connect.

## <a name="error-message-for-implementing-async-interface-provides-incorrect-type"></a>A mensagem de erro de implementação da interface assíncrona apresenta um tipo incorreto


Os componentes do Tempo de Execução do Windows gerenciados não podem implementar as interfaces da UWP (Plataforma Universal do Windows) que representam ações ou operações assíncronas ([IAsyncAction](https://msdn.microsoft.com/library/br205781.aspx), [IAsyncActionWithProgress&lt;TProgress&gt;](https://msdn.microsoft.com/library/br205784.aspx), [IAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br206598.aspx) ou [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/windows/apps/br206594.aspx)). Em vez disso, o .NET Framework fornece a classe [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) para gerar operações assíncronas em componentes do Tempo de Execução do Windows. A mensagem de erro que Winmdexp.exe exibe quando você tenta implementar uma interface async se refere incorretamente a essa classe pelo nome anterior, AsyncInfoFactory. O .NET Framework não inclui mais a classe AsyncInfoFactory.

| Número do erro | Texto da mensagem|       
|--------------|-------------|
| WME1084      | Tipo '{0}'implementa a interface assíncrona de tempo de execução do Windows'{1}'. Os tipos de Tempo de Execução do Windows não podem implementar interfaces assíncronas. Use a classe System.Runtime.InteropServices.WindowsRuntime.AsyncInfoFactory para gerar operações assíncronas a serem exportadas para o Tempo de Execução do Windows. |

> **Observação**as mensagens de erro que se referem ao Windows Runtime usam uma terminologia anterior. Ele agora é conhecido como a Plataforma Universal do Windows (UWP). Por exemplo, agora os tipos de Tempo de Execução do Windows são chamados de tipos UWP.

 

## <a name="missing-references-to-mscorlibdll-or-systemruntimedll"></a>Referências não encontradas a mscorlib. dll ou System.Runtime.dll


Esse problema só ocorre quando você usa Winmdexp.exe na linha de comando. Recomendamos usar a opção /reference para incluir referências a mscorlib. dll e System.Runtime.dll nos assemblies de referência básicos do .NET Framework, que estão em "%ProgramFiles(x86)%\\Reference Assemblies\\Microsoft\\Framework\\.NETCore\\v4.5" ("%ProgramFiles%\\..." em um computador 32 bits).

| Número do erro | Texto da mensagem                                                                                                                                     |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1009      | Nenhuma referência foi feita a mscorlib. Uma referência a esses metadados é necessária exportar corretamente.                               |
| WME1090      | Não foi possível determinar o assembly de referência básico. Assegure-se de que mscorlib.dll e System.Runtime.dll sejam referenciados usando a opção /reference. |

 

## <a name="operator-overloading-is-not-allowed"></a>Não é permitida a sobrecarga do operador


Em um componente do Tempo de Execução do Windows escrito em código gerenciado, não é possível expor operadores sobrecarregados em tipos públicos.

> **Observação**na mensagem de erro, o operador é identificado pelo nome de metadados, como op\_Addition, op\_Multiply, op\_ExclusiveOr, op\_Implicit (conversão implícita) e assim por diante.

 

| Número do erro | Texto da mensagem                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------|
| WME1087      | '{0}' é uma sobrecarga de operador. Os tipos gerenciados não podem expor sobrecargas do operador no Tempo de Execução do Windows. |

 

## <a name="constructors-on-a-class-have-the-same-number-of-parameters"></a>Construtores em uma classe têm o mesmo número de parâmetros


No UWP, uma classe pode ter apenas um construtor com um determinado número de parâmetros; por exemplo, não é possível ter um construtor com um único parâmetro de tipo **String** e outro com um parâmetro único de tipo **int** (**Integer** no Visual Basic). A única solução alternativa é usar um número diferente de parâmetros para cada construtor.

| Número do erro | Texto da mensagem                                                                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1099      | Tipo '{0}'tem vários construtores com'{1}' argumento (s). Os tipos de Tempo de Execução do Windows não podem ter vários construtores com o mesmo número de argumentos. |

 

## <a name="must-specify-a-default-for-overloads-that-have-the-same-number-of-parameters"></a>É preciso especificar um padrão para sobrecargas com o mesmo número de parâmetros


Na UWP, os métodos sobrecarregados só podem ter o mesmo número de parâmetros caso uma sobrecarga seja especificada como a sobrecarga padrão. Consulte "Métodos sobrecarregados" em [Criação de componentes de Tempo de Execução do Windows em C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

| Número do erro | Texto da mensagem                                                                                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1059      | Vários {0}-parâmetro sobrecargas de '{1}. {2}' são decoradas com defaultoverloadattribute.                                                            |
| WME1085      | O {0}-parâmetro sobrecargas de {1}. {2} devem ter exatamente um método especificado como a sobrecarga padrão decorando-o com defaultoverloadattribute. |

 

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>Erros de namespace e nomes inválidos para o arquivo de saída


Na Plataforma Universal do Windows, todos os tipos públicos em um arquivo de metadados do Windows (. winmd) devem estar em um namespace que compartilha o nome do arquivo .winmd ou em subnamespaces do nome do arquivo. Por exemplo, caso o projeto do Visual Studio se chame A.B (ou seja, o componente do Tempo de Execução do Windows é A.B.winmd), ele pode conter classes públicas A.B.Class1 e A.B.C.Class2, mas não A.Class3 (WME0006) ou D.Class4 (WME1044).

> **Observação**essas restrições se aplicam somente a tipos públicos, não a tipos privados usados na sua implementação.

 

No caso de A.Class3, é possível mover Class3 para outro namespace ou alterar o nome do componente do Tempo de Execução do Windows para A.winmd. Embora WME0006 seja um aviso, você deve tratá-lo como um erro. No exemplo anterior, o código que chama A.B.winmd não conseguirá localizar A.Class3.

No caso de D.Class4, nenhum nome de arquivo pode conter D.Class4 e as classes no namespace A.B, logo, alterar o nome do componente do Tempo de Execução do Windows não é uma opção. Você pode mover D.Class4 para outro namespace ou colocá-lo em outro componente do Tempo de Execução do Windows.

O sistema de arquivos não consegue diferenciar maiúsculas e minúsculas, logo, namespaces com o uso de maiúsculas diferente não são permitidas (WME1067).

O componente deve conter pelo menos um tipo **public sealed** (**Public NotInheritable** no Visual Basic). Do contrário, você obterá WME1042 ou WME1043, se o componente contiver tipos privados.

Um tipo em um componente do Tempo de Execução do Windows não pode ter um nome que seja igual ao de um namespace (WME1068).

> **Cuidado**se você chamar Winmdexp.exe diretamente e não use a opção /out para especificar um nome para o componente de tempo de execução do Windows, Winmdexp.exe tenta gerar um nome que inclua todos os namespaces no componente. Renomear namespaces pode alterar o nome do componente.

 

| Número do erro | Texto da mensagem                                                                                                                                                                                                                                                                                                                                             |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME0006      | '{0}' não é um nome de arquivo winmd válido para este assembly. Todos os tipos dentro de um arquivo de metadados do Windows devem estar em um subnamespace do namespace implícito do nome do arquivo. Os tipos que não existem nesse subnamespace não podem ser localizados no tempo de execução. Neste assembly, o menor namespace comum que pode servir como um nome de arquivo é '{1}'. |
| WME1042      | O módulo de entrada deve conter pelo menos um tipo público localizado dentro de um namespace.                                                                                                                                                                                                                                                                   |
| WME1043      | O módulo de entrada deve conter pelo menos um tipo público localizado dentro de um namespace. Os únicos tipos encontrados dentro de namespaces são privados.                                                                                                                                                                                                               |
| WME1044      | Um tipo público tem um namespace ('{1}') que compartilha sem prefixo comum com outros namespaces ('{0}'). Todos os tipos dentro de um arquivo de metadados do Windows devem estar em um subnamespace do namespace implícito do nome do arquivo.                                                                                                                              |
| WME1067      | Nomes de Namespace não podem ser diferentes apenas pelo uso de maiusculas: '{0}','{1}'.                                                                                                                                                                                                                                                                                                |
| WME1068      | Tipo '{0}'não pode ter o mesmo nome do namespace'{1}'.                                                                                                                                                                                                                                                                                                 |

 

## <a name="exporting-types-that-arent-valid-universal-windows-platform-types"></a>Exportação de tipos que não sejam tipos da Plataforma Universal do Windows válidos


A interface pública do componente deve expor somente tipos UWP. No entanto, o .NET Framework fornece mapeamentos para vários tipos mais usados que sejam um pouco diferentes no .NET Framework e na UWP. Isso permite que o desenvolvedor do .NET Framework trabalhe com tipos familiares, em vez de aprender novos. É possível usar esses tipos do .NET Framework mapeados na interface pública do componente. Veja "Declaração de tipos em componentes do Tempo de Execução do Windows" e "Passagem de tipos de Plataforma Universal do Windows para código gerenciado" em [Criação de componentes do Tempo de Execução do Windows em C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) e [Mapeamentos do .NET Framework dos tipos de Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

Muitos desses mapeamentos são interfaces. Por exemplo, [IList&lt;T&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) é mapeado para a interface UWP [IVector&lt;T&gt;](https://msdn.microsoft.com/library/windows/apps/br206631.aspx). Caso você use List&lt;string&gt; (`List(Of String)` em Visual Basic) em vez de IList&lt;string&gt; como tipo de parâmetro, Winmdexp.exe oferece uma lista de alternativas que inclui todas as interfaces mapeadas implementadas por List&lt;T&gt;. Caso você use tipos genéricos aninhados, como List&lt;Dictionary&lt;int, string&gt;&gt; (List(Of Dictionary(Of Integer, String)) em Visual Basic), Winmdexp.exe oferece opções para cada nível de aninhamento. Essas listas podem ficar muito longas.

Em geral, a melhor opção é a interface mais próxima do tipo. Por exemplo, para Dictionary&lt;int, string&gt;, a melhor opção é mais provavelmente IDictionary&lt;int, string&gt;.

> **Importante**JavaScript usa a primeira interface exibida na lista de interfaces implementadas por um tipo gerenciado. Por exemplo, se você retornar Dictionary&lt;int, string&gt; ao código JavaScript, ele será exibido como IDictionary&lt;int, string&gt;, independentemente de qual interface você especificar como o tipo de retorno. Isso significa que, caso a primeira interface não inclua um membro exibido em interfaces posteriores, esse membro não permanece visível para JavaScript.

> **Cuidado**Evite usar as interfaces não genéricas [IList](https://msdn.microsoft.com/library/system.collections.ilist.aspx) e [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) se o componente for usado pelo JavaScript. Essas interfaces são mapeadas para [IBindableVector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindablevector.aspx) e [IBindableIterator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindableiterator.aspx), respectivamente. Elas dão suporte à associação de controles XAML e permanecem invisíveis para JavaScript. O JavaScript emite o erro de tempo de execução "A função 'X' tem uma assinatura inválida e não pode ser chamada".

 

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
<td align="left">Método '{0}'tem um parâmetro'{1}'do tipo'{2}'. '{2}' não é um tipo de parâmetro de tempo de execução do Windows válido.</td>
</tr>
<tr class="even">
<td align="left">WME1038</td>
<td align="left">Método '{0}'tem um parâmetro do tipo'{1}' na assinatura. Embora esse tipo não seja um tipo de Tempo de Execução do Windows válido, ele implementa interfaces que são tipos de Tempo de Execução do Windows válidos. Considere alterar a assinatura do método para usar um dos seguintes tipos em vez disso: '{2}'.</td>
</tr>
<tr class="odd">
<td align="left">WME1039</td>
<td align="left"><p>Método '{0}'tem um parâmetro do tipo'{1}' na assinatura. Embora esse tipo genérico não seja um tipo de Tempo de Execução do Windows válido, o tipo ou os parâmetros genéricos implementam interfaces que são tipos de Tempo de Execução do Windows válidos. {2}</p>
> **Observação**para {2}, Winmdexp.exe acrescenta uma lista de alternativas, como "considere a alteração do tipo ' Collections&lt;T&gt;' na assinatura do método para um dos seguintes tipos em vez disso: ' System.Collections.Generic.IList&lt;T&gt;, System.Collections.Generic.IReadOnlyList&lt;T&gt;, System.Collections.Generic.IEnumerable&lt;T&gt;'. "
</td>
</tr>
<tr class="even">
<td align="left">WME1040</td>
<td align="left">Método '{0}'tem um parâmetro do tipo'{1}' na assinatura. Em vez de usar um tipo de tarefa gerenciado, use Windows.Foundation.IAsyncAction, Windows.Foundation.IAsyncOperation ou uma das outras interfaces assíncronas de Tempo de Execução do Windows. O padrão de espera .NET também se aplica a essas interfaces. Por favor, consulte System.Runtime.InteropServices.WindowsRuntime.AsyncInfo para obter mais informações sobre como converter objetos de tarefa gerenciada em interfaces assíncronas de Tempo de Execução do Windows.</td>
</tr>
</tbody>
</table>

 

## <a name="structures-that-contain-fields-of-disallowed-types"></a>Estruturas que contêm campos de tipos não permitidos


Na UWP, uma estrutura só pode conter campos, e apenas estruturas podem conter campos. Esses campos devem ser públicos. Entre os tipos de campo válidos estão enumerações, estruturas e tipos primitivos.

| Número do erro | Texto da mensagem                                                                                                                                                                                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1060      | Estrutura '{0}'tem o campo'{1}'do tipo'{2}'. '{2}' não é um tipo de campo de tempo de execução do Windows válido. Cada campo em uma estrutura de Tempo de Execução do Windows só pode ser UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String, Enum, ou a própria estrutura. |

 

## <a name="restrictions-on-arrays-in-member-signatures"></a>Restrições de matrizes em assinaturas de membro


Na UWP, as matrizes em assinaturas de membro devem ser unidimensionais com um limite inferior de 0 (zero). Tipos de matrizes aninhados como `myArray[][]` (`myArray()()` em Visual Basic) não são permitidos.

> **Observação**essa restrição não se aplica a matrizes usadas internamente na implementação.

 

| Número do erro | Texto da mensagem                                                                                                                                                     |
|--------------|--------------------|
| WME1034      | Método '{0}'tem uma matriz do tipo'{1}' com o limite inferior diferente de zero na assinatura. As matrizes de assinaturas do método de Tempo de Execução do Windows devem ter um limite mínimo de zero. |
| WME1035      | Método '{0}'tem uma matriz multidimensional do tipo'{1}' na assinatura. As matrizes em assinaturas do método de Tempo de Execução do Windows devem ser unidimensionais.                  |
| WME1036      | Método '{0}'tem uma matriz aninhada do tipo'{1}' na assinatura. As matrizes em assinaturas do Tempo de Execução do Windows não podem ser aninhadas.                                    |

 

## <a name="array-parameters-must-specify-whether-array-contents-are-readable-or-writable"></a>Os parâmetros de matriz devem especificar se o conteúdo da matriz é legível ou gravável


Na UWP, os parâmetros devem ser somente leitura ou somente gravação. Os parâmetros não podem ser marcados **ref** (**ByRef** sem o atributo [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx) em Visual Basic). Isso se aplica ao conteúdo das matrizes, logo, os parâmetros da matriz devem indicar se o conteúdo da matriz é somente leitura ou somente gravação. A direção é clara para parâmetros **out** (parâmetro **ByRef** com o atributo OutAttribute no Visual Basic), mas parâmetros de matriz passados por valor (ByVal no Visual Basic) devem ser marcados. Consulte [Passagem de matrizes para um componente do Tempo de Execução do Windows](passing-arrays-to-a-windows-runtime-component.md).

| Número do erro | Texto da mensagem         |
|--------------|----------------------|
| WME1101      | Método '{0}'tem um parâmetro'{1}' que é uma matriz, e que tem {2} e {3}. No Tempo de Execução do Windows, os parâmetros de matriz de conteúdo devem ser legíveis ou graváveis. Remova um dos atributos de '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| WME1102      | Método '{0}'tem um parâmetro de saída'{1}' que é uma matriz, mas que tem {2}. No Tempo de Execução do Windows, o conteúdo das matrizes de saída é gravável. Remova o atributo de '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| WME1103      | Método '{0}'tem um parâmetro'{1}' que é uma matriz, e que tem InAttribute ou OutAttribute. No Windows Runtime, parâmetros de matriz devem ter {2} ou {3}. Remova esses atributos ou os substitua pelo atributo de Tempo de Execução do Windows apropriado, se necessário.                                                                                                                                                                                                                                                                                                                                                                                          |
| WME1104      | Método '{0}'tem um parâmetro'{1}' que não é uma matriz, e que tem uma {2} ou um {3}. Tempo de execução do Windows não dá suporte à marcação de parâmetros não matriz com {2} ou {3}.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| WME1105      | Método '{0}'tem um parâmetro'{1}' com um InAttribute ou OutAttribute. O Tempo de Execução do Windows não dá suporte à marcação de parâmetros com System.Runtime.InteropServices.InAttribute ou System.Runtime.InteropServices.OutAttribute. Leve em consideração a remoção de System.Runtime.InteropServices.InAttribute e substitua System.Runtime.InteropServices.OutAttribute pelo modificador 'out' em vez disso. Método '{0}'tem um parâmetro'{1}' com um InAttribute ou OutAttribute. O Tempo de Execução do Windows só dá suporte à marcação de parâmetros ByRef com System.Runtime.InteropServices.OutAttribute e não a outros usos desses atributos. |
| WME1106      | Método '{0}'tem um parâmetro'{1}' que é uma matriz. No Tempo de Execução do Windows, o conteúdo dos parâmetros de matriz deve ser legível ou gravável. Aplique {2} ou {3} para '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


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

> **Observação**se você alterar o nome do valor de retorno e o novo nome colidir com o nome de outro parâmetro, você receberá o erro WME1091.

O código JavaScript pode acessar os parâmetros de saída de um método por nome, inclusive o valor de retorno. Por exemplo, consulte o atributo [ReturnValueNameAttribute](https://msdn.microsoft.com/library/windows/apps/system.runtime.interopservices.windowsruntime.returnvaluenameattribute.aspx).

| Número do erro | Texto da mensagem |
|--------------|--------------|
| WME1091 | O método ' \{0}' tem o valor de retorno chamado ' \{1}' que é o mesmo que um nome de parâmetro. Os parâmetros de método de Tempo de Execução do Windows e o valor de retorno devem ter nomes exclusivos. |
| WME1092 | O método ' \{0}' tem um parâmetro chamado ' \{1}' que é o mesmo que o padrão nome do valor de retorno. Leve em consideração usar outro nome para o parâmetro ou usar o System.Runtime.InteropServices.WindowsRuntime.ReturnValueNameAttribute para especificar explicitamente o nome do valor de retorno. |

**Observação**o nome padrão é "returnValue" para acessadores de propriedade e "value" para todos os outros métodos.


## <a name="related-topics"></a>Tópicos relacionados

* [Criando componentes do Tempo de Execução do Windows em C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Winmdexp.exe (Ferramenta de Exportação de Metadados do Tempo de Execução do Windows)](https://msdn.microsoft.com/library/hh925576.aspx)

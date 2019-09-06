---
title: Mapeamentos .NET de tipos de Windows Runtime
description: A tabela a seguir lista os mapeamentos que o .NET faz entre tipos de Plataforma Universal do Windows (UWP) e tipos .NET.
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c035db58fc6aa484f9d47a9af61176a2b05d55ee
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393654"
---
# <a name="net-mappings-of-windows-runtime-types"></a>Mapeamentos .NET de tipos de Windows Runtime

A tabela a seguir lista os mapeamentos que o .NET faz entre tipos de Plataforma Universal do Windows (UWP) e tipos .NET. Em um aplicativo universal do Windows escrito com código gerenciado, o Visual Studio IntelliSense mostra o tipo .NET em vez do tipo UWP. Por exemplo, se um método Windows runtime usa um parâmetro do tipo cadeia&lt;de&gt;caracteres IVector, o IntelliSense mostra um parâmetro do&lt;tipo&gt;cadeia de caracteres IList. Da mesma forma, em um componente Windows Runtime escrito com código gerenciado, você usa o tipo .NET em assinaturas de membro. Quando a [ferramenta de exportação de metadados Windows Runtime (Winmdexp. exe)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) gera seu componente Windows Runtime, o tipo .net é convertido no tipo UWP correspondente.

A maioria dos tipos que têm o mesmo nome de namespace e nome de tipo no UWP e no .NET são estruturas (ou tipos associados a estruturas, como enumerações). No UWP, as estruturas não têm Membros além dos campos e exigem tipos auxiliares, que o .NET oculta. As versões do .NET dessas estruturas têm propriedades e métodos que fornecem a funcionalidade dos tipos auxiliares ocultos.

## <a name="uwp-types-that-map-to-net-types-with-the-same-name-and-namespace"></a>Tipos UWP que são mapeados para tipos .NET com o mesmo nome e namespace

### <a name="in-net-assembly-systemobjectmodeldll"></a>Em .NET assembly System. ObjectModel. dll

| Namespace | Tipo |
|-|-|
| Windows.UI.Xaml.Input | ICommand |

### <a name="in-net-assembly-systemruntimewindowsruntimedll"></a>Em .NET assembly System. Runtime. WindowsRuntime. dll

| Namespace | Tipo |
|-|-|
| Windows.Foundation | Ponto |
| Windows.Foundation | Rect |
| Windows.Foundation | Size |
| Windows.UI | Cor |

### <a name="in-net-assembly-systemruntimewindowsruntimeuixamldll"></a>Em .NET assembly System. Runtime. WindowsRuntime. UI. XAML. dll

| Namespace | Tipo |
|-|-|
| Windows.UI.Xaml | CornerRadius |
| Windows.UI.Xaml | Duração |
| Windows.UI.Xaml | DurationType |
| Windows.UI.Xaml | GridLength |
| Windows.UI.Xaml | GridUnitType |
| Windows.UI.Xaml | Espessura |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition |
| Windows.UI.Xaml.Media | Matriz |
| Windows.UI.Xaml.Media.Animation | KeyTime |
| Windows.UI.Xaml.Media.Animation | RepeatBehavior |
| Windows.UI.Xaml.Media.Animation | RepeatBehaviorType |
| Windows.UI.Xaml.Media.Media3D | Matrix3D |

## <a name="uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace"></a>Tipos UWP que são mapeados para tipos .NET com um nome e/ou namespace diferente

### <a name="in-net-assembly-systemobjectmodeldll"></a>Em .NET assembly System. ObjectModel. dll

| Tipo/namespace da UWP | Tipo/namespace do .NET |
|-|-|
| INotifyCollectionChanged (Windows.UI.Xaml.Interop) | INotifyCollectionChanged (System.Collections.Specialized) | 
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized) | 
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventArgs (System.Collections.Specialized) | 
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop) | NotifyCollectionChangedAction (System.Collections.Specialized) | 
| INotifyPropertyChanged (Windows.UI.Xaml.Data) | INotifyPropertyChanged (System.ComponentModel) | 
| PropertyChangedEventHandler (Windows.UI.Xaml.Data) | PropertyChangedEventHandler (System.ComponentModel) | 
| PropertyChangedEventArgs (Windows.UI.Xaml.Data) | PropertyChangedEventArgs (System.ComponentModel) | 

### <a name="in-net-assembly-systemruntimedll"></a>Em .NET assembly System. Runtime. dll

| Tipo/namespace da UWP | Tipo/namespace do .NET |
|-|-|
| AttributeUsageAttribute (Windows.Foundation.Metadata) | AttributeUsageAttribute (System) |
| AttributeTargets (Windows.Foundation.Metadata) | AttributeTargets (System) |
| DateTime (Windows.Foundation) | DateTimeOffset (System) |
| EventHandler&lt;T&gt; (Windows.Foundation) | EventHandler&lt;T&gt; (System) |
| HResult (Windows.Foundation) | Exception (System) |
| IReference&lt;T&gt; (Windows.Foundation) | Nullable&lt;T&gt; (System) |
| TimeSpan (Windows.Foundation) | TimeSpan (System) |
| Uri (Windows.Foundation) | Uri (System) |
| IClosable (Windows.Foundation) | IDisposable (System) |
| IIterable&lt;T&gt; (Windows.Foundation.Collections) | IEnumerable&lt;T&gt; (System.Collections.Generic) |
| IVector&lt;T&gt; (Windows.Foundation.Collections) | IList&lt;T&gt; (System.Collections.Generic) |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections) | IReadOnlyList&lt;T&gt; (System.Collections.Generic) |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections) | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections) | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections) | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IBindableIterable (Windows.UI.Xaml.Interop) | IEnumerable (System.Collections) |
| IBindableVector (Windows.UI.Xaml.Interop) | IList (System.Collections) |
| TypeName (Windows.UI.Xaml.Interop) | Type (System) |

### <a name="in-net-assembly-systemruntimeinteropserviceswindowsruntimedll"></a>Em .NET assembly System. Runtime. InteropServices. WindowsRuntime. dll

| Tipo/namespace da UWP | Tipo/namespace do .NET |
|-|-|
| EventRegistrationToken (Windows.Foundation) | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) |

## <a name="related-topics"></a>Tópicos relacionados

* [Windows Runtime componentes com C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
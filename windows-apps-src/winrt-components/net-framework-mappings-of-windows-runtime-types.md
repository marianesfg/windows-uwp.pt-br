---
author: martinekuan
title: Mapeamentos do .NET Framework dos tipos do Windows Runtime
description: A tabela a seguir lista os mapeamentos que o .NET Framework faz entre os tipos da Plataforma Universal do Windows (UWP) e os tipos do .NET Framework.
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
---

# Mapeamentos do .NET Framework dos tipos do Windows Runtime


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não dá nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

A tabela a seguir lista os mapeamentos que o .NET Framework faz entre os tipos da Plataforma Universal do Windows (UWP) e os tipos do .NET Framework. Em um aplicativo Universal do Windows escrito com código gerenciado, o IntelliSense mostra o tipo do .NET Framework, em vez do tipo UWP. Por exemplo, caso um método do Windows Runtime utilize um parâmetro do tipo IVector&lt;string&gt;, o IntelliSense mostra um parâmetro do tipo IList&lt;string&gt;. Da mesma forma, em um componente do Tempo de Execução do Windows escrito com código gerenciado, você usa o tipo do .NET Framework em assinaturas de membro. Quando a [Ferramenta de Exportação de Metadados do Tempo de Execução do Windows (Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx) gera o componente do Tempo de Execução do Windows, o tipo do .NET Framework se torna o tipo UWP correspondente.

## Tabelas de mapeamento


A maioria dos tipos com o mesmo nome de namespace e o mesmo nome de tipo na UWP e no .NET Framework é de estruturas (ou tipos associados a estruturas, como enumerações). Na UWP, as estruturas não têm membros que não sejam campos e exigem tipos auxiliares, que o .NET Framework oculta. As versões do .NET Framework dessas estruturas têm propriedades e métodos que oferecem a funcionalidade dos tipos de elemento oculto.

Para obter mais informações sobre a maneira como o .NET Framework usa metadados do Windows para simplificar a programação com o Tempo de Execução do Windows, baixe o white paper [CLR e o Tempo de Execução do Windows](http://download.microsoft.com/download/2/3/E/23E1E9BE-41AA-4716-A7B3-82040271394C/CLR%20and%20the%20Windows%20Runtime.docx) no Centro de Desenvolvimento do Windows.

Tabela 1: Tipos UWP mapeados para tipos do .NET Framework com um nome diferente e/ou o namespace.

| Tipo/namespace da UWP                                            | Tipo/namespace do .NET Framework                                          | Assembly do .NET Framework                           |
|---------------------------------------------------------------|------------------------------------------------------------------------|---------------------------------------------------|
| AttributeUsageAttribute (Windows.Foundation.Metadata)         | AttributeUsageAttribute (System)                                       | System.Runtime.dll                                |
| AttributeTargets (Windows.Foundation.Metadata)                | AttributeTargets (System)                                              | System.Runtime.dll                                |
| DateTime (Windows.Foundation)                                 | DateTimeOffset (System)                                                | System.Runtime.dll                                |
| EventHandler&lt;T&gt; (Windows.Foundation)                    | EventHandler&lt;T&gt; (System)                                         | System.Runtime.dll                                |
| EventRegistrationToken (Windows.Foundation)                   | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) | System.Runtime.InteropServices.WindowsRuntime.dll |
| HResult (Windows.Foundation)                                  | Exception (System)                                                     | System.Runtime.dll                                |
| IReference&lt;T&gt; (Windows.Foundation)                      | Nullable&lt;T&gt; (System)                                             | System.Runtime.dll                                |
| TimeSpan (Windows.Foundation)                                 | TimeSpan (System)                                                      | System.Runtime.dll                                |
| Uri (Windows.Foundation)                                      | Uri (System)                                                           | System.Runtime.dll                                |
| IClosable (Windows.Foundation)                                | IDisposable (System)                                                   | System.Runtime.dll                                |
| IIterable&lt;T&gt; (Windows.Foundation.Collections)           | IEnumerable&lt;T&gt; (System.Collections.Generic)                      | System.Runtime.dll                                |
| IVector&lt;T&gt; (Windows.Foundation.Collections)             | IList&lt;T&gt; (System.Collections.Generic)                            | System.Runtime.dll                                |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections)         | IReadOnlyList&lt;T&gt; (System.Collections.Generic)                    | System.Runtime.dll                                |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections)              | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic)            | System.Runtime.dll                                |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections)          | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic)    | System.Runtime.dll                                |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections)     | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic)           | System.Runtime.dll                                |
| IBindableIterable (Windows.UI.Xaml.Interop)                   | IEnumerable (System.Collections)                                       | System.Runtime.dll                                |
| IBindableVector (Windows.UI.Xaml.Interop)                     | IList (System.Collections)                                             | System.Runtime.dll                                |
| INotifyCollectionChanged (Windows.UI.Xaml.Interop)            | INotifyCollectionChanged (System.Collections.Specialized)              | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized)   | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop)    | NotifyCollectionChangedEventArgs (System.Collections.Specialized)      | System.ObjectModel.dll                            |
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop)       | NotifyCollectionChangedAction (System.Collections.Specialized)         | System.ObjectModel.dll                            |
| INotifyPropertyChanged (Windows.UI.Xaml.Data)                 | INotifyPropertyChanged (System.ComponentModel)                         | System.ObjectModel.dll                            |
| PropertyChangedEventHandler (Windows.UI.Xaml.Data)            | PropertyChangedEventHandler (System.ComponentModel)                    | System.ObjectModel.dll                            |
| PropertyChangedEventArgs (Windows.UI.Xaml.Data)               | PropertyChangedEventArgs (System.ComponentModel)                       | System.ObjectModel.dll                            |
| TypeName (Windows.UI.Xaml.Interop)                            | Type (System)                                                          | System.Runtime.dll                                |

 

Tabela 2: Tipos UWP mapeados para tipos do .NET Framework com o mesmo nome e namespace.

| Namespace                           | Tipo               | Assembly do .NET Framework                   |
|-------------------------------------|--------------------|-------------------------------------------|
| Windows.UI                          | Color              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Point              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Rect               | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Size               | System.Runtime.WindowsRuntime.dll         |
| Windows.UI.Xaml.Input               | ICommand           | System.ObjectModel.dll                    |
| Windows.UI.Xaml                     | CornerRadius       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Duration           | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | DurationType       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridLength         | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridUnitType       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Thickness          | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition  | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media               | Matrix             | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | KeyTime            | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehavior     | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehaviorType | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Media3D       | Matrix3D           | System.Runtime.WindowsRuntime.UI.Xaml.dll |

 

## Tópicos relacionados

* [Criando componentes do Tempo de Execução do Windows em C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)



<!--HONumber=May16_HO2-->



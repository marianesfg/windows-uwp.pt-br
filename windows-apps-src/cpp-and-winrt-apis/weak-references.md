---
author: stevewhims
description: O suporte a referência fraca do C++/WinRT é pago pelo uso, o que significa que você só paga quando o objeto é consultado em busca de IWeakReferenceSource.
title: Referências fracas em C++/WinRT
ms.author: stwhi
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, fraca, referência
ms.localizationpriority: medium
ms.openlocfilehash: 69294115af93ec464abfe908df948c8ff5504efc
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842450"
---
# <a name="weak-references-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Referências fracas em [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Você deve ser capaz, na maioria das vezes, de criar suas próprias APIs C++/WinRT de modo a evitar a necessidade de referências cíclicas e referências fracas. No entanto, quando se trata da implementação nativa da estrutura da IU baseada em XAML, devido ao design histórico da estrutura, o mecanismo de referência fraca em C++/WinRT é necessário para tratar as referências cíclicas. Fora do XAML, você dificilmente precisará usar referências fracas (embora, não haja nada específico de XAML sobre eles na teoria).

Em qualquer tipo declarado, não fica imediatamente óbvio para o C++/WinRT se ou quando são necessárias referências fracas. Portanto, o C++/WinRT oferece suporte a referência fraca automaticamente no modelo de struct [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), do qual são derivados direta ou indiretamente seus próprios tipos C++/WinRT. Ele é pago pelo uso, o que significa que você só pagará se o objeto for consultado em busca de [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609). Você pode optar explicitamente por [recusar esse suporte](#opting-out-of-weak-reference-support).

## <a name="code-examples"></a>Exemplos de código
O modelo de struct [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) é uma opção para obter uma referência fraca em uma instância de classe.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```
Ou você pode usar a função auxiliar [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak).

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

A criação de uma referência fraca não afeta a contagem de referências no próprio objeto; ela apenas faz com que um bloco de controle seja alocado. Esse bloco de controle se encarrega de implementar a semântica da referência fraca. Em seguida, tente promover a referência fraca como uma referência forte e, se você conseguir fazer isso com êxito, use-a.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

Desde que ainda existe alguma outra referência forte, a chamada de [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) incrementará a contagem de referências e retornará a referência forte ao chamador.

## <a name="a-weak-reference-to-the-this-pointer"></a>Uma referência fraca a *este* ponteiro
Um objeto C++/WinRT é derivado direta ou indiretamente do modelo de struct [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). A função membro protegida [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) retorna uma referência fraca a *este* ponteiro de um objeto C++/WinRT. [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) obtém uma referência forte.

## <a name="opting-out-of-weak-reference-support"></a>Recusando o suporte a referência fraca
Os suporte a referência fraca é automático. Mas você pode optar explicitamente por recusar esse suporte passando o struct de marcador [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) como um argumento de modelo para a classe base.

Se você derivar diretamente de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Se você estiver criando uma classe de tempo de execução.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

Não importa onde o struct de marcador apareça no pacote de parâmetros variadic. Se você solicitar uma referência fraca para um tipo recusado, o compilador ajudará você com "*This is only for weak ref support*".

## <a name="important-apis"></a>APIs importantes
* [Função implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [Modelo de função winrt::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [Struct de marcador winrt::no_weak_ref](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [Modelo de struct winrt::weak_ref](/uwp/cpp-ref-for-winrt/weak-ref)

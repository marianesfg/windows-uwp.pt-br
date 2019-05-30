---
description: Um objeto ágil é aquele que pode ser acessado de qualquer conversa. Seus tipos C++/WinRT são ágeis por padrão, mas você pode recusá-los.
title: Objetos ágeis com C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, ágil, objeto, agilidade, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 82dff619e6fa3934f69b93090bee90de6359ca07
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360327"
---
# <a name="agile-objects-in-cwinrt"></a>Objetos Agile em C++/WinRT

Na grande maioria dos casos, uma instância de uma classe de tempo de execução do Windows pode ser acessada de qualquer thread (assim como podem fazer objetos C++ padrão mais). Essa classe de tempo de execução do Windows é *agile*. Somente um pequeno número de classes de tempo de execução do Windows que acompanham o Windows é não agile, mas quando você consumi-los, você precisa levar em consideração seu modelo de threading e comportamento de marshaling (marshaling é passar dados em um limite de apartment). Ele é um bom padrão para cada objeto de tempo de execução do Windows ágeis, portanto, seus próprios [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) tipos são agile por padrão.

Mas você pode recusá-lo. Você pode ter um motivo convincente para exigir que um objeto do tipo residir, por exemplo, em um determinado single-threaded apartment. Normalmente, isso envolve os requisitos de reentrância. Entretanto, cada vez mais, até mesmo APIs da interface do usuário oferecem objetos ágeis. Em geral, a agilidade é a opção mais simples e eficiente. Além disso, quando você implementa uma fábrica de ativação, ela deve ser ágil mesmo que a sua classe de tempo de execução correspondente não seja.

> [!NOTE]
> O Windows Runtime se baseia em COM. Em termos de COM, uma classe ágil é registrada com `ThreadingModel` = *Ambos*. Para obter mais informações sobre modelos e apartments de threading COM, consulte [Compreendendo e usando os modelos de Threading](/previous-versions/ms809971(v=msdn.10)).

## <a name="code-examples"></a>Exemplos de código

Vamos usar um exemplo de implementação de uma classe de tempo de execução para ilustrar como C++/WinRT dá suporte a agilidade.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Como não recusamos a opção, essa implementação é ágil. A estrutura de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) implementa [**IAgileObject**](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject) e [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal). A implementação **IMarshal** usa **CoCreateFreeThreadedMarshaler** para fazer a coisa certa com relação ao código herdado que não sabe sobre **IAgileObject**.

Esse código verifica um objeto em busca de agilidade. A chamada para [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) gera uma exceção se `myimpl` não for ágil.

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

Em vez de manipular uma exceção, você pode chamar [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function).

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** não possui métodos próprios, por isso você não pode fazer muito com ele. A próxima variante, então, é mais comum.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** é uma *interface de marcador*. O mero êxito ou falha da consulta de **IAgileObject** é a extensão da informação e utilidade que você obtém dela.

## <a name="opting-out-of-agile-object-support"></a>Recusando o suporte ao objeto ágil

Você pode escolher recusar explicitamente o suporte ao objeto ágil, passando o marcador de estrutura [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) como argumento de modelo para a sua classe base.

Se você derivar diretamente de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

Se você estiver criando uma classe de tempo de execução.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

Não importa onde o struct de marcador apareça no pacote de parâmetros variadic.

Se você recusar a agilidade, você pode implementar **IMarshal** por conta própria. Por exemplo, você pode usar o **winrt::non_agile** marcador para evitar a implementação padrão de agilidade e implementar **IMarshal** por conta própria&mdash;talvez para dar suporte à semântica de marshaling por valor.

## <a name="agile-references-winrtagileref"></a>Referências ágeis (winrt::agile_ref)

Se você estiver usando um objeto que não é ágil, mas precisa passá-lo em algum contexto potencialmente ágil, uma opção é usar o modelo de estrutura [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) a fim de obter uma referência ágil para uma instância de tipo não ágil, ou para a interface de um objeto não ágil.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

Ou você pode usar a função auxiliar do [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile).

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

Em ambos os casos, agora `agile` pode ser livremente passado a um thread em um apartment diferente e lá usado.

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

A chamada [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) retorna um proxy que pode ser utilizado com segurança dentro do contexto de thread no qual **get** é chamado.

## <a name="important-apis"></a>APIs Importantes

* [Interface IAgileObject](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject)
* [Interface IMarshal](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [winrt::agile_ref struct template](/uwp/cpp-ref-for-winrt/agile-ref)
* [WinRT::Implements struct modelo](/uwp/cpp-ref-for-winrt/implements)
* [modelo de função WinRT::make_agile](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile marker struct](/uwp/cpp-ref-for-winrt/non-agile)
* [WinRT::Windows::Foundation::IUnknown:: como função](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [função WinRT::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>Tópicos relacionados

* [Compreendendo e usando modelos de Threading COM](/previous-versions/ms809971(v=msdn.10))

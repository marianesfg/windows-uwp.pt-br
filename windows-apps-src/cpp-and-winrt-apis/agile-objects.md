---
description: Um objeto ágil é aquele que pode ser acessado de qualquer thread. Seus tipos C++/WinRT são ágeis por padrão, mas você pode recusá-los.
title: Objetos ágeis com C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, ágil, objeto, agilidade, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 82dff619e6fa3934f69b93090bee90de6359ca07
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360327"
---
# <a name="agile-objects-in-cwinrt"></a>Objetos ágeis em C++/WinRT

Na maior parte dos casos, uma instância da classe do Windows Runtime pode ser acessada de qualquer thread (da mesma forma que a maioria dos objetos C++ podem). Essa classe do Windows Runtime é *ágil*. Apenas poucas classes do Windows Runtime que acompanham o Windows não são ágeis, mas ao consumi-las, é necessário levar em consideração o modelo de threading e o comportamento de marshaling (transmissão de dados por um limite de apartment). É um padrão conveniente que todos os objetos do Windows Runtime sejam ágeis, portanto, os seus próprios tipos do [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) serão ágeis por padrão.

Mas você poderá recusar esse padrão. É possível que você tenha um motivo convincente para exigir que um objeto do seu tipo resida, por exemplo, em um determinado single-threaded apartment. Normalmente, isso envolve os requisitos de reentrância. Entretanto, cada vez mais, até mesmo as APIs da interface do usuário oferecem objetos ágeis. Em geral, a agilidade é a opção mais simples e eficiente. Além disso, quando você implementa um alocador de ativação, ele deve ser ágil mesmo que a classe de tempo de execução correspondente não seja.

> [!NOTE]
> O Windows Runtime se baseia em COM. Em termos COM, uma classe ágil é registrada com `ThreadingModel` = *Ambos*. Para saber mais sobre os modelos de threading COM e apartments, confira as [noções básicas e uso de modelos de threading COM](/previous-versions/ms809971(v=msdn.10)).

## <a name="code-examples"></a>Exemplos de código

Vamos usar um exemplo de implementação de uma classe de tempo de execução para ilustrar como C++/WinRT dá suporte à agilidade.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Como não recusamos a opção, essa implementação é ágil. O struct base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) implementa [**IAgileObject**](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject) e [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal). A implementação **IMarshal** usa **CoCreateFreeThreadedMarshaler** para fazer a coisa certa com relação ao código herdado que não sabe sobre **IAgileObject**.

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

O **IAgileObject** não possui métodos próprios, por isso você não pode fazer muito com ele. A próxima variante, então, é mais comum.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

O **IAgileObject** é uma *interface de marcador*. O mero êxito ou falha da consulta por **IAgileObject** é a extensão da informação e utilidade que você obtém dela.

## <a name="opting-out-of-agile-object-support"></a>Recusando o suporte ao objeto ágil

Você pode optar por recusar explicitamente o suporte ao objeto ágil, passando o struct de marcador [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) como um argumento de modelo para a sua classe base.

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

Recusando agilidade ou não, você poderá implementar **IMarshal** por conta própria. Por exemplo, você pode usar o marcador **winrt::non_agile** para evitar a implementação de agilidade padrão e implementar **IMarshal** por conta própria&mdash; para, talvez, oferecer suporte à semântica de marshal-por-valor.

## <a name="agile-references-winrtagileref"></a>Referências ágeis (winrt::agile_ref)

Se você estiver consumindo um objeto que não seja ágil, mas precisa passá-lo em algum contexto potencialmente ágil, uma opção será usar o modelo de struct [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) a fim de obter uma referência ágil para uma instância de tipo não ágil, ou para a interface de um objeto não ágil.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

Ou você poderá usar a função auxiliar do [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile).

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

Em ambos os casos, agora `agile` pode ser livremente passado a um thread em um apartment diferente e ser usado lá.

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

A chamada [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) retorna um proxy que pode ser utilizado com segurança dentro do contexto de thread no qual **get** é chamado.

## <a name="important-apis"></a>APIs Importantes

* [Interface IAgileObject](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject)
* [Interface IMarshal](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [Modelo de struct winrt::agile_ref](/uwp/cpp-ref-for-winrt/agile-ref)
* [Modelo de struct winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Modelo de função winrt::make_agile](/uwp/cpp-ref-for-winrt/make-agile)
* [Struct de marcador winrt::non_agile](/uwp/cpp-ref-for-winrt/non-agile)
* [Função winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Função winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>Tópicos relacionados

* [Noções básicas e uso de modelos de threading COM](/previous-versions/ms809971(v=msdn.10))

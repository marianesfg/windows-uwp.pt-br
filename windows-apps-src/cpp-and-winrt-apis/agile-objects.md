---
author: stevewhims
description: Um objeto ágil é aquele que pode ser acessado de qualquer thread. Seus tipos C++/WinRT são ágeis por padrão, mas você pode recusá-los.
title: Objetos ágeis com C++/WinRT
ms.author: stwhi
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, ágil, objeto, agilidade, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 9af1fb0a9d23727924ae3c165bc8977fb9cc7774
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4504927"
---
# <a name="agile-objects-in-cwinrt"></a>Objetos ágeis em C++/WinRT
Na grande maioria dos casos, a instância de uma classe do Windows Runtime&mdash; como objeto C++&mdash; padrão pode ser acessado de qualquer thread. Essa classe é *ágil*. Apenas um pequeno número de classes do Windows Runtime que acompanha o Windows não é ágil, mas ao usá-las, é necessário levar em consideração o modelo de threading e o comportamento de empacotamento (empacotamento é transmitir dados por um marco de delimitação de thread ou processo). Ele é um bom padrão para cada objeto de tempo de execução do Windows ser ágil, portanto, sua própria [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) tipos são ágeis por padrão.

No entanto, é possível recusá-los. Você pode ter um motivo convincente para exigir que um objeto do seu tipo resida, por exemplo, em um determinado single-threaded apartment. Normalmente, isso envolve os requisitos de reentrância. Entretanto, cada vez mais, até mesmo APIs da interface do usuário oferecem objetos ágeis. Em geral, a agilidade é a opção mais simples e eficiente. Além disso, quando você implementa uma fábrica de ativação, ela deve ser ágil mesmo que a sua classe de tempo de execução correspondente não seja.

> [!NOTE]
> O Windows Runtime se baseia em COM. Em termos de COM, uma classe ágil é registrada com `ThreadingModel` = *Ambos*. Para saber mais sobre modelos de threading COM, consulte [Noções básicas e uso de modelos de threading COM](https://msdn.microsoft.com/library/ms809971).

## <a name="code-examples"></a>Exemplos de código
Vamos usar um exemplo de implementação para ilustrar como C++/WinRT dá suporte à agilidade.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Como não recusamos a opção, essa implementação é ágil. A estrutura de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) implementa [**IAgileObject**](https://msdn.microsoft.com/library/windows/desktop/hh802476) e [**IMarshal**](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993). A implementação **IMarshal** usa **CoCreateFreeThreadedMarshaler** para fazer a coisa certa com relação ao código herdado que não sabe sobre **IAgileObject**.

Esse código verifica um objeto em busca de agilidade. A chamada para [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) gera uma exceção se `myimpl` não for ágil.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
winrt::com_ptr<IAgileObject> iagileobject = myimpl.as<IAgileObject>();
```

Em vez de manipular uma exceção, você pode chamar [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function).

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject = myimpl.try_as<IAgileObject>();
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** não possui métodos próprios, por isso você não pode fazer muito com ele. A próxima variante, então, é mais comum.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** é uma *interface de marcador*. O mero êxito ou falha da consulta de **IAgileObject** é a extensão da informação e utilidade que você obtém dela.

## <a name="opting-out-of-agile-object-support"></a>Recusando o suporte ao objeto ágil
Você pode escolher recusar explicitamente o suporte ao objeto ágil, passando o marcador de estrutura [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non_agile) como argumento de modelo para a sua classe base.

Se você derivar diretamente de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, non_agile>
{
    ...
}
```

Se você estiver criando uma classe de tempo de execução.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, non_agile>
{
    ...
}
```

Não importa onde a estrutura do marcador aparece no pacote de parâmetros variadic.

Recusando agilidade ou não, você pode implementar IMarshal por conta própria. Por exemplo, você pode usar o marcador [**non_agile**] para evitar a implementação de agilidade padrão e implementar IMarshal por conta própria&mdash; para, talvez, oferecer suporte à semântica de marshal-por-valor.

## <a name="agile-references-winrtagileref"></a>Referências ágeis (winrt::agile_ref)
Se você estiver usando um objeto que não é ágil, mas precisa passá-lo em algum contexto potencialmente ágil, uma opção é usar o modelo de estrutura [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) a fim de obter uma referência ágil para uma instância de tipo não ágil, ou para a interface de um objeto não ágil.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```
Ou você pode usar a função auxiliar do [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile).

```cppwinrt
NonAgileType nonagile_obj;
auto agile = winrt::make_agile(nonagile_obj);
```

Em ambos os casos, agora `agile` pode ser livremente passado a um thread em um apartment diferente e lá usado.

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again = agile.get();
winrt::hstring message = nonagile_obj_again.Message();
```

A chamada [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agilerefget-function) retorna um proxy que pode ser utilizado com segurança dentro do contexto de thread no qual **get** é chamado.

## <a name="important-apis"></a>APIs Importantes
* [Interface IAgileObject](https://msdn.microsoft.com/library/windows/desktop/hh802476)
* [Interface IMarshal](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993)
* [Modelo de estrutura winrt::agile_ref](/uwp/cpp-ref-for-winrt/agile-ref)
* [Modelo de estrutura winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Modelo de função winrt::make_agile](/uwp/cpp-ref-for-winrt/make-agile)
* [Marcador de estrutura winrt::non_agile](/uwp/cpp-ref-for-winrt/non_agile)
* [Função winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Função winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)

## <a name="related-topics"></a>Tópicos relacionados
* [Noções básicas e uso de modelos de threading COM](https://msdn.microsoft.com/library/ms809971)

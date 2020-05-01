---
description: Este tópico apresenta detalhes sobre um recurso do C++s/WinRT 2.0 que ajuda você a diagnosticar o erro de criação de um objeto do tipo de implementação na pilha, em vez de usar a família [**winrt::make**](/uwp/cpp-ref-for-winrt/make) de auxiliares, como deveria ser.
title: Diagnosticando alocações diretas
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projeção, direta, pilha, alocações, projetado, implementação
ms.localizationpriority: medium
ms.openlocfilehash: 7fe8ff6653b8655ee25cd9adc0c11acb22d42a11
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68372787"
---
# <a name="diagnosing-direct-allocations"></a>Diagnosticando alocações diretas

Conforme explicado em [Criar APIs com o C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis), ao criar um objeto de tipo de implementação, você deve usar a família de auxiliares [**winrt::make**](/uwp/cpp-ref-for-winrt/make) para fazer isso. Este tópico apresenta detalhes sobre um recurso do C++/WinRT 2.0 que ajuda você a diagnosticar o erro de alocar diretamente um objeto de tipo de implementação na pilha.

Esses erros podem se tornar falhas ou corrupções misteriosas que são difíceis e demorados para serem depurados. Portanto, esse é um recurso importante e vale a pena entender o contexto.

## <a name="setting-the-scene-with-mystringable"></a>Definir a cena, com **MyStringable**

Primeiro, vamos considerar uma implementação simples de [**IStringable**](/uwp/api/windows.foundation.istringable).

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const { return L"MyStringable"; }
};
```

Agora imagine que você precisa chamar uma função (de dentro de sua implementação) que espera um **IStringable** como um argumento.

```cppwinrt
void Print(IStringable const& stringable)
{
    printf("%ls\n", stringable.ToString().c_str());
}
```

O problema é que nosso tipo **MyStringable***não* é um **IStringable**.

- Nosso tipo **MyStringable** é uma implementação da interface **IStringable**.
- O tipo **IStringable** é um tipo projetado.

> [!IMPORTANT]
> É importante entender a distinção entre um *tipo de implementação* e um *tipo projetado*. Para ver termos e conceitos essenciais, leia [Consumir APIs com o C++/WinRT](consume-apis.md) e [Criar APIs com o C++/WinRT](author-apis.md).

Entender o espaço entre uma implementação e a projeção pode ser sutil. Na verdade, para tentar fazer a implementação parecer um pouco mais com a projeção, a implementação oferece conversões implícitas a cada um dos tipos projetados que ela implementa. Isso não significa que podemos simplesmente fazer isso.

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const;
 
    void Call()
    {
        Print(this);
    }
};
```

Em vez disso, é necessário obter uma referência para que os operadores de conversão possam ser usados como candidatos para resolver a chamada.

```cppwinrt
void Call()
{
    Print(*this);
}
```

Isso funciona. Uma conversão implícita oferece uma conversão (muito eficiente) do tipo de implementação no tipo projetado, e isso é bastante conveniente para muitos cenários. Sem esse recurso, muitos tipos de implementação provariam ser muito difíceis de serem criados. Desde que você só use o modelo de função [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (ou [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self)) para alocar a implementação, então tudo bem.

```cppwinrt
IStringable stringable{ winrt::make<MyStringable>() };
```

## <a name="potential-pitfalls-with-cwinrt-10"></a>Possíveis armadilhas com o C++/WinRT 1.0

Ainda assim, as conversões implícitas podem lhe trazer problemas. Considere essa função auxiliar inútil.

```cppwinrt
IStringable MakeStringable()
{
    return MyStringable(); // Incorrect.
}
```

Ou até mesmo essa instrução aparentemente inofensiva.

```cppwinrt
IStringable stringable{ MyStringable() }; // Also incorrect.
```

Um código como esse *foi compilado* com o C++/WinRT 1.0 por causa dessa conversão implícita. O problema (muito sério) é que estamos potencialmente retornando um tipo projetado que aponta para um objeto de referência contada cuja memória de backup está na pilha efêmera.

Veja outra coisa que foi compilada com o C++/WinRT 1.0.

```cppwinrt
MyStringable* stringable{ new MyStringable() }; // Very inadvisable.
```

Ponteiros brutos são uma fonte perigosa e trabalhosa de bugs. Não os use se você não precisar. O C++O/WinRT esforça-se para tornar tudo eficiente sem nunca forçá-lo a usar ponteiros brutos. Veja outra coisa que foi compilada com o C++/WinRT 1.0.

```cppwinrt
auto stringable{ std::make_shared<MyStringable>(); } // Also very inadvisable.
```

Isso é um erro em vários níveis. Temos duas contagens de referência diferentes para o mesmo objeto. O Windows Runtime (e o COM clássico antes dele) baseia-se em uma contagem de referência intrínseca que não é compatível com **std::shared_ptr**. **std::shared_ptr** tem, claro, muitas aplicações válidas; mas é totalmente desnecessário quando você está compartilhando objetos do Windows Runtime (e o COM clássico). Por fim, isso também é compilado com o C++/WinRT 1.0.

```cppwinrt
auto stringable{ std::make_unique<MyStringable>() }; // Highly dubious.
```

Novamente, isso é questionável. A propriedade exclusiva é oposta ao tempo de vida compartilhado da contagem de referência intrínseca de **MyStringable**.

## <a name="the-solution-with-cwinrt-20"></a>A solução com C++/WinRT 2.0

Com o C++/WinRT 2.0, todas essas tentativas de alocar tipos de implementação diretamente levam a um erro do compilador. Esse é o melhor tipo de erro e infinitamente melhor do que um bug de runtime misterioso.

Sempre que precisar realizar uma implementação, você poderá simplesmente usar [**winrt::make**](/uwp/cpp-ref-for-winrt/make) ou [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self), conforme mostrado acima. Agora, se você se esquecer de fazer isso, receberá um erro do compilador que fará alusão a isso com uma referência a uma função abstrata denominada **use_make_function_to_create_this_object**. Não é exatamente um `static_assert`, mas é quase isso. Ainda assim, essa é a maneira mais confiável de detectar todos os erros descritos.

Isso significa que precisamos impor algumas restrições secundárias sobre a implementação. Considerando que dependemos da ausência de uma substituição para detectar a alocação direta, o modelo de função **winrt::make** deve, de alguma forma, atender à função virtual abstrata com uma substituição. Ele faz isso por meio da derivação da implementação com uma classe `final` que fornece a substituição. Há algumas coisas que devem ser observadas sobre esse processo.

Primeiro, a função virtual só está presente em builds de depuração. Isso significa que a detecção não afetará o tamanho do vtable em seus builds otimizados.

Segundo, como a classe derivada que o **winrt::make** usa é `final`, isso significa que qualquer desvirtualização que o otimizador puder possivelmente deduzir acontecerá mesmo que você tenha optado anteriormente por não marcar sua classe de implementação como `final`. Portanto, é uma melhoria. O contrário é que sua implementação *não pode* ser `final`. Novamente, isso não importa, porque o tipo instanciado sempre será `final`.

Terceiro, nada impede que você marque funções virtuais em sua implementação como `final`. É claro que o C++/WinRT é muito diferente do COM clássico e das implementações como WRL, em que tudo sobre sua implementação tende a ser virtual. No C++/WinRT, o envio virtual está limitado à ABI (interface binária do aplicativo) (que é sempre `final`), e seus métodos de implementação dependem do tempo de compilação ou do polimorfismo estático. Isso evita o polimorfismo de runtime desnecessário e também significa que há uma pequena razão para as funções virtuais em sua implementação do C++/WinRT. O que é uma coisa boa e leva a um inlining bem mais previsível.

Quarto, como **winrt::make** injeta uma classe derivada, sua implementação não pode ter um destruidor privado. Os destruidores privados eram populares com implementações COM clássicas, porque, novamente, tudo era virtual e era comum lidar diretamente com ponteiros brutos e, portanto, era fácil chamar acidentalmente `delete` em vez de [**Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release). O C++/WinRT esforça-se para que seja difícil você lidar diretamente com ponteiros brutos. E você precisa *realmente* se esforçar para obter um ponteiro bruto em C++/WinRT no qual você possa eventualmente chamar `delete`. Semântica de valor significa que você está lidando com valores e referências; raramente com ponteiros.

Portanto, o C++/WinRT desafia nossas noções preconcebidas do que significa escrever código COM clássico. E isso é perfeitamente razoável, porque o WinRT não é o COM clássico. O COM clássico é a linguagem assembly do Windows Runtime. Ele não deve ser o código que você escreve todos os dias. Em vez disso, o C++/WinRT o leva a escrever um código mais parecido com o C++ moderno e menos parecido com o COM clássico.

## <a name="important-apis"></a>APIs importantes
* [modelo da função winrt::make](/uwp/cpp-ref-for-winrt/make)
* [modelo da função winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>Tópicos relacionados
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Criar APIs com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
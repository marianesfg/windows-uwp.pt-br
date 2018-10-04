---
author: stevewhims
description: Este tópico descreve as várias categorias de valores que existem em C++. Você já deve ter ouvido de lvalues e rvalues, mas há outros tipos, também.
title: Categorias de valor e referências a eles
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, padrão, c++, cpp, winrt, projeção, mover, encaminhamento, categorias de valor, semântica de movimento, encaminhamento perfeito, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.openlocfilehash: cbccaf78b45d85d93619977d149431c4eec9e10a
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4353094"
---
# <a name="value-categories-and-references-to-them"></a>Categorias de valor e referências a eles
Este tópico descreve as várias categorias de valores (e referências aos valores) que existem em C++. Você já deve ter ouvido de *lvalues* e *rvalues*, mas você pode não pensar em termos deste tópico apresenta. E há outros tipos de valores, também.

Cada expressão no C++ produz um valor que pertence a uma das categorias discutidas neste tópico. Há aspectos da linguagem C++, facilies e regras, que exigem uma compreensão adequada dessas categorias de valor e referências a elas. Por exemplo, pegar o endereço de um valor, copiar um valor, movendo um valor e um valor para outra função de encaminhamento. Este tópico não entra em todos os aspectos em detalhes, mas ela fornece informações fundamentais para ter uma compreensão sólida deles.

As informações neste tópico é quadro em termos de análise de Stroustrup das categorias de valor pelas duas propriedades independentes de identidade e movability [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Lvalue tem identidade
O que significa um valor ter *identidade*? Se você tiver (ou você pode tirar) o endereço de memória de um valor e usá-lo com segurança, em seguida, o valor tem identidade. Dessa forma, você pode fazer mais do que comparar o conteúdo dos valores: você pode comparar ou diferenciá-las por identidade.

Um *lvalue* tem identidade. Agora é uma questão de interesse apenas histórico que o "l" no "lvalue" é a abreviação de "esquerda" (como à esquerda-à-lado de uma atribuição). Em C++, lvalue pode ser exibidos nas esquerda *ou* à direita de uma atribuição. O "l" no "lvalues", em seguida, não realmente ajuda você a compreender nem definir quais eles são. Você só precisa compreender o que o que chamamos de lvalue é um valor que tem identidade.

Exemplos de expressões que são lvalues: uma variável nomeada ou constante; ou uma função que retorna uma referência. Exemplos de expressões que são *não* lvalues: um temporário; ou uma função que retorna um valor.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    std::vector<byte> vec{ 99, 98, 97 };
    std::vector<byte>* addr1{ &vec }; // ok: vec is an lvalue.
    int* addr2{ &get_by_ref() }; // ok: get_by_ref() is an lvalue.

    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is not an lvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is not an lvalue.
}
```

Agora, embora seja uma instrução true lvalues têm identidade, portanto, fazer xvalues. Vamos mais em que um *xvalue* é posteriormente neste tópico. Por enquanto, apenas Lembre-se que há uma categoria de valor chamada glvalue, "generalizada lvalue". O superconjunto de glvalues contém lvalues (também conhecido como *lvalues clássico*) e xvalues. Portanto, enquanto "lvalue tem identidade" for true, o conjunto completo de coisas que têm identidade é o conjunto de glvalues, conforme mostrado nesta ilustração.

![Lvalue tem identidade](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Um rvalue é móvel; lvalue não é
Mas há valores que não são glvalues. Consequentemente, há valores que você *não pode* obter um endereço de memória para (ou você não pode confiar nele válido). Vimos alguns esses valores no exemplo de código acima. Isso parece uma desvantagem. Mas, na verdade a vantagem de um valor que seja como que você pode *Mover* dele (que é geralmente barato), em vez de cópia dele (que é geralmente cara). Migrar de um valor significa que ele não está mais no lugar de ser. Portanto, tentar acessá-lo no lugar de ser é algo a ser evitado. Uma discussão sobre quando e *como* mover que um valor está fora do escopo deste tópico. Neste tópico, precisamos apenas saber que um valor que é móvel é conhecido como um *rvalue* (ou *rvalue clássico*).

O "r" em "rvalue" é a abreviação de "direita" (como em, a direita-à lado de uma atribuição). Mas você pode usar rvalues e referências a rvalues fora atribuições. O "r" em "rvalues", em seguida, não é a coisa concentrar. Você só precisa compreender o que o que chamamos de um rvalue é um valor que é móvel.

Lvalue, por outro lado, não móvel, conforme mostrado nesta ilustração. Lvalue movido seria enfrente a definição de *lvalue*e seria um problema para o código que muito razoavelmente deve ser capaz de continuar a acessar o lvalue inesperado.

![Um rvalue é móvel; lvalue não é](images/is-movable.png)

Você não pode mover lvalue. Mas há *é* um tipo de glvalue (o conjunto de coisas com identidade) que você pode mover&mdash;se você souber que você está fazendo (incluindo tenha cuidado para não acessá-lo após a mudança)&mdash;que é o xvalue. Nós vai revisitar essa ideia mais uma vez abaixo, quando analisaremos o quadro completo das categorias de valor.

## <a name="rvalue-references-and-reference-binding-rules"></a>Referências de Rvalue e as regras de associação de referência
Esta seção apresenta a sintaxe para uma referência a um rvalue. Teremos esperar para outro tópico entrar em um tratamento considerável de mover e encaminhamento, mas esses são os problemas que são resolvidos por referências rvalue. Antes de examinarmos rvalue referências, porém, primeiro precisamos ser mais claras sobre `T&` &mdash;a coisa que estamos anteriormente conversa apenas "uma referência". Ele é realmente "uma lvalue (não-const) referência", que se refere a um valor para o qual o usuário da referência pode escrever.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Uma referência de lvalue pode associar lvalue, mas não um rvalue.

Em seguida, há referências const lvalue (`T const&`), que se referem a objetos ao qual o usuário a referência *não pode* gravar (por exemplo, uma constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Uma referência const lvalue pode associar lvalue ou um rvalue.

A sintaxe para uma referência a um rvalue do tipo `T` é escrita como `T&&`. Uma referência rvalue refere-se a um valor móvel&mdash;um valor cujo conteúdo não precisamos preservar depois usamos-lo (por exemplo, um temporário). Desde o ponto principal está mover de (assim modificação) o valor associado a uma referência de rvalue, `const` e `volatile` qualificadores (também conhecido como VC-qualificadores) não se aplicam ao rvalue referências.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Uma referência rvalue se associa a um rvalue. Na verdade, em termos de resolução de sobrecarga, um rvalue *prefere* seja vinculado a uma referência de rvalue que uma referência const lvalue. Mas uma referência de rvalue não é possível associar a lvalue porque, como estamos disse, uma referência rvalue se refere a um valor cujo conteúdo será considerado não precisamos preservar (digamos, o parâmetro de um construtor de movimento).

Você também pode passar um rvalue onde um argumento por valor é esperado, por meio de construção de cópia (ou por meio de construção de movimento, se o rvalue é um xvalue).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Um glvalue tem identidade; um prvalue não
Nesse estágio, sabemos que tem identidade. E sabemos que é móvel ou não é. Mas ainda não tenha sido chamado o conjunto de valores que *não* têm identidade. Esse conjunto é conhecido como o *prvalue*ou *rvalue pura*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Lvalue tem identidade; um prvalue não](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>A imagem completa de categorias de valor
Ele permanece apenas combinar as informações e ilustrações acima em uma única imagem grande.

![A imagem completa de categorias de valor](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Um glvalue (generalizado lvalue) tem identidade.

### <a name="lvalue-im"></a>lvalue (\ i \ & \!m)
Lvalue (um tipo de glvalue) tem identidade, mas não móvel. Estes são os valores de leitura / gravação normalmente que você passa ao redor por referência ou const referência ou por valor se copiar é barata. Lvalue não pode ser vinculado a uma referência de rvalue.

### <a name="xvalue-im"></a>xValue (\ i \ & m)
Um xvalue (um tipo de glvalue, mas também um tipo de rvalue) tem identidade e também é móvel. Isso pode ser lvalue mesma que você decidiu mover porque a cópia é cara, e você vai ter cuidado para não acessá-lo posteriormente. Veja como você pode transformar lvalue em um xvalue.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

No exemplo de código acima, podemos ainda não tiver movido nada ainda. Que acabamos de criar um xvalue pela transmissão lvalue para uma referência rvalue sem nome. Ele ainda possa ser identificado pelo nome lvalue; Porém, como um xvalue, ele agora é *capaz* de sendo movido. Os motivos para isso, e quais mover, na verdade, se parece com, terá que esperar para outro tópico. Mas você pode pensar no "x" no "xvalue" como significado "especialista"somente se o que ajuda. Pela transmissão lvalue em um xvalue (um tipo de rvalue), o valor então se torna capaz de ser associada a uma referência de rvalue.

Aqui estão duas outros exemplos de xvalues&mdash;chamar uma função que retorna uma referência rvalue sem nome e acessar um membro de um xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
Um prvalue (rvalue pura; um tipo de rvalue) não tem identidade, mas é móvel. Em geral, são temporaries, o resultado da chamada de uma função que retorna por valor, ou o resultado da avaliação de qualquer expressão que não seja um glvalue,

### <a name="rvalue-m"></a>rvalue (m)
Um rvalue é móvel. Uma rvalue *referência* sempre se refere a um rvalue (um valor cujo conteúdo será considerado não precisamos preservar).

Porém, é uma referência de rvalue em si um rvalue? Uma referência de rvalue *sem nome* (como os mostrados nos exemplos de código xvalue acima) é um xvalue portanto, Sim, é um rvalue. Ele prefere seja vinculado a um parâmetro de função de referência rvalue, como de um construtor de movimento. Por outro lado (e talvez counter-intuitively), se uma referência rvalue tem um nome, a expressão consiste nesse nome é lvalue. Portanto, ele *não pode* ser vinculado a um parâmetro de referência rvalue. Mas é fácil para torná-lo a fazer isso&mdash;apenas convertê-lo para uma referência de rvalue sem nome (um xvalue) novamente.

```cppwinrt
void foo(A&) { ... }
void foo(A&&) { ... }
void bar(A&& a) // a is a named rvalue reference; it's an lvalue.
{
    foo(a); // Calls foo(A&).
    foo(static_cast<A&&>(a)); // Calls foo(A&&).
}
A&& get_by_rvalue_ref() { ... } // This unnamed rvalue reference is an xvalue.
```

### <a name="im"></a>\!i\ & \!m
Tipo de valor que não tem identidade e não móveis é a combinação de um que ainda não abordadas. Mas, pode ignorar, porque essa categoria não é uma ideia útil na linguagem C++.

## <a name="reference-collapsing-rules"></a>Regras de recolhimento de referência
Várias referências como em uma expressão (uma referência lvalue para uma referência de lvalue ou uma referência de rvalue para uma referência rvalue) cancelam a um outro out.

- `A& &` Recolhe em `A&`.
- `A&& &&` Recolhe em `A&&`.

Recolher múltiplo ao contrário de referências em uma expressão para uma referência de lvalue.

- `A& &&` Recolhe em `A&`.
- `A&& &` Recolhe em `A&`.

## <a name="forwarding-references"></a>Referências de encaminhamento
Esta seção final contrasta rvalue referências, que já discutimos, com o conceito de diferente de uma *referência de encaminhamento*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` é uma referência de rvalue, como vimos. Const e volátil não se aplicam ao rvalue referências.
- `foo` aceita apenas rvalues de **tipo**.
- O motivo rvalue faz referência (como `A&&`) existe para que você pode criar uma sobrecarga que é otimizada para o caso de um temporário (ou outro rvalue) que está sendo passado.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` é uma *referência de encaminhamento*. Dependendo do que você passa para `bar`, tipo **Ty** poderia ser const/não const independentemente volátil/não volátil.
- `bar` aceita qualquer lvalue ou rvalue do tipo **Ty**.
- Passar lvalue faz com que a referência de encaminhamento para se tornar `_Ty& &&`, que reduz a referência de lvalue `_Ty&`.
- Passar um rvalue faz com que a referência de encaminhamento para se tornar `_Ty&& &&`, que reduz a referência de rvalue `_Ty&&`.
- O motivo pelo qual referências de encaminhamento (como `_Ty&&`) existe é *não* para otimização, mas para tirar o que você passe a eles e encaminhá-la no modo transparente e eficiente. Você provavelmente encontrar uma referência de encaminhamento somente se você escreve (ou investigar detalhadamente) código da biblioteca de&mdash;por exemplo, uma função de fábrica que encaminha argumentos de construtor.

## <a name="sources"></a>Fontes
* \[Stroustrup, 2013\] Stroustrup b: A linguagem de programação C++, a quarta edição. Addison-Wesley. 2013.

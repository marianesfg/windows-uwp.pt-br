---
description: Este tópico descreve as diversas categorias de valores que existem em C++. Você já deve ter ouvido falar de lvalues e rvalues, mas há outros tipos também.
title: Categorias de valor e as referências a eles
ms.date: 08/11/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, projeção, mover, encaminhamento, categorias de valor, semântica de movimentação, o encaminhamento perfeito, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593011"
---
# <a name="value-categories-and-references-to-them"></a>Categorias de valor e as referências a eles
Este tópico descreve as diversas categorias de valores (e referências aos valores) que existem em C++. Você já deve ter ouvido dos *lvalues* e *rvalues*, mas você pode não pensar em termos que este tópico apresenta. E há outros tipos de valores, muito.

Todas as expressões em C++ gera um valor que pertence a uma das categorias de discutidos neste tópico. Há aspectos da linguagem C++, seu facilies e regras, que exigem uma compreensão adequada dessas categorias de valor e as referências a eles. Por exemplo, colocar o endereço de um valor, copiando um valor, movendo um valor e um valor de logon em outra função de encaminhamento. Este tópico não entra em todos esses aspectos detalhadamente, mas ele fornece informações básicas sobre uma compreensão sólida deles.

As informações neste tópico é emoldurada em termos de análise de Stroustrup das categorias de valor por duas propriedades independentes da identidade e movability [Stroustrup 2013].

## <a name="an-lvalue-has-identity"></a>Um lvalue tem uma identidade
O que significa para um valor tenha *identidade*? Se você tiver o endereço de memória de um valor (ou você pode tirar uma) e usá-lo com segurança, em seguida, o valor não tem identidade. Dessa forma, você pode fazer mais de comparar o conteúdo de valores: você pode comparar ou distingui-los por identidade.

Uma *lvalue* tem uma identidade. Agora é uma questão de interesse apenas histórico que o "l" em "lvalue" é uma abreviação de "esquerda" (por exemplo, o esquerdo lado de uma atribuição). No C++, um lvalue pode aparecer à esquerda *ou* à direita de uma atribuição. O "l" em "Values", em seguida, não, na verdade, ajuda você a compreender nem defini-las. Você só precisa entender que o que chamamos de lvalue é um valor que tem identidade.

Exemplos de expressões que são l-Values: uma variável nomeada ou uma constante; ou uma função que retorna uma referência. Exemplos de expressões que são *não* lvalues incluem: um temporário; ou uma função que retorna por valor.

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

Agora, embora seja uma instrução verdadeira lvalues têm identidade, então, fazer xvalues. Falaremos mais sobre o que um *xvalue* é mais adiante neste tópico. Por enquanto, apenas esteja ciente de que há uma categoria de valor chamada glvalue, para "generalizada lvalue". O superconjunto de glvalues contém lvalues (também conhecido como *lvalues clássico*) e xvalues. Portanto, enquanto "um lvalue tem identidade" for true, o conjunto completo de coisas que têm identidade é o conjunto de glvalues, conforme mostrado nesta ilustração.

![Um lvalue tem uma identidade](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Um rvalue é movido; um lvalue não é
Mas há valores que não são glvalues. Consequentemente, há valores que você *não é possível* obter um endereço de memória para (ou você não pode contar que elas sejam válidas). Vimos alguns esses valores no exemplo de código acima. Isso parece uma desvantagem. Mas na verdade a vantagem de um valor que é como que você pode *mover* de (o que é geralmente barato), em vez de copiar dele (que é geralmente caro). Mover de um valor significa que ele não está mais no local que costumava ser. Portanto, tentar acessá-lo no lugar costumava ser é algo a ser evitado. Uma discussão sobre quando e *como* para mover um valor está fora do escopo deste tópico. Para este tópico, só precisamos saber que um valor que pode ser movido é conhecido como um *rvalue* (ou *rvalue clássico*).

O "r" em "rvalue" é uma abreviação de "right" (por exemplo, o direito lado de uma atribuição). Mas você pode usar rvalues e as referências a rvalues, fora de atribuições. O "r" em "Values", em seguida, não é a coisa a se concentrar em. Você só precisa entender que o que chamamos de um rvalue é um valor que pode ser movido.

Um lvalue, por outro lado, não é móvel, conforme mostrado nesta ilustração. Um lvalue que movido seria enfrente a definição de *lvalue*, e seria um problema inesperado para o código que espera, razoavelmente muito ser capaz de continuar a acessar o lvalue.

![Um rvalue é movido; um lvalue não é](images/is-movable.png)

É possível mover um lvalue. Mas há *é* uma espécie de glvalue (o conjunto de coisas com identidade) que você pode mover&mdash;se você souber que você está fazendo (incluindo tomando cuidado para não acessá-lo após a movimentação)&mdash;e que é o xvalue. Voltaremos essa ideia mais uma vez abaixo, quando examinarmos a visão completa das categorias de valor.

## <a name="rvalue-references-and-reference-binding-rules"></a>Referências de Rvalue e regras de associação de referência
Esta seção apresenta a sintaxe para uma referência a um rvalue. Teremos que aguardar outro tópico entrar em um tratamento substancial de mover e encaminhamento, mas esses são problemas que são resolvidos por referências de rvalue. Antes de examinarmos as referências de rvalue, no entanto, primeiro precisamos ficar mais claro sobre `T&` &mdash;a coisa que podemos anteriormente conversa apenas "uma referência". É realmente "uma lvalue (non-const) referência", que se refere a um valor para o qual o usuário da referência pode gravar.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Uma referência de lvalue pode associar a um lvalue, mas não a um rvalue.

Em seguida, há referências de lvalue constantes (`T const&`), que se referem a objetos aos quais o usuário da referência *não é possível* gravação (por exemplo, uma constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Uma referência de lvalue const pode vincular a um lvalue ou a um rvalue.

A sintaxe para uma referência a um rvalue do tipo `T` é escrito como `T&&`. Uma referência rvalue se refere a um valor Movível&mdash;um valor cujo conteúdo não precisamos preservar depois usamos (por exemplo, um temporário). Desde o ponto é mover de (e, portanto, modificação) o valor associado a uma referência de rvalue `const` e `volatile` qualificadores (também conhecido como qualificadores cv) não se aplicam a referências de rvalue.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Associa uma referência de rvalue a um rvalue. Na verdade, em termos de resolução de sobrecarga, um rvalue *prefere* a ser associado a uma referência de rvalue que para uma referência de lvalue const. Mas uma referência de rvalue não é possível associar a um lvalue porque, como já dissemos, uma referência rvalue se refere a um valor cujo conteúdo é considerado, não precisamos preservar (digamos, o parâmetro para um construtor de movimentação).

Você também pode passar um rvalue onde um argumento por valor é esperado, por meio da construção de cópia (ou por meio da construção de movimentação, se o r-Value é um xvalue).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Um glvalue tem identidade; não um prvalue
Nesse estágio, nós sabemos o que tem identidade. E sabemos o que é móvel e o que não tem. Mas ainda não tiver ainda nomeamos o conjunto de valores que *não* têm identidade. Se o conjunto é conhecido como o *prvalue*, ou *rvalue puro*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Um lvalue tem identidade; não um prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>A visão completa das categorias de valor
Ele permanece apenas combinar as informações e ilustrações acima em uma imagem única e grande.

![A visão completa das categorias de valor](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Um glvalue (generalizado lvalue) tem uma identidade.

### <a name="lvalue-im"></a>lvalue (eu\&\!m)
Um lvalue (um tipo de glvalue) tem uma identidade, mas não é móvel. Estes são os valores de leitura / gravação normalmente que você passa ao redor por referência ou por referência const ou valor se a cópia é barato. Não pode ser associado a um lvalue em uma referência rvalue.

### <a name="xvalue-im"></a>xValue (eu\&m)
Um xvalue (um tipo de glvalue, mas também um tipo de rvalue) tem uma identidade e também é movido. Isso pode ser um lvalue a mesma que você decidiu mover como a cópia é cara, e você vai ter cuidado para não acessá-lo posteriormente. Aqui está como você pode transformar um lvalue em uma xvalue.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

No exemplo de código acima, podemos ainda não mudou nada. Assim, criamos um xvalue ao converter um lvalue em uma referência de rvalue sem nome. Ele ainda pode ser identificado por seu nome de lvalue; mas, como um xvalue, agora é *compatível com* do que está sendo movido. Os motivos para fazer isso, e, na verdade, o que mover é semelhante, terá que aguardar outro tópico. Mas você pode pensar no "x" no "xvalue" como o significado "especialista"somente se isso ajuda. Ao converter um lvalue em uma xvalue (um tipo de rvalue), o valor, em seguida, torna-se capaz de ser associado a uma referência de rvalue.

Aqui estão dois outros exemplos de xvalues&mdash;chamando uma função que retorna uma referência de rvalue sem nome e acesso a um membro de um xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!eu\&m)
Um prvalue (rvalue pura; um tipo de rvalue) não tem identidade, mas pode ser movido. Normalmente, esses são os temporários, o resultado de chamar uma função que retorna por valor ou o resultado da avaliação de qualquer outra expressão que não é um glvalue,

### <a name="rvalue-m"></a>rvalue (m)
Um rvalue é movido. Um rvalue *referência* sempre faz referência a um rvalue (um valor cujo conteúdo é considerado, não precisamos preservar).

Mas é uma referência de rvalue um rvalue? Uma *sem-nome* referência de rvalue (como aqueles mostrados nos exemplos de código xvalue acima) é um xvalue dessa forma, Sim, é um rvalue. Ele prefere a ser associado a um parâmetro de função de referência de rvalue, como de um construtor de movimentação. Por outro lado (e talvez counter-intuitively), se uma referência de rvalue tem um nome, a expressão formada por esse nome é um lvalue. Portanto, ele *não é possível* ser associado a um parâmetro de referência de rvalue. Mas é fácil torná-lo a fazer isso&mdash;apenas convertê-lo em uma referência de rvalue sem nome (um xvalue) novamente.

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

### <a name="im"></a>\!Eu\&\!m
O tipo de valor que não tem identidade e não é móvel é a combinação de um que ainda não discutimos. Mas podemos poderá ignorá-lo, porque essa categoria não é uma ideia útil na linguagem C++.

## <a name="reference-collapsing-rules"></a>Regras de recolhimento de referência
Várias referências like em uma expressão (uma referência lvalue para uma referência de lvalue ou uma referência rvalue para uma referência de rvalue) cancelar um outro fora.

- `A& &` Recolhe em `A&`.
- `A&& &&` Recolhe em `A&&`.

Recolher várias Diferentemente de referências em uma expressão para uma referência de lvalue.

- `A& &&` Recolhe em `A&`.
- `A&& &` Recolhe em `A&`.

## <a name="forwarding-references"></a>Referências de encaminhamento
Nesta seção final contrasta as referências de rvalue, que já discutimos, com o conceito de diferente de um *referência de encaminhamento*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` Como vimos, é uma referência de rvalue. Const e volatile não se aplicam a referências de rvalue.
- `foo` aceita apenas rvalues do tipo **um**.
- Referências de rvalue o motivo (como `A&&`) existe é para que você pode criar uma sobrecarga que é otimizada para o caso de um temporário (ou outro rvalue) que está sendo passado.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` é um *referência de encaminhamento*. Dependendo de você passar para `bar`, digite **_Ty** poderia ser const/não const independentemente volátil/não-volátil.
- `bar` aceita qualquer lvalue ou rvalue do tipo **_Ty**.
- Passar um lvalue faz com que a referência de encaminhamento para se tornar `_Ty& &&`, que se recolhe para a referência de lvalue `_Ty&`.
- Passar um rvalue faz com que a referência de encaminhamento para se tornar `_Ty&& &&`, que se recolhe para uma referência rvalue `_Ty&&`.
- O motivo pelo qual as referências de encaminhamento (como `_Ty&&`) existe é *não* para a otimização, mas para tirar o que é passado para elas e encaminhá-lo no modo transparente e eficiente. Você provavelmente encontrará uma referência de encaminhamento, somente se você escrever (ou Estude atentamente) código de biblioteca&mdash;, por exemplo, uma função de fábrica que encaminha nos argumentos de construtor.

## <a name="sources"></a>Fontes
* \[Stroustrup, 2013\] Stroustrup B.: A linguagem de programação C++, quarta edição. Addison-Wesley. 2013.

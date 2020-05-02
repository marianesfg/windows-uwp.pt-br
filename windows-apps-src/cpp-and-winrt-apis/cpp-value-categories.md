---
description: Este tópico descreve as diversas categorias de valores que existem em C++. Você já deve ter ouvido falar de lvalues e rvalues, mas há outros tipos também.
title: Categorias de valores e referências a eles
ms.date: 08/11/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, moving, forwarding, value categories, move semantics, perfect forwarding, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1312b84ded26859cd4b83ffbe3e8a75bfdef6950
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77037876"
---
# <a name="value-categories-and-references-to-them"></a>Categorias de valores e referências a eles
Este tópico descreve as diversas categorias de valores (e referências a valores) que existem em C++. Sem dúvida você já ouviu falar de *lvalues* e *rvalues*, mas talvez não pense neles nos termos apresentados neste tópico. E também há outros tipos de valores.

Todas as expressões em C++ geram um valor que pertence a uma das categorias discutidas neste tópico. Há aspectos, facilidades e regras da linguagem C++ que exigem um bom entendimento das categorias de valores e das referências a eles. Por exemplo, usar o endereço, copiar, mover e encaminhar um valor para outra função. Neste tópico, não abordaremos todos esses aspectos em profundidade, mas forneceremos informações básicas para promover um entendimento sólido.

As informações deste tópico estão formuladas nos termos da análise de Stroustrup das categorias de valor por meio de duas propriedades independentes de identidade e mobilidade [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Um lvalue tem uma identidade
Para um valor, o que significa ter uma *identidade*? Se você tiver (ou puder obter) o endereço de memória de um valor e usá-lo com segurança, o valor terá uma identidade. Dessa maneira, é possível fazer mais que comparar o conteúdo dos valores: você pode comparar ou diferenciá-los pela identidade.

Um *lvalue* tem uma identidade. Agora, é somente uma questão de interesse histórico o fato de que o "l" em "lvalue" é uma abreviação de "left" (como no lado esquerdo de uma atribuição). Em C++, um lvalue pode aparecer à esquerda *ou* à direita de uma atribuição. Portanto, o "l" de "lvalues" não ajuda você a realmente compreender nem definir o que eles são. Você só precisa entender que o que chamamos de lvalue é um valor com uma identidade.

Alguns exemplos de expressões lvalues: uma variável ou constante com nome ou uma função que retorna uma referência. Alguns exemplos de expressões que *não* são lvalues: um temporário ou uma função retornada por valor.

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

No entanto, não são só os lvalues que têm identidade. Os xvalues também. Falaremos mais sobre o que é um *xvalue* posteriormente neste tópico. Por enquanto, saiba que há uma categoria de valores chamada glvalue, para "lvalue generalizado". O superconjunto de glvalues contém tanto os lvalues (também conhecidos como *lvalues clássicos*) como os xvalues. Portanto, embora a afirmação "um lvalue tem identidade" seja verdadeira, o conjunto completo de itens com identidade é o conjunto de glvalues, conforme mostrado na ilustração.

![Um lvalue tem uma identidade](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Um rvalue pode ser movido; um lvalue não
Porém, há valores que não são glvalues. Consequentemente, *não* é possível obter o endereço de memória de alguns valores (ou você não pode confiar na validade dele). Vimos alguns valores desse tipo no código de exemplo acima. Parece uma desvantagem. Mas, na verdade, a vantagem de um valor desse tipo é que você pode *movê-lo* (o que geralmente é barato) em vez de copiá-lo (o que geralmente é caro). Mover um valor significa que ele não está mais em seu local de origem. Portanto, tentar acessá-lo no local antigo é algo que deve ser evitado. Não está no escopo deste tópico discutir quando e *como* mover um valor. Neste tópico, só precisamos saber que um valor móvel é conhecido como *rvalue* (ou *rvalue clássico*).

O "r" em "rvalue" é uma abreviação de "right" (como no lado direito de uma atribuição). No entanto, é possível usar rvalues e referências a eles fora de atribuições. Portanto, o "r" em "rvalues" não é o foco. Você só precisa entender que o que chamamos de rvalue é um valor móvel.

Por outro lado, um lvalue não é móvel, conforme mostrado na ilustração. Um lvalue movido desafiaria a definição de *lvalue*. Além disso, seria um problema inesperado para o código que, razoavelmente, espera poder continuar a acessar o lvalue.

![Um rvalue pode ser movido; um lvalue não](images/is-movable.png)

Não é possível mover um lvalue. Contudo, *há* um tipo de glvalue (o conjunto de itens com identidade) que poderá ser movido&mdash;se você souber o que está fazendo (por exemplo, ter cuidado para não acessá-lo após a movimentação)&mdash;o xvalue. Voltaremos a essa ideia mais adiante ao analisarmos o panorama completo das categorias de valores.

## <a name="rvalue-references-and-reference-binding-rules"></a>Referências rvalue e regras de associação de referências
Nesta seção, apresentamos a sintaxe de uma referência a um rvalue. Precisaremos de outro tópico para tratar de movimentação e encaminhamento de maneira significativa, mas basta dizer que as referências de rvalue são uma parte necessária da solução desses problemas. No entanto, antes de examinarmos as referências rvalue, primeiro é preciso esclarecer `T&`&mdash;o que estamos chamando formalmente de "referência". É realmente "uma referência lvalue (não const)", que se refere a um valor em que o usuário de referência pode gravar.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Uma referência lvalue pode ser associada a um lvalue, mas não a um rvalue.

Desse modo, há referências const lvalue (`T const&`), que se referem a objetos em que o usuário da referência *não* pode gravar (por exemplo, uma constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Uma referência const lvalue pode ser associada a um lvalue ou a um rvalue.

A sintaxe de uma referência a um rvalue do tipo `T` é gravada como `T&&`. Uma referência rvalue se refere a um valor móvel &mdash; um valor cujo conteúdo não precisa ser preservado após o uso (por exemplo, um temporário). Como o objetivo é mover-se (e, portanto, modificar) do valor associado para uma referência rvalue, os qualificadores `const` e `volatile` (também conhecidos como cv-qualifiers) não se aplicam às referências rvalue.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Uma referência rvalue pode ser associada a um rvalue. Na verdade, em termos de resolução de sobrecarga, um rvalue *prefere* estar associado a uma referência rvalue do que a uma referência const lvalue. No entanto, uma referência rvalue não pode ser associada a um lvalue porque, como dito, assume-se que uma referência rvalue se refere a um valor cujo conteúdo não precisa ser preservado (por exemplo, o parâmetro de um construtor de movimentação).

Também é possível passar um rvalue em que se espera um argumento por valor, por meio de uma construção de cópia (ou uma construção de movimentação, se o rvalue for um xvalue).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Um glvalue tem identidade; um prvalue não
Nesse estágio, sabemos o que tem identidade. E sabemos o que é móvel ou não. No entanto, ainda não nomeamos o conjunto de valores que *não* tem identidade. Esse conjunto é conhecido como *prvalue* ou *rvalue puro*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Um lvalue tem identidade; um prvalue não](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>Panorama completo das categorias de valores
Somente para combinar as informações e as ilustrações acima em uma única imagem.

![Panorama completo das categorias de valores](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Um glvalue (lvalue generalizado) tem identidade.

### <a name="lvalue-im"></a>lvalue (i\&\!m)
Um lvalue (um tipo de glvalue) tem identidade, mas não é móvel. Normalmente, esses são os valores de leitura/gravação que você passa por referência ou referência const ou por valor se a cópia for barata. Um lvalue não pode ser associado a uma referência rvalue.

### <a name="xvalue-im"></a>xvalue (i\&m)
Um xvalue (um tipo de glvalue, mas também um tipo de rvalue) tem identidade e é móvel. Pode ser um lvalue antigo que você decidiu mover porque copiar é caro e você terá cuidado para não acessá-lo depois. Veja como transformar um lvalue em um xvalue.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

No exemplo de código acima, nada foi movido. Somente criamos um xvalue ao transmitir um lvalue para uma referência rvalue sem nome. Ainda é possível identificá-lo pelo nome do lvalue; no entanto, como xvalue, agora ele *pode* ser movido. Os motivos para isso e como a movimentação é feita ficará para outro tópico. Mas você pode pensar no "x" de "xvalue" como "expert-only" se isso for útil. Ao transmitir um lvalue para um xvalue (um tipo de rvalue), o valor poderá ser associado a uma referência rvalue.

Veja dois outros exemplos de xvalue &mdash; chamar uma função que retorna uma referência rvalue sem nome e acessar um membro de um xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\&m)
Um prvalue (rvalue puro; um tipo de rvalue) não tem identidade, mas é móvel. Normalmente, eles são temporários, o resultado de chamar uma função retornada por valor ou de avaliar qualquer outra expressão que não seja um glvalue.

### <a name="rvalue-m"></a>rvalue (m)
Um rvalue é móvel. Uma *referência* rvalue sempre se refere a um rvalue (um valor cujo conteúdo não precisa ser preservado).

No entanto, uma referência rvalue é um rvalue? Uma referência rvalue *sem nome* (como as mostradas nos exemplos de código xvalue acima) é um xvalue, então, sim, é um rvalue. Ele prefere ser associado a um parâmetro de função de referência rvalue, como o de um construtor de movimentação. Por outro lado (e talvez de modo contraintuitivo), se uma referência rvalue tiver nome, a expressão formada por esse nome será um lvalue. Então, ele *não* pode ser associado a um parâmetro de referência rvalue. Entretanto, é fácil reverter essa situação, basta transmiti-lo para uma referência rvalue sem nome (um xvalue) novamente.

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

### <a name="im"></a>\!i\&\!m
O tipo de valor que não tem identidade e não é móvel é a única combinação que ainda não abordamos. No entanto, podemos ignorá-lo, porque essa categoria não é uma ideia útil na linguagem C++.

## <a name="reference-collapsing-rules"></a>Regras de recolhimento de referências
Muitas referências semelhantes em uma expressão (uma referência lvalue a uma referência lvalue ou uma referência rvalue a uma referência rvalue) cancelam umas as outras.

- `A& &` se recolhe em `A&`.
- `A&& &&` se recolhe em `A&&`.

Muitas referências diferentes em uma expressão se recolhem em uma referência lvalue.

- `A& &&` se recolhe em `A&`.
- `A&& &` se recolhe em `A&`.

## <a name="forwarding-references"></a>Referências de encaminhamento
Nesta seção final, contrastamos as referências rvalue, que já foram discutidas, com o diferente conceito de uma *referência de encaminhamento*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` é uma referência rvalue, como já sabemos. Const e volatile não se aplicam a referências rvalue.
- `foo` aceita somente rvalues do tipo **A**.
- As referências rvalue (como `A&&`) existem para que você possa criar uma sobrecarga otimizada caso um temporário (ou outro rvalue) seja passado.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` é uma *referência de encaminhamento*. De acordo com o que você passar para `bar`, o tipo **_Ty** poderá ser const/non-const independentemente de volatile/non-volatile.
- `bar` aceita qualquer lvalue ou rvalue do tipo **_Ty**.
- Passar um lvalue faz com que a referência de encaminhamento se transforme em `_Ty& &&`, que recolhe para a referência lvalue `_Ty&`.
- Passar um rvalue faz com que a referência de encaminhamento se transforme em `_Ty&& &&`, que recolhe para a referência rvalue `_Ty&&`.
- As referências de encaminhamento (como `_Ty&&`) *não* existem para otimização, mas para encaminhar o que você passa para elas de maneira transparente e eficiente. Você encontrará uma referência de encaminhamento somente se gravar (ou estudar atentamente) o código da biblioteca, por exemplo, uma função de fábrica que encaminha por meio de argumentos de construtor.

## <a name="sources"></a>Fontes
* \[Stroustrup, 2013\] B. Stroustrup: The C++ Programming Language, quarta edição. Addison-Wesley. 2013.

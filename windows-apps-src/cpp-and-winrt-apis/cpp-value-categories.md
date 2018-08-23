---
author: stevewhims
description: Este tópico descreve as várias categorias de valores que existem no C++. Você já deve ter ouvido de lvalues e rvalues, mas há outros tipos, muito.
title: Categorias de valor e referências a eles
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, projeção, mover, encaminhamento, categorias de valor, semântica de movimentação, encaminhamento perfeito, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.openlocfilehash: cbccaf78b45d85d93619977d149431c4eec9e10a
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2814668"
---
# <a name="value-categories-and-references-to-them"></a>Categorias de valor e referências a eles
Este tópico descreve as várias categorias de valores (e referências aos valores) que existem no C++. Você já deve ter ouvido de *lvalues* e *rvalues*, mas você não pode pensa nos termos que este tópico apresenta. E existem outros tipos de valores, muito.

Cada expressão no C++ produz um valor que pertence a uma das categorias discutidas neste tópico. Há aspectos da linguagem C++, seu facilies e regras, que exigem uma compreensão adequada dessas categorias de valor e referências a eles. Por exemplo, pegar o endereço de um valor, copiando um valor, um valor de movimentação e um valor em outra função de encaminhamento. Este tópico não entram em todos os aspectos detalhadamente, mas ela fornece informações fundamentais para ter uma compreensão sólida deles.

As informações neste tópico é com moldura em termos de análise de Stroustrup das categorias de valor pelas duas propriedades independentes de identidade e movability [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Lvalue tem identidade
O que significa um valor ter *identidade*? Se você tiver o endereço de memória de um valor (ou você pode tomar uma) e usá-lo com segurança, e em seguida, o valor com a identidade. Dessa forma, você pode fazer mais do que compare o conteúdo dos valores: você pode comparar ou diferenciá-los pelo identity.

Um *lvalue* tem uma identidade. Agora é uma questão de interesse apenas histórica que "l" em "lvalue" é uma abreviação de "left" (como à esquerda-à-lado de uma atribuição). No C++, lvalue pode aparecer na esquerda *ou* à direita de uma atribuição. "L" em "lvalues", em seguida, não realmente ajudá-lo a compreender nem definir quais são. Você só precisa entender que que chamamos lvalue é um valor que tem identity.

Exemplos de expressões que são lvalues: uma variável nomeada ou constante; ou uma função que retorna uma referência. Exemplos de expressões que são *não* lvalues: temporária; ou uma função que retorna por valor.

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

Agora, embora seja uma instrução true que possuem identidade lvalues, então, fazer xvalues. Passaremos mais a quais um *xvalue* é posteriormente neste tópico. No momento, basta esteja ciente de que haja uma categoria de valor chamada glvalue, para "generalizada lvalue". O subconjunto de glvalues contém lvalues (também conhecido como *lvalues clássico*) e o xvalues. Portanto, enquanto "lvalue possui a identidade" for true, o conjunto completo de coisas que possuem identidade é o conjunto de glvalues, conforme mostrado nesta ilustração.

![Lvalue tem identidade](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Um rvalue é móvel; lvalue não é
Mas há valores que não são glvalues. Consequentemente, há valores que você *não pode* obter um endereço de memória para (ou se você não pode depender seja válido). Vimos alguns valores tão no exemplo de código acima. Isso SOA como uma desvantagem. Mas na verdade a vantagem de um valor like, ou seja, que você pode *Mover* a partir dele (que é geralmente barato), ao invés copiem (que é geralmente caro). Mudando de um valor significa que ele não está mais no lugar costumava ser. Então, tentar acessá-lo no lugar de ser é algo a ser evitada. Uma discussão sobre quando e *como* mover que um valor está fora do escopo deste tópico. Para este tópico, precisamos apenas saber que um valor que é móvel é conhecido como um *rvalue* (ou *rvalue clássico*).

O "r" em "rvalue" é uma abreviação de "direita" (como o direita-à-lado de uma atribuição). Mas você pode usar rvalues e referências para rvalues, fora de atribuições. O "r" em "rvalues", em seguida, não é a coisa para focalizar em. Você só precisa entender que que chamamos uma rvalue é um valor que é móvel.

Lvalue, de modo oposto, não movível, conforme mostrado nesta ilustração. Lvalue movidos seria enfrente a definição de *lvalue*e seria um problema inesperado para o código que muito razoavelmente esperado possam continuar acessando o lvalue.

![Um rvalue é móvel; lvalue não é](images/is-movable.png)

Não é possível mover lvalue. Mas há *é* uma espécie de glvalue (o conjunto de coisas com identidade) que você pode mover&mdash;se você souber que você está fazendo (incluindo tomando cuidado para não acessá-lo após a movimentação)&mdash;e que é o xvalue. Podemos vai rever essa ideia mais uma vez abaixo, quando procuramos a imagem completa das categorias de valor.

## <a name="rvalue-references-and-reference-binding-rules"></a>Referências de Rvalue e regras de ligação de referência
Esta seção apresenta a sintaxe de uma referência a um rvalue. Teremos esperar por outro tópico entrar em um tratamento substancial da movimentação e encaminhamento, mas esses são os problemas que são resolvidos por rvalue referências. Antes de observarmos rvalue referências, no entanto, primeiro precisamos ser mais claro sobre `T&` &mdash;a coisa podemos anteriormente conversa apenas "referência". É realmente "uma lvalue (não-const) referência", que se refere a um valor para o qual o usuário da referência pode gravar.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Uma referência de lvalue pode ligar para lvalue, mas não para um rvalue.

E em seguida, há referências de const lvalue (`T const&`), que se referem a objetos aos quais o usuário sobre a referência *não é possível* gravar (por exemplo, uma constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Uma referência de const lvalue pode vincular a lvalue ou a um rvalue.

A sintaxe de uma referência a um rvalue do tipo `T` está escrito como `T&&`. Uma referência de rvalue se refere a um valor Movível&mdash;um valor cujo conteúdo não precisamos preservar depois que usamos ela (por exemplo, um temporário). Desde o principal objetivo para mover de (e, portanto, modificando) o valor acoplado a uma referência de rvalue `const` e `volatile` qualificador (também conhecido como VC-qualificador) não se aplicam ao rvalue referências.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Uma referência de rvalue vincula a um rvalue. Na verdade, em termos de resolução de sobrecarga, um rvalue *prefira* ser vinculado a uma referência de rvalue que a uma referência de const lvalue. Mas, uma referência de rvalue não é possível vincular a lvalue porque, como dissemos, uma referência de rvalue se refere a um valor cujo conteúdo será considerado não precisamos preservar (digamos, o parâmetro para um construtor de movimentação).

Você também pode passar um rvalue onde um argumento por valor é esperado, por meio de construção de cópia (ou construção de movimentação, se o rvalue for um xvalue).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Um glvalue com a identidade; um prvalue não
Nesse estágio, nós sabemos o que possui a identidade. E sabemos o que é móvel e o que não está. Mas não temos ainda chamado o conjunto de valores que *não* possuem identidade. Esse conjunto é conhecido como o *prvalue*ou *rvalue puro*.

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

## <a name="the-complete-picture-of-value-categories"></a>A imagem completa das categorias de valor
Ele permanece apenas combinar a info e as ilustrações acima em uma única imagem ganhar.

![A imagem completa das categorias de valor](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Um glvalue (generalizada lvalue) tem uma identidade.

### <a name="lvalue-im"></a>lvalue (i\ & \!m)
Lvalue (uma espécie de glvalue) tem identidade, mas não movível. Estes são os valores de leitura-gravação normalmente que você passa ao redor por referência ou por referência constante ou pelo valor se a cópia é barato. Lvalue não pode ser associado a uma referência de rvalue.

### <a name="xvalue-im"></a>xValue (i\ & m)
Um xvalue (uma espécie de glvalue, mas também um tipo de rvalue) tem identity e também é móvel. Isso pode ser lvalue mesma que você decidiu mover porque copiando é cara e você vai cuidado para não acessá-lo posteriormente. Aqui está como você pode transformar lvalue em um xvalue.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

No exemplo de código acima, podemos ainda não tiver movido nada ainda. Que acabamos de criar um xvalue por casting lvalue para uma referência de rvalue sem nome. Ele ainda pode ser identificado por seu nome de lvalue; mas, como um xvalue, agora é *capaz* de sendo movido. Os motivos para fazer isso, e quais movendo realmente aparência, terão de aguardar para outro tópico. Mas você pode pensar no "x" no "xvalue" como o significado "especialista"somente se isto ajuda. Convertendo lvalue em um xvalue (uma espécie de rvalue), o valor passará a ser capaz de sendo vinculados a uma referência de rvalue.

Aqui estão dois outros exemplos de xvalues&mdash;chamar uma função que retorna uma referência de não-nomeada rvalue e acessar um membro de um xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
Um prvalue (puro rvalue; um tipo de rvalue) não tem a identidade, mas é móvel. Geralmente, essas são temporaries, o resultado de chamar uma função que retorna por valor ou o resultado da avaliação de qualquer outra expressão que não seja um glvalue,

### <a name="rvalue-m"></a>rvalue (m)
Um rvalue é móvel. Uma *referência* de rvalue sempre se refere a um rvalue (um valor cujo conteúdo será considerado não precisamos preservar).

Mas, é uma referência de rvalue próprio um rvalue? Uma referência de rvalue *sem nome* (como aqueles mostrados nos exemplos de código xvalue acima) é um xvalue assim, Sim, é um rvalue. Ele prefere ser vinculado a um parâmetro de função de referência rvalue, como o de um construtor de movimentação. Inversamente (e talvez counter-intuitively), caso uma referência de rvalue tem um nome, a expressão formada por esse nome será lvalue. Portanto ele *não pode* ser vinculada a um parâmetro de referência rvalue. Mas é fácil para torná-lo a fazê-lo&mdash;apenas convertê-lo para uma referência de não-nomeada rvalue (um xvalue) novamente.

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
Tipo de valor que não possui a identidade e não é Movível é a combinação de um, ainda não tiver discutido. Mas nós pode desconsiderar, porque essa categoria não for uma ideia útil na linguagem C++.

## <a name="reference-collapsing-rules"></a>Regras de recolhimento de referência
Várias referências like em uma expressão (uma referência de lvalue a uma referência de lvalue, ou uma referência de rvalue a uma referência de rvalue) cancelar a uma outra out.

- `A& &` Recolhe em `A&`.
- `A&& &&` Recolhe em `A&&`.

Vários diferentemente referências em uma expressão recolher para uma referência de lvalue.

- `A& &&` Recolhe em `A&`.
- `A&& &` Recolhe em `A&`.

## <a name="forwarding-references"></a>Referências de encaminhamento
Esta seção final contrasta as referências de rvalue, que já discutidas, com o conceito de diferente de uma *referência de encaminhamento*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` é uma referência de rvalue, como vimos. Const e voláteis não se aplicam às referências rvalue.
- `foo` aceita somente rvalues do tipo **uma**.
- As referências a rvalue motivo (como `A&&`) existir para que você pode criar uma sobrecarga que é otimizada para o caso de temporária (ou outro rvalue) está sendo passado.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` é uma *referência de encaminhamento*. Dependendo de você passa para `bar`, tipo **_Ty** poderia ser a constante/não const independentemente volátil/não voláteis.
- `bar` aceita qualquer lvalue ou rvalue do tipo **_Ty**.
- Passar lvalue faz com que a referência de encaminhamento para se tornar `_Ty& &&`, qual recolhe para a referência de lvalue `_Ty&`.
- Passar uma rvalue faz com que a referência de encaminhamento para se tornar `_Ty&& &&`, qual recolhe para a referência de rvalue `_Ty&&`.
- O motivo pelo qual encaminhamento referências (como `_Ty&&`) existir é *não* para otimização, mas assuma o que você passa para eles e encaminhá-la em transparente e eficiência. Você provavelmente encontrar uma referência de encaminhamento somente se você gravar (ou bastante estudar) código biblioteca&mdash;por exemplo, uma função de fábrica que encaminha nos argumentos do construtor.

## <a name="sources"></a>Fontes
* \[Stroustrup, 2013\] B. Stroustrup: A linguagem de programação do C++, a quarta edição. Addison-Wesley. 2013.

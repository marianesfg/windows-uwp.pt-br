---
description: Este tópico explica os mapeamentos de namespace XML/XAML (xmlns) conforme encontrados no elemento raiz da maioria dos arquivos XAML. Descreve também como produzir mapeamentos similares para tipos e assemblies personalizados.
title: Namespaces XAML e mapeamento de namespace
ms.assetid: A19DFF78-E692-47AE-8221-AB5EA9470E8B
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4332bd0b19d381937e477efc472634d6d81afd58
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9046489"
---
# <a name="xaml-namespaces-and-namespace-mapping"></a>Namespaces XAML e mapeamento de namespace


Este tópico explica os mapeamentos de namespace XML/XAML (**xmlns**) conforme encontrados no elemento raiz da maioria dos arquivos XAML. Descreve também como produzir mapeamentos similares para tipos e assemblies personalizados.

## <a name="how-xaml-namespaces-relate-to-code-definition-and-type-libraries"></a>Como os namespaces XAML se relacionam a definição de código e bibliotecas de tipos

Em termos de finalidade geral e aplicação na programação de aplicativos do Tempo de Execução do Windows, o XAML é usado para declarar objetos, propriedades desses objetos e relações entre objeto e propriedade expressas como hierarquias. Os objetos declarados no XAML são auxiliados pelas bibliotecas de tipos ou outras representações definidas por outras técnicas e linguagens de programação. Essas bibliotecas podem ser:

-   O conjunto de objetos interno do Tempo de Execução do Windows. Trata-se de um conjunto fixo de objetos, e o acesso a esses objetos a partir do XAML usa a lógica de ativação e o mapeamento de tipos interno.
-   Bibliotecas distribuídas que são fornecidas pela Microsoft ou por terceiros.
-   Bibliotecas que representam a definição de um controle de terceiros que o seu aplicativo incorpora e o pacote redistribui.
-   Sua própria biblioteca, que faz parte de seu projeto e contém algumas ou todas as definições de seu código de usuário.

As informações de tipo de suporte são associadas a determinadas funções do namespace XAML. Estruturas XAML, como o Windows Runtime, podem agregar vários assemblies e diversos namespaces de código para o mapeamento para um único namespace XAML. Isso possibilita o conceito de um vocabulário XAML abrangendo uma tecnologia ou estrutura de programação maior. Um vocabulário XAML pode ser bastante extensivo por exemplo, a maior parte do XAML documentado para aplicativos do Windows Runtime nessa referência constitui um único vocabulário XAML. Um vocabulário XAML também é extensível: você o amplia adicionando tipos às definições de código de suporte, garantindo a inclusão dos tipos nos namespaces de código já usados como fontes de namespaces mapeadas do vocabulário XAML.

Um processador XAML pode examinar tipos e membros nos assemblies de suporte associados ao namespace XAML quando ele cria uma representação do objeto de tempo de execução. Por isso que o XAML é útil para formalizar e trocar definições de comportamento de construção de objeto e é usado como uma técnica de definição de interface do usuário em um aplicativo UWP.

## <a name="xaml-namespaces-in-typical-xaml-markup-usage"></a>Namespaces XAML em uso típico de marcação XAML

Um arquivo XAML nem sempre declara um namespace XAML padrão no elemento raiz. O namespace XAML padrão define quais elementos podem ser declarados sem qualificação via prefixo. Por exemplo, se você declarar um elemento `<Balloon />`, o analisador XAML esperará até que um elemento **Balloon** exista e seja válido no namespace XAML padrão. Por outro lado, se **Balloon** não for definido no namespace XAML padrão, qualifique esse nome de elemento com um prefixo; por exemplo, `<party:Balloon />`. O prefixo indica que o elemento existe em outro namespace XAML diferente do namespace padrão e que você deve mapear um namespace XAML para o prefixo **party** para poder usar esse elemento. Os namespaces XAML se aplicam ao elemento específico em que são declarados e também a qualquer elemento contido por esse elemento na estrutura XAML. Por isso, os namespaces XAML quase sempre são declarados nos elementos raiz de um arquivo XAML, para obter as vantagens dessa herança.

## <a name="the-default-and-xaml-language-xaml-namespace-declarations"></a>Declarações do namespace XAML padrão e de linguagem XAML

No elemento raiz da maioria dos arquivos XAML, há duas declarações de **xmlns**. A primeira declaração mapeia um namespace XAML como o padrão:  `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`

Este é o mesmo identificador de namespace XAML usado em várias tecnologias Microsoft predecessoras, que também usam XAML como um formato de marcação de definição de interface do usuário. O uso do mesmo identificador é proposital, além de ser útil quando você migra a interface do usuário definida anteriormente para um aplicativo do Tempo de Execução do Windows em C++, C# ou Visual Basic.

A segunda declaração mapeia um namespace XAML separado para os elementos de linguagem definidos por XAML, geralmente mapeando-os para o prefixo "x:":  `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`

Esse valor de **xmlns** e o prefixo "x:" para o qual ele é mapeado também são idênticos às definições usadas em várias tecnologias anteriores da Microsoft que usam XAML.

A relação entre essas declarações é que o XAML é uma definição de linguagem e o Windows Runtime é uma implementação que usa o XAML como uma linguagem e define um vocabulário específico, no qual os tipos são referenciados em XAML.

A linguagem XAML especifica determinados elementos de linguagem e cada um deles deve ser acessível às implementações de processador XAML que estão trabalhando com base no namespace XAML. A convenção de mapeamento "x:" do namespace XAML da linguagem XAML é cumprida pelos modelos de projeto, exemplos de código e pela documentação de recursos da linguagem. O namespace da linguagem XAML define vários recursos de uso comum, que são necessários até mesmo aos aplicativos básicos do Tempo de Execução do Windows em C++, C# ou Visual Basic. Por exemplo, para adicionar qualquer code-behind a um arquivo XAML por meio de uma classe parcial, você tem que nomear essa classe como o [atributo x:Class](x-class-attribute.md) no elemento raiz do arquivo XAML relevante. Ou então, qualquer elemento definido em uma página XAML como um recurso inserido em um [ResourceDictionary e referências de recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273) deve ter o [atributo x:Key](x-key-attribute.md) definido no elemento de objeto em questão.

<span id="other-XAML-namespaces"/>

## <a name="other-xaml-namespaces"></a>Outros namespaces XAML

Além do namespace padrão e do namespace XAML da linguagem XAML "x:", você também pode ver outros namespaces XAML mapeados no XAML padrão inicial para aplicativos, conforme gerados pelo Microsoft Visual Studio.

### **<a name="d-httpschemasmicrosoftcomexpressionblend2008"></a>d: (`http://schemas.microsoft.com/expression/blend/2008`)**

O namespace XAML "d:" foi projetado para auxiliar o designer, especificamente nas áreas de design XAML do Microsoft Visual Studio. O namespace XAML "d:" permite atributos de designer ou de tempo de design em elementos XAML. Esses atributos de designer afetam apenas os aspectos de design referentes ao comportamento do XAML. Os atributos de designer são ignorados quando o mesmo XAML é carregado pelo analisador XAML do Tempo de Execução do Windows, durante a execução de um aplicativo. Os atributos de designer geralmente são válidos em qualquer elemento XAML, mas, na prática, há apenas alguns cenários em que a aplicação do atributo de designer é adequada. Em especial, muitos dos atributos de designer são projetados para fornecer uma experiência melhor de interação com os contextos e as fontes de dados, quando você desenvolve XAML e código que usam a vinculação de dados.

-   **Atributos d:DesignHeight e d:DesignWidth:** Esses atributos são, às vezes, aplicados na raiz de um arquivo XAML que o Visual Studio ou outra superfície do designer do XAML cria para você. Por exemplo, esses atributos são definidos na raiz do [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) do XAML que é criado se você adiciona um novo **UserControl** ao seu projeto de aplicativo. Esses atributos facilitam a criação da composição do conteúdo XAML, por isso você tem alguma previsão das restrições de layout que podem existir depois que o conteúdo XAML é usado para uma instância de controle ou outra parte de uma página de interface do usuário maior.

   **Observação**se você estiver migrando o XAML do Microsoft Silverlight poderá ter esses atributos nos elementos raiz que representam uma página de interface do usuário inteira. Talvez você queira remover os atributos nesse caso. Outros recursos dos designers de XAML, como o simulador, provavelmente são mais úteis na criação de layouts de página que manipulem dimensionamento e estados de exibição do que um layout de página de tamanho fixo que usa **d:DesignHeight** e **d:DesignWidth**.

-   **Atributo d:DataContext:** Você pode definir esse atributo em uma raiz de página ou um controle para substituir qualquer [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) explícito ou herdado que o objeto tenha.
-   **Atributo d:DesignSource:** Especifica uma fonte de dados de tempo de design para um [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833), substituindo [**Source**](https://msdn.microsoft.com/library/windows/apps/br209835).
-   **Extensões de marcação d:DesignInstance e d:DesignData:** Essas extensões de marcação são usadas para fornecer os recursos de dados de tempo de design para **d:DataContext** ou **d:DesignSource**. Não documentamos totalmente aqui a maneira de usar os recursos de dados de tempo de design. Para obter mais informações, consulte [Atributos de tempo de design](https://go.microsoft.com/fwlink/p/?LinkId=272504). Para obter alguns exemplos de uso, consulte [Dados de exemplo na área de design e para a criação de protótipo](https://msdn.microsoft.com/library/windows/apps/mt517866).

### **<a name="mc-httpschemasopenxmlformatsorgmarkup-compatibility2006"></a>mc: (`http://schemas.openxmlformats.org/markup-compatibility/2006`)**

" mc:" indica e dá suporte ao modo de compatibilidade de marcação para leitura de XAML. Normalmente, o prefixo "d:" prefix é associado ao atributo **mc:Ignorable**. Essa técnica permite que os analisadores XAML de tempo de execução ignorem os atributos de design em "d:".

### <a name="local-and-common"></a>**local:** e **common:**

"local:" é um prefixo que geralmente é mapeado para você nas páginas XAML para um projeto modelo de aplicativo UWP. Ele é mapeado para se referir ao mesmo namespace que é criado para conter o [x:Class attribute](x-class-attribute.md) e o código para todos os arquivos XAML, inclusive app.xaml. Contanto que defina as classes personalizadas que deseja usar no XAML nesse mesmo namespace, você pode usar o prefixo **local:** para se referir aos seus tipos personalizados em XAML. Um prefixo relacionado proveniente de um projeto modelo de aplicativo UWP é **common:**. Esse prefixo se refere a um namespace "Common" aninhado que contém classes de utilitários, como conversores e comandos, e você pode encontrar as definições na pasta Common no modo de exibição do **Gerenciador de Soluções**.

### **<a name="vsm"></a>vsm:**

Não use. "vsm:" é um prefixo que, às vezes, é visto em modelos XAML antigos importados de outras tecnologias Microsoft. Originalmente, o namespace tratava de um problema de ferramenta de namespace herdado. Exclua definições de namespace XAML para "vsm:" em qualquer XAML que usar para o Windows Runtime e mude todos os usos de prefixo de [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014) e objetos relacionados para usar o namespace XAML padrão. Para saber mais sobre migração de XAML, veja [Migrando XAML/código do Silverlight ou WPF para um aplicativo do Tempo de Execução do Windows](https://msdn.microsoft.com/library/windows/apps/br229571).

## <a name="mapping-custom-types-to-xaml-namespaces-and-prefixes"></a>Mapeando tipos personalizados para namespaces XAML e prefixos

Você pode mapear um namespace XAML para poder usar o XAML para acessar seus próprios tipos personalizados. Em outras palavras, você mapeia um namespace de código, da forma como ele aparece em uma representação de código que define o tipo personalizado, e o atribui a um namespace XAML juntamente com um prefixo para utilização. Tipos personalizados de XAML podem ser definidos em uma linguagem do Microsoft .NET (C# ou Microsoft Visual Basic) ou em C++. O mapeamento é feito pela definição de um prefixo **xmlns**. Por exemplo, `xmlns:myTypes` define um novo namespace XAML que é acessado por meio da prefixação de todos os usos com o token `myTypes:`.

Uma definição de **xmlns** inclui um valor e também a nomeação do prefixo. O valor é uma cadeia de caracteres colocada entre aspas, seguida de um sinal de igual. Uma convenção XML comum é associar o namespace XML a um URI (Uniform Resource Identifier), e há uma convenção de exclusividade e identificação. Você também vê essa convenção no namespace XAML padrão, no namespace XAML da linguagem XAML e em alguns namespaces XAML usados com menos frequência, que são utilizados pelo XAML do Tempo de Execução do Windows. Entretanto, para um namespace XAML que mapeia tipos personalizados, em vez de especificar um URI, comece a definição do prefixo com o token "using:". Em seguida ao token "using:", dê um nome ao namespace de código.

Por exemplo, para mapear um prefixo "custom1" que permite referenciar um namespace "CustomClasses" e usar classes desse namespace ou assembly como elementos de objeto no XAML, a página XAML deve incluir o seguinte mapeamento no elemento raiz:  `xmlns:custom1="using:CustomClasses"`

Classes parciais do mesmo escopo de página não precisam ser mapeadas. Por exemplo, prefixos não são necessários para referenciar qualquer manipulador de eventos definido para manipulação de eventos na definição da interface do usuário XAML da sua página. Além disso, muitas das páginas XAML iniciais de projetos gerados com o Visual Studio para um aplicativo do Windows Runtime em C++, C# ou Visual Basic já mapeiam um prefixo "local:", que menciona o namespace padrão especificado pelo projeto e o namespace usado pelas definições de classes parciais.

### <a name="clr-language-rules"></a>Regras da linguagem CLR

Se estiver escrevendo um código de suporte em uma linguagem do .NET (C# ou Microsoft Visual Basic), talvez esteja usando convenções que utilizam um ponto (".") como parte dos nomes de namespaces para criar uma hierarquia conceitual de namespaces de código. Se a definição de namespace contiver um ponto, o ponto deve fazer parte do valor especificado depois do token "using:".

Se o arquivo code-behind ou o arquivo de definição de código for um arquivo em C++, haverá certas convenções que ainda seguem a forma de linguagem CLR (Common Language Runtime) para que não haja qualquer diferença na sintaxe XAML. Se você declarar namespaces aninhados em C++, o separador entre as cadeias de caracteres de namespaces aninhados sucessivos deverá ser "." em vez de "::" quando você especificar o valor que vem depois do token "using:".

Não use tipos aninhados (como aninhar uma enumeração dentro de uma classe) ao definir o seu código para ser usado com XAML. Tipos aninhados não podem ser avaliados. Não é possível para o analisador XAML distinguir que um ponto faz parte do nome do tipo aninhado e não parte do nome do namespace.

## <a name="custom-types-and-assemblies"></a>Tipos personalizados e assemblies

O nome do assembly que define os tipos de suporte de um namespace XAML não é especificado no mapeamento. A lógica em que assemblies estão disponíveis é controlada no nível da definição do aplicativo e faz parte dos princípios básicos de desenvolvimento e segurança do aplicativo. Declare qualquer assembly a ser incluído como fonte de definição de código do XAML como um assembly dependente nas configurações do projeto. Para saber mais, veja [Criando componentes do Tempo de Execução do Windows em C# e Visual Basic](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx).

Se estiver referenciando tipos personalizados na definição do aplicativo principal ou nas definições de páginas, esses tipos serão disponibilizados sem outras configurações de assembly dependente, mas ainda será preciso mapear o namespace de código que contém esses tipos. Uma convenção comum é mapear o prefixo "local" para o namespace de código padrão de qualquer página XAML fornecida. Essa convenção é frequentemente incluída nos modelos de projeto iniciais para projetos XAML.

## <a name="attached-properties"></a>Propriedades anexadas

Se você estiver fazendo referência a propriedades anexadas, a parte do tipo de proprietário do nome da propriedade anexada deve estar no namespace XAML padrão ou ser prefixado. É raro prefixar atributos separadamente de seus elementos, mas. às vezes. é necessário para alguns casos, especialmente para uma propriedade personalizada anexada. Para saber mais, veja [Propriedades anexadas personalizadas](custom-attached-properties.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Visão geral do XAML](xaml-overview.md)
* [Guia de sintaxe do XAML](xaml-syntax-guide.md)
* [Criando componentes do Tempo de Execução do Windows em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [Modelos de projeto C#, VB e C++ para Tempo de Execução do Windows](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [Migrando do XAML/código de Silverlight ou WPF para um aplicativo do Tempo de Execução do Windows](https://msdn.microsoft.com/library/windows/apps/br229571)
 


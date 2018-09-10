---
author: jwmsft
description: Apresentamos a linguagem XAML e os conceitos de XAML para o público de desenvolvedores de aplicativos do Windows Runtime e descrevemos as diferentes maneiras de declarar objetos e definir atributos no XAML durante seu uso para criar um aplicativo do Windows Runtime.
title: Visão geral de XAML
ms.assetid: 48041B37-F1A8-44A4-BB8E-1D4DE30E7823
ms.author: jimwalk
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: d28f6bca5a0d849661f158e25767f27586ace101
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2018
ms.locfileid: "3820570"
---
# <a name="xaml-overview"></a>Visão geral do XAML

Apresentamos a linguagem XAML e os conceitos de XAML para o público de desenvolvedores de aplicativos do Windows Runtime e descrevemos as diferentes maneiras de declarar objetos e definir atributos no XAML durante seu uso para criar um aplicativo do Windows Runtime.

## <a name="what-is-xaml"></a>O que é XAML?

A linguagem XAML (Extensible Application Markup Language) é uma linguagem declarativa. A linguagem XAML pode, especificamente, iniciar objetos e definir as propriedades deles usando uma estrutura de linguagem que mostra relacionamentos hierárquicos entre vários objetos e uma convenção de tipo de suporte compatível com a extensão de tipos. Você pode criar elementos de interface do usuário visíveis na marcação XAML declarativa. Em seguida, você pode associar um arquivo code-behind separado a cada arquivo XAML capaz de responder a eventos e manipular os objetos declarados originalmente na linguagem XAML.

A linguagem XAML permite o intercâmbio de fontes entre ferramentas e funções diferentes no processo de desenvolvimento, como o intercâmbio de fontes XAML entre ferramentas de design e um IDE ou entre desenvolvedores primários e desenvolvedores de localização. Usando XAML como formato de intercâmbio, as funções de designer e desenvolvedor podem ser mantidas separadas ou juntas, e os designers e desenvolvedores podem iterar durante a produção de um aplicativo.

Quando você as vê como parte de seus projetos de aplicativo do Windows Runtime, os arquivos XAML são arquivos XML com a extensão de nome de arquivo .xaml.

## <a name="basic-xaml-syntax"></a>Sintaxe XAML básica

O XAML tem uma sintaxe básica compilada em XML. Por definição, o XAML válido também deve ser um XML válido. Mas o XAML também apresenta conceitos de sintaxe com um significado diferente e mais completo, apesar de ainda ser válido em XML de acordo com a especificação XML 1.0. Por exemplo, o XAML permite a *sintaxe de elemento de propriedade*, em que é possível definir valores de propriedade em elementos, em vez de valores de cadeia de caracteres em atributos ou conteúdo. Para o XML normal, um elemento de propriedade XAML é um elemento com um ponto em seu nome, portanto, ele é valido para XML simples, mas não tem o mesmo significado.

## <a name="xaml-and-microsoft-visual-studio"></a>XAML e Microsoft Visual Studio

O Microsoft Visual Studio ajuda você a produzir uma sintaxe XAML válida, seja no editor de texto XAML e na área de design XAML mais orientada graficamente. Portanto, quando você escreve XAML para seu aplicativo usando o Visual Studio, não se preocupe muito com a sintaxe em cada pressionamento de tecla. O IDE encoraja a sintaxe XAML válida através de dicas de preenchimento automático, sugestões em listas e menus suspensos do Microsoft IntelliSense, bibliotecas de elementos de interface do usuário na caixa de ferramentas ou outras técnicas. Se esta é sua primeira experiência com XAML, talvez seja útil conhecer as regras de sintaxe e, particularmente, a terminologia que às vezes é usada para descrever as restrições ou opções quando representamos a sintaxe XAML em tópicos de referência ou outros tópicos. Nós abordamos esses pontos de sintaxe XAML em um tópico separado, [Guia de sintaxe XAML](xaml-syntax-guide.md).

## <a name="xaml-namespaces"></a>Namespaces XAML

Em programação geral, um namespace é um conceito de organização que determina como identificadores para entidades de programação são interpretados. Com o uso de namespaces, uma estrutura de programação pode separar os identificadores declarados pelo usuário dos identificadores declarados pela estrutura, remover a ambiguidade de identificadores por meio das qualificações do namespace, impor regras para nomes de escopo, e assim por diante. O XAML tem seu próprio conceito de namespace XAML que atende a essa finalidade na linguagem XAML. Veja como o XAML aplica e estende os conceitos de namespace de linguagem XML:

-   O XAML usa o atributo XML reservado **xmlns** para declarações de namespace. O valor do atributo é geralmente um URI (Uniform Resource Identifier), que é uma convenção herdada do XML.
-   O XAML usa prefixos em declarações para declarar namespaces não padrão e faz uso de prefixos em elementos e atributos que referenciam esse namespace.
-   O XAML tem um conceito de namespace padrão, que é o namespace usado quando não há prefixo em um uso ou declaração. O namespace padrão pode ser definido de forma diferente para cada estrutura de programação XAML.
-   As definições de namespace são herdadas, em um arquivo ou construção XAML, do elemento pai para o elemento filho. Por exemplo, se você definir um namespace no elemento raiz de um arquivo XAML, todos os elementos dentro desse arquivo herdarão essa definição de namespace. Se um elemento adicional na página redefinir o namespace, os descendentes desse elemento herdarão a nova definição.
-   Os atributos de um elemento herdam os namespaces desse elemento. É muito incomum ver prefixos em atributos XAML.

Um arquivo XAML nem sempre declara um namespace XAML padrão no elemento raiz. O namespace XAML padrão define quais elementos podem ser declarados sem qualificação via prefixo. Para projetos típicos de aplicativo do Windows Runtime esse padrão do namespace contém todo o vocabulário XAML incorporado para o Windows Runtime que é usado para definições de interface do usuário: os controles padrão, elementos de texto, elementos gráficos e animações XAML, tipos de suporte a associação de dados e estilização, e assim por diante. A maior parte do XAML que você escreverá para aplicativos do Windows Runtime será capaz de evitar o uso de namespaces e prefixos XAML ao se referir a elementos comuns da interface do usuário.

Aqui está um trecho que mostra uma raiz [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) criada por um modelo da página inicial de um aplicativo (mostrando apenas a marca de abertura e simplificada). Ele declara o namespace padrão e também o namespace **x** (que explicaremos em seguida).

```xml
<Page
    x:Class="Application1.BlankPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
>
```

## <a name="the-xaml-language-xaml-namespace"></a>Namespace XAML da linguagem XAML

Um determinado namespace XAML que é declarado em praticamente todos os arquivos XAML do Tempo de Execução do Windows é o namespace XAML. Este namespace inclui elementos e conceitos que são definidos pela linguagem XAML, por sua especificação de linguagem. Por convenção, o namespace XAML da linguagem XAML é mapeado ao prefixo "x". O projeto padrão e os modelos de arquivo para projetos de aplicativos do Windows Runtime sempre definem o namespace XAML padrão (sem prefixo, apenas `xmlns=`) e o namespace XAML da linguagem XAML (prefixo "x") como parte do elemento raiz.

O prefixo "x" e o namespace XAML da linguagem XAML contêm vários construtores de programação que você sempre usa em seu XAML. Veja a seguir os mais comuns:

| Termo | Descrição |
|------|-------------|
| [x:Key](x-key-attribute.md) | Estabelece uma chave exclusiva, definida pelo usuário, para cada recurso em um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) XAML. A cadeia de tokens da chave é o argumento da extensão de marcação **StaticResource** e essa chave é usada posteriormente para recuperar o recurso XAML de outro uso do XAML em outro lugar do aplicativo XAML. |
| [x:Class](x-class-attribute.md) | Especifica o namespace de código e o nome da classe de código referente à classe que apresenta o code-behind de uma página XAML. Isso nomeia a classe que é criada ou unida por ações de criação ao criar o aplicativo. Essas ações criam suporte ao compilador de marcação XAML e combina a marcação e código subjacente quando o aplicativo é compilado. Você deve ter essa classe para suportar o código subjacente para uma página XAML. [**Window.Content**](https://msdn.microsoft.com/library/windows/apps/br209051) no modelo de ativação do Windows Runtime padrão. |
| [x:Name](x-name-attribute.md) | Especifica um nome de objeto em tempo de execução para a instância existente no código em tempo de execução após o processamento de um elemento de objeto definido em XAML. Você pode pensar em configurar o **x:Name** em XAML como declarar uma variável nomeada no código. Conforme você aprenderá mais tarde, isso é exatamente o que acontece quando o XAML é carregado como um componente de um aplicativo do Windows Runtime. <br/><div class="alert">**Observação**  [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) é uma propriedade semelhante na estrutura, mas nem todos os elementos a suportam. Você pode usar **x:Name** para identificação de elemento, sempre que o **FrameworkElement.Name** não for suportado nesse tipo de elemento. |
| [x:Uid](x-uid-directive.md) | Identifica elementos que devem usar recursos localizados para alguns dos valores de propriedade. Para saber mais sobre como usar **x:Uid**, veja o [Guia de início rápido: traduzindo recursos de interface de usuário](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329). |
| [Tipos de dados XAML intrínsecos](xaml-intrinsic-data-types.md) | Esses tipos podem especificar valores para tipos de valores simples, quando isso for necessário para um atributo ou recurso. Esses tipos intrínsecos correspondem aos tipos de valores simples que são tipicamente definidos como parte de definições intrínsecas de cada linguagem de programação. Por exemplo, você pode precisar de um objeto representando um valor booliano **true** para usar em um estado visual com storyboard [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320). Para esse valor em XAML, você usaria o tipo intrínseco **x:Boolean** como o elemento de objeto, como este: <code>&lt;x:Boolean&gt;True&lt;/x:Boolean&gt;</code> | 

Existem outros construtores de programação no namespace XAML da linguagem XAML, mas eles não são tão comuns.

## <a name="mapping-custom-types-to-xaml-namespaces"></a>Mapeando tipos personalizados para namespaces XAML

Um dos aspectos mais importantes do XAML como linguagem é que é fácil de estender o vocabulário XAML para os aplicativos do Tempo de Execução do Windows. Você pode definir seus próprios tipos personalizados em linguagem de programação do aplicativo e, em seguida, fazer referência aos tipos personalizados na marcação XAML. O suporte a extensão através de tipos personalizados está fundamentalmente ligado à forma que a linguagem XAML trabalha. As estruturas ou os desenvolvedores de aplicativos são responsáveis​por criar os objetos de backup a que o XAML faz referência. Nem as estruturas nem o desenvolvedor do aplicativo estão vinculados pelas especificações do que os objetos em seus vocabulários representam ou fazem além das regras básicas de sintaxe XAML (há algumas expectativas do que os tipos de namespace XAML de linguagem XAML devem fazer, mas o Windows Runtime fornece todo o suporte necessário).

Se você usa XAML para tipos que vêm de bibliotecas que não sejam as bibliotecas básicas do Windows Runtime e metadados, você deve declarar e mapear um namespace XAML com um prefixo. Use esse prefixo em utilizações do elemento para referenciar os tipos que foram definidos na biblioteca. Você declara mapeamentos de prefixo como atributos **xmlns** geralmente em um elemento raiz junto com outras definições de namespace XAML.

Para criar sua própria definição de namespace que referencia tipos personalizados, você especifica primeiro a palavra-chave **xmlns:**, em seguida, o prefixo que deseja. O valor desse atributo tem de incluir a palavra-chave **using:** como a primeira parte do valor. O restante do valor é um token de cadeia de caracteres que menciona pelo nome o namespace de suporte de código específico que contém seus tipos personalizados, por nome.

O prefixo define o token de marcação usado para mencionar esse namespace XAML no restante da marcação nesse arquivo XAML. Um caractere de dois-pontos (:) é colocado entre o prefixo e a entidade a ser mencionada no namespace XAML.

Por exemplo, a sintaxe do atributo para mapear o prefixo `myTypes` ao namespace `myCompany.myTypes` é: `    xmlns:myTypes="using:myCompany.myTypes"`, e o uso do elemento representante é: `<myTypes:CustomButton/>`

Para saber mais sobre namespaces XAML de mapeamento para tipos personalizados, incluindo considerações especiais para as extensões do componente Visual C++ (C++/CX), consulte [mapeamento de namespaces e namespaces XAML](xaml-namespaces-and-namespace-mapping.md).

## <a name="other-xaml-namespaces"></a>Outros namespaces XAML

É bastante frequente ver arquivos XAML que definem o prefixo "d" (para o namespace de designer) e "mc" (para compatibilidade de marcação). Geralmente, isso serve para suporte de infraestrutura ou para permitir cenários em uma ferramenta de tempo de design. Para obter mais informações, consulte a [seção "Outros namespaces XAML" do tópico Namespaces XAML](xaml-namespaces-and-namespace-mapping.md#other-XAML-namespaces).

## <a name="markup-extensions"></a>Extensões de marcação

As extensões de marcação constituem um conceito de linguagem XAML geralmente usado na implementação de XAML do Windows Runtime. As extensões de marcação frequentemente representam algum tipo de "atalho" que permite que um arquivo XAML acesse um valor ou comportamento que não está simplesmente declarando elementos com base em tipos de suporte. Algumas extensões de marcação podem definir propriedades com cadeia de caracteres sem formatação ou com elementos aninhados adicionalmente, com o objetivo de simplificar a sintaxe ou a fatoração entre diferentes arquivos XAML.

Na sintaxe de atributo XAML, as chaves "{" e "}" indicam o uso de uma extensão de marcação XAML. Esse uso direciona o processamento de XAML para que ele saia do tratamento geral de tratamento de valores de atributos como uma cadeia de caracteres literal ou um valor que possa ser diretamente convertido em cadeia de caracteres. Em vez disso, um analisador XAML chama o código que oferece comportamento para essa extensão de marcação específica e esse código gera um resultado de objeto ou comportamento alternativo de que o analisador XAML precisa. As extensões de marcação podem ter argumentos que seguem o nome de extensão de marcação e também são colocados entre chaves. Geralmente, uma extensão de marcação avaliada fornece um valor de retorno do objeto. Durante a análise, esse valor de retorno é posicionado na árvore de objetos onde o uso de extensão de marcação estava no XAML de origem.

O XAML do Windows Runtime permite essas extensões de marcação, que são definidas no namespace XAML padrão e interpretadas pelo analisador XAML do Windows Runtime:

-   [{xBind}](x-bind-markup-extension.md): permite a vinculação de dados, que adia a avaliação de propriedade até o tempo de execução, executando código de finalidade especial, que ele gera no momento da compilação. Essa extensão de marcação permite uma ampla variedade de argumentos.
-   [{Binding}](binding-markup-extension.md): permite a vinculação de dados, que adia a avaliação de propriedade até o tempo de execução, executando a inspeção de objeto em tempo de execução de finalidade geral. Essa extensão de marcação permite uma ampla variedade de argumentos.
-   [{StaticResource}](staticresource-markup-extension.md): dá suporte a valores de recurso de referência que são definidos em um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794). Esses recursos podem estar em um arquivo XAML diferente, mas devem ser localizáveis em última análise pelo analisador XAML em tempo de carregamento. O argumento do uso de um `{StaticResource}` identifica a chave (o nome) de um recurso de chave em um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
-   [{ThemeResource}](themeresource-markup-extension.md): similar a [{StaticResource}](staticresource-markup-extension.md), mas pode responder a alterações no tema em tempo de execução. O {ThemeResource} aparece com bastante frequência nos modelos XAML padrão do Tempo de Execução do Windows, porque a maioria desses modelos são projetados para ter compatibilidade com a mudança de tema feita pelo usuário, enquanto o aplicativo estiver sendo executado.
-   [{TemplateBinding}](templatebinding-markup-extension.md): um caso especial de [{Binding}](binding-markup-extension.md) que permite modelos de controle em XAML e seu eventual uso em tempo de execução.
-   [{RelativeSource}](relativesource-markup-extension.md): permite uma forma específica de associação de modelo onde os valores vêm do pai de modelo.
-   [{CustomResource}](customresource-markup-extension.md): para cenários avançados de pesquisa de recursos.

O Windows Runtime também permite a extensão de marcação [{x:Null}](x-null-markup-extension.md). Use-a para definir valores de [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx) como **null** em XAML. Por exemplo, você pode usá-la em um modelo de controle para uma [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316), que interpreta **null** como um estado de verificação indeterminado (disparando o estado visual "Indeterminado").

As extensões de marcação geralmente retornam uma instância existente de alguma outra parte do gráfico de objeto do aplicativo ou adiam um valor para o tempo de execução Como é possível usar uma extensão de marcação como um valor de atributo, que é o uso comum, você sempre vê extensões de marcação passando valores para propriedades do tipo de referência que possam ter solicitado de outra forma uma sintaxe de elemento de propriedade.

Por exemplo, veja a sintaxe para fazer referência a um [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) reutilizável de um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794): `<Button Style="{StaticResource SearchButtonStyle}"/>`. [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) é um tipo de referência, não um valor simples; assim, sem o uso de `{StaticResource}`, talvez fosse necessário um elemento de propriedade `<Button.Style>` e uma definição de `<Style>` incluída para definir a propriedade [**FrameworkElement.Style**](https://msdn.microsoft.com/library/windows/apps/br208743).

Usando extensões de marcação, cada propriedade configurável em XAML é potencialmente configurável na sintaxe do atributo. Você pode usar a sintaxe de atributo para fornecer valores de referência para uma propriedade, mesmo que ela não permita uma sintaxe de atributo para instanciação direta de objetos. Você também pode habilitar um comportamento específico que adie o requisito geral de que as propriedades XAML têm que ser preenchidas por tipos de valor ou por tipos de referência recém-criados.

Para ilustrar, o exemplo XAML a seguir define o valor da propriedade [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) de uma [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) usando sintaxe de atributos. A propriedade [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) obtém uma instância da classe [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849), um tipo de referência que, por padrão, não pôde ser criado usando uma cadeia de sintaxe de atributos. Mas, neste caso, o atributo faz referência a uma extensão de marcação específica, [StaticResource](staticresource-markup-extension.md). Quando essa extensão de marcação é processada, ela retorna uma referência para um elemento **Style** definido anteriormente como um recurso inserido em um dicionário de recursos.

```xml
<Canvas.Resources>
  <Style TargetType="Border" x:Key="PageBackground">
    <Setter Property="BorderBrush" Value="Blue"/>
    <Setter Property="BorderThickness" Value="5"/>
  </Style>
</Canvas.Resources>
...
<Border Style="{StaticResource PageBackground}">
  ...
</Border>
```

Você pode aninhar extensões de marcação. A extensão de marcação mais interna é avaliada primeiro.

Devido às extensões de marcação, você precisa de uma sintaxe especial para um valor "{" literal em um atributo. Para obter mais informações, consulte [Guia de sintaxe XAML](xaml-syntax-guide.md).

## <a name="events"></a>Eventos

XAML é uma linguagem declarativa para objetos e suas propriedades, mas também inclui uma sintaxe para anexar manipuladores de eventos a objetos na marcação. A sintaxe de eventos XAML pode integrar eventos declarados por XAML por meio do modelo de programação do Windows Runtime. Especifique o nome do evento como um nome de atributo do objeto onde o evento é manipulado. Para o valor de atributo, especifique o nome da função do manipulador de evento definido no código. O processador XAML usa esse nome para criar uma representação delegada na árvore de objetos carregada e adiciona o manipulador especificado a uma lista de manipuladores internos. Quase todos os aplicativos do Windows Runtime são definidos por fontes de marcação e code-behind.

A seguir está um exemplo simples. A classe [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) dá suporte a um evento chamado [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737). Você pode gravar um manipulador para **Click** que executa o código que deve ser invocado depois que o usuário clica em **Button**. No XAML, especifique **Click** como um atributo em **Button**. Para o valor do atributo, forneça uma cadeia de caracteres que seja o nome do método do manipulador.

```xml
<Button Click="showUpdatesButton-Click">Show updates</Button>
```

Quando você compila, o compilador agora espera que haverá um método chamado `showUpdatesButton-Click` definido no arquivo code-behind, no namespace declarado no valor [x:Class](x-class-attribute.md) da página XAML. Além disso, esse método deve satisfazer o contrato representante para o evento [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737). Por exemplo:

```csharp
namespace App1
{
    public sealed partial class MainPage: Page {
        ...
        private void showUpdatesButton_Click (object sender, RoutedEventArgs e) {
            //your code
        }
    }
}
```

```vb
' Namespace included at project level
Public NotInheritable Class MainPage
    Inherits Page
        ...
        Private Sub showUpdatesButton_Click (sender As Object, e As RoutedEventArgs e)
            ' your code
        End Sub
    ...
End Class
```

```cppwinrt
namespace winrt::App1::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        ...
        void showUpdatesButton_Click(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);
    };
}
```

```cpp
// .h
namespace App1
{
    public ref class MainPage sealed {
        ...
    private:
        void showUpdatesButton_Click(Object^ sender, RoutedEventArgs^ e);
    };
}
```

Em um projeto, a linguagem XAML é escrita como um arquivo .xaml, e você usa a linguagem preferida (C#, Visual Basic, C++/CX) para escrever um arquivo code-behind. Quando um arquivo XAML é compilado por marcação como parte de uma ação de compilação do projeto, o local do arquivo code-behind XAML de cada página XAML é identificado por meio da especificação de um namespace e de uma classe como o atributo [x:Class](x-class-attribute.md) do elemento raiz da página XAML. Para saber mais sobre como esses mecanismos funcionam em XAML e como estão relacionados aos modelos de programação e aplicativo, consulte [Visão geral de eventos e eventos encaminhados](events-and-routed-events-overview.md).

**Observação**  Para C++/CX há dois arquivos code-behind, um deles é um cabeçalho(.xaml.h) e o outro é implementação (.xaml.cpp). A implementação faz referência ao cabeçalho e tecnicamente é o cabeçalho que representa o ponto de entrada para a conexão code-behind.

## <a name="resource-dictionaries"></a>Dicionários de recursos

A criação de um [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) é uma tarefa comum geralmente realizada por meio da criação de um dicionário de recursos como a área de uma página XAML ou um arquivo XAML separado. Os dicionários de recursos e como usá-los é um assunto mais amplo que está fora do escopo deste tópico. Para saber mais, veja [Referências de recursos de ResourceDictionary e XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).

## <a name="xaml-and-xml"></a>XAML e XML

A linguagem XAML é fundamentalmente baseada na linguagem XML. Porém, a XAML amplia a XML de forma considerável. Em particular, ela trata do conceito de esquema de forma bem diferente (em função de seu relacionamento com o conceito de tipo de suporte) e adiciona elementos de linguagem como membros anexados e extensões de marcação. **xml:lang** é válido em XAML, mas influencia o tempo de execução (e não o comportamento de análise) e normalmente tem um alias para uma propriedade em nível de estrutura. Para saber mais, veja [**FrameworkElement.Language**](https://msdn.microsoft.com/library/windows/apps/hh702066). **xml:base** é válido em marcação, mas os analisadores o ignoram. **xml:space** é válido, mas só é importante para cenários descritos no tópico [XAML e espaços em branco](xaml-and-whitespace.md). O atributo **encoding** é válido em XAML. Somente as codificações UTF-8 e UTF-16 são compatíveis. A codificação UTF-32 não tem suporte.

###  <a name="case-sensitivity-in-xaml"></a>Diferenciação de maiúsculas e minúsculas em XAML

XAML diferencia maiúsculas de minúsculas. Essa é outra consequência de XAML ter como base a linguagem XML, que faz essa diferenciação. Os nomes dos elementos e atributos XAML diferenciam maiúsculas de minúsculas. Potencialmente, o valor de um atributo faz essa diferenciação. Isso depende de como o valor do atributo é manipulado para propriedades específicas. Por exemplo, se o valor de atributo declarar o nome de um membro de uma enumeração, o comportamento interno que realiza a conversão por tipo de uma cadeia de nome de membro a fim de gerar o valor do membro de enumeração não fará diferenciação entre maiúsculas e minúsculas. Em contraste, o valor da propriedade **Name** e os métodos utilitários para trabalhar com objetos baseados no nome declarado pela propriedade **Name** consideram que a cadeia de nome diferencia maiúsculas de minúsculas.

## <a name="xaml-namescopes"></a>Namescopes XAML

A linguagem XAML define o conceito de namescope XAML. Esse conceito influencia o modo como os processadores XAML devem tratar o valor de **x:Name** ou **Name** aplicado a elementos XAML, particularmente os escopos nos quais os nomes devem ser confiáveis para serem identificadores exclusivos. Os namescopes XAML são abordados com mais detalhes em um tópico separado; veja [namescopes XAML](xaml-namescopes.md).

## <a name="the-role-of-xaml-in-the-development-process"></a>A função de XAML no processo de desenvolvimento

O XAML desempenha vários papéis importantes no processo de desenvolvimento de aplicativos.

-   O XAML é o principal formato para declaração da interface do usuário e respectivos elementos de um aplicativo, caso você esteja programando em C#, Visual Basic ou C++/CX. Em geral, pelo menos um arquivo XAML do projeto representa uma metáfora de página no aplicativo, para a interface do usuário inicialmente exibida. Arquivos XAML adicionais podem declarar páginas adicionais para a interface do usuário de navegação. Outros arquivos XAML podem declarar recursos, como modelos ou estilos.
-   O formato XAML é usado para declaração dos estilos e modelos aplicados aos controles e à interface do usuário de um aplicativo.
-   Você pode usar estilos e modelos para modelagem de controles existentes ou se definir um controle que forneça um modelo padrão como parte de um pacote de controles. Ao usar o formato para definir estilos e modelos, o XAML relevante é geralmente declarado como um arquivo XAML discreto com uma raiz [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
-   O XAML é o formato comum de suporte ao designer para a criação da interface do usuário do aplicativo e para a troca do design da interface do usuário entre os vários aplicativos e designer. Em especial, o XAML para o aplicativo pode ser intercambiado entre as diferentes ferramentas de design XAML (ou janelas de design nas ferramentas).
-   Várias outras tecnologias também definem a interface do usuário básica no XAML. Em relação ao XAML do WPF (Windows Presentation Foundation) e ao XAML do Microsoft Silverlight, o XAML de um aplicativo do Windows Runtime usa o mesmo URI para o namespace XAML padrão compartilhado. O vocabulário XAML para um aplicativo do Windows Runtime se sobrepõe consideravelmente ao vocabulário XAML para interface do usuário também usado pelo Silverlight e, ainda que um pouco menos, pelo WPF. Dessa forma, o XAML promove uma via de migração eficiente para a interface do usuário originalmente definida por tecnologias precursoras, que também usavam XAML.
-   O XAML define a aparência de uma interface do usuário e um arquivo code-behind associado define a lógica. É possível ajustar o design da interface do usuário sem fazer alterações na lógica do code-behind. O XAML simplifica o fluxo de trabalho entre designers e desenvolvedores.
-   Devido à riqueza do designer visual e do suporte de superfície de design para a linguagem XAML, o XAML dá suporte à prototipagem rápida da interface do usuário nas fases iniciais de desenvolvimento.

Dependendo da sua própria função no processo de desenvolvimento, você pode não interagir muito com o XAML. Seu grau de interação com arquivos XAML também depende do ambiente de desenvolvimento em uso, se você usa recursos interativos de ambiente de design (como caixas de ferramentas e editores de propriedades), do escopo e da finalidade do aplicativo do Tempo de Execução do Windows. No entanto, é provável que, durante o desenvolvimento do aplicativo, você modifique um arquivo XAML no nível do elemento usando um editor de texto ou de XML. Com essas informações, você pode modificar com segurança o XAML em uma representação de texto ou XML e manter a validade das declarações e a finalidade desse arquivo XAML quando ele for consumido por ferramentas, operações de compilação de marcação ou na fase de tempo de execução do aplicativo do Tempo de Execução do Windows.

## <a name="optimize-your-xaml-for-load-performance"></a>Otimizar o XAML para desempenho de carga

A seguir estão algumas dicas para definir elementos da interface do usuário no XAML usando práticas recomendadas de desempenho. Muitas dessas dicas se relacionam ao uso dos recursos XAML, mas elas estão listadas aqui na Visão geral do XAML para fins de conveniência. Para saber mais sobre os recursos XAML, consulte [Referências de recursos de ResourceDictionary e XAML](https://msdn.microsoft.com/library/windows/apps/mt187273). Para obter mais algumas dicas sobre desempenho, incluindo XAML que demonstra de propósito algumas das más práticas de desempenho que você deve evitar em seu XAML, consulte [Otimizar carregamento de XAML](https://msdn.microsoft.com/library/windows/apps/mt204779).

-   Se você usar o mesmo pincel de cores com frequência no XAML, defina [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) como um recurso, em vez de sempre usar uma cor nomeada como um valor de atributo.
-   Se você usar o mesmo recurso em mais de uma página da interface do usuário, avalie defini-lo em [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338), em vez de em cada página. Em contrapartida, se uma única página usar um recurso, não o defina em **Application.Resources** mas sim somente para a página que precisa dele. Isso é bom para faturamento de XAML enquanto você cria seu aplicativo e para o desempenho durante a análise do XAML.
-   Para recursos que o seu aplicativo empacota, verifique se há recursos não utilizados (um recurso que tem uma chave, mas sem nenhuma referência de [StaticResource](staticresource-markup-extension.md) no aplicativo que o use). Remova-os de seu XAML antes de lançar seu aplicativo.
-   Se você estiver usando arquivos XAML separados que forneçam recursos de design ([**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801)), avalie se desabilitará ou removerá os recursos não utilizados desses arquivos. Mesmo que você tenha um ponto inicial de XAML compartilhado que esteja usando em mais de um aplicativo ou que forneça recursos comuns para todos os seus aplicativos, ainda é seu aplicativo que empacota os recursos XAML a cada vez, e potencialmente precisa carregá-los.
-   Não defina elementos da interface do usuário de que não precisa para composição, e use os modelos de controle padrão sempre que possível (esses modelos já foram testados e verificados em termos de desempenho de carga).
-   Use contêineres como [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250), em vez de retiradas deliberas de elementos da interface do usuário. Basicamente, não desenhe o mesmo pixel várias vezes. Para saber mais sobre exceder e como testá-lo, veja [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/hh701823).
-   Use os modelos de itens padrão para [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) ou [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705); esses tem lógica especial **Presenter** que resolve problemas de desempenho ao criar a árvore visual para um grande número de itens da lista.

## <a name="debugging-xaml"></a>Depurando o XAML

Como o XAML é uma linguagem de marcação, algumas das estratégias típicas para a depuração no Microsoft Visual Studio não estão disponíveis. Por exemplo, não há nenhuma maneira de definir um ponto de interrupção em um arquivo XAML. No entanto, existem outras técnicas que podem ajudar a depurar problemas de definições de interface do usuário ou de outra marcação XAML enquanto você ainda está desenvolvendo seu aplicativo.

Quando há problemas com um arquivo XAML, o resultado mais típico é que algum sistema ou o seu aplicativo acione uma exceção de análise XAML. Sempre que há uma exceção de análise XAML, o XAML carregado pelo analisador XAML não conseguiu criar uma árvore de objeto válida. Em alguns casos, como quando o XAML representa a primeira "página" de seu aplicativo que é carregada como a raiz visual, a exceção de análise XAML não é recuperável.

O XAML é frequentemente editado dentro de um IDE, como o Visual Studio e uma de suas superfícies de design de XAML. O Visual Studio muitas vezes pode fornecer validação e verificação de erros em tempo de projeto de um código fonte XAML enquanto você o edita. Por exemplo, ele pode exibir "rabiscos" no editor de texto XAML assim que você inserir um valor de atributo inválido, e você nem terá que esperar por uma passagem de compilação de XAML para ver que há algo errado com sua definição de interface do usuário.

Depois que o aplicativo for executado, se houver erros de análise de XAML não detectados no tempo de projeto, eles serão relatados pelo CLR (Common Language Runtime) como um [**XamlParseException**](https://msdn.microsoft.com/library/windows/apps/hh673774). Para saber mais sobre o que você pode fazer para um tempo de execução **XamlParseException**, veja [Manipulação de exceções para aplicativos do Windows Runtime do Windows em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/dn532194).

**Observação**  Os aplicativos que usam C++/CX para o código não recebem [**XamlParseException**](https://msdn.microsoft.com/library/windows/apps/hh673774) específica. Mas a mensagem na exceção esclarece que a origem do erro está relacionada ao XAML, e inclui informações de contexto, como números de linha em um arquivo XAML, assim como o **XamlParseException** faz.

Para saber sobre a depuração de um aplicativo do Windows Runtime, veja [Iniciar uma sessão de depuração](https://msdn.microsoft.com/library/windows/apps/xaml/hh781607.aspx).
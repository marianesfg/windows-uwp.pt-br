---
title: Portar a amostra de Clipboard de C# para C++/WinRT (um estudo de caso)
description: Este tópico apresenta um estudo de caso de portabilidade de um [exemplo de aplicativo UWP (Plataforma Universal do Windows)](https://github.com/microsoft/Windows-universal-samples) de [C#](/visualstudio/get-started/csharp) para [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
ms.date: 03/20/2020
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, portabilidade, migrar, C#, exemplo, área de transferência, caso, estudo
ms.localizationpriority: medium
ms.openlocfilehash: 570f3538bf15616a45a17cdbce9a56066c8036bc
ms.sourcegitcommit: 23c5d8dfaeb6edbca780637ffd26fe892db27519
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123642"
---
# <a name="porting-the-clipboard-sample-tocwinrtfromcmdasha-case-study"></a>Portar a amostra de Clipboard de C# para C++/WinRT &mdash; um estudo de caso

Este tópico apresenta um estudo de caso de portabilidade de um [exemplo de aplicativo UWP (Plataforma Universal do Windows)](https://github.com/microsoft/Windows-universal-samples) de [C#](/visualstudio/get-started/csharp) para [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Você pode adquirir prática e experiência em portabilidade seguindo o passo a passo e fazendo a portabilidade da amostra para si mesmo à medida que avança.

Confira também [Mover do C# para C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp), que apresenta várias seções com os detalhes técnicos específicos envolvidos na portabilidade do C# para C++/WinRT.

## <a name="download-and-test-the-clipboard-sample"></a>Baixar e testar a amostra de Clipboard

Visite a página da Web [Amostra de Clipboard](https://docs.microsoft.com/samples/microsoft/windows-universal-samples/clipboard/) e clique em **Baixar ZIP**. Descompacte o arquivo baixado e verifique a estrutura da pasta.

- A versão C# do código-fonte da amostra está contida na pasta denominada `cs`. Outros arquivos usados pela versão C# podem ser encontrados nas pastas `shared` e `SharedContent`.
- Você pode encontrar a versão C++/WinRT do código-fonte da amostra na pasta [cppwinrt](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard/cppwinrt) no repositório do GitHub.

O passo a passo neste tópico mostra como recriar a versão C++/WinRT, portando-a do código-fonte C#. Dessa forma, você pode ver como portar seus próprios projetos de C# para C++/WinRT.

Para ter uma ideia do que a amostra faz, abra a solução C# (`\Clipboard_sample\cs\Clipboard.sln`), altere a configuração conforme apropriado (talvez para *x64*), compile e execute. A interface do usuário da amostra fornece orientações sobre os vários recursos, passo a passo.

## <a name="createablankappcwinrt-named-clipboard"></a>Criar um aplicativo em branco (C++/WinRT) denominado Clipboard

> [!NOTE]
> Para saber mais sobre a instalação e o uso da Extensão do Visual Studio (VSIX) para C++/WinRT e do pacote NuGet (que, juntos, fornecem um modelo de projeto e suporte ao build), confira  [Suporte do Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Inicie o processo de portabilidade criando um novo projeto C++/WinRT no Microsoft Visual Studio. Crie um novo projeto usando o modelo de projeto **Aplicativo em Branco (C++/WinRT)** . Defina o nome como *Clipboard* e (para que a estrutura de pastas corresponda ao passo a passo) verifique se a opção **Posicionar a solução e o projeto no mesmo diretório** está desmarcada.

Apenas para ter uma linha de base, verifique se o projeto novo e vazio é compilado e executado.

## <a name="packageappxmanifest-and-asset-files"></a>Package.appxmanifest e arquivos de ativos

Se as versões C# e C++/WinRT do exemplo não precisarem ser instaladas lado a lado no mesmo computador, os arquivos de origem do manifesto do pacote do aplicativo dos dois projetos (`Package.appxmanifest`) poderão ser idênticos. Nesse caso, basta copiar o `Package.appxmanifest` do projeto C# para o projeto C++/WinRT e pronto.

Para que as duas versões do exemplo coexistam, elas precisam de identificadores diferentes. Nesse caso, no projeto C++/WinRT, abra o arquivo `Package.appxmanifest` em um editor XML e anote estes três valores.

- Dentro do elemento **/Package/Identity**, observe o valor do atributo **Name**. Esse é o *nome do pacote*. Um projeto recém-criado fornecerá como valor inicial um GUID (identificador global exclusivo).
- Dentro do elemento **/Package/Applications/Application**, observe o valor do atributo **Id**. Essa é a *ID do aplicativo*.
- Dentro do elemento **/Package/mp:PhoneIdentity**, observe o valor do atributo **PhoneProductId**. Novamente, em um projeto recém-criado, isso será definido com o mesmo GUID que o nome do pacote.

Em seguida, copie `Package.appxmanifest` do projeto C# para o projeto C++/WinRT. Por fim, você pode restaurar os três valores que anotou. Ou pode editar os valores copiados para torná-los exclusivos e/ou adequados ao aplicativo e a sua organização (como faria normalmente em um novo projeto). Por exemplo, nesse caso, em vez de restaurar o valor do nome do pacote, podemos apenas alterar o valor copiado de *Microsoft.SDKSamples.Clipboard.CS* para *Microsoft.SDKSamples.Clipboard.CppWinRT*. E podemos deixar a ID do aplicativo definida como *App*. Desde que o nome do pacote *ou* a ID do aplicativo sejam diferentes, os dois aplicativos terão AUMIDs (IDs de modelo de usuário de aplicativo) diferentes.

A fim de atender aos objetivos deste passo a passo, faremos algumas outras alterações em `Package.appxmanifest`. Há três ocorrências da cadeia de caracteres *Clipboard C# Sample*. Altere para *Clipboard C++/WinRT Sample*.

No projeto C++/WinRT, o arquivo `Package.appxmanifest` e o projeto agora estão fora de sincronia com relação aos arquivos de ativos aos quais eles fazem referência. Para remediar isso, primeiro remova os ativos do projeto C++/WinRT selecionando todos os arquivos na pasta `Assets` (no Gerenciador de Soluções no Visual Studio) e removendo-os (escolha **Excluir** na caixa de diálogo).

O projeto C# faz referência a arquivos de ativos de uma pasta compartilhada. Você pode fazer o mesmo no projeto C++/WinRT ou copiar os arquivos, como faremos neste passo a passo.

Navegue para a pasta `\Clipboard_sample\SharedContent\media`. Selecione os sete arquivos incluídos no projeto C# (`microsoft-sdk.png` a`windows-sdk.png`), copie-os e cole-os na pasta `\Clipboard\Clipboard\Assets` no novo projeto.

Clique com o botão direito do mouse na pasta `Assets` (no Gerenciador de Soluções no projeto C++/WinRT) > **Adicionar** > **Item existente...* e navegue até `\Clipboard\Clipboard\Assets`. No seletor de arquivos, selecione os sete arquivos e clique em **Adicionar**.

O `Package.appxmanifest` agora está em sincronia novamente com os arquivos de ativos do projeto.

## <a name="mainpage-including-the-sample-configuration-functionality"></a>**MainPage**, incluindo a funcionalidade de configuração de amostra

A amostra de Clipboard &mdash; como todas as [ amostras de aplicativos UWP (Plataforma Universal do Windows) ](https://github.com/microsoft/Windows-universal-samples)&mdash; consiste em uma coleção de cenários que o usuário pode percorrer um a um. A coleção de cenários em determinada amostra é configurada no código-fonte da amostra. Cada cenário na coleção é um item de dados que armazena um título, assim como o tipo de classe do projeto que implementa o cenário.

Na versão C# da amostra, se você observar o arquivo de código-fonte `SampleConfiguration.cs`, verá duas classes. A maior parte da lógica de configuração está na classe **MainPage**, que é uma classe parcial (forma uma classe completa quando combinada com a marcação no `MainPage.xaml` e o código imperativo no `MainPage.xaml.cs`). A outra classe nesse arquivo de código-fonte é **Scenario**, com suas propriedades **Title** e **ClassType**.

Nas próximas subseções, veremos como portar **MainPage** e **Scenario**.

### <a name="idl-for-the-mainpage-type"></a>IDL para o tipo **MainPage**

Os arquivos de código-fonte C# que implementam juntos o tipo **MainPage** são: `MainPage.xaml` (que será portado em breve, copiando-o), `MainPage.xaml.cs` e `SampleConfiguration.cs`.

Na versão C++/WinRT, fatoraremos nosso tipo **MainPage** em arquivos de código-fonte de forma semelhante. Pegaremos a lógica em `MainPage.xaml.cs` e converteremos a maior parte para `MainPage.h` e `MainPage.cpp`. E, quanto à lógica em `SampleConfiguration.cs`, converteremos para `SampleConfiguration.h` e `SampleConfiguration.cpp`.

As classes em um aplicativo UWP (Plataforma Universal do Windows) C# são tipos do Windows Runtime. Mas quando você cria um tipo em um aplicativo C++/WinRT, pode escolher entre o tipo do Windows Runtime ou uma classe/estrutura/enumeração regular.

No projeto C++/WinRT, **MainPage** já é um tipo do Windows Runtime, portanto, não precisamos alterar esse aspecto. Especificamente, é uma *classe de runtime*.

- Para obter mais detalhes sobre a criação ou não de uma classe de runtime, confira o tópico [Criar APIs com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).
- Os conceitos de *tipo de implementação* e *tipo projetado* são importantes no C++/WinRT. Você pode aprender sobre eles no tópico mencionado acima e em [Criar APIs com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-apis).
- Para saber mais sobre a conexão entre classes de runtime e arquivos `.idl`, leia e acompanhe o tópico [Controles XAML; associar a uma propriedade de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property). Esse tópico aborda o processo de criação de uma classe de runtime, cuja primeira etapa é adicionar um novo item **Midl File (.idl)** ao projeto.

Para **MainPage**, já temos o arquivo `MainPage.idl` necessário no projeto C++/WinRT. Mas, durante este passo a passo, adicionaremos *novos* arquivos `.idl` ao projeto.

Em breve, veremos uma lista exata de qual IDL precisamos adicionar ao arquivo `MainPage.idl` existente. Antes disso, vamos discutir o que precisa e o que não precisa entrar na IDL.

Para determinar quais membros de **MainPage** precisamos declarar no `MainPage.idl` (para que eles se tornem parte da classe de runtime **MainPage**) e quais podem ser simplesmente membros do tipo de implementação **MainPage**, vamos fazer uma lista dos membros da classe **MainPage** em C#. Busque os membros nos arquivos `MainPage.xaml.cs` e `SampleConfiguration.cs`.

Constatamos um total de doze campos e métodos `protected` e `private`. E encontramos os membros `public` a seguir.

- O construtor padrão `MainPage()`.
- Os campos estáticos **Current** e **FEATURE_NAME**.
- As propriedades **IsClipboardContentChangedEnabled** e **Scenarios**.
- Os métodos **BuildClipboardFormatsOutputString**, **DisplayToast**, **EnableClipboardContentChangedNotifications** e **NotifyUser**.

São esses membros `public` que são candidatos a declaração em `MainPage.idl`. Vamos examinar cada um e ver se precisam fazer parte da classe de runtime **MainPage** ou se precisam apenas fazer parte da implementação.

- O construtor padrão `MainPage()`. Para uma **Page** XAML, é normal declarar um construtor padrão em sua IDL. Dessa forma, a estrutura da IU XAML pode ativar o tipo.
- O campo estático **Current** é usado nas páginas XAML do cenário individual para acessar a instância de **MainPage** do aplicativo. Como o **Current** não está sendo usado para interoperar com a estrutura XAML (nem em unidades de compilação), podemos reservá-lo para que seja somente um membro do tipo de implementação. Em casos como esse, nos seus próprios projetos, você pode fazer isso. Porém, como o campo é uma instância do tipo projetado, parece lógico declará-lo na IDL. Portanto, é isso que faremos aqui (e isso também torna o código um pouco mais limpo).
- O caso é semelhante para o campo **FEATURE_NAME** estático, que é acessado no tipo **MainPage**. Mais uma vez, optar por declará-lo na IDL torna nosso código um pouco mais limpo.
- A propriedade **IsClipboardContentChangedEnabled** é usada apenas na classe **OtherScenarios**. Portanto, durante a portabilidade, simplificaremos um pouco e a transformaremos em um campo privado da classe de runtime **OtherScenarios**. Assim, ela não irá para a IDL.
- A propriedade **Scenarios** é uma coleção de objetos do tipo **Scenario** (um tipo que mencionamos anteriormente). Falaremos sobre **Scenario** na próxima subseção, então vamos deixar a propriedade **Scenarios** para depois.
- Os métodos **BuildClipboardFormatsOutputString**, **DisplayToast** e **EnableClipboardContentChangedNotifications** são funções de utilitário que parecem mais relacionadas ao estado geral da amostra do que à página principal. Portanto, durante a portabilidade, refatoraremos esses três métodos para um novo tipo de utilitário chamado **SampleState** (que não precisa ser um tipo do Windows Runtime). Por esse motivo, esses três métodos não vão na IDL.
- O método **NotifyUser** é chamado de dentro das páginas XAML do cenário individual na instância de **MainPage** retornada do campo estático *Current*. Como **Current** é uma instância do tipo projetado (conforme mencionado anteriormente), precisamos declarar **NotifyUser** na IDL. **NotifyUser** usa um parâmetro do tipo **NotifyType**. Falaremos sobre isso na próxima subseção.

Estamos fazendo progresso no desenvolvimento de uma lista de quais membros adicionar ou não ao arquivo `MainPage.idl`. Mas ainda é preciso discutir a propriedade **Scenarios** e o tipo **NotifyType**. Vamos fazer isso a seguir.

### <a name="idl-for-the-scenario-and-notifytype-types"></a>IDL para os tipos **Scenario** e **NotifyType**

A classe **Scenario** é definida em `SampleConfiguration.cs`. Temos que decidir como fazer a portabilidade dessa classe para C++/WinRT. Por padrão, provavelmente a tornaríamos uma `struct` C++ comum. Porém, se **Scenario** estiver sendo usada entre binários ou para interoperar com a estrutura XAML, ela precisará ser declarada na IDL como um tipo do Windows Runtime.

Ao estudarmos o código-fonte em C#, descobrimos que **Scenario** é usada nesse contexto.

```xaml
<ListBox x:Name="ScenarioControl" ... >
```

```csharp
var itemCollection = new List<Scenario>();
int i = 1;
foreach (Scenario s in scenarios)
{
    itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
}
ScenarioControl.ItemsSource = itemCollection;
```

Uma coleção de objetos da classe  **Scenario** está sendo atribuída à propriedade [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) de uma **ListBox** (que é um controle de itens). Como a classe **Scenario** *precisa* interoperar com o XAML, ela tem que ser um tipo do Windows Runtime. Portanto, precisa ser definida na IDL. Definir o tipo **Scenario** na IDL faz com que o sistema de build C++/WinRT gere uma definição de código-fonte de **Scenario** para você em um arquivo de cabeçalho nos bastidores (cujo nome e local não são importantes para esta explicação passo a passo).

E você se lembrará de que **MainPage.Scenarios** é uma coleção de objetos de **Scenario**, que acabamos de dizer que precisam estar na IDL. Por esse motivo, **MainPage.Scenarios** também precisa ser declarada na IDL.

**NotifyType** é uma `enum` declarada em `MainPage.xaml.cs` do C#. Como passamos **NotifyType** para um método pertencente à classe de runtime **MainPage**, **NotifyType** também precisa ser um tipo do Windows Runtime, além de ser definido em `MainPage.idl`.

Agora, vamos adicionar ao arquivo `MainPage.idl` os novos tipos e o novo membro de **Mainpage** que decidimos declarar na IDL. Ao mesmo tempo, removeremos da IDL os membros de espaço reservado de **Mainpage** que o modelo de projeto do Visual Studio nos forneceu.

Portanto, no seu projeto C++/WinRT, abra o arquivo `MainPage.idl` e edite-o para que ele se pareça com a listagem abaixo. Observe que uma das edições é alterar o nome do namespace de **Clipboard** para **SDKTemplate**. Se desejar, basta excluir o conteúdo atual do `MainPage.idl` e colar na lista abaixo. Outro ajuste a ser observado é que estamos alterando o nome de **Scenario::ClassType** para **Scenario::ClassName**.

```idl
// MainPage.idl
namespace SDKTemplate
{
    struct Scenario
    {
        String Title;
        Windows.UI.Xaml.Interop.TypeName ClassName;
    };

    enum NotifyType
    {
        StatusMessage,
        ErrorMessage
    };

    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();

        static MainPage Current{ get; };
        static String FEATURE_NAME{ get; };

        static Windows.Foundation.Collections.IVector<Scenario> scenarios{ get; };

        void NotifyUser(String strMessage, NotifyType type);
    };
}
```

> [!NOTE]
> Para obter mais informações sobre o conteúdo de um arquivo `.idl` em um projeto C++/WinRT, confira [Linguagem IDL da Microsoft 3.0](/uwp/midl-3/).

Com seu próprio trabalho de portabilidade, talvez você não queira nem precise alterar o namespace, como fizemos acima. Estamos fazendo isso aqui apenas porque o namespace padrão do projeto C# que estamos portando é **SDKTemplate**; enquanto o nome do projeto e do assembly é **Clipboard**.

Mas, à medida que prosseguirmos com a portabilidade neste passo a passo, mudaremos todas as ocorrências no código-fonte do namespace **Clipboard** para **SDKTemplate**. Também há um local nas propriedades do projeto C++/WinRT em que o nome do namespace **Clipboard** aparece, portanto, aproveitaremos a oportunidade para mudar isso agora.

No Visual Studio, para o projeto C++/WinRT, defina a propriedade de projeto  **Common Properties** \> **C++/WinRT** \> **Root Namespace** como *SDKTemplate*.

### <a name="save-the-idl-and-re-generate-stub-files"></a>Salvar a IDL e gerar os arquivos stub novamente

No tópico [Controles XAML; associar a uma propriedade de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property), você encontra a noção de *arquivos stub*. Os arquivos stub são gerados para você (por uma ferramenta denominada `cppwinrt.exe`, e com base nos seus arquivos `.idl`) quando você compila o projeto C++/WinRT. Esse tópico contém mais dados sobre eles.

Neste ponto do passo a passo, terminamos de editar o arquivo `MainPage.idl` por enquanto, então é preciso salvá-lo. O projeto não será totalmente compilado neste momento, mas é útil começar a compilar agora a fim de regenerar os arquivos stub para **MainPage**.

Para este projeto C++/WinRT, os arquivos stub são gerados na pasta `\Clipboard\Clipboard\Generated Files\sources`. Você os encontrará na pasta após a conclusão do build parcial (novamente, conforme esperado, o build não será feito por completo. Porém, o passo em que estamos interessados na&mdash;geração de stubs&mdash;*será* concluído). Os arquivos de interesse ​​são `MainPage.h` e `MainPage.cpp`.

Nesses dois arquivos stub, você verá implementações de stub dos novos membros de **MainPage** que adicionamos à IDL (**Current** e **FEATURE_NAME**, por exemplo). Copiaremos essas implementações de stub nos arquivos `MainPage.h` e `MainPage.cpp` que já estão no projeto. Ao mesmo tempo, assim como fizemos com a IDL, removeremos desses arquivos existentes os membros de espaço reservado de **Mainpage** que o modelo de projeto do Visual Studio nos forneceu (a propriedade fictícia denominada **MyProperty** e o manipulador de eventos denominado **ClickHandler**).

De fato, o único membro da versão atual de **MainPage** que queremos manter é o construtor.

Depois que você copiar os novos membros dos arquivos stub, excluir os membros que não queremos e atualizar o namespace, os arquivos `MainPage.h` e `MainPage.cpp` em seu projeto devem ficar como mostrado a seguir. Observe que a única alteração que fizemos no tipo **factory_implementation** foi a atualização do namespace.

```cppwinrt
// MainPage.h
#pragma once
#include "MainPage.g.h"

namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        static SDKTemplate::MainPage Current();
        static hstring FEATURE_NAME();
        static Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> scenarios();
        void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
    };
}
namespace winrt::SDKTemplate::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::SDKTemplate::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
    }
    SDKTemplate::MainPage MainPage::Current()
    {
        throw hresult_not_implemented();
    }
    hstring MainPage::FEATURE_NAME()
    {
        throw hresult_not_implemented();
    }
    Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> MainPage::scenarios()
    {
        throw hresult_not_implemented();
    }
    void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
    {
        throw hresult_not_implemented();
    }
}
```

Para cadeias de caracteres, o C# usa **System.String**. Confira o método **MainPage.NotifyUser** para obter um exemplo. Em nossa IDL, declaramos uma cadeia de caracteres com **String** e, quando a ferramenta `cppwinrt.exe` gera o código C++/WinRT, usa o tipo [**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring). Sempre que encontrarmos uma cadeia de caracteres no código C#, nós a portaremos para **winrt::hstring**. Para saber mais, confira [Processamento da cadeia de caracteres em C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings).

Para uma explicação dos parâmetros `const&` nas assinaturas do método, confira [Passagem de parâmetro](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing).

### <a name="update-all-remaining-namespace-declarationsreferences-and-build"></a>Atualizar todas as declarações/referências de namespace restantes e compilar

Antes de compilar o projeto C++/WinRT, localize quaisquer declarações ou referências ao namespace **Clipboard** e altere-as para **SDKTemplate**.

- `MainPage.xaml` e `App.xaml`. O namespace aparece nos valores dos atributos `x:Class` e `xmlns:local`.
- `App.idl`.
- `App.h`.
- `App.cpp`. Existem duas diretivas `using namespace` (pesquise pela substring `using namespace Clipboard`), e duas qualificações do tipo **MainPage** (pesquise por `Clipboard::MainPage`). Elas precisam ser alteradas.

Como removemos o manipulador de eventos de **MainPage**, entre também no `MainPage.xaml` e exclua o elemento **Button** da marcação.

Salve todos os arquivos. Limpe a solução (**Build** > **Limpar Clipboard**) e, em seguida, compile-a. O build será bem-sucedido se as alterações feitas até o momento forem válidas.

### <a name="implement-the-mainpage-members-that-we-declared-in-idl"></a>Implementar os membros de **MainPage** que declaramos na IDL

#### <a name="the-constructor-current-and-feature_name"></a>O construtor, **Current** e **FEATURE_NAME**

Aqui está o código relevante (do projeto C#) que precisamos portar.

```xaml
<!-- MainPage.xaml -->
...
<TextBlock x:Name="SampleTitle" ... />
...
```

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
    public static MainPage Current;

    public MainPage()
    {
        InitializeComponent();
        Current = this;
        SampleTitle.Text = FEATURE_NAME;
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
    public const string FEATURE_NAME = "Clipboard C# sample";
...
}
...
```

Em breve, reutilizaremos `MainPage.xaml` em sua totalidade (copiando-o). Por enquanto, adicionaremos temporariamente um elemento **TextBlock**, com o nome apropriado, ao `MainPage.xaml` do projeto C++/WinRT.

**FEATURE_NAME** é um campo estático de **MainPage** (um campo `const` C# é essencialmente estático em seu comportamento), definido em `SampleConfiguration.cs`. Para C++/WinRT, em vez de um campo (estático), nós o tornaremos a expressão C++/WinRT de uma propriedade somente leitura (estática). A maneira do C++/WinRT de expressar um getter de propriedades é como uma função que retorna o valor da propriedade e não aceita parâmetros (um acessador). Portanto, o campo estático **FEATURE_NAME** C# torna-se a função de acessador estático **FEATURE_NAME** do C++/WinRT (neste caso, retornando a literal de cadeia de caracteres).

Aliás, faríamos o mesmo se estivéssemos portando uma propriedade somente leitura de C#. Para uma propriedade gravável em C#, a maneira do C++/ WinRT de expressar um setter de propriedades é como uma função `void` que assume o valor da propriedade como um parâmetro (um modificador). Seja qual for o caso, se o campo ou propriedade C# for estático, o acessador e/ou modificador do C++/WinRT também será.

**Current** é um campo estático (não uma constante) de **MainPage**. Novamente, vamos transformá-lo em (a expressão C++/WinRT de) uma propriedade somente leitura e, mais uma vez, torná-lo estático. Quando **FEATURE_NAME** é constante, **Current** não é. No C++/WinRT, precisaremos de um campo de apoio, e nosso acessador retornará isso. Portanto, no projeto C++/WinRT, declararemos em `MainPage.h` um campo estático privado denominado **current**, definiremos/inicializaremos **current** em `MainPage.cpp` (porque tem duração de armazenamento estático) e o acessaremos por meio de uma função de acessador estático público chamada **Current**.

O próprio construtor executa algumas atribuições, que são fáceis de portar.

No projeto C++/WinRT, adicione um novo item **Visual C++**  > **Código** > Arquivo C++ **(. cpp)** com o nome `SampleConfiguration.cpp`.

Edite `MainPage.xaml`, `MainPage.h`, `MainPage.cpp` e `SampleConfiguration.cpp` para corresponder às listagens abaixo.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    <TextBlock x:Name="SampleTitle" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        static SDKTemplate::MainPage Current() { return current; }
...
    private:
        static SDKTemplate::MainPage current;
...
    };
...
}

// MainPage.cpp
...
namespace winrt::SDKTemplate::implementation
{
    SDKTemplate::MainPage MainPage::current{ nullptr };
...
    MainPage::MainPage()
    {
        InitializeComponent();
        MainPage::current = *this;
        SampleTitle().Text(FEATURE_NAME());
    }
...
}

// SampleConfiguration.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace SDKTemplate;

hstring implementation::MainPage::FEATURE_NAME()
{
    return L"Clipboard C++/WinRT Sample";
}
```

Além disso, certifique-se de excluir os corpos de função existentes de `MainPage.cpp` para **MainPage::Current()** e **MainPage::FEATURE_NAME()** , pois agora estamos definindo esses métodos em outro lugar.

Como você pode ver, **MainPage:: Current** é declarado como sendo do tipo **SDKTemplate:: MainPage**, que é o tipo projetado. Não é do tipo **SDKTemplate:: Implementation:: MainPage**, que é o tipo de implementação. O tipo projetado é aquele criado para ser consumido no projeto para interoperação XAML ou entre binários. O tipo de implementação é o que você usa para implementar os recursos que expôs no tipo projetado. Como a declaração de **MainPage::current** (em `MainPage.h`) aparece dentro do namespace de implementação (**winrt::SDKTemplate::implementation**), um **MainPage** não qualificado teria se referido ao tipo de implementação. Portanto, qualificamos **SDKTemplate::** para ficar claro que queremos que **MainPage::current** seja uma instância do tipo projetado **winrt::SDKTemplate::MainPage**.

No construtor, há alguns pontos relacionados a `MainPage::current = *this;` que merecem uma explicação.
- Quando você usa o ponteiro `this` dentro de um membro do tipo de implementação, é claro que o ponteiro `this` é *um ponteiro para o tipo de implementação*.
- A fim de converter o ponteiro `this` para o tipo projetado correspondente, desfaça a referência a ele. Desde que você gere seu tipo de implementação pela IDL (como temos aqui), o tipo de implementação terá um operador de conversão que se converte em seu tipo projetado. É por isso que a atribuição funciona aqui.

Para saber mais, confira [Criar instâncias e retornar tipos de implementação e interfaces](/windows/uwp/cpp-and-winrt-apis/author-apis#instantiating-and-returning-implementation-types-and-interfaces).

`SampleTitle().Text(FEATURE_NAME());` também está no construtor. A parte `SampleTitle()` é uma chamada para uma função de acessador simples denominada **SampleTitle**, que retorna o **TextBlock** que adicionamos ao XAML. Sempre que você define `x:Name` para um elemento XAML, o compilador XAML gera um acessador que é nomeado para o elemento. A parte `.Text(...)` chama a função de modificador **Text** no objeto **TextBlock** que o acessador **SampleTitle** retornou. E `FEATURE_NAME()` chama nossa função de acessador estático **MainPage::FEATURE_NAME** para retornar a literal de cadeia de caracteres. No total, essa linha de código define a propriedade **Text** do **TextBlock** chamada *SampleTitle*.

Observe que, como as cadeias de caracteres são longas no Windows Runtime, para portar uma literal de cadeia de caracteres, nós a prefixamos com o prefixo de codificação de caractere largo `L`. Então, alteramos (por exemplo) "uma literal de cadeia de caracteres" para "uma literal de cadeia de caracteres" L. Confira também [Literais de cadeia de caracteres longa](/cpp/cpp/string-and-character-literals-cpp#wide-string-literals).

#### <a name="scenarios"></a>**Cenários**

Aqui está o código C# relevante que precisamos portar.

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
...
    public List<Scenario> Scenarios
    {
        get { return this.scenarios; }
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
...
    List<Scenario> scenarios = new List<Scenario>
    {
        new Scenario() { Title = "Copy and paste text", ClassType = typeof(CopyText) },
        new Scenario() { Title = "Copy and paste an image", ClassType = typeof(CopyImage) },
        new Scenario() { Title = "Copy and paste files", ClassType = typeof(CopyFiles) },
        new Scenario() { Title = "Other Clipboard operations", ClassType = typeof(OtherScenarios) }
    };
...
}
...
```

A partir de nossa investigação anterior, sabemos que esta coleção de objetos de **Scenario** está sendo exibida em uma **ListBox**. No C++/WinRT, existem limites para *o tipo de coleção* que podemos atribuir à propriedade **ItemsSource** de um controle de itens. A coleção deve ser um vetor ou um vetor observável, e seus elementos devem ser um dos indicados a seguir.

- Classes de runtime ou
- [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable).

No caso de **IInspectable**, se os elementos não forem eles próprios classes de runtime, precisarão ser de um tipo que possa passar por conversão boxing e unboxing para (e de) [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). E isso significa que eles precisam ser tipos do Windows Runtime (confira [Fazer conversão boxing e unboxing de valores escalares para IInspectable](/windows/uwp/cpp-and-winrt-apis/boxing)).

Para este estudo de caso, não transformamos o **Scenario** em uma classe de runtime. Essa ainda é uma opção razoável, no entanto. E haverá casos em seu próprio trabalho de portabilidade em que uma classe de runtime será definitivamente a opção mais adequada. Por exemplo, se você precisar tornar o tipo de elemento *observável* (confira [Controles XAML; associar a uma propriedade de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property)) ou se o elemento precisar ter métodos por qualquer outro motivo e for mais do que apenas um conjunto de membros de dados.

Como, neste passo a passo, *não* estamos trabalhando com uma classe de runtime para o tipo **Scenario**, precisamos pensar em conversão boxing. Se tivéssemos tornado **Scenario** um `struct` C++ regular, não seria possível empregar a conversão boxing. Mas nós declaramos **Scenario** como `struct` na IDL, então *podemos* usar a conversão boxing.

Temos a opção de fazer a conversão boxing de **Scenario** antecipadamente ou aguardar até estarmos prestes a atribuir ao **ItemsSource** e fazer a conversão boxing just-in-time. Aqui estão algumas considerações sobre essas duas opções.

- Conversão boxing antecipada. Para essa opção, nosso membro de dados é uma coleção de **IInspectable** pronta para atribuição à interface do usuário. Na inicialização, fazemos a conversão boxing dos objetos de **Scenario** para esse membro de dados. Precisamos de apenas uma cópia dessa coleção, mas é necessário fazer a conversão unboxing de um elemento toda vez que precisamos ler seus campos.
- Conversão boxing just-in-time. Para essa opção, nosso membro de dados é uma coleção de **Scenario**. Quando chega a hora de atribuir à interface do usuário, fazemos a conversão boxing dos objetos de **Scenario** do membro de dados para uma nova coleção de **IInspectable**. Podemos ler os campos dos elementos no membro de dados sem fazer a conversão unboxing, mas precisamos de duas cópias da coleção.

Como você pode ver, para uma coleção pequena como essa, os prós e os contras fazem com que pareça um esforço desnecessário. Portanto, para este estudo de caso, usaremos a opção just-in-time.

O membro **scenarios** é um campo de **MainPage**, definido e inicializado em `SampleConfiguration.cs`. E **Scenarios** é uma propriedade somente leitura de **MainPage**, definida em `MainPage.xaml.cs` (e implementada para simplesmente retornar o campo **scenarios**). Faremos algo semelhante no projeto C++/WinRT, mas tornaremos os dois membros estáticos (já que precisamos de apenas uma instância no aplicativo, para que possamos acessá-los sem a necessidade de uma instância de classe). E os nomearemos *scenariosInner* e *scenarios*, respectivamente. Declararemos *scenariosInner* em `MainPage.h`. E, como tem duração de armazenamento estático, nós o definiremos/inicializaremos em um arquivo `.cpp` (`SampleConfiguration.cpp`, neste caso).

Edite `MainPage.h` e `SampleConfiguration.cpp` para corresponder às listagens abaixo.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    static Windows::Foundation::Collections::IVector<Scenario> scenarios() { return scenariosInner; }
...
private:
    static winrt::Windows::Foundation::Collections::IVector<Scenario> scenariosInner;
...
};

// SampleConfiguration.cpp
...
using namespace Windows::Foundation::Collections;
...
IVector<Scenario> implementation::MainPage::scenariosInner = winrt::single_threaded_observable_vector<Scenario>(
{
    Scenario{ L"Copy and paste text", xaml_typename<SDKTemplate::CopyText>() },
    Scenario{ L"Copy and paste an image", xaml_typename<SDKTemplate::CopyImage>() },
    Scenario{ L"Copy and paste files", xaml_typename<SDKTemplate::CopyFiles>() },
    Scenario{ L"History and roaming", xaml_typename<SDKTemplate::HistoryAndRoaming>() },
    Scenario{ L"Other Clipboard operations", xaml_typename<SDKTemplate::OtherScenarios>() },
});
```

Além disso, exclua o corpo da função existente de `MainPage.cpp` para **MainPage::scenarios()** , porque agora estamos definindo esse método no arquivo de cabeçalho.

Como você pode ver no `SampleConfiguration.cpp`, inicializamos o membro de dados estáticos *scenariosInner* chamando uma função auxiliar C++/WinRT denominada [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector). Essa função cria um novo objeto de coleção do Windows Runtime para nós e o retorna como uma interface [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_). Como, nesta amostra, a coleção não é *observável* (não precisa ser, pois não adiciona nem remove elementos após a inicialização), poderíamos optar por chamar [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector). Essa função retorna a coleção como uma interface [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_).

Para saber mais sobre coleções e a associação a elas, confira [Controles de itens XAML; associar a uma coleção de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection) e [Coleções com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/collections).

O código de inicialização que você acabou de adicionar faz referência a tipos que ainda não estão no projeto (por exemplo, **winrt::SDKTemplate::CopyText**. Para corrigir isso, vamos adicionar cinco novas páginas XAML em branco ao projeto.

#### <a name="add-five-new-blank-xaml-pages"></a>Adicionar cinco novas páginas XAML em branco

Adicione um novo item **XAML** > **Página em Branco C++(/WinRT)**   ao projeto (tenha certeza de que ele é o modelo de item **Página em Branco (C++/WinRT)** , e não a **Página em Branco**). Nomeie-o como  `CopyText`. A nova página XAML é definida no namespace **SDKTemplate**, que é o que queremos.

Repita o processo acima mais quatro vezes e nomeie as páginas XAML `CopyImage`, `CopyFiles`, `HistoryAndRoaming` e `OtherScenarios`.

Agora você poderá compilar novamente, se desejar.

#### <a name="notifyuser"></a>**NotifyUser**

No projeto C#, você encontrará a implementação do método **MainPage.NotifyUser** em `MainPage.xaml.cs`. **MainPage.NotifyUser** tem uma dependência em **MainPage.UpdateStatus**, e esse método, por sua vez, depende dos elementos XAML que ainda não portamos. Sendo assim, por enquanto, apenas removeremos um método **UpdateStatus** no projeto C++/WinRT e o portaremos posteriormente.

Aqui está o código C# relevante que precisamos portar.

```csharp
// MainPage.xaml.cs
...
public void NotifyUser(string strMessage, NotifyType type)
if (Dispatcher.HasThreadAccess)
{
    UpdateStatus(strMessage, type);
}
else
{
    var task = Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
}
private void UpdateStatus(string strMessage, NotifyType type) { ... }{
...
```

**NotifyUser** usa a enumeração [**Windows.UI.Core.CoreDispatcherPriority**](/uwp/api/windows.ui.core.coredispatcherpriority). No C++/WinRT, sempre que você quiser usar um tipo de namespace do Windows, inclua o arquivo de cabeçalho correspondente do namespace do Windows C++/WinRT (para saber mais, confira [Introdução ao C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started)). Neste caso, como você verá na listagem de código abaixo, o cabeçalho é `winrt/Windows.UI.Core.h`, e o incluiremos em `pch.h`.

**UpdateStatus** é privado. Portanto, tornaremos esse método privado em nosso tipo de implementação **MainPage**. **UpdateStatus** não deve ser chamado na classe de runtime, portanto, não o declararemos na IDL.

Depois de portar **MainPage.NotifyUser** e remover **MainPage.UpdateStatus**, é isso que temos no projeto C++/WinRT. Após a listagem do código, examinaremos alguns detalhes.

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Core.h>
...

// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
private:
    void UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
};

// MainPage.cpp
...
void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    if (Dispatcher().HasThreadAccess())
    {
        UpdateStatus(strMessage, type);
    }
    else
    {
        Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [strMessage, type, this]()
            {
                UpdateStatus(strMessage, type);
            });
    }
}
void MainPage::UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    throw hresult_not_implemented();
}
...
```

Em C#, você *pontua* propriedades aninhadas. Portanto, o tipo **MainPage** C# pode acessar sua própria propriedade **Dispatcher** com a sintaxe `Dispatcher`. E C# pode *pontuar* esse valor com sintaxe como `Dispatcher.HasThreadAccess`. No C++/WinRT, as propriedades são implementadas como funções de acessador, portanto, a sintaxe difere apenas no fato de que você adiciona parênteses para cada chamada de função.

|C#|C++/WinRT|
|-|-|
|`Dispatcher.HasThreadAccess`|`Dispatcher().HasThreadAccess()`|

Quando a versão C# do **NotifyUser** chama [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync), implementa o delegado de retorno de chamada assíncrono como uma função lambda. A versão C++/WinRT faz o mesmo, mas a sintaxe é um pouco diferente. No C++/WinRT, *capturamos* os dois parâmetros que vamos usar, bem como o ponteiro `this` (já que vamos chamar uma função de membro). Há mais informações sobre como implementar delegados como lambdas e exemplos de código no tópico [Manipular eventos usando delegados em C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events). Além disso, podemos desconsiderar a parte `var task =` neste caso em particular. Não estamos aguardando o objeto assíncrono retornado, portanto, não há necessidade de armazená-lo. 

### <a name="implement-the-remaining-mainpage-members"></a>Implementar os membros restantes de **MainPage**

Vamos fazer uma lista completa dos membros da classe **MainPage** (implementados em `MainPage.xaml.cs` e `SampleConfiguration.cs`) para que possamos ver quais portamos até agora e quais ainda estão pendentes.

|Membro|Acesso|Status|
|-|-|-|
|Construtor **MainPage**|`public`|Portabilidade efetuada|
|Propriedade **Current**|`public`|Portabilidade efetuada|
|Propriedade **FEATURE_NAME**|`public`|Portabilidade efetuada|
|Propriedade **IsClipboardContentChangedEnabled**|`public`|Não iniciado|
|Propriedade **Scenarios**|`public`|Portabilidade efetuada|
|Método **BuildClipboardFormatsOutputString**|`public`|Não iniciado|
|Método **DisplayToast**|`public`|Não iniciado|
|Método **EnableClipboardContentChangedNotifications**|`public`|Não iniciado|
|Método **NotifyUser**|`public`|Portabilidade efetuada|
|Método **OnNavigatedTo**|`protected`|Não iniciado|
|Campo **isApplicationWindowActive**|`private`|Não iniciado|
|Campo **needToPrintClipboardFormat**|`private`|Não iniciado|
|Campo **scenarios**|`private`|Portabilidade efetuada|
|Método **Button_Click**|`private`|Não iniciado|
|Método **DisplayChangedFormats**|`private`|Não iniciado|
|Método **Footer_Click**|`private`|Não iniciado|
|Método **HandleClipboardChanged**|`private`|Não iniciado|
|Método **OnClipboardChanged**|`private`|Não iniciado|
|Método **OnWindowActivated**|`private`|Não iniciado|
|Método **ScenarioControl_SelectionChanged**|`private`|Não iniciado|
|Método **UpdateStatus**|`private`|Removido|

Falaremos sobre os membros ainda não portados nas próximas subseções.

> [!NOTE]
> De tempos em tempos, encontraremos referências no código-fonte a elementos da interface do usuário na marcação XAML (em `MainPage.xaml`). À medida que chegarmos a essas referências, trabalharemos temporariamente com elas adicionando elementos simples de espaço reservado ao XAML. Dessa forma, o projeto continuará a ser compilado após cada subseção. A alternativa é resolver as referências copiando *todo* o conteúdo de `MainPage.xaml` do projeto C# para o projeto C++/WinRT agora. Porém, se fizermos isso, levará um longo tempo até que possamos parar e compilar outra vez (potencialmente encobrindo quaisquer erros de digitação ou outros erros que cometermos ao longo do caminho).
>
> Quando terminarmos de portar o código imperativo para a classe **MainPage**, *em seguida*, copiaremos o conteúdo do arquivo XAML e teremos certeza de que o projeto ainda será compilado.

#### <a name="isclipboardcontentchangedenabled"></a>**IsClipboardContentChangedEnabled**

Esta é uma propriedade C# get-set cujo padrão é `false`. É membro de **MainPage** e está definida em `SampleConfiguration.cs`.

Para C++/WinRT, precisaremos de uma função de acessador, uma função de modificador e um membro de dados de apoio como um campo. Como **IsClipboardContentChangedEnabled** representa o estado de um dos cenários da amostra, e não o estado de **MainPage**, criaremos os novos membros em um novo tipo de utilitário chamado **SampleState**. Implementaremos isso em nosso arquivo de código-fonte `SampleConfiguration.cpp` e criaremos os membros `static` (já que precisamos de apenas uma instância no aplicativo; assim, poderemos acessá-los sem precisar de uma instância de classe).

Para acompanhar nosso `SampleConfiguration.cpp` no projeto C++/WinRT, adicione um novo item **Visual C++**  > **Código** > **Arquivo de Cabeçalho (.h)** com o nome `SampleConfiguration.h`. Edite `SampleConfiguration.h` e `SampleConfiguration.cpp` para corresponder às listagens abaixo.

```cppwinrt
// SampleConfiguration.h
#pragma once 
#include "pch.h"

namespace winrt::SDKTemplate
{
    struct SampleState
    {
        static bool IsClipboardContentChangedEnabled();
        static void IsClipboardContentChangedEnabled(bool checked);
    private:
        static bool isClipboardContentChangedEnabled;
    };
}

// SampleConfiguration.cpp
...
#include "SampleConfiguration.h"
...
bool SampleState::isClipboardContentChangedEnabled = false;
...
bool SampleState::IsClipboardContentChangedEnabled()
{
    return isClipboardContentChangedEnabled;
}
void SampleState::IsClipboardContentChangedEnabled(bool checked)
{
    if (isClipboardContentChangedEnabled != checked)
    {
        isClipboardContentChangedEnabled = checked;
    }
}
```

Novamente, um campo com armazenamento `static` (como **SampleState::isClipboardContentChangedEnabled**) deve ser definido uma vez no aplicativo, e um arquivo `.cpp` é um bom local para isso (`SampleConfiguration.cpp`, nesse caso).

#### <a name="buildclipboardformatsoutputstring"></a>**BuildClipboardFormatsOutputString**

Esse método é um membro público de **MainPage** e é definido em `SampleConfiguration.cs`.

```csharp
// SampleConfiguration.cs
...
public string BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent = Windows.ApplicationModel.DataTransfer.Clipboard.GetContent();
    StringBuilder output = new StringBuilder();

    if (clipboardContent != null && clipboardContent.AvailableFormats.Count > 0)
    {
        output.Append("Available formats in the clipboard:");
        foreach (var format in clipboardContent.AvailableFormats)
        {
            output.Append(Environment.NewLine + " * " + format);
        }
    }
    else
    {
        output.Append("The clipboard is empty");
    }
    return output.ToString();
}
...
```

No C++/WinRT, tornaremos **BuildClipboardFormatsOutputString** um método público estático de **SampleState**. Podemos torná-lo `static` porque ele não acessa nenhum membro de instância.

Para usar os tipos **Clipboard** e **DataPackageView** no C++/WinRT, precisaremos incluir o arquivo de cabeçalho do namespace do C++/WinRT `winrt/Windows.ApplicationModel.DataTransfer.h`.

No C#, a propriedade **DataPackageView.AvailableFormats** é uma **IReadOnlyList**, de modo que podemos acessar sua propriedade **Count**. No C++/WinRT, a função de acessador **DataPackageView::AvailableFormats** retorna um **IVectorView**, que tem uma função de acessador **Size** que podemos chamar.

Para portar o uso do tipo **System.Text.StringBuilder** C#, usaremos o tipo padrão C++ [**std::wostringstream**](/cpp/standard-library/sstream-typedefs#wostringstream). Esse tipo é um fluxo de saída para cadeias de caracteres longas (e, para usá-lo, precisamos incluir o arquivo de cabeçalho `sstream`). Em vez de usar um método **Append** como você faz com um **StringBuilder**, você usa o [operador de inserção](/cpp/standard-library/using-insertion-operators-and-controlling-format) (`<<`) com um fluxo de saída como **wostringstream**. Para saber mais, confira [Programação iostream](/cpp/standard-library/iostream-programming) e [Formatar cadeias de caracteres C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings#formatting-strings).

O código C# constrói um **StringBuilder** com a palavra-chave `new`. No C#, os objetos são tipos de referência por padrão, declarados no heap com `new`. No padrão C++ moderno, os objetos são tipos de valor por padrão, declarados na pilha (sem usar `new`). Portanto, portamos `StringBuilder output = new StringBuilder();` para C++/WinRT como simplesmente `std::wostringstream output;`.

A palavra-chave `var` do C# solicita que o compilador infira um tipo. Você porta `var` para `auto` no C++/WinRT. Mas, no C++/WinRT, há casos em que (para evitar cópias) é interessante ter uma *referência* a um tipo inferido (ou deduzido), e você expresse isso com `auto&`. Também há casos em que convém ter um tipo especial de referência que seja associado corretamente, independentemente de ser inicializado com um *lvalue* ou com um *rvalue*. E você expressa isso com `auto&&`. Esse é o formulário que você vê sendo usado no loop `for` no código portado abaixo. Para ver uma introdução a *lvalues* e *rvalues*, confira [Categorias de valor e referência a elas](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories).

Edite `pch.h`, `SampleConfiguration.h` e `SampleConfiguration.cpp` para corresponder às listagens abaixo.

```cppwinrt
// pch.h
...
#include <sstream>
#include "winrt/Windows.ApplicationModel.DataTransfer.h"
...

// SampleConfiguration.h
...
static hstring BuildClipboardFormatsOutputString();
...

// SampleConfiguration.cpp
...
using namespace Windows::ApplicationModel::DataTransfer;
...
hstring SampleState::BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent{ Clipboard::GetContent() };
    std::wostringstream output;

    if (clipboardContent && clipboardContent.AvailableFormats().Size() > 0)
    {
        output << L"Available formats in the clipboard:";
        for (auto&& format : clipboardContent.AvailableFormats())
        {
            output << std::endl << L" * " << std::wstring_view(format);
        }
    }
    else
    {
        output << L"The clipboard is empty";
    }

    return hstring{ output.str() };
}
```

> [!NOTE]
> A sintaxe na linha de código `DataPackageView clipboardContent{ Clipboard::GetContent() };` usa um recurso do C++ padrão moderno chamado de *inicialização uniforme*, com o uso característico de chaves em vez de um sinal de `=`. Essa sintaxe torna claro que a inicialização, em vez da atribuição, está ocorrendo. Se você preferir a forma de sintaxe que *parece* atribuição (mas, na verdade, não é), poderá substituir a sintaxe acima pela `DataPackageView clipboardContent = Clipboard::GetContent();` equivalente. No entanto, é uma boa ideia se sentir confortável com as duas maneiras de expressar a inicialização, pois você provavelmente verá ambas sendo usadas com frequência no código que encontrar.

#### <a name="displaytoast"></a>**DisplayToast**

**DisplayToast** é um método estático público da classe **MainPage** C#, e você encontrará sua definição em `SampleConfiguration.cs`. No C++/WinRT, nós o tornaremos um método público estático de **SampleState**.

Já encontramos a maioria dos dados e técnicas relevantes para portar esse método. Um novo item a ser observado é a portabilidade de uma literal de cadeia de caracteres textual C# (`@`) para uma [literal de cadeia de caracteres bruta](/cpp/cpp/string-and-character-literals-cpp#raw-string-literals-c11) C++ padrão (`LR`).

Além disso, quando você faz referência aos tipos [**ToastNotification**](/uwp/api/windows.ui.notifications.toastnotification) e [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) no C++/WinRT, pode qualificá-los pelo namespace ou editar `SampleConfiguration.cpp` e adicionar diretivas `using namespace`, como no exemplo a seguir.

```cppwinrt
using namespace Windows::UI::Notifications;
```

Você tem a mesma opção ao fazer referência ao tipo [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) e sempre que faz referência a qualquer outro tipo do Windows Runtime.

Além desses itens, basta seguir as mesmas orientações que você seguiu anteriormente para realizar as próximas etapas.

- Declare o método no `SampleConfiguration.h` e defina-o em `SampleConfiguration.cpp`.
- Edite `pch.h` para incluir todos os arquivos de cabeçalho de namespace do Windows C++/WinRT necessários.
- Construa objetos C++/WinRT na pilha, não no heap.
- Substitua chamadas para acessadores de propriedade pela sintaxe de chamada de função (`()`).

Uma causa muito comum de erros de compilador/vinculador é esquecer de incluir os arquivos de cabeçalho de namespace do Windows C++/WinRT necessários. Para mais informações sobre um possível erro, confira [Por que o vinculador mostra um erro "LNK2019: erro externo não resolvido"?](/windows/uwp/cpp-and-winrt-apis/faq#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error).

Se você quiser acompanhar o passo a passo e fazer a portabilidade do **DisplayToast** você mesmo, poderá comparar seus resultados com o código na versão C++/WinRT do código-fonte da amostra de Clipboard que você baixou (está em [`Windows-universal-samples/Samples/Clipboard/cppwinrt`](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard/cppwinrt)`/Clipboard.sln`).

#### <a name="enableclipboardcontentchangednotifications"></a>**EnableClipboardContentChangedNotifications**

**EnableClipboardContentChangedNotifications** é um método estático público da classe **MainPage** C# e está definido em `SampleConfiguration.cs`.

```csharp
// SampleConfiguration.cs
...
public bool EnableClipboardContentChangedNotifications(bool enable)
{
    if (IsClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled = enable;
    if (enable)
    {
        Clipboard.ContentChanged += OnClipboardChanged;
        Window.Current.Activated += OnWindowActivated;
    }
    else
    {
        Clipboard.ContentChanged -= OnClipboardChanged;
        Window.Current.Activated -= OnWindowActivated;
    }
    return true;
}
...
private void OnClipboardChanged(object sender, object e) { ... }
private void OnWindowActivated(object sender, WindowActivatedEventArgs e) { ... }
...
```

No C++/WinRT, nós o tornaremos um método público estático de **SampleState**.

No C#, você usa a sintaxe de operador `+=` e `-=` para registrar e revogar delegados de manipulação de eventos. No C++/WinRT, há várias opções sintáticas para registrar/revogar um delegado, conforme descrito em [Manipular eventos usando delegados em C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events). Mas a forma geral é a que você registra e revoga com chamadas a um par de funções nomeadas para o evento. Para se registrar, passe seu representante para a função de registro e recupere um token de revogação em troca (um [**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)). Para revogar, passe esse token para a função de revogação. Nesse caso, o manipulador é estático, e (como você pode ver na listagem de códigos a seguir) a sintaxe da chamada de função é direta.

O tipo **object** aparece nas assinaturas do manipulador de eventos C#. No C#, o **object** é um [alias](/dotnet/csharp/language-reference/builtin-types/reference-types) para o tipo .NET [**System.Object**](/dotnet/api/system.object). O equivalente no C++/WinRT é [**winrt::Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable). Portanto, você verá **IInspectable** nos manipuladores de eventos C++/WinRT.

Edite `SampleConfiguration.h` e `SampleConfiguration.cpp` para corresponder às listagens abaixo.

```cppwinrt
// SampleConfiguration.h
...
private:
    static event_token clipboardContentChangedToken;
    static event_token activatedToken;
    static void OnClipboardChanged(Windows::Foundation::IInspectable const& sender, Windows::Foundation::IInspectable const& e);
    static void OnWindowActivated(Windows::Foundation::IInspectable const& sender, Windows::UI::Core::WindowActivatedEventArgs const& e);
...

// SampleConfiguration.cpp
...
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml;
...
event_token SampleState::clipboardContentChangedToken;
event_token SampleState::activatedToken;
...
bool SampleState::EnableClipboardContentChangedNotifications(bool enable)
{
    if (isClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled(enable);
    if (enable)
    {
        clipboardContentChangedToken = Clipboard::ContentChanged(OnClipboardChanged);
        activatedToken = Window::Current().Activated(OnWindowActivated);
    }
    else
    {
        Clipboard::ContentChanged(clipboardContentChangedToken);
        Window::Current().Activated(activatedToken);
    }
    return true;
}
void SampleState::OnClipboardChanged(IInspectable const&, IInspectable const&){}
void SampleState::OnWindowActivated(IInspectable const&, WindowActivatedEventArgs const& e){}
```

Deixe os próprios delegados de manipulação de eventos (**OnClipboardChanged** e **OnWindowActivated**) como stubs por enquanto. Eles já estão em nossa lista de membros para portar, portanto, nós os acessaremos em subseções posteriores.

#### <a name="onnavigatedto"></a>**OnNavigatedTo**

**OnNavigatedTo** é um método protegido da classe **MainPage** C# e está definido em `MainPage.xaml.cs`. Aqui está, junto com o **ListBox** XAML a que ele faz referência.

```xaml
<!-- MainPage.xaml -->
...
<ListBox x:Name="ScenarioControl" ... />
...
```

```csharp
// MainPage.xaml.cs
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Populate the scenario list from the SampleConfiguration.cs file
    var itemCollection = new List<Scenario>();
    int i = 1;
    foreach (Scenario s in scenarios)
    {
        itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
    }
    ScenarioControl.ItemsSource = itemCollection;

    if (Window.Current.Bounds.Width < 640)
    {
        ScenarioControl.SelectedIndex = -1;
    }
    else
    {
        ScenarioControl.SelectedIndex = 0;
    }
}
```

É um método importante e interessante, porque é nele que nossa coleção de objetos de **Scenario** é atribuída à interface do usuário. O código C# compila um [**System.Collections.Generic.List**](/dotnet/api/system.collections.generic.list-1) dos objetos de **Scenario** e atribui à propriedade [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) de uma **ListBox** (que é um controle de itens). E, no C#, usamos [interpolação de cadeia de caracteres](/dotnet/csharp/language-reference/tokens/interpolated) para criar o título para cada objeto de **Scenario** (observe o uso do caractere especial `$`).

No C++/WinRT, tornaremos **OnNavigatedTo** um método público de **MainPage**. E adicionaremos um elemento **ListBox** stub ao XAML para que um build seja bem-sucedido. Após a listagem do código, examinaremos alguns detalhes.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <ListBox x:Name="ScenarioControl" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
void OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
...

// MainPage.cpp
...
using namespace winrt::Windows::UI::Xaml;
using namespace winrt::Windows::UI::Xaml::Navigation;
...
void MainPage::OnNavigatedTo(NavigationEventArgs const& /* e */)
{
    auto itemCollection = winrt::single_threaded_observable_vector<IInspectable>();
    int i = 1;
    for (auto s : MainPage::scenarios())
    {
        s.Title = winrt::to_hstring(i++) + L") " + s.Title;
        itemCollection.Append(winrt::box_value(s));
    }
    ScenarioControl().ItemsSource(itemCollection);

    if (Window::Current().Bounds().Width < 640)
    {
        ScenarioControl().SelectedIndex(-1);
    }
    else
    {
        ScenarioControl().SelectedIndex(0);
    }
}
...
```

Novamente, estamos chamando a função [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector), porém, desta vez, para criar uma coleção de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Isso fez parte da decisão que tomamos de realizar a conversão boxing de nossos objetos de **Scenario** just-in-time.

E, no lugar do uso da [interpolação de cadeia de caracteres](/dotnet/csharp/language-reference/tokens/interpolated) do C# aqui, usamos uma combinação da função [**to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) e do [operador de concatenação](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator) do **winrt::hstring**.

#### <a name="isapplicationwindowactive"></a>**isApplicationWindowActive**

No C#, **isApplicationWindowActive** é um campo `bool` privado simples pertencente à classe **MainPage** e é definido em `SampleConfiguration.cs`. O padrão é `false`. No C++/WinRT, nós o tornaremos um campo estático público de **SampleState** (pelas razões que já descrevemos) nos campos `SampleConfiguration.h` e `SampleConfiguration.cpp`, com o mesmo padrão.

Já vimos como declarar, definir e inicializar um campo estático. Para um lembrete, volte ao que fizemos com o campo **isClipboardContentChangedEnabled** e faça o mesmo com **isApplicationWindowActive**.

#### <a name="needtoprintclipboardformat"></a>**needToPrintClipboardFormat**

Mesmo padrão que **isApplicationWindowActive** (confira o cabeçalho imediatamente antes deste).

#### <a name="button_click"></a>**Button_Click**

**Button_Click** é um método privado (manipulador de eventos) da classe **MainPage** do C# e é definido em `MainPage.xaml.cs`. Aqui está, junto com o **SplitView** XAML a que ele faz referência.

```xaml
<!-- MainPage.xaml -->
...
<SplitView x:Name="Splitter" ... />
...
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Splitter.IsPaneOpen = !Splitter.IsPaneOpen;
}
```

E o equivalente, portado para C++/WinRT. Observe que, na versão C++/WinRT, o manipulador de eventos é `public` (como vê, você o declara *antes* das declarações `private:`).

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <SplitView x:Name="Splitter" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
    void Button_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
void MainPage::Button_Click(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* e */)
{
    Splitter().IsPaneOpen(!Splitter().IsPaneOpen());
}
```

#### <a name="displaychangedformats"></a>**DisplayChangedFormats**

No C#, **DisplayChangedFormats** é um método privado que pertence à classe **MainPage** e é definido em `SampleConfiguration.cs`.

```csharp
private void DisplayChangedFormats()
{
    string output = "Clipboard content has changed!" + Environment.NewLine;
    output += BuildClipboardFormatsOutputString();
    NotifyUser(output, NotifyType.StatusMessage);
}
```

No C++/WinRT, nós o tornaremos um campo estático privado de **SampleState** (ele não acessa nenhum membro de instância) nos arquivos `SampleConfiguration.h` e `SampleConfiguration.cpp`. O código C# para esse método não usa **System.Text.StringBuilder**; mas emprega formatação de cadeia de caracteres o suficiente para que C++/WinRT use **std::wostringstream**.

Em vez da propriedade [**System.Environment.NewLine**](/dotnet/api/system.environment.newline) estática que é usada no código C#, inseriremos o padrão C++ `std::endl` (um caractere de nova linha) no fluxo de saída.

```cppwinrt
// SampleConfiguration.h
...
private:
    static void DisplayChangedFormats();
...

// SampleConfiguration.cpp
void SampleState::DisplayChangedFormats()
{
    std::wostringstream output;
    output << L"Clipboard content has changed!" << std::endl;
    output << BuildClipboardFormatsOutputString().c_str();
    MainPage::Current().NotifyUser(output.str(), NotifyType::StatusMessage);
}
```

Há uma pequena ineficiência no design da versão C++/WinRT acima. Primeiro, criamos um **std::wostringstream**. Mas também chamamos o método **BuildClipboardFormatsOutputString** (que portamos anteriormente). Esse método cria seu próprio **std::wostringstream**. E ele transforma seu fluxo em um **winrt::hstring** e retorna isso. Chamamos a função [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) para transformar essa **hstring** retornada em uma cadeia de caracteres no estilo C e, em seguida, nós a inserimos em nosso fluxo. Seria mais eficiente criar apenas um **std::wostringstream** e passá-lo (uma referência a ele), para que os métodos pudessem inserir cadeias de caracteres diretamente nele.

É o que fazemos na versão C++/WinRT do [código-fonte](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard/cppwinrt) da amostra de Clipboard. Nesse código-fonte, há um novo método estático privado chamado **SampleState::AddClipboardFormatsOutputString**, que utiliza e opera em uma referência a um fluxo de saída. E, então, os métodos **SampleState::DisplayChangedFormats** e **SampleState::BuildClipboardFormatsOutputString** são refatorados para chamar esse novo método. É funcionalmente equivalente às listagens de código deste tópico, mas é mais eficiente.

#### <a name="footer_click"></a>**Footer_Click**

**Footer_Click** é um manipulador de eventos assíncrono pertencente à classe C# **MainPage** e é definido em `MainPage.xaml.cs`. A lista de códigos abaixo é funcionalmente equivalente ao método no código-fonte que você baixou. Mas, aqui, ela foi descompactada de uma linha para quatro, a fim de facilitar a visualização do que está fazendo e, consequentemente, de como devemos portá-la.

```csharp
async void Footer_Click(object sender, RoutedEventArgs e)
{
    var hyperlinkButton = (HyperlinkButton)sender;
    string tagUrl = hyperlinkButton.Tag.ToString();
    Uri uri = new Uri(tagUrl);
    await Windows.System.Launcher.LaunchUriAsync(uri);
}
```

Embora, tecnicamente, o método seja assíncrono, ele não faz nada após `await`, portanto, não precisa de `await` (nem da palavra-chave `async`). Provavelmente, ele as utiliza para evitar a mensagem do IntelliSense no Visual Studio.

O método C++/WinRT equivalente também será assíncrono (porque chama [**Launcher.LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)). Mas ele não precisa de `co_await`, nem retornar um objeto assíncrono. Para saber mais sobre `co_await` e objetos assíncronos, confira [Simultaneidade e operações assíncronas com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

Agora, vamos falar sobre o que o método está fazendo. Como esse é um manipulador de eventos para o evento **Click** de um **HyperlinkButton**, o objeto denominado *sender* é, na verdade, um **HyperlinkButton**. Portanto, a conversão é segura (como alternativa, poderíamos ter expressado essa conversão como `sender as HyperlinkButton`). Em seguida, recuperamos o valor da propriedade **Tag** (se você observar a marcação XAML no projeto C#, verá que está definida como uma cadeia de caracteres que representa uma URL da Web). Embora a propriedade **FrameworkElement.Tag** (**HyperlinkButton** é um **FrameworkElement**) seja do tipo **objeto**, no C# podemos convertê-la em cadeia de caracteres com [**Object.ToString**](/dotnet/api/system.object.tostring). A partir da cadeia de caracteres resultante, construímos um objeto **Uri**. Por fim (com a ajuda do Shell), abrimos um navegador e navegamos até a URL.

Aqui está o método portado para C++/WinRT (novamente, expandido para maior clareza), após o qual está uma descrição dos detalhes.

```cppwinrt
// pch.h
...
#include "winrt/Windows.System.h"
...

// MainPage.h
...
    void Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
...
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::UI::Xaml::Controls;
...
void MainPage::Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const&)
{
    auto hyperlinkButton{ sender.as<HyperlinkButton>() };
    hstring tagUrl{ winrt::unbox_value<hstring>(hyperlinkButton.Tag()) };
    Uri uri{ tagUrl };
    Windows::System::Launcher::LaunchUriAsync(uri);
}
```

Como sempre, criamos o manipulador de eventos `public`. Usamos a função [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) no objeto *sender* para converter em **HyperlinkButton**. No C++/WinRT, a propriedade **Tag** é um [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (o equivalente de [**Object**](/dotnet/api/system.object)). Mas não há **ToString** em **IInspectable**. Em vez disso, precisamos fazer a conversão unboxing de **IInspectable** para um valor escalar (uma cadeia de caracteres, neste caso). Novamente, para saber mais sobre conversões boxing e unboxing, confira [Fazer conversão boxing e unboxing de valores escalares para IInspectable](/windows/uwp/cpp-and-winrt-apis/boxing).

As duas últimas linhas repetem os padrões de portabilidade que já vimos antes e praticamente copiam a versão C#.

#### <a name="handleclipboardchanged"></a>**HandleClipboardChanged**

Não há nada de novo envolvido na portabilidade desse método. Você pode comparar as versões C# e C++/WinRT no código-fonte da amostra.

#### <a name="onclipboardchanged-and-onwindowactivated"></a>**OnClipboardChanged** e **OnWindowActivated**

Até o momento, temos apenas stubs vazios para esses dois manipuladores de eventos. Mas a portabilidade é simples e não gera nada de novo a discutir.

#### <a name="scenariocontrol_selectionchanged"></a>**ScenarioControl_SelectionChanged**

Esse é outro manipulador de eventos privado pertencente à classe **MainPage** C# e definido em `MainPage.xaml.cs`. No C++/WinRT, nós o tornaremos público e o implementaremos em `MainPage.h` e `MainPage.cpp`.

Para esse método, precisaremos de **MainPage::navigating**, que é um campo booliano privado, inicializado como `false`. E você precisará de um **Frame** em `MainPage.xaml`, chamado *ScenarioFrame*. Mas, além desses detalhes, a portabilidade desse método não revela novas técnicas.

#### <a name="updatestatus"></a>**UpdateStatus**

Até o momento, temos apenas um stub para **MainPage.UpdateStatus**. A portabilidade da implementação, novamente, cobre uma ampla área. Um novo ponto a ser observado é que, enquanto no C# podemos comparar uma **string** com **String.Empty**, no C++/WinRT chamamos a função [**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function). Outro ponto é que `nullptr` é o equivalente padrão no C++ de `null` no C#.

Você pode executar o restante da portabilidade com técnicas que já abordamos. Aqui está uma lista dos tipos de ações que você precisará realizar antes que a versão portada desse método seja compilada.

- Para `MainPage.xaml`, adicione um **Border** denominado *StatusBorder*.
- Para `MainPage.xaml`, adicione um **TextBlock** denominado *StatusBlock*.
- Para `MainPage.xaml`, adicione um **StackPanel** denominado *StatusPanel*.
- Para `pch.h`, adicione `#include "winrt/Windows.UI.Xaml.Media.h"`.
- Para `pch.h`, adicione `#include "winrt/Windows.UI.Xaml.Automation.Peers.h"`.
- Para `MainPage.cpp`, adicione `using namespace winrt::Windows::UI::Xaml::Media;`.
- Para `MainPage.cpp`, adicione `using namespace winrt::Windows::UI::Xaml::Automation::Peers;`.

### <a name="copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage"></a>Copiar o XAML e os estilos necessários para concluir a portabilidade de **MainPage**

Para o XAML, o ideal é usar *a mesma* marcação XAML em um projeto C# e em um projeto C++/WinRT. E a amostra de Clipboard consiste em um desses casos.

Em seu arquivo `Styles.xaml`, a amostra de Clipboard tem um XAML **ResourceDictionary** de estilos, que são aplicados aos botões, menus e outros elementos da interface do usuário do aplicativo. A página `Styles.xaml` é mesclada em `App.xaml`. E há o ponto de partida padrão `MainPage.xaml` para a interface do usuário, que já vimos brevemente. Agora, podemos reutilizar esses três arquivos `.xaml`, inalterados, na versão C++/WinRT do projeto.

Assim como nos arquivos de ativos, você pode optar por fazer referência aos mesmos arquivos XAML compartilhados de várias versões do seu aplicativo. Neste passo a passo, por uma questão de simplicidade, copiaremos os arquivos no projeto C++/WinRT e os adicionaremos dessa maneira.

Navegue para a pasta `\Clipboard_sample\SharedContent\xaml`, selecione e copie `App.xaml` e `MainPage.xaml` e cole esses dois arquivos na pasta `\Clipboard\Clipboard` no seu projeto C++/WinRT, optando por substituir arquivos quando solicitado.

Adicione uma nova pasta ao projeto C++/WinRT, imediatamente abaixo do nó do projeto, e denomine `Styles`. Navegue até a pasta `\Clipboard_sample\SharedContent\xaml`, selecione e copie `Styles.xaml` e cole na pasta `\Clipboard\Clipboard\Styles` em seu projeto C++/WinRT. Clique com o botão direito do mouse na pasta `Styles` (no Gerenciador de Soluções no projeto C++/WinRT) > **Adicionar** > **Item existente...** e navegue para `\Clipboard\Clipboard\Styles`. No seletor de arquivos, selecione `Styles` e clique em **Adicionar**.

Agora terminamos de portar **MainPage** e, se você acompanhou as etapas, seu projeto C++/WinRT será compilado e executado.

## <a name="consolidate-your-idl-files"></a>Consolidar seus arquivos `.idl`

Além do ponto de partida `MainPage.xaml` padrão da interface do usuário, o exemplo de Área de Transferência tem cinco outras páginas XAML específicas do cenário, juntamente com os arquivos code-behind correspondentes. Vamos usar novamente a marcação XAML real de todas essas páginas, inalteradas, na versão do C++/WinRT do projeto. E examinaremos como portar o code-behind nas próximas seções principais. Mas, antes disso, vamos falar sobre IDL.

Há um valor na consolidação de suas classes de runtime em um único arquivo IDL (confira [Fatorar classes de runtime em arquivos Midl (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)). Em seguida, consolidaremos o conteúdo de `CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` e `OtherScenarios.idl` movendo essa IDL para um único arquivo chamado `Project.idl` (e, em seguida, excluindo os arquivos originais).

Enquanto fazemos isso, vamos remover também a propriedade fictícia gerada automaticamente (`Int32 MyProperty;` e a implementação dela) de cada um desses cinco tipos de página XAML.

Primeiro, adicione um novo item **Arquivo Midl (.idl)** ao projeto do C++/WinRT. Nomeie-o como `Project.idl`. Exclua o conteúdo padrão de `Project.idl` e, no lugar, cole a listagem abaixo.

```idl
// Project.idl
namespace SDKTemplate
{
    [default_interface]
    runtimeclass CopyFiles : Windows.UI.Xaml.Controls.Page
    {
        CopyFiles();
    }

    [default_interface]
    runtimeclass CopyImage : Windows.UI.Xaml.Controls.Page
    {
        CopyImage();
    }

    [default_interface]
    runtimeclass CopyText : Windows.UI.Xaml.Controls.Page
    {
        CopyText();
    }

    [default_interface]
    runtimeclass HistoryAndRoaming : Windows.UI.Xaml.Controls.Page
    {
        HistoryAndRoaming();
    }

    [default_interface]
    runtimeclass OtherScenarios : Windows.UI.Xaml.Controls.Page
    {
        OtherScenarios();
    }
}
```

Como você pode ver, essa é apenas uma cópia do conteúdo dos arquivos `.idl` individuais, tudo dentro de um namespace e com `MyProperty` removido de cada classe de runtime.

No Gerenciador de Soluções no Visual Studio, selecione vários arquivos IDL originais (`CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` e `OtherScenarios.idl`) e **Edite-os** > **Remova-os** (escolha **Excluir** na caixa de diálogo).

Por fim,&mdash;e para concluir a remoção de `MyProperty`&mdash;nos arquivos `.h` e `.cpp` para cada um dos cinco tipos de página XAML, exclua as declarações e definições das funções de acessador `int32_t MyProperty()` e de modificador `void MyProperty(int32_t)`.

## <a name="copyfiles"></a>**CopyFiles**

No projeto do C#, o tipo de página XAML **CopyFiles** é implementado nos arquivos de código-fonte `CopyFiles.xaml` e `CopyFiles.xaml.cs`. Vamos examinar cada um dos membros de **CopyFiles**, por sua vez.

### <a name="rootpage"></a>**rootPage**

Esse é um campo privado.

```csharp
// CopyFiles.xaml.cs
...
public sealed partial class CopyFiles : Page
{
    MainPage rootPage = MainPage.Current;
    ...
}
...
```

No C++/WinRT, podemos definir e inicializá-lo como este.

```cppwinrt
// CopyFiles.h
...
struct CopyFiles : CopyFilesT<CopyFiles>
{
    ...
private:
    SDKTemplate::MainPage rootPage{ MainPage::Current() };
};
...
```

Novamente (assim como acontece com **MainPage::current**), **CopyFiles::rootPage** é declarado como sendo do tipo **SDKTemplate::MainPage**, que é o tipo projetado, e não o tipo de implementação.

### <a name="copyfiles-the-constructor"></a>**CopyFiles** (o construtor)

No projeto do C++/WinRT, o tipo **CopyFiles** já tem um construtor que contém o código desejado (ele apenas chama **InitializeComponent**).

### <a name="copybutton_click"></a>**CopyButton_Click**

O método **CopyButton_Click** do C# é um manipulador de eventos e, na palavra-chave `async` na assinatura, podemos dizer que o método faz o trabalho assíncrono. No C++/WinRT, implementamos um método assíncrono como uma *corrotina*. Para obter uma introdução à simultaneidade no C++/WinRT, junto com uma descrição do que é uma *corrotina*, confira [Simultaneidade e operações assíncronas com o C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

É comum querer agendar mais trabalho após a conclusão de uma corrotina e, para tais casos, a corrotina retornaria um tipo de objeto assíncrono que pode ser esperado e que, opcionalmente, relata o progresso. Porém, essas considerações normalmente não se aplicam a um manipulador de eventos. Assim, quando você tiver um manipulador de eventos que execute operações assíncronas, poderá implementá-lo como uma corrotina que retorne **winrt::fire_and_forget**. Para obter mais informações, confira [Disparar e esquecer](/windows/uwp/cpp-and-winrt-apis/concurrency-2#fire-and-forget).

Embora a ideia de uma corrotina do tipo disparar e esquecer seja você não se importar com a conclusão, o trabalho ainda continua (ou é suspenso, aguardando a continuação) em segundo plano. Você pode ver na implementação do C# que **CopyButton_Click** depende do ponteiro `this` (ele acessa o membro de dados de instância `rootPage`). Portanto, devemos ter certeza de que o ponteiro `this` (um ponteiro para um objeto **CopyFiles**) dura mais que a corrotina **CopyButton_Click**. Em uma situação como a desse aplicativo de exemplo, em que o usuário navega entre páginas de interface do usuário, não podemos controlar diretamente o tempo de vida dessas páginas. Se a página **CopyFiles** for destruída (saindo dela) enquanto **CopyButton_Click** ainda estiver em trânsito em um thread em segundo plano, não será seguro acessar `rootPage`. Para tornar a corrotina correta, ela precisa obter uma referência forte ao ponteiro `this` e manter essa referência durante a corrotina. Para obter mais informações, confira [Referências fortes e fracas no C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references).

Se você examinar a versão do C++/WinRT do exemplo, em **CopyFiles::CopyButton_Click**, verá que isso é feito com uma declaração simples na pilha.

```cppwinrt
fire_and_forget CopyFiles::CopyButton_Click(IInspectable const&, RoutedEventArgs const&)
{
    auto lifetime{ get_strong() };
    ...
}
```

Vamos examinar os outros aspectos notáveis do código portado.

No código, criamos uma instância de um objeto [**FileOpenPicker**](/uwp/api/windows.storage.pickers.fileopenpicker) e, duas linhas depois, acessamos a propriedade [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) do objeto. O tipo retornado dessa propriedade implementa um **IVector** de cadeias de caracteres. E, nesse **IVector**, chamamos o método [IVector<T>.ReplaceAll(T[])](/uwp/api/windows.foundation.collections.ivector-1.replaceall). O aspecto interessante é o valor que estamos passando para esse método, em que espera-se uma matriz. Veja a linha de código.

```cppwinrt
filePicker.FileTypeFilter().ReplaceAll({ L"*" });
```

O valor que estamos passando (`{ L"*" }`) é uma *lista de inicializadores* do C++ padrão. Ele contém um único objeto, nesse caso, mas uma lista de inicializadores pode conter qualquer número de objetos separados por vírgulas. As partes de C++/WinRT que oferecem a você a conveniência de passar uma lista de inicializadores para um método como esse são explicadas em [Listas de inicializadores padrão](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists).

Portamos a palavra-chave `await` do C# para `co_await` no C++/WinRT. Veja o exemplo do código.

```cppwinrt
auto storageItems{ co_await filePicker.PickMultipleFilesAsync() };
```

Em seguida, considere esta linha de código C#.

```csharp
dataPackage.SetStorageItems(storageItems);
```

O C# é capaz de converter implicitamente o **IReadOnlyList<StorageFile>** representado por *storageItems* no **IEnumerable<IStorageItem>** esperado por [**DataPackage.SetStorageItems**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.setstorageitems). Contudo, no C++/WinRT, precisamos converter explicitamente de **IVectorView<StorageFile>** em **IIterable<IStorageItem>** . Além disso, temos outro exemplo da função [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) em ação.

```cppwinrt
dataPackage.SetStorageItems(storageItems.as<IVectorView<IStorageItem>>());
```

Quando usamos a palavra-chave `null` C# em (por exemplo, `Clipboard.SetContentWithOptions(dataPackage, null)`), usamos `nullptr` no C++/WinRT (por exemplo, `Clipboard::SetContentWithOptions(dataPackage, nullptr)`).

### <a name="pastebutton_click"></a>**PasteButton_Click**

Esse é outro manipulador de eventos na forma de uma corrotina do tipo disparar e esquecer. Vamos examinar os aspectos notáveis do código portado.

Na versão do C# do exemplo, capturamos exceções com `catch (Exception ex)`. No código C++/WinRT portado, você verá a expressão `catch (winrt::hresult_error const& ex)`. Para obter mais informações sobre [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) e como trabalhar com ele, confira [Tratamento de erro com o C++/WinRT](/windows/uwp/cpp-and-winrt-apis/error-handling).

Um exemplo de testar se um objeto do C# é `null` ou não é `if (storageItems != null)`. No C++/WinRT, podemos contar com um operador de conversão para `bool`, que faz o teste em relação a `nullptr` internamente.

Veja uma versão ligeiramente simplificada de um fragmento de código da versão do C++/WinRT portada do exemplo.

```cppwinrt
std::wostringstream output;
output << std::wstring_view(ApplicationData::Current().LocalFolder().Path());
```

Construir um **std::wstring_view** de um **winrt::hstring** como esse ilustra uma alternativa para chamar a função [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) (para converter o **winrt::hstring** em uma cadeia de caracteres no estilo C). Essa alternativa funciona graças ao [operador de conversão **do **hstring** para std::wstring_view**](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view).

Considere este fragmento do C#.

```csharp
var file = storageItem as StorageFile;
if (file != null)
...
```

Para portar a palavra-chave `as` do C# para o C++/WinRT, até agora vimos a função [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) usada algumas vezes. Essa função vai gerar uma exceção se a conversão falhar. No entanto, se desejarmos que a conversão retorne `nullptr` se ela falhar (para que possamos lidar com essa condição no código), em vez disso, usaremos a função [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function).

```cppwinrt
auto file{ storageItem.try_as<StorageFile>() };
if (file)
...
```

### <a name="copy-the-xaml-necessary-to-finish-up-porting-copyfiles"></a>Copiar o XAML necessário para concluir a portabilidade de **CopyFiles**

Agora você pode selecionar todo o conteúdo do arquivo `CopyFiles.xaml` do projeto C# e colá-lo no arquivo `CopyFiles.xaml` no projeto do C++/WinRT (substituindo o conteúdo existente desse arquivo no projeto do C++/WinRT).

Por fim, edite `CopyFiles.h` e `.cpp` e exclua a função fictícia **ClickHandler**, já que acabamos de substituir a marcação XAML correspondente.

Agora terminamos de portar **CopyFiles** e, se você tiver acompanhado as etapas, então seu projeto do C++/WinRT será criado e executado e o cenário **CopyFiles** estará funcional.

## <a name="copyimage"></a>**CopyImage**

Para portar o tipo de página XAML **CopyImage**, siga o mesmo processo que para **CopyFiles**. Ao portar **CopyImage**, você encontrará o uso da [*instrução using*](/dotnet/csharp/language-reference/keywords/using-statement) do C#, que verifica se os objetos que implementam a interface [**IDisposable**](/dotnet/api/system.idisposable) estão dispostos corretamente.

```csharp
if (imageReceived != null)
{
    using (var imageStream = await imageReceived.OpenReadAsync())
    {
        ... // Pass imageStream to other APIs, and do other work.
    }
}
```

A interface equivalente no C++/WinRT é [**IClosable**](/uwp/api/windows.foundation.iclosable), com o único método **Close**. Veja o C++/WinRT equivalente do C# código acima.

```cppwinrt
if (imageReceived)
{
    auto imageStream{ co_await imageReceived.OpenReadAsync() };
    ... // Pass imageStream to other APIs, and do other work.
    imageStream.Close();
}
```

Os objetos do C++/WinRT implementam **IClosable** principalmente para o benefício de linguagens que não têm finalização determinística. O C++/WinRT tem finalização determinística; portanto, não precisamos chamar **IClosable::Close** com frequência quando estamos escrevendo em C++/WinRT. Todavia, há ocasiões em que é bom chamá-lo, e essa é uma delas. Aqui, o identificador *imageStream* é um wrapper contado por referência em um objeto do Windows Runtime subjacente (nesse caso, um objeto que implementa [**IRandomAccessStreamWithContentType**](/uwp/api/windows.storage.streams.irandomaccessstreamwithcontenttype)). Embora possamos determinar que o finalizador de *imageStream* (o destruidor dele) será executado no final do escopo delimitador (as chaves), não podemos ter certeza de que esse finalizador chamará **Fechar**. Isso é porque passamos *imageStream* a outras APIs, e elas ainda podem estar contribuindo para a contagem de referência do objeto do Windows Runtime subjacente. Portanto, esse é um caso em que é uma boa ideia chamar **Close** explicitamente. Para obter mais informações, confira [É necessário chamar IClosable::Close nas classes de runtime que eu consumo?](/windows/uwp/cpp-and-winrt-apis/faq#do-i-need-to-call-iclosableclose-on-runtime-classes-that-i-consume).

Em seguida, considere a expressão `(uint)(imageDecoder.OrientedPixelWidth * 0.5)` em C#, que você encontrará no manipulador de eventos **OnDeferredImageRequestedHandler**. Essa expressão multiplica uma `uint` por um `double`, resultando em uma `double`. Em seguida, ela converte isso em um `uint`. No C++/WinRT, *poderíamos* usar uma conversão de estilo C (`(uint32_t)(imageDecoder.OrientedPixelWidth() * 0.5)`) semelhante, mas é preferível esclarecer exatamente o tipo de conversão que pretendemos e, nesse caso, faríamos isso com `static_cast<uint32_t>(imageDecoder.OrientedPixelWidth() * 0.5)`.

A versão do C# de **CopyImage.OnDeferredImageRequestedHandler** tem uma cláusula `finally`, mas não uma cláusula `catch`. Aprofundamo-nos um pouco mais na versão do C++/WinRT e implementamos uma cláusula `catch` para podermos relatar se a renderização atrasada foi bem-sucedida ou não.

A portabilidade do restante desta página XAML não produz nada de novo para ser discutido. Assim como ocorreu com **CopyFiles**, a última etapa da portabilidade é selecionar todo o conteúdo de `CopyImage.xaml` e colá-lo no mesmo arquivo no projeto do C++/WinRT.

## <a name="copytext"></a>**CopyText**

Você pode portar `CopyText.xaml` e `CopyText.xaml.cs` usando técnicas que já abordamos.

## <a name="historyandroaming"></a>**HistoryAndRoaming**

Há alguns pontos de interesse que surgem ao portar o tipo de página XAML **HistoryAndRoaming**.

Primeiro, examine o código-fonte C# e siga o fluxo de controle de **OnNavigatedTo** até o manipulador de eventos **OnHistoryEnabledChanged** e finalmente até a função assíncrona **CheckHistoryAndRoaming** (que não é esperada, portanto, é basicamente disparar e esquecer). Como **CheckHistoryAndRoaming** é assíncrono, precisaremos ter cuidado com o tempo de vida do ponteiro `this` no C++/WinRT. Você poderá ver o resultado se examinar a implementação no arquivo de código-fonte `HistoryAndRoaming.cpp`. Primeiro, quando anexamos representantes aos eventos **Clipboard::HistoryEnabledChanged** e **Clipboard::RoamingEnabledChanged**, usamos apenas uma referência fraca ao objeto da página **HistoryAndRoaming**. Fazemos isso criando o representante com uma dependência do valor retornado de [**winrt::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function), em vez de uma dependência do ponteiro `this`. Isso significa que o próprio delegado, que eventualmente chama o código assíncrono, não manterá a página **HistoryAndRoaming** ativa se sairmos dela.

E, em segundo lugar, quando finalmente atingirmos nossa corrotina **CheckHistoryAndRoaming** do tipo disparar e esquecer, a primeira coisa que deveremos fazer é realizar uma referência forte a `this` para garantir que a página **HistoryAndRoaming** sobreviva pelo menos até que a corrotina seja finalmente concluída. Para obter mais informações sobre os dois aspectos que acabamos de descrever, confira [Referências fortes e fracas no C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references).

Encontramos outro ponto de interesse ao portar **CheckHistoryAndRoaming**. Ele contém o código para atualizar a interface do usuário; portanto, precisamos ter certeza de que estamos fazendo isso no thread da IU principal. Normalmente, um método assíncrono pode ser executado e/ou retomado em qualquer thread arbitrário. No C#, a solução é chamar [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) e atualizar a interface do usuário de dentro da função lambda. No C++/WinRT, podemos usar a função [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) em conjunto com o [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) do ponteiro `this` para suspender a corrotina e retomar imediatamente o thread da IU principal.

A expressão relevante é `co_await winrt::resume_foreground(Dispatcher());`. Como alternativa, embora com menos clareza, poderíamos expressar isso de maneira tão simples quanto `co_await Dispatcher();`. A versão mais curta é alcançada como cortesia de um operador de conversão fornecido pelo C++/WinRT.

## <a name="otherscenarios"></a>**OtherScenarios**

Você pode portar `OtherScenarios.xaml` e `OtherScenarios.xaml.cs` usando técnicas que já abordamos.

## <a name="conclusion"></a>Conclusão

Este passo a passo forneceu informações e técnicas de portabilidade suficientes para que agora você possa prosseguir e portar seus próprios aplicativos em C# para o C++/WinRT. Por meio de um atualizador, você pode continuar consultando as versões anteriores (C#) e posteriores (C++/WinRT) do código-fonte no exemplo da Área de Transferência e compará-las lado a lado para ver a correspondência.

## <a name="related-topics"></a>Tópicos relacionados
* [Mover do C# para C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp)
---
description: Este tópico orienta você pelas etapas de criação de um controle personalizado simples usando C++/WinRT. Você pode usar as informações aqui como base para criar seus próprios controles de interface do usuário personalizáveis e ricos em recursos.
title: Controles personalizados XAML (modelos) com C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, personalização, modelo, controle
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c0b2d8fb17b90bc55834f6bf2200b22af9352ef6
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270074"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Controles personalizados XAML (modelos) com C++/WinRT

> [!IMPORTANT]
> Para ver conceitos e termos essenciais que ajudam a entender como utilizar e criar classes de tempo de execução com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), confira [Utilizar APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

Um dos recursos mais poderosos da Plataforma Universal do Windows (UWP) é a flexibilidade que a pilha da interface do usuário (IU) fornece para criar controles personalizados com base no tipo [**Control**](/uwp/api/windows.ui.xaml.controls.control) de XAML. A estrutura de interface do usuário do XAML fornece recursos como [propriedades de dependência personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties), [propriedades anexadas](/windows/uwp/xaml-platform/custom-attached-properties) e [modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates), que facilitam a criação de controles com recursos sofisticados e personalizáveis. Este tópico o orienta pelas etapas de criação de um controle personalizado (modelo) com C++/WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Criar um aplicativo em branco (BgLabelControlApp)
Comece criando um novo projeto no Microsoft Visual Studio. Crie um projeto **Aplicativo em Branco (C++/WinRT)** e nomeie-o como *BgLabelControlApp*. Em uma seção posterior deste tópico, você será direcionado para compilar seu projeto (não compile até lá).

Vamos criar uma nova classe para representar um controle personalizado (modelo). Criaremos e utilizaremos a classe dentro da mesma unidade de compilação. Entretanto, queremos poder instanciar essa classe pela marcação do XAML e, por isso, ela será uma classe de tempo de execução. E usaremos o C++/WinRT para criar e usar.

A primeira etapa na criação de uma nova classe de tempo de execução é adicionar um novo item **Midl File (.idl)** ao projeto. Nomeie-o como `BgLabelControl.idl`. Exclua o conteúdo padrão do `BgLabelControl.idl` e cole esta declaração de classe de tempo de execução.

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

A listagem acima mostra o padrão a seguir ao declarar uma propriedade de dependência (DP). Há duas partes para cada DP. Primeiro, declare uma propriedade estática somente leitura do tipo [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Ela tem o nome da sua DP e *Property*. Você usará essa propriedade estática em sua implementação. Em seguida, declare uma propriedade de instância de leitura/gravação com o tipo e o nome da sua DP. Se você quiser criar uma *propriedade anexada* (ao invés de uma DP), confira os exemplos de código [Propriedades anexadas personalizadas](/windows/uwp/xaml-platform/custom-attached-properties).

> [!NOTE]
> Se quiser uma DP com um tipo de ponto flutuante, crie-a como `double` (`Double` no [MIDL 3.0](/uwp/midl-3/)). Declarar e implementar uma DP do tipo `float` (`Single` em MIDL) e, em seguida, definir um valor para essa DP na marcação XAML resulta no erro *Falha ao criar “Windows.Foundation.Single” do texto “<NUMBER>”* .

Salve o arquivo e compile o projeto. Durante o processo de compilação, a ferramenta `midl.exe` é executada para criar um arquivo de metadados do Windows Runtime (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`), descrevendo a classe de tempo de execução. Em seguida, a ferramenta `cppwinrt.exe` é executada para gerar arquivos de código fonte para dar suporte à criação e utilização da classe de tempo de execução. Esses arquivos incluem stubs para ajudá-lo a começar a implementar a classe de tempo de execução **BgLabelControl**, que foi declarada em seu IDL. Esses stubs são `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` e `BgLabelControl.cpp`.

Copie os arquivos stub `BgLabelControl.h` e `BgLabelControl.cpp` de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` para a pasta do projeto, que é `\BgLabelControlApp\BgLabelControlApp\`. No **Gerenciador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Clique com o botão direito do mouse nos arquivos de stub copiados e clique em **Incluir no Projeto**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implementar a classe personalizada **BgLabelControl**
Agora vamos abrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` e `BgLabelControl.cpp`, e implementar nossa classe de tempo de execução. Em `BgLabelControl.h`, altere o construtor para definir a chave de estilo padrão, implemente **Label** e **LabelProperty**, adicione um manipulador de evento estático chamado **OnLabelChanged** para processar as alterações no valor da propriedade de dependência e adicione um membro privado para armazenar o campo de suporte para **LabelProperty**.

Após a inclusão desses controles, `BgLabelControl.h` terá a seguinte aparência.

```cppwinrt
// BgLabelControl.h
...
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
    BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

    winrt::hstring Label()
    {
        return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
    }

    void Label(winrt::hstring const& value)
    {
        SetValue(m_labelProperty, winrt::box_value(value));
    }

    static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const&, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const&);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

Em `BgLabelControl.cpp`, defina os membros estáticos da seguinte maneira.

```cppwinrt
// BgLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
);

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // Call members of the projected type via theControl.

        BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
        // Call members of the implementation type via ptr.
    }
}
...
```

Neste passo a passo, nós não utilizaremos **OnLabelChanged**. Entretanto, ele continua lá para que você possa ver como registrar uma propriedade de dependência com um retorno de chamada de propriedade alterada. A implementação de **OnLabelChanged** também mostra como obter um tipo projetado derivado de um tipo projetado básico (que é o tipo **DependencyObject** nesse caso). E mostra como obter um ponteiro para o tipo que implementa o tipo projetado. Naturalmente, essa segunda operação só é possível no projeto que implementa o tipo projetado (ou seja, o projeto que implementa a classe de tempo de execução).

> [!NOTE]
> Se você ainda não instalou o SDK do Windows, versão 10.0.17763.0 (Windows 10, versão 1809) ou posterior, precisará chamar [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) no manipulador de evento da propriedade alterada de dependência ao invés de [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

## <a name="design-the-default-style-for-bglabelcontrol"></a>Definir o estilo padrão de design para **BgLabelControl**

Em seu construtor, **BgLabelControl** define uma chave de estilo padrão para si mesmo. O que *é* um estilo padrão? Um controle personalizado (modelo) precisa ter um estilo padrão (com um modelo de controle padrão), que pode ser usado para renderizar a si mesmo no caso do consumidor do controle não definir um estilo e/ou modelo. Nesta seção, adicionaremos um arquivo de marcação ao projeto que contém nosso estilo padrão.

No nó do projeto, crie uma nova pasta e nomeie-a como "Temas". Em `Themes`, adicione um novo item do tipo **Visual C++**   >  **XAML** > **Exibição XAML** e nomeie-o como "Generic.xaml". Os nomes de pasta e arquivo precisam ser dessa maneira para que a estrutura XAML localize o estilo padrão para um controle personalizado. Exclua o conteúdo padrão do `Generic.xaml` e cole na marcação abaixo.

```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

Nesse caso, a única propriedade que o estilo padrão define é o modelo de controle. O modelo consiste em um quadrado (cujo plano de fundo está associado à propriedade **Background** que todas as instâncias do tipo [**Control**](/uwp/api/windows.ui.xaml.controls.control) de XAML têm) e um elemento de texto (cujo texto está associado à propriedade de dependência **BgLabelControl::Label**).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Adicionar uma instância de **BgLabelControl** à página da interface do usuário principal

Abra `MainPage.xaml`, que contém a marcação XAML para a página principal da interface do usuário. Imediatamente após o elemento **Button** (dentro de **StackPanel**), adicione a seguinte marcação.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Além disso, adicione a seguinte diretiva de inclusão à `MainPage.h` para que o tipo **MainPage** (uma combinação de marcação de compilação de XAML e código imperativo) esteja ciente do tipo de controle personalizado **BgLabelControl**. Se você quiser usar **BgLabelControl** de outra página XAML, adicione essa mesma diretiva de inclusão ao arquivo de cabeçalho para essa página também. Ou, você pode apenas colocar uma única diretiva de inclusão em seu arquivo de cabeçalho pré-compilado.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Agora compile e execute o projeto. Você verá que o modelo de controle padrão está associado ao pincel do plano de fundo e ao rótulo da instância **BgLabelControl** na marcação.

Este passo a passo mostrou um exemplo simples de um controle personalizado (modelo) no C++/WinRT. Você pode criar seus próprios controles personalizados avançados e completos. Por exemplo, um controle personalizado pode assumir a forma de algo mais complicado, como uma grade de dados editável, um player de vídeo ou um visualizador de geometria 3D.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementando funções *substituíveis*, como **MeasureOverride** e **OnApplyTemplate**

É possível derivar um controle personalizado da classe de tempo de execução [**Control**](/uwp/api/windows.ui.xaml.controls.control), que por si só deriva de classes básicas de tempo de execução. E há métodos substituíveis de **Control**, [**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement), e [**UIElement**](/uwp/api/windows.ui.xaml.uielement) que você pode substituir em sua classe derivada. Aqui está um exemplo de código mostrando como você pode fazer isso.

```cppwinrt
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
...
    // Control overrides.
    void OnPointerPressed(Windows::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

    // FrameworkElement overrides.
    Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
    void OnApplyTemplate() const { ... };

    // UIElement overrides.
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
...
};
```

Funções *substituíveis* apresentam-se diferentemente em projeções de linguagem diferentes. Em C#, por exemplo, as funções substituíveis normalmente são exibidas como funções virtuais protegidas. Em C++/WinRT, elas não são virtuais nem protegidas, mas é possível substituí-las e fornecer sua própria implementação, conforme mostrado acima.

## <a name="important-apis"></a>APIs Importantes
* [Classe Control](/uwp/api/windows.ui.xaml.controls.control)
* [Classe DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)
* [Classe FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)
* [Classe UIElement](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Tópicos relacionados
* [Modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propriedades de dependência personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties)

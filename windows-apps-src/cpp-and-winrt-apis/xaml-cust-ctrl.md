---
description: Este tópico orienta você pelas etapas de criação de um controle personalizado simples usando C++/WinRT. Você pode usar as informações aqui como base para criar seus próprios controles de interface do usuário personalizáveis e ricos em recursos.
title: Controles personalizados (modelos) XAML com C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, projeção, XAML, controle personalizado, modelo,
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ce4f7eea074233c625a2cc92ef773f0b06c2be9f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635141"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Controles personalizados (modelos) XAML com C++/WinRT

> [!IMPORTANT]
> Para termos que dão suporte a sua compreensão de como consumir e criar classes de tempo de execução com e conceitos essenciais [C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), consulte [consumir APIs com C + + c++ /CLI WinRT](consume-apis.md) e [APIs de autor com C + + c++ /CLI WinRT](author-apis.md).

Um dos recursos mais poderosos do Universal Windows Platform (UWP) é a flexibilidade que a pilha de interface do usuário (IU) fornece para criar controles personalizados com base no XAML [ **controle** ](/uwp/api/windows.ui.xaml.controls.control) tipo. A estrutura XAML UI fornece recursos como [propriedades de dependência personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties) e [propriedades anexadas](/windows/uwp/xaml-platform/custom-attached-properties), e [modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates), que tornam fácil criar controles de recursos sofisticados e personalizáveis. Este tópico orienta você pelas etapas de criação de um controle (modelo) personalizado com C + + c++ /CLI WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Criar um aplicativo em branco (BgLabelControlApp)
Comece criando um novo projeto no Microsoft Visual Studio. Criar uma **Visual C++** > **Windows Universal** > **aplicativo em branco (C + + c++ /CLI WinRT)** de projeto e nomeie- *BgLabelControlApp* . Em uma seção posterior deste tópico, você será direcionado para compilar seu projeto (não crie até lá).

Vamos criar uma nova classe para representar um controle (modelo) personalizado. Estamos criando e consumindo a classe dentro da mesma unidade de compilação. Mas queremos poder criar uma instância dessa classe da marcação XAML e para que ele vai ser uma classe de tempo de execução do motivo. E vamos usar C++/WinRT para criar e consumi-lo.

A primeira etapa na criação de uma nova classe de tempo de execução é adicionar um novo item **Midl File (.idl)** ao projeto. Nomeie-o `BgLabelControl.idl`. Exclua o conteúdo padrão do `BgLabelControl.idl` e cole nessa declaração de classe de tempo de execução.

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

A listagem acima mostra o padrão que você segue ao declarar uma propriedade de dependência (DP). Há duas partes para cada ponto de distribuição. Primeiro, você pode declarar uma propriedade estática somente leitura do tipo [ **DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Ele tem o nome do seu ponto de distribuição de adição *propriedade*. Você usará essa propriedade estática em sua implementação. Em segundo lugar, você deve declarar uma propriedade de instância de leitura / gravação com o tipo e o nome do seu ponto de distribuição. Se você quiser criar uma *propriedade anexada* (em vez de um ponto de distribuição), em seguida, consulte os exemplos de código [propriedades anexadas personalizadas](/windows/uwp/xaml-platform/custom-attached-properties).

> [!NOTE]
> Se você quiser um ponto de distribuição com um tipo de ponto flutuante, em seguida, torná-lo `double` (`Double` na [3.0 MIDL](/uwp/midl-3/)). Declarar e implementar um ponto de distribuição do tipo `float` (`Single` em MIDL), e, em seguida, definir um valor para esse ponto de distribuição na marcação XAML, resulta no erro *Falha ao criar 'Windows.Foundation.Single' do texto '<NUMBER>'*.

Salve o arquivo e compile o projeto. Durante o processo de compilação, a ferramenta `midl.exe` é executada para criar um arquivo de metadados do componente do Tempo de Execução do Windows (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`), descrevendo a classe de tempo de execução. Em seguida, a ferramenta `cppwinrt.exe` é executada para gerar arquivos de código fonte para dar suporte a você na criação e no consumo da classe de tempo de execução. Esses arquivos incluem stubs para você começar a implementar o **BgLabelControl** classe de tempo de execução que você declarou em seu IDL. Esses stubs são `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` e `BgLabelControl.cpp`.

Copie os arquivos stub `BgLabelControl.h` e `BgLabelControl.cpp` de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` na pasta do projetor, que é `\BgLabelControlApp\BgLabelControlApp\`. No **Gerenciador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Clique nos arquivos stub com o botão direito do mouse e clique em **Incluir no projeto**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implemente a **BgLabelControl** classe de controle personalizado
Agora, vamos abrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` e `BgLabelControl.cpp` e implementar nossa classe de tempo de execução. Na `BgLabelControl.h`, altere o construtor para definir o implemente de chave, de estilo padrão **rótulo** e **LabelProperty**, adicionar um manipulador de evento estático chamado **OnLabelChanged** para processar alterações no valor da propriedade de dependência e adicionar um membro particular para armazenar o campo de suporte para **LabelProperty**.

Depois de adicionar os, seu `BgLabelControl.h` se parece com isto.

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

No `BgLabelControl.cpp`, definir os membros estáticos como este.

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

Neste passo a passo, nós não o utilizarem **OnLabelChanged**. Mas existe é para que você possa ver como registrar uma propriedade de dependência com um retorno de chamada de propriedade alterada. A implementação de **OnLabelChanged** também mostra como obter um tipo projetado de derivado de um tipo de base projetado (é o tipo de base projetado **DependencyObject**, nesse caso). E ele mostra como, em seguida, obter um ponteiro para o tipo que implementa o tipo projetado. Essa segunda operação naturalmente só será possível no projeto que implementa o tipo projetado (ou seja, o projeto que implementa a classe de tempo de execução).

> [!NOTE]
> Se você ainda não instalou o SDK do Windows versão 10.0.17763.0 (Windows 10, versão 1809) ou posterior, em seguida, você precisará chamar [ **winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi) no manipulador de evento de alteração de propriedade de dependência acima, em vez de [ **winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

## <a name="design-the-default-style-for-bglabelcontrol"></a>O estilo padrão de design **BgLabelControl**

Em seu construtor **BgLabelControl** define uma chave de estilo padrão para si mesmo. Mas o que *é* um estilo padrão? Um controle (modelo) personalizado precisa ter um estilo padrão&mdash;que contém um modelo de controle padrão&mdash;que pode ser usada para renderizar em si com caso o consumidor do controle não define um estilo e/ou modelo. Nesta seção, adicionaremos um arquivo de marcação para o projeto que contém nosso estilo padrão.

Sob o nó do projeto, crie uma nova pasta e nomeie-a como "Temas". Sob `Themes`, adicione um novo item do tipo **Visual C++** > **XAML** > **exibição XAML**e nomeie-a como "Generic. XAML". Os nomes de pasta e arquivo precisam ser da seguinte maneira para que a estrutura XAML localizar o estilo padrão para um controle personalizado. Excluir o conteúdo padrão do `Generic.xaml`e cole na marcação abaixo.

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

Nesse caso, a única propriedade que define o estilo padrão é o modelo de controle. O modelo consiste em um quadrado (cujo plano de fundo é associado à **em segundo plano** propriedade que todas as instâncias da XAML [ **controle** ](/uwp/api/windows.ui.xaml.controls.control) tipo têm) e um elemento de texto (cuja texto está associado a **BgLabelControl::Label** propriedade de dependência).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Adicionar uma instância do **BgLabelControl** para a página principal da interface do usuário

Abra `MainPage.xaml`, que contém a marcação XAML para nossa página da interface do usuário principal. Imediatamente após o **botão** elemento (dentro de **StackPanel**), adicione a seguinte marcação.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Além disso, adicione o seguinte incluem a diretiva para `MainPage.h` para que o **MainPage** tipo (uma combinação de compilação de marcação XAML e código imperativo) está ciente do **BgLabelControl** tipo de controle personalizado. Se você quiser usar **BgLabelControl** de outra página XAML, em seguida, adicione isso mesmo incluem a diretiva para o arquivo de cabeçalho para essa página, muito. Ou, Alternativamente, simplesmente colocar uma única diretiva include em seu arquivo de cabeçalho pré-compilado.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Agora compile e execute o projeto. Você verá que o modelo de controle padrão é associando o pincel do plano de fundo e o rótulo da **BgLabelControl** instância na marcação.

Este passo a passo mostrou um exemplo simples de um controle (modelo) personalizado no C + + c++ /CLI WinRT. Você pode tornar seus próprios controles personalizados arbitrariamente avançado e completo. Por exemplo, um controle personalizado pode assumir a forma de algo mais complicado, como uma grade de dados editável, um player de vídeo ou um visualizador de geometria 3D.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementando *substituível* funções, como **MeasureOverride** e **OnApplyTemplate**

Você deriva um controle personalizado a [ **controle** ](/uwp/api/windows.ui.xaml.controls.control) deriva de classe de tempo de execução, que por si só ainda mais classes de base de tempo de execução. E há métodos substituíveis **controle**, [ **FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement), e [ **UIElement** ](/uwp/api/windows.ui.xaml.uielement) que você pode substituir em sua classe derivada. Aqui está um exemplo de código mostrando como fazer isso.

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

*Substituível* funções se apresentam diferentemente nas projeções de linguagem diferentes. No C#, por exemplo, funções substituíveis normalmente são exibidos como funções virtuais protegidas. No C + + c++ /CLI WinRT, eles são não virtuais nem protegido, mas você pode substituí-las e fornecer sua própria implementação, conforme mostrado acima.

## <a name="important-apis"></a>APIs Importantes
* [Classe de controle](/uwp/api/windows.ui.xaml.controls.control)
* [Classe DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)
* [Classe FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)
* [Classe UIElement](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Tópicos relacionados
* [Modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propriedades de dependência personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties)

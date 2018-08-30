---
author: stevewhims
description: Este tópico o orienta pelas etapas de criação de um controle personalizado simples usando C++ c++ WinRT. Você pode se basear as informações para criar seus próprios controles de interface do usuário rico e personalizáveis.
title: Controles personalizados (modelos) de XAML com C++ c++ WinRT
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle personalizado, modelo,
ms.localizationpriority: medium
ms.openlocfilehash: 81eb7f29e511f76d1126c1e4a43a2b96f1fa6f9f
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3119098"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Controles personalizados (modelos) de XAML com [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **Algumas informações estão relacionadas a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.**

> [!IMPORTANT]
> Para conceitos e termos essenciais que dão suporte ao entendimento de como consumir e criar classes de tempo de execução com C++/WinRT, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

Um dos recursos mais avançados de Universal Windows Platform (UWP) é a flexibilidade que fornece a pilha de interface do usuário (IU) para criar controles personalizados com base no tipo de [**controle**](/uwp/api/windows.ui.xaml.controls.control) do XAML. A estrutura de XAML UI fornece recursos, como [Propriedades de dependência personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties) e propriedades anexadas e [modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates), que tornam mais fácil criar controles rico e personalizáveis. Este tópico o orienta pelas etapas de criação de um controle personalizado (modelo) com C++ c++ WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Criar um aplicativo em branco (BgLabelControlApp)
Comece criando um novo projeto no Microsoft Visual Studio. Criar um **aplicativo em branco do Visual C++ (C++ c++ WinRT)** do projeto e nomeie- *BgLabelControlApp*.

Vamos criar uma nova classe para representar um controle personalizado (modelo). Estamos criando e consumindo a classe dentro da mesma unidade de compilação. Mas queremos poder criar uma instância dessa classe da marcação XAML e para que o motivo pelo qual vai ser uma classe de tempo de execução. E vamos usar C++/WinRT para criar e consumi-lo.

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

A listagem acima mostra o padrão que você segue ao declarar uma propriedade de dependência (DP). Há duas partes para cada ponto de distribuição. Primeiro, você pode declarar uma propriedade somente leitura estática do tipo [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Ele tem o nome do seu DP além de *propriedade*. Você usará essa propriedade estática na sua implementação. Segundo, você declara uma propriedade de instância de leitura-gravação com o tipo e o nome do seu DP.

> [!NOTE]
> Se você quiser um DP com um tipo de ponto flutuante, em seguida, torná-lo `double` (`Double` no [MIDL 3.0](/uwp/midl-3/)). Declaração e implementação de um ponto de distribuição do tipo `float` (`Single` no MIDL), e, em seguida, definir um valor para esse ponto de distribuição na marcação XAML, resulta no erro *Falha ao criar 'Windows.Foundation.Single' do texto '<NUMBER>'*.

Salve o arquivo e compile o projeto. Durante o processo de compilação, a ferramenta `midl.exe` é executada para criar um arquivo de metadados do componente do Tempo de Execução do Windows (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`), descrevendo a classe de tempo de execução. Em seguida, a ferramenta `cppwinrt.exe` é executada para gerar arquivos de código fonte para dar suporte a você na criação e no consumo da classe de tempo de execução. Esses arquivos incluem stubs para ajudar você a começar a implementar a classe de tempo de execução **BgLabelControl** que foi declarada em sua IDL. Esses stubs são `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` e `BgLabelControl.cpp`.

Copie os arquivos stub `BgLabelControl.h` e `BgLabelControl.cpp` de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` na pasta do projetor, que é `\BgLabelControlApp\BgLabelControlApp\`. No **Explorador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Clique nos arquivos stub com o botão direito do mouse e clique em **Incluir no projeto**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implemente a classe de controle personalizado **BgLabelControl**
Agora, vamos abrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` e `BgLabelControl.cpp` e implementar nossa classe de tempo de execução. Em `BgLabelControl.h`, altere o construtor para definir a chave de estilo padrão, implementar o **rótulo** e **LabelProperty**, adicionar um manipulador de eventos estáticos chamado **onlabelchanged é** para processar alterações para o valor da propriedade de dependência e adicione um membro particular para armazenar o campo de suporte para **LabelProperty**.

Após a inclusão desses controles, seu `BgLabelControl.h` tem esta aparência.

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

Em `BgLabelControl.cpp`, defina os membros estáticos desta forma.

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

        BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::from_abi<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
        // Call members of the implementation type via ptr.
    }
}
...
```

Neste passo a passo, nós não estiver usando o **onlabelchanged é**. Mas ele existe para que você possa ver como registrar uma propriedade de dependência com um retorno de chamada de propriedade alterada. A implementação de **onlabelchanged é** também mostra como obter um tipo projetado derivado de um tipo projetado base (o tipo projetado base é **DependencyObject**, neste caso). E mostra como obter um ponteiro para o tipo que implementa o tipo projetado. Essa operação segunda naturalmente só será possível no projeto que implementa o tipo projetado (ou seja, o projeto que implementa a classe de tempo de execução).

> [!NOTE]
> Se você instalou o [Windows 10 SDK versão prévia 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)ou posterior, em seguida, você pode chamar [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) no manipulador de eventos alterado da propriedade de dependência acima, em vez de [**WinRT:: from_abi**](/uwp/cpp-ref-for-winrt/from-abi).

## <a name="design-the-default-style-for-bglabelcontrol"></a>O estilo padrão de design para **BgLabelControl**

Em seu construtor, **BgLabelControl** define uma chave de estilo padrão para si mesmo. Mas o que *é* um estilo padrão? Um controle personalizado (modelo) precisa ter um estilo padrão&mdash;que contém um modelo de controle padrão&mdash;que ele pode usar para renderizar em si com caso o consumidor do controle não define um estilo e/ou modelo. Nesta seção, adicionaremos um arquivo de marcação para o projeto que contém o nosso estilo padrão.

Sob o nó do seu projeto, crie uma nova pasta e nomeie-a como "Themes". Em `Themes`, adicione um novo item do **Visual C++** do tipo > **XAML** > **Modo de exibição XAML**e nomeie-a como "Generic". Os nomes de arquivo e pasta precisam ser assim para que a estrutura XAML encontrar o estilo padrão para um controle personalizado. Exclua o conteúdo padrão do `Generic.xaml`e cole na marcação abaixo.

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

Nesse caso, a única propriedade que define o estilo padrão é o modelo de controle. O modelo consiste em um quadrado (cujo em segundo plano é associado à propriedade **em segundo plano** que têm todas as instâncias do tipo XAML [**controle**](/uwp/api/windows.ui.xaml.controls.control) ) e um elemento de texto (cujo texto está vinculado à propriedade de dependência **BgLabelControl::Label** ).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Adicionar uma instância de **BgLabelControl** para a página principal da interface do usuário

Abra `MainPage.xaml`, que contém a marcação XAML para nossa página da interface do usuário principal. Imediatamente após o elemento de **botão** (dentro de **StackPanel**), adicione a seguinte marcação.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Além disso, adicione o seguinte incluem diretiva para `MainPage.h` para que o tipo de **MainPage** (uma combinação de compilação de marcação XAML e código imperativo) está ciente do tipo de controle personalizado **BgLabelControl** .

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Agora compile e execute o projeto. Você verá que o modelo de controle padrão é vincular o pincel de plano de fundo e o rótulo, da instância **BgLabelControl** na marcação.

Este passo a passo mostrou um exemplo simples de um controle personalizado (modelo) em C++ c++ WinRT. Você pode fazer seus próprios controles personalizados arbitrariamente avançado e completo. Por exemplo, um controle personalizado pode levar a forma de algo tão complicado como uma grade de dados editável, um player de vídeo ou um visualizador de geometria 3D.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementando *substituíveis* funções, como **MeasureOverride** e **OnApplyTemplate**

Um controle personalizado deriva da classe de tempo de execução de [**controle**](/uwp/api/windows.ui.xaml.controls.control) , que por si só ainda mais deriva de classes de base de tempo de execução. E há métodos substituíveis de **controle**, [**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement)e [**UIElement**](/uwp/api/windows.ui.xaml.uielement) que você pode substituir em sua classe derivada. Aqui está um exemplo de código mostrando como fazer isso.

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

Funções de *Overridable* apresentam propriamente ditos diferente de projeções de idiomas diferentes. Em c#, por exemplo, função substituível normalmente aparecem como funções virtuais protegidas. No C++ c++ WinRT, elas são virtuais nem protegido, mas você ainda pode substituí-los e fornecer sua própria implementação, como mostrado acima.

## <a name="important-apis"></a>APIs importantes
* [Controle](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Tópicos relacionados
* [Modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propriedades de dependência personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties)

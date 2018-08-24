---
author: stevewhims
description: Este tópico o orienta pelas etapas de criação de um controle personalizado simples usando C + + / WinRT. Você pode se basear as informações para criar seus próprios controles de interface do usuário rico e personalizáveis.
title: Controles personalizados (modelos) de XAML com C + + / WinRT
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, projeção, XAML, controle personalizado, com modelos,
ms.localizationpriority: medium
ms.openlocfilehash: c108175c66d27b2cdbd910a0f7653ca1befb68e9
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2843688"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Controles personalizados (modelos) de XAML com [C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

Um dos recursos mais poderosos da plataforma do Windows Universal (UWP) é a flexibilidade com que a pilha de interface do usuário (UI) oferece para criar controles personalizados com base no tipo de XAML [controle](/uwp/api/windows.ui.xaml.controls.control) . A estrutura de UI XAML fornece recursos como [Propriedades personalizadas de dependência](/windows/uwp/xaml-platform/custom-dependency-properties) e propriedades anexadas e [modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates), que tornam mais fácil criar controles rico e personalizáveis. Este tópico o orienta pelas etapas de criação de um controle (modelo) personalizado com C + + / WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Criar um aplicativo em branco (BgLabelControlApp)
Comece criando um novo projeto no Microsoft Visual Studio. Criar um **App em branco do Visual C++ (C + + / WinRT)** project e nomeie-o *BgLabelControlApp*.

Vamos para criar uma nova classe para representar um controle (modelo) personalizado. Estamos criando e consumindo a classe dentro da mesma unidade de compilação. Mas, se deseja ser capaz de criar uma instância desta classe da marcação XAML e para que vai ser uma classe de tempo de execução de razão. E vamos usar C++/WinRT para criar e consumi-lo.

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

A lista acima mostra o padrão que você segue ao declarar uma propriedade de dependência (DP). Há duas partes para cada DP. Primeiro, você pode declarar uma propriedade estática somente leitura do tipo [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty). Ele tem o nome do seu DP além de *propriedade*. Você usará essa propriedade estática na sua implementação. Em segundo lugar, você deve declarar uma propriedade de leitura-gravação instância com o tipo e o nome do seu DP.

> [!NOTE]
> Se desejar que um ponto de distribuição com um tipo de ponto flutuante, em seguida, torná-lo `double` (`Double` no [MIDL 3.0](/uwp/midl-3/)). Declarando e implementação de um ponto de distribuição do tipo `float` (`Single` em MIDL), e, em seguida, definir um valor para esse DP na marcação XAML, resulta em erro *Falha ao criar um 'Windows.Foundation.Single' do texto '<NUMBER>'*.

Salve o arquivo e compile o projeto. Durante o processo de compilação, a ferramenta `midl.exe` é executada para criar um arquivo de metadados do componente do Tempo de Execução do Windows (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`), descrevendo a classe de tempo de execução. Em seguida, a ferramenta `cppwinrt.exe` é executada para gerar arquivos de código fonte para dar suporte a você na criação e no consumo da classe de tempo de execução. Esses arquivos incluem stubs para você começar a implementar a classe de tempo de execução **BgLabelControl** declarado na sua IDL. Esses stubs são `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` e `BgLabelControl.cpp`.

Copie os arquivos stub `BgLabelControl.h` e `BgLabelControl.cpp` de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` na pasta do projetor, que é `\BgLabelControlApp\BgLabelControlApp\`. No **Explorador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Clique nos arquivos stub com o botão direito do mouse e clique em **Incluir no projeto**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implementar a classe de controle personalizado **BgLabelControl**
Agora, vamos abrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` e `BgLabelControl.cpp` e implementar nossa classe de tempo de execução. Em `BgLabelControl.h`, altere o construtor para definir a chave de estilo padrão, implementar **rótulo** e **LabelProperty**, adicionar um manipulador de evento estático chamado **OnLabelChanged** para processar alterações para o valor da propriedade de dependência e adicionar um membro privado para armazenar o campo de backup para **LabelProperty**.

Este passo a passo, estamos não usando **OnLabelChanged**. Mas há é para que você possa ver como registrar uma propriedade de dependência com um retorno de chamada de propriedade alterada.

Depois de adicionar aqueles, sua `BgLabelControl.h` tem esta aparência.

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

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

Em `BgLabelControl.cpp`, definem os membros estáticos semelhante a esta.

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

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e) {}
...
```

## <a name="design-the-default-style-for-bglabelcontrol"></a>Projetar o estilo padrão para o **BgLabelControl**

Em seu construtor, **BgLabelControl** define uma chave de estilo padrão para si mesma. Mas o que *é* um estilo padrão? Um controle (modelo) personalizado precisa ter um estilo padrão&mdash;contendo um modelo de controle padrão&mdash;que ele pode usar para renderizar próprio com, no caso de consumidor do controle não define um estilo e/ou modelo. Nesta seção vamos adicionar um arquivo de marcação para o projeto que contém o nosso estilo padrão.

Sob o nó do seu projeto, crie uma nova pasta e nomeie-o "Temas". Sob `Themes`, adicione um novo item do tipo **Do Visual C++** > **XAML** > **Exibição XAML**e nomeie-o "Generic". Os nomes de pasta e arquivo precisam ser semelhante a esta na ordem para o framework XAML encontrar o estilo padrão para um controle personalizado. Exclua o conteúdo padrão da `Generic.xaml`e cole na marcação abaixo.

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

Nesse caso, a única propriedade que define o estilo padrão é o modelo de controle. O modelo consiste em um quadrado (cujo plano de fundo é acoplado para a propriedade **Background** que têm todas as instâncias do tipo XAML [controle](/uwp/api/windows.ui.xaml.controls.control) ) e um elemento de texto (cujo texto é acoplado à propriedade dependência **BgLabelControl::Label** ).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Adicionar uma instância de **BgLabelControl** para a página principal da interface do usuário

Abra `MainPage.xaml`, que contém a marcação XAML para nossa página da interface do usuário principal. Imediatamente após o elemento de **botão** (interna **StackPanel**), adicione a seguinte marcação.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Além disso, adicione o seguinte incluir diretriz para `MainPage.h` para que o tipo de **MainPage** (uma combinação de compilação de marcação XAML e código imperativo) está ciente do tipo **BgLabelControl** controle personalizado.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Agora compile e execute o projeto. Você verá que o modelo de controle padrão é associando o pincel de plano de fundo e de rótulo, da instância **BgLabelControl** na marcação.

Este passo a passo mostrou um exemplo simples de um controle (modelo) personalizado em C + + / WinRT. Você pode fazer seus próprios controles personalizados arbitrariamente rica e completo. Por exemplo, um controle personalizado pode assumir a forma de algo tão complicado quanto um visualizador de geometria 3D, um player de vídeo ou uma grade de dados editável.

## <a name="important-apis"></a>APIs importantes
* [Controle](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)

## <a name="related-topics"></a>Tópicos relacionados
* [Modelos de controle](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propriedades de dependência personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties)

---
description: Este tópico orienta você no processo de adicionar suporte simples ao WinUI em um projeto C++/WinRT.
title: Um exemplo simples da Biblioteca de Interface do Usuário do Windows em C++/WinRT
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, Biblioteca de Interface do Usuário do Windows, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 5d0066abb2a6eb15f1d31aaf930ed2c0f0faf81a
ms.sourcegitcommit: 4e74c920f1fef507c5cdf874975003702d37bcbb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372714"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>Um exemplo simples da Biblioteca de Interface do Usuário do Windows em C++/WinRT

Este tópico descreve o processo de adição de suporte simples ao [WinUI (Biblioteca de Interface do Usuário do Windows)](https://github.com/Microsoft/microsoft-ui-xaml) ao seu projeto do C++/WinRT. Por acaso, a Biblioteca de Interface do Usuário do Windows é escrita em C++/WinRT.

> [!NOTE]
> O kit de ferramentas do WinUI (Biblioteca de Interface do Usuário do Windows) está disponível como pacotes NuGet que você pode adicionar a qualquer projeto novo ou existente usando o Visual Studio, como veremos neste tópico. Para obter mais informações de contexto, configuração e suporte, confira [Introdução à Biblioteca de Interface do Usuário do Windows](/uwp/toolkits/winui/getting-started).

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>Criar um aplicativo em branco (HelloWinUICppWinRT)

No Visual Studio, crie um projeto usando o modelo de projeto **Aplicativo em Branco (C++/WinRT)** e nomeie-o *HelloWinUICppWinRT*.

## <a name="install-the-microsoftuixaml-nuget-package"></a>Instalar o pacote NuGet Microsoft.UI.Xaml

Clique em **Projeto** \> **Gerenciar Pacotes NuGet...** \> **Procurar**, digite ou cole **Microsoft.UI.Xaml** na caixa de pesquisa, selecione o item nos resultados da pesquisa e, em seguida, clique em **Instalar** para instalar o pacote no projeto (você também verá um prompt do contrato de licença). Tenha cuidado para instalar apenas o pacote **Microsoft.UI.Xaml** e não **Microsoft.UI.Xaml.Core.Direct**.

## <a name="declare-winui-application-resources"></a>Declarar recursos do aplicativo do WinUI

Abra `App.xaml` e cole a marcação a seguir entre as marcas **Application** de abertura e fechamento existentes.

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>Adicionar um controle do WinUI à MainPage

Em seguida, abra `MainPage.xaml`. Na marca **Application** de abertura existente, há algumas declarações de namespace de XML. Adicione a declaração de namespace de XML `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`. Em seguida, cole a marcação a seguir entre as marcas **Page** de abertura e fechamento existentes, substituindo o elemento **StackPanel** existente.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpageh-and-cpp-as-necessary"></a>Editar MainPage.h e .cpp conforme necessário

Quando você adiciona um pacote NuGet a um projeto do C++/WinRT (como o pacote **Microsoft.UI.Xaml**, que você adicionou anteriormente), as ferramentas geram um conjunto de cabeçalhos de projeção em sua pasta `\Generated Files\winrt` do projeto. Para levar esses arquivos de cabeçalhos para seu projeto, para que as referências a esses novos tipos sejam resolvidas, será necessário incluí-los.

Portanto, no `MainPage.h`, edite as inclusões para que elas tenham a aparência das que estão na listagem abaixo. Se você usasse o WinUI em mais de uma página XAML, poderia entrar no arquivo de cabeçalho pré-compilado (normalmente `pch.h`) e incluí-lo lá.

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

Por fim, em `MainPage.cpp`, exclua o código dentro da implementação de **MainPage::ClickHandler**, pois *myButton* não está mais na marcação XAML.

Agora você poderá compilar e executar o projeto.

![Captura de tela simples da Biblioteca de Interface do Usuário do Windows em C++/WinRT](images/winui.png)

## <a name="related-topics"></a>Tópicos relacionados
* [Introdução à Biblioteca de Interface do Usuário do Windows](/uwp/toolkits/winui/getting-started)
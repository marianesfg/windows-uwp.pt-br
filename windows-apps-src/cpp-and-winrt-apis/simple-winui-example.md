---
description: Este tópico orienta você no processo de adicionar suporte simples ao WinUI em um projeto C++/WinRT.
title: Um exemplo simples da Biblioteca de Interface do Usuário do Windows em C++/WinRT
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, Biblioteca de Interface do Usuário do Windows, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: aadf177bc4a44f67550dba1f6f706525b8460857
ms.sourcegitcommit: c9bab19599c0eb2906725fd86d0696468bb919fa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78256169"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>Um exemplo simples da Biblioteca de Interface do Usuário do Windows em C++/WinRT

Este tópico descreve o processo de adição de suporte simples ao [WinUI (Biblioteca de Interface do Usuário do Windows)](https://github.com/Microsoft/microsoft-ui-xaml) ao seu projeto do C++/WinRT. Por acaso, a Biblioteca de Interface do Usuário do Windows é escrita em C++/WinRT.

> [!NOTE]
> O kit de ferramentas do WinUI (Biblioteca de Interface do Usuário do Windows) está disponível como pacotes NuGet que você pode adicionar a qualquer projeto novo ou existente usando o Visual Studio, como veremos neste tópico. Para obter mais informações de contexto, configuração e suporte, confira [Introdução à Biblioteca de Interface do Usuário do Windows](/uwp/toolkits/winui/getting-started).

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>Criar um aplicativo em branco (HelloWinUICppWinRT)

No Visual Studio, crie um novo projeto usando o modelo **Aplicativo em branco (C++/WinRT)** . Certifique-se de que você está usando o modelo **(C++/WinRT)** e não o **(Universal do Windows)** .

Nomeie o novo projeto como *HelloWinUICppWinRT* e, para que sua estrutura de pastas corresponda ao passo a passo, desmarque **Posicionar a solução e o projeto no mesmo diretório**.

## <a name="install-the-microsoftuixaml-nuget-package"></a>Instalar o pacote NuGet Microsoft.UI.Xaml

Clique em **Projeto** \> **Gerenciar Pacotes NuGet...** \> **Procure**, digite ou cole **Microsoft.UI.Xaml** na caixa de pesquisa, selecione o item nos resultados e, em seguida, clique em **Instalar** para instalar o pacote no seu projeto (você também verá uma solicitação de contrato de licença). Tenha cuidado para instalar apenas o pacote **Microsoft.UI.Xaml** e não **Microsoft.UI.Xaml.Core.Direct**.

## <a name="declare-winui-application-resources"></a>Declarar recursos do aplicativo do WinUI

Abra `App.xaml` e cole a marcação a seguir entre as marcas **Application** de abertura e fechamento existentes.

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>Adicionar um controle do WinUI à MainPage

Em seguida, abra `MainPage.xaml`. Na marcação da **Página** de abertura existente, há algumas declarações de namespace de XML. Adicione a declaração de namespace de XML `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`. Em seguida, cole a marcação a seguir entre as marcas **Page** de abertura e fechamento existentes, substituindo o elemento **StackPanel** existente.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpagecpp-and-h-as-necessary"></a>Editar MainPage.cpp e .h conforme necessário

Em `MainPage.cpp`, exclua o código dentro da implementação de **MainPage::ClickHandler**, pois *myButton* não está mais na marcação XAML.

Em `MainPage.h`, edite as inclusões para que pareçam com as da listagem abaixo. Se você usasse o WinUI em mais de uma página XAML, poderia entrar no arquivo de cabeçalho pré-compilado (normalmente `pch.h`) e incluí-lo lá.

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

Então, compile o projeto.

Quando você adiciona um pacote NuGet a um projeto C++/WinRT (como o pacote **Microsoft.UI.Xaml**, que você adicionou anteriormente) e compila o projeto, as ferramentas geram um conjunto de arquivos de cabeçalhos de projeção em sua pasta `\Generated Files\winrt` do projeto. Se você seguiu as instruções passo a passo, terá agora uma pasta `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt`. A alteração feita ao `MainPage.h` acima faz com que esses arquivos de cabeçalho de projeção sejam incluídos no seu projeto. E isso é necessário para que as referências a tipos do pacote NuGet sejam resolvidas.

Agora você pode executar o projeto.

![Captura de tela simples da Biblioteca de Interface do Usuário do Windows em C++/WinRT](images/winui.png)

## <a name="related-topics"></a>Tópicos relacionados
* [Introdução à Biblioteca de Interface do Usuário do Windows](/uwp/toolkits/winui/getting-started)
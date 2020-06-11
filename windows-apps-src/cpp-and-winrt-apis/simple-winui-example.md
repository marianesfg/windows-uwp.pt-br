---
description: Este tópico orienta você no processo de adicionar suporte simples ao WinUI em um projeto C++/WinRT.
title: Um exemplo simples da Biblioteca de Interface do Usuário do Windows em C++/WinRT
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, Biblioteca de Interface do Usuário do Windows, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 8242055e3c448e2720226859f2ea10e1ae54794f
ms.sourcegitcommit: db48036af630f33f0a2f7a908bfdfec945f3c241
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84437130"
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

## <a name="edit-pchh-as-necessary"></a>Edite pch.h, conforme necessário

Quando você adiciona um pacote NuGet a um projeto C++/WinRT (como o pacote **Microsoft.UI.Xaml**, que você adicionou anteriormente) e compila o projeto, as ferramentas geram um conjunto de arquivos de cabeçalhos de projeção em sua pasta `\Generated Files\winrt` do projeto. Se você seguiu as instruções passo a passo, terá agora uma pasta `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt`. Para usar esses arquivos de cabeçalho no seu projeto, de modo que as referências a esses novos tipos sejam resolvidas, acesse o arquivo de cabeçalho pré-compilado (normalmente, `pch.h`) e inclua-os.

Você precisa incluir somente os cabeçalhos que correspondem aos tipos que você usa. Segue um exemplo que inclui todos os arquivos de cabeçalho gerados para o pacote **Microsoft.UI.Xaml**.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

## <a name="edit-mainpagecpp"></a>Edite MainPage.cpp

Em `MainPage.cpp`, exclua o código dentro da implementação de **MainPage::ClickHandler**, pois *myButton* não está mais na marcação XAML.

Agora você poderá compilar e executar o projeto.

![Captura de tela simples da Biblioteca de Interface do Usuário do Windows em C++/WinRT](images/winui.png)

## <a name="related-topics"></a>Tópicos relacionados
* [Introdução à Biblioteca de Interface do Usuário do Windows](/uwp/toolkits/winui/getting-started)
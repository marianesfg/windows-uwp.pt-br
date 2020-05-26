---
title: Introdução à Biblioteca de Interface do Usuário do Windows
description: Como instalar e usar a Biblioteca de Interface do Usuário do Windows.
ms.topic: reference
ms.date: 05/08/2020
keywords: windows 10, uwp, sdk do kit de ferramentas
ms.openlocfilehash: 585475df4138c6a5d4d8b885582137c972a64287
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775864"
---
# <a name="getting-started-with-the-windows-ui-library"></a>Introdução à Biblioteca de Interface do Usuário do Windows

A versão [WinUI 2.4](release-notes/winui-2.4.md) é a menos estável do WinUI e deve ser usada para aplicativos em produção.

A biblioteca está disponível como um pacote do NuGet que pode ser adicionado a qualquer projeto novo ou existente do Visual Studio.

> [!NOTE]
> Para saber mais sobre como experimentar as versões prévias anteriores do WinUI 3.0, confira o [WinUI 3.0 Versão Prévia 1](../winui3/index.md).

## <a name="download-and-install-the-windows-ui-library"></a>Baixar e instalar a Biblioteca de Interface do Usuário do Windows

1. Baixe o [Visual Studio 2019](https://developer.microsoft.com/windows/downloads) e escolha a carga de trabalho de **desenvolvimento da Plataforma Universal do Windows** no instalador do Visual Studio.

2. Abra um projeto existente ou crie um novo usando o modelo Aplicativo em Branco em Visual C# -> Windows -> Universal, ou o modelo adequado para a projeção de idioma.  

    > [!IMPORTANT]
    > Para usar o WinUI 2.4, você precisa definir TargetPlatformVersion como 10.0.18362.0 e TargetPlatformMinVersion como 10.0.15063.0 nas propriedades do projeto.

3. No painel do Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto e selecione **Gerenciar Pacotes NuGet**. Selecione a guia **Procurar** e pesquise **Microsoft.UI.Xaml** ou **WinUI**. Em seguida, escolha os [Pacotes NuGet da Biblioteca de Interface do Usuário do Windows](nuget-packages.md) que você quer usar.
O pacote **Microsoft.UI.Xaml** contém controles e recursos Fluent adequados para todos os aplicativos.  
Outra opção é verificar "Incluir pré-lançamento" para ver as versões de pré-lançamento mais recentes que incluem recursos experimentais novos.

    ![Pacotes NuGet](images/ManageNugetPackages.png "Imagem de Gerenciar Pacotes NuGet")

    ![Pacotes NuGet](images/NugetPackages.png)

4. Adicione os Recursos de Tema do WinUI (Interface do Usuário do Windows) aos recursos App.xaml. Se você tem outros recursos de aplicativo, há duas maneiras de fazer isso.

    a. Se você não tem outros recursos de aplicativo, adicione `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` a Application.Resources:

    ``` XAML
    <Application>
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </Application>
    ```

    b. Caso contrário, se houver mais de um conjunto de recursos de aplicativo, adicione `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` a Application.Resources.MergedDictionaries:

    ``` XAML
    <Application>
        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>
    </Application>
    ```

    > [!IMPORTANT]
    > A ordem dos recursos adicionados a um ResourceDictionary afeta a ordem em que eles serão aplicados. O dicionário `XamlControlsResources` substitui várias chaves de recurso padrão. Portanto, deve ser adicionado a `Application.Resources` primeiro, para que não substitua outros estilos ou recursos personalizados no aplicativo. Para saber mais sobre o carregamento de recursos, confira [Referências de recursos de ResourceDictionary e XAML](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

5. Adicione uma referência do kit de ferramentas às páginas XAML e às páginas code-behind.

    * Adicione uma referência à parte superior da página XAML.

        ```xaml
        xmlns:controls="using:Microsoft.UI.Xaml.Controls"
        ```

    * Se você quiser usar os nomes de tipo sem qualificá-los, adicione uma diretriz de uso ao código.

        ```csharp
        using MUXC = Microsoft.UI.Xaml.Controls;
        ```

## <a name="additional-steps-for-a-cwinrt-project"></a>Etapas adicionais para um projeto do C++/WinRT

Ao adicionar um pacote NuGet a um projeto do C++/WinRT, as ferramentas geram um conjunto de cabeçalhos de projeção na pasta `\Generated Files\winrt` do projeto. Para usar esses arquivos de cabeçalho no seu projeto, de modo que as referências a esses novos tipos sejam resolvidas, acesse o arquivo de cabeçalho pré-compilado (normalmente, `pch.h`) e inclua-os. Veja a seguir um exemplo que inclui os arquivos de cabeçalho gerados para o pacote **Microsoft.UI.Xaml**.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

Para ver um passo a passo completo de como adicionar uma compatibilidade simples com a Biblioteca de Interface do Usuário do Windows ao projeto do C++/WinRT, confira [Exemplo simples de Biblioteca de Interface do Usuário do Windows no C++/WinRT](/windows/uwp/cpp-and-winrt-apis/simple-winui-example).

## <a name="contributing-to-the-windows-ui-library"></a>Contribuir para a Biblioteca de Interface do Usuário do Windows

O WinUI é um projeto de software livre hospedado no GitHub.

Relatórios de bugs, solicitações de recursos e contribuições de código da comunidade são bem-vindos no [Repositório da Biblioteca de Interface do Usuário do Windows](https://aka.ms/winui).

## <a name="other-resources"></a>Outros recursos

Se você for iniciante na UWP, é recomendável visitar as páginas [Introdução ao desenvolvimento na UWP](https://developer.microsoft.com/windows/getstarted) no Portal do Desenvolvedor.

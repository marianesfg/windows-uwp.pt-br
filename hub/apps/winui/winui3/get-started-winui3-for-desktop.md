---
description: Este guia mostra como começar a criar aplicativos .NET e C++/Win32 da área de trabalho com uma interface do usuário do WinUI 3.
title: Introdução ao WinUI 3 para aplicativos da área de trabalho
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, ilhas xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: ab67507153e0ff7065baffa92ea6ec35aee5b132
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580763"
---
# <a name="get-started-with-winui-30-for-desktop-apps"></a>Introdução ao WinUI 3.0 para aplicativos da área de trabalho

O WinUI 3.0 Versão prévia 1 introduz novos modelos de projeto que permitem criar aplicativos gerenciados da área de trabalho com C#/.NET e aplicativos nativos da área de trabalho com C++/Win32, com uma interface do usuário totalmente baseada em WinUI. Quando você cria aplicativos usando esses modelos de projeto, toda a interface do usuário do aplicativo é implementada usando janelas, controles e outros tipos de interface do usuário fornecidos pelo WinUI 3.0. 

O WinUI 3.0 Versão prévia 1 adiciona os seguintes modelos de projeto do **WinUI da Área de Trabalho** no Visual Studio 2019:

* Aplicativos e bibliotecas de C# voltados para .NET 5:
  * Aplicativo em branco, empacotado (WinUI na Área de Trabalho)
  * Biblioteca de classes (WinUI na Área de Trabalho)

* Aplicativos C++/Win32:
  * Aplicativo em branco, empacotado (WinUI na Área de Trabalho)

Os modelos de projeto do aplicativo geram um projeto de aplicativo do WinUI e um [Projeto de Empacotamento de Aplicativo do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), que é configurado para compilar o aplicativo em um [pacote MSIX](https://docs.microsoft.com/windows/msix/overview) para implantação.

## <a name="prerequisites"></a>Pré-requisitos

Para usar o WinUI 3 para os modelos de projeto para área de trabalho descritos neste artigo, configure o computador de desenvolvimento seguindo estas instruções:

1. Verifique se o computador de desenvolvimento tem o Windows 10, versão 1803 (build 17134) ou uma versão mais recente instalada. O WinUI 3 para aplicativos da área de trabalho requer a versão 1803 ou posterior do sistema operacional.

2. Instale o Visual Studio 2019, versão 16.7 versão prévia 1. Para obter detalhes, confira [estas instruções](index.md#configure-your-dev-environment).

3. Instale as versões x64 e x86 do .NET 5 versão prévia 4:
    * x64: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe)
    * x86: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe)

4. Instale a extensão do VSIX que inclui os modelos de projeto do WinUI 3.0 Versão prévia 1 para o Visual Studio 2019. Para obter detalhes, confira [estas instruções](index.md#visual-studio-project-templates).

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>Criar um aplicativo da área de trabalho WinUI 3 para C# e .NET 5

1. No Visual Studio 2019, selecione **Arquivo** -> **Novo** -> **Projeto**.

2. Nos filtros em menu suspenso do projeto, selecione **C#** , **Windows** e **WinUI**, respectivamente.

3. Selecione o tipo de projeto **Aplicativo em Branco, Empacotado (WinUI na Área de Trabalho)** e clique em **Avançar**.

    ![Modelo de Projeto de Aplicativo em Branco](images/WinUI-csharp-newproject.png)

4. Insira um nome para o projeto, escolha qualquer outra opção desejada e clique em **Criar**.

5. Na caixa de diálogo a seguir, defina a **Versão de destino** como o Windows 10, versão 1903 (build 18362) e a **Versão mínima** como o Windows 10, versão 1803 (build 17134) e clique em **OK**.

    ![Versão de destino e versão mínima](images/WinUI-min-target-version.png)

6. Neste ponto, o Visual Studio gera dois projetos:

    * ***Nome do projeto* (Área de Trabalho)** : Este projeto contém o código do aplicativo. O arquivo de código **App.xaml.cs** define uma classe `Application` que representa a instância do aplicativo, e o arquivo de código **MainWindow.xaml.cs** define uma classe `MainWindow` que representa a janela principal exibida pelo aplicativo. Essas classes derivam dos tipos no namespace **Microsoft.UI.Xaml** fornecido pelo WinUI.

        ![Projeto do Aplicativo](images/WinUI-csharp-appproject.png)

    * ***Nome do projeto* (Pacote)** : Este é um [Projeto de Empacotamento de Aplicativo do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), que é configurado para compilar o aplicativo em um pacote MSIX para implantação. Este projeto contém o [manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) do aplicativo e é o projeto de inicialização de sua solução por padrão.

        ![Projeto do Aplicativo](images/WinUI-csharp-packageproject.png)

7. Para adicionar um novo item ao projeto do aplicativo, clique com o botão direito do mouse no nó do projeto ***Nome do projeto* (Área de Trabalho)** no **Gerenciador de Soluções** e selecione **Adicionar** -> **Novo Item**. Na caixa de diálogo **Adicionar Novo Item**, selecione a guia **WinUI**, escolha o item que deseja adicionar e, em seguida, clique em **Adicionar**. É possível escolher entre os seguintes tipos de itens:

    * **Página em Branco**
    * **Janela em Branco**
    * **Controle Personalizado**
    * **Dicionário de recursos**
    * **Arquivo de Recursos**
    * **Controle do Usuário**

    ![Novo Item](images/WinUI-csharp-newitem.png)

8. Compile e execute a solução para confirmar se o aplicativo é executado sem erros.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>Criar um aplicativo da área de trabalho WinUI 3 para C++/Win32

1. No Visual Studio 2019, selecione **Arquivo** -> **Novo** -> **Projeto**.

2. Nos filtros em menu suspenso do projeto, selecione **C++** , **Windows** e **WinUI**.

3. Selecione o tipo de projeto **Aplicativo em Branco, Empacotado (WinUI na Área de Trabalho)** e clique em **Avançar**.

    ![Modelo de Projeto de Aplicativo em Branco](images/WinUI-cpp-newproject.png)

4. Insira um nome para o projeto, escolha qualquer outra opção desejada e clique em **Criar**.

5. Na caixa de diálogo a seguir, defina a **Versão de destino** como o Windows 10, versão 1903 (build 18362) e a **Versão mínima** como o Windows 10, versão 1803 (build 17134) e clique em **OK**.

    ![Versão de destino e versão mínima](images/WinUI-min-target-version.png)

6. Neste ponto, o Visual Studio gera dois projetos:

    * ***Nome do projeto* (Área de Trabalho)** : Este projeto contém o código do aplicativo. O **App.xaml** e vários arquivos de código **App** definem uma classe `Application` que representa a instância do aplicativo, e o **MainWindow.xaml.cs** e vários arquivos de código **MainWindow** definem uma classe `MainWindow` que representa a Janela principal exibida pelo aplicativo. Essas classes derivam dos tipos no namespace **Microsoft.UI.Xaml** fornecido pelo WinUI.

        ![Projeto do Aplicativo](images/WinUI-cpp-appproject.png)

    * ***Nome do projeto* (Pacote)** : Este é um [Projeto de Empacotamento de Aplicativo do Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), que é configurado para compilar o aplicativo em um pacote MSIX para implantação. Este projeto contém o [manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) do aplicativo e é o projeto de inicialização de sua solução por padrão.

        ![Projeto de Pacote](images/WinUI-cpp-packageproject.png)

7. Para adicionar um novo item ao projeto do aplicativo, clique com o botão direito do mouse no nó do projeto ***Nome do projeto* (Área de Trabalho)** no **Gerenciador de Soluções** e selecione **Adicionar** -> **Novo Item**. Na caixa de diálogo **Adicionar Novo Item**, selecione a guia **WinUI**, escolha o item que deseja adicionar e, em seguida, clique em **Adicionar**. É possível escolher entre os seguintes tipos de itens:

    * **Página em Branco**
    * **Janela em Branco**
    * **Controle Personalizado**
    * **Dicionário de recursos**
    * **Arquivo de Recursos**
    * **Controle do Usuário**

    ![Novo Item](images/WinUI-cpp-newitem.png)

8. Compile e execute a solução para confirmar se o aplicativo é executado sem erros.

## <a name="known-issues-and-limitations"></a>Limitações e problemas conhecidos

Para ver uma lista de problemas conhecidos e limitações da Versão prévia 1, confira [esta seção](index.md#preview-1-limitations-and-known-issues).

## <a name="related-topics"></a>Tópicos relacionados

* [WinUI 3.0](index.md)
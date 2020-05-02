---
description: Esse tutorial demonstra como adicionar interfaces de usuário XAML da UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo do WPF.
title: Adicionar um controle InkCanvas da UWP usando ilhas de XAML
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, ilhas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6bb90fb9cbe7c9f54f60fd1920f0e73e174a3772
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80482577"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Parte 2: Adicionar um controle InkCanvas da UWP usando ilhas de XAML

Esta é a segunda parte de um tutorial que demonstra como modernizar um aplicativo da área de trabalho do WPF de exemplo chamado Contoso Expenses. Para obter uma visão geral do tutorial, dos pré-requisitos e das instruções para baixar o aplicativo de exemplo, confira [Tutorial: modernizar um aplicativo do WPF](modernize-wpf-tutorial.md). Este artigo pressupõe que você já concluiu a [parte 1](modernize-wpf-tutorial-1.md).

No cenário fictício deste tutorial, a equipe de desenvolvimento da Contoso deseja adicionar suporte para assinaturas digitais ao aplicativo Contoso Expenses. O controle **InkCanvas** da UWP é uma ótima opção para esse cenário, pois ele dá suporte à tinta digital e a recursos habilitados para IA, como a capacidade de reconhecimento de texto e formas. Para fazer isso, você usará o controle [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulado da UWP disponível no Windows Community Toolkit. Esse controle encapsula a interface e a funcionalidade do controle **InkCanvas** da UWP para uso em um aplicativo do WPF. Para obter mais detalhes sobre os controles da UWP encapsulados, confira [Hospedar controles XAML da UWP em aplicativos da área de trabalho (ilhas XAML)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Configurar o projeto para usar ilhas XAML

Antes de adicionar um controle **InkCanvas** ao aplicativo Contoso Expenses, primeiro você precisa configurar o projeto para dar suporte às ilhas XAML da UWP.

1. No Visual Studio 2019, clique com o botão direito do mouse no projeto **ContosoExpenses.Core** em **Gerenciador de Soluções** e escolha **Gerenciar Pacotes do NuGet**.

    ![Menu Gerenciar Pacotes do NuGet no Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. Na janela **Gerenciador de Pacotes do NuGet**, clique em **Procurar**. Procure o pacote `Microsoft.Toolkit.Wpf.UI.Controls` e instale a versão 6.0.0 ou uma versão posterior.

    > [!NOTE]
    > Esse pacote contém toda a infraestrutura necessária para hospedar ilhas XAML da UWP em um aplicativo do WPF, incluindo o controle **InkCanvas** encapsulado da UWP. Um pacote semelhante chamado `Microsoft.Toolkit.Forms.UI.Controls` está disponível para aplicativos Windows Forms.

3. Clique com o botão direito do mouse no projeto **ContosoExpenses.Core** no **Gerenciador de Soluções** e escolha **Adicionar -> Novo item**.

4. Selecione o **Arquivo de Manifesto do Aplicativo**, nomeie-o como **app.manifest** e clique em **Adicionar**. Para obter mais informações sobre manifestos do aplicativo, confira [este artigo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests).

5. No arquivo de manifesto, remova a marca de comentário do elemento `<supportedOS>` a seguir para Windows 10.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. No arquivo de manifesto, localize o elemento a seguir `<application>` comentado.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. Exclua essa seção e substitua-a pelo XML a seguir. Isso configura o aplicativo para ser compatível com DPI e lidar melhor com fatores de dimensionamento diferentes com suporte do Windows 10.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. Salve e feche o arquivo `app.manifest`.

9. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto **ContosoExpenses.Core** e escolha **Propriedades**.

10. Na seção **Recursos** da guia **Aplicativo**, verifique se a lista suspensa **Manifesto** está definida como **app.manifest**.

    ![Manifesto do aplicativo .NET Core](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. Salve as alterações feitas nas propriedades do projeto.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Adicionar um controle InkCanvas ao aplicativo

Agora que configurou seu projeto para usar as ilhas XAML da UWP, você está pronto para adicionar um controle [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulado da UWP ao aplicativo.

1. Em **Gerenciador de Soluções**, expanda a pasta **Exibições** do projeto **ContosoExpenses.Core** e clique duas vezes no arquivo **ExpenseDetail.xaml**.

2. No elemento **Window** próximo à parte superior do arquivo XAML, adicione o atributo a seguir. Isso faz referência ao namespace XAML para o controle [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulado da UWP.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Depois de adicionar esse atributo, o elemento **Janela** deverá ficar como a seguir.

    ```xml
    <Window x:Class="ContosoExpenses.Views.ExpenseDetail"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:converters="clr-namespace:ContosoExpenses.Converters"
            DataContext="{Binding Source={StaticResource ViewModelLocator}, Path=ExpensesDetailViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Expense Detail" Height="500" Width="800"
            Background="{StaticResource HorizontalBackground}">
    ```

4. No arquivo **ExpenseDetail.xaml**, localize a marca de fechamento `</Grid>` que precede imediatamente o comentário `<!-- Chart -->`. Adicione o XAML a seguir logo antes da marca de fechamento `</Grid>`. Esse XAML adiciona um controle **InkCanvas** (prefixado pela palavra-chave **toolkit** que você definiu anteriormente como um namespace) e um **TextBlock** simples que atua como um cabeçalho para o controle.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Salve o arquivo **ExpenseDetail.xaml**.

6. Pressione F5 para executar o aplicativo no depurador.

7. Escolha um funcionário na lista e, em seguida, escolha uma das despesas disponíveis. Observe que a página de detalhes de despesas contém espaço para o controle **InkCanvas**.

    ![Somente caneta tela de tinta](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Se você tiver um dispositivo que dá suporte a uma caneta digital, como o Surface, e você estiver executando este laboratório em um computador físico, tente usá-lo. Você verá a tinta digital aparecendo na tela. No entanto, se você não tiver um dispositivo compatível com caneta e tentar entrar com o mouse, nada acontecerá. Isso ocorre porque o controle **InkCanvas** é habilitado apenas para canetas digitais por padrão. Porém, é possível alterar esse comportamento.

8. Feche o aplicativo e clique duas vezes no arquivo **ExpenseDetail.xaml.cs** na pasta **Exibições** do projeto **ContosoExpenses.Core**.

9. Adicione a seguinte declaração de namespace na parte superior da classe:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Localize o construtor `ExpenseDetail()`.

11. Adicione a linha de código a seguir após o método `InitializeComponent()` e salve o arquivo de código.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Você pode usar o objeto **InkPresenter** para personalizar a experiência de tinta padrão. Esse código usa a propriedade **InputDeviceTypes** para habilitar o mouse, bem como a entrada à caneta.

12. Pressione F5 novamente para recompilar e executar o aplicativo no depurador. Escolha um funcionário na lista e, em seguida, escolha uma das despesas disponíveis.

13. Tente agora desenhar algo no espaço de assinatura com o mouse. Dessa vez, você verá a tinta aparecendo na tela.

    ![Assinatura](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Próximas etapas

Neste ponto do tutorial, você adicionou com êxito um controle **InkCanvas** da UWP ao aplicativo de Contoso Expenses. Agora você está pronto para a [Parte 3: adicionar um controle CalendarView da UWP usando ilhas de XAML](modernize-wpf-tutorial-3.md).

---
description: Este tutorial demonstra como adicionar interfaces de usuário do UWP XAML, criar pacotes do MSIX e incorporar outros componentes modernos ao seu aplicativo do WPF.
title: Adicionar um controle InkCanvas da UWP usando ilhas de XAML
ms.topic: article
ms.date: 08/15/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 943b2d90564dc059ed487c1f7fb7b89e689681bb
ms.sourcegitcommit: 8cbc9ec62a318294d5acfea3dab24e5258e28c52
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70911586"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Parte 2: Adicionar um controle InkCanvas da UWP usando ilhas de XAML

Esta é a segunda parte de um tutorial que demonstra como modernizar um aplicativo de área de trabalho WPF de exemplo chamado contoso despesas. Para obter uma visão geral do tutorial, dos pré-requisitos e das instruções para baixar o aplicativo de exemplo [, consulte o tutorial: Modernizar um aplicativo](modernize-wpf-tutorial.md)do WPF. Este artigo pressupõe que você já concluiu a [parte 1](modernize-wpf-tutorial-1.md).

No cenário fictício deste tutorial, a equipe de desenvolvimento da Contoso deseja adicionar suporte para assinaturas digitais ao aplicativo contoso despesas. O controle **InkCanvas** do UWP é uma ótima opção para esse cenário, pois ele dá suporte a tinta digital e a recursos do ia, como a capacidade de reconhecer texto e formas. Para fazer isso, você usará o controle UWP encapsulado do [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) , disponível no kit de ferramentas da Comunidade do Windows. Esse controle encapsula a interface e a funcionalidade do controle **InkCanvas** do UWP para uso em um aplicativo do WPF. Para obter mais detalhes sobre os controles UWP encapsulados, consulte [hospedar controles XAML do UWP em aplicativos de área de trabalho (Ilhas XAML)](xaml-islands.md).

> [!NOTE]
> Neste tutorial, o aplicativo do WPF hospedará apenas os controles UWP de primeira parte do SDK do Windows. Para dar suporte a outros cenários da ilha XAML, incluindo controles UWP personalizados, o projeto de aplicativo deve ter acesso `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` a uma instância da classe fornecida pelo kit de ferramentas da Comunidade do Windows. A maneira recomendada para fazer isso é adicionar um projeto de **aplicativo em branco (universal do Windows)** à mesma solução que o projeto do WPF (ou Windows Forms) e revisar a classe padrão `App` neste projeto. Como essa etapa não é necessária para o cenário básico de Hospedagem de controles UWP primários da SDK do Windows, este tutorial omite essa etapa. Para obter mais detalhes, consulte [Este artigo](host-standard-control-with-xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Configurar o projeto para usar ilhas XAML

Antes de adicionar um controle **InkCanvas** ao aplicativo contoso despesas, primeiro você precisa configurar o projeto para dar suporte a ilhas UWP XAML.

1. No Visual Studio 2019, clique com o botão direito do mouse no projeto **ContosoExpenses. Core** no **Gerenciador de soluções** e escolha **gerenciar pacotes NuGet**.

    ![Menu gerenciar pacotes NuGet no Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. Na janela **Gerenciador de pacotes NuGet** , clique em **procurar**. Selecione a opção **incluir pré-lançamento** , pesquise o `Microsoft.Toolkit.Wpf.UI.Controls` pacote e instale a versão de visualização mais recente do pacote mostrada nos resultados. Certifique-se de instalar a versão 6.0.0-preview7 ou uma versão posterior.

    > [!NOTE]
    > Este pacote contém toda a infraestrutura necessária para hospedar ilhas de XAML do UWP em um aplicativo do WPF, incluindo o controle do UWP empacotado do **InkCanvas** . Um pacote semelhante chamado `Microsoft.Toolkit.Forms.UI.Controls` está disponível para aplicativos Windows Forms.

3. Clique com o botão direito do mouse em projeto **ContosoExpenses. Core** em **Gerenciador de soluções** e escolha **Adicionar-> novo item**.

4. Selecione o **arquivo de manifesto do aplicativo**, nomeie-o **app. manifest**e clique em **Adicionar**. Para obter mais informações sobre manifestos de aplicativo, consulte [Este artigo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests).

5. No arquivo de manifesto, remova o comentário do `<supportedOS>` seguinte elemento para Windows 10.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. No arquivo de manifesto, localize o seguinte elemento `<application>` comentado.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. Exclua esta seção e substitua-a pelo XML a seguir. Isso configura o aplicativo para ser compatível com DPI e lida melhor com fatores de dimensionamento diferentes com suporte do Windows 10.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. Salve e feche o `app.manifest` arquivo.

9. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **ContosoExpenses. Core** e escolha **Propriedades**.

10. Na seção **recursos** da guia **aplicativo** , verifique se a lista suspensa **manifesto** está definida como **app. manifest**.

    ![Manifesto do aplicativo .NET Core](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. Salve as alterações nas propriedades do projeto.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Adicionar um controle InkCanvas ao aplicativo

Agora que você configurou seu projeto para usar as ilhas do UWP XAML, agora você está pronto para adicionar um controle UWP encapsulado do [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) ao aplicativo.

1. Em **Gerenciador de soluções**, expanda a pasta exibições do projeto **ContosoExpenses. Core** e clique duas vezes no arquivo **ExpenseDetail. XAML** .

2. No elemento **Window** próximo à parte superior do arquivo XAML, adicione o atributo a seguir. Isso faz referência ao namespace XAML para o controle UWP encapsulado do [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) .

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Depois de adicionar esse atributo, o elemento **Window** agora deve ser assim.

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

4. No arquivo **ExpenseDetail. XAML** , localize a marca de `</Grid>` fechamento que precede imediatamente o `<!-- Chart -->` comentário. Adicione o XAML a seguir logo antes da `</Grid>` marca de fechamento. Esse XAML adiciona um controle **InkCanvas** (prefixado pela palavra-chave do **Kit de ferramentas** que você definiu anteriormente como um namespace) e um **TextBlock** simples que atua como um cabeçalho para o controle.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Salve o arquivo **ExpenseDetail. XAML** .

6. Pressione F5 para executar o aplicativo no depurador.

7. Escolha um funcionário na lista e, em seguida, escolha uma das despesas disponíveis. Observe que a página de detalhes de despesas contém o espaço para o controle **InkCanvas** .

    ![Somente caneta da tela de tinta](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Se você tiver um dispositivo que dá suporte a uma caneta digital, como uma superfície, e você estiver executando este laboratório em um computador físico, vá e tente usá-lo. Você verá a tinta digital aparecendo na tela. No entanto, se você não tiver um dispositivo com capacidade de caneta e tentar entrar com o mouse, nada acontecerá. Isso ocorre porque o controle **InkCanvas** está habilitado apenas para canetas digitais por padrão. No entanto, podemos alterar esse comportamento.

8. Feche o aplicativo e clique duas vezes no arquivo **ExpenseDetail.XAML.cs** na pasta **views** do projeto **ContosoExpenses. Core** .

9. Adicione a seguinte declaração de namespace na parte superior da classe:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Localize o `ExpenseDetail()` Construtor.

11. Adicione a seguinte linha de código após o `InitializeComponent()` método e salve o arquivo de código.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Você pode usar o objeto InkPresenter para personalizar a experiência de tinta padrão. Esse código usa a propriedade **InputDeviceTypes** para habilitar o mouse, bem como a entrada de caneta.

12. Pressione F5 novamente para recompilar e executar o aplicativo no depurador. Escolha um funcionário na lista e, em seguida, escolha uma das despesas disponíveis.

13. Tente agora desenhar algo no espaço de assinatura com o mouse. Desta vez, você verá a tinta exibida na tela.

    ![Assinatura](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Próximas etapas

Neste ponto do tutorial, você adicionou com êxito um controle **InkCanvas** do UWP ao aplicativo de despesas da contoso. Agora você está pronto para [a parte 3: Adicione um controle CalendarView UWP usando ilhas](modernize-wpf-tutorial-3.md)XAML.

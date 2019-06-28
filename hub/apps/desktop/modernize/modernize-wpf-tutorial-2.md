---
description: Este tutorial demonstra como adicionar interfaces do usuário XAML UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo do WPF.
title: Adicionar um controle InkCanvas UWP usando ilhas de XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, do windows forms, wpf, Ilhas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 2f8cf18bce7bec880a2cb0bef298c0b565e20208
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420093"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Parte 2: Adicionar um controle InkCanvas UWP usando ilhas de XAML

Isso é a segunda parte de um tutorial que demonstra como modernizar um aplicativo da área de trabalho do WPF de exemplo chamado Contoso despesas. Para obter uma visão geral do tutorial, os pré-requisitos e instruções para baixar o aplicativo de exemplo, consulte [Tutorial: Modernize um aplicativo WPF](modernize-wpf-tutorial.md). Este artigo pressupõe que você já concluiu [parte 1](modernize-wpf-tutorial-1.md).

O cenário fictício deste tutorial, a equipe de desenvolvimento do Contoso deseja adicionar suporte para assinaturas digitais para o aplicativo de despesas da Contoso. A UWP **InkCanvas** controle é uma ótima opção para esse cenário, porque ele dá suporte à tinta digital e recursos com inteligência Artificial, como a capacidade de reconhecer formas e texto. Para fazer isso, você usará o [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulado de controle UWP disponível no Kit de ferramentas de comunidade do Windows. Esse controle encapsula a interface e a funcionalidade da UWP **InkCanvas** controle para uso em um aplicativo WPF. Para obter mais detalhes sobre os controles UWP encapsulados, consulte [controles de Host UWP XAML em aplicativos da área de trabalho (Ilhas de XAML)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Configurar o projeto para usar ilhas de XAML

Antes de adicionar um **InkCanvas** controle para o aplicativo de despesas de Contoso, você precisa primeiro configurar o projeto para dar suporte a UWP ilhas de XAML.

1. No Visual Studio de 2019, clique com botão direito no **ContosoExpenses.Core** project no **Gerenciador de soluções** e escolha **Manage NuGet Packages**.

    ![Gerenciar o menu de pacotes do NuGet no Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. No **Gerenciador de pacotes NuGet** janela, clique em **procurar**. Selecione o **incluir pré-lançamento** opção, procure o `Microsoft.Toolkit.Wpf.UI.Controls` empacotar e instalar a versão de visualização mais recente do pacote mostrado nos resultados.

    > [!NOTE]
    > Este pacote contém toda a infraestrutura necessária para a hospedagem de ilhas de XAML do UWP em um aplicativo do WPF, incluindo o **InkCanvas** encapsulado controle UWP. Um pacote semelhante denominado `Microsoft.Toolkit.Forms.UI.Controls` está disponível para aplicativos do Windows Forms.

3. Clique com botão direito **ContosoExpenses.Core** project no **Gerenciador de soluções** e escolha **Adicionar -> novo item**.

4. Selecione **o arquivo de manifesto de aplicativo**, nomeie- **manifest**e clique em **adicionar**.

5. No arquivo de manifesto aberto, localize o **compatibilidade** seção e identificar a seguinte entrada comentada.

    ```xml
    <!-- Windows 10 -->
    <!--<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />-->
    ```

6. Abaixo dessa entrada, adicione o item a seguir.

    ```xml
    <maxversiontested Id="10.0.18362.0"/>
    ```

7. Remova os **supportedOS** entrada para o Windows 10. Esta seção deve ficar assim.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    <maxversiontested Id="10.0.18362.0"/>
    ```

    > [!NOTE]
    > Essa entrada especifica que o aplicativo requer o Windows 10, versão 1903 (build 18362) ou posterior. Isso é a primeira versão do Windows 10 que dá suporte a ilhas de XAML. Sem essa entrada no manifesto do aplicativo, o aplicativo irá acionar uma exceção em tempo de execução.

8. No arquivo de manifesto, localize o seguinte comentado **aplicativo** seção.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

9. Excluir esta seção e substituí-lo com o XML a seguir. Isso configura o aplicativo para ser DPI identificador melhor e com suporte a diferente fatores de dimensionamento tem suportado do Windows 10.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

10. Salve e feche o `app.manifest` arquivo.

12. Na **Gerenciador de soluções**, clique com botão direito do **ContosoExpenses.Core** do projeto e escolha **propriedades**.

13. No **recursos** seção o **aplicativo** guia, certifique-se o **manifesto** lista suspensa é definida como **manifest**.

    ![Manifesto de aplicativo do .NET core](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

16. Salve as alterações às propriedades do projeto.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Adicionar um controle InkCanvas ao aplicativo

Agora que você configurou seu projeto para usar ilhas de XAML UWP, agora você está pronto para adicionar um [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulado controle UWP para o aplicativo.

1. No **Gerenciador de soluções**, expanda o **exibições** pasta do **ContosoExpenses.Core** do projeto e clique duas vezes o **ExpenseDetail.xaml**arquivo.

2. No **janela** elemento na parte superior do arquivo XAML, adicione o seguinte atributo. Isso faz referência ao namespace XAML para o [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulado controle UWP.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Depois de adicionar esse atributo, o **janela** elemento deve ficar assim.

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

4. No **ExpenseDetail.xaml** do arquivo, localize o fechamento `</Grid>` marca que precede imediatamente o `<!-- Chart -->` comentário. Adicione o seguinte XAML antes do fechamento `</Grid>` marca. Esse XAML adiciona uma **InkCanvas** controle (prefixado pela **toolkit** você definiu anteriormente como um namespace de palavra-chave) e um simples **TextBlock** que atua como um cabeçalho para o controle.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Salvar a **ExpenseDetail.xaml** arquivo.

6. Pressione F5 para executar o aplicativo no depurador.

7. Escolha um funcionário da lista e, em seguida, escolha uma das despesas disponíveis. Observe que a página de detalhes de despesas contém espaço para o **InkCanvas** controle.

    ![Somente a caneta de tela de tinta](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Se você tiver um dispositivo que dá suporte a uma caneta digital, como uma superfície, e você estiver executando este laboratório em uma máquina física, vá e tentar usá-lo. Você verá a tinta digital que aparecem na tela. No entanto, se você não tiver um dispositivo com capacidade de caneta e você tentar fazer logon com o mouse, nada acontecerá. Isso está acontecendo porque o **InkCanvas** controle é habilitada somente para canetas digitais por padrão. No entanto, podemos alterar esse comportamento.

8. Feche o aplicativo e clique duas vezes o **ExpenseDetail.xaml.cs** arquivo sob o **modos de exibição** pasta do **ContosoExpenses.Core** projeto.

9. Adicione a seguinte declaração de namespace na parte superior da classe:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Localize o `ExpenseDetail()` construtor.

11. Adicione a seguinte linha de código diretamente após o `InitializeComponent()` método e salve o arquivo de código.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Você pode usar o **InkPresenter** objeto para personalizar a experiência de escrita de padrão. Esse código usa o **InputDeviceTypes** propriedade para habilitar o mouse, bem como a entrada de caneta.

12. Pressione F5 novamente para recompilar e executar o aplicativo no depurador. Escolha um funcionário da lista e, em seguida, escolha uma das despesas disponíveis.

13. Experimente agora desenhar algo no espaço de assinatura com o mouse. Neste momento, você verá a tinta que aparecem na tela.

    ![Assinatura](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Próximas etapas

Neste ponto no tutorial, você adicionou com êxito um UWP **InkCanvas** controle para o aplicativo de despesas da Contoso. Agora você está pronto para [parte 3: Adicionar um controle de exibição de calendário UWP usando XAML Ilhas](modernize-wpf-tutorial-3.md).

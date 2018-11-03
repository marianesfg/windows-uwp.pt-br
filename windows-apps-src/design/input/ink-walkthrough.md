---
author: Karl-Bridge-Microsoft
ms.assetid: ''
title: Oferecer suporte à tinta no aplicativo UWP
description: Um tutorial passo a passo para adicionar suporte à tinta ao aplicativo UWP.
keywords: tinta, escrita à tinta, tutorial
ms.author: kbridge
ms.date: 01/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 62c62aacd894163ef2c65b9ddfe6d8299733a2e5
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5985953"
---
# <a name="tutorial-support-ink-in-your-uwp-app"></a>Tutorial: Oferecer suporte à tinta no aplicativo UWP

![Caneta Surface](images/ink/ink-hero-small.png)  
*Caneta Surface* (disponível para compra na [Microsoft Store](https://aka.ms/purchasesurfacepen)).

Este tutorial demonstra como criar um aplicativo UWP (Plataforma Universal do Windows) básico que oferece suporte a gravação e desenho com o Windows Ink. Usamos trechos de um aplicativo de exemplo, que você pode baixar no GitHub (consulte [Código de exemplo](#sample-code)), para demonstrar os diversos recursos e APIs associadas do Windows Ink (consulte [Componentes da plataforma Windows Ink](#components-of-the-windows-ink-platform)) abordados em cada etapa.

Centraremos a atenção no seguinte:
* Adicionando suporte básico a tinta
* Adicionando uma barra de ferramentas de tinta
* Oferecendo suporte a reconhecimento de manuscrito
* Oferecendo suporte básico a reconhecimento de formas
* Salvando e carregando tinta

Para obter mais detalhes sobre como implementar esses recursos, consulte [Interações da Caneta e Windows Ink nos aplicativos UWP](https://docs.microsoft.com/windows/uwp/input/pen-and-stylus-interactions).

## <a name="introduction"></a>Introdução

Com o Windows Ink, você pode fornecer aos clientes o equivalente digital de praticamente qualquer experiência de caneta e papel imaginável, desde anotações manuscritas rápidas a demonstrações no quadro de comunicações, desde desenhos arquitetônicos e de engenharia a obras de arte pessoais.

## <a name="prerequisites"></a>Pré-requisitos

* Um computador (ou uma máquina virtual) executando a versão atual do Windows 10
* [Visual Studio 2017 e o SDK do RS2](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Dependendo da configuração, talvez seja necessário instalar o pacote do NuGet [Microsoft.NETCore.UniversalWindowsPlatform](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/6.1.9) e habilitar o **modo de desenvolvedor** nas configurações do sistema (Configurações -> atualização & segurança -> para desenvolvedores -> Use recursos de desenvolvedor).
* Se você for novato no desenvolvimento de aplicativos UWP (Plataforma Universal do Windows) com o Visual Studio, dê uma olhada nestes tópicos antes de iniciar este tutorial:  
    * [Prepare-se para começar](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [Criar um aplicativo "Olá, Mundo" (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)
* **[OPCIONAL]** Uma caneta digital e um computador com um monitor que ofereça suporte a entrada nessa caneta digital.

> [!NOTE] 
> Embora o Windows Ink possa oferecer suporte a desenho com mouse e toque (mostramos como fazer isso na etapa 3 deste tutorial) para proporcionar uma experiência ideal do Windows Ink, recomendamos que você tenha uma caneta digital e um computador com um monitor que ofereça suporte a entrada nessa caneta digital.

## <a name="sample-code"></a>Código de exemplo
Neste tutorial, usamos um aplicativo de tinta de exemplo para demonstrar os conceitos e a funcionalidade abordados.

Baixe este código-fonte e de exemplo do Visual Studio no [GitHub](https://github.com/) em [windows-appsample-get-started-ink sample](https://aka.ms/appsample-ink):

1. Selecione o botão verde **Clone or download**  
![Clonando o repositório](images/ink/ink-clone.png)
2. Se você tiver uma conta do GitHub, poderá clonar o repositório no computador local escolhendo **Open in Visual Studio** 
3. Se você não tiver uma conta do GitHub ou apenas quiser uma cópia local do projeto, escolha **Download ZIP** (você precisará fazer verificações regulares para baixar as atualizações mais recentes)

> [!IMPORTANT]
> Grande parte do código no exemplo é comentado. À medida que percorrermos cada etapa, você será solicitado a remover os comentários das várias seções do código. No Visual Studio, basta realçar as linhas de código, pressionar CTRL-K e, em seguida, CTRL-U.

## <a name="components-of-the-windows-ink-platform"></a>Componentes da plataforma Windows Ink

Esses objetos fornecem a maior parte da experiência de escrita à tinta em aplicativos UWP.

| Componente | Descrição |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) | Um controle de plataforma XAMLUI que, por padrão, recebe e exibe todas as entradas de uma caneta como um traço de tinta ou um traço para apagar. |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | Objeto code-behind, instanciado juntamente com um controle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (exposto por meio da propriedade [**InkCanvas.InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter)). Esse objeto fornece todas as funcionalidades de escrita à tinta padrão expostas pelo [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), juntamente com um conjunto abrangente de APIs para personalização adicional. |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | Um controle de plataforma XAMLUI que contém uma coleção personalizável e extensível de botões que ativam recursos relacionados à tinta em um associado [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas). |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263)<br/>Não abordaremos funcionalidade aqui; para obter mais informações, consulte o [Exemplo de tinta complexo](http://go.microsoft.com/fwlink/p/?LinkID=620314). | Permite a renderização de traços de tinta no contexto designado do dispositivo Direct2D de um aplicativo universal do Windows, e não no controle padrão [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535). |

## <a name="step-1-run-the-sample"></a>Etapa 1: Executar o exemplo

Após baixar o aplicativo de exemplo RadialController, verifique se ele é executado:
1. Abra o projeto de exemplo no Visual Studio.
2. Defina a lista suspensa **Solution Platforms** para uma seleção não ARM.
3. Pressione F5 para compilar, implantar e executar.  

   > [!NOTE]
   > Se desejar, selecione o item de menu **Debug** > **Start debugging** ou selecione o botão **Local Machine** mostrado aqui.
   > ![Botão de projeto Visual Studio Build](images/ink/ink-vsrun-small.png)

A janela do aplicativo será aberta e, depois que uma tela inicial aparecer por alguns segundos, você verá esta tela inicial.

![Aplicativo vazio](images/ink/ink-app-step1-empty-small.png)

Agora temos o aplicativo UWP básico que usaremos durante todo o restante deste tutorial. Nas etapas a seguir, adicionamos nossa funcionalidade de tinta.

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>Etapa 2: Usar o InkCanvas para oferecer suporte básico à escrita à tinta

Talvez você já tenha observado que o aplicativo, em sua forma inicial, não permite que você desenhe nada com a caneta (embora você possa usar a caneta como um dispositivo apontador padrão para interagir com o aplicativo). 

Vamos corrigir essa pequena lacuna nesta etapa.

Para adicionar funcionalidade básica de escrita à tinta, basta colocar um controle de plataforma UWP [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) na página apropriada do aplicativo.

> [!NOTE]
> Um InkCanvas tem propriedades padrão [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) e [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width) igual a zero, a menos que ele seja o filho de um elemento que dimensiona automaticamente seus elementos filhos. 

### <a name="in-the-sample"></a>No exemplo:
1. Abra o arquivo MainPage.xaml.cs.
2. Localize o código marcado com o título desta etapa ("// Etapa 2: Usar o InkCanvas para oferecer suporte básico à escrita à tinta").
3. Remova o comentário das linhas a seguir. (Essas referências são necessárias à funcionalidade usada nas etapas subsequentes).  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. Abra o arquivo MainPage.xaml.
5. Localize o código marcado com o título desta etapa ("\<!-- Etapa 2: Escrita à tinta básica com InkCanvas-->").
6. Remova o comentário da linha a seguir.  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

Pronto! 

Agora execute o aplicativo novamente. Vá em frente e rabisque, escreva seu nome ou (se você estiver segurando um espelho ou tem uma memória muito boa) desenhe seu autorretrato.

![Escrita à tinta básica](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>Etapa 3: Suporte à escrita à tinta com toque e mouse

Você observará que, por padrão, há suporte à tinta somente para entrada de caneta. Se você tentar escrever ou desenhar com o dedo, o mouse ou o touchpad, ficará desapontado.

Para mudar essa realidade, adicione uma segunda linha de código. Dessa vez, ela está no code-behind do arquivo XAML no qual você declarou o [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas). 

Nesta etapa, apresentamos o objeto [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter), que fornece um gerenciamento mais refinado da entrada, processamento e renderização da entrada da tinta (padrão e modificada) no [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas).

> [!NOTE]
> A entrada de tinta padrão (ponta da caneta ou da borracha/botão) não é modificada com uma funcionalidade de hardware secundária, como um botão de caneta, botão direito do mouse ou mecanismo semelhante. 

Para habilitar a escrita à tinta por mouse e toque, defina a propriedade [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) do [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) para a combinação de valores [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) que você deseja.

### <a name="in-the-sample"></a>No exemplo:
1. Abra o arquivo MainPage.xaml.cs.
2. Localize o código marcado com o título desta etapa ("// Etapa 3: Oferecer suporte à escrita à tinta com toque e mouse").
3. Remova o comentário das linhas a seguir.  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

Execute o aplicativo novamente, e você constatará que todos os seus sonhos de pintar a tela de um computador com o dedo se tornaram realidade!

> [!NOTE]
> Ao especificar os tipos de dispositivo de entrada, você deve indicar o suporte para cada tipo de entrada específico (incluindo a caneta), porque a configuração dessa propriedade substitui a configuração padrão do [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas).

## <a name="step-4-add-an-ink-toolbar"></a>Etapa 4: Adicionar uma barra de ferramentas de tinta

O [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) é um controle de plataforma UWP que fornece um conjunto personalizável e extensível de botões que ativa recursos relacionados à tinta. 

Por padrão, o [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) inclui um conjunto básico de botões que permitem aos usuários selecionar rapidamente entre uma caneta, um lápis, um marca-texto ou uma borracha, qualquer um desses pode ser usado com um estêncil (régua ou transferidor). Os botões de caneta, lápis e marca-texto também fornecem um submenu para a seleção da cor da tinta e do tamanho do traço.

Para adicionar um [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) padrão a um aplicativo de escrita à tinta, basta colocá-lo na mesma página que o [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) e associe os dois controles.

### <a name="in-the-sample"></a>No exemplo
1. Abra o arquivo MainPage.xaml.
2. Localize o código marcado com o título desta etapa ("\<!-- Etapa 4: Adicionar uma barra de ferramentas de tinta -->").
3. Remova o comentário das linhas a seguir.  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> Para manter a interface do usuário e o código o mais organizado e simples possível, usamos um layout de grade básico e declaramos o [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) após o [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) em uma linha de grade. Se você declará-lo antes do [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), o [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) será renderizado primeiro, abaixo do canvas e de forma inacessível ao usuário.  

Agora execute o aplicativo novamente para ver o [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) e experimente algumas ferramentas.

![InkToolbar no bloco de esboços do Espaço de Trabalho do Ink](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>Desafio: Adicionar um botão personalizado
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar no bloco de esboços do Espaço de Trabalho do Ink](images/challenge-icon.png)

</td>
<td>

Aqui está um exemplo de um **[InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)** personalizado (no bloco de esboços do Espaço de Trabalho do Windows Ink).

![InkToolbar no bloco de esboços do Espaço de Trabalho do Ink](images/ink/ink-inktoolbar-sketchpad-small.png)

Para obter mais detalhes sobre como personalizar um [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), consulte [Adicionar um InkToolbar a um aplicativo de escrita à tinta UWP (Plataforma Universal do Windows)](ink-toolbar.md).

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>Etapa 5: Oferecer suporte a reconhecimento de manuscrito

Agora que você pode escrever e desenhar no aplicativo, vamos tentar fazer algo útil com esses rabiscos.

Nesta etapa, usamos os recursos de reconhecimento de manuscrito do Windows Ink para tentar decifrar o que você escreveu.

> [!NOTE]
> É possível melhorar o reconhecimento de manuscrito por meio das configurações de **Caneta e Windows Ink**:
> 1. Abra o menu Iniciar e selecione **Configurações**.
> 2. Na tela Configurações, selecione **Dispositivos** > **Caneta e Windows Ink**.
> ![InkToolbar no bloco de esboços do Espaço de Trabalho do Ink](images/ink/ink-settings-small.png)
> 3. Selecione **Conhecer meu manuscrito** para abrir a caixa de diálogo **Personalização de Manuscrito**.
> ![InkToolbar no bloco de esboços do Espaço de Trabalho do Ink](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>No exemplo:
1. Abra o arquivo MainPage.xaml.
2. Localize o código marcado com o título desta etapa ("\<!-- Etapa 5: Oferecer suporte a reconhecimento de manuscrito -->").
3. Remova o comentário das linhas a seguir.  

``` xaml
    <Button x:Name="recognizeText" 
            Content="Recognize text"  
            Grid.Row="0" Grid.Column="0"
            Margin="10,10,10,10"
            Click="recognizeText_ClickAsync"/>
    <TextBlock x:Name="recognitionResult" 
                Text="Recognition results: "
                VerticalAlignment="Center" 
                Grid.Row="0" Grid.Column="1"
                Margin="50,0,0,0" />
```

4. Abra o arquivo MainPage.xaml.cs.
5. Localize o código marcado com o título desta etapa (" Etapa 5: Oferecer suporte a reconhecimento de manuscrito").
6. Remova o comentário das linhas a seguir.  

- Essas são as variáveis globais necessárias a essa etapa.

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- Este é o manipulador do botão **Recognize text**, no qual podemos executar o processamento de reconhecimento.

``` csharp
    private async void recognizeText_ClickAsync(object sender, RoutedEventArgs e)
    {
        strokesText = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (strokesText.Count > 0)
        {
            analyzerText.AddDataForStrokes(strokesText);

            resultText = await analyzerText.AnalyzeAsync();

            if (resultText.Status == InkAnalysisStatus.Updated)
            {
                words = analyzerText.AnalysisRoot.FindNodes(InkAnalysisNodeKind.InkWord);
                foreach (var word in words)
                {
                    InkAnalysisInkWord concreteWord = (InkAnalysisInkWord)word;
                    foreach (string s in concreteWord.TextAlternates)
                    {
                        recognitionResult.Text += s;
                    }
                }
            }
            analyzerText.ClearDataForAllStrokes();
        }
    }
```

7. Execute o aplicativo novamente, escreva algo e clique no botão **Recognize text**
8. Os resultados do reconhecimento são exibidos ao lado do botão

### <a name="challenge-1-international-recognition"></a>Desafio 1: Reconhecimento internacional
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar no bloco de esboços do Espaço de Trabalho do Ink](images/challenge-icon.png)

</td>
<td>

O Windows Ink oferece suporte ao reconhecimento de texto em muitos idiomas para os quais o Windows oferece suporte. Cada pacote de idiomas inclui um mecanismo de reconhecimento de manuscrito que pode ser instalado com o pacote de idiomas.

Direcione um idioma específico consultando os mecanismos de reconhecimento de manuscrito instalados.

Para obter mais detalhes sobre o reconhecimento de manuscrito internacional, consulte [Reconhecer traços do Windows Ink como texto](convert-ink-to-text.md).

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>Desafio 2: Reconhecimento dinâmico
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar no bloco de esboços do Espaço de Trabalho do Ink](images/challenge-icon.png)

</td>
<td>

Neste tutorial, solicitamos que um botão seja pressionado para iniciar o reconhecimento. Você também pode executar o reconhecimento dinâmico usando uma função básica de sincronização.

Para obter mais detalhes sobre o reconhecimento dinâmico, consulte [Reconhecer traços do Windows Ink como texto](convert-ink-to-text.md).

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>Etapa 6: Reconhecer formas

Agora você pode converter anotações manuscritas em algo um pouco mais legível. Mas e aqueles rabiscos tremidos e cafeinados feitos na reunião anônima sobre fluxogramas pela manhã?

Com a análise de tinta, seu aplicativo também pode reconhecer um conjunto de formas básicas, incluindo:

- Circle
- Diamond
- Drawing
- Ellipse
- EquilateralTriangle
- Hexágono
- IsoscelesTriangle
- Parallelogram
- Pentagon
- Quadrilateral
- Rectangle
- RightTriangle
- Square
- Trapezoid
- Triangle

Nesta etapa, usamos os recursos de reconhecimento de formas do Windows Ink para tentar limpar seus rabiscos.

Neste exemplo, não tente redesenhar traços de tinta (embora isso seja possível). Em vez disso, adicionamos uma tela padrão abaixo do InkCanvas na qual podemos desenhar objetos Ellipse ou Polygon equivalentes derivados da tinta original. Em seguida, excluímos os traços de tinta correspondentes.

### <a name="in-the-sample"></a>No exemplo:
1. Abra o arquivo MainPage.xaml
2. Localize o código marcado com o título desta etapa ("\<!-- Etapa 6: Reconhecer formas-->")
3. Remova o comentário desta linha.  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. Abra o arquivo MainPage.xaml.cs
5. Localize o código marcado com o título desta etapa ("// Etapa 6: Reconhecer formas")
6. Remova o comentário destas linhas:  

``` csharp
    private async void recognizeShape_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        ...
    }

    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        ...
    }
```

7. Execute o aplicativo, desenhe algumas formas e clique no botão **Recognize shape**

Este é um exemplo de fluxograma rudimentar de um guardanapo digital.

![Fluxograma de tinta original](images/ink/ink-app-step6-shapereco1-small.png)

Este é o mesmo fluxograma após o reconhecimento de formas.

![Fluxograma de tinta original](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>Etapa 7: Salvar e carregar tinta

Você terminou de rabiscar e gosta do que vê, mas acha que ajustaria algumas coisas mais tarde? Salve os traços de tinta em um arquivo ISF e carregue-os para edição sempre que se sentir inspirado. 

O arquivo ISF é uma imagem GIF básica que inclui metadados adicionais que descrevem comportamentos e propriedades do traço de tinta. Os aplicativos que não são habilitados para tinta podem ignorar os metadados extras e ainda carregar a imagem GIF básica (incluindo transparência de plano de fundo de canal alfa).

Nesta etapa, conectamos os botões **Salvar** e **Carregar** localizados ao lado da barra de ferramentas de tinta.

### <a name="in-the-sample"></a>No exemplo:
1. Abra o arquivo MainPage.xaml.
2. Localize o código marcado com o título desta etapa ("\<!-- Etapa 7: Salvar e carregar tinta-->").
3. Remova o comentário das linhas a seguir. 

``` xaml
    <Button x:Name="buttonSave" 
            Content="Save" 
            Click="buttonSave_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
    <Button x:Name="buttonLoad" 
            Content="Load"  
            Click="buttonLoad_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
```

4. Abra o arquivo MainPage.xaml.cs.
5. Localize o código marcado com o título desta etapa ("// Etapa 7: Salvar e carregar tinta").
6. Remova o comentário das linhas a seguir.  

``` csharp
    private async void buttonSave_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private async void buttonLoad_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }
```

7. Execute o aplicativo e desenhe algo.
8. Selecione o botão **Salvar** e salve o desenho.
9. Apague a tinta ou reinicie o aplicativo.
10. Selecione o botão **Carregar** e abra o arquivo de tinta que acabou de salvar.

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>Desafio: Usar a área de transferência para copiar e colar traços de tinta 
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar no bloco de esboços do Espaço de Trabalho do Ink](images/challenge-icon.png)

</td>

<td>

O Windows Ink também oferece suporte à cópia e colagem de traços de tinta de/para a área de transferência.

Para obter mais detalhes sobre como usar a área de transferência com tinta, consulte [Armazenar e recuperar dados de traço do Windows Ink](save-and-load-ink.md).

</td>
</tr>
</table>

## <a name="summary"></a>Resumo

Parabéns, você concluiu o tutorial **Entrada: Suporte a tinta no aplicativo UWP**! Mostramos a você o código básico necessário para oferecer suporte à tinta nos aplicativos UWP e como proporcionar algumas das experiências de usuário mais sofisticadas compatíveis com a plataforma Windows Ink.

## <a name="related-articles"></a>Artigos relacionados

* [Interações com caneta e Windows Ink em aplicativos UWP](pen-and-stylus-interactions.md)

### <a name="samples"></a>Exemplos

* [Exemplo de análise de tinta (básico) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [Exemplo de reconhecimento de manuscrito à tinta (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)
* [Salvar e carregar traços de tinta de um arquivo ISF (Ink Serialized Format)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [Salvar e carregar traços de tinta da área de transferência](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)
* [Exemplo de localização e orientação da barra de ferramentas de tinta (básico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [Exemplo de localização e orientação da barra de ferramentas de tinta (dinâmico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)
* [Exemplo de tinta simples (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Amostra de tinta complexa (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Exemplo de tinta (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Tutorial de Introdução: oferecer suporte à tinta em seu aplicativo UWP](https://aka.ms/appsample-ink)
* [Exemplo de livro de colorir](https://aka.ms/cpubsample-coloringbook)
* [Exemplo de anotações da família](https://aka.ms/cpubsample-familynotessample)

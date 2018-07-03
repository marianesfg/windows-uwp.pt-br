---
author: serenaz
title: Introdução ao Windows ML
description: Crie seu primeiro aplicativo do Windows ML com este tutorial passo a passo.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, windows machine learning, winml, windows ML
ms.localizationpriority: medium
ms.openlocfilehash: eec2ada8e3aadad134381a93bca2652133912b2e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843619"
---
# <a name="get-started-with-windows-ml"></a>Introdução ao Windows ML

Neste tutorial, criaremos um aplicativo UWP simples que usa um modelo de aprendizado de máquina treinado para reconhecer um dígito numérico desenhado pelo usuário. Este tutorial se concentra principalmente em como carregar e usar o Windows ML em seu app.

## <a name="prerequisites"></a>Pré-requisitos

- [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) (Build 17110 ou superior)
- [Visual Studio](https://developer.microsoft.com/windows/downloads)

## <a name="1-download-the-sample"></a>1. Baixar o exemplo

Primeiro, você precisará baixar nosso [Exemplo MNIST_GetStarted](https://github.com/Microsoft/Windows-Machine-Learning) do GitHub. Fornecemos um modelo com controles e eventos XAML implementados, incluindo:

- Uma [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) para desenhar o dígito.
- [Buttons](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button) para interpretar o dígito e limpar a tela.
- Rotinas auxiliares para converter a saída de InkCanvas em um [VideoFrame](https://docs.microsoft.com/uwp/api/windows.media.videoframe).

Uma exemplo MNIST concluído também está disponível para download do GitHub.

## <a name="2-open-project-in-visual-studio-preview"></a>2. Abrir o projeto no Visual Studio Preview

Inicie o Visual Studio Preview e abra o aplicativo de exemplo MNIST. (Se a solução for mostrada como indisponível, você precisará clicar com botão direito do mouse e selecionar "Recarregar Projeto").

Dentro do gerenciador de soluções, o projeto tem três arquivos de código principais:

- `MainPage.xaml` - Todo o seu código XAML para criar a interface do usuário para a InkCanvas, botões e rótulos.
- `MainPage.xaml.cs` - Onde está o código do nosso aplicativo.
- `Helper.cs` - Rotinas auxiliares para recortar e converter formatos de imagem.

![Gerenciador de soluções do Visual Studio com arquivos de projeto](images/get-started1.png)

## <a name="3-build-and-run-project"></a>3. Criar e executar o projeto

Na barra de ferramentas do Visual Studio, altere a Plataforma de Solução de "ARM" para "x64" para executar o projeto em seu computador local.

Para executar o projeto, clique no botão **Iniciar Depuração** na barra de ferramentas ou pressione F5. O aplicativo deve mostrar uma InkCanvas onde os usuários podem escrever um dígito, um botão "Reconhecer" para interpretar o número, um campo de rótulo vazio onde o dígito interpretado será exibido como texto e um botão "Limpar Dígito" para Limpar a InkCanvas.

![Captura de tela do aplicativo](images/get-started2.png)

**Observação**: se você tiver baixado um Build superior ao 17110, talvez seja necessário alterar a versão de destino de implantação do projeto. Na solução de projeto, vá para **Propriedades**. Na guia Aplicativo, defina a versão de destino para corresponder ao seu sistema operacional e SDK.

## <a name="4-download-a-model"></a>4. Baixar um modelo

Em seguida, vamos obter um modelo de aprendizado de máquina para adicionar ao nosso app. Para este tutorial, usaremos um **modelo MNIST** previamente treinado com o [Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) e [exportado para o formato ONNX](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb).

Se você estiver usando o exemplo MNIST_GetStarted do GitHub, o modelo MNIST já foi incluído em sua pasta de ativos, e você precisará adicioná-lo ao seu aplicativo como um item existente. Você também pode baixar o modelo previamente treinado do [ONNX Model Zoo](https://github.com/onnx/models) no GitHub.

Se você estiver interessado em treinar seu próprio modelo, poderá seguir este [tutorial](train-ai-model.md) que usamos para treinar esse modelo MNIST.

## <a name="5-add-the-model"></a>5. Adicionar o modelo

Depois de baixar o modelo MNIST, clique com o botão direito do mouse na pasta de ativos no Gerenciador de Soluções e selecione "**Adicionar** > **Item Existente**". Aponte o seletor de arquivos para a localização do seu modelo ONNX e clique em adicionar.

O projeto agora deve ter dois novos arquivos:

- `MNIST.onnx` - seu modelo treinado.
- `MNIST.cs` - o código gerado do Windows ML.

![Gerenciador de soluções com os novos arquivos](images/get-started3.png)

Para garantir que o modelo seja compilado com o nosso aplicativo, clique com botão direito do mouse no arquivo `mnist.onnx` e selecione **Propriedades**. Para **Ação de Compilação**, selecione **Conteúdo**.

Agora, vamos dar uma olhada no código recém-gerado no arquivo `MNIST.cs`. Temos três classes:

- **MNISTModel** cria a representação de modelo de aprendizado de máquina, associa as entradas e saídas específicas ao modelo e avalia o modelo de forma assíncrona. 
- **MNISTModelInput** inicializa os tipos de entrada que o modelo espera. Nesse caso, a entrada espera um VideoFrame.
- **MNISTModelOutput** inicializa os tipos que o modelo terá como saída. Nesse caso, a saída será uma lista chamada "classLabel" do tipo `<long>` e um Dicionário chamado "previsão" de tipo `<long, float>`

Agora, usaremos essas classes para carregar, associar e avaliar o modelo em nosso projeto.

## <a name="6-load-bind-and-evaluate-the-model"></a>6. Carregar, associar e avaliar o modelo

Para aplicativos do Windows ML, o padrão que queremos a seguir é: Carregar > Associar > Avaliar.

- Carregue o modelo de aprendizado de máquina.
- Associe entradas e saídas ao modelo.
- Avalie o modelo e exiba os resultados.

Vamos usar o código de interface gerado no `MNIST.cs` para carregar, associar e avaliar o modelo em nosso app.

Primeiro, em `MainPage.xaml.cs`, vamos instanciar o modelo, as entradas e as saídas.

```csharp
namespace MNIST_Demo
{
    public sealed partial class MainPage : Page
    {
        private MNISTModel ModelGen = new MNISTModel();
        private MNISTModelInput ModelInput = new MNISTModelInput();
        private MNISTModelOutput ModelOutput = new MNISTModelOutput();
        ...
    }
}
```

Em seguida, em LoadModel(), carregaremos o modelo.

```csharp
private async void LoadModel()
{
    //Load a machine learning model
    StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
    ModelGen = await MNISTModel.CreateMNISTModel(modelFile);
}
```

Depois disso, queremos associar nossa entradas e saídas ao modelo. 

Nesse caso, nosso modelo está esperando a entrada do tipo VideoFrame. Usando nossas funções auxiliares incluídas em `helper.cs`, copiaremos o conteúdo de InkCanvas, o converteremos no tipo VideoFrame e o associaremos ao nosso modelo.

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
     //Bind model input with contents from InkCanvas
     ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
}
```

Para a saída, simplesmente chamamos EvaluateAsync() com a entrada especificada. Como o modelo retorna uma lista de dígitos com uma probabilidade correspondente, precisamos analisar a lista retornada para determinar qual dígito tinha a maior probabilidade e exibi-lo.

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
    //Bind model input with contents from InkCanvas
    ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
    
    //Evaluate the model
    ModelOutput = await ModelGen.EvaluateAsync(ModelInput);
            
    //Iterate through evaluation output to determine highest probability digit
    float maxProb = 0;
    int maxIndex = 0;
    for (int i = 0; i < 10; i++)
    {
        if (ModelOutput.Plus214_Output_0[i] > maxProb)
        {
            maxIndex = i;
            maxProb = ModelOutput.Plus214_Output_0[i];
        }
    }
    numberLabel.Text = maxIndex.ToString();
}
```

Por fim, vamos querer limpar InkCanvas para permitir que os usuários desenhem outro número.

```csharp
private void clearButton_Click(object sender, RoutedEventArgs e)
{
    inkCanvas.InkPresenter.StrokeContainer.Clear();
    numberLabel.Text = "";
}
```

## <a name="7-launch-the-app"></a>7. Iniciar o app

Depois que criarmos e iniciarmos o app, poderemos reconhecer um número desenhado em InkCanvas.

![app completo](images/get-started4.png)

## <a name="8-next-steps"></a>8. Próximas etapas

Isso é tudo - você criou seu primeiro app Windows ML! Para obter mais exemplos que demonstrem como usar o Windows ML, confira [Aplicativos de exemplo](samples.md).
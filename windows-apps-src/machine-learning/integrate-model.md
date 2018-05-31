---
author: serenaz
title: Integrar um modelo ao seu app com o Windows ML
description: Integrar um modelo ao seu app seguindo o padrão de carga, associação e avaliação.
ms.author: sezhen
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, winml, Windows machine learning
ms.localizationpriority: medium
ms.openlocfilehash: 87454c46a08b80b8a315ede1d2e6a1f6cda909df
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815471"
---
# <a name="integrate-a-model-into-your-app-with-windows-ml"></a>Integrar um modelo ao seu app com o Windows ML

A [geração automática de código](overview.md#automatic-interface-code-generation) do Windows ML cria uma interface que chama as [APIs do Windows ML](/uwp/api/windows.ai.machinelearning.preview) para você, permitindo uma interação fácil com o seu modelo. Usando as classes wrapper geradas pela interface, você seguirá o padrão carga, associação e avaliação para integrar seu modelo do ML ao app.

![carregar, associar, avaliar](images/load-bind-evaluate.png)

Neste artigo, vamos usar o modelo MNIST da [Introdução](get-started.md) para demonstrar como carregar, associar e avaliar um modelo em seu app.

## <a name="add-the-model"></a>Adicionar o modelo

Primeiro, você precisará adicionar seu modelo ONNX aos Ativos do seu projeto do Visual Studio. Se você estiver criando um aplicativo UWP com o [Visual Studio (versão 15.7 - Preview 1)](https://www.visualstudio.com/vs/preview/), então o Visual Studio gerará automaticamente as classes wrapper em um novo arquivo. Para outros fluxos de trabalho, você precisará usar a ferramenta [mlgen](overview.md#automatic-interface-code-generation) para gerar as classes wrapper.

A seguir, as classes wrapper geradas pelo Windows ML para o modelo MNIST. Vamos usar o restante deste artigo para explicar como usar essas classes em seu app.

```csharp
public sealed class MNISTModelInput
{
    public VideoFrame Input3 { get; set; }
}

public sealed class MNISTModelOutput
{
    public IList<float> Plus214_Output_0 { get; set; }
    public MNISTModelOutput()
    {
        this.Plus214_Output_0 = new List<float>();
    }
}

public sealed class MNISTModel
{
    private LearningModelPreview learningModel;
    public static async Task<MNISTModel> CreateMNISTModel(StorageFile file)
    {
        LearningModelPreview learningModel = await LearningModelPreview.LoadModelFromStorageFileAsync(file);
        MNISTModel model = new MNISTModel();
        model.learningModel = learningModel;
        return model;
    }
    public async Task<MNISTModelOutput> EvaluateAsync(MNISTModelInput input) {
        MNISTModelOutput output = new MNISTModelOutput();
        LearningModelBindingPreview binding = new LearningModelBindingPreview(learningModel);
        binding.Bind("Input3", input.Input3);
        binding.Bind("Plus214_Output_0", output.Plus214_Output_0);
        LearningModelEvaluationResultPreview evalResult = await learningModel.EvaluateAsync(binding, string.Empty);
        return output;
    }
}
```

**Observação**: para garantir que seu modelo seja compilado com o seu aplicativo, clique com botão direito do mouse no arquivo `.onnx` e selecione **Propriedades**. Para **Ação de Compilação**, selecione **Conteúdo**.

## <a name="load"></a>Carga

Depois de ter as classes wrapper geradas, você carregará o modelo em seu app.

A classe MNISTModel representa o modelo MNIST e para carregar o modelo, chamamos o método CreateMNISTModel, passando o arquivo ONNX como o parâmetro.

```csharp
// Load the model
StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
MNISTModel model = MNISTModel.CreateMNISTModel(modelFile);
```

## <a name="bind"></a>Ligar

O código gerado também inclui classes wrapper Input e Output. A classe Input representa as entradas esperadas do modelo e a classe Output representa as saídas esperadas do modelo.

Para inicializar o objeto de entrada do modelo, chame o construtor da classe Input, passando os dados de aplicativos e certifique-se de que seus dados de entrada correspondam ao tipo de entrada que seu modelo espera. A classe MNISTModelInput espera um VideoFrame, portanto usamos um método auxiliar para obter um VideoFrame para a entrada.

```csharp
//Bind the input data to the model
MNISTModelInputs ModelInput = new MNISTModelInputs();
ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
```

Os objetos de saída são inicializados para valores *Null* e serão atualizados com os resultados do modelo após a próxima etapa, Avaliar.

## <a name="evaluate"></a>Avaliar

Depois que suas entradas forem inicializadas, chame o método EvaluateAsync do modelo para avaliar seu modelo nos dados de entrada. EvaluateAsync associa suas entradas e saídas ao objeto do modelo e avalia o modelo nas entradas.

```csharp
// Evaluate the model
MNISTModelOuput ModelOutput = model.EvaluateAsync(ModelInput);
```

Após a avaliação, a saída conterá resultados do modelo, que você agora pode exibir e analisar. Já que o modelo MNIST gera uma lista de probabilidades, nós iteramos pela lista para encontrar e exibir o dígito com a maior probabilidade.

```csharp
 //Iterate through output to determine highest probability digit
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
```

## <a name="using-the-windows-ml-apis-directly"></a>Como usar as APIs do Windows ML diretamente

Embora o gerador automático de código do Windows ML forneça uma interface conveniente para interagir com seu modelo, você não precisa usar as classes wrapper. Em vez disso, você pode chamar as APIs do Windows ML diretamente em seu app.
Se você optar por fazer isso, seguirá o mesmo padrão: carregar seu modelo, associar suas entradas e saídas e avaliar.

Para obter mais informações sobre como usar APIs, consulte a [Referência de API do Windows ML](/uwp/api/windows.ai.machinelearning.preview).

---
author: serenaz
title: Como treinar um modelo para o Windows ML no Visual Studio
description: Saiba como treinar um modelo para Windows ML usando o Visual Studio Tools for AI com este tutorial passo a passo.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Windows machine learning, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: 5d8b50b6b8779b98de1d93f449aa560b5dcda893
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690212"
---
# <a name="how-to-train-a-model-for-windows-ml-in-visual-studio"></a>Como treinar um modelo para o Windows ML no Visual Studio
Neste tutorial, usaremos o [Visual Studio Tools for AI](http://aka.ms/vstoolsforai), uma extensão de desenvolvimento para criar, testar e implantar soluções de Inteligência Artificial e Deep Learning, para treinar um modelo para app de exemplo MNIST na [Introdução](get-started.md).

Nós treinaremos o modelo com a estrutura [Microsoft Cognitive Toolkit (CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit) e o [conjunto de dados MNIST](http://yann.lecun.com/exdb/mnist/), que tem um conjunto de treinamento de 60.000 exemplos e um conjunto de testes de 10.000 exemplos de dígitos manuscritos. Depois, salvaremos o modelo usando o formato [ONNX (Open Neural Network Exchange)](https://onnx.ai/) a ser usado com o Windows ML. 

## <a name="prerequisites"></a>Pré-requisitos
### <a name="install-visual-studio-tools-for-ai"></a>Instalar o Visual Studio Tools for AI
Para começar, primeiro será necessário baixar e instalar o [Visual Studio](https://www.visualstudio.com/downloads/). Quando você tiver o Visual Studio aberto, ative a extensão **Visual Studio Tools for AI**:

1. Clique na barra de menus no Visual Studio e selecione "Extensões e Atualizações..."
2. Clique na guia "Online" e selecione "Pesquisar Visual Studio Marketplace".
3. Procure "Visual Studio Tools for AI". 
3. Clique no botão **Baixar**. 
4. Após a instalação, reinicie o Visual Studio. 

A extensão estará ativa assim que o Visual Studio for reiniciado. Se você estiver tendo problemas, confira [Como encontrar extensões do Visual Studio](hhttps://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions).

### <a name="download-sample-code"></a>Baixar código de exemplo
Baixe o repositório [Samples for AI](https://github.com/Microsoft/samples-for-ai) do GitHub. Os exemplos abrangem a introdução com aprendizado profundo em TensorFlow, CNTK, Theano e muito mais.

### <a name="install-cntk"></a>Instalar CNTK
Instale o [CNTK para Python no Windows](https://docs.microsoft.com/en-us/cognitive-toolkit/setup-windows-python?tabs=cntkpy24). Observe que você também precisará instalar o Python se ainda não o tiver.

Como alternativa, para preparar seu computador para o desenvolvimento do modelo de aprendizado profundo, consulte [como preparar seu ambiente de desenvolvimento](https://github.com/Microsoft/samples-for-ai/blob/master/README.md) para um instalador simplificado para a instalação do Python, CNTK, TensorFlow, drivers de GPU NVIDIA (opcional) e muito mais.

## <a name="1-open-project"></a>1. Abrir o projeto

Inicie o Visual Studio e selecione **Arquivo > Abrir > Projeto/Solução**. No repositório Samples for AI, selecione a pasta **examples\cntk\python** e abra o arquivo **CNTKPythonExamples.sln**.

![Abrir a solução](images/open-solution.png)

## <a name="2-train-the-model"></a>2. Treine o modelo

Para definir o projeto MNIST como o projeto de inicialização, clique com o botão direito do mouse no projeto python e selecione **Definir como Projeto de Inicialização**.

![Abrir a solução](images/mnist-startup.png)

Em seguida, abra o arquivo ConvNet_MNIST.py e **execute** o projeto pressionando F5 ou o botão Executar verde.

## <a name="3-view-the-model-and-add-it-to-your-app"></a>3. Exibir o modelo e adicioná-lo ao seu app

Abra a pasta **output/Models** no repositório Samples for AI. Haverá um arquivo de modelo DNN treinado para cada época do experimento de treinamento e a etapa final gravará o arquivo do modelo **MNIST.onnx**. 

![Abrir a solução](images/onnx-model-output.png)

Agora, você pode usar esse arquivo do modelo **MNIST.onnx** treinado para criar o app de exemplo MNIST na [Introdução](get-started.md)! 

## <a name="4-learn-more"></a>4. Saiba mais
Para saber como acelerar o treinamento de modelos de aprendizado profundo usando [Máquinas Virtuais do Azure GPU](https://docs.microsoft.com/en-us/visualstudio/ai/tensorflow-vm) e muito mais, visite [Inteligência Artificial na Microsoft](https://www.microsoft.com/ai) e [Tecnologias de Microsoft Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/#More-Microsoft-Machine-Learning-Technologies).
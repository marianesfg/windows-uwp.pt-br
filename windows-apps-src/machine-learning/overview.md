---
author: serenaz
title: Visão geral do Windows ML
description: Saiba mais sobre o Windows Machine Learning e como desenvolver com o Windows ML.
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, windows machine learning
ms.localizationpriority: medium
ms.openlocfilehash: 2ef6ea1a4e1dab23f5ff6a09aec9b8c49c135f5e
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817657"
---
# <a name="windows-ml-overview"></a>Visão geral do Windows ML

![Gráfico do Windows machine learning](images/brain.png)

## <a name="what-is-machine-learning"></a>O que é o aprendizado de máquina?

O aprendizado de máquina (ML) permite que os computadores usem os dados existentes para prever os comportamentos e resultados esperados. Ao processar os dados coletados anteriormente, os algoritmos de ML criam modelos que podem prever a saída correta quando apresentados a uma nova entrada. Por exemplo, um modelo pode ser treinado para avaliar mensagens de email (entrada) como spam ou não spam (saída).

A fase de criação de modelo é chamada de "treinamento". Depois de treinado com os dados existentes, o modelo pode realizar as previsões com dados novos e ainda não vistos, o que se chama "inferência", "avaliação" ou "pontuação".

Geralmente, os modelos treinados produzem resultados melhores do que os programas escritos para seguir um conjunto estrito de instruções, especialmente para tarefas complexas com muitas combinações possíveis de entradas e saídas. Por exemplo, os algoritmos de recomendação oferecem recomendações personalizadas para milhões de usuários em sites de comércio eletrônico e de streaming de mídia, o que seria quase impossível sem o aprendizado de máquina. Outro campo que aproveita o aprendizado de máquina é visão do computador, que permite que computadores classifiquem e identifiquem imagens após o treinamento sobre imagens rotuladas anteriormente.

As possibilidades e os aplicativos de aprendizado de máquina são infinitos; para obter mais informações sobre soluções e pesquisa, visite [Inteligência Artificial na Microsoft](https://www.microsoft.com/ai) e [plataforma de Inteligência Artificial da Microsoft](https://azure.microsoft.com/en-us/overview/ai-platform/). Se você quiser criar modelos de Aprendizado de Máquina e de Inteligência Artificial, também pode conferir [Serviços do Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/overview-what-is-azure-ml).

## <a name="what-is-windows-ml"></a>O que é o Windows ML?

O Windows ML é uma plataforma que avalia modelos de aprendizado de máquina treinados em dispositivos Windows 10, permitindo que os desenvolvedores usem o aprendizado de máquina em seus aplicativos do Windows.

Alguns destaques do Windows ML incluem:

- **Aceleração de hardware**
    
    Em dispositivos compatíveis com DirectX12, o Windows ML acelera a avaliação de modelos de Deep Learning usando a GPU. Além disso, as otimizações de CPU permitem a avaliação de alto desempenho dos algoritmos clássicos de ML e de Deep Learning.

- **Avaliação local**

    O Windows ML avalia no hardware local, removendo preocupações de conectividade, largura de banda e privacidade de dados. A avaliação local também permite baixa latência e alto desempenho para obter resultados de avaliação rápidos.

- **Processamento de imagens**

    Para cenários de visão do computador, o Windows ML simplifica e otimiza o uso de dados de imagem, vídeo e câmera manipulando o pré-processamento de quadros e fornecendo configuração do pipeline de câmera para entrada de modelo.

## <a name="how-to-develop-with-windows-ml"></a>Como desenvolver com o Windows ML

> [!VIDEO https://www.youtube.com/embed/8MCDSlm326U]

### <a name="system-requirements"></a>Requisitos do sistema

Para criar aplicativos que usem o Windows ML, será necessário o [SDK do Windows – Build 17110 ou superior](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK).

### <a name="onnx-models"></a>Modelos ONNX

Para usar o Windows ML, você precisará de uma modelo de aprendizado de máquina treinado previamente no formato [ONNX (Open Neural Network Exchange)](https://onnx.ai). O Windows ML dá suporte à versão v1.0 do formato ONNX, o que permite que os desenvolvedores usem modelos produzidos por estruturas de treinamento diferentes.

Para obter uma lista de modelos ONNX publicamente disponíveis, consulte [Modelos ONNX](https://github.com/onnx/models) no GitHub.

Para saber como treinar um modelo ONNX com o Visual Studio Tools for AI, consulte [Treinar um modelo](train-ai-model.md).

### <a name="convert-existing-models-to-onnx"></a>Converter modelos existentes em ONNX

Muitas estruturas de treinamento já dão suporte nativo para modelos ONNX, e existem ferramentas de conversão para muitas estruturas e bibliotecas. Para saber como exportar de estruturas como Caffe 2, PyTorch, CNTK, Chainer e muito mais, veja [Tutoriais do ONNX](https://github.com/onnx/tutorials) no GitHub.

Você também pode usar [WinMLTools](https://pypi.org/project/winmltools/) para converter o modelo de aprendizado de máquina treinado no formato do ONNX aceito pelo Windows ML. O WinMLTools oferece suporte à conversão destes formatos:

- Core ML
- Scikit-Learn
- XGBoost
- LibSVM

Para saber como instalar e usar o WinMLTools, consulte [Converter um modelo](conversion-samples.md).

### <a name="onnx-operators"></a>Operadores do ONNX

O Windows ML dá suporte a mais de 100 operadores ONNX na CPU e acelera a computação em GPUs compatíveis com DirectX12. Para obter uma lista completa de assinaturas de operadores, consulte a documentação de esquemas de operadores ONNX para os namespaces [ai.onnx](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators.md) (padrão) e [ai.onnx.ml](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators-ml.md).

O Windows ML dá suporte a todos os operadores definidos na documentação do ONNX v1.0 com as seguintes diferenças:

- Os operadores marcados como "experimentais" compatíveis com Windows ML:
    - Affine
    - Crop
    - FC
    - Identidade
    - ImageScaler
    - MeanVarianceNormalization
    - ParametricSoftplus
    - ScaledTanh
    - ThresholdedRelu
    - Upsample
- MatMul - maior do que a multiplicação de matrizes 2D, não tem suporte no momento; tem suporte somente em CPU
- Cast - suporte somente em CPU
- Não há suporte para os seguintes operadores neste momento:
    - RandomUniform
    - RandomUniformLike
    - RandomNormal
    - RandomNormalLike

### <a name="automatic-interface-code-generation"></a>Geração automática de código de interface

Com um arquivo de modelo ONNX, o gerador de código do Windows ML cria uma interface para interagir com o modelo em seu app. A interface gerada inclui as classes wrapper que representam o modelo, as entradas e as saídas. O código gerado chama a [API do Windows ML](/uwp/api/windows.ai.machinelearning.preview) para você, permitindo carregar, associar e avaliar o modelo em seu projeto com facilidade. O gerador de código atualmente dá suporte a C# e C++/CX.

Para os desenvolvedores da UWP, o gerador automático de código do Windows ML é nativamente integrado ao [Visual Studio (versão 15.7 - Preview 1)](https://www.visualstudio.com/vs/preview/). (**Observação**: dentro o Instalador do Visual Studio, você precisará desmarcar o Windows 10 Insider Preview SDK, Build 17110). Dentro de seu projeto do Visual Studio, basta adicionar seu arquivo ONNX como um item existente e o VS irá gerar classes wrapper do Windows ML em um novo arquivo de interface.

Você também pode usar a ferramenta de linha de comando `mlgen.exe`, que vem com o SDK do Windows para gerar classes wrapper do Windows ML. A ferramenta está localizada em `(SDK_root)\bin\<version>\x64` ou em `(SDK_root)\bin\<version>\x86`, onde SDK_root é o diretório de instalação do SDK. Para executar a ferramenta, use o comando abaixo.

```
mlgen -i INPUT-FILE -l LANGUAGE -n NAMESPACE [-o OUTPUT-FILE]
```

Definição de parâmetros de entrada:

- `INPUT-FILE`: o arquivo de modelo ONNX
- `LANGUAGE`: CPPCX ou CS
- `NAMESPACE`: o namespace do código gerado
- `OUTPUT-FILE`: o caminho de arquivo onde o código gerado será gravado. Se OUTPUT-FILE não for especificado, o código gerado será gravado na saída padrão

Para saber como usar o código gerado em seu app, consulte [Integrar um modelo](integrate-model.md).

## <a name="next-steps"></a>Próximas etapas

Tente criar seu primeiro aplicativo Windows ML com um tutorial passo a passo em [Introdução](get-started.md).
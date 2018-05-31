---
author: wschin
title: Converter modelos do ML existentes em ONNX
description: Exemplos de códigos demonstram como usar WinMLTools para converter modelos existentes em scikit-learn e Core ML no formato ONNX.
ms.author: sezhen
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, windows machine learning, WinML, WinMLTools, ONNX, ONNXMLTools, scikit-learn, Core ML
ms.localizationpriority: medium
ms.openlocfilehash: 882efca26730c990093a89a5ed3ff4b5587e05bf
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832674"
---
# <a name="convert-existing-ml-models-to-onnx"></a>Converter modelos do ML existentes em ONNX

O [WinMLTools](https://aka.ms/winmltools) permite aos usuários converter modelos treinados em outras estruturas em formato ONNX. Aqui, demonstramos como instalar o pacote WinMLTools e como converter modelos existentes em scikit-learn e Core ML em ONNX por meio de código Python.

## <a name="install-winmltools"></a>Instalar WinMLTools

O WinMLTools é um pacote Python (winmltools) que dá suporte às versões 2.7 e 3.6 do Python. Se você estiver trabalhando em um projeto de ciências de dados, é recomendável instalar uma distribuição científica do Python, como o Anaconda.

O WinMLTools segue o [processo de instalação de pacote python padrão](https://packaging.python.org/installing/). Do seu ambiente python, execute:

```
pip install winmltools
```

O WinMLTools tem as seguintes dependências:

- numpy v1.10.0+
- onnxmltools 0.1.0.0+
- protobuf v.3.1.0+

Para atualizar os pacotes dependentes, execute o comando pip com o argumento '-U'.

```
pip install -U winmltools
```

Siga [onnxmltools](https://github.com/onnx/onnxmltools) no GitHub para obter mais informações sobre as dependências de onnxmltools.

Detalhes adicionais sobre como usar o WinMLTools podem ser encontrados na documentação específica do pacote com a função de ajuda.

```
help(winmltools)
```

## <a name="scikit-learn-models"></a>Modelos do Scikit-learn

O trecho de código a seguir treina um computador de vetor de suporte linear em scikit-learn e converte o modelo em ONNX.

~~~python
# First, we create a toy data set.
# The training matrix X contains three examples, with two features each.
# The label vector, y, stores the labels of all examples.
X = [[0.5, 1.], [-1., -1.5], [0., -2.]]
y = [1, -1, -1]

# Then, we create a linear classifier and train it.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()
linear_svc.fit(X, y)

# To convert scikit-learn models, we need to specify the input feature's name and type for our converter. 
# The following line means we have a 2-D float feature vector, and its name is "input" in ONNX.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
linear_svc_onnx = convert_sklearn(linear_svc, name='LinearSVC', input_features=[('input', 'float', 2)])    

# Now, we save the ONNX model into binary format.
from winmltools.utils import save_model
save_model(linear_svc_onnx, 'linear_svc.onnx')

# If you'd like to load an ONNX binary file, our tool can also help.
from winmltools.utils import load_model
linear_svc_onnx = load_model('linear_svc.onnx')

# To see the produced ONNX model, we can print its contents or save it in text format.
print(linear_svc_onnx)
from winmltools.utils import save_text
save_text(linear_svc_onnx, 'linear_svc.txt')

# The conversion of linear regression is very similar. See the example below.
from sklearn.svm import LinearSVR
linear_svr = LinearSVR()
linear_svr.fit(X, y)
linear_svr_onnx = convert_sklearn(linear_svr, name='LinearSVR', input_features=[('input', 'float', 2)])   
~~~

Os usuários podem substituir `LinearSVC` por outros modelos do scikit-learn, como `RandomForestClassifier`. Observe que o [gerador automático de código](overview.md#automatic-interface-code-generation) usa o parâmetro `name` para gerar nomes de classe e variáveis. Se `name` não for fornecido, então um GUID será gerado, o que não estará em conformidade com as convenções de nomenclatura de variáveis para linguagens como C++/C#. 

## <a name="scikit-learn-pipelines"></a>Pipelines do Scikit-learn

Em seguida, vamos mostrar como os pipelines do scikit-learn podem ser convertidos em ONNX.

~~~python
# First, we create a toy data set.
# Notice that the first example's last feature value, 300, is large.
X = [[0.5, 1., 300.], [-1., -1.5, -4.], [0., -2., -1.]]
y = [1, -1, -1]

# Then, we declare a linear classifier.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()

# One common trick to improve a linear model's performance is to normalize the input data.
from sklearn.preprocessing import Normalizer
normalizer = Normalizer()

# Here, we compose our scikit-learn pipeline. 
# First, we apply our normalization. 
# Then we feed the normalized data into the linear model.
from sklearn.pipeline import make_pipeline
pipeline = make_pipeline(normalizer, linear_svc)
pipeline.fit(X, y)

# Now, we convert the scikit-learn pipeline into ONNX format. 
# Compared to the previous example, notice that the specified feature dimension becomes 3.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
pipeline_onnx = convert_sklearn(linear_svc, name='NormalizerLinearSVC', input_features=[('input', 'float', 3)])   

# We can print the fresh ONNX model.
print(pipeline_onnx)

# We can also save the ONNX model into binary file for later use.
from winmltools.utils import save_model
save_model(pipeline_onnx, 'pipeline.onnx')

# Our conversion framework provides limited support of heterogeneous feature values.
# We cannot have numerical types and string type in one feature vector. 
# Let's assume that the first two features are floats and the last feature is integer (encoded a categorical attribute).
X_heter = [[0.5, 1., 300], [-1., -1.5, 400], [0., -2., 100]]

# One popular way to represent categorical is one-hot encoding.
from sklearn.preprocessing import OneHotEncoder
one_hot_encoder = OneHotEncoder(categorical_features=[2])

# Let's initialize a classifier. 
# It will be right after the one-hot encoder in our pipeline.
linear_svc = LinearSVC()

# Then, we form a two-stage pipeline.
another_pipeline = make_pipeline(one_hot_encoder, linear_svc)
another_pipeline.fit(X_heter, y)

# Now, we convert, save, and load the converted model. 
# For heterogeneous feature vectors, we need to seperately specify their types for all homogeneous segments.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
another_pipeline_onnx = convert_sklearn(another_pipeline, name='OneHotLinearSVC', input_features=[('input', 'float', 2), ('another_input', 'int64', 1)])
save_model(another_pipeline_onnx, 'another_pipeline.onnx')
from winmltools.utils import load_model
loaded_onnx_model = load_model('another_pipeline.onnx')

# Of course, we can print the ONNX model to see if anything went wrong.
print(another_pipeline_onnx)
~~~

## <a name="core-ml-models"></a>Modelos Core ML

Aqui, presumimos que o caminho de um exemplo de um arquivo de modelo Core ML seja *example.mlmodel*.

~~~python
from coremltools.models.utils import load_spec
# Load model file
model_coreml = load_spec('example.mlmodel')
from winmltools import convert_coreml
# Convert it!
# The automatic code generator (mlgen) uses the name parameter to generate class names.
model_onnx = convert_coreml(model_coreml, name='ExampleModel')   
~~~

O `model_onnx` é um objeto [ModelProto](https://github.com/onnx/onnxmltools/blob/0f453c3f375c1ae928b83a4c7909c82c013a5bff/onnxmltools/proto/onnx-ml.proto#L176) do ONNX. Nós podemos salvá-lo em dois formatos diferentes.

~~~python
from winmltools.utils import save_model
# Save the produced ONNX model in binary format
save_model(model_onnx, 'example.onnx')
# Save the produced ONNX model in text format
from winmltools.utils import save_text
save_text(model_onnx, 'example.txt')
~~~

**Observação**: o CoreMLTools é um pacote do Python fornecido pela Apple, mas não está disponível no Windows. Se você precisar instalar o pacote no Windows, instale o pacote diretamente do repositório:

```
pip install git+https://github.com/apple/coremltools
```

## <a name="core-ml-models-with-image-inputs-or-outputs"></a>Modelos Core ML com entradas ou saídas de imagem

Devido à ausência de tipos de imagem no ONNX, a conversão de modelos de imagem Core ML (isto é, modelos usando imagens como entradas ou saídas) requer algumas etapas de pré-processamento e pós-processamento.

O objetivo do pré-processamento é garantir que a imagem de entrada seja corretamente formatada como um tensor ONNX. Suponha que *X* seja uma imagem de entrada com a forma [C, H, W] no Core ML. Em ONNX, a variável *X* seria uma tensor de ponto flutuante com a mesma forma e *X*[0,:, :]/*X*[1,:, :]/*X*[2,:,:] armazena o canal vermelho/verde/azul da imagem. Para imagens de escala de cinza no Core ML, seus formatos são [1, H, W]-tensors no ONNX porque só temos um canal.

Se o modelo Core ML original produzir uma imagem, converta manualmente os tensores de saída de ponto flutuante do ONNX novamente em imagens. Há duas etapas básicas. A primeira etapa é para truncar valores maiores do que 255 a 255 e alterar todos os valores negativos para 0. A segunda etapa é arredondar todos os valores de pixel para inteiros (pela adição de 0,5 e então truncando os decimais). A ordem de canais de saída (por exemplo, RGB ou BGR) é indicada no modelo Core ML. Para gerar a saída de imagem adequada, é necessário transpor ou embaralhar para recuperar o formato desejado.

Aqui nós consideramos um modelo Core ML, FNS-Candy, baixado do [GitHub](https://github.com/likedan/Awesome-CoreML-Models), como um exemplo concreto de conversão para demonstrar a diferença entre os formatos ONNX e Core ML. Observe que todos os comandos subsequentes serão executados em um ambiente python.

Primeiro, nós carregamos o modelo Core ML e examinamos seus formatos de entrada e de saída.

~~~python
from coremltools.models.utils import load_spec
model_coreml = load_spec('FNS-Candy.mlmodel')
model_coreml.description # Print the content of Core ML model description
~~~

Saída de tela:

~~~
...
input {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
output {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
~~~

Nesse caso, a entrada e saída são 720 x 720 BGR-image. Nossa próxima etapa é converter o modelo Core ML com WinMLTools.

~~~python
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from onnxmltools import convert_coreml
model_onnx = convert_coreml(model_coreml, name='FNSCandy')    
~~~

Um método alternativo para exibir os formatos de modelo de entrada e saída no ONNX é usar o seguinte comando:

~~~python
model_onnx.graph.input # Print out the ONNX input tensor's information
~~~

Saída de tela:

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

A entrada produzida (denotada por *X*) no ONNX é um tensor 4-D. Os últimos 3 eixos são C-, H- e W-, respectivamente. A primeira dimensão será "None", o que significa que esse modelo ONNX pode ser aplicado a qualquer número de imagens. Para aplicar esse modelo para processar um lote de 2 imagens, a primeira imagem corresponde a *X*[0,:,:,:] enquanto *X*[1,:,:,:] corresponde à segunda imagem. Os canais azul/verde/vermelho da primeira imagem são *X*[0, 0,:, :]/*X*[0, 1,:, :]/*X*[0, 2,:,:] e semelhantes para a segunda imagem.

~~~python
model_onnx.graph.output # Print out the ONNX output tensor's information
~~~

Saída de tela:

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

Como você pode ver, o formato produzido é idêntico ao formato de entrada de modelo original. No entanto, nesse caso, ele não é uma imagem porque os valores de pixel são inteiros, e não números de ponto flutuante. Para converter novamente em uma imagem, trunque os valores superiores a 255 a 255 valores, altere os valores negativos para 0 e arredonde todos os decimais para inteiros.
---
author: jwmsft
ms.assetid: 9899F6A0-7EDD-4988-A76E-79D7C0C58126
title: Componentes da Plataforma Universal do Windows e otimização de interoperabilidade
description: Crie aplicativos da Plataforma Universal do Windows (UWP) que usam componentes UWP e interoperam entre tipos nativos e gerenciados, evitando problemas de desempenho de interoperabilidade.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 726dc4aaa34b9b68aa198e236abcef57b78b21f4
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7430955"
---
# <a name="uwp-components-and-optimizing-interop"></a>Componentes UWP e otimização de interoperabilidade


Crie aplicativos da Plataforma Universal do Windows (UWP) que usam componentes UWP e interoperam entre tipos nativos e gerenciados, evitando problemas de desempenho de interoperabilidade.

## <a name="best-practices-for-interoperability-with-uwp-components"></a>Práticas recomendadas de interoperabilidade com componentes UWP

Se não houver cautela, o uso de componentes UWP pode ter um grande impacto no desempenho do seu aplicativo. Esta seção discute como obter bom desempenho quando seu aplicativo usar componentes UWP.

### <a name="introduction"></a>Introdução

Interoperabilidade pode ter um grande impacto no desempenho, e talvez você a esteja usando sem perceber. A UWP manipula grande parte da interoperabilidade, para que você possa ser mais produtivo e reutilizar códigos escritos em outras linguagens. É recomendável tirar proveito do que a UWP faz por você, mas tenha em mente que isso pode afetar o desempenho. Esta seção discute o que pode ser feito para diminuir o impacto que a interoperabilidade tem no desempenho do seu aplicativo.

A UWP tem uma biblioteca de tipos que estão acessíveis em qualquer linguagem na qual se possa escrever um aplicativo UWP. Use os tipos UWP em C# ou Microsoft Visual Basic da mesma forma que usa objetos .NET. Não é preciso fazer chamadas de método de invocação de plataforma para acessar os componentes UWP. Isso torna a escrita dos seus aplicativos menos complexa, mas é importante observar que pode haver mais interoperabilidade do que o esperado. Se um componente UWP for escrito em uma linguagem diferente de C# ou Visual Basic, os limites da interoperabilidade serão transpostos quando você o utilizar. Transpor os limites da interoperabilidade pode afetar o desempenho de um aplicativo.

Quando você desenvolve um aplicativo UWP em C# ou Visual Basic, os dois conjuntos de APIs mais comuns usados são as APIs UWP e as APIs .NET para aplicativos UWP. Em geral, os tipos que são definidos no UWP estão em namespaces que começam com "Windows". e os tipos .NET estão em namespaces que começam com "System". Mas há exceções. Os tipos em .NET para aplicativos UWP não exigem interoperabilidade quando são usados. Se você achar que tem mau desempenho em uma área que usa a UWP, poderá usar APIs .NET para aplicativos UWP para obter melhor desempenho.

**Observação**  a maioria dos componentes UWP fornecidos com o Windows 10 é implementada em C++, para que você possa limites da interoperabilidade ao usá-los em c# ou Visual Basic. Como sempre, não deixe de avaliar seu aplicativo para observar se o uso de componentes UWP afeta o desempenho do aplicativo antes de investir em alterações do seu código.

Neste tópico, a expressão "componentes UWP" refere-se a componentes escritos em linguagem diferente do C# ou do Visual Basic.

 

Sempre que você acessa uma propriedade ou chama um método em um componente UWP, há um custo de interoperabilidade. Na verdade, o custo de criar um componente UWP é superior ao de criar um objeto .NET. Os motivos são que a UWP deve executar um código que transite entre a linguagem do seu aplicativo e a do componente. Além disso, se você passar dados para o componente, eles devem ser convertidos entre tipos gerenciados e não gerenciados.

### <a name="using-uwp-components-efficiently"></a>Usando componentes UWP com eficiência

Se você achar que precisa melhorar o desempenho, poderá verificar se o seu código usa componentes UWP da forma mais eficiente possível. Esta seção apresenta algumas dicas para melhorar o desempenho ao usar componentes UWP.

O impacto sobre o desempenho só se torna perceptível após um número significativo de chamadas em um curto período. Um aplicativo bem projetado que encapsule chamadas para componentes UWP da lógica de negócios e de outro código gerenciado não deve apresentar altos custos de interoperabilidade. Mas se seus testes indicarem que o uso de componentes UWP está afetando o desempenho do aplicativo, as dicas aqui apresentadas ajudam a aprimorar o desempenho.

### <a name="consider-using-net-for-uwp-apps"></a>Considerar o uso de .NET para aplicativos UWP

Há certos casos em que você pode realizar uma tarefa usando UWP ou .NET para aplicativos UWP. É recomendável tentar não misturar tipos .NET e UWP. Tente permanecer em um ou outro. Por exemplo, você pode analisar um fluxo de xml usando o tipo [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/BR206173) (um tipo UWP) ou o tipo [**System.Xml.XmlReader**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.xmlreader.aspx) (um tipo .NET). Use a API proveniente da mesma tecnologia que o fluxo. Por exemplo, se você ler xml de um [**MemoryStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.memorystream.aspx), use o tipo **System.Xml.XmlReader**, pois ambos são tipos .NET. Se você ler de um arquivo, use o tipo **Windows.Data.Xml.Dom.XmlDocument**, pois as APIs do arquivo e o **XmlDocument** são componentes UWP.

### <a name="copy-window-runtime-objects-to-net-types"></a>Copiar objetos do Windows Runtime para tipos .NET

Quando um componente UWP retorna um objeto UWP, pode ser benéfico copiar o objeto retornado em um objeto .NET. Dois momentos em que isso é especialmente importante ocorrem no trabalho com coleções e fluxos.

Se você chamar uma API UWP que retorne uma coleção e salvar e acessar essa coleção muitas vezes, pode ser benéfico copiar a coleção em uma coleção .NET e, daí em diante, usar a versão .NET.

### <a name="cache-the-results-of-calls-to-uwp-components-for-later-use"></a>Armazenar em cache os resultados das chamadas a componentes UWP para uso posterior

Você pode obter melhor desempenho salvando os valores como variáveis locais em vez de acessar várias vezes um tipo UWP. Isso pode ser muito benéfico se você usar um valor dentro de um loop. Avalie sua aplicação para observar se o uso de variáveis locais aprimora o seu desempenho. O uso de valores armazenados em cache pode aumentar a velocidade do aplicativo, pois o tempo gasto em interoperabilidade será menor.

### <a name="combine-calls-to-uwp-components"></a>Combinar chamadas a componentes UWP

Tente concluir as tarefas com o menor número possível de chamadas a objetos UWP. Por exemplo, normalmente é melhor ler um grande volume de dados de um fluxo do que pequenos volumes por vez.

Use APIs que agrupem o trabalho no menor número possível de chamadas, em vez de APIs que executem menos trabalho e exijam mais chamadas. Por exemplo, prefira criar um objeto chamando construtores que inicializem várias propriedades em vez de chamar o construtor padrão e atribuí-las uma por vez.

### <a name="building-a-uwp-components"></a>Criando um componente UWP

Se você escrever um componente UWP que possa ser usado por aplicativos escritos em C++ ou em JavaScript, certifique-se de que seu componente tenha sido projetado para obter bom desempenho. Todas as sugestões com vistas ao bom desempenho nas aplicações são extensivas aos componentes. Avalie seu componente para observar quais APIs têm altos padrões de tráfego e, nessas áreas, considere o fornecimento de APIs que permitam aos seus usuários trabalhar com poucas chamadas.

## <a name="keep-your-app-fast-when-you-use-interop-in-managed-code"></a>Manter seu aplicativo rápido ao usar a interoperabilidade em código gerenciado

A UWP facilita a interoperabilidade entre o código nativo e o gerenciado, mas, se você não tiver cuidado, poderá haver custos de desempenho. Consulte como obter bom desempenho ao usar a interoperabilidade em seus aplicativos UWP gerenciados.

A UWP permite que os desenvolvedores criem aplicativos em XAML usando o idioma de sua preferência, graças às projeções das APIs UWP disponíveis em cada idioma. Quando um aplicativo é desenvolvido em C# ou Visual Basic, essa conveniência tem um custo interoperabilidade, pois as APIs UWP são geralmente implementadas no código nativo, e qualquer invocação da UWP a partir do C# ou do Visual Basic exige que o CLR faça transição de um registro de ativação gerenciado para um nativo e realize marshaling dos parâmetros de funções para representações acessíveis pelo código nativo. Essa sobrecarga é irrisória para a maioria dos aplicativos. Mas quando você faz muitas chamadas (de centenas de milhares a milhões) a APIs UWP no caminho crítico de um aplicativo, esse custo pode se tornar perceptível. Em resumo, você quer ter certeza de que o tempo gasto na transição entre as linguagens seja curto, em comparação com o tempo gasto na execução do resto do seu código. Isso é mostrado no diagrama a seguir.

![As transições de interoperabilidade não podem dominar o tempo de execução do programa.](images/interop-transitions.png)

Os tipos listados em [**.NET for Windows apps**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) não têm esse custo de interoperabilidade quando usados a partir do C# ou do Visual Basic. Como regra geral, você pode intuir que os tipos em namespaces que começam com “Windows.” são parte da UWP, e aqueles que começam com “System.” são tipos .NET. Até mesmo a utilização simples de tipos UWP, como o acesso à propriedade ou alocação acarreta custos de interoperabilidade.

Meça seu aplicativo e determine se a interoperabilidade está ocupando uma parte considerável do tempo de execução dos seus aplicativos, antes de otimizar seus custos de interoperabilidade. Ao analisar o desempenho do seu aplicativo com o Visual Studio, você pode obter uma estimativa dos custos de interoperabilidade usando o modo de exibição **Funções** e consultando o tempo inclusivo gasto em métodos que chamam a UWP.

Se seu aplicativo está lento devido à sobrecarga de interoperabilidade, você pode melhorar seu desempenho reduzindo as chamadas a APIs UWP em caminhos de código executados intensivamente. Por exemplo, um mecanismo de jogo que esteja executando uma grande quantidade de cálculos de física consultando constantemente a posição e as dimensões de [**UIElements**](https://msdn.microsoft.com/library/windows/apps/BR208911) pode poupar muito tempo armazenando as informações necessárias de **UIElements** em variáveis locais, fazendo cálculos nesses valores armazenados em cache e atribuindo o resultado final novamente a **UIElements** após a conclusão dos cálculos. Outro exemplo: se uma coleção é intensamente acessada por código em C# ou Visual Basic, é mais eficiente usar uma coleção do namespace [**System.Collections**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.aspx), em vez de uma coleção do namespace [**Windows.Foundation.Collections**](https://msdn.microsoft.com/library/windows/apps/BR206657). Você também pode considerar o uso de chamadas combinadas a componentes UWP, por exemplo, com o uso de APIs [**Windows.Storage.BulkAccess**](https://msdn.microsoft.com/library/windows/apps/BR207676).

### <a name="building-a-uwp-component"></a>Criando um componente UWP

Se você criou um componente UWP para usar em aplicativos escritos em C++ ou em JavaScript, certifique-se de que o componente foi projetado para obter bom desempenho. Sua superfície de API define o limite de interoperabilidade e o quanto os usuários precisarão pensar sobre as orientações neste tópico. Esse fator é especialmente importante se você está distribuindo seus componentes a terceiros.

Todas as sugestões com vistas ao bom desempenho nos aplicativos são extensivas aos componentes. Avalie seu componente para observar quais APIs têm altos padrões de tráfego e, nessas áreas, considere o fornecimento de APIs que permitam aos seus usuários trabalhar com poucas chamadas. Envidamos esforços significativos durante o desenvolvimento da UWP para permitir que os aplicativos a utilizem sem precisarem transpor o limite da interoperabilidade com frequência.

 


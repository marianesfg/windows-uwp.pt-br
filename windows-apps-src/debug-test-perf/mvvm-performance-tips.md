---
ms.assetid: 159681E4-BF9E-4A57-9FEE-EC7ED0BEFFAD
title: Dicas de MVVM e desempenho de linguagem
description: Este tópico aborda algumas considerações de desempenho relacionadas à sua escolha de padrões de design de software e linguagem de programação.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9027362eccfb8130b181bee26a57f13ce1e1af66
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8794950"
---
# <a name="mvvm-and-language-performance-tips"></a>Dicas de MVVM e desempenho de linguagem


Este tópico aborda algumas considerações de desempenho relacionadas à sua escolha de padrões de design de software e linguagem de programação.

## <a name="the-model-view-viewmodel-mvvm-pattern"></a>O padrão MVVM (Model-View-ViewModel)

O padrão MVVM (Model-View-ViewModel) é comum em muitos aplicativos XAML. (O MVVM é muito semelhante à descrição do Fowler do padrão Model-View-Presenter, mas foi adaptado para XAML). O problema com o padrão MVVM é que ele inadvertidamente pode levar a aplicativos que têm muitas camadas e muitas alocações. As motivações para o MVVM são estas.

-   **Divisão de problemas**. É sempre útil dividir um problema em partes menores, e um padrão como o MVVM ou MVC é uma maneira de dividir um aplicativo (ou até mesmo um único controle) em partes menores: a exibição real, um modelo lógico da exibição (modelo de exibição) e a lógica do aplicativo independente da exibição (o modelo). Em particular, é um fluxo de trabalho popular para que os designers assumam a exibição usando uma ferramenta, os desenvolvedores assumam o modelo usando outra ferramenta e os integradores de design assumam o modelo de exibição usando as duas ferramentas.
-   **Teste de unidade**. Você pode testar a unidade do modelo de exibição (e consequentemente o modelo) independente da exibição, não se baseando, assim, na criação de janelas, na geração de entrada e assim por diante. Mantendo a exibição pequena, você pode testar uma grande parte de seu aplicativo sem precisar criar uma janela.
-   **Agilidade na alteração da experiência do usuário**. A exibição tende ver as alterações mais frequentes e as alterações mais tardias, à medida que a experiência do usuário é ajustada com base nos comentários do usuário final. Mantendo a exibição separada, essas mudanças podem ser acomodadas mais rapidamente e com menos variação para o aplicativo.

Há várias definições concretas do padrão MVVM e das estruturas de terceiros que ajudam a implementá-lo. Mas aderir estritamente a qualquer variação do padrão pode levar a aplicativos com muito mais sobrecarga do que se pode justificar.

-   A vinculação de dados XAML (a extensão de marcação {Binding}) foi projetada em parte para habilitar padrões de modelo/exibição. Mas a extensão {Binding} traz com ela um conjunto de trabalho não trivial e sobrecarga para a CPU. Criar uma extensão {Binding} causa uma série de alocações, e atualizar um destino de associação pode causar reflexão e conversão boxing. Esses problemas estão sendo corrigidos com a extensão de marcação {x:Bind}, que compila as associações no momento da compilação. **Recomendação:** use {x:Bind}.
-   No MVVM, é comum conectar Button.Click ao modelo de exibição usando um ICommand, como os auxiliares comuns DelegateCommand ou RelayCommand. Mas esses comandos são alocações extras, incluindo o ouvinte de eventos CanExecuteChanged, somando-se ao conjunto de trabalho e ao tempo de inicialização/navegação da página. **Recomendação:** como alternativa à prática interface ICommand, você pode colocar manipuladores de eventos em seu code-behind, anexá-los a eventos de exibição e chamar um comando em seu modelo de exibição quando esses eventos ocorrerem. Você também precisará adicionar código extra para desabilitar o botão quando o comando não estiver disponível.
-   No MVVM, é comum criar uma página com todas as configurações possíveis da interface do usuário e, em seguida, recolher partes da árvore associando a propriedade Visibility às propriedades na VM. Isso aumenta desnecessariamente o tempo de inicialização e, possivelmente, o conjunto de trabalho (porque algumas partes da árvore podem nunca ficar visíveis). **Recomendações:** use o recurso [Atributo x:Load](../xaml-platform/x-load-attribute.md) ou [Atributo x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md) para adiar partes desnecessárias da árvore na inicialização. Além disso, crie controles de usuário separados para os diferentes modos da página e use o code-behind para manter somente os controles necessários carregados.

## <a name="ccx-recommendations"></a>Recomendações para C++/CX

-   **Use a versão mais recente**. Há melhorias de desempenho contínuas feitas no compilador C++/CX. Certifique-se de que seu aplicativo está sendo compilado com o conjunto de ferramentas mais recente.
-   **Desative RTTI (/GR-)**. O RTTI é ativado por padrão no compilador, então, a menos que seu ambiente de compilação o desative, você provavelmente está usando-o. O RTTI tem uma sobrecarga significativa e, a menos que seu código dependa profundamente dele, você deve desativá-lo. A estrutura XAML não exige que seu código use o RTTI.
-   **Evite o uso intenso de ppltasks**. Ppltasks são muito convenientes ao chamar APIs assíncronas do WinRT, mas eles vêm com uma sobrecarga de tamanho de código significativa. A equipe da linguagem C++/CX está trabalhando em um recurso da linguagem – await – que fornecerá um desempenho muito melhor. Enquanto isso, modere seu uso de ppltasks nos caminhos intensos de seu código.
-   **Evite o uso de C++/CX na “lógica comercial” do seu aplicativo**. A linguagem C++/CX foi projetada para ser uma maneira conveniente de acessar APIs do WinRT a partir de aplicativos C++. Essa linguagem faz uso de wrappers que têm sobrecarga. Evite usar C++/CX dentro do modelo/lógica de negócios de sua classe e reserve-o para uso nos limites entre o código e o WinRT.
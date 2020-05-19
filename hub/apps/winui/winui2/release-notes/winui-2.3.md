---
title: Notas sobre a versão do WinUI 2.3
description: Notas sobre a versão do WinUI 2.3, incluindo novos recursos e correções de bugs.
ms.date: 04/15/2020
ms.topic: article
ms.openlocfilehash: a61932a6f0060a4be79424e02aad3dd312128aef
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580453"
---
# <a name="windows-ui-library-23"></a>Biblioteca de Interface do Usuário do Windows 2.3

O WinUI 2.3 é a versão oficial mais recente da WinUI (Biblioteca de Interface do Usuário do Windows).

O WinUI é um projeto de software livre hospedado no GitHub no [repositório da Biblioteca de Interface do Usuário do Windows](https://aka.ms/winui). Registre todos os relatórios de bugs, solicitações de recursos e contribuições de código da comunidade nesse repositório.

Versões do WinUI: [Página de versão do GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

É possível adicionar os pacotes do WinUI aos projetos do Visual Studio por meio do Gerenciador de Pacotes do NuGet. Para saber mais, confira [Introdução à Biblioteca de Interface do Usuário do Windows](../getting-started.md).

Download do pacote NuGet: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Novos recursos

### <a name="progress-bar-visual-refresh"></a>Atualização visual da barra de progresso

A **ProgressBar** tem duas representações visuais diferentes.

#### <a name="indeterminate-progress-bar"></a>Barra de progresso indeterminado

Mostra que uma tarefa está em andamento, mas não impede a interação do usuário.

![Barra de progresso indeterminado](../images/IndeterminateProgressBar.gif)

#### <a name="determinate-progress-bar"></a>Barra de progresso determinado

Mostra o progresso realizado em uma quantidade de trabalho conhecida. 

![Barra de progresso determinado](../images/DeterminateProgressBar.gif)

[Link do documento](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/progress-controls)

[Link do exemplo](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/progress-controls#examples)

### <a name="numberbox"></a>NumberBox

O **NumberBox** é um controle que pode ser usado para exibir e editar números. Isso dá suporte para validação, passo a passo incremental e computação de cálculos embutidos de equações básicas, como multiplicação, divisão, adição e subtração.

![NumberBox](../images/NumberBoxGif.gif)

[Link do documento e do exemplo](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/number-box)

### <a name="radiobuttons"></a>RadioButtons

**RadioButtons** é um novo controle de contêiner que permite criar grupos relacionados de elementos RadioButton facilmente, além de também ter a compatibilidade correta com os recursos de teclado e leitor/narrador de tela

![RadioButtons](../images/RadioButtons.png)

[Link do documento e do exemplo](https://github.com/microsoft/microsoft-ui-xaml-specs/blob/c8d3d3668af546091656dfc37436b13cd062f52d/active/radiobuttons/RadioButtons_Spec.md)

## <a name="examples"></a>Exemplos

O aplicativo de exemplo da Galeria de Controles XAML inclui demonstrações interativas e código de exemplo para usar controles WinUI.

* Instale o aplicativo da Galeria de Controles XAML da [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

* A Galeria de Controles XAML também é de [software livre no GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Documentação

Artigos de instruções sobre os controles da Biblioteca de Interface do Usuário do Windows estão incluídos com a [Documentação de controles da Plataforma Universal do Windows](/windows/uwp/design/controls-and-patterns/).

Os documentos da referência de API estão localizados aqui: [APIs da Biblioteca de Interface do Usuário do Windows](/uwp/api/overview/winui/).

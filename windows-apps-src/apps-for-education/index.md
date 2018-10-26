---
author: PatrickFarley
title: Desenvolva aplicativos de educação.
description: Esta seção descreve os recursos de Aplicativos Universais do Windows que estão disponíveis para escrever aplicativos de educação para a plataforma Windows 10.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, educação
ms.assetid: 2431f253-efe3-4895-b131-34653b61f13c
ms.localizationpriority: medium
ms.openlocfilehash: d55aaab22ff51112ca69d1ead72e31468d7fb1ab
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5560690"
---
# <a name="develop-universal-windows-apps-for-education"></a>Desenvolver aplicativos Universais do Windows para educação
![captura de tela do aplicativo fazer um teste](images/take-a-test-screen-small.png)

Os recursos a seguir ajudarão você a gravar um aplicativo Universal do Windows para educação.

### <a name="accessibility"></a>Acessibilidade
Os aplicativos de educação precisam ser acessíveis. Consulte [Desenvolvendo aplicativos para acessibilidade](https://developer.microsoft.com/windows/accessible-apps) para obter mais informações.


### <a name="secure-assessments"></a>Avaliações seguras
Aplicativos de avaliação/teste geralmente precisam produzir um ambiente *bloqueado* para impedir que os alunos usem outros computadores ou recursos de Internet durante um teste. Essa funcionalidade está disponível por meio da [API Fazer um Teste](take-a-test-api.md). Consulte o aplicativo Web [Fazer um Teste](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) no Centro de TI do Windows para obter um exemplo de um ambiente de teste com acesso online bloqueado para testes de alto interesse.

### <a name="user-input"></a>Entrada do usuário
A entrada do usuário é uma parte essencial dos aplicativos de educação; os controles de interface do usuário devem ser responsivos e intuitivos para não distrair os usuários. Para obter uma visão geral das opções de entrada disponíveis em um aplicativo Universal do Windows, consulte a [Cartilha de entrada](https://docs.microsoft.com/windows/uwp/design/input/input-primer) e os tópicos abaixo na seção Design e interface do usuário. Além disso, os seguintes aplicativos de amostra mostram a manipulação básica da interface do usuário na Plataforma Universal do Windows.
- [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput) mostra como manipular a entrada em Aplicativos Universais do Windows.
- [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode) mostra como detectar e responder ao modo de interação do usuário.
- [Amostra de elementos visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) mostra como tirar proveito dos novos elementos visuais de foco desenhados pelo sistema ou criar seus próprios elementos visuais de foco personalizados caso os elementos desenhados pelo sistema não atendam às suas necessidades.

A plataforma do Windows Ink pode promover aplicativos de educação ajustando-os com um modo de entrada com o qual os alunos estão acostumados. Consulte [Windows Ink e interações de caneta](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions) e os tópicos abaixo para obter um guia abrangente de implementação do Windows Ink em seu aplicativo. Os aplicativos de exemplo a seguir fornecem exemplos práticos dessa API.
- [Amostra de tinta](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Ink) demonstra como usar a funcionalidade de tinta (como capturar, manipular e interpretar traços de tinta) em aplicativos Universais do Windows usando JavaScript.
- [Amostra de tinta simples](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk) demonstra como usar a funcionalidade de tinta (como capturar tinta da entrada do usuário e executar o reconhecimento de manuscrito em traços de tinta) em aplicativos Universais do Windows usando C#.
- [Amostra de tinta complexa](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk) demonstra como usar a funcionalidade avançada InkPresenter para intercalar a tinta com outros objetos, selecionar a tinta, copiar/colar e manipular eventos. Ela foi criada na Plataforma Universal do Windows em C++ e pode ser executada em SKUs Desktop e Mobile do Windows 10.


### <a name="microsoft-store"></a>Microsoft Store
Os aplicativos de educação geralmente são lançados em circunstâncias especiais para uma organização específica. Consulte [Distribuir aplicativos de linha de negócios para empresas](https://msdn.microsoft.com/windows/uwp/publish/distribute-lob-apps-to-enterprises) para obter informações sobre isso.

## <a name="related-topics"></a>Tópicos relacionados
- [Windows 10 para educação](https://technet.microsoft.com/edu/windows/index) no Centro de TI do Windows

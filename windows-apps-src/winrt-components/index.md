---
title: Componentes do Tempo de Execução do Windows
description: Os componentes do Tempo de Execução do Windows são objetos independentes para os quais você pode criar uma instância e usar em qualquer linguagem, incluindo C#, Visual Basic, JavaScript e C++.
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 76026c4f499a068bab689eadf050e0ec6277bbe8
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393691"
---
# <a name="windows-runtime-components"></a>Componentes do Tempo de Execução do Windows
Os componentes do Tempo de Execução do Windows são objetos independentes para os quais você pode criar uma instância e usar em qualquer linguagem, incluindo C#, Visual Basic, JavaScript e C++.

É possível usar o Visual Studio e o C#, o Visual Basic ou o C++ para criar componentes do Windows Runtime que podem ser usados em aplicativos UWP (Plataforma Universal do Windows).

| Tópico | Descrição |
|-------|-------------|
| [Componentes do Windows Runtime com C++/CX](creating-windows-runtime-components-in-cpp.md) | Este tópico mostra como usar o C++/CX para criar um componente do Windows Runtime&mdash;, que é um componente que pode ser chamado por um aplicativo Universal do Windows criado usando qualquer linguagem do Windows Runtime. |
| [Passo a passo para criar um componente do Windows Runtime em C++/CX e chamá-lo do JavaScript ou do C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | Este passo a passo mostra como criar uma DLL básica do componente do Windows Runtime que pode ser chamada do JavaScript, do C# ou do Visual Basic. Antes de começar este procedimento passo a passo, assegure-se de que você compreendeu conceitos como a Abstract Binary Interface (ABI), as classes ref e as extensões de componente do Visual C++ que facilitam o trabalho com classes ref. Para obter mais informações, confira [Criação de componentes do Windows Runtime em C++](creating-windows-runtime-components-in-cpp.md) e [Referência da linguagem Visual C++ (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx). |
| [Componentes do Windows Runtime com C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | Você pode usar código gerenciado para criar os seus próprios tipos do Windows Runtime, empacotados em um componente do Windows Runtime. É possível usar o componente em aplicativos da Plataforma Universal do Windows (UWP) com C++, JavaScript, Visual Basic ou C#. Este tópico descreve as regras para a criação de um componente e discute alguns aspectos do suporte do .NET ao Windows Runtime. Em geral, esse suporte foi projetado para ser transparente para o programador do .NET. No entanto, ao criar um componente a ser usado com JavaScript ou C++, você precisa estar ciente das diferenças na maneira como essas linguagens dão suporte ao Windows Runtime. |
| [Passo a passo para criar um componente do Windows Runtime em C# ou Visual Basic e chamá-lo do JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | Este passo a passo mostra como você pode usar o .NET com o Visual Basic ou o C# para criar os seus próprios tipos do Windows Runtime, empacotados em um componente do Windows Runtime, além de como chamar o componente do aplicativo Universal do Windows criado para Windows usando JavaScript. |
| [Acionando eventos em componentes do Windows Runtime](raising-events-in-windows-runtime-components.md) | Caso o componente do Tempo de Execução do Windows acione um evento de um tipo representante definido pelo usuário em um thread em segundo plano (thread de trabalho) e você deseje que o JavaScript seja capaz de receber o evento, é possível implementar e/ou acioná-lo destas formas: | 
| [Componentes do Windows Runtime intermediários para aplicativos UWP de sideload](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | Este tópico discute um recurso direcionado a empresas com suporte do Windows 10 Update e acima, que permite que aplicativos .NET com navegação por toque usem o código existente responsável por operações essenciais para os negócios. |

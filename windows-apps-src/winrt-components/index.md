---
title: Componentes do Tempo de Execução do Windows
description: Os componentes do Tempo de Execução do Windows são objetos independentes para os quais você pode criar uma instância e usar em qualquer linguagem, incluindo C#, Visual Basic, JavaScript e C++.
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3e9dde8027077bb822cc0659c8b9970ae5aded67
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372519"
---
# <a name="windows-runtime-components"></a>Componentes do Tempo de Execução do Windows
Os componentes do Tempo de Execução do Windows são objetos independentes para os quais você pode criar uma instância e usar em qualquer linguagem, incluindo C#, Visual Basic, JavaScript e C++.

É possível usar o Visual Studio e o C#, o Visual Basic ou o C++ para criar componentes do Windows Runtime que podem ser usados em aplicativos UWP (Plataforma Universal do Windows).

| Tópico | Descrição |
|-------|-------------|
| [Criando componentes do Windows Runtime no C++](creating-windows-runtime-components-in-cpp.md) | Este tópico mostra como usar C++/CX para criar um componente do Tempo de Execução do Windows, que é um componente que pode ser chamado por um aplicativo Universal Windows criado usando C#, Visual Basic, C++ ou JavaScript. |
| [Passo a passo: Criando um componente do Tempo de Execução do Windows básico em C++ e chamando-o em JavaScript ou C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | Este procedimento passo a passo mostra como criar uma DLL de componente do Tempo de Execução do Windows básica que pode ser chamada em JavaScript, C# ou Visual Basic. Antes de começar este procedimento passo a passo, assegure-se de que você compreendeu conceitos como a Abstract Binary Interface (ABI), as classes ref e as extensões de componente do Visual C++ que facilitam o trabalho com classes ref. Para obter mais informações, consulte [Criação de componentes do Tempo de Execução do Windows em C++](creating-windows-runtime-components-in-cpp.md) e [Referência da linguagem Visual C++ (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx). |
| [Criando componentes do Windows Runtime em C# ou Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | A partir do .NET Framework 4.5, é possível usar código gerenciado para criar os próprios tipos do Windows Runtime, empacotados em um componente do Tempo de Execução do Windows. É possível usar o componente em aplicativos da Plataforma Universal do Windows (UWP) com C++, JavaScript, Visual Basic ou C#. Este tópico descreve as regras para a criação de um componente e descreve alguns aspectos do suporte do .NET Framework para o Windows Runtime. Em geral, esse suporte foi projetado para ser transparente para o programador do .NET Framework. No entanto, ao criar um componente a ser usado com JavaScript ou C++, você precisa estar ciente das diferenças na maneira como essas linguagens dão suporte ao Windows Runtime. |
| [Passo a passo: Criando um componente do Tempo de Execução do Windows simples e chamando-o em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | Este passo a passo mostra como é possível usar o .NET Framework com Visual Basic ou C# para criar os próprios tipos de Windows Runtime, empacotados em um componente do Tempo de Execução do Windows, e como chamar o componente no aplicativo Universal do Windows em JavaScript para Windows. |
| [Acionando eventos em componentes do Windows Runtime](raising-events-in-windows-runtime-components.md) | Caso o componente do Tempo de Execução do Windows acione um evento de um tipo representante definido pelo usuário em um thread em segundo plano (thread de trabalho) e você deseje que o JavaScript seja capaz de receber o evento, é possível implementar e/ou acioná-lo destas formas: | 
| [Componentes do Windows Runtime intermediários para aplicativos UWP de sideload](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | Este tópico discute um recurso direcionado a empresas com suporte do Windows 10 Update e acima, que permite que aplicativos .NET com navegação por toque usem o código existente responsável por operações essenciais para os negócios. |

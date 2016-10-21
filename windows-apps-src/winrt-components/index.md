---
author: msatranjr
title: "Componentes do Tempo de Execução do Windows"
description: "Os componentes do Tempo de Execução do Windows são objetos independentes para os quais você pode criar uma instância e usar em qualquer linguagem, incluindo C#, Visual Basic, JavaScript e C++."
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
translationtype: Human Translation
ms.sourcegitcommit: 4e9f3de68c44cf545ceee2efd99d9db8cab08676
ms.openlocfilehash: e0dfdce921ee910d0440f01b4f816598499925f5

---

# Componentes do Tempo de Execução do Windows


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Os componentes do Tempo de Execução do Windows são objetos independentes para os quais você pode criar uma instância e usar em qualquer linguagem, incluindo C#, Visual Basic, JavaScript e C++.

É possível usar o Visual Studio e o C#, o Visual Basic ou o C++ para criar componentes do Windows Runtime que podem ser usados em aplicativos UWP (Plataforma Universal do Windows).

| Tópico | Descrição |
|-------|-------------|
| [Criando componentes do Windows Runtime em C++](creating-windows-runtime-components-in-cpp.md) | Este artigo mostra como usar C++ para criar um componente do Windows Runtime, que é uma DLL que pode ser chamada em um aplicativo Universal do Windows compilado com JavaScript – ou C#, Visual Basic ou C++. |
| [Procedimento passo a passo: criando um componente do Windows Runtime básico em C++ e chamando-o em JavaScript ou C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | Esse procedimento passo a passo mostra como criar uma DLL de componente do Windows Runtime básica que pode ser chamada em JavaScript, C# ou Visual Basic. Antes de começar este procedimento passo a passo, assegure-se de que você compreendeu conceitos como a Abstract Binary Interface (ABI), as classes ref e as extensões de componente do Visual C++ que facilitam o trabalho com classes ref. Para obter mais informações, consulte [Criação de componentes do Windows Runtime em C++](creating-windows-runtime-components-in-cpp.md) e [Referência da linguagem Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx). |
| [Criação de componentes do Windows Runtime em C# e Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | Desde o .NET Framework 4.5, é possível usar código gerenciado para criar os próprios tipos do Windows Runtime, empacotados em um componente do Windows Runtime. É possível usar o componente em aplicativos da Plataforma Universal do Windows (UWP) com C++, JavaScript, Visual Basic ou C#. Este artigo descreve as regras para a criação de um componente e descreve alguns aspectos do suporte do .NET Framework para o Windows Runtime. Em geral, esse suporte foi projetado para ser transparente para o programador do .NET Framework. No entanto, ao criar um componente a ser usado com JavaScript ou C++, você precisa estar ciente das diferenças na maneira como essas linguagens dão suporte ao Windows Runtime. |
| [Procedimento passo a passo: criando um componente do Windows Runtime simples e chamando-o em JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | Este passo a passo mostra como você pode usar o .NET Framework com Visual Basic ou c# para criar seus próprios tipos de Windows Runtime, empacotados em um componente do Windows Runtime e como chamar o componente no aplicativo Universal do Windows compilado para Windows usando JavaScript. |
| [Acionar eventos em componentes do Windows Runtime](raising-events-in-windows-runtime-components.md) | Caso o componente do Tempo de Execução do Windows acione um evento de um tipo representante definido pelo usuário em um thread em segundo plano (thread de trabalho) e você deseje que o JavaScript seja capaz de receber o evento, é possível implementar e/ou acioná-lo destas formas: | 
| [Componentes do Tempo de Execução do Windows intermediários para aplicativos de sideload da Windows Store](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | Este tópico discute um recurso direcionado a empresas com suporte do Windows 10 Update e acima, que permite que aplicativos .NET com navegação por toque usem o código existente responsável por operações essenciais para os negócios.
 

 

 






<!--HONumber=Aug16_HO5-->



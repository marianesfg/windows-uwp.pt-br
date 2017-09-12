---
author: normesta
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Acesso a dados
description: "Esta seção aborda o armazenamento de dados no dispositivo em um banco de dados privado e o uso do mapeamento relacional de objeto em apps da Plataforma Universal do Windows (UWP)."
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, dados, banco de dados, relacional, tabelas, sqlite
ms.openlocfilehash: 19690b6877fb4304b7740e6098711ca0b097d567
ms.sourcegitcommit: 378382419f1fda4e4df76ffa9c8cea753d271e6a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2017
---
# <a name="data-access"></a>Acesso a dados

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção aborda o armazenamento de dados no dispositivo em um banco de dados privado e o uso do mapeamento relacional de objeto em aplicativos da Plataforma Universal do Windows (UWP).

SQLite está incluído no SDK da UWP. O Entity Framework Core funciona com o SQLite em aplicativos UWP. Use essas tecnologias para desenvolver para cenários de conectividade intermitente/offline e para manter os dados entre as sessões do app.

| Tópico | Descrição|
|-------|------------|
| [Entity Framework Core com SQLite para apps C#](entity-framework-7-with-sqlite-for-csharp-apps.md) | O Entity Framework (EF) é um mapeador relacional de objeto que permite trabalhar com dados relacionais usando-se objetos específicos de domínio. Este artigo explica como é possível usar o Entity Framework Core com um banco de dados SQLite em um aplicativo universal do Windows. |
| [Bancos de dados SQLite](sqlite-databases.md) | SQLite é um mecanismo de banco de dados sem servidor inserido. Este artigo explica como usar a biblioteca SQLite incluída no SDK, empacotar a própria biblioteca SQLite em um aplicativo Universal do Windows ou compilá-lo desde a origem. |

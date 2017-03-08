---
author: mcleblanc
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Acesso a dados
description: "Esta seção aborda o armazenamento de dados no dispositivo em um banco de dados privado e o uso do mapeamento relacional de objeto em apps da Plataforma Universal do Windows (UWP)."
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, dados, banco de dados, relacional, tabelas, sqlite
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0f7aff1c9e7354fcf92d24ed8acb88b41a23d377
ms.lasthandoff: 02/07/2017

---
# <a name="data-access"></a>Acesso a dados

[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção aborda o armazenamento de dados no dispositivo em um banco de dados privado e o uso do mapeamento relacional de objeto em apps da Plataforma Universal do Windows (UWP).

SQLite está incluído no SDK da UWP. Entity Framework 7 funciona com o SQLite em apps UWP. Use essas tecnologias para desenvolver para cenários de conectividade intermitente/offline e manter dados entre as sessões do app.

| Tópico | Descrição|
|-------|------------|
| [Entity framework 7 com SQLite para apps C#](entity-framework-7-with-sqlite-for-csharp-apps.md) | Entity Framework (FE) é um mapeador relacional de objeto que permite trabalhar com dados relacionais usando-se objetos específicos de domínio. Este artigo explica como é possível usar o Entity Framework 7 com um banco de dados SQLite em um aplicativo universal do Windows. |
| [Bancos de dados SQLite](sqlite-databases.md) | SQLite é um mecanismo de banco de dados sem servidor inserido. Este artigo explica como usar a biblioteca SQLite incluída no SDK, empacotar a própria biblioteca SQLite em um aplicativo universal do Windows ou compilá-lo desde a origem. |


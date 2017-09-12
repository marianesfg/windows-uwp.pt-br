---
author: normesta
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "O Entity Framework (EF) é um mapeador relacional de objetos que permite trabalhar com dados relacionais usando-se objetos específicos de domínio."
title: Entity Framework Core com SQLite para apps C#
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, SQLite, c#, EF, estrutura de entidade
ms.openlocfilehash: 9d447b8a197ed7eaea0f991749064912dffb66d7
ms.sourcegitcommit: 378382419f1fda4e4df76ffa9c8cea753d271e6a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2017
---
# <a name="entity-framework-core-with-sqlite-for-c-apps"></a>Entity Framework Core com SQLite para apps C#

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O Entity Framework (FE) é um mapeador relacional de objeto que permite trabalhar com dados relacionais usando-se objetos específicos de domínio. Este artigo explica como é possível usar o Entity Framework Core com um banco de dados SQLite em um aplicativo universal do Windows.

Originalmente para desenvolvedores do .NET, o Entity Framework Core pode ser usado com SQLite na Plataforma Universal do Windows (UWP) para armazenar e manipular dados relacionais usando-se objetos específicos de domínio. É possível migrar o código de FE de um aplicativo do .NET para um aplicativo UWP e esperar que ele funcione com as alterações apropriadas feitas na cadeia de caracteres da conexão.

Atualmente, FE só dá suporte ao SQLite na UWP. Um procedimento passo a passo detalhado sobre como instalar o Entity Framework Core e como criar modelos está disponível na página [Ponto de Partida da Plataforma Universal do Windows](http://go.microsoft.com/fwlink/p/?LinkId=735013). Ele aborda os seguintes tópicos:

-   Pré-requisitos
-   Criar um novo projeto
-   Instalar o Entity Framework
-   Criar o modelo
-   Criar o banco de dados
-   Usar o modelo

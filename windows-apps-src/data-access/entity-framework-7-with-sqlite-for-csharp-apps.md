---
author: mcleblanc
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "O Entity Framework (FE) é um mapeador relacional de objeto que permite trabalhar com dados relacionais usando-se objetos específicos de domínio."
title: Entity framework 7 com SQLite para aplicativos C#
translationtype: Human Translation
ms.sourcegitcommit: a4680f50b8ef45e4e995d0b9997c0266478fe233
ms.openlocfilehash: 07244b35b2ec20227bccc43638b56b9fda88956a

---

# Entity Framework Core 1 com SQLite para aplicativos C#

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O Entity Framework (FE) é um mapeador relacional de objeto que permite trabalhar com dados relacionais usando-se objetos específicos de domínio. Este artigo explica como é possível usar o Entity Framework Core 1 com um banco de dados SQLite em um aplicativo Universal do Windows.

Originalmente para desenvolvedores do .NET, o Entity Framework Core 1 pode ser usado com SQLite na Plataforma Universal do Windows (UWP) para armazenar e manipular dados relacionais usando objetos específicos de domínio. É possível migrar o código de FE de um aplicativo do .NET para um aplicativo UWP e esperar que ele funcione com as alterações apropriadas feitas na cadeia de caracteres da conexão.

Atualmente, FE só dá suporte ao SQLite na UWP. Um procedimento passo a passo detalhado sobre como instalar o Entity Framework Core 1 e criar modelos está disponível na página [Introdução à Plataforma Universal do Windows](http://go.microsoft.com/fwlink/p/?LinkId=735013). Ele aborda os seguintes tópicos:

-   Pré-requisitos
-   Criar um novo projeto
-   Instalar o Entity Framework
-   Criar o modelo
-   Criar o banco de dados
-   Usar o modelo




<!--HONumber=Nov16_HO1-->



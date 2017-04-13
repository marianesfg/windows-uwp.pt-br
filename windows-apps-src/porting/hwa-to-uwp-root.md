---
author: seksenov
title: Aplicativos Web hospedados - Converter seu aplicativo web em um aplicativo da Plataforma Universal do Windows (UWP) e acessar os recursos nativos do Windows 10
description: Encontre recursos para converter seu aplicativo web em um aplicativo da Plataforma Universal do Windows (UWP) para a Windows Store.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Aplicativos Web hospedados, converter um site em aplicativo do Windows, aplicativos web na Windows Store, aplicativos do Chrome para Windows
ms.openlocfilehash: c9239f3a3c14bf9da99e60cfef8154eefb4305b4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="hosted-web-apps---access-windows-10-features-from-your-web-app"></a>Aplicativos Web hospedados - Acessar recursos do Windows 10 a partir do seu aplicativo Web

Seu aplicativo Web pode ter acesso completo à Plataforma Universal do Windows (UWP), incluindo chamar APIs do Windows Runtime diretamente de um script hospedado em um servidor, aproveitar a integração com a Cortana e usar um provedor de autenticação online. Também há suporte para aplicativos híbridos, pois você pode incluir código local para ser chamado do script hospedado e gerenciar a navegação do aplicativo entre as páginas locais e remotas.

## <a name="get-started"></a>Introdução

Se você estiver em um Mac ou PC, você pode criar um Aplicativo Web hospedado em questão de minutos. A melhor maneira de começar é usando o [Visual Studio Community 2015](https://www.visualstudio.com/vs/community/) gratuito e completo, especialmente se você estiver em um dispositivo Windows. Se você não tem acesso ao Visual Studio, há algumas opções que você pode escolher. Se você estiver familiarizado com a utilitários com interface de linha de comando (CLI), confira [ManifoldJS](http://manifoldjs.com/). Você também pode usar o [App Studio](http://appstudio.windows.com/), uma ferramenta de criação on-line gratuita e sem a necessidade de codificação que permite criar rapidamente aplicativos para o Windows 10.

- [Instruções passo a passo para converter seu aplicativo da web em um aplicativo da Plataforma Universal do Windows (UWP) usando o Windows](hwa-create-windows.md)

- [Instruções passo a passo para converter seu aplicativo da web em um aplicativo da Plataforma Universal do Windows (UWP) usando um Mac](hwa-create-mac.md)

## <a name="enhance-your-app"></a>Melhorar seu aplicativo

- Faça com que seu aplicativo brilhe com os [recursos nativos do Windows Runtime](hwa-access-features.md), você pode chamar diretamente do JavaScript.

- Proteja seu aplicativo definindo Regras de URI de conteúdo do aplicativo (ACURs) com nosso modelo de Política de Segurança de Conteúdo (CSP).

- Execute o aplicativo com o poder da voz por meio da integração de comandos da Cortana.

- Conceda acesso programático a recursos do usuário e à funcionalidade do dispositivo declarando as funcionalidades do aplicativo.

- Crie um fluxo de logon simples e estável para seus usuários verificando sua identidade com OpenID e OAuth.

- Pare de decidir entre um Aplicativo Web hospedado e empacotado. Você pode ter um pouco de ambos, criando um Aplicativo híbrido.

## <a name="convert-your-existing-chrome-app"></a>Converter seu aplicativo Chrome existente

Tornamos fácil [converter seu aplicativo hospedado Chrome existente](hwa-chrome-conversion.md) em um Aplicativo Web hospedado do Windows. [ManifoldJS](http://manifoldjs.com/) agora aceita manifestos do Chrome como uma forma de entrada. Desenvolvemos também uma [ferramenta CLI](https://github.com/MicrosoftEdge/hwa-cli) que gera um `.appx` pacote do seu `.zip` existente ou arquivos `.crx`.

## <a name="demos"></a>Demonstrações

- [Aplicativo de viagens Contoso](http://contosotravel.azurewebsites.net/)


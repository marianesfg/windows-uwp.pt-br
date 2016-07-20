---
title: Aplicativos Web hospedados - Converter seu aplicativo web para um aplicativo da Plataforma Universal do Windows (UWP) e acessar os recursos nativos do Windows 10
description: "Criar um aplicativo da Plataforma Universal do Windows (UWP) a partir da URL do seu site. Acessar recursos de dispositivo nativo do Windows 10 a partir do código que está dentro do seu aplicativo da web. Pontes do Windows da Microsoft para Aplicativos Web hospedados, anteriormente conhecido como Projeto Westminster, torna rápido e fácil incluir seu aplicativo Web na Windows Store."
author: seksenov
translationtype: Human Translation
ms.sourcegitcommit: 765789ef83f9b6a845ab79505b1b9ecbfd987126
ms.openlocfilehash: 491665558f713dcbaae7ea20739ed72c61a12cd2

---

# Aplicativos Web hospedados - Acessar recursos do Windows 10 a partir do seu aplicativo Web

Seu aplicativo Web pode ter acesso completo à Plataforma Universal do Windows (UWP), incluindo chamar APIs do Windows Runtime diretamente de um script hospedado em um servidor, aproveitar a integração com a Cortana e usar um provedor de autenticação online. Também há suporte para aplicativos híbridos, pois você pode incluir código local para ser chamado do script hospedado e gerenciar a navegação do aplicativo entre as páginas locais e remotas.

## Introdução

Se você estiver em um Mac ou PC, você pode criar seu próprio Aplicativo Web hospedado em questão de minutos. A melhor maneira de começar é usando [Visual Studio Community 2015](https://www.visualstudio.com/), gratuito e completo, especialmente se você estiver em um dispositivo Windows. Se você não tem acesso ao Visual Studio, há algumas opções que você pode escolher. Se você estiver familiarizado com a utilitários com interface de linha de comando (CLI), confira [ManifoldJS](http://manifoldjs.com/). Você também pode usar o [App Studio](http://appstudio.windows.com/), uma ferramenta de criação on-line gratuita e sem a necessidade de codificação que permite criar rapidamente aplicativos para o Windows 10.

- [Instruções passo a passo para converter seu aplicativo da web em um aplicativo da Plataforma Universal do Windows (UWP) usando o Windows](hwa-create-windows.md)

- [Instruções passo a passo para converter seu aplicativo da web em um aplicativo da Plataforma Universal do Windows (UWP) usando um Mac](hwa-create-mac.md)

## Melhorar seu aplicativo

- Torne seu aplicativo sparkle [acessando recursos nativos do Windows](hwa-access-features.md) no JavaScript a partir do Windows Runtime.

- Proteja seu aplicativo definindo Regras de URI de conteúdo do aplicativo (ACURs) com nosso modelo de Política de Segurança de Conteúdo (CSP).
- Execute o aplicativo com o poder da voz por meio da integração com comandos de voz da Cortana.

- Conceda acesso programático a recursos do usuário e a funcionalidade do dispositivo declarando as funcionalidades do aplicativo.

- Crie um fluxo de logon simples e estável para seus usuários verificando sua identidade com OpenID e OAuth.

- Pare de decidir entre um Aplicativo Web hospedado e empacotado. Você pode ter um pouco de ambos, criando um Aplicativo híbrido.

## Converter seu aplicativo Chrome existente

Tornamos fácil [converter seu aplicativo hospedado Chrome existente](hwa-chrome-conversion.md) em um Aplicativo Web hospedado do Windows. 
            [ManifoldJS](http://manifoldjs.com/) agora aceita manifestos do Chrome como uma forma de entrada. Desenvolvemos também uma [ferramenta CLI](https://github.com/MicrosoftEdge/hwa-cli) que gera um `.appx` pacote do seu `.zip` existente ou arquivos `.crx`.

## Demonstrações

- [Aplicativo de viagens Contoso](http://contosotravel.azurewebsites.net/)

- [API Tempo do Windows Runtime: exemplos de código JavaScript](http://rjs.azurewebsites.net/)



<!--HONumber=Jul16_HO1-->



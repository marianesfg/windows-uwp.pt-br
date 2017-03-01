---
author: seksenov
title: Aplicativos Web hospedados - Converter seu aplicativo Web em um aplicativo do Windows usando um Mac
description: Use um Mac para tornar seu site um aplicativo da Plataforma Universal do Windows (UWP) para Windows 10.
kw: Hosted Web Apps with a Mac, Porting to Windows 10 with a Mac, Convert website to Windows with Mac, Packaging web application with ManfoldJS for Windows Store, Add website to Windows Store with App Studio
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: aplicativos web hospedados com um Mac, portabilidade para o Windows 10 com um Mac, converter o site para o Windows com Mac, site para a Windows Store, JS variada para aplicativos web, App Studio para aplicativos web
ms.assetid: a4cea1e8-4417-4488-b0e7-45c704dc53e9
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 464097cc3f90c2e84e03e936fb354f4220aff91d
ms.lasthandoff: 02/08/2017

---

# <a name="create-your-hosted-web-app-using-a-mac"></a>Crie seu Aplicativo Web hospedado em um Mac

Crie rapidamente um aplicativo da Plataforma Universal do Windows para Windows 10, começando com apenas uma URL do site. 

> [!NOTE]
> As instruções a seguir referem-se ao uso de uma plataforma de desenvolvimento do Mac. Usuários do Windows, acesse as [instruções sobre como usar uma plataforma de desenvolvimento do Windows](./hwa-create-windows.md).

## <a name="what-you-need-to-develop-on-mac"></a>O que você precisa para desenvolver no Mac

- Um navegador da Web.
- Um prompt de comando.

## <a name="option-1-manifoldjs"></a>Opção 1: ManifoldJS

[ManifoldJS](http://manifoldjs.com/) é um aplicativo Node.js que instala facilmente a partir do NPM. Ele leva os metadados para seu site da Web e gera os aplicativos nativos hospedados no Android, iOS e Windows. Se seu site não tiver um [manifesto do aplicativo Web](https://www.w3.org/TR/appmanifest/), ele será gerado automaticamente para você.

1. Instale [NodeJS](https://nodejs.org/) que inclui o NPM (Gerenciador de pacotes de nó). <br>

2. Abra um prompt de comando e instale o NPM do ManifoldJS:
```
npm install -g manifoldjs
```

3. Execute o comando `manifoldjs` na URL do seu site:
```
manifoldjs http://codepen.io/seksenov/pen/wBbVyb/?editors=101
```

4. Siga as etapas do vídeo abaixo para concluir o empacotamento e publicar seu Aplicativo Web hospedado na Windows Store.

[![Publicando um Aplicativo Web UWP em um Mac usando o ManifoldJS](images/hwa-to-uwp/mac_manifoldjs_video.png)](https://sec.ch9.ms/ch9/0a67/9b06e5c7-d7aa-478d-b30d-f99e145a0a67/ManifoldJS_high.mp4 "Publicando um Aplicativo Web UWP em um Mac usando o ManifoldJS")

## <a name="option-2-app-studio"></a>Opção 2: App Studio

[App Studio](http://appstudio.windows.com/) é uma ferramenta de criação de aplicativo on-line gratuita que permite criar rapidamente aplicativos para o Windows 10.

1. Abra o [App Studio](http://appstudio.windows.com/) no seu navegador da web.

2. Clique em **Começar agora!**.

3. Em **Modelos de aplicativos Web**, clique em **Aplicativo Web hospedado**.

4. Siga a tela instruções para gerar um pacote pronto para publicação na Windows Store.

## <a name="related-topics"></a>Tópicos relacionados

- [Melhorar seu aplicativo Web ao acessar recursos da Plataforma Universal do Windows(UWP)](./hwa-access-features.md)
- [Guia para aplicativos UWP (Plataforma Universal do Windows)](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Baixar ativos de design para aplicativos da Windows Store](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)


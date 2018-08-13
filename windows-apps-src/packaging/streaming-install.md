---
author: laurenhughes
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Instalação de streaming de aplicativo UWP
description: A instalação de streaming da Plataforma Universal do Windows (UWP) permite que você especifique as partes do seu aplicativo que você gostaria que a Microsoft Store baixasse primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: instalar o Windows 10, uwp, streaming install, app uwp streaming
ms.localizationpriority: medium
ms.openlocfilehash: 087226cad4bcf7ea0294d8878564c345d6cfb9d0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "304406"
---
# <a name="uwp-app-streaming-install"></a>Instalação de streaming de aplicativo UWP
A instalação de streaming da Plataforma Universal do Windows (UWP) permite que você especifique as partes do seu aplicativo que você gostaria que a Microsoft Store baixasse primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano. 

Para usar UWP App Streaming instalar, você precisará dividir os arquivos do seu aplicativo em seções. Para fazer isso, você criará um mapa de conteúdo de grupo, que é um arquivo XML que é fornecido com o seu aplicativo, permitindo que você definir a prioridade de download e em ordem. Consulte o tópico vinculado abaixo para obter mais informações.

Para obter um guia completo sobre a adição de UWP App Streaming instalar para seu aplicativo UWP, confira esta [série de blogs](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Tópico | Descrição | 
|-------|-------------|
| [Crie e converta um mapa de grupo de conteúdo de origem](create-cgm.md) | Para preparar o seu aplicativo Universal Windows Platform (UWP) para a instalação de streaming de aplicativos UWP, você precisará criar um mapa de grupo de conteúdo. Este artigo irá ajudá-lo com as especificidades de criar e converter um mapa de grupo de conteúdo, fornecendo algumas dicas e truques ao longo do caminho. |
---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Instalação de streaming de aplicativo UWP
description: A instalação de streaming da Plataforma Universal do Windows (UWP) permite que você especifique as partes do seu aplicativo que você gostaria que a Microsoft Store baixasse primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano.
ms.date: 04/05/2017
ms.topic: article
keywords: Windows 10, uwp, instalação, de streaming de instalação de aplicativo uwp de streaming
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8208332"
---
# <a name="uwp-app-streaming-install"></a>Instalação de streaming de aplicativo UWP
A instalação de streaming da Plataforma Universal do Windows (UWP) permite que você especifique as partes do seu aplicativo que você gostaria que a Microsoft Store baixasse primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano. 

Para usar a instalação de Streaming de aplicativo do UWP, você precisará dividir arquivos do seu aplicativo em seções. Para fazer isso, você criará um mapa de grupo de conteúdo, que é um arquivo XML que é empacotado com seu aplicativo, permitindo que você definir prioridade do download e ordem. Consulte o tópico links abaixo para obter mais informações.

Para obter um guia completo sobre como adicionar a instalação de Streaming de aplicativo do UWP ao seu aplicativo UWP, confira esta [série de blogs](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Tópico | Descrição | 
|-------|-------------|
| [Crie e converta um mapa de grupo de conteúdo de origem](create-cgm.md) | Para preparar o seu aplicativo Universal Windows Platform (UWP) para a instalação de streaming de aplicativos UWP, você precisará criar um mapa de grupo de conteúdo. Este artigo irá ajudá-lo com as especificidades de criar e converter um mapa de grupo de conteúdo, fornecendo algumas dicas e truques ao longo do caminho. |
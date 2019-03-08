---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Instalação de streaming de aplicativo UWP
description: A instalação de streaming da Plataforma Universal do Windows (UWP) permite que você especifique as partes do seu aplicativo que você gostaria que a Microsoft Store baixasse primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano.
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, uwp, instalação de streaming, aplicativo uwp, instalação de streaming
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608651"
---
# <a name="uwp-app-streaming-install"></a>Instalação de streaming de aplicativo UWP
A instalação de streaming da Plataforma Universal do Windows (UWP) permite que você especifique as partes do seu aplicativo que você gostaria que a Microsoft Store baixasse primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano. 

Para usar a instalação de streaming de aplicativo UWP, você precisará dividir arquivos do seu aplicativo em seções. Para fazer isso, você criará um mapa de grupo de conteúdo, que é um arquivo XML que está empacotado com seu aplicativo, permitindo que você defina prioridade e ordem de download. Veja o tópico abaixo relacionado para obter mais informações.

Para obter um guia completo sobre a adição de instalação de streaming ao seu aplicativo UWP, confira [série do blog](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Tópico | Descrição | 
|-------|-------------|
| [Criar e converter um mapa de conteúdo do grupo de origem](create-cgm.md) | Para preparar o seu aplicativo Universal Windows Platform (UWP) para a instalação de streaming de aplicativos UWP, você precisará criar um mapa de grupo de conteúdo. Este artigo irá ajudá-lo com as especificidades de criar e converter um mapa de grupo de conteúdo, fornecendo algumas dicas e truques ao longo do caminho. |